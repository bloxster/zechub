<a href="https://github.com/zechub/zechub/edit/main/site/guides/Keystone_Zashi.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>

# دليل مستخدم Keystone Zodl (سابقًا Zashi)

> احتفظ بـ ZEC المحمي (shielded) في التخزين البارد (cold storage): اربط محفظة مادية (hardware wallet) من Keystone بتطبيق Zodl (سابقًا Zashi، وقد أُعيدت تسميته في فبراير 2026) ووقّع المعاملات (transactions) عبر رمز QR.

## باختصار (TL;DR)

* يعمل Keystone 3 Pro (أو Keystone 3) مع البرنامج الثابت (firmware) Cypherpunk كموقِّع معزول عن الشبكة (air-gapped) لـ Zodl، بحيث يمكن أن يبقى ZEC المحمي في التخزين البارد.
* قبل أن تبدأ: شحن البطارية أعلى من 20%، وكابل USB (أو بطاقة MicroSD بتنسيق FAT32) لتحديثات البرنامج الثابت، وتطبيق Zodl على هاتفك.
* تحقّق من الجهاز على موقع Keystone أولًا. يتأكد هذا من أنه لم يتم العبث به أثناء النقل.
* أنشئ المحفظة (wallet) على Keystone ودوّن عبارة البذور (seed phrase) المكوّنة من 24 كلمة دون اتصال بالإنترنت. لا تُدخلها أبدًا في Zodl.
* الاتصال: على Keystone اضغط على «...» > Connect Software Wallet > Zodl، ثم في Zodl اضغط على Connect Hardware Wallet وامسح رمز QR.
* يُنشئ Zodl كل معاملة؛ وأنت توافق عليها وتوقّعها على شاشة Keystone بمسح رموز QR ذهابًا وإيابًا.

## الشرح الأساسي

تحتفظ المحفظة المادية بمفاتيح الإنفاق (spending keys) الخاصة بك على جهاز منفصل لا يتصل بالإنترنت أبدًا. Keystone معزول عن الشبكة: فهو يتواصل مع هاتفك فقط عبر رموز QR، وليس عبر Bluetooth أو USB.

في هذا الإعداد، يمثّل Zodl جانب «المراقبة». فهو يتزامن مع شبكة Zcash، ويعرض رصيدك المحمي ويُعدّ المعاملات. أما Keystone فيحتفظ بعبارة البذور ويقوم بالتوقيع. يمكن لهاتف مخترق أن يرى رصيدك، لكنه لا يستطيع نقل الأموال دون موافقة على Keystone.

تغطي الخطوات أدناه تجهيز الجهاز، والتحقق منه، وتحديث البرنامج الثابت، وإنشاء المحفظة وربطها بـ Zodl.

## نظرة عامة

