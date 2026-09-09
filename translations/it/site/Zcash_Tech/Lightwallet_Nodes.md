<a href="https://github.com/zechub/zechub/edit/main/site/Zcash_Tech/Lightwallet_Nodes.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>


# Nodi Lightwallet di Zcash

## TL;DR

* La maggior parte delle persone usa Zcash attraverso un light wallet, che non scarica l'intera blockchain. Si collega invece a un server che ha già svolto quel lavoro.
* Oggi due software servono i light wallet: **lightwalletd**, il servizio originale scritto in Go, e **Zaino**, un indicizzatore più recente scritto in Rust.
* Le tue chiavi non lasciano mai il tuo dispositivo e il server non può spendere i tuoi fondi né leggere gli importi e i memo all'interno delle transazioni completamente schermate.
* Ciò che il server può facilmente apprendere è il tuo indirizzo IP e la tempistica della tua attività — le transazioni schermate proteggono ciò che avviene sulla blockchain, non la tua connessione al server.
* Tor rimuove l'identificatore IP; è disponibile nei wallet basati su `zcash_client_backend` e in ZODL è un'impostazione nelle Impostazioni avanzate.
* Puoi cambiare il server usato dal tuo wallet o eseguire il tuo — sia lightwalletd sia Zaino sono open source.

## Spiegazione principale

La maggior parte delle persone usa Zcash attraverso un light wallet, che non scarica l'intera blockchain. Si collega invece a un server che ha già svolto quel lavoro. Questa pagina spiega cosa sono questi server, cosa possono e non possono vedere di te, come instradare la tua connessione tramite Tor e come cambiare il server usato dal tuo wallet.

Oggi due software servono i light wallet. **lightwalletd** è il servizio originale, scritto in Go. **Zaino** è un indicizzatore più recente scritto in Rust, sviluppato nell'ambito del lavoro di deprecazione di zcashd.

### Cosa fa un server per light wallet

Un server per light wallet si colloca tra il tuo wallet e la blockchain di Zcash e gli offre una visualizzazione della catena efficiente in termini di larghezza di banda. Fa tre cose per te.

Fornisce blocchi compatti. Invece di blocchi interi, invia una forma compatta che contiene solo ciò di cui un wallet ha bisogno per rilevare un pagamento al suo indirizzo schermato, rilevare una spesa delle sue note e aggiornare i suoi witness.

Inoltra le tue transazioni. Quando invii, il tuo wallet consegna la transazione completata al server, che la trasmette alla rete.

Risponde alle query sulla catena, come l'altezza attuale e le informazioni sulle commissioni di cui il tuo wallet ha bisogno.

Il tuo wallet svolge comunque il lavoro privato localmente. Conserva le tue chiavi, prova a decrittare i blocchi per trovare le tue note e crea e firma le transazioni sul tuo dispositivo.

### Cosa può e non può vedere il server

Questa è la parte in cui è facile sbagliarsi. Le tue chiavi non lasciano mai il tuo dispositivo, ma questo non significa che il server non apprenda nulla su di te.

