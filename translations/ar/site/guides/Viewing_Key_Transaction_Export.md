<a href="https://github.com/zechub/zechub/edit/main/site/guides/Viewing_Key_Transaction_Export.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>

# تصدير سجل المعاملات (transaction) من Viewing Key

معظم عمليات التصدير من المحافظ (wallet) محدودة. فتصدير الضرائب في ZODL مثلًا يمنحك التواريخ والمبالغ والرسوم (fee) للسنة الميلادية السابقة، لكن دون معرّفات المعاملات، ودون المذكرات (memo)، ودون العناوين (address). وهذا لا يكفي لمسك الدفاتر، أو للتحقق من ترحيل محفظة، أو لمعرفة ما حدث لعملية دفع (payment) ما.

لا تحتاج إلى عبارة البذور (seed phrase) للحصول على الصورة الكاملة. فمفتاح العرض (viewing key) الكامل الموحّد (UFVK، ويبدأ بـ `uview1`) يستطيع رؤية كل معاملة واردة وصادرة في الحساب، ويمكن لاثنتين من الأدوات (tools) تحويل ذلك إلى ملف تحتفظ به: خادم GraphQL من Zkool وـ zingo-cli. يجمع هذا الدليل الأساليب الواردة في [سلسلة المنتدى هذه](https://forum.zcashcommunity.com/t/exporting-transaction-history-to-json-csv-from-ufvk-seed/54662) ويحدّثها لتناسب الإصدارات الحالية.

تم الاختبار في سبتمبر 2026 باستخدام Zkool 6.30.0 وـ zingo-cli من zingolib 6.0.0.

## قبل أن تبدأ

تحتاج إلى أمرين:

1. **مفتاح UFVK** الخاص بالحساب. يشرح [مفاتيح العرض](/zcash-tech/viewing-keys) ما يكشفه وكيفية تصديره.
2. **ارتفاع الميلاد** (birth height)، أي الكتلة (block) التي يبدأ منها المسح. استخدم ارتفاعًا يسبق معاملتك الأولى. إذا ضبطته على قيمة مرتفعة جدًا فسيُفقد السجل الأقدم بصمت. وإذا ضبطته على قيمة منخفضة جدًا فسيستغرق المسح وقتًا أطول فحسب. يُعد ارتفاع تفعيل Sapling (419200) آمنًا دائمًا، لكن مسحه قد يستغرق ساعات.

## حافظ على خصوصيته

لا يستطيع مفتاح العرض الإنفاق، لكنه يكشف سجلك بالكامل لمن يحوزه.

- لا تلصقه في موقع ويب أو مستكشف كتل. استورده في برنامج تشغّله بنفسك.
- يرى الخادم الذي تُجري المزامنة منه عنوان IP الخاص بك والمعاملات التي تنزّلها كاملة. تجلب كلتا الأداتين أدناه كل معاملة من معاملاتك بمعرّفها لقراءة المذكرات والرسوم، ويشير [ZIP 307](https://zips.z.cash/zip-0307) إلى أن ذلك يُعلم الخادم بالمعاملات التي تخصك. وتتجنب ذلك بالمزامنة من عقدة (node) Zebra الخاصة بك باستخدام Zaino أو lightwalletd. ويشرح [دليل Zingolib وـ Zaino التعليمي](/guides/zingolib-and-zaino-tutorial) خطوات الإعداد.
- يرسل zingo-cli 6 المدفوعات عبر شبكة mixnet الخاصة بـ Nym، لكن مزامنته لا تزال تتصل بالخادم مباشرة، لذا تنطبق عليه النقطة أعلاه أيضًا.
- أعطِ هذه الأدوات مفتاح عرض، لا عبارة البذور أبدًا. لا يحتوي خادم GraphQL من Zkool على تسجيل دخول افتراضيًا، وستُعيد واجهته البرمجية API عبارة البذور لأي حساب أُنشئ منها، ويمكنها إرسال الأموال.
- أبقِ الخادم على جهازك. الأمر Docker أدناه يستمع على `127.0.0.1` فقط.
- تخزّن كلتا الأداتين المفتاح وسجلك دون تشفير (encryption). احذف بيانات العمل عند الانتهاء واحفظ الملف المُصدَّر في مكان مشفّر.

## الخيار 1: Zkool GraphQL

`zkool_graphql` هو محرك المحفظة الخاص بـ Zkool في صورة خادم مستقل. وهو برنامج منفصل عن تطبيق Zkool. أبسط طريقة لتشغيله هي صورة Docker الرسمية (amd64 وarm64). وهناك أيضًا ملف تنفيذي لـ Linux بمعمارية x86-64 على [صفحة إصدارات Zkool](https://github.com/hhanh00/zkool2/releases)؛ ويتطلب glibc 2.38 أو أحدث، لذا يعمل على Ubuntu 24.04 ولا يعمل على Debian 12.

### 1. تشغيل الخادم

```bash
docker run -d --name zkool-export \
  -p 127.0.0.1:8000:8000 \
  -v zkool-export:/data \
  hhanh00/zkool-graphql:6.30.0 \
  --db-path /data/zkool.db
```

يُزامن من `https://zec.rocks` ما لم تُضف `--lwd-url` مع خادمك الخاص. عند التشغيل الأول يُنزّل معلمات Sapling (نحو 50 MB). إذا فشل ذلك، يحاول `docker start zkool-export` مجددًا.

افتح `http://127.0.0.1:8000/graphiql` في المتصفح. يمكنك لصق كل خطوة من الخطوات التالية هناك وتشغيلها.

### 2. استيراد المفتاح

```graphql
mutation {
  createAccount(newAccount: {
    name: "export"
    key: "uview1..."
    aindex: 0
    birth: 2500000
    useInternal: true
  })
}
```

يُعيد معرّف الحساب الجديد، وهو 1 على خادم جديد.

- اضبط `birth` دائمًا. فبدونه يبدأ Zkool من الكتلة الحالية ولا يجد شيئًا.
- يجعل `useInternal: true` Zkool يتحقق من عناوين الباقي الشفافة (transparent) أيضًا. أبقه مفعّلًا للمفاتيح القادمة من ZODL، وهو الإعداد نفسه الذي يستخدمه [استرداد الأموال](/using-zcash/recovering-funds) لعبارات بذور ZODL.

### 3. المزامنة

```graphql
mutation { synchronizeAccount(idAccount: 1) }
```

يستمر هذا حتى تنتهي المزامنة. لا تُضف `fast: true`. فهو يتخطى تنزيل المعاملات الكاملة، وهي المصدر الذي تأتي منه المذكرات والرسوم والمخرجات.

الرقم الذي يُعيده هو الارتفاع الذي كان يستهدفه، وليس إثباتًا (proof) على أنه بلغه. قد يُنهي خطأ في الشبكة المزامنة مبكرًا دون الإبلاغ عن أي شيء، لذا تحقق:

```graphql
{ currentHeight accounts { id name height } }
```

إذا كان `height` الخاص بالحساب متأخرًا عن `currentHeight`، فشغّل المزامنة مجددًا. ستستأنف من حيث توقفت.

### 4. التصدير

احفظ هذا باسم `history.graphql`:

```graphql
{
  transactionsByAccount(idAccount: 1) {
    txid height time value fee
    notes { pool scope address value memo }
    spends { pool scope address value }
    outputs { pool vout address value memo }
  }
}
```

لا تُدرج الوسيط `height` إلا إذا كنت تقصده. فهو يحدد حدًا أدنى، لذا فإن `height: 3000000` في مثال المنتدى يُسقط كل ما قبل تلك الكتلة.

اجلبه بصيغة JSON:

```bash
jq -n --rawfile q history.graphql '{query: $q}' |
  curl -s http://127.0.0.1:8000/graphql \
    -H 'content-type: application/json' --data-binary @- > history.json
```

يجب أن تُظهر كل معاملة رسومًا أعلى من 0، باستثناء مكافآت التعدين (mining). إذا أظهرت إحداها `"fee": "0"` دون مذكرة، فهذا يعني أن تفاصيلها لم تُنزَّل. يجلب Zkool المعاملات الكاملة واحدة تلو الأخرى بعد المسح، وأي فشل واحد يوقف البقية بصمت. لسرد أي معاملات متأثرة:

```bash
jq -r '.data.transactionsByAccount[] | select(.fee == "0") | .txid' history.json
```

إذا ظهر أي شيء، فزامن مجددًا بعد بضع دقائق وصدّر مجددًا.

ثم حوّله إلى ملف CSV مسطّح، بصف واحد لكل معاملة:

```bash
jq -r '["txid","height","time_utc","net_zec","fee_zec","memos"],
  (.data.transactionsByAccount[] |
    [.txid, .height, .time, .value, .fee,
     ([.notes[].memo, .outputs[].memo] | map(select(. != null and . != "")) | unique | join(" | "))])
  | @csv' history.json > history.csv
```

### قراءة المخرجات

| الحقل | المعنى |
|---|---|
| `value` | صافي التغيّر في رصيد الحساب بوحدة ZEC، شاملًا الرسوم. سالب في عمليات الإرسال. |
| `fee` | الرسوم بوحدة ZEC. في المدفوعات التي تلقيتها، دفعها المرسل وهي غير مُدرجة في `value`. |
| `time` | وقت الكتلة بالتوقيت العالمي UTC، دون مؤشر للمنطقة الزمنية |
| `notes` | ما تلقاه الحساب في هذه المعاملة، بما في ذلك الباقي. المذكرات المرسلة إليك موجودة هنا. الإدخالات الشفافة ليس لها عنوان. |
| `spends` | Notes الخاصة بالحساب التي استهلكتها هذه المعاملة |
| `outputs` | ما أرسلته المعاملة: كل مخرج شفاف، إضافة إلى المدفوعات المحمية (shielded) إلى عناوين أخرى مع مذكراتها |
| `pool` | 0 شفاف، 1 Sapling، 2 Orchard، 3 Ironwood |
| `scope` | 0 خارجي (دفعة واردة)، 1 داخلي (الباقي) |

يحتوي تطبيق Zkool أيضًا على خيارات Export Transactions وـ Memos وـ Notes في قائمة الحساب، لكنها تفريغات خام للجداول: المبالغ بوحدة zatoshis، والطوابع الزمنية بصيغة Unix، والمذكرات في ملف منفصل.

## الخيار 2: zingo-cli

zingo-cli هي محفظة سطر الأوامر الخاصة بـ Zingo. لا توجد تنزيلات جاهزة، لذا تبنيها باستخدام Rust:

```bash
git clone --branch zingolib_v6.0.0 https://github.com/zingolabs/zingolib.git
cd zingolib
cargo build --release -p zingo-cli
cargo build --release --manifest-path zingo-netutils/Cargo.toml --features nym --bin nym-proxy
cp zingo-netutils/target/release/nym-proxy target/release/
```

تحتاج إلى `nym-proxy` حتى لمجرد المزامنة. لن يتصل zingo-cli 6 بأي خادم بدونه.

ينشئ التشغيل الأول محفظة للعرض فقط، ويزامنها ويطبع السجل:

```bash
./target/release/zingo-cli --data-dir "$HOME/zingo-export" \
  --viewkey "uview1..." --birthday 2500000 \
  --server https://zec.rocks:443 \
  --waitsync transactions > transactions.txt
```

- يجب أن يكون `--data-dir` مسارًا مطلقًا.
- لا ينطبق `--viewkey` وـ `--birthday` إلا عند إنشاء المحفظة. لا تُدرجهما بعد ذلك.
- يبدأ zingo-cli في وضع عدم الاتصال افتراضيًا. يختار `--server` الخادم ويُعد أيضًا موافقة منك على الاتصال بالإنترنت.
- ينتهي المطاف بالمفتاح في سجل أوامر الطرفية، لذا امسحه بعد ذلك.

عمليات التشغيل اللاحقة:

```bash
Z="./target/release/zingo-cli --data-dir $HOME/zingo-export"
$Z --server https://zec.rocks:443 --waitsync transactions > transactions.txt
$Z --offline value_transfers > value_transfers.txt
$Z --offline messages > memos.json
```

يقرأ `--offline` ما تمت مزامنته مسبقًا دون الاتصال بالشبكة.

- يعطي `transactions` إدخالًا واحدًا لكل معاملة: txid، والوقت (UTC)، والارتفاع، والنوع (`received` أو `sent` أو `shield` أو `send-to-self`)، والقيمة، والرسوم، وـ notes المعنية.
- يعطي `value_transfers` إدخالًا واحدًا لكل دفعة، لذا فإن الإرسال إلى شخصين يمثل إدخالين، لكل منهما عنوان المستلم والمذكرات.
- يسرد `messages` المذكرات بصيغة JSON.

بعض الأمور التي يجب معرفتها عن المخرجات:

- يطبع `transactions` وـ `value_transfers` نصًا عاديًا يشبه JSON قليلًا لكنه ليس كذلك.
- المبالغ بوحدة zatoshis (100,000,000 مقابل 1 ZEC) وهي موجبة دائمًا. يخبرك `kind` بالاتجاه. في عمليات الإرسال، يمثل `value` ما ذهب إلى أشخاص آخرين، دون الرسوم.
- تظهر الرسوم على أنها «not available» عندما تنفق المعاملة أموالًا شفافة لم تكن لك. لا تُعرض إلا المذكرات النصية.
- إذا فشلت المزامنة، يذهب الخطأ إلى الطرفية لا إلى الملف، ويخرج zingo-cli مع ذلك بشكل طبيعي. تحقق من الطرفية قبل الوثوق بـ `transactions.txt`.

يحتوي [zingoHelper](https://github.com/dismad/zingoHelper) من dismad على سكربت `exportToJSON.sh` يحوّل `transactions` إلى JSON. وقد كُتب قبل zingo-cli 6، وهو مُعدّ للشبكة التجريبية testnet، ويضع عناصر نائبة مكان بعض إدخالات Sapling الصادرة والإدخالات الشفافة، ويحتاج إلى أدوات GNU، لذا لن يعمل على macOS الافتراضي. تعامل مع مخرجاته كنقطة انطلاق وتحقق من المجاميع.

## ما لا يستطيع مفتاح العرض إخبارك به

- **الأسعار.** لا تسجل أي من الأداتين سعر ZEC وقت كل معاملة. أضف القيم بالعملة الورقية بنفسك.
- **السجل الشفاف، إذا لم يتضمنه المفتاح.** الجزء الشفاف من UFVK اختياري بموجب [ZIP 316](https://zips.z.cash/zip-0316). باستخدام zingo-cli، يُظهر `$Z --offline parse_viewkey uview1...` المجمّعات (pool) التي يغطيها المفتاح.
- **من دفع لك.** لا تحمل المدفوعات المحمية عنوان المرسل. وما لم يضعه المرسل في المذكرة، فهو غير موجود في أي مكان.
- **بعض تفاصيل الإرسال.** يُستعاد عنوان الوجهة والمبلغ والمذكرة لعمليات الإرسال المحمية عبر فك التشفير بالمفتاح. يمكن للمحفظة أن تبني معاملة بحيث لا يكون ذلك ممكنًا، وإن كانت معظم المحافظ لا تفعل ذلك.

## أدوات أخرى

| الأداة | ما تحصل عليه |
|---|---|
| ZODL | ملف CSV ضريبي يتضمن التواريخ والمبالغ والرسوم ووسمًا. للسنة الميلادية السابقة فقط، ويتخطى معاملات الحماية، ولا يتضمن txid أو مذكرة أو عنوانًا. |
| تطبيق Zkool | تصديرات خام للجداول من قائمة الحساب |
| [Zenith](https://code.vergara.tech/Vergara_Tech/zenith) | يستورد UFVK باستخدام `importvk`. يُعيد `listreceived` عبر RPC الـ notes المستلمة مع txid والمذكرة، لكن دون عمليات الإرسال ودون الرسوم. |
| [Zallet](https://github.com/zcash/zallet) | `z_listtransactions` مفصّل لكنه موسوم بأنه تجريبي، ولا يستورد Zallet إلا مفاتيح عرض Sapling، لا UFVK |
| [zcash-devtool](https://github.com/zcash/zcash-devtool) | يستورد UFVK باستخدام `wallet init-fvk`، ثم `wallet list-tx`. لا يتضمن وضع CSV فيه txid أو عنوانًا، ويقول المشروع إنه يجب عدم استخدامه في بيئة الإنتاج. |

## ذات صلة

- [مفاتيح العرض](/zcash-tech/viewing-keys)
- [استرداد الأموال](/using-zcash/recovering-funds)
- [دليل Zingolib وـ Zaino التعليمي](/guides/zingolib-and-zaino-tutorial)
- [المنتدى: تصدير سجل المعاملات إلى JSON/CSV من UFVK/عبارة البذور](https://forum.zcashcommunity.com/t/exporting-transaction-history-to-json-csv-from-ufvk-seed/54662)
- [المنتدى: Zkool وـ GraphQL](https://forum.zcashcommunity.com/t/zkool-graphql/54100)
- [ملف README الخاص بـ zingo-cli](https://github.com/zingolabs/zingolib/blob/zingolib_v6.0.0/zingo-cli/README.md)