دليل Twitter:  => [Zodl (سابقًا Zashi) x تكامل المحفظة المادية Keystone: دليل Twitter](https://x.com/zashi_app/status/1869793574880973144) 

يمثّل هذا التكامل تطورًا مهمًا في سهولة استخدام Zcash من خلال تمكين التخزين البارد لـ ZEC المحمي. واجه مجتمع Zcash انتكاسات مع منصات محافظ مادية أخرى في الماضي، لكن Keystone برزت كشريك متعاون مستعد لتجاوز الحدود والابتكار جنبًا إلى جنب مع Electric Coin Company. حصل فريق Keystone على منحة (grant) من ZCG لتمويل الجزء الخاص به من العمل.

## درس تعليمي: Keystone X Zodl

<div className="my-8 w-full aspect-video max-w-3xl mx-auto rounded-2xl overflow-hidden shadow-lg bg-black">
  <iframe
    className="w-full h-full"
    src="https://www.youtube.com/embed/ktYf7josJKM"
    title="Keystone X Zodl Tutorial"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowFullScreen
    loading="lazy"
  />
</div>
    

## التحضير
[اطلب واستلم Keystone 3 Pro أو Keystone 3](https://keyst.one) 

مستوى البطارية: تأكّد من أن مستوى بطارية جهاز Keystone الخاص بك أعلى من 20%.

كابل USB أو بطاقة SD:

- كابل USB لتحديث البرنامج الثابت (مرفق).
- بطاقة Micro SD (أقل من 1 TB) للترقيات (تُشترى بشكل منفصل).

الوصول إلى موقع Keystone الرسمي للتحقق وتحديث البرنامج الثابت.

إعداد تطبيق Zodl على جهازك المحمول.

## [دليل خطوة بخطوة (جهاز Keystone)](https://keyst.one/get-started) 


**اختر لغتك**

- **التحقق من الجهاز (عبر QR):** يُعد التحقق من الجهاز أمرًا ضروريًا لاكتشاف أي تلوّث محتمل أثناء النقل، ومنع هجمات سلسلة التوريد، وضمان سلامة البرنامج الثابت المثبّت.
  - تفضّل بزيارة صفحة التحقق من الجهاز على موقع Keystone.
  - انقر على Scan QR Code على الموقع الرسمي.
  - استخدم كاميرا Keystone لمسح رمز QR المعروض على الموقع.
  - سيظهر رمز تحقق على شاشة Keystone.
  - أدخل هذا الرمز على الموقع لإكمال عملية التحقق.

- **تحديث البرنامج الثابت:**
  - التحديث عبر بطاقة MicroSD
    - تأكّد من أن شحن بطارية محفظة Keystone لا يقل عن 20%.
    - أدخل بطاقة SD في حاسوبك وهيّئها بتنسيق FAT32.
    - نزّل أحدث إصدار من البرنامج الثابت Cypherpunk من [صفحة تحديث البرنامج الثابت لـ Keystone](https://keyst.one/firmware) واحفظ الملف keystone3.bin في المجلد الجذر لبطاقة MicroSD.
    - ضع بطاقة SD التي تحتوي على البرنامج الثابت في محفظة Keystone.
    - ادخل إلى خيار «Upgrade» على محفظة Keystone، ثم اتبع التعليمات على الشاشة لبدء عملية التحديث.
  - **التحديث عبر كابل USB**
    - إذا كان إصدار البرنامج الثابت لديك أقل من 1.0.4، فستحتاج إلى إجراء التحديث الأولي باستخدام بطاقة MicroSD قبل أن تتمكن من متابعة التحديثات عبر USB.
    - تأكّد من أن شحن بطارية محفظة Keystone لا يقل عن 20%.
    - اضغط على via USB واستخدم كابل USB لتوصيل محفظة Keystone بحاسوبك. اضغط على [Approve] لمنح محفظة Keystone صلاحية الوصول عبر USB، إذ قد تسمح بالشحن فقط بخلاف ذلك.
    - افتح متصفح الويب على حاسوبك وانتقل إلى [صفحة تحديث البرنامج الثابت لـ Keystone](https://keyst.one/firmware)
    - في صفحة التحديث، انقر على زر Install Update واتبع التعليمات المقدّمة لتثبيت أحدث برنامج ثابت.
- **إنشاء المحفظة:**
    - كلمة مرور آمنة: اختر رمز PIN قويًا أو كلمة مرور قوية لحماية محفظتك.
    - تسمية محفظتك (اختياري): يمكنك اختياريًا إعطاء محفظتك اسمًا لتسهيل التعرف عليها، أو تخطي هذه الخطوة.
    - اختر Create New Wallet إذا كنت تُعدّ محفظة لأول مرة.
    - سيُنشئ جهازك عبارة بذور مكوّنة من 24 كلمة.
    - دوّن عبارة البذور هذه واحفظها في مكان آمن.
    - أكّد عبارة البذور بالتحقق من الكلمات بالترتيب الصحيح كما تظهر على الشاشة.
- **ربط محفظة Zodl + Keystone:**
    - على جهاز Keystone: اضغط على … في الصفحة الرئيسية
    - اضغط على Connect Software Wallet واختر Zodl. سيظهر رمز QR للاتصال بـ Zodl.
    - في تطبيق Zodl: اضغط على القائمة المنسدلة Zodl (أعلى يسار الشاشة)
    - اضغط على Connect Hardware Wallet
    - اضغط على Ready to Scan
    - امسح رمز QR المعروض على جهاز Keystone
    - في تطبيق Zodl: أكّد حساب محفظة Keystone بالضغط على الحساب المعروض
    - اضغط على Connect أسفل الشاشة


## مساعدة إضافية

<div className="my-8 w-full aspect-video max-w-3xl mx-auto rounded-2xl overflow-hidden shadow-lg bg-black">
  <iframe
    className="w-full h-full"
    src="https://www.youtube.com/embed/Jr6LqtD1W0s"
    title="Connect Keystone Hardware Wallet to Zodl"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowFullScreen
    loading="lazy"
  />
</div>
    

<div className="my-8 w-full aspect-video max-w-3xl mx-auto rounded-2xl overflow-hidden shadow-lg bg-black">
  <iframe
    className="w-full h-full"
    src="https://www.youtube.com/embed/t_OHb1KqrRg"
    title="Sign an Outgoing Transaction with Keystone"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowFullScreen
    loading="lazy"
  />
</div>
