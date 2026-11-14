# كيفية تشغيل Zakura على Akash Network

مسار خطوة بخطوة لتشغيل عقدة كاملة (full node) لـ Zcash من نوع **Zakura** باستخدام قالب [awesome-akash zcash-zakura](https://github.com/akash-network/awesome-akash/tree/master/zcash-zakura).

Zakura هي عقدة (node) لـ Zcash متوافقة مع الإجماع (consensus) (مشتقة من Zebra)، وتتميز بمزامنة أسرع وتقليم أصلي (pruning) وتمهيد عبر اللقطات (snapshot). يثبّت هذا القالب الإصدار **`zakuracore/zakura:1.8.1`** (مثبّت عبر digest؛ لا تستخدم `:latest` أو `1.1.1`).

### ما الذي ستحصل عليه

- شبكة Mainnet P2P: منفذ الحاوية **8233**، منشور عالميًا (انظر تعيين المنافذ أدناه)
- حالة مقلّمة دائمة (64 GiB افتراضيًا) مع تمهيد من لقطة Valar عند التشغيل الأول
- Prometheus على **9999** وفحص الصحة على **8080** (داخلي، بدون URI عام)
- JSON-RPC على **8232** داخليًا لـ **lightwalletd** المرافق في الموقع نفسه؛ يستمع LWD على منفذ الحاوية **9067** وهو منشور عالميًا

أعلام الميزات في SDL (true/false): `ENABLE_METRICS`، `ENABLE_HEALTH`، `ENABLE_RPC`، `ENABLE_LIGHTWALLETD`. تكون مصادقة ملفات تعريف الارتباط (cookie) معطّلة عندما يكون lightwalletd مفعّلًا، لأن lightwalletd لا يستطيع قراءة ملفات cookie.

### أي موقع تستخدم للدفع (payments)

SDL هو نفسه. تختلف واجهتا الويب لـ Akash في **طريقة الدفع**.

| الموقع | المدفوعات | المحفظة (wallet) |
| --- | --- | --- |
| [console.akash.network](https://console.akash.network) | دفع تقليدي عبر الويب (البطاقة / قنوات العملات الورقية التي يوفرها Console) | حساب Console |
| [air.akash.network](https://air.akash.network) | مدفوعات أصلية على سلسلة (chain) Akash (AKT / ACT) | **Keplr** |

استخدم **Air** عندما تريد الدفع بـ Keplr على السلسلة (on-chain). استخدم **Console** عندما تريد مسار دفع تقليديًا عبر الويب. لا تتوقع تسوية أصلية عبر Keplr على Console.

### النشر

**مدفوعات الويب (Console)**

[![النشر على Akash Console](https://raw.githubusercontent.com/akash-network/console/refs/heads/main/apps/deploy-web/public/images/deploy-with-akash-btn.svg)](https://console.akash.network/new-deployment?step=edit-deployment&templateId=akash-network-awesome-akash-zcash-zakura)

**مدفوعات Akash الأصلية باستخدام Keplr (Air)**

[افتح هذا القالب على Air](https://air.akash.network/new-deployment?step=edit-deployment&templateId=akash-network-awesome-akash-zcash-zakura)

1. افتح الرابط المطابق أعلاه (Console أو Air).
2. على Air، اربط **Keplr** وتأكد من امتلاكك AKT/ACT للإيداع والاستئجار. على Console، أكمل عملية الدفع عبر الويب التي تعرضها الواجهة.
3. حمّل `zcash-zakura/deploy.yaml` إذا لم يكن معبأً مسبقًا.
4. أودِع، واقبل عرضًا (bid)، وانتظر عقد الاستئجار (lease).
5. يجب أن تُظهر السجلات تنزيل اللقطة (عند التشغيل الأول) ثم `zakurad start`.
6. افتح **Leases → URIs**. استخدم host:port الذي خصصه المزوّد، وليس 8233 أو 9067 على عنوان IP العام.

### كيف ينشر Akash المنافذ

منافذ SDL `port` / `as` (8233، 9067، 8232) هي منافذ **الحاوية**. لا يزال Zakura يستمع على 8233، وـ lightwalletd على 9067، وـ RPC على 8232 لحاوية LWD المرافقة (`zakura:8232`).

إنها **ليست** المنافذ الموجودة على عنوان IP العام للمزوّد.

**الافتراضي (هذا القالب):** لا يوجد IP مخصص. يُعيَّن كل منفذ TCP مكشوف عبر `global: true` إلى **منفذ مرتفع عشوائي** على مضيف مشترك لدى المزوّد. يعرضها Console/Air كعناوين URI لعقد الاستئجار، على سبيل المثال:

```
zakura-8233: provider.example:31482
lightwalletd-9067: provider.example:32011
```

تتصل النظائر (peers) بعنوان URI الخاص بالمنفذ **8233** (`provider.example:31482`). تستخدم المحافظ المدمجة عنوان URI الخاص بالمنفذ **9067** (TLS معطّل ما لم تُنهِ TLS بنفسك). اضبط `ZAKURA_NETWORK__EXTERNAL_ADDR` على عنوان URI المعيَّن لـ P2P، وليس `:8233`.

المنافذ المكشوفة **الداخلية** (RPC على `8232`، والمقاييس على `9999`، وفحص الصحة على `8080`) تبقى على شبكة النشر. لا يوجد URI عام.

**عناوين IP المخصصة** (`endpoints` / `kind: ip`) هي إضافة مدفوعة و**ليست** ضمن هذا القالب. يمكنها تثبيت IP عام وأحيانًا منفذ `as`. أضِف ذلك فقط إذا كنت بحاجة إلى `host:8233` ثابت.

### الأرشيفي مقابل المقلّم

الافتراضي هو **المقلّم** (لقطة بحجم ~11 GiB، وقرص بسعة 64 GiB). للحصول على السجل الكامل، اضبط `SNAPSHOT_KIND=archive`، وارفع سعة القرص إلى 350 GiB+، واضبط `ZAKURA_STATE__STORAGE_MODE=archive`.

### ذو صلة

- [Zebra على Akash](/guides/akash-network-zebra) — Zcash Foundation `zebrad` (السجل الكامل، بدون لقطة)
- [هكذا تكلّم Zakura على Akash](/guides/thus-spoke-zakura-akash) — لوحة المعلومات (dashboard) لـ Regtest + التطبيق (v0.4.1)
- الشيفرة المصدرية لـ Zakura: [github.com/zakura-core/zakura](https://github.com/zakura-core/zakura)
- اللقطات: [zakura.com/snapshots/](https://zakura.com/snapshots/)
