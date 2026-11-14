# كيفية تشغيل Zebra على Akash Network

دليل خطوة بخطوة لنشر عقدة كاملة (full node) Zebra Zcash على Akash.

### ما الذي ستنشره

عقدة (node) Zebra كاملة ستقوم بما يلي:

-> مزامنة سلسلة الكتل (blockchain) الخاصة بـ Zcash بالكامل (أكثر من 100GB لـ mainnet، ونحو 40GB لـ testnet)

-> تكلّف نحو $15 شهريًا حسب أسعار رمز (token) AKT

-> تستغرق من عدة ساعات إلى عدة أيام لإتمام المزامنة

-> تستخدم 4 vCPUs، و16GB RAM، و350GB تخزين (mainnet) أو 2 vCPUs، و8GB RAM، و50GB (testnet)

يثبّت قالب awesome-akash الآن الإصدار **Zebra 7.0.0** (وليس `:latest` أبدًا). ويفعّل Prometheus (`9999`) وفحص الصحة (`8080`) داخليًا، وJSON-RPC داخليًا لخدمة **lightwalletd** المستضافة معه على gRPC **9067**، وأعلام الميزات `ENABLE_METRICS` / `ENABLE_HEALTH` / `ENABLE_RPC` / `ENABLE_LIGHTWALLETD`.

إذا أردت عقدة Mainnet مقلّمة وأسرع، راجع [كيفية تشغيل Zakura على Akash](/guides/akash-network-zakura). وإذا أردت لوحة معلومات (dashboard) Regtest مؤقتة، راجع [هكذا تكلّم Zakura على Akash](/guides/thus-spoke-zakura-akash).


### كيف ينشر Akash المنافذ

الأرقام الموجودة في SDL (`port` / `as`) هي **منافذ الحاوية**. لا يزال Zebra يستمع على **8233**، وـ lightwalletd على **9067**، وفحص الصحة على **8080**. هذه الأرقام مخصّصة للعملية نفسها وللخدمات الأخرى في النشر نفسه (`zebra:8232`).

وهي **ليست** المنافذ الموجودة على عنوان (address) IP العام للمزوّد.

**الإعداد الافتراضي (هذا القالب):** لا يوجد IP مخصّص. لكل منفذ TCP مكشوف من نوع `global: true`، يربط المزوّد **منفذًا عاليًا عشوائيًا** على مضيف مشترك بمنفذ حاويتك. بعد بدء الإيجار (lease)، تعرض Console/Air في **Leases → URIs** نقاط النهاية الحقيقية، على سبيل المثال:

```
zebra-8233: provider.example:31482
lightwalletd-9067: provider.example:32011
```

يستخدم النظراء والمحافظ (wallet) قيم host:port **هذه**. انسخ URI الخاص بالمنفذ المكشوف الذي يهمّك (P2P أو lightwalletd). إذا ضبطت `ZEBRA_NETWORK__EXTERNAL_ADDR`، فاضبطه على URI الخاص بـ P2P المربوط (`provider.example:31482`)، وليس `…:8233`.

**HTTP** (`as: 80` مع `http_options`، المستخدم في لوحة معلومات Thus Spoke): ينشر كثير من المزوّدين **اسم مضيف ingress** (غالبًا على 80/443) بدلًا من منفذ TCP عشوائي. افتح URI الخاص بتلك الخدمة في متصفح.

**داخلي** (`global: false` أو `to: service: lightwalletd`): DNS الخاص بالعنقود فقط (`zebra:8232`). لا يوجد URI عام.

**عناوين IP المخصّصة** (`endpoints` / `kind: ip` في SDL) هي **إضافة مدفوعة**. يمكنها تثبيت IP عام، وأحيانًا تثبيت منفذ `as` على ذلك الـ IP. هذه القوالب **لا** تطلب واحدًا. أضف ذلك بنفسك لاحقًا إذا أردت `host:8233` ثابتًا؛ وتوقّع تكلفة إضافية وعددًا أقل من المزوّدين المتقدّمين بعروض.

### أي موقع تستخدم (المدفوعات)

SDL هو نفسه في الحالتين. يختلف موقعا الويب الخاصان بـ Akash في **طريقة الدفع**.

