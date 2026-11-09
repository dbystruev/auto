# Social Sync — Comprehensive Implementation Plan

This document outlines the step-by-step technical execution plan for building the **Social Sync** Kotlin Multiplatform application, Ktor JVM backend, Supabase database vault, and Vercel serverless cron infrastructure.

---

## 🗂 High-Level Navigation Index (Level 1)

* [#1. Database & Vault Subsystem Setup](#1-database--vault-subsystem-setup)
* [#2. Shared KMP Core Module](#2-shared-kmp-core-module)
* [#3. Ktor Backend Subsystem Setup](#3-ktor-backend-subsystem-setup)
* [#4. Compose Multiplatform Client Application](#4-compose-multiplatform-client-application)
* [#5. Deployment & Automation Infrastructure](#5-deployment--automation-infrastructure)

---

## 1. Database & Vault Subsystem Setup

### 1.1 Local Supabase Environment Scaffolding (Level 2)
* [#1.1.1 Directory Initialization](#111-directory-initialization)
* [#1.1.2 Project Initialization](#112-project-initialization)

### 1.2 Database Migrations (Level 2)
* [#1.2.1 Core Relational Tables Migration](#121-core-relational-tables-migration)
* [#1.2.2 Media Storage Migration](#122-media-storage-migration)

### 1.3 Vault Encryption & Row-Level Security (Level 2)
* [#1.3.1 Token Encryption Vault Setup](#131-token-encryption-vault-setup)
* [#1.3.2 RLS Access Policies Configuration](#132-rls-access-policies-configuration)

---

### 1.1.1 Directory Initialization (Level 3)
* [#1.1.1.1 Migration Folder Setup](#1111-migration-folder-setup)

#### 1.1.1.1 Migration Folder Setup (Level 4)
* [#1.1.1.1.1 Local Path Creation](#11111-local-path-creation)

##### 1.1.1.1.1 Local Path Creation (Level 5)
* [#1.1.1.1.1.1 Action Items](#111111-action-items)

###### 1.1.1.1.1.1 Action Items (Level 6)
* <a id="step-1-1-1-1-1-1-1"></a>**Step 1.1.1.1.1.1.1:** Run command `mkdir -p supabase`
* <a id="step-1-1-1-1-1-1-2"></a>**Step 1.1.1.1.1.1.2:** Run command `mkdir -p supabase/migrations`

---

### 1.1.2 Project Initialization (Level 3)
* [#1.1.2.1 Supabase CLI Initialization](#1121-supabase-cli-initialization)

#### 1.1.2.1 Supabase CLI Initialization (Level 4)
* [#1.1.2.1.1 Config Scaffolding](#11211-config-scaffolding)

##### 1.1.2.1.1 Config Scaffolding (Level 5)
* [#1.1.2.1.1.1 Action Items](#112111-action-items)

###### 1.1.2.1.1.1 Action Items (Level 6)
* <a id="step-1-1-2-1-1-1-1"></a>**Step 1.1.2.1.1.1.1:** Run command `supabase init`
* <a id="step-1-1-2-1-1-1-2"></a>**Step 1.1.2.1.1.1.2:** Create file `supabase/config.toml` setting project ID to `social-sync-backend`

---

### 1.2.1 Core Relational Tables Migration (Level 3)
* [#1.2.1.1 Integrations Table Migration](#1211-integrations-table-migration)
* [#1.2.1.2 Posts Table Migration](#1212-posts-table-migration)

#### 1.2.1.1 Integrations Table Migration (Level 4)
* [#1.2.1.1.1 Migration Script Creation](#12111-migration-script-creation)

##### 1.2.1.1.1 Migration Script Creation (Level 5)
* [#1.2.1.1.1.1 Action Items](#121111-action-items)

###### 1.2.1.1.1.1 Action Items (Level 6)
* <a id="step-1-2-1-1-1-1-1"></a>**Step 1.2.1.1.1.1.1:** Create file `supabase/migrations/20260101000000_create_integrations.sql`
* <a id="step-1-2-1-1-1-1-2"></a>**Step 1.2.1.1.1.1.2:** Update file `supabase/migrations/20260101000000_create_integrations.sql` adding `integrations` table SQL schema.

#### 1.2.1.2 Posts Table Migration (Level 4)
* [#1.2.1.2.1 Posts Script Creation](#12121-posts-script-creation)

##### 1.2.1.2.1 Posts Script Creation (Level 5)
* [#1.2.1.2.1.1 Action Items](#121211-action-items)

###### 1.2.1.2.1.1 Action Items (Level 6)
* <a id="step-1-2-1-2-1-1-1"></a>**Step 1.2.1.2.1.1.1:** Create file `supabase/migrations/20260101000001_create_posts_and_targets.sql`
* <a id="step-1-2-1-2-1-1-2"></a>**Step 1.2.1.2.1.1.2:** Update file `supabase/migrations/20260101000001_create_posts_and_targets.sql` adding `posts` and `post_targets` tables.

---

### 1.2.2 Media Storage Migration (Level 3)
* [#1.2.2.1 Supabase Storage Bucket Setup](#1221-supabase-storage-bucket-setup)

#### 1.2.2.1 Supabase Storage Bucket Setup (Level 4)
* [#1.2.2.1.1 Bucket Script Creation](#12211-bucket-script-creation)

##### 1.2.2.1.1 Bucket Script Creation (Level 5)
* [#1.2.2.1.1.1 Action Items](#122111-action-items)

###### 1.2.2.1.1.1 Action Items (Level 6)
* <a id="step-1-2-2-1-1-1-1"></a>**Step 1.2.2.1.1.1.1:** Create file `supabase/migrations/20260101000002_create_storage_buckets.sql`
* <a id="step-1-2-2-1-1-1-2"></a>**Step 1.2.2.1.1.1.2:** Update file `supabase/migrations/20260101000002_create_storage_buckets.sql` defining `videos` bucket rules.

---

### 1.3.1 Token Encryption Vault Setup (Level 3)
* [#1.3.1.1 PostgreSQL Vault Extension](#1311-postgresql-vault-extension)

#### 1.3.1.1 PostgreSQL Vault Extension (Level 4)
* [#1.3.1.1.1 Vault Script Creation](#13111-vault-script-creation)

##### 1.3.1.1.1 Vault Script Creation (Level 5)
* [#1.3.1.1.1.1 Action Items](#131111-action-items)

###### 1.3.1.1.1.1 Action Items (Level 6)
* <a id="step-1-3-1-1-1-1-1"></a>**Step 1.3.1.1.1.1.1:** Create file `supabase/migrations/20260101000003_enable_pgvault.sql`
* <a id="step-1-3-1-1-1-1-2"></a>**Step 1.3.1.1.1.1.2:** Update file `supabase/migrations/20260101000003_enable_pgvault.sql` adding `pgsodium` / Vault secrets table schema.

---

### 1.3.2 RLS Access Policies Configuration (Level 3)
* [#1.3.2.1 Security Isolation Policies](#1321-security-isolation-policies)

#### 1.3.2.1 Security Isolation Policies (Level 4)
* [#1.3.2.1.1 RLS Script Creation](#13211-rls-script-creation)

##### 1.3.2.1.1 RLS Script Creation (Level 5)
* [#1.3.2.1.1.1 Action Items](#132111-action-items)

###### 1.3.2.1.1.1 Action Items (Level 6)
* <a id="step-1-3-2-1-1-1-1"></a>**Step 1.3.2.1.1.1.1:** Create file `supabase/migrations/20260101000004_configure_rls_policies.sql`
* <a id="step-1-3-2-1-1-1-2"></a>**Step 1.3.2.1.1.1.2:** Update file `supabase/migrations/20260101000004_configure_rls_policies.sql` enabling RLS for `integrations` and `posts`.
* <a id="step-1-3-2-1-1-1-3"></a>**Step 1.3.2.1.1.1.3:** Run command `supabase db push`

---

## 2. Shared KMP Core Module

### 2.1 Project Initialization & Build Setup (Level 2)
* [#2.1.1 Multiplatform Module Structure](#211-multiplatform-module-structure)
* [#2.1.2 Shared Gradle Dependencies](#212-shared-gradle-dependencies)

### 2.2 Data Transfer Objects (DTOs) (Level 2)
* [#2.2.1 Authentication & Integrations DTOs](#221-authentication--integrations-dtos)
* [#2.2.2 Posts & Media DTOs](#222-posts--media-dtos)

### 2.3 Shared Ktor API Client (Level 2)
* [#2.3.1 Engine Interface & Serialization](#231-engine-interface--serialization)

---

### 2.1.1 Multiplatform Module Structure (Level 3)
* [#2.1.1.1 Folder Tree Initialization](#2111-folder-tree-initialization)

#### 2.1.1.1 Folder Tree Initialization (Level 4)
* [#2.1.1.1.1 Shared Core Path Creation](#21111-shared-core-path-creation)

##### 2.1.1.1.1 Shared Core Path Creation (Level 5)
* [#2.1.1.1.1.1 Action Items](#211111-action-items)

###### 2.1.1.1.1.1 Action Items (Level 6)
* <a id="step-2-1-1-1-1-1-1"></a>**Step 2.1.1.1.1.1.1:** Run command `mkdir -p shared`
* <a id="step-2-1-1-1-1-1-2"></a>**Step 2.1.1.1.1.1.2:** Run command `mkdir -p shared/src/commonMain/kotlin/com/yourcompany/model`

---

### 2.1.2 Shared Gradle Dependencies (Level 3)
* [#2.1.2.1 Shared Build File Setup](#2121-shared-build-file-setup)

#### 2.1.2.1 Shared Build File Setup (Level 4)
* [#2.1.2.1.1 Gradle Configuration](#21211-gradle-configuration)

##### 2.1.2.1.1 Gradle Configuration (Level 5)
* [#2.1.2.1.1.1 Action Items](#212111-action-items)

###### 2.1.2.1.1.1 Action Items (Level 6)
* <a id="step-2-1-2-1-1-1-1"></a>**Step 2.1.2.1.1.1.1:** Create file `shared/build.gradle.kts`
* <a id="step-2-1-2-1-1-1-2"></a>**Step 2.1.2.1.1.1.2:** Update file `shared/build.gradle.kts` adding `kotlinx.serialization` and `ktor-client-core` dependencies.
* <a id="step-2-1-2-1-1-1-3"></a>**Step 2.1.2.1.1.1.3:** Update file `settings.gradle.kts` appending `include(":shared")`.

---

### 2.2.1 Authentication & Integrations DTOs (Level 3)
* [#2.2.1.1 Integration Data Classes](#2211-integration-data-classes)

#### 2.2.1.1 Integration Data Classes (Level 4)
* [#2.2.1.1.1 Model File Creation](#22111-model-file-creation)

##### 2.2.1.1.1 Model File Creation (Level 5)
* [#2.2.1.1.1.1 Action Items](#221111-action-items)

###### 2.2.1.1.1.1 Action Items (Level 6)
* <a id="step-2-2-1-1-1-1-1"></a>**Step 2.2.1.1.1.1.1:** Create file `shared/src/commonMain/kotlin/com/yourcompany/model/IntegrationDto.kt`
* <a id="step-2-2-1-1-1-1-2"></a>**Step 2.2.1.1.1.1.2:** Create file `shared/src/commonMain/kotlin/com/yourcompany/model/OAuthUrlDto.kt`

---

### 2.2.2 Posts & Media DTOs (Level 3)
* [#2.2.2.1 Media & Scheduling Data Classes](#2221-media--scheduling-data-classes)

#### 2.2.2.1 Media & Scheduling Data Classes (Level 4)
* [#2.2.2.1.1 Post Model File Creation](#22211-post-model-file-creation)

##### 2.2.2.1.1 Post Model File Creation (Level 5)
* [#2.2.2.1.1.1 Action Items](#222111-action-items)

###### 2.2.2.1.1.1 Action Items (Level 6)
* <a id="step-2-2-2-1-1-1-1"></a>**Step 2.2.2.1.1.1.1:** Create file `shared/src/commonMain/kotlin/com/yourcompany/model/MediaUploadDto.kt`
* <a id="step-2-2-2-1-1-1-2"></a>**Step 2.2.2.1.1.1.2:** Create file `shared/src/commonMain/kotlin/com/yourcompany/model/PostRequestDto.kt`
* <a id="step-2-2-2-1-1-1-3"></a>**Step 2.2.2.1.1.1.3:** Create file `shared/src/commonMain/kotlin/com/yourcompany/model/PostResponseDto.kt`

---

### 2.3.1 Engine Interface & Serialization (Level 3)
* [#2.3.1.1 Shared API Client Service](#2311-shared-api-client-service)

#### 2.3.1.1 Shared API Client Service (Level 4)
* [#2.3.1.1.1 Client File Setup](#23111-client-file-setup)

##### 2.3.1.1.1 Client File Setup (Level 5)
* [#2.3.1.1.1.1 Action Items](#231111-action-items)

###### 2.3.1.1.1.1 Action Items (Level 6)
* <a id="step-2-3-1-1-1-1-1"></a>**Step 2.3.1.1.1.1.1:** Create file `shared/src/commonMain/kotlin/com/yourcompany/client/SocialSyncApiClient.kt`
* <a id="step-2-3-1-1-1-1-2"></a>**Step 2.3.1.1.1.1.2:** Update file `shared/src/commonMain/kotlin/com/yourcompany/client/SocialSyncApiClient.kt` adding Auth Bearer interceptor.

---

## 3. Ktor Backend Subsystem Setup

### 3.1 Ktor Engine Project Scaffolding (Level 2)
* [#3.1.1 Backend Module Structure](#311-backend-module-structure)
* [#3.1.2 Backend Gradle Dependencies](#312-backend-gradle-dependencies)

### 3.2 Authentication & Security Middleware (Level 2)
* [#3.2.1 JWT Verification & Cron Guard](#321-jwt-verification--cron-guard)

### 3.3 Endpoint Route Controllers (Level 2)
* [#3.3.1 Integration Routes](#331-integration-routes)
* [#3.3.2 Media & Post Routes](#332-media--post-routes)
* [#3.3.3 Cron Task Route](#333-cron-task-route)

---

### 3.1.1 Backend Module Structure (Level 3)
* [#3.1.1.1 Ktor Directory Initialization](#3111-ktor-directory-initialization)

#### 3.1.1.1 Ktor Directory Initialization (Level 4)
* [#3.1.1.1.1 Backend Directory Scaffolding](#31111-backend-directory-scaffolding)

##### 3.1.1.1.1 Backend Directory Scaffolding (Level 5)
* [#3.1.1.1.1.1 Action Items](#311111-action-items)

###### 3.1.1.1.1.1 Action Items (Level 6)
* <a id="step-3-1-1-1-1-1-1"></a>**Step 3.1.1.1.1.1.1:** Run command `mkdir -p backend-ktor`
* <a id="step-3-1-1-1-1-1-2"></a>**Step 3.1.1.1.1.1.2:** Run command `mkdir -p backend-ktor/src/main/kotlin/com/yourcompany/backend/routes`

---

### 3.1.2 Backend Gradle Dependencies (Level 3)
* [#3.1.2.1 Ktor JVM Build Config](#3121-ktor-jvm-build-config)

#### 3.1.2.1 Ktor JVM Build Config (Level 4)
* [#3.1.2.1.1 Backend Gradle Setup](#31211-backend-gradle-setup)

##### 3.1.2.1.1 Backend Gradle Setup (Level 5)
* [#3.1.2.1.1.1 Action Items](#312111-action-items)

###### 3.1.2.1.1.1 Action Items (Level 6)
* <a id="step-3-1-2-1-1-1-1"></a>**Step 3.1.2.1.1.1.1:** Create file `backend-ktor/build.gradle.kts`
* <a id="step-3-1-2-1-1-1-2"></a>**Step 3.1.2.1.1.1.2:** Update file `backend-ktor/build.gradle.kts` adding Ktor Server Netty and Supabase Java dependencies.
* <a id="step-3-1-2-1-1-1-3"></a>**Step 3.1.2.1.1.1.3:** Update file `settings.gradle.kts` appending `include(":backend-ktor")`.

---

### 3.2.1 JWT Verification & Cron Guard (Level 3)
* [#3.2.1.1 Security Plugin Implementation](#3211-security-plugin-implementation)

#### 3.2.1.1 Security Plugin Implementation (Level 4)
* [#3.2.1.1.1 Backend Middleware Files](#32111-backend-middleware-files)

##### 3.2.1.1.1 Backend Middleware Files (Level 5)
* [#3.2.1.1.1.1 Action Items](#321111-action-items)

###### 3.2.1.1.1.1 Action Items (Level 6)
* <a id="step-3-2-1-1-1-1-1"></a>**Step 3.2.1.1.1.1.1:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/Security.kt`
* <a id="step-3-2-1-1-1-1-2"></a>**Step 3.2.1.1.1.1.2:** Update file `backend-ktor/src/main/kotlin/com/yourcompany/backend/Security.kt` setting up Supabase JWT validator and `X-Cron-Secret` header guard.

---

### 3.3.1 Integration Routes (Level 3)
* [#3.3.1.1 OAuth Route Controller](#3311-oauth-route-controller)

#### 3.3.1.1 OAuth Route Controller (Level 4)
* [#3.3.1.1.1 Integration Handlers](#33111-integration-handlers)

##### 3.3.1.1.1 Integration Handlers (Level 5)
* [#3.3.1.1.1.1 Action Items](#331111-action-items)

###### 3.3.1.1.1.1 Action Items (Level 6)
* <a id="step-3-3-1-1-1-1-1"></a>**Step 3.3.1.1.1.1.1:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/routes/IntegrationRoutes.kt`
* <a id="step-3-3-1-1-1-1-2"></a>**Step 3.3.1.1.1.1.2:** Update file `backend-ktor/src/main/kotlin/com/yourcompany/backend/routes/IntegrationRoutes.kt` adding `/integrations` GET and POST callback endpoints.

---

### 3.3.2 Media & Post Routes (Level 3)
* [#3.3.2.1 Upload and Scheduling Controllers](#3321-upload-and-scheduling-controllers)

#### 3.3.2.1 Upload and Scheduling Controllers (Level 4)
* [#3.3.2.1.1 Post Route Files](#33211-post-route-files)

##### 3.3.2.1.1 Post Route Files (Level 5)
* [#3.3.2.1.1.1 Action Items](#332111-action-items)

###### 3.3.2.1.1.1 Action Items (Level 6)
* <a id="step-3-3-2-1-1-1-1"></a>**Step 3.3.2.1.1.1.1:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/routes/MediaRoutes.kt`
* <a id="step-3-3-2-1-1-1-2"></a>**Step 3.3.2.1.1.1.2:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/routes/PostRoutes.kt`

---

### 3.3.3 Cron Task Route (Level 3)
* [#3.3.3.1 Background Publisher Worker](#3331-background-publisher-worker)

#### 3.3.3.1 Background Publisher Worker (Level 4)
* [#3.3.3.1.1 Cron Controller Files](#33311-cron-controller-files)

##### 3.3.3.1.1 Cron Controller Files (Level 5)
* [#3.3.3.1.1.1 Action Items](#333111-action-items)

###### 3.3.3.1.1.1 Action Items (Level 6)
* <a id="step-3-3-3-1-1-1-1"></a>**Step 3.3.3.1.1.1.1:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/routes/CronRoutes.kt`
* <a id="step-3-3-3-1-1-1-2"></a>**Step 3.3.3.1.1.1.2:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/services/SocialPublisherService.kt`
* <a id="step-3-3-3-1-1-1-3"></a>**Step 3.3.3.1.1.1.3:** Create file `backend-ktor/src/main/kotlin/com/yourcompany/backend/Application.kt` connecting security plugin and all route modules.

---

## 4. Compose Multiplatform Client Application

### 4.1 Frontend App Structure Initialization (Level 2)
* [#4.1.1 Client App Module Setup](#411-client-app-module-setup)
* [#4.1.2 Client App Build Configuration](#412-client-app-build-configuration)

### 4.2 Shared UI Screen Implementations (Level 2)
* [#4.2.1 Authorization & Integration Screen](#421-authorization--integration-screen)
* [#4.2.2 Media Selection & Scheduling Screen](#422-media-selection--scheduling-screen)

### 4.3 Target Platforms Integration (Level 2)
* [#4.3.1 Android, iOS & Web Entry Points](#431-android-ios--web-entry-points)

---

### 4.1.1 Client App Module Setup (Level 3)
* [#4.1.1.1 Compose Directory Scaffolding](#4111-compose-directory-scaffolding)

#### 4.1.1.1 Compose Directory Scaffolding (Level 4)
* [#4.1.1.1.1 Client Directory Path Creation](#41111-client-directory-path-creation)

##### 4.1.1.1.1 Client Directory Path Creation (Level 5)
* [#4.1.1.1.1.1 Action Items](#411111-action-items)

###### 4.1.1.1.1.1 Action Items (Level 6)
* <a id="step-4-1-1-1-1-1-1"></a>**Step 4.1.1.1.1.1.1:** Run command `mkdir -p composeApp`
* <a id="step-4-1-1-1-1-1-2"></a>**Step 4.1.1.1.1.1.2:** Run command `mkdir -p composeApp/src/commonMain/kotlin/com/yourcompany/ui`

---

### 4.1.2 Client App Build Configuration (Level 3)
* [#4.1.2.1 Compose Multiplatform Gradle Config](#4121-compose-multiplatform-gradle-config)

#### 4.1.2.1 Compose Multiplatform Gradle Config (Level 4)
* [#4.1.2.1.1 App Build File Setup](#41211-app-build-file-setup)

##### 4.1.2.1.1 App Build File Setup (Level 5)
* [#4.1.2.1.1.1 Action Items](#412111-action-items)

###### 4.1.2.1.1.1 Action Items (Level 6)
* <a id="step-4-1-2-1-1-1-1"></a>**Step 4.1.2.1.1.1.1:** Create file `composeApp/build.gradle.kts`
* <a id="step-4-1-2-1-1-1-2"></a>**Step 4.1.2.1.1.1.2:** Update file `composeApp/build.gradle.kts` configuring Compose targets (Android, iOS, WasmJs).
* <a id="step-4-1-2-1-1-1-3"></a>**Step 4.1.2.1.1.1.3:** Update file `settings.gradle.kts` appending `include(":composeApp")`.

---

### 4.2.1 Authorization & Integration Screen (Level 3)
* [#4.2.1.1 Social Connections Screen](#4211-social-connections-screen)

#### 4.2.1.1 Social Connections Screen (Level 4)
* [#4.2.1.1.1 Integration UI Composable Files](#42111-integration-ui-composable-files)

##### 4.2.1.1.1 Integration UI Composable Files (Level 5)
* [#4.2.1.1.1.1 Action Items](#421111-action-items)

###### 4.2.1.1.1.1 Action Items (Level 6)
* <a id="step-4-2-1-1-1-1-1"></a>**Step 4.2.1.1.1.1.1:** Create file `composeApp/src/commonMain/kotlin/com/yourcompany/ui/IntegrationsScreen.kt`
* <a id="step-4-2-1-1-1-1-2"></a>**Step 4.2.1.1.1.1.2:** Create file `composeApp/src/commonMain/kotlin/com/yourcompany/ui/IntegrationsViewModel.kt`

---

### 4.2.2 Media Selection & Scheduling Screen (Level 3)
* [#4.2.2.1 Video Posting Screen](#4221-video-posting-screen)

#### 4.2.2.1 Video Posting Screen (Level 4)
* [#4.2.2.1.1 Posting UI Composable Files](#42211-posting-ui-composable-files)

##### 4.2.2.1.1 Posting UI Composable Files (Level 5)
* [#4.2.2.1.1.1 Action Items](#422111-action-items)

###### 4.2.2.1.1.1 Action Items (Level 6)
* <a id="step-4-2-2-1-1-1-1"></a>**Step 4.2.2.1.1.1.1:** Create file `composeApp/src/commonMain/kotlin/com/yourcompany/ui/CreatePostScreen.kt`
* <a id="step-4-2-2-1-1-1-2"></a>**Step 4.2.2.1.1.1.2:** Create file `composeApp/src/commonMain/kotlin/com/yourcompany/ui/CreatePostViewModel.kt`

---

### 4.3.1 Android, iOS & Web Entry Points (Level 3)
* [#4.3.1.1 Native Platform Entrypoints](#4311-native-platform-entrypoints)

#### 4.3.1.1 Native Platform Entrypoints (Level 4)
* [#4.3.1.1.1 Entrypoint Files Setup](#43111-entrypoint-files-setup)

##### 4.3.1.1.1 Entrypoint Files Setup (Level 5)
* [#4.3.1.1.1.1 Action Items](#431111-action-items)

###### 4.3.1.1.1.1 Action Items (Level 6)
* <a id="step-4-3-1-1-1-1-1"></a>**Step 4.3.1.1.1.1.1:** Create file `composeApp/src/androidMain/kotlin/com/yourcompany/MainActivity.kt`
* <a id="step-4-3-1-1-1-1-2"></a>**Step 4.3.1.1.1.1.2:** Create file `composeApp/src/iosMain/kotlin/com/yourcompany/MainViewController.kt`
* <a id="step-4-3-1-1-1-1-3"></a>**Step 4.3.1.1.1.1.3:** Create file `composeApp/src/wasmJsMain/kotlin/com/yourcompany/main.kt`

---

## 5. Deployment & Automation Infrastructure

### 5.1 Vercel Serverless & Cron Configuration (Level 2)
* [#5.1.1 Vercel Deployment Config](#511-vercel-deployment-config)

### 5.2 Build Verification & Deployment Pipeline (Level 2)
* [#5.2.1 Automated Build Verification](#521-automated-build-verification)

---

### 5.1.1 Vercel Deployment Config (Level 3)
* [#5.1.1.1 Vercel Configuration Setup](#5111-vercel-configuration-setup)

#### 5.1.1.1 Vercel Configuration Setup (Level 4)
* [#5.1.1.1.1 Config File Creation](#51111-config-file-creation)

##### 5.1.1.1.1 Config File Creation (Level 5)
* [#5.1.1.1.1.1 Action Items](#511111-action-items)

###### 5.1.1.1.1.1 Action Items (Level 6)
* <a id="step-5-1-1-1-1-1-1"></a>**Step 5.1.1.1.1.1.1:** Create file `backend-ktor/vercel.json` defining Kotlin JVM function builds and daily cron schedule.
* <a id="step-5-1-1-1-1-1-2"></a>**Step 5.1.1.1.1.1.2:** Run command `vercel link --yes`

---

### 5.2.1 Automated Build Verification (Level 3)
* [#5.2.1.1 Gradle & Deployment Pipeline Execution](#5211-gradle--deployment-pipeline-execution)

#### 5.2.1.1 Gradle & Deployment Pipeline Execution (Level 4)
* [#5.2.1.1.1 Deployment Execution Items](#52111-deployment-execution-items)

##### 5.2.1.1.1 Deployment Execution Items (Level 5)
* [#5.2.1.1.1.1 Action Items](#521111-action-items)

###### 5.2.1.1.1.1 Action Items (Level 6)
* <a id="step-5-2-1-1-1-1-1"></a>**Step 5.2.1.1.1.1.1:** Run command `./gradlew check`
* <a id="step-5-2-1-1-1-1-2"></a>**Step 5.2.1.1.1.1.2:** Run command `vercel --prod`