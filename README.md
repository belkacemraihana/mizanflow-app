# MizanFlow

منصة SaaS محاسبية جزائرية، Arabic-first، مبنية باستخدام Next.js وTypeScript وPostgreSQL وPrisma.

## ما يتضمنه Phase 1

- موقع تسويقي ثلاثي اللغات (`/ar`, `/fr`, `/en`)
- مساحة Demo معزولة للقراءة فقط (`/ar/demo`)
- التسجيل وتسجيل الدخول بجلسة HttpOnly
- Onboarding لإنشاء Organization وCompany وعضوية Owner
- RBAC مركزي للأدوار الخمسة
- Dashboard وSidebar وDark mode وتصميم responsive
- Prisma schema أولي وDocker Compose لـPostgreSQL

## التشغيل

```bash
cp .env.example .env
docker compose up -d
npm install
npm run db:generate
npm run db:migrate -- --name phase_1_foundations
npm run db:seed
npm run dev
```

افتح `http://localhost:3000/ar`. حساب البيانات التجريبية المولّد بالـseed: `demo@mizanflow.dz` / `Demo@12345`. غيّر هذه البيانات خارج بيئة التطوير.

## الأمان

- كلمات المرور باستخدام bcrypt cost 12.
- جلسة JWT موقعة مع سجل جلسة hashed في قاعدة البيانات.
- Cookies: HttpOnly, SameSite=Lax, Secure في الإنتاج.
- كل Organization لها Members وأدوار.
- Demo workspace منفصلة ومعلّمة بوضوح.

## حدود Phase 1

النواة المحاسبية، الفواتير، المصروفات، التقارير، OCR، AI، الاشتراكات والمتجر ستُبنى في المراحل المعتمدة التالية. عناصر Demo بصرية وغير قابلة للكتابة ولا تختلط ببيانات المستخدم.

## PART A — الهوية وإدارة الفريق

- إثبات البريد بروابط أحادية الاستخدام وصلاحية 24 ساعة.
- Forgot/Reset Password بروابط صالحة 30 دقيقة مع إبطال جميع الجلسات بعد التغيير.
- MFA متوافق TOTP مع نافذة زمنية محدودة و8 رموز استرداد أحادية الاستخدام.
- دعوات أعضاء صالحة 7 أيام، قبول مقيد بالبريد، وإلغاء الدعوات المعلقة.
- إدارة دور/حالة/حذف العضو محمية بـ RBAC وسجل تدقيق، مع حماية المالك.
- في التطوير تُطبع الرسائل في الطرفية وتُعاد روابط الاختبار في JSON. قبل الإنتاج يجب ربط `sendMail` بمزوّد بريد فعلي وضبط `EMAIL_FROM`.
