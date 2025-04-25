# دليل نشر تطبيق OfficeOnWay المحدث

## مقدمة

هذا الدليل يوفر تعليمات مفصلة لنشر تطبيق OfficeOnWay المحدث على منصة Vercel. تم تحديث التطبيق ليكون متوافقًا مع Next.js 13+ من خلال إضافة توجيه 'use client' إلى المكونات اللازمة وتحديث ملف التكوين next.config.js.

## المتطلبات المسبقة

1. حساب على [Vercel](https://vercel.com)
2. حساب على [Firebase](https://firebase.google.com) مع مشروع مكون
3. حساب على [Twilio](https://twilio.com) (لخدمة الرسائل القصيرة)

## خطوات النشر

### 1. إعداد مشروع Firebase

تأكد من أن مشروع Firebase الخاص بك مكون بشكل صحيح:

1. تفعيل خدمة المصادقة (Authentication) مع تمكين مزود المصادقة عبر رقم الهاتف
2. تفعيل خدمة Firestore Database
3. تفعيل خدمة Storage
4. الحصول على مفاتيح API وتكوين التطبيق

### 2. نشر التطبيق على Vercel

#### الطريقة 1: النشر من واجهة Vercel

1. **إنشاء مشروع جديد على Vercel**:
   - قم بتسجيل الدخول إلى [Vercel](https://vercel.com)
   - انقر على "New Project"
   - اختر "Import Git Repository" أو "Upload"
   - إذا اخترت "Upload"، قم بسحب وإفلات مجلد المشروع المفكوك (بعد فك ضغط ملف ZIP)

2. **تكوين متغيرات البيئة**:
   - في صفحة تكوين المشروع، انتقل إلى تبويب "Environment Variables"
   - أضف جميع متغيرات البيئة المطلوبة:
     ```
     NEXT_PUBLIC_FIREBASE_API_KEY=your_firebase_api_key
     NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_firebase_auth_domain
     NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_firebase_project_id
     NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_firebase_storage_bucket
     NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_firebase_messaging_sender_id
     NEXT_PUBLIC_FIREBASE_APP_ID=your_firebase_app_id
     TWILIO_ACCOUNT_SID=your_twilio_account_sid
     TWILIO_AUTH_TOKEN=your_twilio_auth_token
     TWILIO_PHONE_NUMBER=your_twilio_phone_number
     NEXT_PUBLIC_PROJECT_DOMAIN=officeonway.com
     NEXT_PUBLIC_PROJECT_VERCEL=https://officeonway.vercel.app
     NEXT_PUBLIC_DEFAULT_LANGUAGE=ar
     ```

3. **بدء النشر**:
   - انقر على "Deploy"
   - انتظر حتى اكتمال عملية النشر

#### الطريقة 2: النشر باستخدام Vercel CLI

1. **تثبيت Vercel CLI**:
   ```bash
   npm install -g vercel
   ```

2. **تسجيل الدخول إلى Vercel**:
   ```bash
   vercel login
   ```

3. **نشر المشروع**:
   ```bash
   cd path/to/officeonway_updated
   vercel --prod
   ```

4. **تكوين متغيرات البيئة**:
   - يمكنك تكوين متغيرات البيئة أثناء عملية النشر أو من خلال لوحة تحكم Vercel

### 3. ربط النطاق المخصص

1. **إضافة النطاق في Vercel**:
   - في لوحة تحكم Vercel، انتقل إلى مشروعك
   - انقر على تبويب "Settings" ثم "Domains"
   - أضف النطاق `officeonway.com` و `www.officeonway.com`

2. **تكوين سجلات DNS**:
   - قم بتكوين سجلات DNS لدى مزود النطاق الخاص بك وفقًا للتعليمات التي يوفرها Vercel
   - عادةً ما تحتاج إلى إضافة سجل CNAME لـ `www` يشير إلى `cname.vercel-dns.com`
   - وسجل A للنطاق الرئيسي يشير إلى عناوين IP التي يوفرها Vercel

### 4. التحقق من النشر

بعد اكتمال عملية النشر، تحقق من أن التطبيق يعمل بشكل صحيح:

1. افتح المتصفح وانتقل إلى `https://officeonway.com`
2. تأكد من أن الصفحة الرئيسية تظهر بشكل صحيح
3. جرب تسجيل الدخول للتأكد من أن نظام المصادقة يعمل
4. تنقل بين صفحات لوحة التحكم للتأكد من أن جميع المكونات تعمل بشكل صحيح

## استكشاف الأخطاء وإصلاحها

إذا واجهت أي مشاكل أثناء النشر، يمكنك التحقق من الأمور التالية:

### مشاكل البناء

1. **تحقق من سجلات البناء في Vercel**:
   - انتقل إلى تبويب "Deployments" في لوحة تحكم Vercel
   - انقر على آخر نشر لعرض سجلات البناء
   - ابحث عن أي أخطاء أو تحذيرات

2. **مشاكل ESLint**:
   - إذا فشل البناء بسبب أخطاء ESLint، يمكنك تعديل ملف `.eslintrc.json` لتخفيف القواعد:
     ```json
     {
       "extends": "next/core-web-vitals",
       "rules": {
         "@typescript-eslint/no-explicit-any": "warn",
         "@typescript-eslint/no-unused-vars": "warn",
         "react/no-unescaped-entities": "warn"
       }
     }
     ```

### مشاكل وقت التشغيل

1. **تحقق من متغيرات البيئة**:
   - تأكد من أن جميع متغيرات البيئة المطلوبة تم تكوينها بشكل صحيح في Vercel

2. **مشاكل المصادقة**:
   - تأكد من أن خدمة المصادقة عبر رقم الهاتف مفعلة في Firebase
   - تحقق من صحة مفاتيح API الخاصة بـ Twilio

3. **مشاكل CORS**:
   - إذا واجهت مشاكل CORS، تأكد من تكوين قواعد CORS في Firebase بشكل صحيح

## تحديث التطبيق

لتحديث التطبيق بعد إجراء تغييرات:

1. قم بإجراء التغييرات المطلوبة على الكود
2. اختبر التغييرات محليًا باستخدام `npm run dev`
3. قم ببناء التطبيق للتأكد من عدم وجود أخطاء: `npm run build`
4. أعد نشر التطبيق على Vercel باستخدام نفس الخطوات المذكورة أعلاه

## الخلاصة

تم تحديث تطبيق OfficeOnWay ليكون متوافقًا مع Next.js 13+ من خلال إضافة توجيه 'use client' إلى المكونات اللازمة وتحديث ملف التكوين next.config.js. باتباع هذا الدليل، يمكنك نشر التطبيق المحدث على منصة Vercel والتأكد من أنه يعمل بشكل صحيح.

إذا واجهت أي مشاكل أو كانت لديك أسئلة، يرجى الرجوع إلى الوثائق المرفقة:
- `next13_compatibility_guide.md`: دليل توافق Next.js 13+
- `test-report-next13.md`: تقرير اختبار التوافق