Il riferimento qui è il [modello delle minacce dell'app wallet Zcash](https://zcash.readthedocs.io/en/latest/rtd_pages/wallet_threat_model.html), che vale la pena leggere integralmente se ti interessa questo argomento. Descrive diversi tipi di avversario. Quello rilevante per questa pagina è un avversario che può osservare il traffico tra il tuo wallet e internet e tra il server e internet. Chiunque gestisca il server si trova intrinsecamente in parte in quella posizione, perché il tuo wallet si connette direttamente a lui.

Iniziamo da ciò che è protetto. Contro ogni avversario del modello, incluso uno che ha compromesso il server, non può “apprendere alcun materiale crittografico dell'utente (chiavi di spesa, chiavi di visualizzazione, seed phrase ecc.)”, non può rubare i tuoi fondi e non può indurti a inviare fondi che non intendevi inviare. Gli importi e i memo all'interno delle transazioni completamente schermate rimangono crittografati.

Poi c'è ciò che non è protetto. Il modello delle minacce elenca queste come debolezze note contro un avversario che osserva il traffico:

| Debolezza | Come |
|:--|:--|
| Stabilire chi sei | “L'avversario conosce l'indirizzo IP dell'utente, che potrebbe condurlo alla sua identità reale” |
| Stabilire approssimativamente dove ti trovi | Cercando il tuo IP “in un database di geolocalizzazione per approssimarne la posizione” |
| Stabilire se e quando hai inviato o ricevuto una transazione schermata | L'invio “usa più larghezza di banda, il che è visibile anche se la connessione è crittografata”. Il modello nota che l'atto di inviare e ricevere è visibile al server stesso |
| Contare quante transazioni hai effettuato nel tempo | Gli stessi schemi di larghezza di banda, osservati per un periodo più lungo |
| Individuare schemi di pagamento ricorrenti | Osservando quando avviene l'attività |
| Stabilire se un indirizzo è tuo | Un avversario che già conosce un indirizzo “potrebbe inviare fondi a quell'indirizzo e osservare se ci sono picchi di larghezza di banda” dal tuo wallet che lo recupera |

Il modello osserva inoltre che il caso ordinario presuppone “una relazione di fiducia tra l'utente e l'operatore del server lightwalletd”.

Quindi, il riepilogo onesto è questo. Un server per light wallet non può spendere il tuo denaro e non può leggere gli importi o i memo nelle tue transazioni schermate. Ciò che può facilmente apprendere è il tuo indirizzo IP e la tempistica della tua attività, e questi due elementi insieme possono dire molto di una persona. Le transazioni schermate proteggono ciò che avviene sulla blockchain. Non nascondono, da sole, la tua connessione al server.

## Visuale / Analogia

Pensa a una biblioteca pubblica che conserva ogni giornale mai stampato. Un nodo completo è un lettore che porta a casa l'intero archivio. Un light wallet è un lettore che chiede invece al bibliotecario una rassegna quotidiana — un foglio sottile che contiene appena abbastanza per individuare se qualcosa lo riguarda.

La rassegna è sigillata: il bibliotecario la prepara senza poter leggere quali elementi ti interessano, e tu la apri a casa con la tua chiave. Questo è il blocco compatto, e l'apertura è la decrittazione di prova sul tuo dispositivo.

Ma il bibliotecario vede comunque quale lettore è entrato, a che ora e quanto spesso era il fascio che ha portato via. Questo è l'indirizzo IP e la tempistica — visibili dal bancone, per quanto bene sia sigillata la busta. Tor equivale a inviare un corriere anonimo: il bibliotecario consegna comunque lo stesso fascio, ma non sa più a quale casa sia diretto.

## Approfondimento

### Instradamento tramite Tor

Tor interrompe il collegamento tra il tuo indirizzo IP e il traffico del tuo wallet, eliminando l'identificatore più forte nella tabella sopra.

Il supporto esiste nelle librerie Rust su cui si basano molti wallet Zcash. zcash_client_backend include un modulo Tor basato su [Arti](https://tpo.pages.torproject.net/core/arti/), l'implementazione Rust di Tor, così un wallet può instradare tramite Tor la sincronizzazione, la trasmissione delle transazioni e le consultazioni dei prezzi senza distribuire un client Tor separato.

Gli sviluppatori di Zaino sostengono la stessa tesi, citando direttamente il modello delle minacce: esiste “la necessità di utilizzare protocolli di trasporto anonimi (come Nym o Tor) per offuscare le identità dei client dai server di indicizzazione di Zcash”.

In **ZODL**, Tor è un'impostazione nelle Impostazioni avanzate. Le note di rilascio del wallet indirizzano gli utenti alla modalità di connessione manuale “più l'abilitazione di Tor nelle Impostazioni avanzate” se “preferiscono ridurre l'esposizione dei metadati”, e l'app offre di attivare Tor prima del ripristino di un wallet, ovvero il momento in cui un nuovo IP sarebbe altrimenti associato all'intera cronologia di un wallet.

Due avvertenze. Tor nasconde il tuo IP al server, ma non cambia ciò che il server apprende dalle richieste che effettui. E il routing onion aggiunge latenza, quindi la sincronizzazione richiede più tempo. Eseguire il proprio server affronta diversamente la questione della fiducia, poiché in quel caso l'operatore sei tu.

### Zaino, l'indicizzatore Rust

[Zaino](/zcash-tech/zaino) è un indicizzatore scritto in Rust dal team Zingo, sviluppato per sostituire lightwalletd nell'ambito del lavoro di deprecazione di zcashd. Serve client leggeri, client completi ed esploratori di blocchi, leggendo dati della catena detenuti da “un validatore completo Zebra o Zcashd”.

È in sviluppo attivo, con la versione 0.8.0 rilasciata nell'agosto 2026. Mira a rimanere retrocompatibile con lightwalletd dove possibile, così i wallet possono puntarlo senza dover essere riscritti.

Zaino ha una propria pagina con diagrammi dell'architettura, quindi questa pagina copre solo il suo ruolo di server per light wallet.

### Eseguire il proprio

L'opzione più forte è essere il proprio operatore, il che elimina completamente la questione della fiducia. Entrambi i server sono open source: [lightwalletd](https://github.com/zcash/lightwalletd) in Go e [Zaino](https://github.com/zingolabs/zaino) in Rust. Entrambi leggono da un validatore completo, quindi vorrai anche [Zebra](/zcash-tech/zebra-full-node).

## Implicazioni pratiche

### Elenco dei server

La dashboard [hosh.zec.rocks](https://hosh.zec.rocks/zec) monitora i server pubblici e il loro stato di salute, ed è il posto giusto per verificare cosa è effettivamente attivo. [status.zec.rocks](https://status.zec.rocks/) mostra lo stato dei servizi.

Server elencati su quella dashboard al momento della stesura:

| Server | Note |
|:--|:--|
| zec.rocks:443 | Gli endpoint regionali sono elencati accanto ad esso su na.zec.rocks, eu.zec.rocks, ap.zec.rocks e sa.zec.rocks |
| zec-node.cakewallet.com:443 | Sul dominio di Cake Wallet |
| zec.0xrpc.io:443 | Gestito da 0xRPC, che offre endpoint pubblici gratuiti per diverse catene e chiede donazioni per coprire la capacità |
| zaino.unsafe.zec.rocks:443 | Un'istanza Zaino. Nota il nome host, considerala sperimentale |
| testnet.zec.rocks:443 | Testnet, con un'istanza Zaino testnet elencata su zaino.testnet.unsafe.zec.rocks |

Controlla la dashboard invece di fidarti di questo elenco. Gli operatori arrivano e se ne vanno, e una pagina come questa invecchia.

### Cambiare il server nel tuo wallet

Vale la pena farlo se vuoi scegliere un operatore di cui ti fidi, distribuire l'attività tra operatori o puntare al tuo.

I percorsi di menu qui sotto erano corretti quando questa pagina è stata aggiornata, ma le interfacce dei wallet cambiano, quindi considerali un'indicazione anziché un percorso esatto. Cerca Impostazioni avanzate o un'opzione del server.

#### ZODL

Precedentemente Zashi. L'icona dell'ingranaggio nell'angolo in alto a destra, poi Impostazioni avanzate. Tor si trova nella stessa schermata. ZODL offre anche una scorciatoia Cambia server quando un errore di sincronizzazione è causato dal server non aggiornato.

#### Ywallet

L'icona dell'ingranaggio nell'angolo in alto a destra, poi la scheda Zcash.

![Impostazioni del server Ywallet](/content-images/b0a2910b-dbdf-4292-8e69-af5a386aa183-f51f098d19.webp)

#### Zingo

Il menu hamburger nell'angolo in alto a sinistra, poi Impostazioni, quindi scorri verso il basso.

![Impostazioni del server Zingo](/content-images/ea8f7672-e644-41a5-a422-db131740404a-2626f5fa79.webp)

#### eZcash

Il menu hamburger nell'angolo in alto a sinistra, poi Impostazioni, quindi Avanzate.

![Impostazioni del server eZcash](/content-images/655c0172-61a0-4322-b8cf-4eee4bb53b51-0b93df2e71.webp)

Questi screenshot sono stati acquisiti nel marzo 2025 e da allora le app hanno rilasciato aggiornamenti, quindi i pulsanti potrebbero essere stati spostati.

## Errori comuni

**Pensare che il server possa leggere le tue transazioni**. Non può. Le tue chiavi rimangono sul tuo dispositivo e gli importi e i memo all'interno delle transazioni completamente schermate rimangono crittografati — anche contro un avversario che ha compromesso il server.

**Interpretare “schermato” come “connessione anonima”**. Le transazioni schermate proteggono ciò che avviene sulla blockchain. Il tuo indirizzo IP e la tempistica della tua attività sono un livello separato, e quel livello è esattamente ciò che il server vede.

**Supporre che Tor rimuova ogni traccia**. Tor nasconde il tuo IP al server, ma non cambia ciò che il server apprende dalle richieste che effettui e aggiunge latenza alla sincronizzazione.

**Fidarsi di un elenco di server su una pagina wiki**. Gli operatori arrivano e se ne vanno. Controlla [hosh.zec.rocks](https://hosh.zec.rocks/zec) per verificare cosa è effettivamente in esecuzione prima di puntare il tuo wallet a qualunque cosa.

## Riepilogo

I light wallet ti offrono il pool schermato senza lo spazio su disco, ed è un buon compromesso. Sii solo chiaro su cosa stai cedendo in cambio. Il server non può prendere i tuoi fondi né leggere i tuoi importi schermati, ma può facilmente vedere il tuo indirizzo IP e quando effettui transazioni. Instrada tramite Tor, scegli deliberatamente il tuo operatore oppure esegui il tuo.

## Pagine correlate

- [Chi può vedere il tuo pagamento Zcash](/start-here/who-can-see-your-zcash-payment) — la visuale per principianti della stessa questione.
- [Cosa può vedere un esploratore di blocchi](/zcash-tech/what-a-block-explorer-can-see) — ciò che è visibile on-chain, anziché sul server.
- [Zaino](/zcash-tech/zaino) — diagrammi dell'architettura e il ruolo più ampio dell'indicizzatore Rust.
- [Nodo completo Zebra](/zcash-tech/zebra-full-node) — il validatore da cui legge un server per light wallet.
- [Sincronizzazione del wallet Zcash](/zcash-tech/zcash-wallet-syncing) — come il tuo wallet elabora i blocchi compatti inviati da un server.

**Ultimo aggiornamento:** agosto 2026
