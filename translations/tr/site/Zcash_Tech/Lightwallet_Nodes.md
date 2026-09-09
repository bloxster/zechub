<a href="https://github.com/zechub/zechub/edit/main/site/Zcash_Tech/Lightwallet_Nodes.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>


# Zcash Lightwallet Düğümleri

## Kısaca

* Çoğu kişi Zcash'i, blokzincirin tamamını indirmeyen bir hafif cüzdan aracılığıyla kullanır. Bunun yerine, bu işi zaten yapmış bir sunucuyla iletişim kurar.
* Günümüzde hafif cüzdanlara iki yazılım hizmet verir: Go ile yazılmış özgün hizmet **lightwalletd** ve Rust ile yazılmış daha yeni bir indeksleyici olan **Zaino**.
* Anahtarlarınız cihazınızdan asla ayrılmaz; sunucu fonlarınızı harcayamaz veya tamamen korumalı işlemlerin içindeki tutarları ve notları okuyamaz.
* Sunucunun öğrenmek için elverişli olduğu şey IP adresiniz ve etkinliğinizin zamanlamasıdır — korumalı işlemler blokzincirde olup biteni korur, sunucuyla bağlantınızı değil.
* Tor, IP tanımlayıcısını ortadan kaldırır; `zcash_client_backend` üzerine inşa edilen cüzdanlarda kullanılabilir ve ZODL'de Gelişmiş Ayarlar içinde bir ayardır.
* Cüzdanınızın kullandığı sunucuyu değiştirebilir veya kendiniz çalıştırabilirsiniz — hem lightwalletd hem de Zaino açık kaynaklıdır.

## Temel Açıklama

Çoğu kişi Zcash'i, blokzincirin tamamını indirmeyen bir hafif cüzdan aracılığıyla kullanır. Bunun yerine, bu işi zaten yapmış bir sunucuyla iletişim kurar. Bu sayfa, bu sunucuların ne olduğunu, sizin hakkınızda neleri görüp göremeyeceklerini, bağlantınızı Tor üzerinden nasıl yönlendireceğinizi ve cüzdanınızın kullandığı sunucuyu nasıl değiştireceğinizi açıklar.

Günümüzde hafif cüzdanlara iki yazılım hizmet verir. **lightwalletd**, Go ile yazılmış özgün hizmettir. **Zaino**, zcashd kullanım dışı bırakma çalışmasının bir parçası olarak geliştirilen, Rust ile yazılmış daha yeni bir indeksleyicidir.

### Hafif cüzdan sunucusu ne yapar?

Bir hafif cüzdan sunucusu, cüzdanınız ile Zcash blokzinciri arasında yer alır ve cüzdanınıza zincirin bant genişliği açısından verimli bir görünümünü sunar. Sizin için üç şey yapar.

Sıkıştırılmış blokları sunar. Tam bloklar yerine, bir cüzdanın korumalı adresine gelen ödemeyi algılaması, notlarının harcanmasını algılaması ve tanıklarını güncellemesi için gerekenleri taşıyan sıkıştırılmış bir biçim gönderir.

İşlemlerinizi iletir. Gönderim yaptığınızda cüzdanınız tamamlanmış işlemi sunucuya verir; sunucu da bunu ağa yayınlar.

Mevcut yükseklik ve cüzdanınızın ihtiyaç duyduğu ücret bilgisi gibi zincir sorgularını yanıtlar.

Cüzdanınız gizli işleri yine yerelde yapar. Anahtarlarınızı tutar, notlarınızı bulmak için blokları deneme amaçlı şifre çözer ve cihazınızda işlemler oluşturup imzalar.

### Sunucu neleri görebilir ve göremez?

Yanlış anlaması kolay olan kısım burasıdır. Anahtarlarınız cihazınızdan asla ayrılmaz, ancak bu sunucunun sizin hakkınızda hiçbir şey öğrenmediği anlamına gelmez.

