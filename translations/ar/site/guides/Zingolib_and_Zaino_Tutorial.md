# Z3: (zebrad)(zaino)(zingo-cli)

**zebrad**    : عقدة كاملة (full node) لـ zcash

**zaino**     : مُفهرِس سلسلة الكتل (blockchain) لـ zcash

**zingo-cli** : عميل zaino-proxy لـ zcash يعمل من سطر الأوامر (مجموعة فرعية من Zingolib)

## الفيديو

<div className="my-8 w-full aspect-video max-w-3xl mx-auto rounded-2xl overflow-hidden shadow-lg bg-black">
  <iframe
    className="w-full h-full"
    src="https://www.youtube.com/embed/b5dIuGstMvI"
    title="An introduction to Zingolib + Zaino"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowFullScreen
    loading="lazy"
  />
</div>


## الصورة الكاملة

[بنية النظام](https://github.com/zingolabs/zaino/blob/dev/docs/zaino_live_system_architecture.pdf)


- Zcash يثبّت المستخدم Zingolib أو يجمّعه، وهو ما يمنحه الوصول إلى zingo-cli. ويمكنه إرسال ZEC واستقبالها حسب الحاجة.
- يتصل zingo-cli بـ zaino إما محليًا أو عبر قناة آمنة على الإنترنت (لا يهتم مستخدم Zcash بكيفية عمل ذلك!)
- يتيح Zaino الوصول إلى zebrad أو zcashd            
- تُعدّ عقدة zebrad المتزامنة بالكامل مصدر الحقيقة (لا مزيد من المحافظ (wallet) هنا!)



## التثبيت

ستحتاج إلى تثبيت 3 أشياء حتى يعمل هذا بشكل صحيح. كما أوصيك باستخدام screen أو أداة مشابهة للمساعدة في إدارة الشاشات

`sudo apt install screen`

### zebrad

```
git clone https://github.com/ZcashFoundation/zebra.git
cd zebra
cargo install --git https://github.com/ZcashFoundation/zebra --tag v7.0.0 zebrad
```

استخدم Zebra 7.0.0 أو أحدث: فقد فُعِّل NU7 على Mainnet عند الكتلة (block) 3,543,405، ولا تستطيع إصدارات `zebrad` الأقدم متابعة السلسلة (chain) بعد هذا الارتفاع ([تفاصيل الإصدار](https://github.com/ZcashFoundation/zebra/releases/tag/v7.0.0)).

 
*اختياري* (أنشئ جلسة screen لـ zebrad)

```
screen -S zebra
zebrad start
```

تنبيه: سيحتاج هذا إلى مزامنة كاملة! 

### zaino

```
git clone https://github.com/zingolabs/zaino.git
cd zaino
cargo build --release
PATH=$PATH:~/Desktop/zaino/target/release/
```


*اختياري* (أنشئ جلسة screen لـ zaino)

```
screen -S zaino
cd ~/zaino/zainod
nano zindexer.toml  => Adjust port to 8232 for mainnet
zainod --config zindexer.toml
```


### zingo-cli

```
git clone https://github.com/zingolabs/zingolib.git
cd zingolib
cargo build --release --package zingo-cli
```

*اختياري* (أنشئ جلسة screen لـ zingo-cli)

```
screen -S zingo
./zingo-cli --server http://127.0.0.1:8137 --data-dir /media/zebra5/zebra/.cache/lightwalletd
```

تنبيه: سيحتاج هذا إلى مزامنة كاملة، تمامًا كما فعل lightwalletd. أوصيك باستخدام قرص خارجي لتوفير الوقت :)


## التشغيل

إذا كنت تشغّل هذه البرامج داخل جلسات screen، فإن `screen -r` سيعرض لك قائمة بكل جلسة لتنتقل إليها حسب الحاجة
