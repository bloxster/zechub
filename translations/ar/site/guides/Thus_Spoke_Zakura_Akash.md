# Thus Spoke Zakura على Akash (Regtest)

شغّل حزمة **Thus Spoke Zakura v0.4.1** على Akash: Zakura Regtest وـ lightwalletd وـ **لوحة المعلومات (dashboard) / واجهة HTTP API** الخاصة بالمحفظة (wallet). العملات التجريبية ليس لها أي قيمة. لا ترسل ZEC من الشبكة الرئيسية (mainnet) إلى هنا.

القالب: [awesome-akash/thus-spoke-zakura](https://github.com/akash-network/awesome-akash/tree/master/thus-spoke-zakura)

المصدر الأصلي (Upstream): [zcashlabs/thus-spoke-zakura](https://github.com/zcashlabs/thus-spoke-zakura) الوسم **v0.4.1**

### الصور

| الخدمة | الصورة |
| --- | --- |
| لوحة المعلومات + API (`app`) | `ghcr.io/zcashlabs/thus-spoke-zakura-app:0.4.1` |
| lightwalletd | `ghcr.io/zcashlabs/thus-spoke-zakura-lightwalletd:0.4.1` |
| Zakura | `zakuracore/zakura:1.6.0` (إصدار مثبّت مُدمج في المُشغِّل 0.4.1) |

### التهيئة الذاتية

عند التشغيل الأول، تُشغّل حاوية `app` الأمر `ths-server init` على وحدة التخزين الدائمة الخاصة بها، ثم `ths-server serve`. يعيد Serve المحاولة حتى يعمل كلٌّ من JSON-RPC الخاص بـ Zakura وـ lightwalletd. لا تحتاج إلى تشغيل `ths` على المزوّد؛ فملف SDL هو بيئة التشغيل.

### كيف ينشر Akash المنافذ

منافذ `port` / `as` في SDL هي منافذ **الحاوية**. يستمع التطبيق على المنفذ **8080** (`THS_LISTEN`)، ويستمع lightwalletd على **9067**، ويستمع Zakura على **18232** لـ RPC وعلى **18233** لـ P2P. داخل النشر، تستخدم الخدمات هذه المنافذ بالفعل (`http://zakura:18232`، `http://lightwalletd:9067`).

وهي **ليست** المنافذ الموجودة على عنوان IP العام للمزوّد.

**الإعداد الافتراضي (هذا القالب):** لا يوجد عنوان IP مخصص.

| الخدمة | منفذ الاستماع في الحاوية | `as` في SDL | كيف تظهر في الإيجار |
| --- | --- | --- | --- |
| `app` (لوحة المعلومات + HTTP API) | 8080 | 80 (HTTP) | اسم مضيف Ingress، غالبًا على 80/443 — **افتح عنوان URI هذا في المتصفح** |
| `lightwalletd` | 9067 | 9067 (TCP، عام) | `host:&lt;random-high-port&gt;` |
| `zakura` RPC | 18232 | 18232 (TCP، عام) | `host:&lt;random-high-port&gt;` |
| `zakura` P2P | 18233 | 18233 (TCP، عام) | `host:&lt;random-high-port&gt;` |

بعد أن يصبح الإيجار جاهزًا، يكون قسم **Leases → URIs** هو المرجع الموثوق. افتح عنوان URI الخاص بـ **`app`** للوصول إلى لوحة المعلومات (وهي نفس الواجهة التي يفتحها `ths` محليًا). وجّه العملاء المُدمجين (compact clients) إلى عنوان URI الخاص بـ **lightwalletd** (`THS_LIGHTWALLETD_TLS=false`). لا تكتب `:8080` أو `:80` أو `:9067` مع عنوان IP الخاص بالمزوّد إلا إذا جعل إيجارٌ بعنوان IP مخصص هذه المنافذ عامة.

**عناوين IP المخصصة** (`endpoints` / `kind: ip`) ميزة إضافية مدفوعة وهي **غير** مضمّنة في هذا القالب. يمكنها تثبيت عنوان IP عام، وأحيانًا منفذ `as` أيضًا. اتركها معطّلة ما لم تكن بحاجة إلى ذلك.

### أي موقع تستخدم: المدفوعات (payments)

ملف SDL هو نفسه. تختلف واجهتا الويب الخاصتان بـ Akash في **طريقة الدفع**.

| الموقع | المدفوعات | المحفظة |
| --- | --- | --- |
| [console.akash.network](https://console.akash.network) | دفع تقليدي عبر الويب (البطاقة / قنوات العملات الورقية التي يوفرها Console) | حساب Console |
| [air.akash.network](https://air.akash.network) | مدفوعات أصلية على سلسلة (chain) Akash (AKT / ACT) | **Keplr** |

استخدم **Air** عندما تريد الدفع بـ Keplr على السلسلة (on-chain). استخدم **Console** عندما تريد مسار دفع تقليديًا عبر الويب.

### النشر

**مدفوعات الويب (Console)**

[![النشر على Akash Console](https://raw.githubusercontent.com/akash-network/console/refs/heads/main/apps/deploy-web/public/images/deploy-with-akash-btn.svg)](https://console.akash.network/new-deployment?step=edit-deployment&templateId=akash-network-awesome-akash-thus-spoke-zakura)

**مدفوعات Akash الأصلية باستخدام Keplr (Air)**

[افتح هذا القالب على Air](https://air.akash.network/new-deployment?step=edit-deployment&templateId=akash-network-awesome-akash-thus-spoke-zakura)

أنشئ النشر من القالب، واقبل عرضًا، وانتظر حتى تُظهر سجلات `app` أن `ths-server serve` يبقى قيد التشغيل، ثم افتح **عنوان URI الخاص بـ `app`** للوصول إلى لوحة المعلومات.

### ذو صلة

- [Zakura على Akash](/guides/akash-network-zakura) — عقدة (node) تحقق للشبكة الرئيسية/الشبكة التجريبية
- [Zebra على Akash](/guides/akash-network-zebra)
- الكتاب: [github.com/zcashlabs/thus-spoke-zakura/tree/main/book](https://github.com/zcashlabs/thus-spoke-zakura/tree/main/book)
