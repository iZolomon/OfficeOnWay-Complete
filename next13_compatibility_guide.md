# دليل توافق Next.js 13+ لمشروع OfficeOnWay

## مقدمة

هذا الدليل يشرح التغييرات التي تم إجراؤها لضمان توافق مشروع OfficeOnWay مع Next.js 13+ وحل المشكلات التي كانت تمنع تشغيل الصفحات بشكل صحيح. يتضمن الدليل شرحًا للتغييرات، وكيفية التحقق من صحتها، وإرشادات للتطوير المستقبلي.

## التغييرات الرئيسية

### 1. إضافة توجيه 'use client' للمكونات

في Next.js 13+، تم تقديم نموذج جديد للتمييز بين مكونات العميل ومكونات الخادم. يجب تحديد المكونات التي تستخدم ميزات React مثل hooks (useState، useEffect، إلخ) كمكونات عميل باستخدام توجيه 'use client' في بداية الملف.

**المكونات التي تم تحديثها:**
- `src/components/auth/AuthGuard.tsx`: تمت إضافة توجيه 'use client' في بداية الملف

### 2. تحديث ملف التكوين next.config.js

تم تحديث ملف `next.config.js` لإزالة الخيارات المهجورة أو غير المدعومة في Next.js 13+:

**التغييرات:**
- إزالة تكوين `i18n` غير المدعوم في App Router
- إزالة خيار `swcMinify` غير المعترف به

**الملف قبل التحديث:**
```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  swcMinify: true,
  i18n: {
    locales: ['ar', 'en'],
    defaultLocale: 'ar',
    localeDetection: true,
  },
  images: {
    domains: ['firebasestorage.googleapis.com'],
  },
  // ... باقي التكوين
};
```

**الملف بعد التحديث:**
```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  images: {
    domains: ['firebasestorage.googleapis.com'],
  },
  // ... باقي التكوين
};
```

## كيفية التحقق من التغييرات

للتحقق من أن التغييرات تعمل بشكل صحيح:

1. **تشغيل خادم التطوير:**
   ```bash
   npm run dev
   ```
   يجب أن يبدأ الخادم بدون تحذيرات تكوين متعلقة بـ i18n أو swcMinify.

2. **التنقل في التطبيق:**
   - تحقق من أن صفحة تسجيل الدخول تعمل بشكل صحيح
   - تحقق من أن مكون AuthGuard يعمل بشكل صحيح (يجب أن يتم توجيه المستخدمين غير المصادق عليهم إلى صفحة تسجيل الدخول)
   - تحقق من أن جميع صفحات لوحة التحكم تعمل بشكل صحيح بعد تسجيل الدخول

3. **بناء التطبيق للإنتاج:**
   ```bash
   npm run build
   ```
   يجب أن يكتمل البناء بنجاح (مع بعض تحذيرات ESLint المتعلقة بجودة الكود).

## إرشادات للتطوير المستقبلي

### 1. توجيه 'use client'

عند إضافة مكونات جديدة تستخدم ميزات React مثل hooks، تأكد من إضافة توجيه 'use client' في بداية الملف:

```tsx
"use client";

import { useState, useEffect } from 'react';

export default function MyComponent() {
  const [state, setState] = useState(initialState);
  // ... باقي الكود
}
```

### 2. تعدد اللغات (i18n) في App Router

لتنفيذ تعدد اللغات في Next.js 13+ باستخدام App Router، اتبع النهج الموصى به في الوثائق الرسمية:

1. **إنشاء مجلدات للغات المختلفة:**
   ```
   app/
     [lang]/
       page.tsx
       layout.tsx
       ...
   ```

2. **استخدام معلمات المسار للغة:**
   ```tsx
   // app/[lang]/layout.tsx
   export default function Layout({
     children,
     params: { lang }
   }: {
     children: React.ReactNode;
     params: { lang: string };
   }) {
     // ... استخدام معلمة اللغة
     return (
       <html lang={lang}>
         <body>{children}</body>
       </html>
     );
   }
   ```

### 3. معالجة تحذيرات ESLint

لتحسين جودة الكود، يجب معالجة تحذيرات ESLint التالية:

1. **المتغيرات غير المستخدمة:**
   - إزالة المتغيرات غير المستخدمة أو استخدامها بشكل صحيح
   - استخدام الشرطة السفلية (_) للمتغيرات التي يجب تجاهلها

2. **أنواع `any` الصريحة:**
   - استبدال `any` بأنواع أكثر تحديدًا
   - استخدام `unknown` إذا كان النوع غير معروف بدلاً من `any`

3. **علامات اقتباس غير مهربة:**
   - استخدام `&quot;` أو `&ldquo;` بدلاً من `"` في JSX

## الخلاصة

بعد تطبيق هذه التغييرات، يجب أن يكون مشروع OfficeOnWay متوافقًا تمامًا مع Next.js 13+ ويعمل بشكل صحيح في بيئة الإنتاج. إذا واجهت أي مشكلات أخرى، فتأكد من مراجعة [وثائق Next.js الرسمية](https://nextjs.org/docs) للحصول على أحدث الممارسات الموصى بها.
