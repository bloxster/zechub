#  <img src="/content-images/e38b13a9-d410-426a-a1e6-2dde105d56c4-6b8154e5ae.webp" alt="Alt Text" width="50"/> ZingoLabs

[الموقع الرسمي](https://zingolabs.org/) - [GitHub](https://github.com/zingolabs) - [X/Twitter](https://x.com/ZingoLabs) - [Instagram](https://www.instagram.com/zingolabesp/)

ZingoLabs هو فريق من أصحاب الرؤى مكرّس لتحسين التجربة الإنسانية. نؤمن بأن التكنولوجيا يجب أن تخدم البشرية، وبأننا نزدهر من خلال التفاعلات القائمة على التراضي. ونحن نعمل على تحديد الأنماط التي تجعل ذلك ممكناً.

تعمل Zingo Lab Cyan وفق نموذج المنظمة المستقلة اللامركزية (DAO) المحمية (shielded). نحتفظ بأموالنا في خزينة (treasury) يمتلك فيها كل عضو مفتاح العرض (viewing key). وتُصرف الأموال من الخزينة عندما يصوّت الأعضاء لصالح مقترح ما.

## المشاريع

### محفظة Zingo! ([GitHub](https://github.com/zingolabs/zingo-mobile))
محفظة (wallet) Zingo هي محفظة Zcash متكاملة الميزات صُمّمت لتكون سهلة الاستخدام، رغم أنها تتضمن بعض الميزات المتقدمة للمستخدمين الأكثر خبرة. تدعم المجمّعات (pools) الشفافة (transparent) ومجمّعات Sapling وIronwood، وتحتوي على دفتر عناوين (address) للمدفوعات (payments) المتكررة، وهي متاحة بلغات متعددة. وكانت أول محفظة تدعم Orchard وتطبّق صيغ NU5.

من أبرز ميزات Zingo! قدرتها على استخدام حقل المذكرة (memo) لتقديم معلومات قيّمة حول معاملاتك (transactions).

تتوفر Zingo! للأجهزة المحمولة وأجهزة الحاسوب الشخصية. ستجد جميع ملفات التنزيل [هنا](https://zingolabs.org/)

### Zingolib ([GitHub](https://github.com/zingolabs/zingolib))
واجهة API وتطبيق اختباري يتيحان وظائف Zcash لاستخدامها في التطبيقات. توفّر Zingolib مكتبةً لـ zingo-mobile، إضافةً إلى تطبيق سطر أوامر مُضمَّن يُسمّى zingo-cli، وهو محفظة خفيفة تعمل من سطر الأوامر وتتواصل مع مُفهرس للسلسلة (chain) مثل [Zaino](/zcash-tech/zaino).

### مُفهرس Zaino ([GitHub](https://github.com/zingolabs/zaino))
Zaino هو مُفهرس Rust لسلسلة الكتل (blockchain) الخاصة بـ Zcash، طوّره فريق Zingo. وهو المُفهرس ضمن [حزمة Z3](/zcash-tech/z3-stack): إذ يفهرس بيانات السلسلة من مُدقِّق [Zebra](/zcash-tech/zebra-full-node) ويخدم المحافظ ومستكشفات الكتل وغيرها من العملاء. ويمثّل المسار الخلف لـ lightwalletd.

يقدّم Zaino ميزات أساسية لكلٍّ من العملاء الخفيفة (light client)، مثل المحافظ والتطبيقات التي لا تحتاج إلى السجل الكامل لسلسلة الكتل، والعملاء أو المحافظ الكاملة. كما يدعم مستكشفات الكتل (block)، إذ يتيح الوصول إلى سلسلة الكتل النهائية وإلى أفضل سلسلة غير نهائية ومجمّع المعاملات غير المؤكدة (mempool) اللذين يديرهما مُدقِّق Zebra كامل.

وصل `zcashd` إلى توقف نهاية الدعم في 18 يوليو 2026 عند الكتلة 3,417,100، ولا يدعم Ironwood (NU6.3). ويقرأ Zaino من مُدقِّق Zebra.

###  ZLN (zcash-local-net) ([GitHub](https://github.com/zingolabs/infrastructure/tree/dev/zcash_local_net))

مجموعة من الأدوات المساعدة التي تشغّل عمليات Zcash وتديرها. وتُستخدم لاختبارات التكامل في تطوير:
- العملاء الخفيفة
- المُفهرسات
- المُدقِّقات

هدفها توفير بيئة اختبار شديدة المرونة ومتينة للعقد (nodes) الأساسية (المُدقِّقات) مثل Zebra، والمُفهرسات مثل Zaino، ومحافظ العميل الخفيف مثل zingo-cli.

الحزمة الافتراضية التي تشغّلها هي Zebrad وzainod وzcash-devtool. أما عمليتا `zcashd` وlightwalletd القديمتان فتقعان خلف ميزة `legacy-stack` غير المدعومة، ومن المقرر إزالتهما.

إلى جانب توفير أدوات (tools) لبدء بيانات سلسلة Zcash وتخزينها مؤقتاً وتحميلها (للشبكة الرئيسية mainnet وشبكة الاختبار testnet وشبكة regtest)، تُستخدم zcash-local-net من مجموعة اختبارات Zaino لتقييم خدمات Lightwallet RPC المستضافة في Zaino (راجع [Zaino وثائق الاختبار](https://github.com/zingolabs/zaino/blob/dev/docs/testing.md)).
