<a href="https://github.com/zechub/zechub/edit/main/site/Using_Zcash/Metamask_Snap.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>

# دليل تكامل MetaMask Zcash Snap

> احتفظ بـ ZEC المحمي (shielded) وأرسله من إضافة المتصفح MetaMask، باستخدام Zcash Snap من ChainSafe ومحفظة (wallet) الويب WebZjs.

## باختصار

* تضيف ChainSafe Zcash Snap دعم ZEC المحمي إلى MetaMask على أجهزة سطح المكتب (Chrome، Edge، Firefox). لا حاجة إلى برنامج Zcash منفصل.
* تُشتق مفاتيح Zcash الخاصة بك من عبارة الاسترداد في MetaMask. وهذه العبارة الواحدة تحمي الآن كلاً من ETH وـ ZEC لديك.
* تدير ZEC عبر محفظة WebZjs على webzjs.chainsafe.dev، وليس في قائمة الأصول الخاصة بـ MetaMask.
* يحصل حسابك على عنوان (address) موحّد، فيدفع لك المرسلون عبر أكثر مجمّع (pool) خصوصيةً يدعمونه.
* موّله عن طريق المبادلة (swap) (على سبيل المثال من ETH إلى ZEC) أو بسحب ZEC من منصة تداول (exchange) إلى عنوانك المحمي.
* الاسترداد: أعد تثبيت MetaMask وـ Snap، ثم استورد عبارة الاسترداد الحالية لديك.

## الشرح الأساسي

MetaMask Snap هي إضافة صغيرة تعمل في بيئة معزولة، تعلّم MetaMask التعامل مع سلسلة كتل (blockchain) جديدة. تتيح Zcash Snap، التي طوّرتها ChainSafe ودقّقتها Hacken، لـ MetaMask اشتقاق مفاتيح Zcash من عبارة الاسترداد الحالية لديك وتوقيع معاملات (transactions) Zcash.

لا تعرض MetaMask نفسها رصيد ZEC. بدلاً من ذلك، تتصل محفظة الويب WebZjs بـ Snap، وتتزامن مع شبكة Zcash عبر خادم lightwalletd، وتعرض عناوينك ورصيدك. تُحسب الإثباتات (proofs) المحمية داخل متصفحك باستخدام WebAssembly، ولهذا قد يستغرق الإرسال بعض الوقت.

تغطي الخطوات أدناه تثبيت Snap، وربط WebZjs، وتمويل المحفظة، والإرسال والاستلام، والتحقق من مكونات عنوانك الموحّد.

## الدليل الكامل

