<a href="https://github.com/zechub/zechub/edit/main/site/guides/Zcash_Node_RPC_and_Indexing.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>

# دليل عملي: RPC لعقدة (node) Zcash وفهرسة سلسلة الكتل (blockchain)

هذه الصفحة دليل وصفات للتواصل مع عقدة كاملة (full node) Zcash قيد التشغيل عبر JSON-RPC، ولربط مُفهرِس (indexer) يمكن للمحافظ (wallets) الخفيفة ومستكشفات الكتل الاستعلام منه. إنها **ليست** دليل تثبيت ثانياً.

لمعرفة كيفية تشغيل حزمة ما بعد `zcashd` (Zebra + Zallet، وـ Zaino الاختياري، والمنافذ، ووحدات التخزين، والإقلاع على مرحلتين)، استخدم صفحة [حزمة Z3](/zcash-tech/z3-stack) و[مستودع Z3](https://github.com/ZcashFoundation/z3). للاطلاع على خلفية عقد الإجماع (consensus)، راجع [العقد الكاملة](/zcash-tech/full-nodes) و[العقدة الكاملة Zebra](/zcash-tech/zebra-full-node).

وصل `zcashd` إلى نهاية عمره في 18 يوليو 2026. ينبغي أن يستهدف عمل المشغّلين الجديد Zebra (أو [Zakura](/zcash-tech/zakura-node)) إلى جانب محفظة/مُفهرِس، لا `zcashd`.

---

## الخلاصة (TL;DR)

* **JSON-RPC** هو الطريقة التي يسأل بها المشغّلون والمحافظ العقدةَ عن رأس السلسلة (chain) والأقران، و(في عملية المحفظة) عن الأرصدة. في Z3، يخدم Zebra واجهة RPC للعقدة؛ ويخدم Zallet واجهة RPC للمحفظة.
* **Regtest** هو المكان المناسب لتعلّم الاستدعاءات. يقبل موجّه rpc-router الخاص بـ Z3 على `http://127.0.0.1:8181` بيانات الاعتماد `zebra` / `zebra` ويحوّل أساليب العقدة إلى Zebra وأساليب المحفظة إلى Zallet.
* تستخدم **الشبكة الرئيسية (mainnet) وشبكة الاختبار (testnet)** مصادقة cookie على Zebra. لا تنسخ اسم المستخدم/كلمة المرور الخاصين بـ regtest إلى عقدة عامة.
* **الفهرسة** مهمة منفصلة عن التحقق. يتحقق Zebra. ويخدم [Zaino](/zcash-tech/zaino) (`--profile indexer` في Z3) واجهة gRPC متوافقة مع lightwalletd. أما [Ztreamer](https://github.com/zcashlabs/ztreamer) فهو مُفهرِس مدعوم بـ Zakura؛ وليس ما تشحنه Z3.
* إن واجهة JSON-RPC الخاصة بـ Zaino وواجهة gRPC للمُفهرِس الاختياري في Zebra مخصصتان لواجهة loopback أو لشبكة موثوقة. لا تعرضهما دون مصادقة على الإنترنت العام.

---

## أي عملية تجيب عن أي استدعاء

| ما تريده | تواصل مع | منفذ المضيف على الشبكة الرئيسية في Z3 | تفاصيل |
| --- | --- | --- | --- |
| رأس السلسلة، الأقران، `getblockchaininfo` | JSON-RPC لـ Zebra | 8232 | مصادقة cookie |
| رصيد المحفظة، العناوين (addresses)، الإرسال | RPC لـ Zallet | 28232 | عملية منفصلة؛ برنامج تجريبي (beta) |
| الكتل (blocks) المضغوطة للمحافظ الخفيفة | gRPC لـ Zaino | 8137 | فقط مع `--profile indexer`؛ h2c نصي غير مشفّر |
| وكيل JSON-RPC للمستكشف / faucet | JSON-RPC لـ Zaino | 8237 | للشبكة الموثوقة فقط |
| التدرّب على كل ما سبق دون مزامنة | rpc-router لـ regtest في Z3 | 8181 | اسم المستخدم `zebra` / كلمة المرور `zebra` |

مصفوفة المنافذ وأسماء مشاريع Compose: [حزمة Z3](/zcash-tech/z3-stack) و[`z3-contract.yaml`](https://github.com/ZcashFoundation/z3/blob/main/z3-contract.yaml).

---

## تدرّب على regtest أولاً

من نسخة Z3 مسحوبة (checkout)، بعد `./scripts/regtest-init.sh` وـ `docker compose --env-file .env.regtest up -d`:

```bash
# Node method (Zebra)
curl -s -X POST -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"getblockchaininfo","params":[],"id":1}' \
  http://127.0.0.1:8181

# Wallet method (Zallet)
curl -s -X POST -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"getwalletinfo","params":[],"id":2}' \
  http://127.0.0.1:8181
```

تتضمن نتيجة `getblockchaininfo` السليمة `"blocks"` وخريطة `"upgrades"`. إذا تعلّق curl، فهذا يعني أن الموجّه (router) لا يعمل؛ وإذا حصلت على `401`، فأنت تتصل بـ Zebra مباشرة بدلاً من الموجّه (الوصول المباشر إلى Zebra على regtest يكون عبر `29232` وما زال يتوقع بيانات اعتماد الموجّه ما لم تغيّرها).

سير عمل regtest الكامل، بما في ذلك تعدين (mining) كتل التفعيل: [Z3 `docs/regtest.md`](https://github.com/ZcashFoundation/z3/blob/main/docs/regtest.md).

---

## JSON-RPC على الشبكة الرئيسية / شبكة الاختبار (مصادقة cookie)

على الشبكة الرئيسية وشبكة الاختبار، يقرأ Zallet وخدمة Zaino الاختيارية **وحدة تخزين cookie مشتركة**. يُعاد توليد ملف cookie هذا عند الإقلاع؛ فهو ليس هدفاً للنسخ الاحتياطي.

**لا** تنشر واجهة JSON-RPC الخاصة بـ Zebra على الإنترنت العام. استدعِها من شبكة Compose (`zebra:8232`) أو من المضيف فقط إذا ربطتها بواجهة loopback.

فحوصات المشغّل المعتادة بعد أن يصبح Zebra `/ready`:

```bash
# Wait until Zebra is synced (Z3 helper)
./scripts/check-zebra-readiness.sh

# Then start Zallet
docker compose --env-file .env.mainnet up -d
```

إن `getblocktemplate` / `getmininginfo` هي استدعاءات RPC للتعدين. تحتاج المجمّعات (pools) التي كانت لا تزال تتوقع عملية `zcashd` واحدة إلى Zebra (أو Zakura) للقوالب، إضافةً إلى أي محفظة تستخدمها لمعاملة coinbase. راجع [مجمّعات التعدين لـ Zcash](/using-zcash/zcash-mining-pools) و[دليل الترحيل](/guides/migration-guide-zcashd-to-zebrad-zallet).

---

## ربط مُفهرِس

التحقق والفهرسة مهمتان مختلفتان.

### Zaino (ما تشحنه Z3)

يقرأ Zaino من Zebra ويخدم واجهة gRPC API متوافقة مع lightwalletd إضافةً إلى وكيل JSON-RPC. **لا** يحتاج Zallet إلى حاوية Zaino المستقلة؛ فهو يضمّن مكتبات Zaino ويتواصل مع Zebra عبر JSON-RPC. شغّل Zaino المستقل عندما تريد محافظ خفيفة خارجية أو مستكشفات أو faucet:

```bash
docker compose --env-file .env.mainnet --profile indexer up -d
```

انتظر حتى تتم مزامنة Zebra قبل توجيه العملاء إلى Zaino. يمكن لمُفهرِس جماعي في مرحلة اللحاق يعمل بتزامن جلب عالٍ أن يبطئ أداة التحقق في Zebra؛ أبقِ `fetch_concurrency` عند أرقام آحاد منخفضة حتى يلحق المُفهرِس بالركب.

gRPC على regtest (نص غير مشفّر؛ المنفذ `28137`):

```bash
# From the Z3 repo, after: scripts/vendor.sh zaino
# and: docker compose --env-file .env.regtest --profile indexer up -d zaino

grpcurl -plaintext \
  -import-path vendor/zaino/zaino-proto/proto \
  -proto service.proto \
  127.0.0.1:28137 \
  cash.z.wallet.sdk.rpc.CompactTxStreamer/GetLightdInfo

grpcurl -plaintext \
  -import-path vendor/zaino/zaino-proto/proto \
  -proto service.proto \
  -d '{}' \
  127.0.0.1:28137 \
  cash.z.wallet.sdk.rpc.CompactTxStreamer/GetLatestBlock
```

إن واجهة gRPC الخاصة بـ Zaino هي h2c نصي غير مشفّر على جميع الشبكات. أنهِ TLS عند وكيل عكسي إذا كان أي شيء من خارج المضيف سيتصل بها. التفاصيل: [Zaino](/zcash-tech/zaino) و[Z3 `docs/regtest.md`](https://github.com/ZcashFoundation/z3/blob/main/docs/regtest.md).

### مُفهرِسات أخرى (ليست Z3)

* **lightwalletd** — خادم الكتل المضغوطة الأقدم. لا يزال منشوراً على أرض الواقع؛ ويمثّل Zaino المسار الخلف لهذا الدور.
* **Ztreamer** — مُفهرِس مدعوم بـ Zakura وتطبيق لـ `CompactTxStreamer`. وهو ليس خدمة من خدمات Z3. راجع [مستودع Ztreamer](https://github.com/zcashlabs/ztreamer).
* **Zebra `rpc.indexer_listen_addr`** — مستمع gRPC اختياري *داخل* Zebra. لا تتطلبه Z3 ولا تنشره إلى المضيف.

---

## أخطاء شائعة

**استدعاء أساليب المحفظة على Zebra.** تنتمي `getwalletinfo` واستدعاءات RPC للإرسال إلى Zallet (أو محفظة أخرى)، لا إلى عقدة الإجماع.

**تشغيل Zallet قبل أن يصبح Zebra `/ready`.** سيدخل Zallet في حلقة إعادة تشغيل. استخدم `scripts/check-zebra-readiness.sh` على الشبكة الرئيسية/شبكة الاختبار.

**نسخ `zebra`/`zebra` الخاصين بـ regtest إلى الشبكة الرئيسية.** إن مصادقة cookie هي الإعداد الافتراضي للشبكة الرئيسية/شبكة الاختبار لسبب وجيه.

**معاملة Zaino كعقدة كاملة.** يتحقق Zebra (أو Zakura). ويفهرس Zaino.

**عرض واجهة JSON-RPC الخاصة بـ Zaino أو واجهة gRPC لمُفهرِس Zebra للعامة.** هذه النقاط الطرفية مخصصة لواجهة loopback أو لشبكة خاصة موثوقة.

**افتراض أن كل محفظة خفيفة تتحدث Zaino بالفعل.** تحقّق من الإصدار الحالي للمحفظة مقابل البروتوكول الذي تنفّذه فعلياً.

---

## صفحات ذات صلة

* [حزمة Z3](/zcash-tech/z3-stack) — التثبيت، والمنافذ، ووحدات التخزين، والإقلاع على مرحلتين
* [Zaino](/zcash-tech/zaino) — دور المُفهرِس وإرشادات للمشغّلين
* [العقد الكاملة](/zcash-tech/full-nodes)
* [العقدة الكاملة Zebra](/zcash-tech/zebra-full-node)
* [عقد المحافظ الخفيفة](/zcash-tech/lightwallet-nodes)
* [دليل الترحيل: من zcashd إلى Zebrad/Zallet](/guides/migration-guide-zcashd-to-zebrad-zallet)
* [دليل Zingolib وـ Zaino التعليمي](/guides/zingolib-and-zaino-tutorial)

## الموارد

* [مستودع Z3](https://github.com/ZcashFoundation/z3)
* [دليل وصفات regtest في Z3 (curl / grpcurl)](https://github.com/ZcashFoundation/z3/blob/main/docs/regtest.md)
* [الأسئلة الشائعة حول Z3](https://github.com/ZcashFoundation/z3/blob/main/docs/faq.md)
* [Zebra](https://zebra.zfnd.org/)
* [Zaino](https://github.com/zingolabs/zaino)
* [كتاب Zallet](https://zcash.github.io/zallet/)
* [Ztreamer](https://github.com/zcashlabs/ztreamer)

**آخر تحديث:** سبتمبر 2026
