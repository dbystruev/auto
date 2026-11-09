# Social Sync — KMP Automated Video Publisher

An end-to-end, **100% Kotlin Multiplatform (KMP)** solution for authorizing and auto-publishing user-uploaded videos to multiple social networks automatically (e.g., scheduled daily runs).

Built with **Compose Multiplatform** (Android, iOS, Web), **Ktor Server** (Kotlin/JVM on Vercel Serverless), and **Supabase** (PostgreSQL Database, Auth, Storage, and Token Vault).

---

## 🌟 Key Features

* **Single-Point OAuth Authorization:** Log in once via OAuth 2.0 with offline access scope (`refresh_token`). Store credentials securely in a server-side vault for automated background posting without re-authenticating.
* **Cross-Platform Clients:** Unified Kotlin UI across Android, iOS, and Web via Compose Multiplatform.
* **100% Pure Kotlin Stack:** Share code, data transfer objects (DTOs), validation logic, and API clients seamlessly between the mobile/web applications and the Ktor backend.
* **Direct Pre-signed Video Uploads:** Upload heavy video binaries straight from the client to Supabase Storage, bypassing backend server bottlenecks.
* **Automated Cron Publisher:** Vercel Cron Jobs invoke Ktor Server endpoints to automatically process scheduled posts, refresh expired OAuth tokens, chunk-upload videos, and log platform posting metrics.

---

## 🏗 System Architecture

```
                               ┌─────────────────────────────────────────┐
                               │     Compose Multiplatform Clients       │
                               │        (Android / iOS / Web)            │
                               └───────────┬─────────────────┬───────────┘
                                           │ Direct Upload   │ API Requests
                                           ▼                 ▼
┌─────────────────────────┐      ┌─────────────────┐       ┌─────────────────────────┐
│ Target Social Networks  │      │ Supabase        │       │ Ktor Server Backend     │
│ (YouTube, TikTok, Meta, │◄─────┤ Video Storage   │       │ (Kotlin/JVM on Vercel)  │
│  X, LinkedIn, etc.)     │      └─────────────────┘       └───────────┬─────────────┘
└───────────▲─────────────┘                                            │
            │ Background Cron Uploads & OAuth Token Refresh            │ Database / Auth
            └──────────────────────────────────────────────────────────┘
                                                                       ▼
                                                           ┌─────────────────────────┐
                                                           │ Supabase PostgreSQL     │
                                                           │ (Vault + RLS Policies)  │
                                                           └─────────────────────────┘
```

---

## 📊 Supported Social Platforms

| Platform | Format | Auth Standard | Background Refresh Validity |
| :--- | :--- | :--- | :--- |
| **YouTube** | Shorts & Long-Form | Google OAuth 2.0 (`offline`) | Persistent `refresh_token` (unlimited until revoked) |
| **Instagram** | Reels & Feed Videos | Facebook OAuth 2.0 | 60-day Long-Lived Token (auto-refreshed by backend) |
| **Facebook** | Page Videos & Reels | Facebook OAuth 2.0 | Page Access Tokens (no expiration) |
| **TikTok** | Short-Form Videos | TikTok OAuth 2.0 | 365-day Refresh Token |
| **LinkedIn** | Native Video Posts | OAuth 2.0 (`offline`) | 60 to 365 days (tier dependent) |
| **X (Twitter)** | Videos (up to 1080p) | OAuth 2.0 (`offline.access`) | Persistent `refresh_token` |
| **Pinterest** | Video Pins | Pinterest API v5 | 60-day Refresh Token |
| **Bluesky** | Short Video Clips | AT Protocol / App Password | Continuous authorization |

---

## 📄 API & System Specifications

For the complete API reference, schema definitions, response codes, and background cron logic, please read:

👉 **[API Specification (API.md)](./API.md)**

### Quick Reference of Key API Endpoints
* `GET /api/v1/integrations` — List connected social accounts and token health status.
* `GET /api/v1/integrations/{platform}/auth-url` — Generate platform-specific OAuth URLs.
* `POST /api/v1/integrations/{platform}/callback` — Exchange auth codes for long-lived backend tokens.
* `POST /api/v1/media/upload-url` — Request a pre-signed Supabase Storage upload URL.
* `POST /api/v1/posts` — Schedule a video for automated target publication.
* `POST /api/v1/cron/process-scheduled-posts` — Background endpoint executed by Vercel Cron.

---

## 📁 Repository Structure

```
.
├── API.md                       # Full API Specification Document
├── README.md                    # Project Overview & Quick Start Guide
├── shared/                      # Shared KMP Library (Android, iOS, Web, Ktor Backend)
│   └── src/commonMain/kotlin/
│       └── com/yourcompany/model/ # Shared DTOs (PostRequest, Integration, etc.)
├── backend-ktor/                # Ktor Server API App (Kotlin/JVM)
│   ├── src/main/kotlin/        # Backend routes, services, and Vercel handler
│   └── vercel.json              # Vercel Serverless & Cron configuration
└── composeApp/                  # Compose Multiplatform Frontend
    ├── src/androidMain/         # Android specific entrypoint
    ├── src/iosMain/             # iOS target configuration & SwiftUI embedding
    ├── src/wasmJsMain/          # WebAssembly / JS target
    └── src/commonMain/          # Shared Compose UI screens and ViewModels
```

---

## 🛠 Local Development Setup (Linux / Ubuntu)

### Prerequisites

* **JDK 17+** (`openjdk-17-jdk`)
* **OpenCode CLI / Terminal**
* **Gradle 8.x**
* **Supabase CLI** (`npm i -g supabase` or via package manager)
* **Vercel CLI** (`npm i -g vercel`)

### 1. Clone & Configure Environment

```bash
git clone https://github.com/your-org/social-sync-kmp.git
cd social-sync-kmp
```

Create `.env` file in the project root:

```env
SUPABASE_URL=https://<your-supabase-project>.supabase.co
SUPABASE_ANON_KEY=your-supabase-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-supabase-service-role-key
VERCEL_CRON_SECRET=your-random-cron-secret
YOUTUBE_CLIENT_ID=your-google-client-id
YOUTUBE_CLIENT_SECRET=your-google-client-secret
TIKTOK_CLIENT_KEY=your-tiktok-client-key
TIKTOK_CLIENT_SECRET=your-tiktok-client-secret
META_APP_ID=your-meta-app-id
META_APP_SECRET=your-meta-app-secret
```

### 2. Run Database Migrations

```bash
supabase link --project-ref <your-supabase-project-id>
supabase db push
```

### 3. Run Ktor Backend Locally

```bash
./gradlew :backend-ktor:run
```
The server will start at `http://localhost:8080`.

### 4. Run Mobile & Web Clients

* **Android:**
  ```bash
  ./gradlew :composeApp:assembleDebug
  ```
* **Web (Wasm):**
  ```bash
  ./gradlew :composeApp:wasmJsBrowserDevelopmentRun
  ```
* **iOS (Requires Xcode / macOS host):**
  ```bash
  ./gradlew :composeApp:iosSimulatorArm64Test
  ```

---

## 🚀 Deployment

### Deploy Backend to Vercel

```bash
cd backend-ktor
vercel --prod
```

Vercel will build the Kotlin/JVM serverless functions using Gradle and set up the cron schedule defined in `backend-ktor/vercel.json`.

---

## 📜 License

This project is licensed under the MIT License - see the LICENSE file for details.