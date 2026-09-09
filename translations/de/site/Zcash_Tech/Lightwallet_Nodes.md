<a href="https://github.com/zechub/zechub/edit/main/site/Zcash_Tech/Lightwallet_Nodes.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>


# Zcash-Lightwallet-Knoten

## TL;DR

* Die meisten Menschen nutzen Zcash über eine Light Wallet, die nicht die gesamte blockchain herunterlädt. Stattdessen kommuniziert sie mit einem Server, der diese Arbeit bereits erledigt hat.
* Zwei Softwareprogramme bedienen heute Light Wallets: **lightwalletd**, der ursprüngliche, in Go geschriebene Dienst, und **Zaino**, ein neuerer, in Rust geschriebener Indexer.
* Deine Schlüssel verlassen niemals dein Gerät, und der Server kann weder deine Gelder ausgeben noch die Beträge und Memos in vollständig abgeschirmten Transaktionen lesen.
* Was der Server gut herausfinden kann, sind deine IP-Adresse und der Zeitpunkt deiner Aktivität — abgeschirmte Transaktionen schützen, was auf der blockchain geschieht, nicht deine Verbindung zum Server.
* Tor entfernt den IP-Identifier; es ist in Wallets verfügbar, die auf `zcash_client_backend` aufbauen, und in ZODL ist es eine Einstellung unter Erweiterte Einstellungen.
* Du kannst ändern, welchen Server deine Wallet verwendet, oder deinen eigenen betreiben — sowohl lightwalletd als auch Zaino sind Open Source.

## Grundlegende Erklärung

Die meisten Menschen nutzen Zcash über eine Light Wallet, die nicht die gesamte blockchain herunterlädt. Stattdessen kommuniziert sie mit einem Server, der diese Arbeit bereits erledigt hat. Diese Seite erklärt, was diese Server sind, was sie über dich sehen können und was nicht, wie du deine Verbindung über Tor leitest und wie du den Server wechselst, den deine Wallet verwendet.

Zwei Softwareprogramme bedienen heute Light Wallets. **lightwalletd** ist der ursprüngliche, in Go geschriebene Dienst. **Zaino** ist ein neuerer, in Rust geschriebener Indexer, der im Rahmen der Ablösung von zcashd entwickelt wurde.

### Was ein Light-Wallet-Server macht

Ein Light-Wallet-Server sitzt zwischen deiner Wallet und der Zcash-blockchain und bietet ihr eine bandbreiteneffiziente Ansicht der Kette. Er erledigt drei Dinge für dich.

Er stellt kompakte Blöcke bereit. Statt ganzer Blöcke sendet er eine kompakte Form, die nur das enthält, was eine Wallet benötigt, um eine Zahlung an ihre abgeschirmte Adresse zu erkennen, eine Ausgabe ihrer Notes zu erkennen und ihre Witnesses zu aktualisieren.

Er leitet deine Transaktionen weiter. Wenn du sendest, übergibt deine Wallet die fertige Transaktion dem Server, der sie an das Netzwerk überträgt.

Er beantwortet Abfragen zur Kette, etwa zur aktuellen Höhe und zu den Gebühreninformationen, die deine Wallet benötigt.

Deine Wallet erledigt die private Arbeit weiterhin lokal. Sie verwahrt deine Schlüssel, entschlüsselt Blöcke probeweise, um deine Notes zu finden, und erstellt und signiert Transaktionen auf deinem Gerät.

### Was der Server sehen kann und was nicht

Dieser Teil wird leicht missverstanden. Deine Schlüssel verlassen niemals dein Gerät, doch das bedeutet nicht, dass der Server nichts über dich erfährt.