للحصول على شرح تفصيلي كامل وتوضيح مرئي، شاهد [**دليل YouTube**](https://www.youtube.com/watch?v=UJh9Ilkohdw) هذا: 

<div className="my-8 w-full aspect-video max-w-3xl mx-auto rounded-2xl overflow-hidden shadow-lg bg-black">
  <iframe
    className="w-full h-full"
    src="https://www.youtube.com/embed/UJh9Ilkohdw"
    title="How to use ZEC on Metamask"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowFullScreen
    loading="lazy"
  />
</div>
     

يدعم MetaMask الآن **Zcash المحمي (ZEC)** عبر **Zcash Snap التي طوّرتها ChainSafe**، مما يتيح لك إرسال ZEC الخاص واستلامه وإدارته مباشرةً في محفظة متصفحك. وقد دقّقتها **Hacken** وأُدرجت في **دليل Snaps الرسمي لـ MetaMask**، وهي **لا تتطلب أي برنامج Zcash منفصل** - فقط MetaMask وـ Snap.

---

## **المتطلبات المسبقة**


> [**إضافة MetaMask**](https://snaps.metamask.io/snap/npm/zcashcommunitygrants/webzjs-zcash-snap/) (لسطح المكتب فقط) - Chrome أو Edge أو Firefox.
> حساب MetaMask - مع عبارة بذور (seed phrase) محفوظة بأمان؛ إذ تشتق Snap مفاتيح Zcash منها.  
> اتصال إنترنت مستقر - للتزامن مع شبكة Zcash.  
> الأموال - ETH لمبادلته بـ ZEC أو ZEC من منصة تداول.

> **نصيحة:** احمِ عبارة الاسترداد الخاصة بـ MetaMask - فهي تتحكم في كلٍّ من ETH وـ ZEC.

---

## **1. ثبّت Zcash Snap**

1. انتقل إلى [**دليل MetaMask Snaps**](https://snaps.metamask.io/snap/npm/zcashcommunitygrants/webzjs-zcash-snap/).  
2. ابحث عن [**«Zcash WebZjs Wallet»**](https://snaps.metamask.io/snap/npm/zcashcommunitygrants/webzjs-zcash-snap/) أو [**«WebZjs Zcash Snap»**](https://snaps.metamask.io/snap/npm/zcashcommunitygrants/webzjs-zcash-snap/).  
3. انقر على **تثبيت/إضافة إلى MetaMask**.
4. وافق على الأذونات مثل:
   ```
      Manage Zcash accounts 
      Store data on your device
   ```

![Zcash-snap-install](/content-images/Hy5MSG2Oex-42d0c5b346.webp)


---

## **2. (اختياري) أضف شبكة Zcash**

في MetaMask، اختر **إضافة شبكة** وأدخل:

بالنسبة لـ **BNB SmartChain**؛
```markdown
-  Name: BNB Smart Chain
-  RPC URL: https://bsc-dataseed.binance.org
-  Chain ID: 56
-  Symbol: BNB
-  Block Explorer URL: https://bscscan.com
```
يتيح ذلك عرض معلومات الشبكة وروابط المستكشف.
![Add-a-custom-Net....](/content-images/S1hq7f2Oel-e1ca8b9044.webp)

بالنسبة لـ **الشبكة الرئيسية لـ Zcash**؛
```markdown
- Name: Zcash Mainnet  
- RPC URL: https://zjs.zec.rocks 
- Symbol: ZEC
```
`https://zjs.zec.rocks` هو خادم lightwalletd متوافق مع WebZjs (gRPC-web) يديره [zec.rocks](https://zec.rocks) (@emersonian). للشبكة التجريبية، استخدم `https://zjs.zec.rocks/testnet`. إذا كنت تشغّل محفظة الويب WebZjs بنفسك، فهذه هي القيمة التي يجب تعيينها كـ `LIGHTWALLETD_PROXY`.

---

## **3. اتصل بمحفظة ChainSafe WebZjs**

1. زُر [webzjs.chainsafe.dev](https://webzjs.chainsafe.dev).  
2. انقر على **ربط MetaMask Snap**.  

![Zcash-web-wallet](/content-images/Sk8nSz3dgl-98ce36cc67.webp)

3. وافق على الاتصال.  
4. اعرض ملخص حساب Zcash، بما في ذلك:
   - العناوين الموحّدة والعنوان الشفاف (transparent)

![Account-summary-unif....](/content-images/r17c_Mhdel-f4963826d5.webp)


5. انتظر حتى تكتمل المزامنة.




---

## **4. موّل محفظتك**

> **مبادلة ETH -> ZEC** - استخدم خدمات مثل **LeoDex** وأرسل إلى عنوانك المحمي.  
> **السحب من منصة التداول** - اسحب ZEC الذي اشتريته إلى عنوان WebZjs المحمي الخاص بك.  

![LeoDex-SWAP](/content-images/HyLQ0G2ugg-8d82ef24f6.webp)


> => استخدم العناوين المحمية (z) للحصول على **خصوصية (privacy) كاملة**.

---

## **5. إرسال / استلام ZEC**

1. في **WebZjs**، انتقل إلى **تحويل الرصيد**.  
2. أدخل:
```
   - Shielded recipient address  
   - Amount
```
   ![Transfer-Balance](/content-images/rkvcFfhdex-bd55d079eb.webp)

4. أكّد المعاملة في MetaMask (وقّع المعاملة).  
5. ستظهر الأموال المستلمة في WebZjs بعد التأكيد.

---

## **6. التحقق / استكشاف الأخطاء وإصلاحها**

> تحقق من **WebZjs** للاطلاع على الأرصدة المحدّثة **(لم تُدرج MetaMask ZEC مباشرةً)** .  
> إذا واجهت مشكلات:
  ```
  - Confirm you have the official ChainSafe Snap.  
  - Check correct network settings.  
  - Ensure correct address format.  
  - Reconnect via **Connect Snap** if needed.
  ``` 

> **نصيحة أمان:** لا تثبّت إلا **ChainSafe Snap المدقَّقة**؛ وراجع الأذونات قبل الموافقة.

---

## **7. تحقق من مكونات العنوان**

1. انتقل إلى قسم **استلام** - سيُعرض عنوانك الموحّد افتراضياً.  
2. انسخ العنوان الموحّد وزُر [مستكشف كتل Zcash](https://mainnet.zcashexplorer.app/).  
3. الصق عنوانك الموحّد في شريط البحث.  
4. سترى الآن جميع مكونات العنوان الموحّد، والتي تشمل:
``` 
   Orchard Address  
   Sapling Address  
   Transparent Address
``` 

![Address-components](/content-images/SyPR2f2_gg-3907c5bf58.webp)



---

## **معلومات إضافية**

> استخدم [**أحدث إصدار من MetaMask**](https://chromewebstore.google.com/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn?hl=en) - الإصدار العام يدعم Snaps.  
> قد تستغرق الإثباتات المحمية بعض الوقت، إذ تتولى WebAssembly إجراء الحسابات داخل المتصفح.  
> الاسترداد بسيط: ثبّت MetaMask وـ Snap، ثم استورد عبارة البذور الحالية لديك.  
> تعتمد Snap افتراضياً على **ZEC المحمي**، والعناوين الشفافة **ليست محور التركيز**.  
> استخدم [mainnet.zcashexplorer.app](https://mainnet.zcashexplorer.app) لتأكيدات المعاملات.
