<a href="https://github.com/zechub/zechub/edit/main/site/Zcash_Tech/Zallet.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>

# Zallet

Zallet هي محفظة (wallet) Zcash من نوع العقدة الكاملة (full node)، مكتوبة بلغة Rust. وهي البديل عن المحفظة التي كانت مدمجة سابقًا في `zcashd`. بعد أن بلغت `zcashd` توقف نهاية الدعم في 18 يوليو 2026 عند ارتفاع الكتلة (block) 3417100، جرى فصل مهام الإجماع (consensus) عن مهام المحفظة: إذ تتحقق **Zebra** أو **Zakura** من صحة السلسلة (chain)، بينما تحتفظ **Zallet** بالمفاتيح، وتفحص الـ notes، وتوفّر واجهة JSON-RPC الخاصة بالمحفظة.

Zallet حاليًا في مرحلة **تجريبية (beta)**. ولم تخضع لمراجعة كاملة بعد. وقد تتطلب التغييرات الجذرية حذف المحفظة وإعادة إنشائها. لا تعتمد عليها كحلّ حفظ إنتاجي لمبالغ كبيرة من ZEC قبل قراءة التحذيرات الأمنية في [كتاب Zallet](https://zcash.github.io/zallet/).

---

## باختصار

- Zallet هي **محفظة RPC من نوع العقدة الكاملة**، وليست محفظة خفيفة للهاتف المحمول، وليست عقدة (node) إجماع.
- تحل محل نصف المحفظة في `zcashd`. أما نصف العقدة فهو [Zebra](Zebra_Full_Node.md) أو [Zakura](Zakura_Node.md).
- مكتوبة بلغة **Rust**، ومرخّصة ترخيصًا مزدوجًا MIT / Apache-2.0، ويجري صيانتها في [zcash/zallet](https://github.com/zcash/zallet).
- أحدث إصدار منشور حتى أواخر أغسطس 2026: **v0.1.0-beta.3**.
- تتواصل مع بيانات السلسلة عبر إحدى واجهتين خلفيتين: **zebra-state** (`ReadStateService` مباشر مقابل `zebrad` محلي) أو **Zaino**.
- توفّر مجموعة فرعية من **JSON-RPC متوافقة مع zcashd**. تغيّرت بعض الأساليب، وحُذف بعضها عمدًا.
- تُحمى مواد المفاتيح دائمًا بالتشفير (encryption) باستخدام **age**. أما سجل المعاملات (transactions) والعناوين (addresses) ومفاتيح العرض (viewing keys) فتُخزَّن دون تشفير في `wallet.db`.
- تُشحن ثلاثة ملفات تنفيذية في أرشيف واحد موقّع: `zallet` (المشغّل)، و`zallet-zebra`، و`zallet-zaino`.
- الوثائق الرسمية: [كتاب Zallet](https://zcash.github.io/zallet/).

---

## لماذا وُجدت Zallet

جمعت `zcashd` عقدة إجماع مشتقة من Bitcoin Core ومحفظة في عملية واحدة. وقد انتهى هذا التصميم.

| الدور | المكدس القديم | المكدس الحالي |
|------|-----------|---------------|
| الإجماع / P2P | `zcashd` | Zebra (`zebrad`) أو Zakura |
| المحفظة / المفاتيح / الأرصدة | `zcashd` `wallet.dat` | **Zallet** (`wallet.db`) |
| مفهرس العميل الخفيف (light client) | `lightwalletd` | Zaino أو `lightwalletd` |

يعني فصل المحفظة عن العقدة ما يلي:

- يمكن استبدال برمجيات العقدة (Zebra مقابل Zakura) دون نقل المفاتيح.
- يجري فحص المحفظة وصلاحية الإنفاق في عملية يمكن تأمينها بشكل منفصل.
- يمكن لدلالات RPC أن تتطور نحو حسابات ZIP 32 والعناوين الموحّدة وPCZTs بدلًا من البقاء جامدة على خصوصيات `zcashd`.

Zallet هي المحفظة المخصّصة للمشغّلين الذين كانوا يشغّلون `zcashd` سابقًا كمحفظة ساخنة، أو كواجهة خلفية لمنصة تداول (exchange)، أو كـ faucet، أو كمحفظة لمدفوعات التعدين (mining).

---

## الحالة

Zallet في مرحلة **تجريبية (beta)**.

ما يعنيه ذلك عمليًا:

- قد تصل تغييرات جذرية في أي إصدار تجريبي. وقد تضطر إلى حذف دليل البيانات والبدء من جديد.
- لم تُنقل جميع أساليب RPC الخاصة بمحفظة `zcashd` بعد.
- تختلف دلالات بعض الأساليب المنقولة عن `zcashd`. يجب على عمليات التكامل قراءة [صفحة الدلالات المعدّلة](https://zcash.github.io/zallet/zcashd/json_rpc.html).
- الحزم (crates) قيد التطوير ولم تخضع لمراجعة كاملة.
- Zallet **ليست** مكتبة Rust. ولا توجد أي ضمانات إذا اعتمدت عليها بهذه الصفة.

تُرسَل الآراء والتعليقات إلى [مشكلات GitHub](https://github.com/zcash/zallet/issues/new) أو إلى قناة `#wallet-dev` على [خادم Discord للبحث والتطوير في Zcash](https://discord.gg/xpzPR53xtU).

من المخطط الانتقال لاحقًا إلى مرحلة مستقرة بمجرد اكتمال واجهة RPC المقصودة. وسيُتوقع حينها من المستدعين الانتقال إلى أساليب Zallet، بما في ذلك الاختلافات الدلالية الموثّقة.

---

## البنية

تنقسم Zallet على ثلاث مساحات عمل Cargo بحيث يمكن لكل من الواجهتين الخلفيتين للسلسلة تتبّع رسوم بيانية مختلفة للاعتماديات.

```
zallet            launcher: reads `backend` in zallet.toml (default "zebra")
                  and execs zallet-zebra or zallet-zaino
zallet-core       shared wallet: CLI, config, JSON-RPC, SQLite DB, sync
zallet-zebra      zebra-state backend (ReadStateService + Zebra JSON-RPC)
zallet-zaino      Zaino indexer backend
```

تفتح الملفات التنفيذية الثلاثة `wallet.db` **نفسه**. يختار المشغّل الواجهة الخلفية وقت التشغيل؛ فلا حاجة إلى إعادة الترجمة للتبديل.

النشر النموذجي:

```
zebrad  (or Zakura)
   │  JSON-RPC / ReadStateService
   ▼
Zallet  (zallet-zebra or zallet-zaino)
   │  JSON-RPC on 127.0.0.1
   ▼
Your application, exchange, faucet, or operator scripts
```

Zallet هي **محفظة من نوع العقدة الكاملة**: فهي تتوقع وجود عقدة تحقق محلية. وليست عميلًا خفيفًا. للمحافظ الخفيفة وخوادم الكتل المضغوطة، راجع [Zaino](Zaino.md) و[عقد المحافظ الخفيفة](Lightwallet_Nodes.md).

يشغّل مكدس compose [Z3](https://github.com/ZcashFoundation/z3) التابع لـ Zcash Foundation كلًا من Zebra و Zallet معًا، مع Zaino مستقل اختياري للعملاء الخفيفين الخارجيين.

---

## الحسابات والعناوين والمفاتيح

بُنيت Zallet حول حسابات ZIP 32، وليس حول الحساب الضمني الوحيد في `zcashd`.

- يمكن للمحفظة أن تحتوي على **عدة عبارات استذكارية BIP 39**. كل عبارة استذكارية هي جذر إنفاق مستقل، يُعرَّف بـ **بصمة البذرة** (`zip32seedfp1…`).
- تُشتق **الحسابات** من البذرة باستخدام فهرس حساب ZIP 32. وداخل نسخة واحدة من Zallet يكون لها أيضًا **UUID** محلي. أما الهوية القابلة للنقل للحساب فهي `(seedfp, account index)`.
- العناوين هي **عناوين موحّدة وفق ZIP 316**، تُنتَج باستخدام `z_getaddressforaccount`. يمكن أن يكون للحساب الواحد عناوين متنوّعة كثيرة؛ ولا يمكن ربط المستقبِلات المحمية (shielded) على السلسلة (on-chain).
- تصبح مفاتيح الإنفاق (spending keys) المستوردة (`z_importkey`) والعناوين المخصّصة للمراقبة فقط (`z_importaddress`) حسابات UUID لا تغطيها أي عبارة استذكارية.
- يمكن تصدير مفاتيح العرض واستيرادها (`z_exportviewingkey`، `z_importviewingkey`)، بما في ذلك مفاتيح العرض الكاملة الموحّدة ومفاتيح العرض الواردة.

`getnewaddress` غير مُنفَّذ. استخدم `z_getnewaccount` و `z_getaddressforaccount`.

إذا كان `keystore.require_backup` مُفعّلًا (وهو الشكل المُرحَّل من `walletrequirebackup` في `zcashd`)، فإن Zallet ترفض اشتقاق صلاحية إنفاق جديدة من عبارة استذكارية لم يُؤكَّد نسخها الاحتياطي.

---

## التشفير والنسخ الاحتياطية

مواد المفاتيح مشفّرة **دائمًا**. لا يوجد وضع غير مشفّر، ولا يوجد أسلوب RPC `encryptwallet` — فهذا الأسلوب في `zcashd` لم يكن مدعومًا بالكامل قط.

- ينشئ الإعداد هوية **age**، ومسارها الافتراضي `{datadir}/encryption-identity.txt`.
- تُخزَّن العبارات الاستذكارية ومفاتيح الإنفاق المستوردة كنصوص مشفّرة بـ age في `wallet.db`.
- بقية قاعدة البيانات **غير** مشفّرة. فالسجل والعناوين ومفاتيح العرض قابلة للقراءة إذا حصل أحد على الملف.
- يمكن تغليف الهوية بعبارة مرور (`generate-encryption-identity -p`). افتح القفل باستخدام أسلوب RPC `walletpassphrase`، وأقفل باستخدام `walletlock`.
- يؤدي فقدان ملف الهوية أو عبارة مروره إلى استحالة استعادة مفاتيح الإنفاق. احتفظ بنسخة احتياطية من الهوية، ومن كل عبارة استذكارية، و(بشكل منفصل ومشفّر) من أي نسخة `wallet.db` تحتفظ بها.

نسخ `wallet.db` أثناء تشغيل Zallet ليس نسخًا احتياطيًا آمنًا. فقد يتلف SQLite. فضّل إيقاف العملية أولًا، أو انتظر أمرًا رسميًا للنسخ الاحتياطي أثناء التشغيل.

---

## JSON-RPC

تنفّذ Zallet مجموعة فرعية من أساليب RPC الخاصة بمحفظة `zcashd` عبر HTTP مع مصادقة Basic. اربطها بواجهة loopback. يجب أن يمر الاستخدام عن بُعد عبر نفق مشفّر. الخيار `rpc.allow_insecure_remote_bind` موجود لكنه غير آمن.

اختلافات بارزة عن `zcashd`:

- حقول الرصيد في `getwalletinfo` فارغة. استخدم `z_getbalances`، و`z_getbalanceforaccount`، و`z_gettotalbalance`.
- تتبع الرسوم (fees) **ZIP 317**. لا يوجد `settxfee`.
- ينتقل بناء عمليات الإنفاق إلى **PCZTs** (معاملات Zcash المُنشأة جزئيًا، ZIP 374). وقد وصلت أساليب RPC الخاصة بـ PCZT في سلسلة الإصدارات التجريبية.
- يحظر **قفل مزامنة** عام أساليب RPC الخاصة بالرصيد والإنفاق أثناء لحاق المحفظة بالسلسلة أو تعافيها من إعادة تنظيم السلسلة (`ClientInInitialDownload` / `ForbiddenBySafeMode`).

تشمل الأساليب المحذوفة عمدًا `createrawtransaction`، و`fundrawtransaction`، و`getnewaddress`، و`getrawchangeaddress`، و`keypoolrefill`، و`importwallet`، و`encryptwallet`. وترد البدائل في [كتاب Zallet](https://zcash.github.io/zallet/zcashd/json_rpc.html).

---

## البدء

توجد مسارات التثبيت الرسمية (حزم Debian، وDocker، والملفات التنفيذية للإصدارات) في [دليل التثبيت](https://zcash.github.io/zallet/guide/installation/index.html). تُسمّى أرشيفات الإصدارات `zallet-<version>-<arch>.tar.gz` وتحتوي على الملفات التنفيذية الثلاثة.

الحد الأدنى لخطوات إنشاء محفظة جديدة:

```bash
# data directory; default is $HOME/.zallet
zallet -d /path/to/zallet/datadir example-config > /path/to/zallet/datadir/zallet.toml
# edit zallet.toml: network, backend, indexer / read-state, rpc.bind

zallet -d /path/to/zallet/datadir generate-encryption-identity
zallet -d /path/to/zallet/datadir init-wallet-encryption
zallet -d /path/to/zallet/datadir generate-mnemonic
zallet -d /path/to/zallet/datadir confirm-backup
zallet -d /path/to/zallet/datadir start
```

وجّه `[indexer]` إلى نقطة نهاية JSON-RPC محلية لـ `zebrad`. تحتاج الواجهة الخلفية zebra أيضًا إلى `[indexer.read_state_service]` وإلى `zebrad` مبنيّ مع ميزة المفهرس حتى تتمكن Zallet من قراءة حالة السلسلة مباشرة.

يمكن بناء صور قابلة لإعادة الإنتاج باستخدام [StageX](https://codeberg.org/stagex/stagex/) (Docker 25+، ومخزن صور containerd، وGNU Make).

---

## الانتقال من zcashd

احتفظ بدليل بيانات `zcashd` القديم إلى أن تتأكد من الأرصدة وتختبر عملية استعادة ناجحة.

```bash
zallet init-wallet-encryption
zallet migrate-zcash-conf --zcashd-datadir /path/to/zcashd/datadir \
  -o /path/to/zallet/datadir/zallet.toml
zallet migrate-zcashd-wallet --zcashd-datadir /path/to/zcashd/datadir
```

لا يتوفر `migrate-zcashd-wallet` إلا في الإصدارات المبنية مع ميزة `zcashd-import`. وتتطلب قراءة `wallet.dat` الأداة `db_dump` من **Berkeley DB 6.2**، وهي النسخة التي استخدمتها `zcashd`.

ملاحظات تشغيلية خطوة بخطوة: [دليل الانتقال: من zcashd إلى Zebrad/Zallet](/guides/migration-guide-zcashd-to-zebrad-zallet).

---

## علاقة Zallet بالبرمجيات الأخرى

| | Zallet | zecd | Zashi / ZODL / YWallet | Zebra / Zakura | Zaino |
|--|--------|------|------------------------|----------------|-------|
| ما هو | محفظة RPC من نوع العقدة الكاملة | خادم محفظة يعطي الأولوية للمعاملات المحمية | محافظ للمستخدم النهائي | عقدة إجماع | مفهرس / بديل لـ lightwalletd |
| يحل محل | محفظة `zcashd` | ليس نسخة بديلة مباشرة عن `zcashd` | تطبيقات الهاتف/سطح المكتب | عقدة `zcashd` | `lightwalletd` |
| يحتاج إلى عقدة محلية | نعم | نعم (Zebra افتراضيًا) | لا (عميل خفيف) | هو *ذاته* العقدة | نعم |
| توافق RPC مع zcashd | مصمَّم ليكون مسار التوافق | مجموعة فرعية صغيرة مختارة فقط | لا ينطبق | جزئي / وضع توافق Zakura | واجهة API مختلفة |
| نموذج الحفظ | يحتفظ المشغّل بالمفاتيح في `wallet.db` | خادم قابل للاستعادة من البذرة | مفاتيح على جهاز المستخدم | لا توجد محفظة | لا توجد مفاتيح |

يمكن لكل من Zallet و **zecd** أن يعمل أمام Zebra. اختر Zallet عندما تحتاج إلى واجهة محفظة `z_*` ومسار انتقال من `wallet.dat`. واختر zecd عندما تريد خادمًا يعطي الأولوية للمعاملات المحمية وليس *صراحةً* نسخة عن `zcashd`.

يوجد منتج استهلاكي منفصل على [zallet.io](https://www.zallet.io/) يستخدم الاسم نفسه. وذلك التطبيق ليس هذا المشروع.

---

## صفحات ذات صلة

- [العقد الكاملة](Full_Nodes.md) — Zebra و Zakura وعقدة `zcashd` المتقاعدة
- [العقدة الكاملة Zebra](Zebra_Full_Node.md) — العقدة التي تقرأ منها الواجهة الخلفية الافتراضية لـ Zallet
- [عقدة Zakura](Zakura_Node.md) — عقدة تحقق بديلة
- [Zaino](Zaino.md) — الواجهة الخلفية للمفهرس وخادم العميل الخفيف
- [zecd](ZECD.md) — تصميم آخر لخادم محفظة يعمل على librustzcash
- [مزامنة محافظ Zcash](Zcash_Wallet_Syncing.md) — كيف تفحص المحافظ المحمية السلسلة
- [مفاتيح العرض](Viewing_Keys.md)

## الموارد

- [كتاب Zallet](https://zcash.github.io/zallet/)
- [zcash/zallet على GitHub](https://github.com/zcash/zallet)
- [الإصدارات](https://github.com/zcash/zallet/releases)
- [الدلالات المعدّلة لـ JSON-RPC](https://zcash.github.io/zallet/zcashd/json_rpc.html)
- [دليل الانتقال إلى ZecHub](/guides/migration-guide-zcashd-to-zebrad-zallet)
- [دليل Raspberry Pi لـ ZecHub (Zebra + Zallet)](/guides/raspberry-pi-4-full-node)
- [Z3 (مكدس compose يضم Zebra + Zallet)](https://github.com/ZcashFoundation/z3)
- [خادم Discord للبحث والتطوير في Zcash](https://discord.gg/xpzPR53xtU) — `#wallet-dev`
