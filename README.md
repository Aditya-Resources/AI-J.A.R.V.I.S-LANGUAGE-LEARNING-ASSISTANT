# 🤖 AI J.A.R.V.I.S. – Language Learning Assistant

**An AI-powered Android application designed to make language learning interactive, accessible, and engaging.**

AI J.A.R.V.I.S. is a mobile language learning assistant that combines AI-powered conversations, voice interaction, structured lessons, daily quizzes, learning progress tracking, and persistent chat history in a single Android application.

The application uses Firebase for authentication and cloud data storage, OpenRouter for AI-powered conversations, and Android speech services for voice-based interaction.

## ✨ Features

- **🔐 User Authentication** – Register, log in, authenticate with Google, and request password resets using Firebase Authentication.
- **💬 AI-Powered Chat** – Practice conversations and receive AI-generated responses through the OpenRouter API.
- **🎙️ Voice Assistant** – Convert speech into text using Android SpeechRecognizer and listen to responses using TextToSpeech.
- **📚 Interactive Lessons** – Access structured language learning content through dedicated lesson screens.
- **📝 Daily Quizzes** – Test language knowledge with quizzes and earn experience points (XP).
- **📊 Learning Dashboard** – View learning progress, XP, daily goals, and quick actions.
- **🗂️ Chat History** – Save, search, rename, delete, and reopen previous conversations.
- **☁️ Cloud Synchronization** – Store user profiles and conversations in Cloud Firestore.
- **💾 Local Persistence** – Store conversation history locally using SharedPreferences and Gson.
- **🎨 Personalization** – Select a target language, change the theme, and manage account settings.
- **📱 Modern UI** – Use an Android interface built around Material Design 3, fragments, and RecyclerView.
- **🛡️ Error Handling** – Handle network failures, unsuccessful API responses, and missing response data.

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| Android SDK | Android application development |
| Java / Kotlin | Application programming and build configuration, as applicable to the project |
| Material Design 3 | User interface components and styling |
| Firebase Authentication | User registration and sign-in |
| Cloud Firestore | Cloud-based profile and conversation storage |
| OpenRouter API | AI-powered chat responses |
| Retrofit | HTTP API communication |
| OkHttp | Network client and timeout configuration |
| Android SpeechRecognizer | Speech-to-text conversion |
| Android TextToSpeech | Spoken AI responses |
| RecyclerView | Chat message and history lists |
| SharedPreferences | Local conversation storage |
| Gson | JSON serialization and deserialization |
| Gradle | Build automation |

## 🏗️ Application Architecture

The application follows a modular architecture that separates user interface components, API communication, data persistence, and utility functions.

### Main Components

- **Activities:** Manage authentication, the main application screen, quizzes, lesson details, and voice interaction.
- **Fragments:** Provide Home, Learn, Chat, History, and Profile sections.
- **AI Layer:** Handles OpenRouter API requests, responses, and network configuration.
- **Repository Layer:** Manages conversation history, user profiles, progress, and settings.
- **Data Models:** Represent chat messages, conversations, and AI API request and response data.
- **Utility Classes:** Format Markdown and prepare text for speech output.

### High-Level Architecture

```text
                 ┌──────────────────────┐
                 │      Android UI      │
                 │ Activities/Fragments │
                 └──────────┬───────────┘
                            │
             ┌──────────────┼──────────────┐
             │              │              │
             ▼              ▼              ▼
       AI Chat Layer   Repository Layer  Voice Assistant
             │              │              │
             ▼              ▼              ▼
        OpenRouter      Firebase /       Android Speech
            API          Local Storage    Services
             │              │              │
             └──────────────┼──────────────┘
                            ▼
                 Response and UI Updates
```

## 📱 Application Modules

### 1. Authentication
- Splash screen and authentication session checking.
- Email/password registration and login.
- Google sign-in integration.
- Password reset functionality.

