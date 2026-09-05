<a href="https://github.com/zechub/zechub/edit/main/site/Start_Here/Developer_Resources.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>


# Risorse per sviluppatori

Le risorse necessarie per sviluppare su Zcash, raggruppate in base allo scopo di ciascuna anziché elencate tutte insieme.

Lo stack è cambiato molto nel 2026. zcashd, che ha gestito la rete per gran parte della sua storia, ha raggiunto la fine del suo ciclo di vita il 18 luglio 2026 all'altezza del blocco 3417100, e ogni nodo non modificato si è arrestato a tale altezza e rifiuterà di riavviarsi. Le guide scritte per zcashd sono ormai storia, non un punto di partenza, quindi questa pagina è organizzata attorno a ciò che lo ha sostituito.

## Lo stack in breve

| Livello | Cosa usare | Inizia da |
|:--|:--|:--|
| Nodo completo | Zebra o Zakura | [Il libro di Zebra](https://zebra.zfnd.org/), [zakura.com](https://zakura.com/) |
| Wallet per nodo completo | Zallet, in beta | [Il libro di Zallet](https://zcash.github.io/zallet/) |
| Server per wallet leggeri | Zaino o lightwalletd | [Zaino](https://github.com/zingolabs/zaino), [lightwalletd](https://github.com/zcash/lightwalletd) |
| Librerie wallet | I crate di librustzcash | [librustzcash](https://github.com/zcash/librustzcash) |
| Mobile | SDK Android e iOS | [Android](https://github.com/zcash/zcash-android-wallet-sdk), [iOS](https://github.com/zcash/zcash-swift-wallet-sdk) |
| Specifica | Specifica del protocollo e ZIP | [zips.z.cash](https://zips.z.cash) |

## Nodi

Un nodo convalida il consenso e conserva la catena. Esistono due implementazioni sviluppate attivamente.

[Zebra](/zcash-tech/zebra-full-node) è il nodo della Zcash Foundation, scritto in Rust, ed è quello presupposto oggi dalla maggior parte delle guide. [Il libro di Zebra](https://zebra.zfnd.org/) spiega come installarlo ed eseguirlo, mentre lo [repository](https://github.com/ZcashFoundation/zebra) è dove avviene lo sviluppo.

[Zakura](/zcash-tech/zakura-node) è un nodo più recente, descritto dai suoi autori come un "nodo completo Zcash compatibile con il consenso, progettato per la scalabilità", con sincronizzazione più rapida, potatura dei blocchi e una modalità di compatibilità con zcashd. È guidato da Sean Bowe, cofondatore di Zcash, e Dev Ojha. È open source con licenza Apache 2.0 su [zakura-core/zakura](https://github.com/zakura-core/zakura).

ZecHub dispone di una pagina sui [nodi completi](/zcash-tech/full-nodes) che illustra i compromessi tra essi.

## Il wallet per nodo completo

zcashd includeva un wallet insieme al nodo. Quel wallet non esiste più e [Zallet](https://github.com/zcash/zallet) ne è il sostituto. Il libro di Zallet lo descrive come "un wallet Zcash per nodo completo scritto in Rust", "sviluppato come sostituto del wallet zcashd".

Leggi l'avvertenza di sicurezza prima di farvi affidamento. Zallet è in beta, "non è stato completamente revisionato", potrebbero verificarsi modifiche incompatibili "in qualsiasi momento, che richiederebbero di eliminare e ricreare il tuo wallet Zallet", e non tutti i metodi RPC di zcashd sono stati ancora portati.

Se stai migrando una configurazione esistente, ZecHub ha una [guida alla migrazione da zcashd a Zebra e Zallet](/guides/migration-guide-zcashd-to-zebrad-zallet) e un [riferimento rapido di Zallet](/using-zcash/zallet-quick-reference-guide).

## Server per wallet leggeri

La maggior parte dei wallet non esegue un nodo. Comunicano con un server che conserva la catena e ne restituisce una visualizzazione compatta.

[lightwalletd](https://github.com/zcash/lightwalletd) è il servizio originale, scritto in Go, descritto come "un servizio backend che fornisce un'interfaccia efficiente in termini di larghezza di banda alla blockchain Zcash". [Zaino](/zcash-tech/zaino) è l'indicizzatore più recente, scritto in Rust, e legge da un validatore completo anziché mantenere una propria copia della catena.

La documentazione del [protocollo Light Client](https://zcash.readthedocs.io/en/latest/lightwalletd/index.html) tratta il protocollo stesso. La pagina sui [nodi Lightwallet](/zcash-tech/lightwallet-nodes) illustra ciò che questi server possono e non possono vedere di un utente, un aspetto importante da comprendere prima di sceglierne uno.

## Creare un wallet

La maggior parte del lavoro sui wallet avviene nei crate Rust di [librustzcash](https://github.com/zcash/librustzcash), sui quali si basano gli SDK mobile e vari wallet desktop. Ogni crate è documentato su [docs.rs](https://docs.rs).

| Crate | A cosa serve |
|:--|:--|
| zcash_client_backend | "API per creare client leggeri Zcash schermati", inclusa la sincronizzazione e la costruzione delle transazioni |
| zcash_client_sqlite | "Un client leggero Zcash basato su SQLite", il livello di archiviazione per quello sopra |
| zcash_keys | "Gestione di chiavi e indirizzi Zcash" |
| zcash_primitives | "Implementazioni Rust delle primitive Zcash" |
| zcash_protocol | "Costanti di rete e tipi di valore del protocollo Zcash" |
| orchard | "Il protocollo di transazione schermata Orchard" |
| sapling-crypto | "Libreria crittografica per Zcash Sapling" |
| pczt | "Strumenti per lavorare con transazioni Zcash parzialmente create", usati per la firma hardware e multi-dispositivo |
| zip321 | URI per richieste di pagamento, come specificato in ZIP 321 |

Per il mobile, l'[SDK Android](https://github.com/zcash/zcash-android-wallet-sdk) e l'[SDK iOS](https://github.com/zcash/zcash-swift-wallet-sdk) incapsulano tali librerie. Il repository iOS si chiamava in precedenza ZcashLightClientKit, quindi i collegamenti e gli articoli meno recenti usano quel nome.

## Specifica e crittografia

La [specifica del protocollo](https://zips.z.cash/protocol/protocol.pdf) è l'autorità sul funzionamento di Zcash, incluse le [codifiche di indirizzi e chiavi](https://zips.z.cash/protocol/protocol.pdf#5.6%20Encodings%20of%20Addresses%20and%20Keys).

Gli [ZIP](https://zips.z.cash) sono il luogo in cui vengono proposte e specificate le modifiche, e l'indice mostra quali sono bozze e quali definitivi. Le modifiche al consenso vengono distribuite negli aggiornamenti di rete e ZecHub li tiene traccia nella pagina degli [aggiornamenti di rete](/start-here/network-upgrades).

Per la crittografia sottostante, leggi [Il libro di halo2](https://zcash.github.io/halo2/index.html) e [Il libro di Orchard](https://zcash.github.io/orchard/), insieme alla documentazione dei crate [halo2](https://docs.rs/halo2_proofs/latest/halo2_proofs/) e [orchard](https://docs.rs/orchard/latest/orchard/). [Il libro di FROST](https://frost.zfnd.org/) tratta le firme a soglia e ZecHub dispone di una pagina su [FROST](/zcash-tech/frost).

## Testnet

Testnet è una catena separata con monete prive di valore, chiamate TAZ. Sia Zebra sia Zakura possono essere eseguiti su di essa e la [guida alla testnet](https://zcash.readthedocs.io/en/latest/rtd_pages/testnet_guide.html) tratta la configurazione del nodo.

[testnet.zcashexplorer.app](https://testnet.zcashexplorer.app/) è un block explorer testnet funzionante, con una controparte mainnet su [mainnet.zcashexplorer.app](https://mainnet.zcashexplorer.app/).

Ottenere TAZ è la parte complicata. I faucet pubblici appaiono e scompaiono, e quelli collegati dalla documentazione meno recente non rispondevano quando questa pagina è stata scritta. La via affidabile è chiedere nel Discord R&D di Zcash, che è ciò che suggerisce la stessa documentazione di Zcash.

## Documentazione generale

La [documentazione Zcash](https://zcash.readthedocs.io/en/latest/) è ancora la fonte singola più ampia e tratta concetti del protocollo, integrazione e mining. Leggila con una certa cautela. È versionata rispetto a zcashd, quindi alcune sue parti descrivono un nodo che non è più operativo, mentre le sezioni sul protocollo e sui client leggeri rimangono utili. Vale la pena leggere il [modello di minaccia dell'app wallet Zcash](https://zcash.readthedocs.io/en/latest/rtd_pages/wallet_threat_model.html) disponibile lì prima di progettare qualsiasi cosa che tocchi la privacy degli utenti.

Se sei nuovo alle blockchain in generale, [Mastering Bitcoin](https://github.com/bitcoinbook/bitcoinbook) è la raccomandazione abituale per i fondamenti condivisi ed è disponibile gratuitamente per la lettura integrale. Non tratta le transazioni schermate.

## Altri strumenti menzionati dagli sviluppatori

[Arti](https://docs.rs/arti/latest/arti/) è l'implementazione Rust di Tor, usata da zcash_client_backend per instradare il traffico dei wallet. [Tailscale](https://github.com/tailscale/tailscale) viene menzionato per connettersi a un nodo che esegui autonomamente. [warp2](https://github.com/hhanh00/warp2) è un'implementazione di sincronizzazione rapida di Hanh, sebbene non sia stata aggiornata dal 2023.

## Comunità ed eventi

Il [Discord R&D di Zcash](https://discord.gg/6AK7keWFaK) è dove si discute dello sviluppo del protocollo e dei wallet, mentre il [forum della comunità Zcash](https://forum.zcashcommunity.com/) ospita proposte più lunghe e discussioni di supporto.

I risultati dei recenti hackathon offrono un buon quadro di ciò che le persone stanno realizzando: [ZecHub 2024](https://x.com/ZecHub/status/1845212469809033489), [ZecHub 2025](https://x.com/ZecHub/status/1975565960661635283) e l'[hackathon Zypherpunk 2025](https://forum.zcashcommunity.com/t/zypherpunk-hackathon-winners/53985).

## Risorse ritirate

Mantenute perché gli articoli meno recenti vi fanno riferimento e perché restano il riferimento per comprendere il comportamento del nodo ritirato. Non iniziare da qui.

[Il libro di Zcashd](https://zcash.github.io/zcash/) e il [riferimento RPC di zcashd](https://zcash.github.io/rpc/) documentano software che ha raggiunto la [fine del ciclo di vita](https://zcash.github.io/zcash/user/end-of-life.html) nel luglio 2026. Il repository [zcash/zcash](https://github.com/zcash/zcash) è archiviato.

Se hai una risorsa da aggiungere, o noti qualcosa qui che non è più aggiornato, apri una issue o una pull request. I team non sempre hanno la capacità di mantenere tutto aggiornato e segnalare ciò che hai incontrato aiuta a indirizzare le guide.

**Ultimo aggiornamento:** agosto 2026
