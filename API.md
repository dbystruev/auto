# Social Media Video Auto-Publisher API Specification

**Version:** 1.0.0  
**Stack standard:** Pure Kotlin (Kotlin Multiplatform Client + Ktor JVM Backend + Supabase Platform)  
**Base URL:** `https://api.yourdomain.com/api/v1` (or `https://<vercel-project>.vercel.app/api/v1`)  
**Authentication Standard:** Supabase Auth JWT (`Authorization: Bearer <SUPABASE_JWT_TOKEN>`)

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│               Clients (Android / iOS / Web)             │
│   (Kotlin Multiplatform + Ktor Client + Shared DTOs)    │
└───────────┬─────────────────────────────────┬───────────┘
            │ Direct Binary Upload            │ HTTPS REST Requests
            ▼                                 ▼
┌─────────────────────────┐     ┌─────────────────────────┐
│  Supabase Storage Bucket│     │   Ktor Server Backend   │
│  (Encrypted Video Files)│     │  (Kotlin JVM / Vercel)  │
└─────────────────────────┘     └───────────┬─────────────┘
                                            │ SQL / Auth
                                            ▼
                                ┌─────────────────────────┐
                                │   Supabase PostgreSQL   │
                                │   (RLS + Token Vault)   │
                                └─────────────────────────┘
```

1. **Authentication:** Managed client-side via the Supabase Auth Kotlin SDK. The resulting JWT is attached to every Ktor Client HTTP request header.
2. **Backend Engine:** Implemented exclusively in **Kotlin** using **Ktor Server** running on Vercel Serverless Functions (Kotlin/JVM).
3. **Video Storage:** Clients request pre-signed upload URLs from Ktor Server, upload the raw video binary directly to Supabase Storage, and pass the stored media reference ID back to the API.
4. **OAuth Token Vault:** Refresh tokens for YouTube, TikTok, Meta (Facebook/Instagram), X, LinkedIn, Pinterest, and Bluesky are stored encrypted in Supabase DB using PostgreSQL Vault and Row Level Security (RLS).
5. **Automated Publishing:** Vercel Cron Jobs trigger Ktor Server cron handlers to check pending posts, refresh expiring OAuth tokens server-side, chunk-upload videos to target social APIs, and record post execution metrics.

---

## 1. Global Specifications & Headers

### Mandatory Request Headers

| Header | Type | Description |
| :--- | :--- | :--- |
| `Authorization` | String | `Bearer <supabase_access_token>` (Required for all authenticated client endpoints) |
| `Content-Type` | String | `application/json` |
| `X-Client-Platform` | String | `android` \| `ios` \| `web` |
| `X-App-Version` | String | Client app semantic versioning (e.g., `1.2.0`) |

### Standard Error Response Body

All Ktor endpoints return standard HTTP status codes along with a structured JSON error body matching this Kotlin `Serializable` data class:

```json
{
  "error": {
    "code": "EXPIRED_REFRESH_TOKEN",
    "message": "The linked YouTube authorization token has expired or been revoked.",
    "details": {
      "platform": "youtube",
      "action_required": "reauthenticate"
    },
    "timestamp": "2026-11-09T08:00:00Z"
  }
}
```

---

## 2. Social Account Integrations (`/integrations`)

### 2.1 List Connected Accounts

Retrieve all social media accounts linked to the current user along with their active authorization status.

* **HTTP Method:** `GET`
* **Path:** `/integrations`

#### Response `200 OK`
```json
{
  "integrations": [
    {
      "id": "int_9876543210",
      "platform": "youtube",
      "platform_user_id": "UC_x5XG1OV2P6uZZ5FSM9Ttw",
      "account_name": "My Tech Channel",
      "status": "ACTIVE",
      "connected_at": "2026-10-01T14:20:00Z",
      "expires_at": null
    },
    {
      "id": "int_1234567890",
      "platform": "instagram",
      "platform_user_id": "17841400000000000",
      "account_name": "@mybrand_reels",
      "status": "REQUIRES_REAUTH",
      "connected_at": "2026-08-15T10:00:00Z",
      "expires_at": "2026-11-01T00:00:00Z"
    }
  ]
}
```

---

### 2.2 Get OAuth Authorization URL

Generate the stateful OAuth 2.0 authorization URL to present to the user inside the app browser.

* **HTTP Method:** `GET`
* **Path:** `/integrations/{platform}/auth-url`
* **Path Parameters:** `platform` (`youtube` | `instagram` | `facebook` | `tiktok` | `x` | `linkedin` | `pinterest` | `bluesky`)
* **Query Parameters:**
  * `redirect_scheme` (optional): Deep link scheme for native client callback (e.g., `myapp://oauth-callback`)