### 2. Home Dashboard
- Personalized greeting.
- XP and learning progress indicators.
- Daily goals and quick actions.
- Shortcuts to chat, lessons, quizzes, history, and settings.

### 3. AI Chat
- Submit text prompts to the AI assistant.
- Display AI-generated responses.
- Format supported Markdown content.
- Handle API errors and provide retry options.

### 4. Voice Assistant
- Capture spoken user input.
- Convert speech into text.
- Send transcribed messages to the AI service.
- Read AI responses aloud.
- Save voice conversations to history.

### 5. Learning and Quizzes
- Browse language lessons.
- Open lesson details.
- Attempt daily quizzes.
- Track XP and learning progress according to the implemented rules.

### 6. Conversation History
- Save conversations locally and remotely.
- Search previous conversations.
- Rename and delete individual conversations.
- Clear all history with confirmation.
- Reopen saved conversations.

### 7. Profile and Settings
- Manage target-language preferences.
- Change the application theme.
- Access profile and progress information.
- Log out of the application.

## 🔄 Chat Workflow

1. The user enters a message in the chat interface.
2. The application displays the submitted message.
3. The AI layer sends a request to the configured OpenRouter chat-completions endpoint.
4. The response is validated and parsed.
5. The formatted response is displayed to the user.
6. The conversation is saved to local storage and Cloud Firestore according to the implemented persistence logic.

## 🔌 API Integration

The application uses the OpenRouter chat-completions endpoint:

`https://openrouter.ai/api/v1/chat/completions`

AI responses require internet access and valid API configuration.

**Security note:** Do not commit API keys, Firebase service-account credentials, signing keys, or other secrets to a public repository. Review the API credential architecture before releasing the application.

## 📂 Project Structure

The following is a representative structure based on the documented components. Verify the actual repository before treating it as an exact file listing.

```text
AI-JARVIS-Language-Learning-Assistant/
├── app/
│   ├── src/
│   │   └── main/
│   │       ├── java/com/jarvis/languageassistant/
│   │       │   ├── MainActivity.java
│   │       │   ├── SplashActivity.java
│   │       │   ├── LoginActivity.java
│   │       │   ├── RegisterActivity.java
│   │       │   ├── ForgotPasswordActivity.java
│   │       │   ├── QuizActivity.java
│   │       │   ├── LessonDetailActivity.java
│   │       │   ├── VoiceAssistantActivity.java
│   │       │   ├── ChatAdapter.java
│   │       │   ├── HistoryAdapter.java
│   │       │   ├── ChatMessage.java
│   │       │   ├── Conversation.java
│   │       │   ├── ChatHistoryRepository.java
│   │       │   ├── FirestoreRepository.java
│   │       │   ├── ai/
│   │       │   │   ├── OpenRouterApiClient.java
│   │       │   │   ├── OpenRouterApiService.java
│   │       │   │   ├── OpenRouterRequest.java
│   │       │   │   └── OpenRouterResponse.java
│   │       │   ├── fragments/
│   │       │   │   ├── HomeFragment.java
│   │       │   │   ├── LearnFragment.java
│   │       │   │   ├── ChatFragment.java
│   │       │   │   ├── HistoryFragment.java
│   │       │   │   └── ProfileFragment.java
│   │       │   └── utils/
│   │       │       ├── JarvisTextUtils.java
│   │       │       └── MarkdownFormatter.java
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts
├── build.gradle.kts
├── settings.gradle.kts
├── gradle/
├── gradlew
├── gradlew.bat
├── README.md
└── .gitignore
```

## ⚙️ Prerequisites

Before building the project, ensure you have:

- Android Studio with a compatible Android SDK and JDK.
- A compatible Gradle wrapper and Android Gradle Plugin.
- A configured Firebase project.
- Firebase Authentication and Cloud Firestore enabled.
- Valid OpenRouter API configuration.
- An Android device or emulator for testing.
- Internet connectivity for AI requests and cloud operations.