Die Referenz hierfür ist das [Threat Model der Zcash-Wallet-App](https://zcash.readthedocs.io/en/latest/rtd_pages/wallet_threat_model.html), das du vollständig lesen solltest, wenn dir das wichtig ist. Es beschreibt mehrere Arten von Angreifern. Für diese Seite relevant ist ein Angreifer, der den Verkehr zwischen deiner Wallet und dem Internet sowie zwischen dem Server und dem Internet beobachten kann. Wer den Server betreibt, befindet sich zwangsläufig teilweise in dieser Position, weil deine Wallet sich direkt mit ihm verbindet.

Beginnen wir mit dem, was geschützt ist. Gegen jeden Angreifer im Modell, einschließlich eines Angreifers, der den Server kompromittiert hat, kann er „keines der kryptografischen Schlüsselmaterialien des Nutzers (Ausgabeschlüssel, Viewing Keys, Seed-Phrase usw.) erfahren“, deine Gelder nicht stehlen und dich nicht dazu bringen, Gelder zu senden, die du nicht senden wolltest. Die Beträge und Memos in vollständig abgeschirmten Transaktionen bleiben verschlüsselt.

Dann gibt es das, was nicht geschützt ist. Das Threat Model führt diese bekannten Schwächen gegenüber einem den Datenverkehr beobachtenden Angreifer auf:

| Schwäche | Wie |
|:--|:--|
| Herausfinden, wer du bist | „Der Angreifer kennt die IP-Adresse des Nutzers, was zu dessen echter Identität führen könnte“ |
| Grob herausfinden, wo du bist | Deine IP-Adresse „in einer Geolokalisierungsdatenbank nachschlagen, um ihren Standort anzunähern“ |
| Erkennen, dass und wann du eine abgeschirmte Transaktion gesendet oder empfangen hast | Das Senden „verbraucht mehr Bandbreite, was sichtbar ist, obwohl die Verbindung verschlüsselt ist“. Das Modell weist darauf hin, dass der Vorgang des Sendens und Empfangens für den Server selbst sichtbar ist |
| Zählen, wie viele Transaktionen du im Laufe der Zeit durchgeführt hast | Dieselben Bandbreitenmuster, über einen längeren Zeitraum beobachtet |
| Wiederkehrende Zahlungsmuster erkennen | Beobachten, wann Aktivität stattfindet |
| Herausfinden, ob eine Adresse dir gehört | Ein Angreifer, der eine Adresse bereits kennt, „könnte Gelder an diese Adresse senden und beobachten, ob es Bandbreitenspitzen gibt“, wenn deine Wallet sie abruft |

Das Modell weist außerdem darauf hin, dass der Normalfall „eine Vertrauensbeziehung zwischen dem Nutzer und dem Betreiber des lightwalletd-Servers“ voraussetzt.

Die ehrliche Zusammenfassung lautet also: Ein Light-Wallet-Server kann dein Geld nicht ausgeben und weder die Beträge noch die Memos in deinen abgeschirmten Transaktionen lesen. Was er gut herausfinden kann, sind deine IP-Adresse und der Zeitpunkt deiner Aktivität, und diese beiden Dinge zusammen können viel über eine Person aussagen. Abgeschirmte Transaktionen schützen, was auf der blockchain geschieht. Sie verbergen nicht automatisch deine Verbindung zum Server.

## Visualisierung / Analogie

Stell dir eine öffentliche Bibliothek vor, die jede jemals gedruckte Zeitung besitzt. Ein vollständiger Knoten ist ein Leser, der das gesamte Archiv mit nach Hause nimmt. Eine Light Wallet ist ein Leser, der den Bibliothekar stattdessen um eine tägliche Zusammenfassung bittet — ein dünnes Blatt, das gerade genug enthält, um zu erkennen, ob etwas für ihn relevant ist.

Die Zusammenfassung ist versiegelt: Der Bibliothekar stellt sie zusammen, ohne lesen zu können, welche Einträge für dich wichtig sind, und du öffnest sie zu Hause mit deinem eigenen Schlüssel. Das ist der kompakte Block, und das Öffnen ist die Probeentschlüsselung auf deinem Gerät.

Doch der Bibliothekar sieht weiterhin, welcher Leser hereinkam, zu welcher Zeit und wie dick das Bündel war, das er mitnahm. Das sind die IP-Adresse und der Zeitpunkt — am Schalter sichtbar, egal wie gut der Umschlag versiegelt ist. Tor entspricht einem anonymen Kurier: Der Bibliothekar übergibt weiterhin dasselbe Bündel, weiß aber nicht mehr, zu wessen Haus es geht.

## Vertiefung

### Routing über Tor

Tor trennt die Verbindung zwischen deiner IP-Adresse und deinem Wallet-Datenverkehr und entfernt damit den stärksten Identifier in der obigen Tabelle.

Unterstützung besteht in den Rust-Bibliotheken, auf denen viele Zcash-Wallets aufbauen. zcash_client_backend enthält ein auf [Arti](https://tpo.pages.torproject.net/core/arti/) aufgebautes Tor-Modul, der Rust-Implementierung von Tor. Dadurch kann eine Wallet Synchronisierung, Transaktionsübertragung und Preisabfragen über Tor leiten, ohne einen separaten Tor-Client mitzuliefern.

Die Zaino-Entwickler vertreten dasselbe Argument und zitieren das Threat Model direkt: Es gebe „die Notwendigkeit, anonyme Transportprotokolle (wie Nym oder Tor) zu verwenden, um die Identitäten der Clients vor den Indexierungsservern von Zcash zu verschleiern“.

In **ZODL** ist Tor eine Einstellung unter Erweiterte Einstellungen. Die Release Notes der Wallet verweisen Nutzer auf den manuellen Verbindungsmodus „plus die Aktivierung von Tor in den Erweiterten Einstellungen“, wenn sie „die Offenlegung von Metadaten reduzieren möchten“. Die App bietet außerdem an, Tor zu aktivieren, bevor du eine Wallet wiederherstellst — genau dann würde eine neue IP-Adresse andernfalls mit einer gesamten Wallet-Historie verknüpft.

Zwei Einschränkungen: Tor verbirgt deine IP-Adresse vor dem Server, ändert aber nicht, was der Server aus deinen Anfragen erfährt. Und Onion-Routing erhöht die Latenz, daher dauert die Synchronisierung länger. Der Betrieb deines eigenen Servers vermeidet die Vertrauensfrage auf andere Weise, denn dann bist du selbst der Betreiber.

### Zaino, der Rust-Indexer

[Zaino](/zcash-tech/zaino) ist ein vom Zingo-Team geschriebener Indexer in Rust, der lightwalletd im Rahmen der Ablösung von zcashd ersetzen soll. Er bedient Light Clients, vollständige Clients und Block Explorer und liest Kettendaten, die von „entweder einem vollständigen Validator von Zebra oder Zcashd“ gehalten werden.

Es befindet sich in aktiver Entwicklung; Version 0.8.0 wurde im August 2026 veröffentlicht. Es soll, wo möglich, abwärtskompatibel mit lightwalletd bleiben, sodass Wallets darauf zeigen können, ohne umgeschrieben werden zu müssen.

Zaino hat eine eigene Seite mit Architekturdiagrammen, daher behandelt diese Seite nur seine Rolle als Light-Wallet-Server.

### Eigenen Server betreiben

Die stärkste Option ist, selbst Betreiber zu sein, wodurch die Vertrauensfrage vollständig entfällt. Beide Server sind Open Source: [lightwalletd](https://github.com/zcash/lightwalletd) in Go und [Zaino](https://github.com/zingolabs/zaino) in Rust. Beide lesen von einem vollständigen Validator, daher benötigst du auch [Zebra](/zcash-tech/zebra-full-node).

## Praktische Auswirkungen

### Serverliste

Das Dashboard [hosh.zec.rocks](https://hosh.zec.rocks/zec) verfolgt öffentliche Server und ihren Zustand und ist der Ort, um zu prüfen, was tatsächlich verfügbar ist. [status.zec.rocks](https://status.zec.rocks/) zeigt den Dienststatus.

Zum Zeitpunkt der Erstellung auf diesem Dashboard aufgeführte Server:

| Server | Hinweise |
|:--|:--|
| zec.rocks:443 | Regionale Endpunkte sind daneben unter na.zec.rocks, eu.zec.rocks, ap.zec.rocks und sa.zec.rocks aufgeführt |
| zec-node.cakewallet.com:443 | Auf der Domain von Cake Wallet |
| zec.0xrpc.io:443 | Betrieben von 0xRPC, das kostenlose öffentliche Endpunkte für mehrere Ketten anbietet und um Spenden zur Deckung der Kapazität bittet |
| zaino.unsafe.zec.rocks:443 | Eine Zaino-Instanz. Beachte den Hostnamen und behandle sie als experimentell |
| testnet.zec.rocks:443 | Testnet, mit einer unter zaino.testnet.unsafe.zec.rocks aufgeführten Zaino-Testnet-Instanz |

Prüfe das Dashboard, statt dieser Liste zu vertrauen. Betreiber kommen und gehen, und eine Seite wie diese altert.

### Den Server in deiner Wallet ändern

Das lohnt sich, wenn du einen Betreiber auswählen möchtest, dem du vertraust, deine Aktivität auf verschiedene Betreiber verteilen oder auf deinen eigenen Server verweisen möchtest.

Die nachstehenden Menüpfade waren korrekt, als diese Seite aktualisiert wurde, aber Wallet-Oberflächen ändern sich — betrachte sie daher als Hinweis und nicht als exakte Route. Suche nach Erweiterten Einstellungen oder einer Server-Option.

#### ZODL

Früher Zashi. Das Zahnrad in der oberen rechten Ecke, dann Erweiterte Einstellungen. Tor befindet sich auf demselben Bildschirm. ZODL bietet außerdem die Verknüpfung Server wechseln, wenn ein Synchronisierungsfehler dadurch verursacht wird, dass der Server nicht aktuell ist.

#### Ywallet

Das Zahnrad in der oberen rechten Ecke, dann der Zcash-Tab.

![Ywallet-Servereinstellungen](/content-images/b0a2910b-dbdf-4292-8e69-af5a386aa183-f51f098d19.webp)

#### Zingo

Das Hamburger-Menü in der oberen linken Ecke, dann Einstellungen, dann nach unten scrollen.

![Zingo-Servereinstellungen](/content-images/ea8f7672-e644-41a5-a422-db131740404a-2626f5fa79.webp)

#### eZcash

Das Hamburger-Menü in der oberen linken Ecke, dann Einstellungen, dann Erweitert.

![eZcash-Servereinstellungen](/content-images/655c0172-61a0-4322-b8cf-4eee4bb53b51-0b93df2e71.webp)

Diese Screenshots wurden im März 2025 aufgenommen, und die Apps haben seitdem neue Versionen veröffentlicht; daher könnten Schaltflächen verschoben worden sein.

## Häufige Fehler

**Zu denken, dass der Server deine Transaktionen lesen kann**. Das kann er nicht. Deine Schlüssel bleiben auf deinem Gerät, und die Beträge und Memos in vollständig abgeschirmten Transaktionen bleiben verschlüsselt — selbst gegenüber einem Angreifer, der den Server kompromittiert hat.

**„Abgeschirmt“ als „anonyme Verbindung“ zu verstehen**. Abgeschirmte Transaktionen schützen, was auf der blockchain geschieht. Deine IP-Adresse und der Zeitpunkt deiner Aktivität sind eine separate Ebene, und genau diese Ebene sieht der Server.

**Anzunehmen, dass Tor jede Spur entfernt**. Tor verbirgt deine IP-Adresse vor dem Server, ändert aber nicht, was der Server aus deinen Anfragen erfährt, und erhöht die Latenz bei der Synchronisierung.

**Einer Serverliste auf einer Wiki-Seite zu vertrauen**. Betreiber kommen und gehen. Prüfe [hosh.zec.rocks](https://hosh.zec.rocks/zec), um zu sehen, was tatsächlich läuft, bevor du deine Wallet auf irgendetwas richtest.

## Zusammenfassung

Light Wallets bieten dir den abgeschirmten Pool ohne den Speicherplatzbedarf — ein guter Tausch. Sei dir nur darüber im Klaren, was du eintauschst. Der Server kann deine Gelder nicht nehmen oder deine abgeschirmten Beträge lesen, ist aber gut positioniert, um deine IP-Adresse und den Zeitpunkt deiner Transaktionen zu sehen. Leite über Tor, wähle deinen Betreiber bewusst oder betreibe deinen eigenen.

## Verwandte Seiten

- [Wer kann deine Zcash-Zahlung sehen](/start-here/who-can-see-your-zcash-payment) — die Einsteigerperspektive auf dieselbe Frage.
- [Was ein Block Explorer sehen kann](/zcash-tech/what-a-block-explorer-can-see) — was on-chain sichtbar ist, im Gegensatz zum Server.
- [Zaino](/zcash-tech/zaino) — Architekturdiagramme und die weitergehende Rolle des Rust-Indexers.
- [Zebra Full Node](/zcash-tech/zebra-full-node) — der Validator, von dem ein Light-Wallet-Server liest.
- [Zcash-Wallet-Synchronisierung](/zcash-tech/zcash-wallet-syncing) — wie die kompakten Blöcke, die ein Server sendet, von deiner Wallet verarbeitet werden.

**Zuletzt aktualisiert:** August 2026
