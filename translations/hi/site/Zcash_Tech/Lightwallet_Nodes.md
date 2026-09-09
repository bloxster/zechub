<a href="https://github.com/zechub/zechub/edit/main/site/Zcash_Tech/Lightwallet_Nodes.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>


# Zcash Lightwallet नोड

## संक्षेप में

* अधिकांश लोग Zcash को light wallet के माध्यम से उपयोग करते हैं, जो पूरी blockchain डाउनलोड नहीं करता। इसके बजाय, यह उस सर्वर से बात करता है जो यह काम पहले ही कर चुका होता है।
* आज light wallet को दो सॉफ्टवेयर सेवाएँ प्रदान करते हैं: **lightwalletd**, Go में लिखी मूल सेवा, और **Zaino**, Rust में लिखा एक नया indexer।
* आपकी keys कभी भी आपके डिवाइस से बाहर नहीं जातीं, और सर्वर आपके funds खर्च नहीं कर सकता या पूरी तरह shielded transactions के भीतर की राशि और memos नहीं पढ़ सकता।
* सर्वर आपके IP address और आपकी गतिविधि के समय के बारे में जानने की बेहतर स्थिति में होता है — shielded transactions blockchain पर होने वाली गतिविधि की रक्षा करते हैं, सर्वर से आपके connection की नहीं।
* Tor IP identifier को हटा देता है; यह `zcash_client_backend` पर बने wallets में उपलब्ध है, और ZODL में Advanced Settings का एक setting है।
* आप बदल सकते हैं कि आपका wallet किस सर्वर का उपयोग करे, या अपना सर्वर चला सकते हैं — lightwalletd और Zaino, दोनों open source हैं।

## मूल व्याख्या

अधिकांश लोग Zcash को light wallet के माध्यम से उपयोग करते हैं, जो पूरी blockchain डाउनलोड नहीं करता। इसके बजाय, यह उस सर्वर से बात करता है जो यह काम पहले ही कर चुका होता है। यह पृष्ठ समझाता है कि ये सर्वर क्या हैं, वे आपके बारे में क्या देख सकते हैं और क्या नहीं, अपने connection को Tor के माध्यम से कैसे route करें, और आपका wallet जिस सर्वर का उपयोग करता है उसे कैसे बदलें।

आज light wallet को दो सॉफ्टवेयर सेवाएँ प्रदान करते हैं। **lightwalletd** मूल सेवा है, जो Go में लिखी गई है। **Zaino** Rust में लिखा एक नया indexer है, जो zcashd deprecation कार्य के हिस्से के रूप में बनाया गया है।

### light wallet सर्वर क्या करता है

एक light wallet सर्वर आपके wallet और Zcash blockchain के बीच स्थित होता है और उसे chain का bandwidth-कुशल दृश्य देता है। यह आपके लिए तीन काम करता है।

यह compact blocks उपलब्ध कराता है। पूरे blocks के बजाय, यह एक संक्षिप्त रूप भेजता है जिसमें केवल वही होता है जिसकी wallet को अपने shielded address पर भुगतान पहचानने, अपनी notes के spend को पहचानने और अपने witnesses को अपडेट करने के लिए आवश्यकता होती है।

यह आपके transactions को relay करता है। जब आप भेजते हैं, तो आपका wallet पूर्ण transaction सर्वर को देता है, जो उसे नेटवर्क पर broadcast करता है।

यह chain queries का उत्तर देता है, जैसे वर्तमान height और आपके wallet के लिए आवश्यक fee जानकारी।

आपका wallet फिर भी निजी काम स्थानीय रूप से करता है। वह आपकी keys रखता है, आपकी notes खोजने के लिए blocks को trial-decrypt करता है, और आपके डिवाइस पर transactions बनाता और sign करता है।

### सर्वर क्या देख सकता है और क्या नहीं

यह वह हिस्सा है जिसे गलत समझना आसान है। आपकी keys कभी भी आपके डिवाइस से बाहर नहीं जातीं, लेकिन इसका अर्थ यह नहीं है कि सर्वर आपके बारे में कुछ भी नहीं सीखता।