#### Response `200 OK`
```json
{
  "platform": "youtube",
  "authorization_url": "https://accounts.google.com/o/oauth2/v2/auth?response_type=code&client_id=...&scope=https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fyoutube.upload&access_type=offline&prompt=consent&state=xyz123state",
  "state": "xyz123state"
}
```

---

### 2.3 Process OAuth Callback Code

Exchanges the authorization code for long-lived access and refresh tokens, encrypting and storing them in Supabase.

* **HTTP Method:** `POST`
* **Path:** `/integrations/{platform}/callback`
* **Path Parameters:** `platform` (`youtube` | `tiktok` | `meta` | etc.)

#### Request Body
```json
{
  "code": "4/0AQlBsWE...",
  "state": "xyz123state",
  "redirect_uri": "myapp://oauth-callback"
}
```

#### Response `200 OK`
```json
{
  "success": true,
  "integration": {
    "id": "int_9876543210",
    "platform": "youtube",
    "account_name": "My Tech Channel",
    "status": "ACTIVE"
  }
}
```

---

### 2.4 Disconnect Integration

Revokes backend permissions and removes social platform tokens.

* **HTTP Method:** `DELETE`
* **Path:** `/integrations/{platform}`

#### Response `200 OK`
```json
{
  "success": true,
  "message": "Integration for youtube disconnected successfully."
}
```

---

## 3. Media Upload Management (`/media`)

### 3.1 Request Upload Pre-signed URL

Requests a signed upload destination in Supabase Storage. The KMP client directly uploads the binary file to `upload_url`.

* **HTTP Method:** `POST`
* **Path:** `/media/upload-url`

#### Request Body
```json
{
  "filename": "daily_vlog_ep1.mp4",
  "content_type": "video/mp4",
  "size_in_bytes": 48234910
}
```

#### Response `200 OK`
```json
{
  "media_id": "med_abc123789",
  "upload_url": "https://<supabase-id>.supabase.co/storage/v1/object/upload/sign/videos/user_123/daily_vlog_ep1.mp4?token=eyJhbG...",
  "media_path": "videos/user_123/daily_vlog_ep1.mp4",
  "expires_in": 3600
}
```

---

## 4. Post Scheduling & Execution (`/posts`)

### 4.1 Create / Schedule Video Post

Schedules a video for publication across one or multiple social media targets at a specified UTC timestamp.

* **HTTP Method:** `POST`
* **Path:** `/posts`

#### Request Body
```json
{
  "media_id": "med_abc123789",
  "caption": "Check out today's update on our KMP app launch! 🚀 #buildinpublic #kotlin",
  "scheduled_at": "2026-11-10T15:00:00Z",
  "targets": [
    {
      "platform": "youtube",
      "config": {
        "title": "KMP App Launch Update Ep. 1",
        "privacy_status": "public",
        "made_for_kids": false,
        "is_short": true
      }
    },
    {
      "platform": "instagram",
      "config": {
        "media_type": "REELS",
        "share_to_feed": true
      }
    },
    {
      "platform": "tiktok",
      "config": {
        "privacy_level": "PUBLIC_TO_EVERYONE",
        "allow_comments": true,
        "allow_duet": true
      }
    }
  ]
}
```

#### Response `201 Created`
```json
{
  "id": "post_550e8400-e29b-41d4-a716-446655440000",
  "status": "SCHEDULED",
  "scheduled_at": "2026-11-10T15:00:00Z",
  "created_at": "2026-11-09T07:30:00Z",
  "targets": [
    { "platform": "youtube", "status": "PENDING" },
    { "platform": "instagram", "status": "PENDING" },
    { "platform": "tiktok", "status": "PENDING" }
  ]
}
```

---

### 4.2 List Scheduled & Published Posts

Retrieves user posts with pagination and filter parameters.

* **HTTP Method:** `GET`
* **Path:** `/posts`
* **Query Parameters:**
  * `status` (optional): `SCHEDULED` | `PROCESSING` | `PUBLISHED` | `FAILED`
  * `limit` (optional): Default `20`
  * `page` (optional): Default `1`