The documented minimum Android SDK is API 26, with target SDK 34 and compile SDK 37. Verify these values and all dependency versions against the actual project configuration before building.

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/AI-JARVIS-Language-Learning-Assistant.git
cd AI-JARVIS-Language-Learning-Assistant
```

Replace `YOUR_USERNAME` with your GitHub username and adjust the repository URL if necessary.

### 2. Open in Android Studio

Open the cloned project in Android Studio and allow Gradle synchronization to complete.

### 3. Configure Firebase

1. Create or select a Firebase project.
2. Register the Android application using its actual application ID.
3. Add the required Firebase configuration file.
4. Enable the authentication providers used by the application.
5. Configure Cloud Firestore.
6. Review and deploy appropriate Firestore security rules.

### 4. Configure OpenRouter

Configure the OpenRouter API credentials using the project's existing configuration mechanism.

Do not place a real API key in a committed source file or publish it in your README. For a production application, avoid relying on a long-lived secret embedded in the distributed APK; use an appropriate backend or secure credential architecture.

### 5. Build the Debug APK

On Windows:

```powershell
.\gradlew.bat assembleDebug
```

On macOS or Linux:

```bash
./gradlew assembleDebug
```

The expected output location is:

`app/build/outputs/apk/debug/app-debug.apk`

### 6. Build the Release APK

Configure release signing securely, then run:

**Windows:**
```powershell
.\gradlew.bat assembleRelease
```

**macOS/Linux:**
```bash
./gradlew assembleRelease
```

The expected output location is:

`app/build/outputs/apk/release/app-release.apk`

Actual output paths may differ depending on the project's Gradle configuration.

## 🧪 Testing

Recommended testing areas include:

- Authentication, registration, password reset, and logout.
- Navigation between the five main sections.
- AI request and response handling.
- Network timeouts and API error responses.
- Empty or malformed API responses.
- Speech recognition and text-to-speech.
- Local and cloud conversation persistence.
- Conversation search, rename, deletion, and restoration.
- Theme and target-language settings.
- Debug and release APK generation.

Tests should be executed on supported emulators and physical devices. Document actual results rather than assuming that the listed scenarios have passed.

## ⚠️ Limitations

- AI chat depends on OpenRouter availability and network connectivity.
- Voice functionality depends on device support, permissions, and available speech services.
- Firebase functionality requires correct project configuration and security rules.
- Lesson content, quiz scoring, XP progression, and supported languages depend on the implemented application logic.
- Offline synchronization and local/cloud conflict resolution require further verification.

## 🔮 Future Enhancements

- Personalized learning paths and lesson recommendations.
- More language options and language-specific speech settings.
- Detailed learning analytics and progress reports.
- Improved offline access and synchronization.
- Automated unit and integration testing.
- Accessibility improvements for screen readers, contrast, and text sizing.
- More transparent quiz scoring and XP progression.
- Improved API credential management and privacy controls.

## 🔒 Security and Privacy

- Never commit API keys, passwords, signing keystores, or private credentials.
- Use appropriate Firestore security rules to restrict access to each user's data.
- Avoid logging sensitive user information or authorization headers.
- Review the application's privacy and data-retention practices before public release.
- Ensure configuration files containing sensitive information are handled appropriately.

## 🤝 Contributing

Contributions, suggestions, and bug reports are welcome.

1. Fork the repository.
2. Create a feature branch.
3. Make and test your changes.
4. Commit your changes with a descriptive message.
5. Open a pull request explaining the changes.

## 📄 License

No license has been specified yet. Add a `LICENSE` file before granting others explicit permission to reuse, modify, or distribute the project.

## 👨‍💻 Author

**Your Name**

- GitHub: [@YOUR_USERNAME](https://github.com/YOUR_USERNAME)

Replace the placeholders with your actual details.

---

**AI J.A.R.V.I.S. – Learn, Practice, and Improve with AI.**
