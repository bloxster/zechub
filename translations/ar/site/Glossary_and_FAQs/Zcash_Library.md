# مكتبة Zcash

مسرد شامل للمصطلحات والمفاهيم والموارد الرئيسية المتعلقة بـ Zcash. هذه هي المكتبة الوحيدة للبحث عن المصطلحات الواردة في ملخصات الإصدارات أو نقاشات المنتدى أو سجلات تغييرات المحافظ (wallets).

### التنقل السريع
[A](#a) | [B](#b) | [C](#c) | [D](#d) | [E](#e) | [F](#f) | [G](#g) | [H](#h) | [I](#i) | [J](#j) | [K](#k) | [L](#l) | [M](#m) | [N](#n) | [O](#o) | [P](#p) | [Q](#q) | [R](#r) | [S](#s) | [T](#t) | [U](#u) | [V](#v) | [W](#w) | [X](#x) | [Y](#y) | [Z](#z)

---

## A

| المصطلح | التعريف |
|------|-----------|
| Actions | بدلًا من إنشاء عدة إثباتات (proofs) فردية لكل عملية إنفاق (Spend) وعملية إخراج (Output)، يدمجها بروتوكول Orchard في إجراء (Action) واحد. |
| Addresses | لدى Zcash عناوين محمية (shielded، zaddr) وعناوين شفافة (transparent، taddr). تجمع العناوين الموحدة (UA) عدة أنواع من المستقبِلات، بحيث تستطيع المحفظة المرسِلة اختيار أفضل نوع يدعمه الطرفان. وهي محددة في [ZIP 316](https://zips.z.cash/zip-0316). [العناوين الموحدة](/zcash-tech/unified-addresses) |
| Arborist Call | مكالمة تُعقد كل أسبوعين وتتناول آخر مستجدات تطوير بروتوكول Zcash والأبحاث المتعلقة به. تُستضاف على منتدى مجتمع Zcash وعلى Discord. [محاضر الاجتماعات](https://github.com/ZcashCommunityGrants/arboretum-notes) / [إعلانات المنتدى](https://forum.zcashcommunity.com) |
| Auto-shielding | تتيح للمستخدمين (وبالتحديد لمحافظهم) نقل الأموال تلقائيًا من عنوان شفاف إلى أحدث مجمّع (pool) محمي في ZEC. |

## B

| المصطلح | التعريف |
|------|-----------|
| Benchmarking | يمكن للمعدنين (miners) تقديم مقاييس حول كفاءة مختلف الأجهزة المستخدمة في تعدين Zcash. [اطّلع عليها هنا](https://zcashbenchmarks.info) |
| Block | الكتلة سجل في سلسلة الكتل (blockchain) الخاصة بـ Zcash، يحتوي على مجموعة من المعاملات (transactions) المرسلة على الشبكة. تُضاف كتلة جديدة إلى سلسلة الكتل كل 25 ثانية تقريبًا في المتوسط (كانت المدة 75 ثانية قبل تفعيل NU7 في نوفمبر 2026). |
| Block Explorer | أداة على الإنترنت لعرض جميع المعاملات، السابقة منها والحالية، على سلسلة الكتل. [مستكشف كتل Zcash](https://zcashexplorer.app/) |
| Blogs | [مدونة ZODL (سابقًا Electric Coin Co)](https://zodl.com/blog/) / [مدونة Zcash Foundation](https://zfnd.org/blog/) / [النشرة الإخبارية لـ ZecHub](https://zechub.substack.com/) |
| Blossom | ترقية الشبكة (network upgrade) الرئيسية الثالثة لـ Zcash. [مزيد من المعلومات](https://zcash.readthedocs.io/en/latest/rtd_pages/nu_dev_guide.html?highlight=orchard#blossom) |

## C

| المصطلح | التعريف |
|------|-----------|
| Canopy | ترقية الشبكة الرئيسية الخامسة لـ Zcash. [مزيد من المعلومات](https://zcash.readthedocs.io/en/latest/rtd_pages/nu_dev_guide.html?highlight=orchard#canopy) |
| Commitment Scheme | يتيح للملتزِم أن يلتزم بكثيرة حدود باستخدام قيمة قصيرة، يستطيع المتحقِّق استخدامها لتأكيد صحة التقييمات المُدّعاة لكثيرة الحدود الملتزَم بها. وهو مفيد لتقليل تكاليف الاتصال في بروتوكول Zcash. |
| Community | [منتدى مجتمع Zcash الرسمي](https://forum.zcashcommunity.com) / [مجتمع Zcash على Discord](https://discord.com/channels/669694001464737815/669694001921654794) / [البحث والتطوير في Zcash على Discord](https://discord.com/invite/6AK7keWFaK) / [Reddit](https://www.reddit.com/r/zec/) / [Telegram](https://t.me/Zcash_Community) / [Twitter](https://x.com/zcash) |
| Crosslink | تصميم إجماع (consensus) هجين مقترح يُشغِّل إثبات الحصة (proof of stake) إلى جانب إثبات العمل (proof of work) الحالي في Zcash. تقود Shielded Labs هذا العمل وتحتفظ بنموذج أولي في مستودع `zebra-crosslink`. Crosslink ليس جزءًا من Zcash حاليًا. [بروتوكول Crosslink](/zcash-tech/crosslink-protocol) |
| CrossPay | ميزة في محفظة ZODL تتيح لك إنفاق ZEC المحمي بينما يتلقى المستلم الدفع بالأصل والسلسلة (chain) اللذين يفضلهما، ويمر ذلك عبر NEAR Intents بدلًا من منصة تداول مركزية. |
| Cypherpunk Zero | عالم إبداعي وجهد تعاوني بين ECC والرسام Stranger Wolf وـ Mighty Jaxx وشركاء مختارين من المنظومة. [Cypherpunk Zero على وسائل التواصل الاجتماعي](https://x.com/cypherpunkZero) / [مجموعة OpenSea](https://opensea.io/collection/cypherpunk-zero) |

## D

| المصطلح | التعريف |
|------|-----------|
| DeFi | مشاريع تدمج ZEC مع DeFi: [Maya Protocol](https://www.mayaprotocol.com/ecosystem#user-interfaces/) / [NEAR Intents](https://near-intents.org/) / [ZenRock](https://app.zenrocklabs.io/) / [ShapeShift](https://app.shapeshift.com/) / [LeoDex](https://leodex.io/) / [THORSwap](https://app.thorswap.finance/) |
| Deshielding | يشير إلى إرسال معاملة من zaddr (عنوان محمي) إلى taddr (عنوان شفاف). لا يكون مصدر المعاملة مرئيًا، لكن الأموال تدخل مجمّع قيمة مرئيًا للعموم. |
| Developer Resources | [موارد المطورين](https://www.zcashcommunity.com/developers/) |
| Documentation | [الوثائق الرسمية](https://zcash.readthedocs.io/en/latest/) |

## E

| المصطلح | التعريف |
|------|-----------|
| ECC | Electric Coin Company هي الفريق الذي أطلق بروتوكول Zcash، وكانت تُعرف سابقًا باسم شركة Zcash. استقال فريقها الهندسي بالكامل في يناير 2026 إثر نزاع حول الحوكمة مع مجلس إدارة Bootstrap، ثم أسس أعضاؤه ZODL. |
| ECDSA | خوارزمية التوقيع الرقمي بالمنحنى الإهليلجي (Elliptic Curve Digital Signature Algorithm) هي مخطط توقيع رقمي (signature) آمن تشفيريًا. تعتمد خوارزمية التوقيع/التحقق في ECDSA على ضرب النقاط على المنحنى الإهليلجي. |
| Education | مقاطع فيديو تعليمية تشرح Zcash [هنا](https://www.zcashcommunity.com/zcash-education/) |
| Encrypted Memos | حقل إضافي في المعاملات المرسلة إلى العناوين المحمية، يكون مرئيًا لمستلم الدفع. المذكرة (memo) المشفرة مرئية للمرسل والمستلم فقط. |
| Equihash | خوارزمية تعدين بإثبات العمل تعتمد على الذاكرة، وتُستخدم في Zcash. |
| Events | يمكنك الاطلاع على تقويم الفعاليات المتعلقة بـ Zcash على [Luma](https://luma.com/zcash) وعلى [Zcash Foundation](https://zfnd.org/zf-events/) |
| Exchanges | [قائمة منصات التداول التي تدعم Zcash](https://z.cash/exchanges/) |

## F

| المصطلح | التعريف |
|------|-----------|
| Fiat-Shamir | تقنية تأخذ إثبات معرفة تفاعليًا وتُنشئ توقيعًا رقميًا بناءً عليه. وبهذه الطريقة يمكن إثبات حقيقة ما (مثل معرفة سرّ) علنًا دون الكشف عن المعلومات الكامنة وراءها. |
| Formal Verification | إثبات أن نظامًا ما يتصرف رياضيًا كما هو محدد تمامًا، بدلًا من الاعتماد على الاختبار وحده. جرى التحقق من دائرة Action في Ironwood بهذه الطريقة على يد مساهمين من zkSecurity ومن ZODL باستخدام مُثبِت المبرهنات Lean، لإثبات خلوّها من أخطاء السلامة (soundness). |
| Founders Reward | تمثل مكافأة المؤسسين 20 بالمئة من إجمالي مكافأة الكتلة، وتُقتطع من قيمة كل كتلة وتُوزَّع بشفافية لدفع تطوير البروتوكول ونموه. |
| Free2Z | أداة للمحتوى المجهول والتبرعات الخاصة تعمل بـ Zcash. [Free2Z](https://free2z.com) |
| FROST | مخطط توقيع عتبي من نوع Schnorr، مرن ومُحسَّن لتقليل عدد الجولات. [ورقة بحثية](https://eprint.iacr.org/2020/852) |

## G

| المصطلح | التعريف |
|------|-----------|
| Governance | تُدوَّن القرارات الناتجة عن عملية ZIP في مواصفات Zcash، وكذلك في البرمجيات التي تُشغِّل الشبكة. تُعتمد التغييرات على السلسلة (on-chain) عندما تتبنى غالبية الشبكة الترقية دون كسر الإجماع. [التاريخ الكامل للبروتوكول](https://zfnd.org/protocol-agreements-and-major-decisions/) |

## H

| المصطلح | التعريف |
|------|-----------|
| Halo | يتيح ترقية الدوائر دون الحاجة إلى إعداد موثوق (trusted setup)، مما يجعل البروتوكول المحمي في Zcash أكثر مرونة للتحسينات والتوسعات المستقبلية. [شرح تقني](https://z.cash/learn/what-is-halo-for-zcash/) |
| HD Wallet | تولّد المحافظ الهرمية الحتمية مجموعة متتالية من أزواج المفاتيح من بذرة واحدة، مما يوفر سهولة الاستخدام والإدارة إلى جانب مستوى عالٍ من الأمان. |
| Heartwood | ترقية الشبكة الرئيسية الرابعة لـ Zcash. [مزيد من المعلومات](https://z.cash/upgrade/heartwood/) |

## I

| المصطلح | التعريف |
|------|-----------|
| Index | يمثل مؤشر ZCX التابع لـ CoinDesk سعرًا فوريًا لحظيًا لـ Zcash مقومًا بما يعادله بالدولار الأمريكي. [مؤشر الأسعار](https://www.coindesk.com/indices/zcx/) |
| Integrations | يمكنك قبول مدفوعات Zcash عبر عدد من مزودي الخدمة الخارجيين. [معالجات الدفع](https://z.cash/zcash-for-business/) |
| Interactive Proof System | آلة مجردة تُنمذج الحوسبة على أنها تبادل للرسائل بين طرفين: المُثبِت (Prover) والمتحقِّق (Verifier). |
| Investment | تتوفر عدة خيارات مالية للمستثمرين المؤسسيين أو المكاتب العائلية الراغبين في الانكشاف على Zcash. [القائمة الكاملة](https://z.cash/investors/) |
| Ironwood | ترقية الشبكة NU6.3، مفعّلة على الشبكة الرئيسية منذ الكتلة 3,428,143 في 28 يوليو 2026. تضيف Ironwood مجمّعًا محميًا جديدًا مبنيًا على دائرة Orchard المصحّحة، وتُغلق مجمّع Orchard الأصلي، بحيث يستطيع أي شخص التحقق من نزاهة المعروض المتداول. قواعد الإجماع الخاصة بها محددة في [ZIP 258](https://zips.z.cash/zip-0258). [Ironwood](/zcash-tech/ironwood) |

## J

| المصطلح | التعريف |
|------|-----------|
| JubJub | منحنى إهليلجي مُصمَّم ليكون قابلًا للتنفيذ بكفاءة في دوائر zk-SNARK. |

## K

| المصطلح | التعريف |
|------|-----------|
| Keystone Wallet | محفظة مادية (hardware wallet) معزولة عن الشبكة تدعم Zcash دعمًا أصليًا (المحمي عبر Orchard)، ومتوافقة مع ZODL للتوقيع البارد. [Keystone](https://keyst.one) |

## L

| المصطلح | التعريف |
|------|-----------|
| Layer-1 | تشير إلى الشبكة الأساسية وبنيتها التحتية. يمكن لسلاسل الكتل من الطبقة الأولى التحقق من المعاملات وتثبيتها نهائيًا دون الحاجة إلى شبكة أخرى. Zcash هي سلسلة كتل من الطبقة الأولى (L1). |
| librustzcash | مساحة عمل بلغة Rust تحتوي على جميع الحزم (crates) والاعتماديات اللازمة للعمل مع Zcash. [المستودع](https://github.com/zcash/librustzcash) |
| lightwalletd | خادم عديم الحالة يزوّد العملاء الخفيفين (light clients) بمعلومات سلسلة الكتل. [lightwalletd](https://zcash.readthedocs.io/en/latest/rtd_pages/lightclient_support.html) |

## M

| المصطلح | التعريف |
|------|-----------|
| Metrics | تتوفر مقاييس الشبكة [هنا](https://tokenterminal.com/explorer/projects/zcash/metrics/all) |
| Metadata | بيانات تُولَّد إلى جانب معاملة Zcash الخاصة بالمستخدم. وقد تشمل ارتفاع الكتلة أو إصدار المعاملة أو ارتفاع انتهاء الصلاحية إلخ |
| Mobile SDK | حزمة SDK خفيفة تربط Android بـ Zcash، مما يتيح لتطبيقات Android الخارجية إرسال المعاملات المحمية واستقبالها. [GitHub](https://github.com/zcash/zcash-android-wallet-sdk) |
| Mining | العملية التي تتنافس فيها العقد (nodes) في شبكة Zcash، عند كل كتلة، عبر إجراء حسابات رياضية معقدة لإيجاد حل وفقًا لصعوبة ذاتية التعديل. [دليل](https://z.cash/mining-zcash/) |
| Multisignature | عنوان يتطلب تواقيع عدة مفاتيح خاصة لإنفاق الأموال. حاليًا، لا تدعم وظيفة التوقيع المتعدد (multisig) سوى العناوين الشفافة. |

## N

| المصطلح | التعريف |
|------|-----------|
| Network Sustainability Mechanism (NSM) | مقترح من Shielded Labs لحرق حصة من رسوم (fees) المعاملات، بحيث لا تعتمد ميزانية أمن البروتوكول على المدى الطويل اعتمادًا كليًا على الإصدار. وهو محدد في ZIP 234، وقيد المراجعة في 2026. |
| Nighthawk | محفظة Zcash سابقة للهاتف المحمول من Nighthawk Apps. لم تعد محافظ Zcash الخاصة بها تحظى بالصيانة، وقد بُنيت Nighthawk Wallet 3.0 لـ DarkFi بدلًا من ذلك. لنقل الأموال من بذرة Nighthawk قديمة، راجع [استعادة الأموال](/using-zcash/recovering-funds). |
| Noir Wallet | محفظة على شكل إضافة متصفح لـ Zcash بدعم من Zcash Community Grants، صُممت لربط ZEC المحمي مباشرةً بتطبيقات المتصفح بدلًا من الاعتماد على أكواد QR والتحويلات اليدوية. [zknoir.com](https://www.zknoir.com/) |
| NU5 | ترقية الشبكة الرئيسية السادسة لـ Zcash، وقد قدّمت مجمّع Orchard المحمي والعناوين الموحدة (Unified Addresses). [مزيد من المعلومات](https://zcash.readthedocs.io/en/latest/rtd_pages/nu_dev_guide.html#nu5) |
| NU6 | ترقية الشبكة الرئيسية السابعة لـ Zcash، وقد عدّلت إعانة الكتلة (block subsidy) لتمويل برنامج Zcash Community Grants وصندوق التطوير المؤجل المقفل (Deferred Dev Fund Lockbox). فُعّلت في أواخر 2024. [مزيد من المعلومات](https://zcash.readthedocs.io/en/latest/rtd_pages/nu_dev_guide.html#nu6) |
| NU7 | ترقية الشبكة التي تلي Ironwood، فُعّلت على الشبكة الرئيسية عند الارتفاع 3,543,405 في 6 نوفمبر 2026 (UTC). وفقًا لـ [ZIP 259](https://zips.z.cash/zip-0259)، تجلب هذه الترقية كتلًا مدتها 25 ثانية (ZIP 218)، وآلية استدامة الشبكة مع إعادة تدوير الرسوم (ZIPs 235 و237)، وإنهاء معاملات الإصدار 4، مما يجعل أموال Sprout غير قابلة للإنفاق (ZIP 2003). لا تضيف أي تنسيق جديد للمعاملات؛ كما أن Tachyon والأصول المحمية في Zcash ليست جزءًا منها. |

## O

| المصطلح | التعريف |
|------|-----------|
| Oblivious Synchronization | طريقة قيد التطوير ضمن مشروع Tachyon تتيح للمحفظة طلب البيانات التي تحتاجها من خادم غير موثوق دون الكشف عن الـ notes التي تستفسر عنها. لا يعرف الخادم أبدًا قيم مانع التكرار (nullifiers) الخاصة بك، لأن البروتوكول يجعلها تتطور بطريقة غير قابلة للربط (unlinkable). [شرح مكتوب](https://seanbowe.com/blog/tachyon-scaling-zcash-oblivious-synchronization/) |
| Orchard | المجمّع المحمي الذي قُدِّم في NU5. أغلقته Ironwood بعد اكتشاف خطأ في السلامة، لذا لم يعد بإمكانك الإرسال أو الاستقبال داخل Orchard. لا تخرج الأموال منه إلا عبر البوابة الدوارة (turnstile). [المجمّعات المحمية](/using-zcash/shielded-pools) |
| Overwinter | ترقية الشبكة الأولى لـ Zcash. [مزيد من المعلومات](https://zcash.readthedocs.io/en/latest/rtd_pages/nu_dev_guide.html#overwinter) |

## P

| المصطلح | التعريف |
|------|-----------|
| Payments | يمكنك استخدام Zcash في المشتريات اليومية عبر عدد من مزودي خدمات الدفع المختلفين. [تطبيقات الدفع](https://z.cash/pay-with-zcash/) |
| PCD (Proof-Carrying Data) | بدائية تنتقل فيها البيانات مصحوبة بإثبات على صحتها، بحيث يؤدي دمج البيانات إلى دمج الإثباتات أيضًا. يعيد مشروع Tachyon بناء البروتوكول المحمي حول PCD، مما يتيح لكل محفظة أن تحمل إثباتًا تكراريًا على صحة رصيدها بدلًا من إعادة فحص السلسلة. التطبيق المكتوب بلغة Zcash هو [Ragu](https://github.com/tachyon-zcash/ragu)، الذي يتبع Halo ولا يحتاج إلى إعداد موثوق. |
| Peer-to-Peer Network | تقوم شبكات الند للند (P2P) على مفهوم اللامركزية، وهي البنية الأساسية لتقنية سلسلة الكتل. |
| PIR (Private Information Retrieval) | تقنيات تتيح لك جلب سجل من خادم دون أن يعرف الخادم أي سجل طلبته. وهي قيد البحث النشط لـ Zcash كوسيلة تتيح للمحافظ الخفيفة استرجاع ما تحتاجه دون تسريب ما تبحث عنه. |
| Podcast | [Radiolab (مراسم Zcash)](https://archive.org/details/radiolab_podcast17crypto_zcash_ceremony) / [RealVisionFinance](https://www.youtube.com/watch?v=ibA_4kwd_YI) / [EthDenver](https://www.youtube.com/watch?v=t62isi58XcQ) / [UpOnlyPodcast](https://www.youtube.com/watch?v=AjC9T938o3Q) / [Zcast en Español](https://www.youtube.com/@ZcastEsp) |

## Q

| المصطلح | التعريف |
|------|-----------|
| QR Code | رمز قابل للقراءة آليًا يُستخدم لترميز عناوين Zcash لتسهيل مسحها ضوئيًا. تُشارَك العناوين الموحدة (UAs) عادةً عبر رموز QR في محافظ Zcash الحديثة. |
| Quantum Recoverability | خاصية في notes Ironwood، محددة في [ZIP 2005](https://zips.z.cash/zip-2005)، تُبقي سجل العملة على السلسلة قابلًا للاسترداد إذا تمكّن حاسوب كمومي في المستقبل من كسر التشفير الذي يحميها اليوم. وهي مسار للاسترداد وليست مقاومة كمومية، وتنطبق على notes Ironwood، لا على الأموال الموجودة في Sprout أو Sapling أو Orchard. |

## R

| المصطلح | التعريف |
|------|-----------|
| Recovery Phrase | تسلسل من 12 أو 24 حرفًا ورقمًا يُستخدم لنسخ المحفظة احتياطيًا واستعادتها. في Zcash، تعيد هذه العبارة توليد مفاتيح الإنفاق ومفاتيح العرض، مما يجعلها ضرورية لاسترداد الأموال وللأمان. |

## S

| المصطلح | التعريف |
|------|-----------|
| Sapling | إحدى أهم عمليات ترقية الشبكة (network upgrade)، إذ قدّمت تحسينات كبيرة في كفاءة المعاملات المحمية ومهّدت الطريق لاعتمادها على الأجهزة المحمولة. فُعّلت عند الكتلة 419200. |
| Selective Disclosure | يتيح لمالك العنوان المحمي مشاركة مفاتيح العرض أو إفصاحات الدفع بشكل انتقائي مع أطراف ثالثة، مع إبقاء البيانات خاصة عن أي شخص آخر. |
| Shielded Address | يُسمى أيضًا zaddr. يبدأ بالحرف z. يُخفي المرسل والمستلم والمبلغ والمذكرة باستخدام zk-SNARKs. |
| Shielded Labs | منظمة مستقلة تعمل على اقتصاديات بروتوكول Zcash والإجماع. تقود حاليًا Crosslink وآلية استدامة الشبكة (Network Sustainability Mechanism). [GitHub](https://github.com/ShieldedLabs) |
| Shielded Transaction | معاملة تتم حصريًا بين عناوين محمية. وهي خاصة بالكامل على سلسلة الكتل. |
| Sol/s | حلول في الثانية (Solutions per second)، وهي مقياس لأداء تعدين Equihash. |
| Spending Key | المفتاح الخاص الذي يسمح بالإنفاق من عنوان محمي (ويتيح لك أيضًا عرض الرصيد والسجل). |
| Sprout | النسخة الأصلية من البروتوكول المحمي لـ Zcash (أُطلقت عام 2016). |

## T

| المصطلح | التعريف |
|------|-----------|
| Tachyon | برنامج التوسع الخاص بـ Zcash. وهو ليس جزءًا من NU7 ([ZIP 259](https://zips.z.cash/zip-0259)) وليس له تاريخ تفعيل. ينقل المحافظ من فحص كل كتلة نحو حالة محفظة تحمل الإثبات، والمزامنة العمياء (oblivious synchronization)، وحالة عقدة قابلة للتقليم، بهدف الوصول إلى إنتاجية محمية تبلغ آلاف المعاملات في الثانية. [موقع المشروع](https://tachyon.z.cash/overview/) |
| TAZ | عملة Zcash على شبكة الاختبار (عملة اختبار بلا قيمة). |
| Testnet | سلسلة كتل منفصلة لاختبار الترقيات والميزات قبل إطلاقها على الشبكة الرئيسية (mainnet). |
| Trailing Finality Layer (TFL) | بحث في إضافة طبقة نهائية تعمل خلف سلسلة إثبات العمل الخاصة بـ Zcash، بحيث يمكن اعتماد الكتل الحديثة نهائيًا دون الاستغناء عن التعدين. Crosslink هو التصميم الذي نتج عن هذا البحث. |
| Transaction | عملية دفع بين المستخدمين، تُرسَل إلى الشبكة وتُؤكَّد في النهاية ضمن كتلة. |
| Transaction Expiry | حد لارتفاع الكتلة يحدد المدى الذي يمكن فيه تضمين عملية دفع لم تُعدَّن بعد. يمكن تعدين عملية الدفع العادية ذات ارتفاع انتهاء الصلاحية غير الصفري N في الكتلة N أو قبلها، ولكن ليس في N+1. يؤدي الارتفاع 0 إلى تعطيل انتهاء الصلاحية. وهذا ليس مؤقتًا للاسترداد التلقائي. [ZIP 203](https://zips.z.cash/zip-0203) |
| Transaction Fee | الرسوم الافتراضية هي 0.0001 ZEC. تحصل الرسوم الأعلى على الأولوية، وقد تتسبب الرسوم المنخفضة جدًا في التأخير أو انتهاء الصلاحية. |
| Transparent Address | يُسمى أيضًا taddr. يبدأ بالحرف t. علني بالكامل (مثل Bitcoin). |
| Transparent Transaction | معاملة تتم حصريًا بين عناوين شفافة، وكل شيء فيها مرئي للعامة. |
| Turnstile | قاعدة محاسبية تتتبع مقدار القيمة التي تدخل كل مجمّع محمي وتخرج منه، بحيث لا يمكن لأي مجمّع أن يُخرج أكثر مما دخل إليه. استُخدمت عند كل انتقال بين المجمّعات في تاريخ Zcash، وهي تحرس حاليًا الانتقال من Orchard إلى Ironwood. [مزيد من المعلومات](/zcash-tech/the-turnstile) |

## U

| المصطلح | التعريف |
|------|-----------|
| Unified Address | صيغة عنوان حديثة (قُدّمت في NU5) تصلح للمدفوعات الشفافة والمحمية معًا في نص واحد. |
| Upgrade Activation | ارتفاع الكتلة المحدد الذي تُفعَّل عنده ترقية الشبكة تلقائيًا (مثل NU5 أو NU6). |

## V

| المصطلح | التعريف |
|------|-----------|
| Viewing Key | مفتاح خاص يتيح لك عرض الرصيد وسجل المعاملات لعنوان محمي دون القدرة على إنفاق الأموال. |

## W

| المصطلح | التعريف |
|------|-----------|
| Wallet | برنامج أو جهاز يخزّن المفاتيح الخاصة ويتيح لك إرسال ZEC واستلامها. تشمل المحافظ النشطة ZODL (iOS/Android)، و Zingo! (الهاتف المحمول/سطح المكتب)، و Zkool (الهاتف المحمول/سطح المكتب)، و Zallet (محفظة عقدة كاملة، في مرحلة تجريبية)، و Keystone (محفظة مادية). للاطلاع على القائمة الكاملة، راجع [محافظ منظومة Zcash](https://z.cash/ecosystem/?wallets=#tag-wallets) |
| WebZjs | أول مجموعة تطوير برمجيات (SDK) لـ JavaScript مخصصة لـ Zcash، طوّرتها ChainSafe لبيئات المتصفح. وهي الأساس الذي تقوم عليه إضافة Zcash Shielded Wallet (snap) التي جلبت ZEC المحمي إلى MetaMask. |

## X

| المصطلح | التعريف |
|------|-----------|
| XZC | رمز تداول قديم لـ Zcash كان يُستخدم في بعض منصات التداول القديمة. رمز التداول الرسمي هو ZEC. |

## Y

| المصطلح | التعريف |
|------|-----------|
| YWallet | محفظة Zcash تركّز على الخصوصية وتدعم عناوين Orchard و Sapling والعناوين الشفافة، وتشتهر بسرعة المزامنة. لم تعد تخضع للصيانة: فقد أكد مطورها أنها لن تُحدَّث لـ Ironwood، لذا لم يعد بإمكانها مواكبة الشبكة. Zkool، من المطور نفسه، هي الخليفة الذي تستمر صيانته. |

## Z

| المصطلح | التعريف |
|------|-----------|
| Zcash | عملة مشفرة تركّز على الخصوصية وتستخدم zk-SNARKs. تجمع بين المدفوعات الشفافة (على غرار Bitcoin) والمدفوعات المحمية بالكامل. |
| Zcash Foundation | منظمة مستقلة غير ربحية تدعم منظومة Zcash، وتموّل التطوير، وتعزز الخصوصية. |
| Zcash Network | شبكة نظير إلى نظير من العقد تتحقق من المعاملات وتحافظ على سلسلة الكتل. |
| ZEC | رمز العملة الرسمي لـ Zcash (لا تزال بعض منصات التداول تعرض XZC). |
| Z3 stack | البرامج الثلاثة التي تحل معًا محل zcashd: Zebra للعقدة، و Zaino للفهرسة، و Zallet للمحفظة. [حزمة Z3](/zcash-tech/z3-stack) |
| Zerocash | البروتوكول الأكاديمي (2014) الذي يستند إليه Zcash. |
| Zaino | مُفهرس يقدّم بيانات المحفظة من عقدة Zebra، مكتوب بلغة Rust بواسطة Zingo Labs. يتولى أعمال الفهرسة التي كان zcashd يقوم بها للمحافظ الخفيفة. [Zaino](/zcash-tech/zaino) |
| Zakura | عقدة كاملة منفصلة لـ Zcash مصممة للتوسع، يقودها Sean Bowe و Dev Ojha. تتزامن بشكل أسرع، وتقلّم الكتل بشكل أصلي، ويمكنها تشغيل zcashd فوقها للبرامج التي لا تزال تعتمد عليه. [عقدة Zakura](/zcash-tech/zakura-node) |
| Zallet | المحفظة التي تحل محل محفظة zcashd. انقل `wallet.dat` الموجود لديك بدلًا من البدء من جديد. [Zallet](/zcash-tech/zallet) و [المرجع السريع لـ Zallet](/using-zcash/zallet-quick-reference-guide) |
| Zebra | العقدة الكاملة التي تحل محل zcashd، وتتولى صيانتها Zcash Foundation. شغّلها إذا كنت بحاجة إلى عقدة تحقق. [العقدة الكاملة Zebra](/zcash-tech/zebra-full-node) |
| zcashd | العقدة الكاملة والمحفظة الأصليتان لـ Zcash، وقد أُوقفتا الآن. أوقف الإصدار 6.20.0 نفسه عند الكتلة 3417100 في 18 يوليو 2026، ولم يدعم مطلقًا NU6.3. شغّل حزمة Z3 أو Zakura بدلًا من ذلك. [دليل الترحيل](/guides/migration-guide-zcashd-to-zebrad-zallet) |
| ZIP | مقترح تحسين Zcash (Improvement Proposal)، وهو عملية الحوكمة المجتمعية المستخدمة لاقتراح تعديلات البروتوكول والمصادقة عليها. [مستودع ZIP](https://github.com/zcash/zips) |
| ZODL | Zcash Open Development Lab. وهي المنظمة المستقلة التي أُسست في مطلع عام 2026 على يد Josh Swihart وفريق الهندسة السابق في Electric Coin Company بعد استقالتهم إثر خلاف حول الحوكمة مع Bootstrap. جمعت أكثر من 25 مليون دولار في جولة تمويل أولي في مارس 2026، وتتولى صيانة محفظة ZODL، التي تغيّر اسمها من Zashi في فبراير 2026. [zodl.com](https://zodl.com) |
| zk-SNARKs | حجج المعرفة الموجزة غير التفاعلية القائمة على المعرفة الصفرية (Zero-Knowledge Succinct Non-Interactive Arguments of Knowledge)، وهي التقنية التشفيرية التي تقوم عليها المعاملات المحمية في Zcash. تتيح إثبات صحة عبارة ما (مثل صحة عملية إنفاق) دون الكشف عن أي معلومات سرية. |
| ZSA (Zcash Shielded Assets) | رموز يُصدرها المستخدمون وترث الخصوصية المحمية لـ Zcash، مما يتيح لأصول غير ZEC أن تنتقل بشكل خاص على الشبكة. محددة في [ZIP 226](https://zips.z.cash/zip-0226). وهي ليست جزءًا من NU7 ([ZIP 259](https://zips.z.cash/zip-0259)) وليس لها تاريخ تفعيل. |

---

**آخر تحديث:** أكتوبر 2026
**هل تريد المساهمة؟** [عدّل هذه الصفحة على GitHub](https://github.com/ZecHub/zechub/edit/main/site/Glossary_and_FAQs/Zcash_Library.md)