#### Response `200 OK`
```json
{
  "posts": [
    {
      "id": "post_550e8400-e29b-41d4-a716-446655440000",
      "caption": "Check out today's update on our KMP app launch!",
      "media_url": "https://<supabase-id>.supabase.co/storage/v1/object/public/videos/user_123/daily_vlog_ep1.mp4",
      "status": "PUBLISHED",
      "scheduled_at": "2026-11-09T12:00:00Z",
      "published_at": "2026-11-09T12:00:45Z",
      "target_results": [
        {
          "platform": "youtube",
          "status": "SUCCESS",
          "external_post_url": "https://youtube.com/shorts/dQw4w9WgXcQ",
          "error_message": null
        },
        {
          "platform": "tiktok",
          "status": "SUCCESS",
          "external_post_url": "https://www.tiktok.com/@user/video/71234567890",
          "error_message": null
        }
      ]
    }
  ],
  "pagination": {
    "total": 45,
    "page": 1,
    "limit": 20,
    "has_more": true
  }
}
```

---

### 4.3 Cancel Scheduled Post

* **HTTP Method:** `DELETE`
* **Path:** `/posts/{id}`

#### Response `200 OK`
```json
{
  "success": true,
  "message": "Post canceled successfully."
}
```

---

## 5. Automated Cron Execution System (`/cron`)

> **Note:** Executed directly by Vercel Cron Jobs invoking the Ktor Server backend endpoint. Secured via shared header token (`X-Cron-Secret`).

* **HTTP Method:** `POST`
* **Path:** `/cron/process-scheduled-posts`
* **Header:** `X-Cron-Secret: <VERCEL_CRON_SECRET>`

### Backend Cron Execution Flow

$$ \text{Target Posts} = \{ P \in \text{Database} \mid P.\text{status} = \text{'SCHEDULED'} \land P.\text{scheduled\_at} \le \text{NOW}() \} $$

1. Ktor Server locks rows in Supabase DB (`status = 'PROCESSING'`).
2. For each post target:
   * Retrieves encrypted OAuth Refresh Token from Supabase Vault.
   * Silently exchanges for a fresh Access Token using Ktor Client.
   * Reads video stream from Supabase Storage bucket.
   * Executes chunked multipart binary upload to the social platform's API endpoint.
   * Updates database record status (`SUCCESS` or `FAILED`).

---

## 6. End-to-End Kotlin Shared Code Architecture

By using Kotlin across client and server, your Kotlin Multiplatform shared library (`:shared`) can supply data classes to both the client UI and the Ktor backend:

```
project-root/
├── shared/                       # Shared Kotlin Multiplatform Code
│   └── src/commonMain/kotlin/
│       └── com/yourcompany/model/ # SHARED DTOs used by Client & Backend
│           ├── PostRequest.kt
│           ├── PostResponse.kt
│           └── Integration.kt
├── backend-ktor/                 # Ktor Server App (Deployed to Vercel JVM)
│   └── src/main/kotlin/
│       └── com/yourcompany/backend/
│           ├── Application.kt    # Ktor Engine Entrypoint
│           ├── routes/           # Ktor Route Handlers
│           └── services/         # Social Platform Posting Logic
└── composeApp/                   # KMP App (iOS / Android / Web UI)
```

### Shared DTO Example (`shared/.../PostRequest.kt`)

```kotlin
import kotlinx.serialization.Serializable

@Serializable
data class PostRequest(
    val mediaId: String,
    val caption: String,
    val scheduledAt: String,
    val targets: List<PostTargetConfig>
)

@Serializable
data class PostTargetConfig(
    val platform: String,
    val config: Map<String, String>
)
```

### Ktor Server Route Handler Example (`backend-ktor/.../PostRoutes.kt`)

```kotlin
import io.ktor.server.application.*
import io.ktor.server.request.*
import io.ktor.server.response.*
import io.ktor.server.routing.*
import io.ktor.http.*
import com.yourcompany.model.PostRequest

fun Route.postRoutes() {
    route("/api/v1/posts") {
        post {
            // Parses incoming body directly into the shared Kotlin PostRequest DTO
            val request = call.receive<PostRequest>()
            
            // Business logic: Save to Supabase and schedule background job
            val createdPostId = schedulePost(request)
            
            call.respond(HttpStatusCode.Created, mapOf("id" to createdPostId, "status" to "SCHEDULED"))
        }
    }
}
```