Buradaki referans, bu konu sizin için önemliyse tamamını okumanıza değer olan [Zcash cüzdan uygulaması tehdit modeli](https://zcash.readthedocs.io/en/latest/rtd_pages/wallet_threat_model.html)'dir. Model, çeşitli saldırgan türlerini ortaya koyar. Bu sayfa açısından önemli olan, cüzdanınız ile internet arasındaki ve sunucu ile internet arasındaki trafiği izleyebilen bir saldırgandır. Sunucuyu işleten kişi, cüzdanınız doğrudan ona bağlandığı için doğası gereği kısmen bu konumdadır.

Korunanlarla başlayalım. Modeldeki her saldırgana karşı, sunucuyu ele geçirmiş olan biri de dahil olmak üzere, "kullanıcının hiçbir kriptografik anahtar materyalini (harcama anahtarları, görüntüleme anahtarları, seed phrase vb.) öğrenemez", fonlarınızı çalamaz ve istemediğiniz fonları göndermenize neden olamaz. Tamamen korumalı işlemlerdeki tutarlar ve notlar şifreli kalır.

Ardından korunmayanlar gelir. Tehdit modeli bunları, trafiği gözlemleyen bir saldırgana karşı bilinen zayıflıklar olarak listeler:

| Zayıflık | Nasıl |
|:--|:--|
| Kim olduğunuzu belirleme | "Saldırgan, kullanıcının IP adresini bilir; bu da kullanıcının gerçek kimliğine ulaşmalarına yol açabilir" |
| Yaklaşık olarak nerede olduğunuzu belirleme | IP adresinizi, "konumlarını yaklaşık olarak belirlemek için bir coğrafi konum veritabanında" aramak |
| Korumalı bir işlemi gönderip aldığınızı ve bunun ne zaman olduğunu belirleme | Gönderim, "bağlantı şifreli olsa bile görünür olan daha fazla bant genişliği kullanır". Model, gönderme ve alma eyleminin sunucunun kendisi tarafından görülebildiğini belirtir |
| Zaman içinde kaç işlem yaptığınızı sayma | Daha uzun bir süre boyunca gözlemlenen aynı bant genişliği örüntüleri |
| Tekrarlayan ödeme örüntülerini saptama | Etkinliğin ne zaman gerçekleştiğini gözlemlemek |
| Bir adresin size ait olup olmadığını anlama | Bir adresi zaten bilen bir saldırgan, "o adrese fon gönderebilir ve cüzdanınızdan bunu getirmesi sırasında bant genişliği sıçramaları olup olmadığını izleyebilir" |

Model ayrıca olağan durumda "kullanıcı ile lightwalletd sunucusu işletmecisi arasında bir güven ilişkisi" varsayıldığını belirtir.

Dolayısıyla dürüst özet şudur: Bir hafif cüzdan sunucusu paranızı harcayamaz ve korumalı işlemlerinizdeki tutarları veya notları okuyamaz. Öğrenmek için elverişli olduğu şey IP adresiniz ve etkinliğinizin zamanlamasıdır; bu ikisi birlikte bir kişi hakkında çok şey söyleyebilir. Korumalı işlemler blokzincirde olup biteni korur. Tek başlarına, sunucuyla bağlantınızı gizlemezler.

## Görsel / Benzetme

Şimdiye kadar basılmış her gazeteyi tutan bir halk kütüphanesi düşünün. Tam düğüm, tüm arşivi eve götüren bir okuyucudur. Hafif cüzdan ise bunun yerine kütüphaneciden günlük bir özet isteyen bir okuyucudur — kendisiyle ilgili bir şey olup olmadığını anlamaya yetecek kadar bilgi taşıyan ince bir sayfa.

Özet mühürlüdür: Kütüphaneci hangi öğelerin sizin için önemli olduğunu okuyamadan onu hazırlar ve siz onu evde kendi anahtarınızla açarsınız. Bu, sıkıştırılmış bloktur; açma işlemi de cihazınızdaki deneme amaçlı şifre çözmedir.

Ancak kütüphaneci yine de hangi okuyucunun içeri girdiğini, ne zaman geldiğini ve dışarı ne kadar kalın bir paket taşıdığını görür. Bu, zarf ne kadar iyi mühürlenmiş olursa olsun masadan görülebilen IP adresi ve zamanlamadır. Tor, anonim bir kurye göndermenin karşılığıdır: kütüphaneci aynı paketi teslim eder, ancak artık paketin hangi eve gittiğini bilmez.

## Derinlemesine İnceleme

### Tor üzerinden yönlendirme

Tor, IP adresiniz ile cüzdan trafiğiniz arasındaki bağlantıyı keser; bu da yukarıdaki tablodaki en güçlü tanımlayıcıyı ortadan kaldırır.

Birçok Zcash cüzdanının üzerine inşa edildiği Rust kütüphanelerinde destek bulunmaktadır. zcash_client_backend, Tor'un Rust uygulaması olan [Arti](https://tpo.pages.torproject.net/core/arti/) üzerine kurulmuş bir Tor modülü içerir; böylece bir cüzdan, ayrı bir Tor istemcisi dağıtmadan senkronizasyonu, işlem yayınını ve fiyat sorgularını Tor üzerinden yönlendirebilir.

Zaino geliştiricileri, doğrudan tehdit modeline atıf yaparak aynı görüşü savunur: "istemcilerin kimliklerini Zcash'in indeksleme sunucularından gizlemek için anonim taşıma protokollerinin (Nym veya Tor gibi) kullanılması ihtiyacı" vardır.

**ZODL** içinde Tor, Gelişmiş Ayarlar'da bulunan bir ayardır. Cüzdanın sürüm notları, "meta veri maruziyetini azaltmayı tercih eden" kullanıcılara "Gelişmiş Ayarlar'da Tor'u etkinleştirmenin yanı sıra" manuel bağlantı modunu önerir; ayrıca uygulama, yeni bir IP'nin aksi hâlde tüm cüzdan geçmişiyle ilişkilendirileceği an olan cüzdan geri yükleme işleminden önce Tor'u açmayı teklif eder.

İki uyarı. Tor, IP'nizi sunucudan gizler, ancak yaptığınız isteklerden sunucunun öğrendiklerini değiştirmez. Ayrıca onion yönlendirme gecikme ekler; dolayısıyla senkronizasyon daha uzun sürer. Kendi sunucunuzu çalıştırmak güven meselesini farklı şekilde ortadan kaldırır, çünkü bu durumda işletmeci siz olursunuz.

### Rust indeksleyicisi Zaino

[Zaino](/zcash-tech/zaino), Zingo ekibi tarafından Rust ile yazılmış, zcashd kullanım dışı bırakma çalışmasının bir parçası olarak lightwalletd'nin yerini alması için geliştirilen bir indeksleyicidir. Hafif istemcilere, tam istemcilere ve blok gezginlerine hizmet verir; "Zebra veya Zcashd tam doğrulayıcısının herhangi biri tarafından tutulan" zincir verilerini okur.

Ağustos 2026'da yayımlanan 0.8.0 sürümüyle aktif geliştirme altındadır. Mümkün olduğunda lightwalletd ile geriye dönük uyumlu kalmayı hedefler; böylece cüzdanlar yeniden yazılmadan ona yönlendirilebilir.

Zaino'nun mimari diyagramlarını içeren kendi sayfası vardır; bu nedenle bu sayfa yalnızca hafif cüzdan sunucusu olarak rolünü ele alır.

### Kendi sunucunuzu çalıştırma

En güçlü seçenek, güven meselesini tamamen ortadan kaldıran kendi işletmeciniz olmaktır. Her iki sunucu da açık kaynaklıdır: Go ile yazılmış [lightwalletd](https://github.com/zcash/lightwalletd) ve Rust ile yazılmış [Zaino](https://github.com/zingolabs/zaino). Her ikisi de tam doğrulayıcıdan okur; bu nedenle [Zebra](/zcash-tech/zebra-full-node)'ya da ihtiyaç duyarsınız.

## Pratik Çıkarımlar

### Sunucu listesi

[hosh.zec.rocks](https://hosh.zec.rocks/zec) panosu herkese açık sunucuları ve sağlık durumlarını izler; gerçekten hangi sunucuların çalışır durumda olduğunu kontrol etmek için başvurulacak yerdir. [status.zec.rocks](https://status.zec.rocks/) hizmet durumunu gösterir.

Yazım sırasında bu panoda listelenen sunucular:

| Sunucu | Notlar |
|:--|:--|
| zec.rocks:443 | Bunun yanında na.zec.rocks, eu.zec.rocks, ap.zec.rocks ve sa.zec.rocks bölgesel uç noktaları listelenir |
| zec-node.cakewallet.com:443 | Cake Wallet'ın alan adında |
| zec.0xrpc.io:443 | Birden çok zincir için ücretsiz herkese açık uç noktalar sunan ve kapasite maliyetlerini karşılamak için bağış isteyen 0xRPC tarafından işletilir |
| zaino.unsafe.zec.rocks:443 | Bir Zaino örneği. Ana makine adına dikkat edin; deneysel olarak değerlendirin |
| testnet.zec.rocks:443 | Testnet; zaino.testnet.unsafe.zec.rocks adresinde listelenmiş bir Zaino testnet örneğiyle birlikte |

Bu listeye güvenmek yerine panoyu kontrol edin. İşletmeciler gelir ve gider; böyle bir sayfa zamanla eskir.

### Cüzdanınızdaki sunucuyu değiştirme

Güvendiğiniz bir işletmeciyi seçmek, etkinliği işletmeciler arasında dağıtmak veya kendi sunucunuza yönlendirmek istiyorsanız yapmaya değer.

Aşağıdaki menü yolları bu sayfa güncellendiğinde doğruydu, ancak cüzdan arayüzleri değişebilir; bu nedenle onları kesin bir yol yerine ipucu olarak değerlendirin. Gelişmiş Ayarlar veya bir sunucu seçeneği arayın.

#### ZODL

Eskiden Zashi olarak biliniyordu. Sağ üst köşedeki dişli simgesi, ardından Gelişmiş Ayarlar. Tor aynı ekranda bulunur. ZODL ayrıca, senkronizasyon hatasına sunucunun güncel olmaması neden olduğunda Sunucuyu değiştir kısayolu sunar.

#### Ywallet

Sağ üst köşedeki dişli simgesi, ardından Zcash sekmesi.

![Ywallet sunucu ayarları](/content-images/b0a2910b-dbdf-4292-8e69-af5a386aa183-f51f098d19.webp)

#### Zingo

Sol üst köşedeki hamburger menü, ardından Ayarlar ve sonra aşağı kaydırın.

![Zingo sunucu ayarları](/content-images/ea8f7672-e644-41a5-a422-db131740404a-2626f5fa79.webp)

#### eZcash

Sol üst köşedeki hamburger menü, ardından Ayarlar ve sonra Gelişmiş.

![eZcash sunucu ayarları](/content-images/655c0172-61a0-4322-b8cf-4eee4bb53b51-0b93df2e71.webp)

Bu ekran görüntüleri Mart 2025'te alındı ve uygulamalar o zamandan beri sürümler yayımladı; dolayısıyla düğmeler yer değiştirmiş olabilir.

## Yaygın Hatalar

**Sunucunun işlemlerinizi okuyabileceğini düşünmek**. Okuyamaz. Anahtarlarınız cihazınızda kalır ve tamamen korumalı işlemlerin içindeki tutarlar ve notlar — sunucuyu ele geçirmiş bir saldırgana karşı bile — şifreli kalır.

**"Korumalı"yı "anonim bağlantı" olarak okumak**. Korumalı işlemler blokzincirde olup biteni korur. IP adresiniz ve etkinliğinizin zamanlaması ayrı bir katmandır; sunucunun gördüğü katman da tam olarak budur.

**Tor'un her izi ortadan kaldırdığını varsaymak**. Tor, IP'nizi sunucudan gizler; ancak yaptığınız isteklerden sunucunun öğrendiklerini değiştirmez ve senkronizasyona gecikme ekler.

**Bir wiki sayfasındaki sunucu listesine güvenmek**. İşletmeciler gelir ve gider. Cüzdanınızı herhangi bir şeye yönlendirmeden önce gerçekten neyin çalıştığını görmek için [hosh.zec.rocks](https://hosh.zec.rocks/zec)'u kontrol edin.

## Özet

Hafif cüzdanlar, disk alanına gerek duymadan size korumalı havuzu sunar; bu iyi bir takastır. Yalnızca neyi takas ettiğiniz konusunda net olun. Sunucu fonlarınızı alamaz veya korumalı tutarlarınızı okuyamaz, ancak IP adresinizi ve ne zaman işlem yaptığınızı görmek için elverişli bir konumdadır. Tor üzerinden yönlendirin, işletmecinizi bilinçli seçin veya kendi sunucunuzu çalıştırın.

## İlgili Sayfalar

- [Zcash Ödemenizi Kimler Görebilir](/start-here/who-can-see-your-zcash-payment) — aynı sorunun başlangıç seviyesindeki görünümü.
- [Bir Blok Gezgini Neleri Görebilir](/zcash-tech/what-a-block-explorer-can-see) — sunucu düzeyinde görünenlerin aksine, zincir üzerinde görünenler.
- [Zaino](/zcash-tech/zaino) — mimari diyagramlar ve Rust indeksleyicisinin daha geniş rolü.
- [Zebra Tam Düğümü](/zcash-tech/zebra-full-node) — hafif cüzdan sunucusunun okuduğu doğrulayıcı.
- [Zcash Cüzdan Senkronizasyonu](/zcash-tech/zcash-wallet-syncing) — bir sunucunun gönderdiği sıkıştırılmış blokların cüzdanınız tarafından nasıl işlendiği.

**Son güncelleme:** Ağustos 2026
