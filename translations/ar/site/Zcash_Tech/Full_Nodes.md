<a href="https://github.com/zechub/zechub/edit/main/site/Zcash_Tech/Full_Nodes.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>

# العقد الكاملة (Full Nodes)

## باختصار

- تحتفظ العقدة الكاملة بنسخة كاملة من سلسلة الكتل (blockchain) الخاصة بـ Zcash، وتتحقق من كل كتلة (block) ومعاملة (transaction) جديدة وفقاً لقواعد الإجماع (consensus).
- Zebra (`zebrad`) هي العقدة (node) التي ينبغي لك تثبيتها اليوم. أما Zakura فهو تطبيق ثانٍ، متفرع من Zebra.
- لقد أُوقف zcashd. وقد بلغ نقطة التوقف عند نهاية الدعم (End-of-Support) في 18 يوليو 2026 عند ارتفاع الكتلة 3417100، ولم تعد تلك العقد تعمل.
- أصبحت العقدة والمحفظة (wallet) الآن برنامجين منفصلين. يعمل [Zallet](https://github.com/zcash/zallet) مع عقدة ويحتفظ بالمفاتيح.
- يمنحك تشغيل عقدتك الخاصة تحققاً مستقلاً ويلغي الحاجة إلى الوثوق بخادم شخص آخر.

## الشرح الأساسي

العقدة الكاملة هي برنامج يشغّل نسخة كاملة من سلسلة الكتل الخاصة بعملة مشفرة، مما يمنحك الوصول إلى ميزات البروتوكول.

وهي تحتفظ بسجل كامل لكل معاملة حدثت منذ كتلة التكوين، ولذلك فهي قادرة على التحقق من صحة المعاملات والكتل الجديدة التي تُضاف إلى سلسلة الكتل.

## تطبيقات العقد

### Zebra

Zebra هو تطبيق عقدة كاملة مستقل وجاهز للإنتاج لبروتوكول Zcash، أنشأته Zcash Foundation وهو مكتوب بلغة Rust. ونظراً لإيقاف zcashd، فإن Zebra (`zebrad`) هو العقدة الكاملة الموصى بها لعمليات النشر الجديدة.

يتحقق Zebra من الكتل والمعاملات، ويشارك في شبكة النظير إلى النظير، ويوفر واجهة RPC للتطبيقات. أصبحت المحفظة الآن مكوناً منفصلاً: يعمل [Zallet](https://github.com/zcash/zallet) مع عقدة Zebra ويتولى المفاتيح والأرصدة. ويحل هذا محل zcashd، الذي كان يجمع العقدة والمحفظة في عملية واحدة.

لخدمة المحافظ الخفيفة المحمية (shielded)، تعمل العقدة إلى جانب مُفهرس، إما [lightwalletd](https://github.com/zcash/lightwalletd) الراسخ أو [Zaino](https://zechub.wiki/zcash-tech/zaino) الأحدث.

احرص على قراءة كتاب Zebra للاطلاع على تعليمات الإعداد، وانضم إلى خادم Discord الخاص بالبحث والتطوير للحصول على الدعم.

[GitHub](https://github.com/ZcashFoundation/zebra/)

[كتاب Zebra](https://zebra.zfnd.org)

راجع [العقدة الكاملة Zebra](/zcash-tech/zebra-full-node) للاطلاع على خطوات التثبيت والإعداد ومتطلبات العتاد.

### Zakura

Zakura هو عقدة كاملة ثانية متوافقة مع الإجماع، متفرعة من Zebra وطورتها Valar Group بالتعاون مع مشروع Tachyon. وهي تتبع قواعد البروتوكول نفسها وتضيف مزامنة أسرع، وتقليم الكتل، وطبقة توافق مع RPC الخاص بـ zcashd. راجع [عقدة Zakura](/zcash-tech/zakura-node).

### zcashd (موقوف)

> **ملاحظة:** لقد أُوقف zcashd. وقد [أعلنت Electric Coin Company عن إيقافه](https://z.cash/support/zcashd-deprecation/)، وبلغ التوقف التلقائي عند نهاية الدعم في 18 يوليو 2026 عند ارتفاع الكتلة 3417100. توقفت كل عقدة zcashd 6.20.0 غير معدّلة عند ذلك الارتفاع وترفض إعادة التشغيل، كما أن البرنامج لا يدعم NU6.3. استخدم Zebra. إذا كنت تملك `wallet.dat` في zcashd، فاتبع [دليل الترحيل: من zcashd إلى zebrad/Zallet](https://zechub.wiki/guides/migration-guide-zcashd-to-zebrad-zallet).

كان zcashd هو التطبيق الأصلي للعقدة الكاملة لـ Zcash، وقد طورته وصانته Electric Coin Company. تم الإبقاء على تعليمات البناء أدناه للرجوع إليها ولمساعدة المشغّلين الذين ينتقلون بعيداً عن zcashd.

يوفر zcashd مجموعة من واجهات API عبر واجهة RPC الخاصة به. وتقدم واجهات API هذه وظائف تتيح للتطبيقات الخارجية التفاعل مع العقدة.

يُعد [lightwalletd](https://github.com/zcash/lightwalletd) مثالاً على تطبيق يستخدم عقدة كاملة لتمكين المطورين (developers) من بناء وصيانة محافظ خفيفة محمية مناسبة للهواتف المحمولة دون الحاجة إلى التفاعل مباشرة مع zcashd.

[القائمة الكاملة لأوامر RPC المدعومة](https://zcash.github.io/rpc/)

[كتاب zcashd](https://zcash.github.io/zcash/)

#### تشغيل عقدة (Linux)

- تثبيت التبعيات

      sudo apt update

      sudo apt-get install \
      build-essential pkg-config libc6-dev m4 g++-multilib \
      autoconf libtool ncurses-dev unzip git python3 python3-zmq \
      zlib1g-dev curl bsdmainutils automake libtinfo5

- استنسخ أحدث إصدار، ثم انتقل إليه وأعدّه وابنِه:

      git clone https://github.com/zcash/zcash.git

      cd zcash/

      git checkout v5.4.1
      ./zcutil/fetch-params.sh
      ./zcutil/clean.sh
      ./zcutil/build.sh -j$(nproc)

- مزامنة سلسلة الكتل (قد تستغرق عدة ساعات)

    لتشغيل العقدة، نفّذ:

      ./src/zcashd

- تُخزَّن المفاتيح الخاصة في ~/.zcash/wallet.dat

[دليل zcashd على Raspberry Pi](https://zechub.notion.site/Raspberry-Pi-4-a-zcashd-full-node-guide-6db67f686e8d4b0db6047e169eed51d1)

## الآثار العملية

### الشبكة

بتشغيلك عقدة كاملة، فإنك تساعد في تقوية شبكة Zcash من خلال دعم طابعها اللامركزي (decentralized).

ويساعد ذلك في منع السيطرة العدائية والحفاظ على صمود الشبكة أمام بعض أشكال التعطيل.

تعرض خوادم بذر DNS قائمة بعقد موثوقة أخرى عبر خادم مدمج. ويتيح ذلك انتشار المعاملات في جميع أنحاء الشبكة.

### إحصاءات الشبكة

فيما يلي أمثلة على منصات تتيح الوصول إلى بيانات شبكة Zcash:

[مستكشف الكتل Zcash](https://mainnet.zcashexplorer.app)

[بيانات شبكة Coin Metrics](https://docs.coinmetrics.io/network-data/network-data-overview) — توثيق المقاييس وروابط إلى تغطية الأصول الحالية.

يمكنك أيضاً المساهمة في تطوير الشبكة من خلال إجراء الاختبارات أو اقتراح تحسينات جديدة وتقديم المقاييس.

### التعدين

يحتاج المعدّنون (miners) إلى عقد كاملة للوصول إلى جميع أوامر RPC المتعلقة بالتعدين (mining) مثل getblocktemplate و getmininginfo.

يتيح zcashd أيضاً التعدين إلى coinbase محمية. ويملك المعدّنون ومجمّعات (pools) التعدين خيار التعدين مباشرة لتجميع ZEC المحمية في عنوان (address) من نوع z-address بشكل افتراضي.

اقرأ [دليل التعدين](https://zcash.readthedocs.io/en/latest/rtd_pages/zcash_mining_guide.html) أو انضم إلى صفحة منتدى المجتمع الخاصة بـ [معدّني Zcash](https://forum.zcashcommunity.com/c/mining/13).

### الخصوصية

يتيح لك تشغيل عقدة كاملة التحقق بشكل مستقل من جميع المعاملات والكتل على شبكة Zcash.

يجنّبك تشغيل عقدة كاملة بعض مخاطر الخصوصية (privacy) المرتبطة باستخدام خدمات طرف ثالث للتحقق من المعاملات نيابة عنك.

كما يتيح لك استخدام عقدتك الخاصة الاتصال بالشبكة عبر [Tor](https://zcash.github.io/zcash/user/tor.html).
ولهذا ميزة إضافية تتمثل في السماح للمستخدمين الآخرين بالاتصال بشكل خاص بعنوان .onion الخاص بعقدتك.

## الأخطاء الشائعة

- بناء zcashd وفق التعليمات أعلاه وتوقع الحصول على عقدة تعمل. فهذه الملفات التنفيذية تتوقف عند ارتفاع الإيقاف.
- تشغيل عقدة والافتراض أن محفظتك على الهاتف المحمول تستخدمها الآن. تظل المحفظة الخفيفة تتواصل مع أي خادم مُعدّة للاتصال به إلى أن توجّهها إلى خادمك الخاص. راجع [عقد المحافظ الخفيفة](/zcash-tech/lightwallet-nodes).
- تشغيل `zebrad` وحده وتوقع أن تتصل به المحافظ الخفيفة. تحتاج العقدة إلى مُفهرس بجانبها، إما lightwalletd أو [Zaino](/zcash-tech/zaino).
- البحث عن أوامر RPC الخاصة بالمحفظة على العقدة. فقد انتقلت المفاتيح والأرصدة إلى Zallet.

## صفحات ذات صلة

- [العقدة الكاملة Zebra](/zcash-tech/zebra-full-node) - تثبيت العقدة الموصى بها وإعدادها وتشغيلها
- [عقدة Zakura](/zcash-tech/zakura-node) - تطبيق العقدة الثاني، المتفرع من Zebra
- [عقد المحافظ الخفيفة](/zcash-tech/lightwallet-nodes) - الخوادم التي تستعلم منها المحافظ الخفيفة
- [Zaino](/zcash-tech/zaino) - مُفهرس Rust الذي يخدم المحافظ الخفيفة
- [مزامنة محفظة Zcash](/zcash-tech/zcash-wallet-syncing) - لماذا تعمل المزامنة بالطريقة التي تعمل بها

## لمزيد من التعلم

اقرأ [وثائق الدعم](https://zcash.readthedocs.io/en/latest/)

انضم إلى [خادم Discord](https://discord.gg/zcash) الخاص بنا أو تواصل معنا على [X](https://X.com/ZecHub)