यहाँ संदर्भ [Zcash wallet app threat model](https://zcash.readthedocs.io/en/latest/rtd_pages/wallet_threat_model.html) है, जिसे यदि आपको इसकी परवाह है तो पूरा पढ़ना उचित है। यह adversary के कई प्रकार बताता है। इस पृष्ठ के लिए महत्त्वपूर्ण adversary वह है जो आपके wallet और इंटरनेट के बीच, तथा सर्वर और इंटरनेट के बीच traffic देख सकता है। जो भी सर्वर चलाता है वह स्वाभाविक रूप से कुछ हद तक इसी स्थिति में होता है, क्योंकि आपका wallet सीधे उनसे connect होता है।

पहले देखें कि क्या सुरक्षित है। मॉडल के हर adversary के विरुद्ध, जिसमें वह भी शामिल है जिसने सर्वर compromise कर लिया हो, वह "can't learn any of the user's cryptographic key material (spending keys, viewing keys, seed phrase, etc.)", आपके funds चोरी नहीं कर सकता, और आपको ऐसे funds भेजने के लिए मजबूर नहीं कर सकता जिन्हें आप भेजना नहीं चाहते थे। पूरी तरह shielded transactions के भीतर की राशि और memos encrypted रहते हैं।

फिर वह है जो सुरक्षित नहीं है। threat model traffic-observing adversary के विरुद्ध इन्हें ज्ञात कमजोरियों के रूप में सूचीबद्ध करता है:

| कमजोरी | कैसे |
|:--|:--|
| यह बताना कि आप कौन हैं | "The adversary knows the user's IP address, which could lead them to the user's real identity" |
| यह बताना कि आप लगभग कहाँ हैं | आपके IP को "in a geolocation database to approximate their location" खोजकर |
| यह बताना कि आपने shielded transaction भेजा या प्राप्त किया, और कब किया | भेजने में "uses more bandwidth, which is visible even though the connection is encrypted"। मॉडल नोट करता है कि भेजने और प्राप्त करने की क्रिया स्वयं सर्वर को दिखाई देती है |
| समय के साथ आपके किए गए transactions की संख्या गिनना | वही bandwidth patterns, जिन्हें अधिक लंबे समय तक देखा गया हो |
| बार-बार होने वाले payment patterns को पहचानना | गतिविधि कब होती है, यह देखकर |
| यह पता लगाना कि कोई address आपका है या नहीं | ऐसा adversary जो पहले से किसी address को जानता है, "could send funds to that address and watch to see if there are bandwidth spikes" जब आपका wallet उसे प्राप्त करता है |

मॉडल यह भी नोट करता है कि सामान्य स्थिति में "a trust relationship between the user and the lightwalletd server operator" माना जाता है।

इसलिए ईमानदार सार यह है। light wallet सर्वर आपके पैसे खर्च नहीं कर सकता और आपके shielded transactions की राशि या memos नहीं पढ़ सकता। वह आपके IP address और आपकी गतिविधि के समय के बारे में जानने की बेहतर स्थिति में होता है, और ये दोनों मिलकर किसी व्यक्ति के बारे में बहुत कुछ बता सकते हैं। shielded transactions blockchain पर होने वाली गतिविधि की रक्षा करते हैं। वे अपने-आप सर्वर से आपके connection को नहीं छिपाते।

## दृश्य / सादृश्य

एक सार्वजनिक पुस्तकालय की कल्पना करें जिसमें अब तक छपा हर समाचारपत्र रखा है। एक full नोड वह पाठक है जो पूरा archive घर ले जाता है। एक light wallet वह पाठक है जो इसके बजाय librarian से दैनिक सारांश माँगता है — एक पतली शीट जिसमें इतना ही हो कि वह पहचान सके कि क्या कुछ उससे संबंधित है।

सारांश sealed होता है: librarian इसे इस बात को पढ़े बिना तैयार करता है कि कौन-सी चीजें आपके लिए महत्त्वपूर्ण हैं, और आप इसे अपनी key से घर पर खोलते हैं। यही compact block है, और खोलना आपके डिवाइस पर trial-decryption है।

लेकिन librarian फिर भी देखता है कि कौन-सा पाठक आया, किस समय आया, और वह कितना मोटा bundle लेकर गया। यही IP address और timing है — desk से दिखाई देने वाली चीजें, envelope कितना भी अच्छी तरह sealed हो। Tor एक anonymous courier भेजने के समान है: librarian वही bundle देता है, लेकिन अब उसे नहीं पता कि वह किसके घर जा रहा है।

## विस्तृत जानकारी

### Tor के माध्यम से routing

Tor आपके IP address और आपके wallet traffic के बीच का संबंध तोड़ता है, जिससे ऊपर की तालिका में दिया सबसे मज़बूत identifier हट जाता है।

कई Zcash wallets जिन Rust libraries पर बने हैं, उनमें इसका support मौजूद है। zcash_client_backend में [Arti](https://tpo.pages.torproject.net/core/arti/) पर निर्मित Tor module शामिल है, जो Tor का Rust implementation है, इसलिए कोई wallet अलग Tor client शामिल किए बिना sync, transaction broadcast और price lookups को Tor के माध्यम से route कर सकता है।

Zaino developers भी यही तर्क देते हैं और सीधे threat model का हवाला देते हैं: "a need to use anonymous transport protocols (such as Nym or Tor) to obfuscate clients' identities from Zcash's indexing servers"।

**ZODL** में Tor, Advanced Settings का एक setting है। wallet की release notes उपयोगकर्ताओं को manual connection mode "plus enabling Tor in Advanced Settings" की ओर निर्देशित करती हैं यदि वे "prefer to reduce metadata exposure", और app wallet restore करने से पहले Tor चालू करने का विकल्प देता है, जो वह क्षण है जब अन्यथा एक नया IP पूरे wallet history से जुड़ सकता है।

दो सावधानियाँ। Tor सर्वर से आपका IP छिपाता है, लेकिन आपके किए गए requests से सर्वर जो सीखता है उसे नहीं बदलता। और onion routing latency बढ़ाती है, इसलिए syncing में अधिक समय लगता है। अपना सर्वर चलाना trust के प्रश्न से अलग ढंग से बचाता है, क्योंकि तब operator आप स्वयं होते हैं।

### Zaino, Rust indexer

[Zaino](/zcash-tech/zaino), Zingo team द्वारा Rust में लिखा एक indexer है, जिसे zcashd deprecation कार्य के हिस्से के रूप में lightwalletd को बदलने के लिए बनाया गया है। यह light clients, full clients और block explorers को सेवा देता है, तथा "either a Zebra or Zcashd full validator" द्वारा रखे गए chain data को पढ़ता है।

यह सक्रिय विकास के अधीन है, और संस्करण 0.8.0 अगस्त 2026 में जारी हुआ था। इसका लक्ष्य जहाँ संभव हो वहाँ lightwalletd के साथ backward compatible रहना है, ताकि wallets को दोबारा लिखे बिना उन्हें इसकी ओर point किया जा सके।

Zaino का architecture diagrams सहित अपना पृष्ठ है, इसलिए यह पृष्ठ केवल light wallet सर्वर के रूप में इसकी भूमिका को कवर करता है।

### अपना सर्वर चलाना

सबसे मज़बूत विकल्प स्वयं operator बनना है, जो trust का प्रश्न पूरी तरह हटा देता है। दोनों सर्वर open source हैं: Go में [lightwalletd](https://github.com/zcash/lightwalletd) और Rust में [Zaino](https://github.com/zingolabs/zaino)। दोनों एक full validator से पढ़ते हैं, इसलिए आपको [Zebra](/zcash-tech/zebra-full-node) भी चाहिए होगा।

## व्यावहारिक निहितार्थ

### सर्वर सूची

[hosh.zec.rocks](https://hosh.zec.rocks/zec) dashboard public servers और उनकी health को track करता है, और वास्तव में क्या चालू है यह देखने का स्थान है। [status.zec.rocks](https://status.zec.rocks/) service status दिखाता है।

लिखने के समय उस dashboard पर सूचीबद्ध सर्वर:

| सर्वर | टिप्पणियाँ |
|:--|:--|
| zec.rocks:443 | इसके साथ regional endpoints na.zec.rocks, eu.zec.rocks, ap.zec.rocks और sa.zec.rocks पर सूचीबद्ध हैं |
| zec-node.cakewallet.com:443 | Cake Wallet के domain पर |
| zec.0xrpc.io:443 | 0xRPC द्वारा संचालित, जो कई chains के लिए मुफ्त public endpoints उपलब्ध कराता है और capacity पूरी करने के लिए donations माँगता है |
| zaino.unsafe.zec.rocks:443 | एक Zaino instance। hostname पर ध्यान दें, इसे experimental मानें |
| testnet.zec.rocks:443 | Testnet, जिसमें Zaino testnet instance zaino.testnet.unsafe.zec.rocks पर सूचीबद्ध है |

इस सूची पर भरोसा करने के बजाय dashboard देखें। Operators आते-जाते रहते हैं और ऐसा पृष्ठ पुराना हो जाता है।

### अपने wallet में सर्वर बदलना

यह तब उपयोगी है जब आप किसी ऐसे operator को चुनना चाहते हैं जिस पर आप भरोसा करते हैं, activity को operators में बाँटना चाहते हैं, या अपने सर्वर की ओर point करना चाहते हैं।

नीचे दिए menu paths इस पृष्ठ के अपडेट होने के समय सही थे, लेकिन wallet interfaces बदलते रहते हैं, इसलिए इन्हें सटीक रास्ते के बजाय संकेत मानें। Advanced Settings या server option खोजें।

#### ZODL

पहले Zashi था। ऊपर दाएँ कोने में cog, फिर Advanced Settings। Tor उसी screen में है। यदि sync failure सर्वर के out of date होने के कारण हो, तो ZODL Switch server shortcut भी देता है।

#### Ywallet

ऊपर दाएँ कोने में cog, फिर Zcash tab।

![Ywallet server settings](/content-images/b0a2910b-dbdf-4292-8e69-af5a386aa183-f51f098d19.webp)

#### Zingo

ऊपर बाएँ कोने में hamburger menu, फिर Settings, फिर नीचे scroll करें।

![Zingo server settings](/content-images/ea8f7672-e644-41a5-a422-db131740404a-2626f5fa79.webp)

#### eZcash

ऊपर बाएँ कोने में hamburger menu, फिर Settings, फिर Advanced।

![eZcash server settings](/content-images/655c0172-61a0-4322-b8cf-4eee4bb53b51-0b93df2e71.webp)

वे screenshots मार्च 2025 में लिए गए थे, और apps तब से releases जारी कर चुके हैं, इसलिए buttons स्थान बदल चुके हो सकते हैं।

## सामान्य गलतियाँ

**यह सोचना कि सर्वर आपके transactions पढ़ सकता है**। वह नहीं पढ़ सकता। आपकी keys आपके डिवाइस पर रहती हैं, और पूरी तरह shielded transactions के भीतर की राशि और memos encrypted रहते हैं — उस adversary के विरुद्ध भी जिसने सर्वर compromise कर लिया हो।

**"shielded" को "anonymous connection" समझना**। shielded transactions blockchain पर होने वाली गतिविधि की रक्षा करते हैं। आपका IP address और आपकी गतिविधि का समय अलग layer हैं, और सर्वर ठीक यही देखता है।

**यह मानना कि Tor हर निशान हटा देता है**। Tor सर्वर से आपका IP छिपाता है, लेकिन आपके किए गए requests से सर्वर जो सीखता है उसे नहीं बदलता, और syncing में latency जोड़ता है।

**wiki पृष्ठ की सर्वर सूची पर भरोसा करना**। Operators आते-जाते रहते हैं। अपने wallet को किसी भी चीज़ की ओर point करने से पहले वास्तव में क्या चल रहा है, यह देखने के लिए [hosh.zec.rocks](https://hosh.zec.rocks/zec) देखें।

## सारांश

light wallets आपको disk space के बिना shielded pool देते हैं, जो एक अच्छा trade है। बस यह स्पष्ट रखें कि आप किसका trade कर रहे हैं। सर्वर आपके funds नहीं ले सकता या आपकी shielded राशियाँ नहीं पढ़ सकता, लेकिन वह आपका IP address और आपके transaction का समय देखने की बेहतर स्थिति में होता है। Tor के माध्यम से route करें, जानबूझकर अपना operator चुनें, या अपना सर्वर चलाएँ।

## संबंधित पृष्ठ

- [कौन आपका Zcash भुगतान देख सकता है](/start-here/who-can-see-your-zcash-payment) — उसी प्रश्न का शुरुआती स्तर का दृष्टिकोण।
- [Block Explorer क्या देख सकता है](/zcash-tech/what-a-block-explorer-can-see) — सर्वर पर दिखाई देने वाली चीज़ों के विपरीत, on-chain क्या दिखाई देता है।
- [Zaino](/zcash-tech/zaino) — architecture diagrams और Rust indexer की व्यापक भूमिका।
- [Zebra Full Node](/zcash-tech/zebra-full-node) — वह validator जिससे light wallet सर्वर पढ़ता है।
- [Zcash Wallet Syncing](/zcash-tech/zcash-wallet-syncing) — सर्वर द्वारा भेजे गए compact blocks को आपका wallet कैसे process करता है।

**अंतिम अपडेट:** अगस्त 2026
