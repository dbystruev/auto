# Social Sync — Implementation Plan (Level of Detail Structure)

This document provides a progressive, multi-level implementation plan for building **Social Sync**. Each level of detail (**LoD**) expands the previous high-level view down to atomic, single-action steps.

---

## Level of Detail 1 (LoD 1): System Architecture High-Level Overview

* [1. Database & Vault Subsystem Setup](#lod2-1)
* [2. Shared KMP Core Module](#lod2-2)
* [3. Ktor Backend Subsystem Setup](#lod2-3)
* [4. Compose Multiplatform Client Application](#lod2-4)
* [5. Deployment & Infrastructure](#lod2-5)

---

## Level of Detail 2 (LoD 2): Subsystem Breakdown

<a id="lod2-1"></a>
### 1. Database & Vault Subsystem Setup
* [1.1 Local Supabase Scaffolding](#lod3-1-1)
* [1.2 Relational Database Schema & Storage Bucket](#lod3-1-2)
* [1.3 PostgreSQL Vault Encryption & Row-Level Security](#lod3-1-3)

<a id="lod2-2"></a>
### 2. Shared KMP Core Module
* [2.1 Shared Module Gradle Scaffolding](#lod3-2-1)
* [2.2 Data Transfer Objects (DTOs)](#lod3-2-2)
* [2.3 Ktor Client Service Integration](#lod3-2-3)

<a id="lod2-3"></a>
### 3. Ktor Backend Subsystem Setup
* [3.1 Ktor Backend Engine Scaffolding](#lod3-3-1)
* [3.2 Security & Authentication Middleware](#lod3-3-2)
* [3.3 API Route Controllers & Cron Worker](#lod3-3-3)

<a id="lod2-4"></a>
### 4. Compose Multiplatform Client Application
* [4.1 Compose Client Module Setup](#lod3-4-1)
* [4.2 UI Screens & ViewModels](#lod3-4-2)
* [4.3 Multiplatform Entry Points (Android, iOS, Web)](#lod3-4-3)

<a id="lod2-5"></a>
### 5. Deployment & Infrastructure
* [5.1 Vercel Serverless & Cron Configuration](#lod3-5-1)
* [5.2 Build Verification & Production Deployment](#lod3-5-2)

---

## Level of Detail 3 (LoD 3): Component Specification

### 1. Database & Vault Subsystem Setup

<a id="lod3-1-1"></a>
#### 1.1 Local Supabase Scaffolding
* [1.1.1 Directory Structure Setup](#lod4-1-1-1)
* [1.1.2 CLI Configuration & Project Setup](#lod4-1-1-2)

<a id="lod3-1-2"></a>
#### 1.2 Relational Database Schema & Storage Bucket
* [1.2.1 Integrations Table Migration](#lod4-1-2-1)
* [1.2.2 Posts & Target Publishing Tables Migration](#lod4-1-2-2)
* [1.2.3 Media Storage Bucket Configuration](#lod4-1-2-3)

<a id="lod3-1-3"></a>
#### 1.3 PostgreSQL Vault Encryption & Row-Level Security
* [1.3.1 Pgsodium Vault Encryption Setup](#lod4-1-3-1)
* [1.3.2 Row Level Security (RLS) Isolation Policies](#lod4-1-3-2)

---

### 2. Shared KMP Core Module

<a id="lod3-2-1"></a>
#### 2.1 Shared Module Gradle Scaffolding
* [2.1.1 Shared Directory Tree Initialization](#lod4-2-1-1)
* [2.1.2 Shared Module Build Dependencies](#lod4-2-1-2)

<a id="lod3-2-2"></a>
#### 2.2 Data Transfer Objects (DTOs)
* [2.2.1 Integration & OAuth DTO Models](#lod4-2-2-1)
* [2.2.2 Media Upload & Post Scheduling DTO Models](#lod4-2-2-2)

<a id="lod3-2-3"></a>
#### 2.3 Ktor Client Service Integration
* [2.3.1 Social Sync HTTP Client Configuration](#lod4-2-3-1)
* [2.3.2 Bearer Token Authentication Interceptor](#lod4-2-3-2)

---

### 3. Ktor Backend Subsystem Setup

<a id="lod3-3-1"></a>
#### 3.1 Ktor Backend Engine Scaffolding
* [3.1.1 Backend Project Directory Structure](#lod4-3-1-1)
* [3.1.2 Ktor JVM Server Dependencies & Build Config](#lod4-3-1-2)

<a id="lod3-3-2"></a>
#### 3.2 Security & Authentication Middleware
* [3.2.1 Supabase JWT Token Validator](#lod4-3-2-1)
* [3.2.2 Cron Header Authentication Guard](#lod4-3-2-2)

<a id="lod3-3-3"></a>
#### 3.3 API Route Controllers & Cron Worker
* [3.3.1 OAuth Integration Route Endpoints](#lod4-3-3-1)
* [3.3.2 Media Upload & Post Scheduling Endpoints](#lod4-3-3-2)
* [3.3.3 Background Scheduled Post Publisher Worker](#lod4-3-3-3)

---

### 4. Compose Multiplatform Client Application

<a id="lod3-4-1"></a>
#### 4.1 Compose Client Module Setup
* [4.1.1 Client App Directory Structure](#lod4-4-1-1)
* [4.1.2 Compose Multiplatform Build Setup](#lod4-4-1-2)

<a id="lod3-4-2"></a>
#### 4.2 UI Screens & ViewModels
* [4.2.1 Connected Social Accounts Screen & ViewModel](#lod4-4-2-1)
* [4.2.2 Video Upload & Scheduling Screen & ViewModel](#lod4-4-2-2)

<a id="lod3-4-3"></a>
#### 4.3 Multiplatform Entry Points (Android, iOS, Web)
* [4.3.1 Mobile Native Target Hosts (Android Activity & iOS ViewController)](#lod4-4-3-1)
* [4.3.2 Web Target Main Entry Point (Wasm JS)](#lod4-4-3-2)

---

### 5. Deployment & Infrastructure

<a id="lod3-5-1"></a>
#### 5.1 Vercel Serverless & Cron Configuration
* [5.1.1 Serverless Deployment Config File](#lod4-5-1-1)
* [5.1.2 Vercel CLI Project Linking](#lod4-5-1-2)

<a id="lod3-5-2"></a>
#### 5.2 Build Verification & Production Deployment
* [5.2.1 Gradle Suite Compilation & Test Run](#lod4-5-2-1)
* [5.2.2 Production Deployment Execution](#lod4-5-2-2)

---

## Level of Detail 4 (LoD 4): Atomic Execution Plan

### 1. Database & Vault Subsystem Setup

#### 1.1 Local Supabase Scaffolding

<a id="lod4-1-1-1"></a>
##### 1.1.1 Directory Structure Setup
* **Action 1.1.1.1:** Run command `mkdir -p supabase`
* **Action 1.1.1.2:** Run command `mkdir -p supabase/migrations`

<a id="lod4-1-1-2"></a>
##### 1.1.2 CLI Configuration & Project Setup
* **Action 1.1.2.1:** Run command `supabase init`
* **Action 1.1.2.2:** Create file `supabase/config.toml` configuring local project ID `social-sync-backend`.

---

#### 1.2 Relational Database Schema & Storage Bucket

<a id="lod4-1-2-1"></a>
##### 1.2.1 Integrations Table Migration
* **Action 1.2.1.1:** Create file `supabase/migrations/20260101000000_create_integrations.sql`
* **Action 1.2.1.2:** Populate `supabase/migrations/20260101000000_create_integrations.sql` with SQL schema for `integrations`.

<a id="lod4-1-2-2"></a>
##### 1.2.2 Posts & Target Publishing Tables Migration
* **Action 1.2.2.1:** Create file `supabase/migrations/20260101000001_create_posts_and_targets.sql`
* **Action 1.2.2.2:** Populate `supabase/migrations/20260101000001_create_posts_and_targets.sql` with `posts` and `post_targets` SQL schema.

<a id="lod4-1-2-3"></a>
##### 1.2.3 Media Storage Bucket Configuration
* **Action 1.2.3.1:** Create file `supabase/migrations/20260101000002_create_storage_buckets.sql`
* **Action 1.2.3.2:** Populate `supabase/migrations/20260101000002_create_storage_buckets.sql` with `videos` bucket configuration.

---

#### 1.3 PostgreSQL Vault Encryption & Row-Level Security

<a id="lod4-1-3-1"></a>
##### 1.3.1 Pgsodium Vault Encryption Setup
* **Action 1.3.1.1:** Create file `supabase/migrations/20260101000003_enable_pgvault.sql`
* **Action 1.3.1.2:** Populate `supabase/migrations/20260101000003_enable_pgvault.sql` with `pgsodium` extension setup.

<a id="lod4-1-3-2"></a>
##### 1.3.2 Row Level Security (RLS) Isolation Policies
* **Action 1.3.2.1:** Create file `supabase/migrations/20260101000004_configure_rls_policies.sql`
* **Action 1.3.2.2:** Populate `supabase/migrations/20260101000004_configure_rls_policies.sql` with RLS policies for `integrations` and `posts`.
* **Action 1.3.2.3:** Run command `supabase db push`

---

### 2. Shared KMP Core Module

#### 2.1 Shared Module Gradle Scaffolding

<a id="lod4-2-1-1"></a>
##### 2.1.1 Shared Directory Tree Initialization
* **Action 2.1.1.1:** Run command `mkdir -p shared`
* **Action 2.1.1.2:** Run command `mkdir -p shared/src/commonMain/kotlin/com/yourcompany/model`
* **Action 2.1.1.3:** Run command `mkdir -p shared/src/commonMain/kotlin/com/yourcompany/client`

<a id="lod4-2-1-2"></a>
##### 2.1.2 Shared Module Build Dependencies
* **Action 2.1.2.1:** Create file `shared/build.gradle.kts`
* **Action 2.1.2.2:** Populate `shared/build.gradle.kts` with `kotlinx.serialization` and `ktor-client-core` dependencies.
* **Action 2.1.2.3:** Update file `settings.gradle.kts` adding `include(":shared")`.

---

#### 2.2 Data Transfer Objects (DTOs)

<a id="lod4-2-2-1"></a>
##### 2.2.1 Integration & OAuth DTO Models
* **Action 2.2.1.1:** Create file `shared/src/commonMain/kotlin/com/yourcompany/model/IntegrationDto.kt`
* **Action 2.2.1.2:** Create file `shared/src/commonMain/kotlin/com/yourcompany/model/OAuthUrlDto.kt`

<a id="lod4-2-2-2"></a>
##### 2.2.2 Media Upload & Post Scheduling DTO Models
* **Action 2.2.2.1:** Create file `shared/src/commonMain/kotlin/com/yourcompany/model/MediaUploadDto.kt`
* **Action 2.2.2.2:** Create file `shared/src/commonMain/kotlin/com/yourcompany/model/PostRequestDto.kt`
* **Action 2.2.2.3:** Create file `shared/src/commonMain/kotlin/com/yourcompany/model/PostResponseDto.kt`

---

#### 2.3 Ktor Client Service Integration

<a id="lod4-2-3-1"></a>
##### 2.3.1 Social Sync HTTP Client Configuration
* **Action 2.3.1.1:** Create file `shared/src/commonMain/kotlin/com/yourcompany/client/SocialSyncApiClient.kt`
* **Action 2.3.1.2:** Populate `shared/src/commonMain/kotlin/com/yourcompany/client/SocialSyncApiClient.kt` with Ktor client instance.

<a id="lod4-2-3-2"></a>
##### 2.3.2 Bearer Token Authentication Interceptor
* **Action 2.3.2.1:** Create file `shared/src/commonMain/kotlin/com/yourcompany/client/AuthInterceptor.kt`
* **Action 2.3.2.2:** Populate `shared/src/commonMain/kotlin/com/yourcompany/client/AuthInterceptor.kt` with Supabase JWT bearer injection logic.

---

### 3. Ktor Backend Subsystem Setup

#### 3.1 Ktor Backend Engine Scaffolding

<a id="lod4-3-1-1"></a>
##### 3.1.1 Backend Project Directory Structure
* **Action 3.1.1.1:** Run command `mkdir -p backend-ktor`
* **Action 3.1.1.2:** Run command `mkdir -p backend-ktor/src/main/kotlin/com/yourcompany/backend/routes`
* **Action 3.1.1.3:** Run command `mkdir -p backend-ktor/src/main/kotlin/com/yourcompany/backend/services`

<a id="lod4-3-1-2"></a>
##### 3.1.2 Ktor JVM Server Dependencies & Build Config
* **Action 3.1.2.1:** Create file `backend-ktor/build.gradle.kts`
* **Action 3.1.2.2:** Populate `backend-ktor/build.gradle.kts` with Ktor Server Netty and Supabase dependencies.
* **Action 3.1.2.3:** Update file `settings.gradle.kts` adding `include(":backend-ktor")`.

---

#### 3.2 Security & Authentication Middleware

<a id="lod4-3-2-1"></a>
##### 3.2.1 Supabase JWT Token Validator
* **Action 3.2.1.1:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/JwtSecurity.kt`
* **Action 3.2.1.2:** Populate `backend-ktor/src/main/kotlin/com/yourcompany/backend/JwtSecurity.kt` with JWT verification.

<a id="lod4-3-2-2"></a>
##### 3.2.2 Cron Header Authentication Guard
* **Action 3.2.2.1:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/CronSecurity.kt`
* **Action 3.2.2.2:** Populate `backend-ktor/src/main/kotlin/com/yourcompany/backend/CronSecurity.kt` verifying `X-Cron-Secret` header.

---

#### 3.3 API Route Controllers & Cron Worker

<a id="lod4-3-3-1"></a>
##### 3.3.1 OAuth Integration Route Endpoints
* **Action 3.3.3.1.1:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/routes/IntegrationRoutes.kt`
* **Action 3.3.3.1.2:** Populate `backend-ktor/src/main/kotlin/com/yourcompany/backend/routes/IntegrationRoutes.kt` with OAuth endpoints.

<a id="lod4-3-3-2"></a>
##### 3.3.2 Media Upload & Post Scheduling Endpoints
* **Action 3.3.3.2.1:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/routes/MediaRoutes.kt`
* **Action 3.3.3.2.2:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/routes/PostRoutes.kt`

<a id="lod4-3-3-3"></a>
##### 3.3.3 Background Scheduled Post Publisher Worker
* **Action 3.3.3.3.1:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/routes/CronRoutes.kt`
* **Action 3.3.3.3.2:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/services/SocialPublisherService.kt`
* **Action 3.3.3.3.3:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/Application.kt` binding route controllers.

---

### 4. Compose Multiplatform Client Application

#### 4.1 Compose Client Module Setup

<a id="lod4-4-1-1"></a>
##### 4.1.1 Client App Directory Structure
* **Action 4.1.1.1:** Run command `mkdir -p composeApp`
* **Action 4.1.1.2:** Run command `mkdir -p composeApp/src/commonMain/kotlin/com/yourcompany/ui`

<a id="lod4-4-1-2"></a>
##### 4.1.2 Compose Multiplatform Build Setup
* **Action 4.1.1.3:** Create file `composeApp/build.gradle.kts`
* **Action 4.1.1.4:** Populate `composeApp/build.gradle.kts` with targets (Android, iOS, WasmJs).
* **Action 4.1.1.5:** Update file `settings.gradle.kts` adding `include(":composeApp")`.

---

#### 4.2 UI Screens & ViewModels

<a id="lod4-4-2-1"></a>
##### 4.2.1 Connected Social Accounts Screen & ViewModel
* **Action 4.2.1.1:** Create file `composeApp/src/commonMain/kotlin/com/yourcompany/ui/IntegrationsScreen.kt`
* **Action 4.2.1.2:** Create file `composeApp/src/commonMain/kotlin/com/yourcompany/ui/IntegrationsViewModel.kt`

<a id="lod4-4-2-2"></a>
##### 4.2.2 Video Upload & Scheduling Screen & ViewModel
* **Action 4.2.2.1:** Create file `composeApp/src/commonMain/kotlin/com/yourcompany/ui/CreatePostScreen.kt`
* **Action 4.2.2.2:** Create file `composeApp/src/commonMain/kotlin/com/yourcompany/ui/CreatePostViewModel.kt`

---

#### 4.3 Multiplatform Entry Points (Android, iOS, Web)

<a id="lod4-4-3-1"></a>
##### 4.3.1 Mobile Native Target Hosts (Android Activity & iOS ViewController)
* **Action 4.3.1.1:** Create file `composeApp/src/androidMain/kotlin/com/yourcompany/MainActivity.kt`
* **Action 4.3.1.2:** Create file `composeApp/src/iosMain/kotlin/com/yourcompany/MainViewController.kt`

<a id="lod4-4-3-2"></a>
##### 4.3.2 Web Target Main Entry Point (Wasm JS)
* **Action 4.3.2.1:** Create file `composeApp/src/wasmJsMain/kotlin/com/yourcompany/main.kt`
* **Action 4.3.2.2:** Create file `composeApp/src/wasmJsMain/resources/index.html`

---

### 5. Deployment & Infrastructure

#### 5.1 Vercel Serverless & Cron Configuration

<a id="lod4-5-1-1"></a>
##### 5.1.1 Serverless Deployment Config File
* **Action 5.1.1.1:** Create file `backend-ktor/vercel.json`
* **Action 5.1.1.2:** Populate `backend-ktor/vercel.json` with JVM build and daily cron definition.

<a id="lod4-5-1-2"></a>
##### 5.1.2 Vercel CLI Project Linking
* **Action 5.1.2.1:** Run command `vercel link --yes`
* **Action 5.1.2.2:** Run command `vercel env pull .env.local`

---

#### 5.2 Build Verification & Production Deployment

<a id="lod4-5-2-1"></a>
##### 5.2.1 Gradle Suite Compilation & Test Run
* **Action 5.2.1.1:** Run command `./gradlew check`
* **Action 5.2.1.2:** Run command `./gradlew :backend-ktor:build`

<a id="lod4-5-2-2"></a>
##### 5.2.2 Production Deployment Execution
* **Action 5.2.2.1:** Run command `supabase db push --linked`
* **Action 5.2.2.2:** Run command `vercel --prod`