<a href="https://github.com/zechub/zechub/edit/main/site/Using_Zcash/Zimppy.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>

# Zimppy.xyz

## باختصار

- **Zimppy** بنية تحتية للمدفوعات (payment) تضع الخصوصية (privacy) أولاً، مخصّصة لوكلاء الذكاء الاصطناعي، وتستخدم بروتوكول مدفوعات الآلات (MPP) الخاص بـ Zcash
- **أودِع مرة واحدة** على السلسلة (on-chain) (نحو 25 ثانية لكل كتلة (block) منذ NU7)، ثم نفّذ **طلبات فورية غير محدودة** دون أي تفاعل مع سلسلة الكتل (blockchain) لكل طلب
- يدعم مدفوعات **Zcash (Orchard) المحمية (shielded) بالكامل** — المرسل والمستلم والمبلغ والمذكرة (memo) كلها مشفّرة
- يعمل مع **حزم SDK لـ TypeScript و Rust** لتسهيل الدمج في مسارات الذكاء الاصطناعي وخوادم API
- مثالي لـ **واجهات LLM البرمجية، وأسواق البيانات، وخوادم أدوات (tools) MCP**، ولأي حالة استخدام (use case) للمدفوعات بين الآلات (M2M)

---

> **Zimppy** هو طريقة الدفع ضمن بروتوكول مدفوعات الآلات (MPP) لـ Zcash، ويدعم المدفوعات المحمية والشفافة (transparent) معاً. أودِع مرة واحدة على السلسلة، ثم نفّذ طلبات فورية غير محدودة برمز حامل دون أي تفاعل مع السلسلة (chain) لكل طلب.

---

## جدول المحتويات

