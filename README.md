# 🎬 YT Channel Manager - تطبيق إدارة قناة يوتيوب الاحترافي

<div align="center">

![YouTube](https://img.shields.io/badge/YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white)
![Kotlin](https://img.shields.io/badge/Kotlin-0095D5?style=for-the-badge&logo=kotlin&logoColor=white)
![Android](https://img.shields.io/badge/Android-3DDC84?style=for-the-badge&logo=android&logoColor=white)
![Jetpack Compose](https://img.shields.io/badge/Jetpack%20Compose-4285F4?style=for-the-badge&logo=jetpackcompose&logoColor=white)

</div>

## 📋 جدول المحتويات
- [المميزات](#-المميزات)
- [اللقطات](#-لقطات-من-التطبيق)
- [التقنيات](#-التقنيات-المستخدمة)
- [الإعداد](#-خطوات-الإعداد)
- [البناء](#-بناء-التطبيق)
- [الهيكل](#-هيكل-المشروع)
- [API](#-إعداد-youtube-api)
- [OAuth](#-تسجيل-الدخول-بgoogle)
- [المشاكل](#-حل-المشاكل-الشائعة)

---

## ✨ المميزات

| الميزة | الوصف | الحالة |
|--------|-------|--------|
| 🎬 **تشغيل الفيديوهات** | مشغل YouTube رسمي (IFrame API) | ✅ جاهز |
| 🔍 **بحث متقدم** | بحث فوري + Paging 3 | ✅ جاهز |
| 💬 **التعليقات** | عرض وإضافة تعليقات | ✅ جاهز |
| 📂 **قوائم التشغيل** | إنشاء وإدارة القوائم | ✅ جاهز |
| ⬇️ **التحميل** | تحميل في الخلفية + إشعارات | ✅ جاهز |
| 📊 **إدارة القناة** | إحصائيات + رفع فيديوهات | ✅ جاهز |
| 🌙 **الوضع الليلي** | دعم Material You | ✅ جاهز |
| 🔐 **تسجيل الدخول** | Google Sign-In (OAuth 2.0) | ✅ جاهز |

---

## 📸 لقطات من التطبيق

```
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│   🏠 الرئيسية   │  │   🔍 البحث    │  │   🎬 المشغل    │
│  ┌───────────┐  │  │  ┌───────────┐  │  │  ┌───────────┐  │
│  │  فيديو 1  │  │  │  │ بحث...    │  │  │  │  YouTube  │  │
│  ├───────────┤  │  │  ├───────────┤  │  │  │  Player   │  │
│  │  فيديو 2  │  │  │  │ نتيجة 1   │  │  │  ├───────────┤  │
│  ├───────────┤  │  │  │ نتيجة 2   │  │  │  │ إعجاب 💬  │  │
│  │  فيديو 3  │  │  │  │ نتيجة 3   │  │  │  │ ⬇️ تحميل  │  │
│  └───────────┘  │  │  └───────────┘  │  │  └───────────┘  │
│  ⬇️⬇️⬇️⬇️⬇️   │  │                 │  │                 │
└─────────────────┘  └─────────────────┘  └─────────────────┘
```

---

## 🛠️ التقنيات المستخدمة

### Architecture
- **Clean Architecture** - فصل المسؤوليات
- **MVVM** - Model-View-ViewModel
- **Repository Pattern** - abstraction layer

### Core
- **Kotlin 2.0** - لغة البرمجة
- **Coroutines + Flow** - Async programming
- **Hilt** - Dependency Injection

### UI
- **Jetpack Compose** - Declarative UI
- **Material Design 3** - تصميم عصري
- **Coil** - تحميل الصور

### Data
- **Retrofit 2** - HTTP client
- **Kotlin Serialization** - JSON parsing
- **Room** - Local database
- **Paging 3** - تحميل تدريجي

### Media
- **android-youtube-player** - مشغل YouTube رسمي
- **ExoPlayer** - للفيديوهات المحملة
- **WorkManager** - تحميل في الخلفية

### Auth
- **Credential Manager** - Google Sign-In
- **OAuth 2.0** - YouTube API access

---

## 🚀 خطوات الإعداد

### المتطلبات
- Android Studio Hedgehog (2023.1.1) أو أحدث
- JDK 17
- Android SDK 34
- Gradle 8.6

### 1. استنساخ المشروع
```bash
git clone https://github.com/yourusername/ytchannel-manager.git
cd ytchannel-manager
```

### 2. إعداد YouTube API

#### A. إنشاء مشروع Google Cloud
1. اذهب إلى [Google Cloud Console](https://console.cloud.google.com/)
2. أنشئ **New Project**
3. فعّل **YouTube Data API v3**

#### B. API Key
1. **Credentials** → **Create Credentials** → **API Key**
2. قيّده بتطبيق Android:
   - Package name: `com.ytchannel.app`
   - SHA-1: `keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android`

#### C. OAuth Client ID (لإدارة القناة)
1. **Credentials** → **Create Credentials** → **OAuth client ID**
2. اختر **Android**
3. أضف نفس Package name و SHA-1

### 3. تعديل الإعدادات

#### `app/build.gradle.kts`
```kotlin
buildConfigField("String", "YOUTUBE_API_KEY", '"AIzaSy...YOUR_KEY"')
buildConfigField("String", "OAUTH_CLIENT_ID", '"123456...apps.googleusercontent.com"')
```

#### `local.properties`
```properties
sdk.dir=/Users/YOUR_NAME/Library/Android/sdk
```

#### معرف القناة في `MainActivity.kt`
```kotlin
channelId = "UC...YOUR_CHANNEL_ID"
```

### 4. بناء المشروع
```bash
# Linux/macOS
./gradlew assembleDebug

# Windows
gradlew.bat assembleDebug
```

---

## 📁 هيكل المشروع

```
ytchannel_app/
├── 📄 build.gradle.kts              # Root build
├── 📄 settings.gradle.kts           # Project settings
├── 📄 gradle.properties             # Gradle config
├── 📄 local.properties              # SDK path (local)
├── 📄 README.md                     # This file
├── 📄 SETUP.md                      # Detailed setup guide
│
├── 📁 gradle/wrapper/               # Gradle wrapper
│
├── 📁 app/
│   ├── 📄 build.gradle.kts          # App dependencies
│   ├── 📄 proguard-rules.pro        # ProGuard rules
│   │
│   └── 📁 src/main/
│       ├── 📄 AndroidManifest.xml   # App manifest
│       │
│       ├── 📁 java/com/ytchannel/app/
│       │   ├── 📄 YTChannelApp.kt          # Application class
│       │   │
│       │   ├── 📁 domain/                  # Business logic
│       │   │   ├── 📁 model/               # Data models
│       │   │   ├── 📁 repository/          # Interfaces
│       │   │   └── 📁 usecase/             # Use cases
│       │   │
│       │   ├── 📁 data/                    # Data layer
│       │   │   ├── 📁 remote/              # API + DTOs
│       │   │   ├── 📁 local/               # Room Database
│       │   │   ├── 📁 paging/              # PagingSources
│       │   │   ├── 📁 mapper/              # DTO → Domain
│       │   │   └── 📁 repository/          # Implementations
│       │   │
│       │   ├── 📁 presentation/            # UI layer
│       │   │   ├── 📄 MainActivity.kt      # Entry point
│       │   │   ├── 📁 screens/             # Composables
│       │   │   ├── 📁 components/          # Reusable UI
│       │   │   ├── 📁 navigation/          # Routes
│       │   │   ├── 📁 theme/               # Colors/Typography
│       │   │   ├── 📁 viewmodel/           # ViewModels
│       │   │   └── 📁 auth/                # Google Sign-In
│       │   │
│       │   ├── 📁 di/                      # Hilt modules
│       │   ├── 📁 worker/                  # Background work
│       │   └── 📁 utils/                   # Extensions
│       │
│       └── 📁 res/                         # Resources
│           ├── 📁 values/                  # Strings/Colors/Themes
│           ├── 📁 drawable/                # Images
│           └── 📁 xml/                     # Config files
```

---

## 🔑 إعداد YouTube API

### الحصص (Quota)
| العملية | الوحدات المستهلكة |
|---------|------------------|
| Search | 100 |
| Video list | 1 |
| Comment threads | 1 |
| Upload video | 1600 |

**المجموع اليومي: 10,000 وحدة**

### النطاقات المطلوبة (OAuth)
```
https://www.googleapis.com/auth/youtube.readonly     # قراءة
https://www.googleapis.com/auth/youtube.force-ssl    # تعليقات
https://www.googleapis.com/auth/youtube              # إدارة كاملة
https://www.googleapis.com/auth/youtube.upload       # رفع
```

---

## 🔐 تسجيل الدخول بـ Google

### التدفق:
```
المستخدم يضغط "تسجيل الدخول"
    ↓
Credential Manager يعرض حسابات Google
    ↓
المستخدم يختار حساب
    ↓
Google يرسل ID Token
    ↓
التطبيق يحفظ Token في DataStore
    ↓
API calls تستخدم Token للمصادقة
```

### الشاشات التي تتطلب تسجيل دخول:
- ✅ إضافة تعليق
- ✅ إنشاء قائمة تشغيل
- ✅ رفع فيديو
- ✅ تعديل بيانات القناة

---

## 🐛 حل المشاكل الشائعة

### "API key not valid"
```bash
# تأكد من:
1. تفعيل YouTube Data API v3 ✅
2. صحة المفتاح في build.gradle.kts ✅
3. عدم وجود قيود IP ✅
```

### "Unresolved reference: BuildConfig"
```bash
./gradlew clean build
# أو
Build → Rebuild Project
```

### "Quota exceeded"
```
الحل: انتظر 24 ساعة أو اطلب زيادة الحصة
من: https://console.cloud.google.com/apis/api/youtube.googleapis.com/quotas
```

### "ExoPlayer not playing YouTube links"
```
ملاحظة: ExoPlayer لا يدعم روابط YouTube مباشرة
الحل: استخدم android-youtube-player library (مُضمن في المشروع)
```

### "Gradle sync failed"
```bash
# 1. تحقق من اتصال الإنترنت
# 2. جرب:
./gradlew --stop
./gradlew clean
# 3. File → Invalidate Caches / Restart
```

---

## 📈 خارطة الطريق (Roadmap)

### المرحلة 1 (الحالية)
- [x] تشغيل الفيديوهات
- [x] البحث
- [x] التعليقات
- [x] قوائم التشغيل
- [x] التحميل
- [x] إدارة القناة

### المرحلة 2 (مستقبلية)
- [ ] Shorts/Reels
- [ ] Live Streaming
- [ ] Chromecast
- [ ] Picture-in-Picture
- [ ] Offline playback
- [ ] Push notifications

### المرحلة 3 (متقدمة)
- [ ] AI recommendations
- [ ] Analytics dashboard
- [ ] Multi-channel support
- [ ] Team collaboration
- [ ] Monetization tracking

---

## 🤝 المساهمة

نرحب بالمساهمات! للمساهمة:
1. Fork المشروع
2. أنشئ branch جديد (`git checkout -b feature/amazing-feature`)
3. Commit التغييرات (`git commit -m 'Add amazing feature'`)
4. Push إلى Branch (`git push origin feature/amazing-feature`)
5. افتح Pull Request

---

## 📄 الترخيص

```
MIT License

Copyright (c) 2024 YT Channel Manager

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

---

## 🙏 شكر خاص

- [Pierfrancesco Soffritti](https://github.com/PierfrancescoSoffritti) - android-youtube-player library
- [Google](https://developers.google.com/youtube) - YouTube Data API
- [Jetpack Compose Team](https://developer.android.com/jetpack/compose) - UI framework

---

<div align="center">

**Made with ❤️ and Kotlin**

[⬆ Back to Top](#-yt-channel-manager---تطبيق-إدارة-قناة-يوتيوب-الاحترافي)

</div>
