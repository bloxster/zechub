# الأسئلة الشائعة

قائمة بأكثر الأسئلة شيوعًا حول Zcash. لاستكشاف أخطاء عميل Zcash وإصلاحها، يُرجى مراجعة [دليل استكشاف الأخطاء وإصلاحها الرسمي](https://zcash.readthedocs.io/en/latest/rtd_pages/troubleshooting_guide.html).

### التنقل السريع

<div className="flex flex-wrap gap-2 my-4">
  <a href="#what-is-zcash" className="inline-flex px-3 py-1.5 rounded-full border border-border bg-card text-sm no-underline hover:bg-accent">ما هي Zcash؟</a>
  <a href="#how-can-i-acquire-zcash" className="inline-flex px-3 py-1.5 rounded-full border border-border bg-card text-sm no-underline hover:bg-accent">كيف يمكنني الحصول على Zcash؟</a>
  <a href="#what-is-the-difference-between-zcash-and-other-cryptocurrencies" className="inline-flex px-3 py-1.5 rounded-full border border-border bg-card text-sm no-underline hover:bg-accent">ما الفرق عن العملات المشفرة الأخرى؟</a>
  <a href="#how-is-the-zcash-protocol-governed" className="inline-flex px-3 py-1.5 rounded-full border border-border bg-card text-sm no-underline hover:bg-accent">حوكمة البروتوكول؟</a>
  <a href="#where-is-my-transaction" className="inline-flex px-3 py-1.5 rounded-full border border-border bg-card text-sm no-underline hover:bg-accent">أين معاملتي؟</a>
  <a href="#is-zcash-really-private" className="inline-flex px-3 py-1.5 rounded-full border border-border bg-card text-sm no-underline hover:bg-accent">هل Zcash خاصة حقًا؟</a>
  <a href="#a-few-common-misconceptions" className="inline-flex px-3 py-1.5 rounded-full border border-border bg-card text-sm no-underline hover:bg-accent">مفاهيم خاطئة شائعة</a>
  <a href="#zcashd-stopped-working-what-do-i-run-now" className="inline-flex px-3 py-1.5 rounded-full border border-border bg-card text-sm no-underline hover:bg-accent">توقف zcashd عن العمل؟</a>
  <a href="#how-do-i-move-my-zcashd-wallet-to-zallet" className="inline-flex px-3 py-1.5 rounded-full border border-border bg-card text-sm no-underline hover:bg-accent">نقل محفظتي إلى Zallet؟</a>
  <a href="#what-is-ironwood-and-do-i-need-to-do-anything" className="inline-flex px-3 py-1.5 rounded-full border border-border bg-card text-sm no-underline hover:bg-accent">ما هو Ironwood؟</a>
  <a href="#which-full-node-should-i-run" className="inline-flex px-3 py-1.5 rounded-full border border-border bg-card text-sm no-underline hover:bg-accent">أي عقدة كاملة؟</a>
  <a href="#where-do-i-look-up-a-term-from-a-release-note" className="inline-flex px-3 py-1.5 rounded-full border border-border bg-card text-sm no-underline hover:bg-accent">البحث عن مصطلح؟</a>
</div>

---

## ما هي Zcash؟

<div className="rounded-2xl border border-border bg-card p-5 my-4">

Zcash عملة رقمية تتيح معاملات (transactions) سريعة وسرية برسوم (fees) منخفضة. الخصوصية (privacy) هي الميزة المحورية لـ Zcash. وقد كانت رائدة في استخدام إثباتات (proofs) المعرفة الصفرية (zero-knowledge) لتشفير (encryption) جميع المعاملات.

تتوفر عدة محافظ (wallets) لمدفوعات (payments) فورية ومحمولة وآمنة وخاصة: [المحافظ](/using-zcash/wallets)

</div>

## كيف يمكنني الحصول على Zcash؟

<div className="rounded-2xl border border-border bg-card p-5 my-4">

يمكنك شراء ZEC على [منصات التداول الوصائية](/using-zcash/custodial-exchanges)، أو [منصات التداول اللامركزية](/using-zcash/dex)، أو [منصات المبادلة المركزية](/using-zcash/centralized-swaps).

يمكنك أيضًا شراء Zcash من نظير إلى نظير أو الحصول عليها عن طريق التعدين (mining).

</div>

## ما الفرق بين Zcash والعملات المشفرة الأخرى؟

<div className="rounded-2xl border border-border bg-card p-5 my-4">

Zcash أكثر خصوصية بشكل جوهري من Bitcoin أو Ethereum. فهي توفر أزمنة كتل (block) سريعة (25 ثانية منذ تفعيل NU7 في نوفمبر 2026، بعد أن كانت 75 ثانية)، ورسومًا منخفضة، وترقيات منتظمة.

يمكن للمستخدمين الاختيار بين المعاملات **الشفافة (transparent)** أو **المحمية (shielded)**. لمزيد من المعلومات، راجع [منظومة محمية](https://electriccoin.co/blog/shielded-ecosystem).

</div>

## كيف تتم حوكمة بروتوكول Zcash؟

<div className="rounded-2xl border border-border bg-card p-5 my-4">

يخضع البروتوكول للحوكمة (governance) عبر عملية **مقترح تحسين Zcash (ZIP)**. يمكن لأي شخص تقديم مسودة ZIP. يناقش المجتمع المسودات، ثم يقبلها محررو ZIP أو يرفضونها:

- [Daira Hopwood](https://twitter.com/feministPLT) (Electric Coin Company)
- [Deirdre Connolly](https://twitter.com/durumcrustulum) (Zcash Foundation)

تُدوَّن القرارات في المواصفات ويُصادَق عليها على السلسلة (on-chain) عندما تعتمدها الشبكة.

</div>

## أين معاملتي؟

<div className="rounded-2xl border border-border bg-card p-5 my-4">

اقرأ أولًا [دليلنا إلى مستكشفات الكتل](/guides/blockchain-explorers). ثم تحقق من [مستكشف كتل Zcash](https://mainnet.zcashexplorer.app).

لا توجد مدة ثابتة قدرها 25 دقيقة لانتهاء الصلاحية ولا مؤقت استرداد تلقائي. تحقق من ارتفاع انتهاء الصلاحية الذي سجلته محفظتك، ومما إذا كان الدفع الأصلي قد تم تعدينه. إذا انتهت صلاحيته دون أن يُعدَّن، فإنه لم تتم تسويته على السلسلة؛ ولا ينشئ انتهاء الصلاحية معاملة استرداد. دع المحفظة تُكمل المزامنة قبل اتباع إرشاداتها لإعادة المحاولة. راجع [استكشاف أخطاء المعاملات وإصلاحها](/using-zcash/transactions#troubleshooting-stuck-transactions) و[ZIP 203](https://zips.z.cash/zip-0203).

**الأسباب الشائعة لعدم ظهور المعاملة:**

- انقطاع الاتصال
- رسوم المعاملة منخفضة جدًا
- ازدحام الشبكة
- عدد كبير جدًا من المدخلات الشفافة (حجم كبير جدًا)

**نصائح للنجاح:**

- استخدم اتصالًا مستقرًا
- ادفع الرسوم القياسية (أو أعلى منها للأولوية)
- انتظر وأعد المحاولة لاحقًا
- استخدم مدخلات أقل لإبقاء المعاملة صغيرة

</div>

## هل Zcash خاصة حقًا؟

<div className="rounded-2xl border border-border bg-card p-5 my-4">

**نعم.** تشفّر Zcash بيانات المرسل والمبلغ والمستلم في المعاملات المحمية.

Zcash **لا** تقوم بما يلي:

- تشفير المعاملات متعددة التوقيعات (دمج FROST قيد الانتظار)
- الحماية من الارتباطات مع المعاملات الشفافة
- إخفاء عناوين (addresses) IP

لمزيد من القراءة: [منظومة محمية](https://web.archive.org/web/20260903010654/https://electriccoin.co/blog/shielded-ecosystem/)

</div>

## توقف zcashd عن العمل، فماذا أشغّل الآن؟

<div className="rounded-2xl border border-border bg-card p-5 my-4">

تم إيقاف zcashd نهائيًا. أوقف الإصدار 6.20.0 نفسه عند الكتلة 3417100 في 18 يوليو 2026، ولم يدعم قط Ironwood (NU6.3)، لذا لا يوجد إصدار لاحق للترقية إليه.

شغّل حزمة Z3 بدلًا من ذلك. تتقاسم ثلاثة برامج المهمة التي كان zcashd يقوم بها وحده:

- **[Zebra](/zcash-tech/zebra-full-node)** يتحقق من صحة السلسلة (chain)
- **[Zaino](/zcash-tech/zaino)** يفهرسها ويقدّم بيانات المحفظة
- **[Zallet](/zcash-tech/zallet)** يحتفظ بأموالك

[Zakura](/zcash-tech/zakura-node) عقدة كاملة (full node) بديلة. كما أنه يشغّل zcashd فوقه، وهو ما يفيد إذا كانت برامج أخرى لا تزال تتوقع الواجهة القديمة.

ابدأ بـ [دليل الترحيل](/guides/migration-guide-zcashd-to-zebrad-zallet).

</div>

## كيف أنقل محفظة zcashd الخاصة بي إلى Zallet؟

<div className="rounded-2xl border border-border bg-card p-5 my-4">

يقوم Zallet بترحيل `wallet.dat` موجود، لذا تحتفظ بأموالك وعناوينك.

انسخ `wallet.dat` احتياطيًا أولًا، ثم اتبع [دليل الترحيل](/guides/migration-guide-zcashd-to-zebrad-zallet). احتفظ بتلك النسخة الاحتياطية حتى تتأكد من رصيدك في Zallet.

يغطي [المرجع السريع لـ Zallet](/using-zcash/zallet-quick-reference-guide) الأوامر اليومية بعد انتقالك.

</div>

## ما هو Ironwood، وهل أحتاج إلى فعل أي شيء؟

<div className="rounded-2xl border border-border bg-card p-5 my-4">

Ironwood هو ترقية الشبكة (network upgrade) NU6.3، وهي مفعّلة على الشبكة الرئيسية منذ الكتلة 3,428,143 في 28 يوليو 2026. تضيف مجمّعًا (pool) محميًا مبنيًا على دائرة مصحّحة، وتُغلق مجمّع [Orchard](/using-zcash/shielded-pools) الأقدم، بحيث يمكن لأي شخص التحقق من عدم تزوير أي ZEC.

**إذا كنت تحتفظ بأموال محمية، فقد تحتاج إلى نقلها.** لا تغادر القيمة Orchard إلا عبر [البوابة الدوارة](/zcash-tech/the-turnstile)، وتتعامل المحافظ مع النقل في أوقات مختلفة وبطرق مختلفة. اسأل مزوّد محفظتك قبل نقل أي شيء يدويًا.

اقرأ [Ironwood](/zcash-tech/ironwood) لمعرفة ما الذي تغيّر ولماذا.

</div>

## أي عقدة كاملة يجب أن أشغّل؟

<div className="rounded-2xl border border-border bg-card p-5 my-4">

**[Zebra](/zcash-tech/zebra-full-node)** هو البديل المباشر لـ zcashd والعقدة (node) التي يفترضها دليل الترحيل. اختره ما لم يكن لديك سبب لعدم ذلك.

**[Zakura](/zcash-tech/zakura-node)** تطبيق ثانٍ موجّه نحو التوسع، مع مزامنة أسرع وتقليم أصلي للكتل. اختره إذا كنت تحتاج إلى ذلك، أو إذا كنت تعتمد على برامج لا تزال تتواصل مع zcashd.

كلاهما يتحقق من صحة السلسلة نفسها، لذا فهذا خيار تشغيلي وليس خيارًا يتعلق بالإجماع (consensus).

</div>

## أين أبحث عن مصطلح ورد في بيان إصدار؟

<div className="rounded-2xl border border-border bg-card p-5 my-4">

راجع [مكتبة Zcash](/glossary-and-faqs/zcash-library). تغطي برامج العقد والمحافظ، والمجمّعات المحمية، وأرقام ZIP الأكثر تكرارًا.

</div>

## بعض المفاهيم الخاطئة الشائعة

<div className="rounded-2xl border border-border bg-card p-5 my-4 overflow-x-auto">

<table className="w-full border-collapse">
  <thead>
    <tr className="border-b border-border bg-amber-100 dark:bg-zinc-800">
      <th className="py-4 px-5 text-left font-bold text-amber-800 dark:text-white">المفهوم الخاطئ</th>
      <th className="py-4 px-5 text-left font-bold text-amber-800 dark:text-white">الإجابة الصحيحة</th>
    </tr>
  </thead>
  <tbody>
    <tr className="border-b border-border hover:bg-amber-50 dark:hover:bg-zinc-700">
      <td className="py-4 px-5 font-medium text-foreground">هل Zcash عملة مركزية؟</td>
      <td className="py-4 px-5 text-foreground">لا. تمنع اتفاقية العلامة التجارية Zcash Foundation أو ECC من التصرف ضد إجماع المجتمع. وقد ثبت أن الحوكمة لامركزية (decentralized) (راجع [تقرير Messari](https://messari.io/report/decentralizing-zcash)). تتيح استطلاعات المجتمع، وZecHub، ونادي Zcash Foundation A/V جميعها مشاركة واسعة.</td>
    </tr>
    <tr className="border-b border-border hover:bg-amber-50 dark:hover:bg-zinc-700">
      <td className="py-4 px-5 font-medium text-foreground">هل لدى Zcash باب خلفي؟</td>
      <td className="py-4 px-5 text-foreground">لا. لا يحتوي Zcash ولا أي برنامج تشفيري قمنا ببنائه على باب خلفي، ولن يحتوي عليه أبدًا.</td>
    </tr>
    <tr className="border-b border-border hover:bg-amber-50 dark:hover:bg-zinc-700">
      <td className="py-4 px-5 font-medium text-foreground">هل Zcash خاضعة لسيطرة شركة؟</td>
      <td className="py-4 px-5 text-foreground">غير صحيح. على الرغم من أننا نتشارك مع شركات في مجال الأبحاث، تظل Zcash ملتزمة باللامركزية. تعمل عدة منظمات مستقلة معًا من أجل الحفظ الذاتي وحقوق الخصوصية.</td>
    </tr>
    <tr className="hover:bg-amber-50 dark:hover:bg-zinc-700">
      <td className="py-4 px-5 font-medium text-foreground">Zcash ذات خصوصية محدودة مقارنةً بعملات الخصوصية الأخرى</td>
      <td className="py-4 px-5 text-foreground">لا. تعتمد الخصوصية على نمط Monero/Grin على الطُّعوم (التي يمكن التغلب عليها). تشفّر Zcash جميع بيانات المعاملات المحمية بحيث لا يمكن تمييز أي معاملة في المجمّع عن غيرها. راجع [ليست خاصة بما يكفي؟](https://electriccoin.co/blog/not-private-enough-mixers-and-decoys-wont-protect-you-for-long/).</td>
    </tr>
  </tbody>
</table>

</div>

---

**آخر تحديث:** أكتوبر 2026
**هل تريد المساهمة؟** [عدّل هذه الصفحة على GitHub](https://github.com/ZecHub/zechub/edit/main/site/Glossary_and_FAQs/FAQ.md)