1. [ما هو Zimppy.xyz؟](#what-is-zimppyxyz)
2. [لماذا المدفوعات المحمية لوكلاء الذكاء الاصطناعي؟](#why-shielded-payments-for-ai-agents)
3. [بروتوكول مدفوعات الآلات MPP](#machine-payment-protocol-mpp)
4. [كيف يعمل Zimppy](#how-zimppy-works)
   - [الجلسات (موصى بها)](#sessions-recommended)
   - [البث](#streaming)
   - [الدفع الفردي](#charge)
5. [حالات الاستخدام والأمثلة](#use-cases--examples)
6. [التثبيت](#installation)
7. [إعداد محفظة Zimppy](#setting-up-the-zimppy-wallet)
8. [دمج Zimppy](#integrating-zimppy--typescript-sdk)
   - [الخادم (المحمي)](#typescript-server--shielded)
   - [الخادم (الشفاف)](#typescript-server--transparent)
   - [العميل](#typescript-client)
9. [دمج Zimppy - حزمة SDK لـ Rust](#integrating-zimppy--rust-sdk)
   - [الخادم (Axum)](#rust-server-axum)
   - [العميل](#rust-client)
10. [مرجع CLI](#cli-reference)
11. [الميزات الرئيسية](#key-features)
12. [البنية](#architecture)
13. [الأمثلة والعروض التوضيحية](#examples--demos)

---

## ما هو Zimppy.xyz؟

**Zimppy.xyz** بنية تحتية للمدفوعات تضع الخصوصية أولاً، صُمّمت خصيصاً لوكلاء الذكاء الاصطناعي ولمسارات العمل الآلية بين الآلات (M2M). وهي تطبّق **بروتوكول مدفوعات الآلات (MPP)** باستخدام **Zcash** عملةً أساسية، مما يتيح وضعَي دفع: المحمي (الخاص بالكامل) والشفاف.

على خلاف أنظمة الدفع التقليدية القائمة على سلسلة الكتل، حيث تكون كل معاملة (transaction) مرئية للجميع على السلسلة، صُمّم Zimppy حول بنية قائمة على الجلسات تلغي زمن الانتظار لكل طلب مع الحفاظ على الخصوصية التشفيرية. وهذا يجعله مناسباً على نحو فريد لوكلاء الذكاء الاصطناعي الذين يحتاجون إلى الدفع برمجياً مقابل واجهات API أو البيانات أو قدرة الحوسبة أو أدوات الذكاء الاصطناعي، دون تسريب بيانات وصفية سلوكية.

### الخصائص الأساسية

- **أودِع مرة واحدة** على السلسلة (نحو 25 ثانية لكتلة Zcash منذ NU7؛ و75 ثانية قبل ذلك)
- **طلبات فورية غير محدودة** بعد فتح الجلسة، دون أي تفاعل مع السلسلة لكل طلب
- **المدفوعات المحمية** تشفّر المرسل والمستلم والمبلغ والمذكرة باستخدام بروتوكول Orchard الخاص بـ Zcash
- **المدفوعات الشفافة** تستخدم عناوين T (T-addresses) خاصة بكل تحدٍّ لمنع إعادة التشغيل دون خصوصية كاملة
- **متوافق مع المواصفات**، تحديات HMAC-SHA256، وأخطاء RFC 9457، واكتشاف عبر `/.well-known/payment`

---

## لماذا المدفوعات المحمية لوكلاء الذكاء الاصطناعي؟

بالنسبة لوكلاء الذكاء الاصطناعي الذين يتولّون مسارات عمل حساسة، كالبحث القانوني والاستعلامات الطبية والتحليل المالي والاستخبارات التنافسية، فإن **كل دفعة علنية هي تسريب للبيانات الوصفية**. Zimppy هو طريقة الدفع الوحيدة في MPP التي تكون **خاصة افتراضياً**.

### جدول مقارنة الخصوصية

| الخاصية | السلاسل العامة (USDC، ETH) | Zimppy المحمي | Zimppy الشفاف |
|---|---|---|---|
| **المرسل** | مرئي | مشفّر | مرئي |
| **المستلم** | مرئي | مشفّر | لكل تحدٍّ (غير قابل للربط (unlinkable)) |
| **المبلغ** | مرئي | مشفّر | مرئي |
| **المذكرة** | مرئية | مشفّرة | غير متاح |
| **الحماية من إعادة التشغيل** | لا شيء | ربط المذكرة | عنوان T لكل تحدٍّ |
| **نمط استخدام الخدمة** | قابل للربط | خاص | غير قابل للربط (عنوان جديد) |

### مشكلة زمن الانتظار، وحلّها عبر الجلسات

> *«لكن كتل Zcash لا تزال تستغرق 25 ثانية (75 ثانية قبل NU7).»*

**الجلسات تحلّ هذه المشكلة.** يحدث الانتظار على السلسلة **مرة واحدة** فقط عند الإيداع. وكل طلب لاحق يكون فورياً.

```
Agent  ->  deposit 100,000 zat           (one on-chain tx, ~25s)
Agent  ->  open session                  (bearer token issued)
Agent  ->  request -> response           (0ms - no chain interaction)
Agent  ->  request -> response           (0ms - no chain interaction)
Agent  ->  request -> response           (0ms - no chain interaction)
           ... hundreds of requests ...
Agent  ->  close session                 (refund unused balance)
```

**ادفع مرة واحدة، واتصل فوراً، واسترد الباقي.** زمن الانتظار لكل طلب يساوي صفراً.

---

## بروتوكول مدفوعات الآلات (MPP)

**بروتوكول مدفوعات الآلات (MPP)** بروتوكول موحّد يتيح للوكلاء البرمجيين المستقلين (وكلاء الذكاء الاصطناعي، والروبوتات البرمجية، والسكربتات) اكتشاف متطلبات الدفع للوصول إلى واجهات API والتفاوض بشأنها والوفاء بها، كل ذلك دون تدخل بشري.

### كيف يندمج MPP مع واجهات API

يتّبع MPP مسار HTTP **402 Payment Required**:

1. **يطلب الوكيل** مورداً من نقطة نهاية API مدفوعة.
2. **يستجيب الخادم** بـ `402 Payment Required` + تحدٍّ موقّع (المبلغ، والمستلم، والمذكرة).
3. **يدفع الوكيل** باستخدام طريقة دفع متوافقة (مثل Zcash المحمي عبر Zimppy).
4. **يعيد الوكيل** إرسال الطلب مع `Authorization: Payment {txid}`.
5. **يتحقق الخادم** من الدفع تشفيرياً (فك التشفير (encryption) عبر IVK لـ Orchard، والتحقق من المبلغ + المذكرة).
6. **يستجيب الخادم** بـ `200 OK` + ترويسة `Payment-Receipt`.

### التوافق مع المواصفات

- توقيع التحديات بـ **HMAC-SHA256**
- استجابات أخطاء منظَّمة وفق **RFC 9457**
- نقطة نهاية **`/.well-known/payment`** للاكتشاف التلقائي لطريقة الدفع
- **IVK لـ Orchard** (Incoming Viewing Key) للتحقق من الدفع على جانب الخادم دون كشف مفاتيح الإنفاق (spending keys)

---

## كيف يعمل Zimppy

### الجلسات (موصى بها)

الجلسات هي نمط التفاعل الأساسي. يودِع الوكيل رصيداً على السلسلة مرة واحدة، ويتلقى رمزاً (token) حاملاً، ويستخدمه في جميع الطلبات اللاحقة بزمن انتظار صفري.

```
Agent  ->  deposit 100,000 zat           (on-chain, ~75s one-time)
Agent  ->  open session                  (bearer token issued)
Agent  ->  GET /api/query + bearer       (instant, balance deducted)
Agent  ->  GET /api/query + bearer       (instant, balance deducted)
Agent  ->  close session                 (refund unused balance on-chain)
```

**الأنسب لـ:** استدعاءات API عالية التكرار، واستدلال LLM، واستعلامات البيانات المتكررة.

---

### البث

محتوى مُقاس بنظام الدفع لكل رمز يُسلَّم عبر **الأحداث المرسلة من الخادم (SSE)**. يخصم الخادم من رصيد الجلسة لكل كلمة أو رمز يُبَث.

```
Agent  ->  open session with deposit
Agent  ->  GET /api/stream (SSE)
Server ->  stream word by word, deducting per token
Agent  ->  close session, refund remaining
```

**الأنسب لـ:** استجابات LLM المتدفقة، وتغذيات البيانات الفورية، وأدوات الذكاء الاصطناعي المدفوعة لكل رمز.

---

### الدفع الفردي

دفعة محمية واحدة لكل طلب. يُنفَّذ مسار HTTP 402 الكامل في كل استدعاء. مناسب عندما تكون الطلبات نادرة أو عالية القيمة.

```
Agent  ->  GET /api/resource
Server ->  402 + challenge (amount, recipient, memo)
Agent  ->  shielded ZEC with memo "zimppy:{challenge_id}"
Agent  ->  GET /api/resource + Authorization: Payment {txid}
Server ->  decrypt with Orchard IVK, verify amount + memo
Server ->  200 OK + Payment-Receipt
```

**الأنسب لـ:** الطلبات الفردية عالية القيمة، واستدعاءات API النادرة، ونقاط نهاية البيانات المميزة.

---

## حالات الاستخدام والأمثلة

### 1. وكيل ذكاء اصطناعي

يستعلم وكيل ذكاء اصطناعي قانوني في قاعدة بيانات مدفوعة للسوابق القضائية. باستخدام جلسات Zimppy المحمية، لا تكون هوية مكتب المحاماة ولا الاستعلامات المحددة مرئية على السلسلة - مما يحمي السرية بين المحامي وموكّله على مستوى البنية التحتية.

```
Agent opens session (100,000 zat deposit)
-> GET /api/cases?q=patent+infringement+2024     (instant)
-> GET /api/cases?q=prior+art+semiconductor      (instant)
-> GET /api/document/US11234567B2                (instant)
Session closed, unused balance refunded
```

### 2. وكيل ذكاء اصطناعي لمسار الاستعلامات الطبية

يستعلم وكيل تشخيص طبي في عدة قواعد بيانات سريرية. تضمن المدفوعات المحمية ألا تكون أنماط استعلامات المرضى قابلة للربط بين المزوّدين.

### 3. وكيل التحليل المالي

يدفع وكيل تداول خوارزمي مقابل واجهات API لبيانات السوق الفورية. تستخدم المدفوعات الشفافة عناوين T جديدة لكل تحدٍّ، مما يمنع الربط بين أنماط الاستخدام عبر مزوّدي البيانات.

### 4. خادم أدوات MCP، أدوات ذكاء اصطناعي مدفوعة

يعرض خادم MCP (Model Context Protocol) أدوات ذكاء اصطناعي مدفوعة. كل استدعاء لأداة يُطلق دفعة Zimppy فردية، مما يتيح سوقاً لقدرات الذكاء الاصطناعي المدرّة للدخل.

### 5. ملخِّص LLM، الدفع لكل رمز

تفرض خدمة تلخيص قائمة على LLM رسوماً على الوكلاء لكل رمز مُخرَج عبر بث SSE، مع خصم تلقائي من الرصيد واسترداد الرصيد المدفوع مسبقاً غير المستخدم.

---

## التثبيت

### Node.js / TypeScript

```bash
npm install zimppy          # CLI + wallet
npm install zimppy-ts       # TypeScript SDK
```

### Rust

```toml
[dependencies]
zimppy-core = "0.5"         # Rust verification engine
zimppy-rs = "0.5"           # Rust SDK (charge, session, axum)
```

---

## إعداد محفظة Zimppy

توفّر واجهة سطر الأوامر CLI الخاصة بـ Zimppy واجهة محفظة (wallet) كاملة. جميع الأوامر متاحة عبر `npx zimppy`.

### الخطوة 1 : أنشئ محفظة

```bash
npx zimppy wallet create
```

يولّد مفاتيح تشفيرية ويعرض **عبارة البذور (seed phrase)** الخاصة بك. احفظها بأمان - لا يمكن استعادتها إذا فُقدت.

### الخطوة 2 : تحقّق من عنوانك ورصيدك

```bash
npx zimppy wallet whoami
```

يعرض **العنوان (address) الموحّد (UA)** الخاص بك، و**عنوان T**، والرصيد الحالي، والشبكة النشطة.

```bash
npx zimppy wallet balance --all
```

يعرض تفصيل الرصيد لكل حساب عبر جميع حسابات ZIP-32.

### الخطوة 3 : موّل محفظتك

أرسل ZEC إلى عنوانك الموحّد من أي محفظة أو منصة تداول (exchange) متوافقة مع Zcash. تذهب الإيداعات المحمية مباشرةً إلى حساب Orchard الخاص بك.

### الخطوة 4 : أرسل الأموال واحمِها

```bash
# Send ZEC to any address (shielded or transparent)
npx zimppy wallet send <addr> 42000

# Move transparent funds into Orchard (shielded)
npx zimppy wallet shield

# Transfer between your own accounts
npx zimppy wallet transfer 0 1 50000

# Switch active wallet identity
npx zimppy wallet use work
```

### الخطوة 5 : نفّذ طلباً بالدفع التلقائي

```bash
npx zimppy request <url>
```

يتولّى تلقائياً المسار الكامل 402 -> دفع -> إعادة المحاولة. تُفتح الجلسات وتُدار بشكل غير مرئي لك.

---

## دمج Zimppy - حزمة SDK لـ TypeScript

### خادم TypeScript - محمي

```typescript
import { Mppx } from 'mppx/server'
import { zcash } from 'zimppy-ts/server'

const mppx = Mppx.create({
  methods: [await zcash({ wallet: 'server' })],
  realm: 'my-api',
  secretKey: process.env.MPP_SECRET_KEY,
})

const result = await mppx.charge({
  amount: '42000',
  currency: 'zec',
})(request)

if (result.status === 402) return result.challenge

return result.withReceipt(Response.json({ data }))
```

**النقاط الرئيسية:**
- `zcash({ wallet: 'server' })` يحمّل المحفظة المحمية للخادم
- `mppx.charge()` يتولّى دورة حياة التحدي/التحقق الكاملة لـ 402
- `result.withReceipt()` يرفق إيصال الدفع التشفيري بالاستجابة

---

### خادم TypeScript - شفاف

```typescript
import { Mppx } from 'mppx/server'
import { zcashTransparent } from 'zimppy-ts/server'

const mppx = Mppx.create({
  methods: [await zcashTransparent({ wallet: 'server' })],
  // per-challenge T-address generated automatically (replay-safe)
})
```

يولّد كل تحدٍّ **عنوان T جديداً**، مما يجعل طلبات الدفع غير قابلة للربط عبر الجلسات.

---

### عميل TypeScript

```typescript
import { Mppx } from 'mppx/client'
import { zcash } from 'zimppy-ts/client'

const mppx = Mppx.create({ methods: [zcash({ wallet: 'default' })] })

// Session opened automatically; 402 is handled transparently
const res = await mppx.fetch('https://api.example.com/resource')
```

يعترض العميل استجابات `402`، ويفتح جلسة تلقائياً، ويعيد إرسال الطلب - ولا تتطلّب الشيفرة المستدعية أي منطق خاص بالدفع.

---

## دمج Zimppy - حزمة SDK لـ Rust

### خادم Rust (Axum)

```rust
use mpp::server::axum::*;
use zimppy_rs::ZcashChallenger;

struct Price;

impl ChargeConfig for Price {
    fn amount() -> &'static str { "42000" }
}

async fn handler(charge: MppCharge<Price>) -> WithReceipt<Json<Value>> {
    WithReceipt {
        receipt: charge.receipt,
        body: Json(data),
    }
}
```

**النقاط الرئيسية:**
- `MppCharge<Price>` مستخرِج Axum يتحقق من الدفع قبل تشغيل المعالج
- `WithReceipt` يغلّف الاستجابة بإيصال دفع تشفيري
- `ChargeConfig` يحدّد منطق التسعير - ويمكن أن يكون ديناميكياً بناءً على معاملات الطلب

---

### عميل Rust

```rust
use mpp::client::Fetch;
use zimppy_rs::ZcashPaymentProvider;

let provider = ZcashPaymentProvider::new(wallet_config, &rpc);

let resp = client
    .get("https://api.example.com/resource")
    .send_with_payment(&provider)
    .await?;
```

`send_with_payment` يوسّع أي عميل HTTP بمعالجة تلقائية لـ 402، وإدارة الجلسات، وإتمام الدفع بـ Zcash.

---

## مرجع CLI

| الأمر | الوصف |
|---|---|
| `npx zimppy wallet create` | توليد المفاتيح وعرض عبارة البذور |
| `npx zimppy wallet whoami` | عرض العنوان (UA + عنوان T)، والرصيد، والشبكة |
| `npx zimppy wallet balance --all` | تفصيل الرصيد لكل حساب |
| `npx zimppy wallet send <addr> <zat>` | إرسال ZEC محمي أو شفاف |
| `npx zimppy wallet transfer <from> <to> <zat>` | تحويل داخلي بين الحسابات |
| `npx zimppy wallet shield` | نقل الأموال الشفافة إلى Orchard (محمي) |
| `npx zimppy wallet use <name>` | تبديل هوية المحفظة النشطة |
| `npx zimppy request <url>` | طلب تلقائي 402 -> دفع -> إعادة المحاولة |

---

## الميزات الرئيسية

### محافظ أصيلة للوكلاء

صُمّمت محافظ Zimppy للاستخدام البرمجي من قِبل وكلاء الذكاء الاصطناعي - لا كإضافات متصفح يديرها البشر. تُدار المفاتيح عبر CLI أو حزم SDK، ويمكن تدوير الحسابات عبر **اشتقاق الحسابات وفق ZIP-32**، وتدعم المحفظة مسارات دفع مؤتمتة بالكامل دون موافقة بشرية لكل معاملة.

### دعم الوكلاء المتعددين

يمكن لعدة وكلاء العمل من المحفظة نفسها باستخدام **تدوير الحسابات وفق ZIP-32** - إذ يحصل كل وكيل على حسابه الخاص مع تتبّع معزول للرصيد، وإمكانية التحويل بين الحسابات، وتقارير الرصيد لكل حساب. وهذا يتيح إدارة أسطول من الوكلاء الكثيرين من بنية تحتية واحدة للمحفظة.

### معاملات Zcash محمية بالكامل (Orchard)

تستخدم المدفوعات المحمية **بروتوكول Orchard** الخاص بـ Zcash، وهو المجمّع (pool) المحمي الذي قدّمه NU5. يتحقق الخادم من المدفوعات باستخدام **Incoming Viewing Key (IVK)**، الذي يستطيع فك تشفير notes المستلمة دون كشف مفتاح الإنفاق. تُمنع هجمات إعادة التشغيل عبر **ربط المذكرة** - إذ يتضمّن كل تحدٍّ مذكرة `zimppy:{challenge_id}` فريدة يُتحقَّق منها تشفيرياً.

### الجلسات، زمن انتظار صفري لكل طلب

تفصل بنية الجلسات انتظار التأكيد على السلسلة عن زمن الانتظار لكل طلب. بعد إيداع واحد (نحو 25 ثانية منذ NU7)، تُخدَم جميع الطلبات اللاحقة بالرمز الحامل فوراً دون أي تفاعل مع سلسلة الكتل حتى إغلاق الجلسة.

### البث، الدفع لكل رمز

يتيح الدعم الأصيل لـ **SSE (الأحداث المرسلة من الخادم)** محتوى مُقاساً بنظام الدفع لكل رمز. وهو مثالي لواجهات API الخاصة باستدلال LLM حيث يكون طول المُخرَج متغيراً وينبغي أن تعكس الفوترة الاستهلاك الفعلي.

### التوافق مع المواصفات

- التحديات الموقّعة بـ **HMAC-SHA256** تمنع التزوير
- تنسيق أخطاء منظَّم وفق **RFC 9457** لمعالجة أخطاء قابلة للتشغيل البيني
- **`/.well-known/payment`** للاكتشاف التلقائي لطريقة الدفع من قِبل أي وكيل متوافق مع MPP

---

## البنية

```
crates/
  zimppy-core/       Zcash verification engine (Orchard decryption, replay protection)
  zimppy-wallet/     Native Zcash wallet (zingolib)
  zimppy-rs/         Rust SDK (ChargeMethod, SessionMethod, PaymentProvider, axum extractors)
  zimppy-napi/       Node.js native bindings (NAPI-RS)

packages/
  zimppy-ts/         TypeScript SDK (charge, session, SSE)
  zimppy-cli/        CLI with auto-pay and session management
```

### مسؤوليات المكوّنات

**`zimppy-core`** - النواة التشفيرية. تتولّى فك تشفير notes الخاصة بـ Orchard باستخدام IVK الخاص بالخادم، وتحليل المذكرات، ومنطق الحماية من إعادة التشغيل، والتحقق من التحديات. مكتوبة بـ Rust من أجل الأداء والصحة.

**`zimppy-wallet`** - محفظة Zcash أصيلة مدعومة بـ `zingolib`. تدير المفاتيح والحسابات والأرصدة المحمية/الشفافة وإرسال المعاملات.

**`zimppy-rs`** - حزمة SDK لـ Rust. توفّر سمات `ChargeMethod` و `SessionMethod` و `PaymentProvider`، إضافة إلى مستخرِجات Axum (`MppCharge`، `WithReceipt`) لدمج سهل الاستخدام في الخادم.

**`zimppy-napi`** - روابط NAPI-RS تعرض نواة Rust لـ Node.js، مما يتيح لحزمة SDK الخاصة بـ TypeScript استخدام المحرك التشفيري نفسه دون إعادة تنفيذ بدائيات Zcash بـ JavaScript.

**`zimppy-ts`** - حزمة SDK لـ TypeScript. تغلّف روابط NAPI بواجهات API اصطلاحية بنمط async/await لمسارات الدفع الفردي والجلسات وبث SSE.

**`zimppy-cli`** - المحفظة وأداة الطلبات عبر سطر الأوامر. تدعم الدفع التلقائي (402 -> دفع -> إعادة المحاولة)، وإدارة الجلسات، وجميع عمليات المحفظة.

---

## الأمثلة والعروض التوضيحية

| المثال | الوصف |
|---|---|
| `examples/fortune-teller/` | عروض توضيحية للدفع الفردي والجلسات والبث - خادم + عميل Rust |
| `examples/llm-summarizer/` | عرض توضيحي لبث LLM بنظام الدفع لكل رمز |
| `examples/mcp-server/` | خادم أدوات MCP مع أدوات ذكاء اصطناعي مدفوعة |
| `examples/ts-server/` | التنفيذ المرجعي لخادم MPP بـ TypeScript |

---

## ما الذي يتضمّنه - ملخص الميزات

| الميزة | الوصف |
|---|---|
| **الجلسات** | إيداع مرة واحدة، وطلبات فورية برمز حامل، واسترداد عند الإغلاق |
| **البث** | محتوى مُقاس بنظام الدفع لكل رمز عبر SSE |
| **الدفع الفردي** | دفعة محمية أو شفافة لكل طلب HTTP (مسار 402) |
| **المدفوعات الشفافة** | عناوين T مع منع إعادة التشغيل لكل تحدٍّ + أمر الحماية |
| **الحسابات المتعددة** | تدوير الحسابات وفق ZIP-32، والتحويلات بين الحسابات، والأرصدة لكل حساب |
| **محفظة CLI** | Send، shield، transfer، balance --all، whoami، الدفع التلقائي |
| **حزمتا SDK** | TypeScript و Rust |
| **متوافق مع المواصفات** | تحديات HMAC-SHA256، وأخطاء RFC 9457، واكتشاف عبر `/.well-known/payment` |

---

*لمزيد من المعلومات، زُر [zimppy.xyz](https://zimppy.xyz)*

---

## صفحات ذات صلة

- [المحافظ](/using-zcash/wallets) — محافظ Zcash التي تدعم المعاملات المحمية
- [المجمّعات المحمية](/using-zcash/shielded-pools) — كيف تحمي معاملات Orchard المحمية بيانات الدفع
- [معالجات الدفع](/using-zcash/payment-processors) — طرق أخرى لقبول مدفوعات Zcash
- [أصول Zcash المحمية](/zcash-tech/zcash-shielded-assets) — ZSAs ومستقبل قابلية البرمجة في Zcash
- [مشاريع المجتمع](/zcash-community/community-projects) — المزيد من مشاريع منظومة Zcash
