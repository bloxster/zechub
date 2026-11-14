<a href="https://github.com/zechub/zechub/edit/main/site/Zcash_Tech/Z3_Stack.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>

# حزمة Z3 (Z3 Stack)

**حزمة Z3** هي منصة العقد المُجمَّعة التابعة لـ Zcash Foundation، وتتكوّن من **Zebra** (عقدة كاملة (full node)) + **Zallet** (محفظة (wallet) عقدة كاملة)، مع مُفهرس **Zaino** اختياري. صُمّمت لتحلّ محل عملية `zcashd` المستقلة، التي كانت تجمع الإجماع (consensus) والمحفظة في ملف تنفيذي واحد، وانتهى عمرها في 18 يوليو 2026.

التطبيق المرجعي هو مشروع Docker Compose على [github.com/ZcashFoundation/z3](https://github.com/ZcashFoundation/z3).

---

## باختصار (TL;DR)

* Z3 **ليس عميل إجماع جديدًا**. إنه طريقة تشغيل الحزمة التي حلّت محل `zcashd` كوحدة واحدة: يتحقق Zebra من صحة السلسلة (chain)، ويحتفظ Zallet بالمفاتيح ويقدّم RPC المحفظة، ويتحدث Zaino (اختياري) بروتوكول lightwalletd gRPC.
* كان `zcashd` يجمع العقدة (node) والمحفظة معًا. أما Z3 **فيفصل هذين الدورين**. تنتقل منصات التداول (exchanges) ومجمّعات التعدين (mining pools) وغيرها من مشغّلي محافظ العقد الكاملة إلى هذا المزيج، بدلًا من الانتقال إلى Zebra وحده.
* يمكن تشغيل ثلاثة مشاريع Compose معزولة على مضيف واحد: **mainnet** و**testnet** و**regtest**.
* تستغرق المزامنة الأولى للشبكة الرئيسية ما يقارب **24–72 ساعة** وتشغل نحو **300 GB**. أما regtest فيعمل خلال ثوانٍ، وهو المكان المناسب لتتعلّم الحزمة.
* يدمج Zallet مكتبات المفهرس الخاصة بـ Zaino ويتواصل مع Zebra عبر JSON-RPC. لا تحتاج إلى خدمة Zaino المستقلة إلا إذا أردت نقطة نهاية متوافقة مع lightwalletd للمحافظ الخارجية.
* لا يزال Zallet في مرحلة **بيتا**. قد تتطلب التغييرات الجذرية حذف المحفظة وإعادة إنشائها. لا تعامله كبرنامج حفظ وصائي (custodial) مكتمل للمبالغ الكبيرة.

---

## لماذا وُجدت Z3

طوال معظم عمر Zcash، كان `zcashd` هو العقدة الكاملة المرجعية، وفي الوقت نفسه المحفظة الوحيدة الجاهزة للإنتاج من نوع العقدة الكاملة. وعلى هذا التصميم بنت منصات التداول والمجمّعات والجهات الوصائية أنظمتها.

تقاعد `zcashd`. انتقل الإجماع إلى [Zebra](/zcash-tech/zebra-full-node) (وأصبح متاحًا الآن أيضًا في [Zakura](/zcash-tech/zakura-node)). وانتقلت المحفظة المدمجة إلى [Zallet](https://github.com/zcash/zallet). وتنتقل خدمة المحافظ الخفيفة حاليًا من [lightwalletd](/zcash-tech/lightwallet-nodes) إلى [Zaino](/zcash-tech/zaino).

لكل قطعة من هذه القطع الثلاث مستودعها ودورة إصداراتها وإعداداتها الخاصة. ويربط Z3 بينها: صور مثبّتة الإصدار، وفحوصات صحة تُبقي المحفظة متوقفة حتى تكتمل مزامنة العقدة، ومنافذ ووحدات تخزين خاصة بكل شبكة، ومسار تشغيل موثّق للمشغّلين.

الاسم اختصار غير رسمي متداول في المنظومة، أي Zebra و Zaino و Zallet، مع أن ملف Compose الافتراضي لا يشغّل إلا Zebra و Zallet. فـ Zaino ملف تعريف (profile) في Compose، وليس عملية ثالثة إلزامية.

---

## البنية

```
                    ┌──────────────────────── Z3 (per network) ────────────────────────┐
                    │                                                                  │
  peers ◄──P2P──►  Zebra (zebrad)  ──JSON-RPC──►  Zallet                                │
                    │   full node                    │  embeds Zaino libraries          │
                    │                                │  wallet RPC for operators        │
                    │                                └─────────────────────────────────┤
                    │                                                                  │
                    │   Zaino (optional, --profile indexer)                            │
                    │     lightwalletd-compatible gRPC + JSON-RPC proxy                │
                    │            │                                                     │
                    └────────────┼─────────────────────────────────────────────────────┘
                                 ▼
                        light wallets / explorers
```

| المكوّن | الدور في Z3 | إلزامي؟ |
| --- | --- | --- |
| **Zebra** | يزامن السلسلة ويتحقق من صحتها، وينشر البيانات بين الأقران (gossip)، ويوفّر JSON-RPC ونقطة نهاية للصحة | نعم |
| **Zallet** | محفظة عقدة كاملة. تدمج مكتبات Zaino. تتصل مباشرة بـ JSON-RPC الخاص بـ Zebra. **لا** تستدعي حاوية Zaino المستقلة | نعم |
| **Zaino** | مفهرس مستقل. يوفّر gRPC متوافقًا مع lightwalletd للعملاء الخفيفين (light clients) الخارجيين، إضافةً إلى وكيل JSON-RPC لمستكشفات الكتل وخدمات faucet | لا — `--profile indexer` |

يثبّت Z3 إصدارات الصور في `docker-compose.yml`. يمكنك تغييرها عبر `Z3_ZEBRA_IMAGE` أو `Z3_ZAINO_IMAGE` أو `Z3_ZALLET_IMAGE` إذا احتجت إلى وسم مختلف.

---

## كيف يختلف هذا عن zcashd

| | zcashd | Z3 |
| --- | --- | --- |
| اللغة | C++ (تفرّع من Bitcoin) | خدمات Rust تُنسَّق باستخدام Docker Compose |
| نموذج العمليات | ملف تنفيذي واحد: عقدة + محفظة | حاويتان منفصلتان للعقدة والمحفظة |
| الإجماع | متقاعد (انتهى الدعم في 18 يوليو 2026) | Zebra (أو عقدة متوافقة أخرى) |
| المحفظة | `wallet.dat` مدمجة | Zallet، مع مجلد بيانات مشفّر بأداة age |
| العملاء الخفيفون | عادةً lightwalletd منفصل | ملف تعريف Zaino اختياري |
| الإعدادات | `zcash.conf` | ملفات لكل شبكة ضمن `config/<network>/` إضافةً إلى ملفات env الخاصة بـ Compose |
| عدة شبكات على مضيف واحد | تعارضات مزعجة في المنافذ | مدعوم بالكامل: `z3-mainnet`، `z3-testnet`، `z3-regtest` |

إذا كانت لديك محفظة `zcashd`، فاستخدم [دليل الترحيل](/guides/migration-guide-zcashd-to-zebrad-zallet) الخاص بـ ZecHub والأمر `migrate-zcashd-wallet` الخاص بـ Zallet، ولا تنسخ `wallet.dat` إلى وحدة تخزين Z3.

---

## الشبكات

يتكوّن Z3 من ثلاثة مشاريع Compose مستقلة لا تتشارك المنافذ ولا وحدات التخزين.

| الشبكة | اسم المشروع | الاستخدام | المزامنة الأولى | أموال حقيقية |
| --- | --- | --- | --- | --- |
| **mainnet** | `z3-mainnet` | الإنتاج | 24–72 ساعة | نعم |
| **testnet** | `z3-testnet` | بيئة ما قبل الإنتاج على شبكة الاختبار العامة | 2–12 ساعة | لا (ZEC تجريبية) |
| **regtest** | `z3-regtest` | التدرّب المحلي: كتل (blocks) فورية، بلا أقران | ثوانٍ | لا |

ننصح المشغّلين الجدد بالبدء على **regtest**، والتأكد من عمل RPC وعمليات المحفظة، ثم الانتقال إلى testnet أو mainnet.

---

## منافذ المضيف الافتراضية

صُمّمت الشبكات الثلاث لتعمل معًا على جهاز واحد. القيم أدناه هي القيم الافتراضية المعتمدة، ويمكنك تغيير أي منها عبر متغير البيئة `Z3_*` المقابل. الجدول المرجعي الكامل موجود في [`z3-contract.yaml`](https://github.com/ZcashFoundation/z3/blob/main/z3-contract.yaml).

| الخدمة | Mainnet | Testnet | Regtest |
| --- | --- | --- | --- |
| Zebra JSON-RPC | 8232 | 18232 | 29232 |
| Zebra P2P | 8233 | 18233 | (غير منشور) |
| Zebra الصحة (`/ready`) | 8080 | 18080 | 28080 |
| Zaino gRPC (ملف تعريف المفهرس) | 8137 | 18137 | 28137 |
| Zaino JSON-RPC (ملف تعريف المفهرس) | 8237 | 18237 | 28237 |
| Zallet RPC | 28232 | 40232 | 50232 |

داخل شبكة Compose، يُوصَل إلى الخدمات بأسمائها (`zebra`، `zaino`، `zallet`).

---

## البيانات والنسخ الاحتياطية

| وحدة التخزين | ما تحتويه | هل تحتاج إلى نسخة احتياطية؟ |
| --- | --- | --- |
| `z3-<network>-chain` | حالة السلسلة لـ Zebra (~300 GB للشبكة الرئيسية) | اختياري — يمكن إعادة مزامنتها |
| `z3-<network>-zallet` | قاعدة بيانات المحفظة المشفّرة **و**هوية age التي تفتحها | **نعم — هذه هي وحدة التخزين الوحيدة التي يجب نسخها احتياطيًا** |
| `z3-<network>-zaino` | حالة المفهرس (مع ملف تعريف المفهرس فقط) | اختياري — يمكن إعادة بنائها |
| `z3-<network>-cookie` | ملف cookie لـ RPC الخاص بـ Zebra | لا — يُعاد توليده |

لوضع حالة السلسلة على قرص آخر قبل التشغيل الأول:

```bash
export Z3_CHAIN_DATA_PATH=/mnt/ssd/zebra-state
./scripts/fix-permissions.sh zebra /mnt/ssd/zebra-state
```

يوقف `docker compose --env-file .env.<network> --profile "*" down` الحزمة ويحتفظ بوحدات التخزين. أما إضافة `-v` فتحذفها وتفرض إعادة مزامنة كاملة. أضِف `--profile "*"` حتى تُزال فعليًا الخدمات المرتبطة بملفات التعريف (المفهرس، المراقبة).

---

## البدء

المتطلبات المسبقة: Docker Engine، و Docker Compose v2.24.4+، وGit. ولا تحتاج إلى `openssl` إلا لـ regtest.

### Regtest (أسرع طريقة لرؤية الحزمة)

```bash
git clone https://github.com/ZcashFoundation/z3 && cd z3
./scripts/regtest-init.sh
docker compose --env-file .env.regtest up -d
```

راجع [docs/regtest.md](https://github.com/ZcashFoundation/z3/blob/main/docs/regtest.md) لأوامر الاختبار.

### Mainnet (تشغيل على مرحلتين)

يجب أن يُنهي Zebra المزامنة قبل أن يصبح Zallet مفيدًا. إذا شغّلت Zallet مبكرًا، فسيعيد التشغيل مرارًا إلى أن تصبح قيمة `/ready` صحيحة.

```bash
git clone https://github.com/ZcashFoundation/z3 && cd z3

# 1. One-time setup: local config + Zallet wallet identity
./scripts/setup-network.sh mainnet

# 2. Start Zebra and wait until it is synced
docker compose --env-file .env.mainnet up -d zebra
./scripts/check-zebra-readiness.sh

# 3. Start Zallet (and anything else in the default profile)
docker compose --env-file .env.mainnet up -d
```

خطوات Testnet هي نفسها، مع `.env.testnet` و `./scripts/check-zebra-readiness.sh 18080`.

تبقى التعديلات داخل `config/<network>/` محلية ولا يمحوها `git pull`.

### ملفات التعريف الاختيارية

```bash
# Lightwalletd-compatible gRPC + JSON-RPC proxy
docker compose --env-file .env.mainnet --profile indexer up -d

# Prometheus, Grafana, Jaeger, Alertmanager
docker compose --env-file .env.mainnet --profile monitoring up -d
```

منافذ Grafana الافتراضية هي 3000 (mainnet) و13000 (testnet) و23000 (regtest).

---

## إرشادات للمشغّلين

* **صور مثبّتة الإصدار.** لا ينتقل Z3 تلقائيًا إلى `:latest` من دون علمك. حدّث الإصدار المثبّت في تغيير يخضع للمراجعة، أو اضبط `Z3_<SERVICE>_IMAGE`.
* **حاويات لا تعمل بصلاحيات root.** تُسحب صلاحيات Linux. تمنع فحوصات الصحة تشغيل المحفظة إلى أن يصبح Zebra جاهزًا. وسياسة إعادة التشغيل مفعّلة افتراضيًا.
* **السجلات.** لا يحدّد Z3 برنامج تشغيل للسجلات. اضبط حدود الحجم في إعدادات الخدمة الخلفية (daemon) لـ Docker، وإلا ستكبر السجلات بلا حدود على عقدة تعمل على مدار الساعة.
* **P2P.** تنشر mainnet وtestnet منفذ P2P الخاص بـ Zebra. إذا كنت خلف NAT، فاضبط `ZEBRA_NETWORK__EXTERNAL_ADDR` على العنوان (address) الذي يجب أن يتصل به الأقران. لا يوجد أقران في regtest.
* **Zaino على ARM.** صورة Zaino الرسمية متوفرة لـ `linux/amd64` فقط. على Apple Silicon تعمل عبر المحاكاة ما لم تبنِها من المصدر. أما Zebra و Zallet فيدعمان عدة معماريات.
* **الأجهزة المشتركة.** لا توجد حدود افتراضية لاستخدام المعالج أو الذاكرة. أضِف `deploy.resources.limits` في ملف override إذا لم يكن الجهاز مخصصًا للعقدة.

قائمة تحقق لبيئات الإنتاج وأسئلة شائعة: [docs/faq.md](https://github.com/ZcashFoundation/z3/blob/main/docs/faq.md)، [docs/docker-architecture.md](https://github.com/ZcashFoundation/z3/blob/main/docs/docker-architecture.md).

---

## من يحتاج إلى تشغيل Z3

**يناسب**

* منصات التداول والجهات الوصائية ومجمّعات التعدين التي كانت تستخدم `zcashd` كعقدة ومحفظة معًا
* المشغّلين الذين يريدون RPC مدعومًا لمحفظة عقدة كاملة مرتبطة بـ Zebra متزامن
* المطورين (developers) الذين يحتاجون إلى تشغيل mainnet وtestnet وregtest معًا
* كل من يريد إنشاء نقطة نهاية خاصة متوافقة مع lightwalletd عبر ملف تعريف Zaino

**غالبًا لا يناسب**

* المستخدمين العاديين الذين يريدون فقط إرسال ZEC واستقبالها — استخدم محفظة خفيفة مثل ZODL / Zashi أو Zingo أو YWallet
* من يريد فقط التحقق من صحة السلسلة — شغّل Zebra (أو Zakura) وحده
* من يريد فقط تقديم الكتل المضغوطة — شغّل Zebra + Zaino، أو Zebra + lightwalletd، من دون Zallet

---

## صفحات ذات صلة

* [العقدة الكاملة Zebra](/zcash-tech/zebra-full-node) — عقدة الإجماع التي يبني عليها Z3
* [Zaino](/zcash-tech/zaino) — ملف تعريف المفهرس الاختياري
* [العقد الكاملة](/zcash-tech/full-nodes) — Zebra و Zakura و zcashd المتقاعد
* [عقد المحافظ الخفيفة](/zcash-tech/lightwallet-nodes) — العقد التي يتصل بها العملاء الخفيفون
* [عقدة Zakura](/zcash-tech/zakura-node) — عقدة كاملة بديلة، لكنها لا تأتي مع Z3 حاليًا
* [دليل الترحيل: من zcashd إلى Zebrad/Zallet](/guides/migration-guide-zcashd-to-zebrad-zallet)
* [موارد المطورين](/start-here/developer-resources)

---

## الموارد

* [مستودع Z3](https://github.com/ZcashFoundation/z3)
* [مواصفات Z3 (المنافذ، وحدات التخزين، أسماء المشاريع)](https://github.com/ZcashFoundation/z3/blob/main/z3-contract.yaml)
* [Zebra](https://github.com/ZcashFoundation/zebra) · [كتاب Zebra](https://zebra.zfnd.org/)
* [Zaino](https://github.com/zingolabs/zaino)
* [Zallet](https://github.com/zcash/zallet) · [كتاب Zallet](https://zcash.github.io/zallet/)
* [منتدى مجتمع Zcash — تحديثات Z3](https://forum.zcashcommunity.com/t/zcash-z3-updates-formerly-zcashd-deprecation/48965)
* [Z3 Launcher](https://github.com/Jubrilabdulazeez/z3-launcher) — لوحة تحكم من المجتمع لإدارة حزمة Compose الرسمية (هاكاثون ZecHub)
