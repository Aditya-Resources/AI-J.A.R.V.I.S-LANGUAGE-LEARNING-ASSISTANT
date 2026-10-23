# AI J.A.R.V.I.S. – Language Learning Assistant
## Comprehensive Project Documentation & Step-by-Step Architecture Guide

---

## 1. Project Overview
**AI J.A.R.V.I.S.** is an advanced, intelligent Android language learning assistant designed to provide personalized multilingual tutoring, interactive AI chat practice, voice conversational capabilities, structured lessons, daily AI quizzes, and robust persistent chat history.

---

## 2. Tech Stack & Core Dependencies
- **Platform:** Android (Min SDK 26, Target SDK 34, Compile SDK 37)
- **Build System:** Android Gradle Plugin (AGP) `9.4.1`, Gradle Wrapper `9.8.1`, Kotlin `2.4.21`
- **Backend & Authentication:** Firebase Authentication & Cloud Firestore
- **AI Integration:** OpenRouter API (`https://openrouter.ai/api/v1/chat/completions`) using Retrofit `3.0` & OkHttp `5.5` with 60-second timeouts
- **Voice Services:** Android `SpeechRecognizer` (Speech-to-Text) & `TextToSpeech` (Text-to-Speech synthesis)
- **UI & Architecture:** Material Design 3, Fragment-based Navigation, RecyclerView, Markdown Rich-Text Formatting (`Html.fromHtml`)

---

## 3. Project Directory Structure
```text
com.jarvis.languageassistant/
├── MainActivity.java                # Main container with BottomNavigationView
├── SplashActivity.java              # Initial auth-checking splash screen
├── LoginActivity.java               # Secure email/password & Credential Manager Google Sign-In
├── RegisterActivity.java            # New account initialization
├── ForgotPasswordActivity.java      # Password reset flow
├── QuizActivity.java                # Daily AI language quiz & XP rewards
├── LessonDetailActivity.java        # Structured lesson viewer
├── VoiceAssistantActivity.java      # Live voice conversation with OpenRouter AI & TTS
├── ChatAdapter.java                 # RecyclerView adapter for user & AI chat bubbles
├── HistoryAdapter.java              # RecyclerView adapter for saved conversations
├── ChatMessage.java                 # Data model for individual chat messages
├── Conversation.java                # Data model for conversation sessions
├── ChatHistoryRepository.java       # Dual persistence repository (Local SharedPreferences JSON + Firestore)
├── FirestoreRepository.java         # User profile, progress, and settings management
├── ai/
│   ├── OpenRouterApiClient.java     # Retrofit client with OkHttp 60s timeout & logging
│   ├── OpenRouterApiService.java    # Retrofit API interface for POST /api/v1/chat/completions
│   ├── OpenRouterRequest.java       # Request payload data models
│   └── OpenRouterResponse.java      # Response payload data models
├── fragments/
│   ├── HomeFragment.java            # Dashboard with daily goals, XP, and quick actions
│   ├── ChatFragment.java            # Interactive chat screen with Markdown rendering & AI integration
│   ├── HistoryFragment.java         # Dedicated history screen with real-time search, rename, and delete
│   ├── LearnFragment.java           # Curated language lessons list
│   └── ProfileFragment.java         # User settings, target language selector, theme toggle, and logout
└── utils/
    ├── JarvisTextUtils.java         # Text cleaning utility for natural speech synthesis
    └── MarkdownFormatter.java       # Rich-text markdown converter for chat UI rendering
```

---

## 4. Step-by-Step Implementation Guide

### Step 1: Authentication & User Initialization
- **Activities:** `SplashActivity`, `LoginActivity`, `RegisterActivity`, `ForgotPasswordActivity`.
- **Workflow:** 
  - `SplashActivity` checks if a Firebase user session exists (`FirebaseAuth.getInstance().getCurrentUser()`). If authenticated, routes to `MainActivity`; otherwise, routes to `LoginActivity`.
  - `LoginActivity` supports email/password authentication and Google Sign-In via Android Credential Manager (`GetGoogleIdOption`).
  - Upon successful registration or login, user profile documents are initialized in Firestore (`users/{uid}`) with default streak, XP (`100`), level (`Beginner`), and target language (`English`).

### Step 2: Dashboard & Navigation
- **Containers:** `MainActivity` and `HomeFragment`.
- **Workflow:**
  - `MainActivity` hosts a `BottomNavigationView` connecting `HomeFragment`, `LearnFragment`, `ChatFragment`, `HistoryFragment`, and `ProfileFragment`.
  - `HomeFragment` displays personalized greetings, daily goal progress bars, XP counters, and quick action buttons for Voice Assistant, New Chat, Chat History, Language Selection, Daily Quiz, and Settings.

### Step 3: AI Chat System & OpenRouter Integration
- **Components:** `ChatFragment`, `ChatAdapter`, `OpenRouterApiClient`.
- **Workflow:**
  - Users type a message or use speech input.
  - Tapping **Send** triggers `sendChatMessage()`, which immediately inserts the user message into the chat UI (`renderChatMessage()`) and sends a POST request to OpenRouter (`/api/v1/chat/completions`).
  - `parseAIResponse()` safely extracts `choices[0].message.content`.
  - `MarkdownFormatter` parses Markdown formatting (`**bold**`, lists, headings, and tables) into rich Android `Spanned` text for clean mobile rendering.
  - If a request fails, the user's typed message is restored, and an error message with retry options is displayed.

### Step 4: Persistent Chat History System
- **Components:** `ChatHistoryRepository`, `HistoryFragment`, `HistoryAdapter`, `Conversation`.
- **Workflow:**
  - Conversations are automatically saved both locally (via SharedPreferences + Gson JSON storage) and in Firestore (`users/{uid}/conversations/{id}`).
  - `HistoryFragment` loads all saved conversations sorted by most recently updated (`updatedAt`).
  - Supports real-time search filtering, conversation renaming, single deletion, and clearing all history with confirmation dialogs.
  - Tapping any conversation restores its complete message sequence in `ChatFragment`.

### Step 5: Voice Assistant & Text-to-Speech
- **Components:** `VoiceAssistantActivity`, `JarvisTextUtils`.
- **Workflow:**
  - `VoiceAssistantActivity` listens for voice input via Android `SpeechRecognizer`.
  - Transcribed queries are sent to OpenRouter AI.
  - `JarvisTextUtils.cleanTextForSpeech()` strips markdown, code blocks, and symbol noise into natural conversational sentences.
  - Android `TextToSpeech` speaks the cleaned response aloud while saving the voice conversation to history.

---

## 5. Error Handling & Resilience
- **Timeout Protection:** OkHttp client configured with 60-second timeouts.
- **HTTP Status Mapping:** Explicit error parsing for HTTP 401, 403, 404, 429, and 5xx errors with user-friendly diagnostics.
- **Crash Prevention:** Extensive null-checks on API response objects (`response.body()`, `choices`, `message`) and lifecycle state checks (`isAdded()`, `isFinishing()`) prevent unhandled exceptions.

---

## 6. Build & Deployment Instructions

### Generating a Debug APK (Testing)
1. Open the project in **Android Studio**.
2. Run `gradle_sync` to synchronize project structure.
3. In the terminal or command prompt at project root:
   ```powershell
   .\gradlew assembleDebug
   ```
4. Output APK location: `app/build/outputs/apk/debug/app-debug.apk`.

### Generating a Release APK
1. Configure your signing keystore in `app/build.gradle.kts`.
2. Run:
   ```powershell
   .\gradlew assembleRelease
   ```
3. Output APK location: `app/build/outputs/apk/release/app-release.apk`.