| الموقع | المدفوعات | المحفظة |
| --- | --- | --- |
| [console.akash.network](https://console.akash.network) | دفع تقليدي عبر الويب (البطاقة / قنوات العملات الورقية التي توفرها Console) | حساب Console |
| [air.akash.network](https://air.akash.network) | مدفوعات أصلية على سلسلة (chain) Akash (AKT / ACT) | **Keplr** |

استخدم **Air** عندما تريد الدفع بـ Keplr على السلسلة (on-chain). واستخدم **Console** عندما تريد طريقة دفع تقليدية عبر الويب. لا تتوقع تسوية أصلية عبر Keplr على Console.

### المتطلبات المسبقة

**إذا كنت تدفع على Console (الدفع عبر الويب)**

1. حساب Console على [console.akash.network](https://console.akash.network)
2. وسيلة دفع تقبلها واجهة Console
3. بضع دقائق في الواجهة

**إذا كنت تدفع على Air (الدفع الأصلي على Akash / Keplr)**

1. إضافة المتصفح **Keplr** (Chrome/Brave/Firefox)
2. **AKT / ACT** في Keplr للإيداع والإيجار (من منصة تداول (exchange) أو عبر IBC)
3. افتح [air.akash.network](https://air.akash.network)

#### الخطوة 1: الاتصال

**Console (مدفوعات الويب)**

-> انتقل إلى [https://console.akash.network](https://console.akash.network)

-> سجّل الدخول / أكمل خطوات إنشاء حساب Console

-> استخدم صفحة الدفع التي تعرضها الواجهة (وليس Keplr على السلسلة)

**Air (مدفوعات Akash الأصلية)**

-> انتقل إلى [https://air.akash.network](https://air.akash.network)

-> انقر على **«Connect Wallet»**

-> اختر **Keplr** ووافق على الاتصال

-> تأكد من أن رصيدك من AKT/ACT يكفي للإيداع وللإيجار

#### الخطوة 2: إنشاء النشر

-> انقر على **«Deploy»**

-> اختر **«Build your template»** (أو انتقل مباشرة إلى رفع SDL)


##### الخيار A: رفع ملف SDL (موصى به)

**مدفوعات الويب (Console)**

[![النشر على Akash Console](/content-images/deploy-with-akash-btn-74abb88d44.svg)](https://console.akash.network/new-deployment?step=edit-deployment&templateId=akash-network-awesome-akash-zcash-zebra)

**مدفوعات Akash الأصلية باستخدام Keplr (Air)**

[افتح هذا القالب على Air](https://air.akash.network/new-deployment?step=edit-deployment&templateId=akash-network-awesome-akash-zcash-zebra)

##### الخيار B: استخدام محرر SDL

إذا أردت لصق [ملف SDL](https://github.com/akash-network/awesome-akash/blob/master/zcash-zebra/deploy.yaml) يدويًا:

-> انسخ محتويات *zebra-akash.yml*

-> الصقها في محرر SDL

-> عدّلها حسب الحاجة (راجع قسم الإعدادات أدناه)

-> انقر على **«Create Deployment»**


#### الخطوة 3: مراجعة الإيداع والموافقة عليه

ستعرض لك الواجهة:

-> **إيداع النشر**: نحو 5 AKT (تستردّه عند إغلاق النشر)

-> **التكلفة المقدّرة**: بناءً على التسعير المحدّد في SDL الخاص بك

على **Air**، انقر على **«Approve»** ووقّع في **Keplr**. وعلى **Console**، أكمل الدفع عبر الويب الذي تعرضه الواجهة.

#### الخطوة 4: اختيار مزوّد

بعد نحو 30 ثانية، سترى عروضًا من المزوّدين. يعرض كل عرض:

-> **السعر لكل كتلة (block)** (بـ AKT أو USDC)

-> **التكلفة الشهرية المقدّرة**

-> **تفاصيل المزوّد** (مدة التشغيل، المنطقة، إلخ)


**لا تختر الأرخص فحسب.** تحقّق من:

-> نسبة مدة التشغيل % (استهدف أكثر من 95%)

-> المنطقة (كلما كان أقرب إليك كان زمن الاستجابة أفضل، لكن ذلك لا يهم كثيرًا لعقد سلسلة الكتل)

-> حالة التدقيق (علامة صح خضراء = موثوقية أعلى)


انقر على **«Accept Bid»** لدى المزوّد الذي اخترته. على Air، وقّع في Keplr. وعلى Console، أكّد عبر خطوات الدفع على الويب.

#### الخطوة 5: انتظار اكتمال النشر

ستقوم Console بما يلي:

-> إنشاء الإيجار مع المزوّد الذي اخترته

-> إرسال البيان (manifest) (الذي يخبر المزوّد بما يجب تشغيله)

-> تشغيل حاويتك

يستغرق ذلك 1-2 دقيقة. سترى تحديثات الحالة في الواجهة.

#### الخطوة 6: التحقق من أن العقدة تعمل

بعد اكتمال النشر، سترى:

-> علامة التبويب **Services**: تعرض خدمة *zebra* وحالتها

-> علامة التبويب **Logs**: سجلات الحاوية المباشرة

-> علامة التبويب **Leases**: تفاصيل النشر (DSEQ، المزوّد، التكلفة)


##### فحص السجلات

انقر على **Logs**، ويُفترض أن ترى Zebra وهو يبدأ التشغيل:

```bash
Loading config from environment variables
Mainnet network selected
Listening for peer connections on [::]:8233
Starting initial sync...
```

ستستغرق المزامنة **من ساعات إلى أيام** حسب الشبكة. راقب ما يلي:

-> ارتفاع الكتل بشكل متزايد

-> الاتصالات بالنظراء (ينبغي أن تكون بين 10 و30 نظيرًا)

-> عدم تكرار الأخطاء


#### الخطوة 7: الحصول على عنوان عقدتك

انقر على علامة التبويب **Leases**، ثم **URIs**.

سترى شيئًا مثل:

```
zebra-8233: provider-hostname.com:31482
lightwalletd-9067: provider-hostname.com:32011
```

كل سطر يمثّل منفذًا مكشوفًا واحدًا من نوع `global: true`. يتضمن الاسم منفذ `as` المحدّد في SDL (8233، 9067). الرقم الذي يلي اسم المضيف هو **المنفذ الذي ربطه المزوّد**، ويُخصَّص عند بدء الإيجار. تتصل عقد Zcash الأخرى بـ `provider-hostname.com:31482`. وتستخدم المحافظ المدمجة URI الخاص بـ lightwalletd (مع إيقاف TLS ما لم تُنهِ TLS بنفسك).

لا تزال الحاوية تستمع على 8233 / 9067. العنوان العام هو دائمًا URI، ما لم تُضف لاحقًا IP مخصّصًا (غير موجود في هذا القالب).

### خيارات الإعداد

#### التبديل إلى Testnet

يستخدم SDL شبكة Mainnet افتراضيًا. لاستخدام Testnet بدلًا منها:

-> **علّق إعدادات Mainnet** في قسم *env*:

   ```yaml
   # - "ZEBRA_NETWORK__NETWORK=Mainnet"
   # - "ZEBRA_NETWORK__LISTEN_ADDR=[::]:8233"
   ```

-> **ألغِ تعليق إعدادات Testnet**:

   ```yaml
   - "ZEBRA_NETWORK__NETWORK=Testnet"
   - "ZEBRA_NETWORK__LISTEN_ADDR=[::]:18233"
   ```

-> **حدّث المنفذ المكشوف** في قسم *expose*:

   ```yaml
   # Comment out Mainnet port:
   # - port: 8233
   #   as: 8233
   #   to:
   #     - global: true
   #   proto: tcp

   # Uncomment Testnet port:
   - port: 18233
     as: 18233
     to:
       - global: true
     proto: tcp
   ```

-> **اختياري: قلّل الموارد** لـ Testnet في *profiles.compute.zebra.resources*:

   ```yaml
   cpu:
     units: 2  # Down from 4
   memory:
     size: 8Gi  # Down from 16Gi
   storage:
     - size: 50Gi  # Down from 150Gi
   ```

-> **اختياري: خفّض التسعير** في *profiles.placement.akash.pricing*:

   ```yaml
   amount: 5000  # Down from 10000
   ```

#### تفعيل الوصول إلى RPC

يكون RPC معطّلًا افتراضيًا لأسباب أمنية. لتفعيله:

**لـ Mainnet:**

-> ألغِ التعليق في قسم *env*:

   ```yaml
   - "ZEBRA_RPC__LISTEN_ADDR=0.0.0.0:8232"
   - "ZEBRA_RPC__COOKIE_DIR=/home/zebra/.cache/zebra"
   ```

-> ألغِ تعليق منفذ RPC الخاص بـ Mainnet في *expose*:

   ```yaml
   - port: 8232
     as: 8232
     to:
       - global: false  # Keep internal for security
     proto: tcp
   ```

**لـ Testnet:**

-> ألغِ التعليق في قسم *env*:

   ```yaml
   - "ZEBRA_RPC__LISTEN_ADDR=0.0.0.0:18232"
   - "ZEBRA_RPC__COOKIE_DIR=/home/zebra/.cache/zebra"
   ```

-> ألغِ تعليق منفذ RPC الخاص بـ Testnet في *expose*:

   ```yaml
   - port: 18232
     as: 18232
     to:
       - global: false
     proto: tcp
   ```

**تحذير**: إذا ضبطت *global: true* لـ RPC، فأنت تكشفه للإنترنت. يستخدم Zebra المصادقة عبر ملفات تعريف الارتباط افتراضيًا، ومع ذلك، لا تفعل هذا ما لم تكن تعرف ما تفعله.

إذا كشفت RPC باستخدام `global: true`، فسيظل العنوان العام هو **URI الإيجار** (`host:&lt;random-high-port&gt;`)، وليس `host:8232`. ومع `global: false` (هذا القالب)، يبقى RPC على شبكة النشر (`zebra:8232` لـ lightwalletd). إيجارات IP المخصّص خيار مدفوع منفصل، وليست موجودة في SDL.

#### تفعيل المقاييس (Prometheus)

لجمع المقاييس لأغراض المراقبة:

-> ألغِ التعليق في *env*:

   ```yaml
   - "ZEBRA_METRICS__ENDPOINT_ADDR=0.0.0.0:9999"
   ```

-> ألغِ تعليق منفذ المقاييس في *expose*:

   ```yaml
   - port: 9999
     as: 9999
     to:
       - global: false
     proto: tcp
   ```

#### ضبط الموارد/التسعير

إذا لم تكن تتلقى عروضًا أو أردت تحسين التكلفة:

**للمزوّدين ذوي المواصفات الأقل**، قلّل القيم في قسم *profiles.compute.zebra.resources*:

-> CPU: *units: 2* (الحد الأدنى لسرعة مزامنة معقولة)

-> الذاكرة: *size: 12Gi* (الحد الأدنى للاستقرار)

-> التخزين: *size: 120Gi* (الحد الأدنى لـ mainnet)

**لجذب مزيد من العروض**، زِد القيمة في *profiles.placement.akash.pricing*:

-> Mainnet: جرّب *amount: 1000000* uakt/block

-> Testnet: جرّب *amount: 1000000* uakt/block

### تحديث النشر

هل تحتاج إلى تغيير الإعدادات بعد النشر؟

-> انتقل إلى **My Deployments** في Console

-> ابحث عن نشر Zebra الخاص بك

-> انقر على **«Update Deployment»**

-> عدّل SDL

-> انقر على **«Update»** (Air: وقّع في Keplr؛ Console: أكّد عبر خطوات الدفع على الويب)

**تنبيه**: سيؤدي التحديث إلى إعادة تشغيل حاويتك. ستستأنف العقدة العمل من حالتها المحفوظة (التخزين الدائم)، لكن توقّع توقفًا لمدة 1-2 دقيقة.

### المراقبة

#### عبر Console

-> **علامة التبويب Logs**: سجلات الحاوية المباشرة

-> **علامة التبويب Shell**: فتح صدفة أوامر داخل الحاوية (مفيدة لتصحيح الأخطاء)

-> **علامة التبويب Events**: أحداث Kubernetes (عديمة الفائدة غالبًا ما لم يتعطّل شيء)


#### عبر RPC (إذا كان مفعّلًا)

إذا فعّلت RPC، يمكنك الاستعلام من عقدتك كما تفعل مع أي عقدة zebrad كاملة عادية (لأنها كذلك فعلًا!)

### إغلاق النشر

عندما تنتهي أو تريد التوقف عن الدفع:

-> انتقل إلى **My Deployments**

-> ابحث عن نشر Zebra الخاص بك

-> انقر على **«Close Deployment»**

-> أكّد (Air: وقّع في Keplr؛ Console: أكّد في واجهة Console)

سيُعاد إليك إيداعك البالغ 5 AKT. يُفترض أن يحتفظ المزوّد بـ **التخزين الدائم**، لكن لا تعتمد على ذلك، وتعامل معه كما تتعامل مع أي مزوّد سحابي آخر.

### استكشاف الأخطاء وإصلاحها

#### خطأ «Insufficient funds»

على Air، موّل Keplr بـ AKT/ACT. وعلى Console، اشحن (top up) وسيلة الدفع في Console.

#### لا تظهر أي عروض

السبب أحد ما يلي:

-> تسعيرك منخفض جدًا (زِد قيمة *amount* في SDL)

-> متطلبات الموارد لديك مرتفعة جدًا بالنسبة للمزوّدين المتاحين (قلّل CPU/الذاكرة/التخزين)

-> انتظر وقتًا أطول (يستغرق ظهور العروض أحيانًا 60-90 ثانية)


#### النشر عالق في حالة «pending»

قد يواجه المزوّد مشكلات. أغلق النشر وجرّب مزوّدًا آخر.

#### سجلات Zebra تعرض «No peers connected»

هذا طبيعي في الدقائق الأولى. سيكتشف Zebra النظراء تلقائيًا. إذا استمرت المشكلة أكثر من 10 دقائق، فقد تكون لديك مشكلة في الشبكة (وهذا غير مرجّح على Akash).

#### أخطاء «Out of memory» في السجلات

لقد بخلت على ذاكرة RAM. أغلق النشر وأعد النشر بذاكرة لا تقل عن 12Gi (يوصى بـ 16Gi).

#### المزامنة تستغرق وقتًا لا ينتهي

ماذا تقصد بـ «لا ينتهي»؟

-> **ساعات**: أمر طبيعي

-> **أيام**: طبيعي أيضًا لـ mainnet عند البدء من الصفر

-> **أسابيع**: هناك خطأ ما، فافحص السجلات بحثًا عن الأخطاء


### إدارة التكلفة

راقب إنفاقك في Console أو Air:

-> **My Deployments** -> نشرك -> يعرض تقدير «Cost per month»

-> Air: ينخفض رصيدك من AKT/ACT في Keplr بمرور الوقت. Console: ينخفض رصيد الدفع في Console.


عندما ينخفض رصيدك كثيرًا، سيغلق Akash نشرك تلقائيًا. **اشحن محفظتك بشكل دوري** أو اضبط تنبيهات.

#### خفض التكاليف

-> **استخدم Testnet** للاختبارات غير الإنتاجية (أرخص بنسبة 50%)

-> **قلّل CPU/الذاكرة** إذا لم تكن بحاجة إلى مزامنة سريعة

-> **اختر مزوّدين أرخص** (ليس قرارًا حكيمًا دائمًا، فمدة التشغيل مهمة)


### Mainnet مقابل Testnet

```markdown
----------------------------------------------------------------------------------
|            | Mainnet (default)               | Testnet                         |
---------------------------------------------------------------------------------|
| Purpose   | Production Zcash blockchain      | Testing and development         |
| Network   | ZEBRA_NETWORK__NETWORK=Mainnet   | ZEBRA_NETWORK__NETWORK=Testnet  |
| P2P Port  | 8233                             | 18233                           |
| RPC Port  | 8232                             | 18232                           |
| Sync time | Days                             | Hours                           |
| Storage   | 350GB+                           | 50GB                            |
| Resources | 4 CPU / 16GB RAM                 | 2 CPU / 8GB RAM                 |
| Cost      | ~$15/month                       | ~$5/month                       |
----------------------------------------------------------------------------------
```

ابدأ بـ Testnet إذا كنت تختبر عملية النشر فقط. راجع قسم «التبديل إلى Testnet» أعلاه لمعرفة الإعدادات.

### موارد إضافية

**Akash Console** (مدفوعات الويب): [https://console.akash.network](https://console.akash.network)

**Akash Air** (AKT/ACT الأصلي + Keplr): [https://air.akash.network](https://air.akash.network)

**وثائق Akash**: [https://akash.network/docs/](https://akash.network/docs/)

**وثائق Zebra**: [https://zebra.zfnd.org/](https://zebra.zfnd.org/)

**مستكشفات Zcash**: [https://zechub.wiki/guides/blockchain-explorers](https://zechub.wiki/guides/blockchain-explorers)

**Discord الخاص بـ Akash**: [https://discord.akash.network](https://discord.akash.network) (لمشكلات المزوّدين)
