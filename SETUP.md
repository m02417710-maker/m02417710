# دليل الإعداد الكامل - YT Channel Manager

## 🎯 قبل البدء
هذا التطبيق يتطلب **مفتاح API من Google** و**OAuth Client ID** للعمل بكامل مميزاته.

---

## 1️⃣ إنشاء مفتاح API من Google

### الخطوات:
1. افتح [Google Cloud Console](https://console.cloud.google.com/)
2. أنشئ **New Project** (أو استخدم مشروع موجود)
3. من القائمة الجانبية: **APIs & Services** → **Library**
4. ابحث عن **YouTube Data API v3** واضغط **Enable**
5. اذهب إلى **Credentials** → **Create Credentials** → **API Key**
6. انسخ المفتاح واحفظه في مكان آمن

### تقييد المفتاح (مهم جداً):
- اذهب إلى Credentials → اضغط على مفتاحك → **Edit**
- في **Application restrictions** اختر **Android apps**
- أضف `com.ytchannel.app` كPackage name
- في **API restrictions** اختر **Restrict key** → حدد `YouTube Data API v3` فقط

---

## 2️⃣ إعداد OAuth 2.0 (لإدارة القناة)

### الخطوات:
1. في Google Cloud Console → **Credentials** → **Create Credentials** → **OAuth client ID**
2. اختر **Android**
3. Package name: `com.ytchannel.app`
4. SHA-1 fingerprint:
   ```bash
   # لتوليد fingerprint في Terminal:
   keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android
   ```
5. انسخ **Client ID** للاستخدام في التطبيق

---

## 3️⃣ تعديل ملفات المشروع

### A. تعديل `app/build.gradle.kts`

ابحث عن هذه الأسطر واستبدلها:
```kotlin
buildConfigField("String", "YOUTUBE_API_KEY", '"YOUR_YOUTUBE_API_KEY_HERE"')
buildConfigField("String", "OAUTH_CLIENT_ID", '"YOUR_OAUTH_CLIENT_ID.apps.googleusercontent.com"')
```

### B. تعديل `local.properties`

حدد مسار Android SDK في جهازك:
```properties
# macOS/Linux:
sdk.dir=/Users/YOUR_USERNAME/Library/Android/sdk

# Windows:
sdk.dir=C:\Users\YOUR_USERNAME\AppData\Local\Android\Sdk
```

### C. تعديل معرف القناة في `MainActivity.kt`

ابحث عن هذا السطر واستبدل بمعرف قناتك:
```kotlin
channelId = "UC_x5XG1OV2P6uZZ5FSM9Ttw", // ← استبدل هذا
```

---

## 4️⃣ استيراد المشروع في Android Studio

### الطريقة:
1. افتح Android Studio
2. **File** → **Open** → حدد مجلد `ytchannel_app`
3. انتظر حتى ينتهي Gradle sync (قد يستغرق 5-10 دقائق أول مرة)
4. إذا طلب تحديث Gradle wrapper، اضغط **Update**

### حل مشاكل Gradle Wrapper:
إذا ظهر خطأ بأن `gradle-wrapper.jar` غير موجود:
```bash
# في Terminal داخل مجلد المشروع:
gradle wrapper
```
أو انسخ `gradle-wrapper.jar` من أي مشروع Android آخر إلى:
```
ytchannel_app/gradle/wrapper/
```

---

## 5️⃣ بناء التطبيق

### خيار A: تشغيل مباشر
```bash
./gradlew :app:installDebug
```

### خيار B: APK للتوزيع
```bash
./gradlew :app:assembleRelease
```
الملف يظهر في:
```
app/build/outputs/apk/release/app-release.apk
```

---

## 6️⃣ هيكل المعمارية (Clean Architecture)

```
┌─────────────────────────────────────────┐
│         Presentation Layer (UI)         │
│  - Jetpack Compose Screens              │
│  - ViewModels (StateFlow)               │
│  - Navigation Component                 │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│          Domain Layer (Business)        │
│  - Models (Pure Kotlin)                 │
│  - Repository Interfaces                │
│  - Use Cases (Single Responsibility)      │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│           Data Layer (Sources)          │
│  - Remote: Retrofit + DTOs              │
│  - Local: Room Database                 │
│  - Paging: PagingSources                │
│  - Mapper: DTO → Domain                 │
└─────────────────────────────────────────┘
```

**تدفق البيانات:**
```
UI → ViewModel → UseCase → Repository → API/Room → Server/DB
```

---

## 7️⃣ شرح المميزات

### 🎬 تشغيل الفيديوهات
- يستخدم **ExoPlayer** (أفضل مشغل Android)
- دعم الوضع الكامل (Fullscreen)
- يعرض المدة والجودة تلقائياً

### 🔍 البحث
- بحث فوري (Instant Search)
- Paging 3 للتحميل التدريجي
- لا يوجد حدود على عدد النتائج

### 💬 التعليقات
- عرض التعليقات في BottomSheet
- إمكانية إضافة تعليق (يتطلب OAuth)
- عرض الردود والإعجابات

### 📂 قوائم التشغيل
- عرض قوائم القناة
- إنشاء قوائم جديدة
- إضافة فيديوهات للقوائم

### ⬇️ التحميل
- WorkManager للتحميل في الخلفية
- إشعارات التقدم
- إمكانية الإلغاء وإعادة المحاولة

### 📊 إدارة القناة
- إحصائيات القناة (مشتركين/فيديوهات/مشاهدات)
- رفع فيديوهات جديدة (يتطلب OAuth)
- تعديل بيانات القناة

---

## 8️⃣ اختبار كل ميزة

### اختبار البحث:
1. افتح التطبيق
2. اضغط أيقونة البحث في Bottom Navigation
3. اكتب "Android tutorial"
4. تأكد من ظهور النتائج مع الصور المصغرة

### اختبار التشغيل:
1. اضغط على أي فيديو
2. تأكد من تشغيل ExoPlayer
3. اضغط أيقونة التكبير للوضع الكامل
4. جرب أزرار الإعجاب/المشاركة/التحميل

### اختبار التعليقات:
1. في شاشة المشغل اضغط "تعليق"
2. اكتب تعليقاً وارسل
3. يجب أن يظهر في القائمة (يتطلب تسجيل دخول)

### اختبار التحميل:
1. في أي فيديو اضغط أيقونة التحميل
2. اذهب إلى قسم "التحميلات"
3. تأكد من ظهور شريط التقدم

---

## 9️⃣ حل المشاكل الشائعة

### مشكلة: `Unresolved reference: BuildConfig`
**الحل:**
```bash
./gradlew clean build
```
أو اذهب إلى **Build** → **Rebuild Project**

### مشكلة: `Cannot resolve symbol 'YTChannelApp'`
**الحل:** تأكد من أن `@HiltAndroidApp` موجود في `YTChannelApp.kt` وأنك بنيت المشروع مرة واحدة على الأقل.

### مشكلة: `API key not valid`
**الحل:**
- تأكد من تفعيل YouTube Data API v3
- تأكد من عدم وجود قيود IP على المفتاح
- تأكد من صحة المفتاح في `build.gradle.kts`

### مشكلة: `Quota exceeded`
**الحل:**
- YouTube API يعطيك 10,000 وحدة يومياً
- كل عملية تستهلك وحدات مختلفة
- انتظر 24 ساعة أو اطلب زيادة الحصة من Google Cloud

### مشكلة: `ExoPlayer not playing`
**الحل:**
- ExoPlayer لا يدعم تشغيل روابط YouTube مباشرة
- للتطبيقات الحقيقية، استخدم **YouTube Android Player API** أو **WebView** مع IFrame
- الكود الحالي يستخدم MediaItem كمثال، استبدله بمكتبة مناسبة

---

## 🔟 تحسينات مقترحة للمستقبل

| الميزة | التقنية | الأولوية |
|--------|---------|----------|
| تسجيل الدخول OAuth | Google Sign-In SDK | 🔴 عالية |
| مشغل YouTube حقيقي | YouTube Android Player API | 🔴 عالية |
| تحميل فعلي | youtube-dl (قانوني) أو yt-dlp | 🟡 متوسطة |
| تصفية الفيديوهات | Chips + Filter Menu | 🟢 منخفضة |
| الوضع الليلي التلقائي | Material You Dynamic Colors | 🟢 منخفضة |
| Shorts/Reels | ViewPager2 | 🟡 متوسطة |
| Live Streaming | HLS ExoPlayer | 🟡 متوسطة |
| اختصارات Widget | Glance API | 🟢 منخفضة |

---

## 📞 دعم

لأي استفسارات أو مشاكل:
1. راجع **logcat** في Android Studio
2. تأكد من صحة المفتاح في Google Cloud Console
3. تحقق من اتصال الإنترنت
4. جرب `./gradlew clean` ثم إعادة البناء

---

**بالتوفيق! 🚀**
