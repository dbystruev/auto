# Social Sync — Progressive Implementation Plan

This document provides a multi-level, progressively refined implementation plan for building the **Social Sync** Kotlin Multiplatform application, Ktor JVM backend, Supabase database vault, and Vercel serverless cron infrastructure.

---

## Iteration Step 1: High-Level System Overview (Level 1)

* [<a id="idx-1">1. Database & Vault Subsystem Setup</a>](#step2-sec-1)
* [<a id="idx-2">2. Shared KMP Core Module</a>](#step2-sec-2)
* [<a id="idx-3">3. Ktor Backend Subsystem Setup</a>](#step2-sec-3)
* [<a id="idx-4">4. Compose Multiplatform Client Application</a>](#step2-sec-4)
* [<a id="idx-5">5. Deployment & Infrastructure</a>](#step2-sec-5)

---

## Iteration Step 2: Subsystem Structural Outline (Levels 1 & 2)

### <a id="step2-sec-1">1. Database & Vault Subsystem Setup</a>
* [<a id="idx-1-1">1.1 Local Supabase Scaffolding</a>](#step3-sec-1-1)
* [<a id="idx-1-2">1.2 Relational Database Schema & Storage Bucket</a>](#step3-sec-1-2)
* [<a id="idx-1-3">1.3 PostgreSQL Vault Encryption & Row-Level Security</a>](#step3-sec-1-3)

### <a id="step2-sec-2">2. Shared KMP Core Module</a>
* [<a id="idx-2-1">2.1 Shared Module Gradle Scaffolding</a>](#step3-sec-2-1)
* [<a id="idx-2-2">2.2 Data Transfer Objects (DTOs)</a>](#step3-sec-2-2)
* [<a id="idx-2-3">2.3 Ktor Client Service Integration</a>](#step3-sec-2-3)

### <a id="step2-sec-3">3. Ktor Backend Subsystem Setup</a>
* [<a id="idx-3-1">3.1 Ktor Backend Engine Scaffolding</a>](#step3-sec-3-1)
* [<a id="idx-3-2">3.2 Security & Authentication Middleware</a>](#step3-sec-3-2)
* [<a id="idx-3-3">3.3 API Route Controllers & Cron Worker</a>](#step3-sec-3-3)

### <a id="step2-sec-4">4. Compose Multiplatform Client Application</a>
* [<a id="idx-4-1">4.1 Compose Client Module Setup</a>](#step3-sec-4-1)
* [<a id="idx-4-2">4.2 UI Screens & ViewModels</a>](#step3-sec-4-2)
* [<a id="idx-4-3">4.3 Multiplatform Entry Points (Android, iOS, Web)</a>](#step3-sec-4-3)

### <a id="step2-sec-5">5. Deployment & Infrastructure</a>
* [<a id="idx-5-1">5.1 Vercel Serverless & Cron Configuration</a>](#step3-sec-5-1)
* [<a id="idx-5-2">5.2 Build Verification & Production Deployment</a>](#step3-sec-5-2)

---

## Iteration Step 3: Detailed Component Plan (Levels 1, 2 & 3)

### 1. Database & Vault Subsystem Setup

#### <a id="step3-sec-1-1">1.1 Local Supabase Scaffolding</a>
* [<a id="idx-1-1-1">1.1.1 Directory Structure Setup</a>](#step4-sec-1-1-1)
* [<a id="idx-1-1-2">1.1.2 CLI Configuration & Project Setup</a>](#step4-sec-1-1-2)

#### <a id="step3-sec-1-2">1.2 Relational Database Schema & Storage Bucket</a>
* [<a id="idx-1-2-1">1.2.1 Integrations Table Migration</a>](#step4-sec-1-2-1)
* [<a id="idx-1-2-2">1.2.2 Posts & Target Publishing Tables Migration</a>](#step4-sec-1-2-2)
* [<a id="idx-1-2-3">1.2.3 Media Storage Bucket Configuration</a>](#step4-sec-1-2-3)

#### <a id="step3-sec-1-3">1.3 PostgreSQL Vault Encryption & Row-Level Security</a>
* [<a id="idx-1-3-1">1.3.1 Pgsodium Vault Encryption Setup</a>](#step4-sec-1-3-1)
* [<a id="idx-1-3-2">1.3.2 Row Level Security (RLS) Isolation Policies</a>](#step4-sec-1-3-2)

---

### 2. Shared KMP Core Module

#### <a id="step3-sec-2-1">2.1 Shared Module Gradle Scaffolding</a>
* [<a id="idx-2-1-1">2.1.1 Shared Directory Tree Initialization</a>](#step4-sec-2-1-1)
* [<a id="idx-2-1-2">2.1.2 Shared Module Build Dependencies</a>](#step4-sec-2-1-2)

#### <a id="step3-sec-2-2">2.2 Data Transfer Objects (DTOs)</a>
* [<a id="idx-2-2-1">2.2.1 Integration & OAuth DTO Models</a>](#step4-sec-2-2-1)
* [<a id="idx-2-2-2">2.2.2 Media Upload & Post Scheduling DTO Models</a>](#step4-sec-2-2-2)

#### <a id="step3-sec-2-3">2.3 Ktor Client Service Integration</a>
* [<a id="idx-2-3-1">2.3.1 Social Sync HTTP Client Configuration</a>](#step4-sec-2-3-1)
* [<a id="idx-2-3-2">2.3.2 Bearer Token Authentication Interceptor</a>](#step4-sec-2-3-2)

---

### 3. Ktor Backend Subsystem Setup

#### <a id="step3-sec-3-1">3.1 Ktor Backend Engine Scaffolding</a>
* [<a id="idx-3-1-1">3.1.1 Backend Project Directory Structure</a>](#step4-sec-3-1-1)
* [<a id="idx-3-1-2">3.1.2 Ktor JVM Server Dependencies & Build Config</a>](#step4-sec-3-1-2)

#### <a id="step3-sec-3-2">3.2 Security & Authentication Middleware</a>
* [<a id="idx-3-2-1">3.2.1 Supabase JWT Token Validator</a>](#step4-sec-3-2-1)
* [<a id="idx-3-2-2">3.2.2 Cron Header Authentication Guard</a>](#step4-sec-3-2-2)

#### <a id="step3-sec-3-3">3.3 API Route Controllers & Cron Worker</a>
* [<a id="idx-3-3-1">3.3.1 OAuth Integration Route Endpoints</a>](#step4-sec-3-3-1)
* [<a id="idx-3-3-2">3.3.2 Media Upload & Post Scheduling Endpoints</a>](#step4-sec-3-3-2)
* [<a id="idx-3-3-3">3.3.3 Background Scheduled Post Publisher Worker</a>](#step4-sec-3-3-3)

---

### 4. Compose Multiplatform Client Application

#### <a id="step3-sec-4-1">4.1 Compose Client Module Setup</a>
* [<a id="idx-4-1-1">4.1.1 Client App Directory Structure</a>](#step4-sec-4-1-1)
* [<a id="idx-4-1-2">4.1.2 Compose Multiplatform Build Setup</a>](#step4-sec-4-1-2)

#### <a id="step3-sec-4-2">4.2 UI Screens & ViewModels</a>
* [<a id="idx-4-2-1">4.2.1 Connected Social Accounts Screen & ViewModel</a>](#step4-sec-4-2-1)
* [<a id="idx-4-2-2">4.2.2 Video Upload & Scheduling Screen & ViewModel</a>](#step4-sec-4-2-2)

#### <a id="step3-sec-4-3">4.3 Multiplatform Entry Points (Android, iOS, Web)</a>
* [<a id="idx-4-3-1">4.3.1 Mobile Native Target Hosts (Android Activity & iOS ViewController)</a>](#step4-sec-4-3-1)
* [<a id="idx-4-3-2">4.3.2 Web Target Main Entry Point (Wasm JS)</a>](#step4-sec-4-3-2)

---

### 5. Deployment & Infrastructure

#### <a id="step3-sec-5-1">5.1 Vercel Serverless & Cron Configuration</a>
* [<a id="idx-5-1-1">5.1.1 Serverless Deployment Config File</a>](#step4-sec-5-1-1)
* [<a id="idx-5-1-2">5.1.2 Vercel CLI Project Linking</a>](#step4-sec-5-1-2)

#### <a id="step3-sec-5-2">5.2 Build Verification & Production Deployment</a>
* [<a id="idx-5-2-1">5.2.1 Gradle Suite Compilation & Test Run</a>](#step4-sec-5-2-1)
* [<a id="idx-5-2-2">5.2.2 Production Deployment Execution</a>](#step4-sec-5-2-2)

---

## Iteration Step 4: Complete Atomic Action Plan (All Levels down to Atomic Actions)

### 1. Database & Vault Subsystem Setup

#### 1.1 Local Supabase Scaffolding

##### <a id="step4-sec-1-1-1">1.1.1 Directory Structure Setup</a>
* **Action 1.1.1.1:** Run command `mkdir -p supabase`
* **Action 1.1.1.2:** Run command `mkdir -p supabase/migrations`

##### <a id="step4-sec-1-1-2">1.1.2 CLI Configuration & Project Setup</a>
* **Action 1.1.2.1:** Run command `supabase init`
* **Action 1.1.2.2:** Create file `supabase/config.toml` configuring local project ID `social-sync-backend`.

---

#### 1.2 Relational Database Schema & Storage Bucket

##### <a id="step4-sec-1-2-1">1.2.1 Integrations Table Migration</a>
* **Action 1.2.1.1:** Create file `supabase/migrations/20260101000000_create_integrations.sql`
* **Action 1.2.1.2:** Populate `supabase/migrations/20260101000000_create_integrations.sql` with SQL schema for `integrations`.

##### <a id="step4-sec-1-2-2">1.2.2 Posts & Target Publishing Tables Migration</a>
* **Action 1.2.2.1:** Create file `supabase/migrations/20260101000001_create_posts_and_targets.sql`
* **Action 1.2.2.2:** Populate `supabase/migrations/20260101000001_create_posts_and_targets.sql` with `posts` and `post_targets` SQL schema.

##### <a id="step4-sec-1-2-3">1.2.3 Media Storage Bucket Configuration</a>
* **Action 1.2.3.1:** Create file `supabase/migrations/20260101000002_create_storage_buckets.sql`
* **Action 1.2.3.2:** Populate `supabase/migrations/20260101000002_create_storage_buckets.sql` with `videos` bucket configuration.

---

#### 1.3 PostgreSQL Vault Encryption & Row-Level Security

##### <a id="step4-sec-1-3-1">1.3.1 Pgsodium Vault Encryption Setup</a>
* **Action 1.3.1.1:** Create file `supabase/migrations/20260101000003_enable_pgvault.sql`
* **Action 1.3.1.2:** Populate `supabase/migrations/20260101000003_enable_pgvault.sql` with `pgsodium` extension setup.

##### <a id="step4-sec-1-3-2">1.3.2 Row Level Security (RLS) Isolation Policies</a>
* **Action 1.3.2.1:** Create file `supabase/migrations/20260101000004_configure_rls_policies.sql`
* **Action 1.3.2.2:** Populate `supabase/migrations/20260101000004_configure_rls_policies.sql` with RLS policies for `integrations` and `posts`.
* **Action 1.3.2.3:** Run command `supabase db push`

---

### 2. Shared KMP Core Module

#### 2.1 Shared Module Gradle Scaffolding

##### <a id="step4-sec-2-1-1">2.1.1 Shared Directory Tree Initialization</a>
* **Action 2.1.1.1:** Run command `mkdir -p shared`
* **Action 2.1.1.2:** Run command `mkdir -p shared/src/commonMain/kotlin/com/yourcompany/model`
* **Action 2.1.1.3:** Run command `mkdir -p shared/src/commonMain/kotlin/com/yourcompany/client`

##### <a id="step4-sec-2-1-2">2.1.2 Shared Module Build Dependencies</a>
* **Action 2.1.2.1:** Create file `shared/build.gradle.kts`
* **Action 2.1.2.2:** Populate `shared/build.gradle.kts` with `kotlinx.serialization` and `ktor-client-core` dependencies.
* **Action 2.1.2.3:** Update file `settings.gradle.kts` adding `include(":shared")`.

---

#### 2.2 Data Transfer Objects (DTOs)

##### <a id="step4-sec-2-2-1">2.2.1 Integration & OAuth DTO Models</a>
* **Action 2.2.1.1:** Create file `shared/src/commonMain/kotlin/com/yourcompany/model/IntegrationDto.kt`
* **Action 2.2.1.2:** Create file `shared/src/commonMain/kotlin/com/yourcompany/model/OAuthUrlDto.kt`

##### <a id="step4-sec-2-2-2">2.2.2 Media Upload & Post Scheduling DTO Models</a>
* **Action 2.2.2.1:** Create file `shared/src/commonMain/kotlin/com/yourcompany/model/MediaUploadDto.kt`
* **Action 2.2.2.2:** Create file `shared/src/commonMain/kotlin/com/yourcompany/model/PostRequestDto.kt`
* **Action 2.2.2.3:** Create file `shared/src/commonMain/kotlin/com/yourcompany/model/PostResponseDto.kt`

---

#### 2.3 Ktor Client Service Integration

##### <a id="step4-sec-2-3-1">2.3.1 Social Sync HTTP Client Configuration</a>
* **Action 2.3.1.1:** Create file `shared/src/commonMain/kotlin/com/yourcompany/client/SocialSyncApiClient.kt`
* **Action 2.3.1.2:** Populate `shared/src/commonMain/kotlin/com/yourcompany/client/SocialSyncApiClient.kt` with Ktor client instance.

##### <a id="step4-sec-2-3-2">2.3.2 Bearer Token Authentication Interceptor</a>
* **Action 2.3.2.1:** Create file `shared/src/commonMain/kotlin/com/yourcompany/client/AuthInterceptor.kt`
* **Action 2.3.2.2:** Populate `shared/src/commonMain/kotlin/com/yourcompany/client/AuthInterceptor.kt` with Supabase JWT bearer injection logic.

---

### 3. Ktor Backend Subsystem Setup

#### 3.1 Ktor Backend Engine Scaffolding

##### <a id="step4-sec-3-1-1">3.1.1 Backend Project Directory Structure</a>
* **Action 3.1.1.1:** Run command `mkdir -p backend-ktor`
* **Action 3.1.1.2:** Run command `mkdir -p backend-ktor/src/main/kotlin/com/yourcompany/backend/routes`
* **Action 3.1.1.3:** Run command `mkdir -p backend-ktor/src/main/kotlin/com/yourcompany/backend/services`

##### <a id="step4-sec-3-1-2">3.1.2 Ktor JVM Server Dependencies & Build Config</a>
* **Action 3.1.2.1:** Create file `backend-ktor/build.gradle.kts`
* **Action 3.1.2.2:** Populate `backend-ktor/build.gradle.kts` with Ktor Server Netty and Supabase dependencies.
* **Action 3.1.2.3:** Update file `settings.gradle.kts` adding `include(":backend-ktor")`.

---

#### 3.2 Security & Authentication Middleware

##### <a id="step4-sec-3-2-1">3.2.1 Supabase JWT Token Validator</a>
* **Action 3.2.1.1:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/JwtSecurity.kt`
* **Action 3.2.1.2:** Populate `backend-ktor/src/main/kotlin/com/yourcompany/backend/JwtSecurity.kt` with JWT verification.

##### <a id="step4-sec-3-2-2">3.2.2 Cron Header Authentication Guard</a>
* **Action 3.2.2.1:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/CronSecurity.kt`
* **Action 3.2.2.2:** Populate `backend-ktor/src/main/kotlin/com/yourcompany/backend/CronSecurity.kt` verifying `X-Cron-Secret` header.

---

#### 3.3 API Route Controllers & Cron Worker

##### <a id="step4-sec-3-3-1">3.3.1 OAuth Integration Route Endpoints</a>
* **Action 3.3.1.1:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/routes/IntegrationRoutes.kt`
* **Action 3.3.1.2:** Populate `backend-ktor/src/main/kotlin/com/yourcompany/backend/routes/IntegrationRoutes.kt` with OAuth endpoints.

##### <a id="step4-sec-3-3-2">3.3.2 Media Upload & Post Scheduling Endpoints</a>
* **Action 3.3.2.1:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/routes/MediaRoutes.kt`
* **Action 3.3.2.2:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/routes/PostRoutes.kt`

##### <a id="step4-sec-3-3-3">3.3.3 Background Scheduled Post Publisher Worker</a>
* **Action 3.3.3.1:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/routes/CronRoutes.kt`
* **Action 3.3.3.2:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/services/SocialPublisherService.kt`
* **Action 3.3.3.3:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/Application.kt` binding route controllers.

---

### 4. Compose Multiplatform Client Application

#### 4.1 Compose Client Module Setup

##### <a id="step4-sec-4-1-1">4.1.1 Client App Directory Structure</a>
* **Action 4.1.1.1:** Run command `mkdir -p composeApp`
* **Action 4.1.1.2:** Run command `mkdir -p composeApp/src/commonMain/kotlin/com/yourcompany/ui`

##### <a id="step4-sec-4-1-2">4.1.2 Compose Multiplatform Build Setup</a>
* **Action 4.1.1.3:** Create file `composeApp/build.gradle.kts`
* **Action 4.1.1.4:** Populate `composeApp/build.gradle.kts` with targets (Android, iOS, WasmJs).
* **Action 4.1.1.5:** Update file `settings.gradle.kts` adding `include(":composeApp")`.

---

#### 4.2 UI Screens & ViewModels

##### <a id="step4-sec-4-2-1">4.2.1 Connected Social Accounts Screen & ViewModel</a>
* **Action 4.2.1.1:** Create file `composeApp/src/commonMain/kotlin/com/yourcompany/ui/IntegrationsScreen.kt`
* **Action 4.2.1.2:** Create file `composeApp/src/commonMain/kotlin/com/yourcompany/ui/IntegrationsViewModel.kt`

##### <a id="step4-sec-4-2-2">4.2.2 Video Upload & Scheduling Screen & ViewModel</a>
* **Action 4.2.2.1:** Create file `composeApp/src/commonMain/kotlin/com/yourcompany/ui/CreatePostScreen.kt`
* **Action 4.2.2.2:** Create file `composeApp/src/commonMain/kotlin/com/yourcompany/ui/CreatePostViewModel.kt`

---

#### 4.3 Multiplatform Entry Points (Android, iOS, Web)

##### <a id="step4-sec-4-3-1">4.3.1 Mobile Native Target Hosts (Android Activity & iOS ViewController)</a>
* **Action 4.3.1.1:** Create file `composeApp/src/androidMain/kotlin/com/yourcompany/MainActivity.kt`
* **Action 4.3.1.2:** Create file `composeApp/src/iosMain/kotlin/com/yourcompany/MainViewController.kt`

##### <a id="step4-sec-4-3-2">4.3.2 Web Target Main Entry Point (Wasm JS)</a>
* **Action 4.3.2.1:** Create file `composeApp/src/wasmJsMain/kotlin/com/yourcompany/main.kt`
* **Action 4.3.2.2:** Create file `composeApp/src/wasmJsMain/resources/index.html`

---

### 5. Deployment & Infrastructure

#### 5.1 Vercel Serverless & Cron Configuration

##### <a id="step4-sec-5-1-1">5.1.1 Serverless Deployment Config File</a>
* **Action 5.1.1.1:** Create file `backend-ktor/vercel.json`
* **Action 5.1.1.2:** Populate `backend-ktor/vercel.json` with JVM build and daily cron definition.

##### <a id="step4-sec-5-1-2">5.1.2 Vercel CLI Project Linking</a>
* **Action 5.1.2.1:** Run command `vercel link --yes`
* **Action 5.1.2.2:** Run command `vercel env pull .env.local`

---

#### 5.2 Build Verification & Production Deployment

##### <a id="step4-sec-5-2-1">5.2.1 Gradle Suite Compilation & Test Run</a>
* **Action 5.2.1.1:** Run command `./gradlew check`
* **Action 5.2.1.2:** Run command `./gradlew :backend-ktor:build`

##### <a id="step4-sec-5-2-2">5.2.2 Production Deployment Execution</a>
* **Action 5.2.2.1:** Run command `supabase db push --linked`
* **Action 5.2.2.2:** Run command `vercel --prod`