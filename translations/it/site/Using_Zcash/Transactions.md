<a href="https://github.com/zechub/zechub/edit/main/site/Using_Zcash/Transactions.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>


# Transazioni

ZEC è un asset digitale ampiamente utilizzato per i pagamenti e offre solide funzionalità di privacy che lo rendono adatto a diverse transazioni, come pagare amici, effettuare acquisti o fare donazioni. Per massimizzare privacy e sicurezza, è essenziale comprendere come funzionano i diversi tipi di transazione all'interno di Zcash.

## In breve

- Zcash supporta due tipi di transazione: **shielded**, che mantiene privati i dettagli, e **transparent**, che li registra pubblicamente.
- Gli indirizzi shielded iniziano con `u` o `z`. Gli indirizzi transparent iniziano con `t` e si comportano in modo molto simile a un indirizzo Bitcoin.
- La scelta spetta a te per ogni pagamento. La privacy è un'opzione che Zcash ti offre, non un'impostazione che qualcun altro decide per te.
- Il prelievo da un exchange è il caso più comune in cui le persone perdono privacy. Se l'exchange supporta solo prelievi transparent, rendi shielded i fondi autonomamente una volta ricevuti.
- Le commissioni seguono [ZIP 317](https://zips.z.cash/zip-0317) e aumentano in base alla dimensione della transazione. I wallet che inviano ancora la vecchia commissione fissa possono vedere le proprie transazioni ritardate.
- La maggior parte delle transazioni Zcash ha un'altezza di scadenza ai sensi di [ZIP 203](https://zips.z.cash/zip-0203). Se una transazione scade prima di essere inclusa in un blocco, non può essere confermata dopo tale altezza di scadenza e potrebbe dover essere inviata di nuovo.

## Transazioni Shielded

<div className="my-8 w-full aspect-video max-w-3xl mx-auto rounded-2xl overflow-hidden shadow-lg bg-black">
  <iframe
    className="w-full h-full"
    src="https://www.youtube.com/embed/bZM3o_eIovU"
    title="Zcash Explained: Zcash Shielded Transactions"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowFullScreen
    loading="lazy"
  />
</div>

---

Le transazioni shielded avvengono quando sposti ZEC nel tuo wallet shielded. L'indirizzo del tuo wallet shielded inizia con una `u` o una `z`. Quando invii transazioni shielded, tu e le persone con cui effettui transazioni potete mantenere un livello di privacy impossibile sulle reti di pagamento pubbliche per impostazione predefinita.

Inviare una transazione shielded è più semplice quando utilizzi un wallet che supporta la rete Zcash attuale e gli attuali pool shielded. Prima di affidarti a un wallet per la privacy, verifica se supporta l'invio shielded, la ricezione shielded e il pool che intendi usare. Quando prelevi ZEC da un exchange, verifica se l'exchange supporta prelievi shielded o transparent. Se supporta soltanto prelievi transparent, sposta i fondi in un wallet compatibile con shielded dopo averli ricevuti.

Utilizzare transazioni shielded per inviare e ricevere fondi è il modo migliore per preservare la privacy e ridurre il rischio di divulgare dati di pagamento.

## Transazioni Transparent

Le transazioni transparent funzionano in modo simile alle transazioni Bitcoin. I dettagli delle transazioni sono pubblicamente visibili sulla blockchain, inclusi indirizzi transparent e valori transparent. Le transazioni transparent dovrebbero essere evitate quando la privacy è una priorità.

Gli indirizzi transparent sono comunque utili in alcune situazioni, soprattutto quando un exchange o un servizio non supporta gli indirizzi shielded. Se ricevi ZEC a un indirizzo transparent, considera di renderli shielded prima di effettuare pagamenti successivi.

<div className="my-8 w-full aspect-video max-w-3xl mx-auto rounded-2xl overflow-hidden shadow-lg bg-black">
  <iframe
    className="w-full h-full"
    src="https://www.youtube.com/embed/R-krX1UpsIg"
    title="Learn Zcash shielded wallets!"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowFullScreen
    loading="lazy"
  />
</div>

## Un modo semplice per immaginarlo

Una transazione transparent è una cartolina. Il postino la consegna, ma chiunque la maneggi lungo il percorso può leggere il messaggio, vedere chi l'ha inviata e chi la riceve.

Una transazione shielded è una busta sigillata. Il servizio postale conferma comunque che una lettera reale con un'affrancatura reale è passata attraverso il sistema, e nessuno può falsificarne una o inviare due volte la stessa lettera. Ciò che contiene la busta resta tra mittente e destinatario.

L'aspetto importante è che Zcash ti consente di decidere quale inviare, pagamento per pagamento.

## Commissioni Zcash

Zcash non utilizza unità di gas in stile Ethereum. Le commissioni delle transazioni Zcash vengono pagate in ZEC, di solito misurate in **zatoshis**. Un ZEC equivale a 100.000.000 zatoshis.

[ZIP 317](https://zips.z.cash/zip-0317) definisce un meccanismo di commissione convenzionale che si adatta alla complessità della transazione. Invece di utilizzare per ogni transazione la vecchia commissione fissa di 1.000 zatoshi, la commissione convenzionale si basa su "azioni logiche" quali input, output e azioni shielded. Le transazioni semplici iniziano comunemente intorno a 10.000 zatoshi, ovvero 0,0001 ZEC, mentre le transazioni più complesse possono richiedere di più.

Nella maggior parte dei wallet attuali, gli utenti non dovrebbero dover calcolare manualmente le commissioni ZIP 317. Il wallet dovrebbe scegliere automaticamente una commissione appropriata. Se un wallet utilizza ancora la vecchia commissione fissa o consente di impostare una commissione molto al di sotto della commissione convenzionale ZIP 317, la transazione potrebbe subire ritardi, essere deprioritizzata, essere scartata da alcuni nodi o non essere trasmessa in modo affidabile.

## Risoluzione dei problemi delle transazioni bloccate

Una transazione Zcash non è definitiva solo perché appare nel tuo wallet. Diventa definitiva per l'uso ordinario dopo essere stata inclusa in un blocco e aver ricevuto un numero sufficiente di conferme per la tua situazione. Gli exchange e i servizi possono richiedere più conferme di quelle visualizzate da un wallet per impostazione predefinita.

Usa questo albero decisionale prima di inviare di nuovo:

1. **Il tuo wallet mostra un ID transazione?**
   - Se no, il wallet potrebbe non aver ancora creato o trasmesso la transazione. Controlla lo stato di sincronizzazione, la connessione Internet, la versione del wallet ed eventuali messaggi di errore del wallet.
   - Se sì, copia l'ID transazione e continua.
2. **La transazione è confermata in un blocco?**
   - Se sì, attendi il numero di conferme richiesto dal tuo wallet, exchange, commerciante o servizio.
   - Se no, continua.
3. **La transazione ha raggiunto la sua altezza di scadenza?**
   - Se no, non inviare ancora manualmente lo stesso pagamento. La transazione originale potrebbe ancora essere confermata.
   - Se sì, la transazione non può essere inclusa in un blocco dopo tale altezza di scadenza. Il tuo wallet potrebbe contrassegnarla come scaduta o non riuscita e potresti dover creare una nuova transazione.
4. **La transazione appare su un server o explorer ma non su un altro?**
   - Consideralo un problema di visibilità della rete, non la prova che la transazione sia fallita. Nodi diversi possono avere visualizzazioni diverse del mempool.
   - Attendi, risincronizza il wallet oppure passa a un altro server affidabile, se il tuo wallet lo supporta.
5. **La transazione è scomparsa dopo essere apparsa come confermata?**
   - Una breve riorganizzazione della catena può rimuovere temporaneamente una transazione dalla catena migliore.
   - Attendi altri blocchi. Se la transazione ricompare, continua ad attendere le conferme. Se non ricompare e in seguito scade, crea una nuova transazione.
6. **Il wallet ti chiede di inviare di nuovo?**
   - Segui le indicazioni attuali del wallet solo dopo aver verificato che la transazione precedente sia scaduta, fallita o non più valida.
   - Se non sei sicuro, chiedi assistenza prima di inviare di nuovo.

## In attesa, scaduta, scartata e riorganizzata

- **In attesa** significa che la transazione è stata creata o trasmessa, ma non è ancora stata inclusa in un blocco.
- **Scaduta** significa che l'altezza di scadenza della transazione è stata superata. Ai sensi di ZIP 203, una transazione con un'altezza di scadenza non può essere inclusa in un blocco dopo tale altezza.
- **Scartata** significa che uno o più nodi non conservano più la transazione nel proprio mempool. Ciò può accadere a causa della scadenza, di commissioni basse, della politica del mempool, del comportamento al riavvio o di differenze nella trasmissione.
- **Riorganizzata** significa che un blocco che in precedenza conteneva la transazione non fa più parte della catena migliore. La transazione potrebbe essere inclusa nuovamente in un blocco in seguito, oppure potrebbe tornare in attesa se è ancora valida.

## Quando non inviare di nuovo

Non inviare nuovamente subito solo perché una transazione è in attesa, lenta o assente da un explorer. Inviare di nuovo troppo presto può creare confusione e, a seconda di come il wallet costruisce il nuovo pagamento, potrebbe comportare il rischio di pagare due volte.

Attendi o chiedi prima assistenza quando:

- La transazione ha un ID transazione e non è scaduta.
- Un server la mostra mentre un altro no.
- È stata inclusa di recente in un blocco, ma ha perso conferme dopo una possibile riorganizzazione.
- Il servizio ricevente non ha terminato di conteggiare le conferme.
- Il tuo wallet sta ancora eseguendo la sincronizzazione.

Di solito è più sicuro inviare di nuovo soltanto dopo che il wallet contrassegna chiaramente la transazione come scaduta o fallita, oppure dopo che l'assistenza conferma che la transazione originale non può essere confermata.

## Verifiche sicure per la privacy

Puoi verificare lo stato di base della transazione senza esporre più informazioni del necessario:

- Verifica che il tuo wallet sia completamente sincronizzato.
- Verifica che l'app del wallet sia aggiornata.
- Verifica che la transazione abbia un ID transazione.
- Verifica che la transazione sia confermata, in attesa, scaduta o fallita.
- Verifica l'altezza attuale del blocco e confrontala con l'altezza di scadenza della transazione, se il tuo wallet la mostra.
- Per le transazioni transparent, un block explorer può mostrare la transazione pubblica, gli indirizzi, i valori e le conferme.
- Per le transazioni shielded, un block explorer può mostrare che una transazione esiste, ma non può mostrare mittente shielded, destinatario, importo o dettagli del memo.

## Cosa non condividere pubblicamente

Non pubblicare mai questi elementi in chat pubbliche, sui social media o in un issue tracker:

- Frase seed o frase di recupero
- Chiave di spesa, chiave privata o backup del wallet
- Full Viewing Key
- Screenshot che mostrano saldi, indirizzi completi, memo, codici QR o dettagli dell'account exchange
- Documenti di identità personali o registri di recupero dell'account

Un ID transazione è pubblico sulla catena, ma può comunque collegare la tua richiesta di assistenza alla tua identità. Se la privacy è importante, condividilo solo tramite un canale di assistenza affidabile.

## Cosa serve ai team di assistenza

Quando chiedi aiuto all'assistenza di un wallet, exchange o servizio, condividi solo le informazioni utili minime:

- Nome del wallet o del servizio
- Versione dell'app e sistema operativo
- Se la transazione è shielded, transparent o tra indirizzi shielded e transparent
- ID transazione, se ti senti a tuo agio nel condividerlo
- Ora approssimativa dell'invio
- Se il wallet è completamente sincronizzato
- Stato attuale mostrato dal wallet
- Messaggio di errore esatto, con i dati privati rimossi
- Screenshot con saldi, indirizzi, memo e dettagli dell'account nascosti

I team di assistenza non hanno bisogno della tua frase seed, chiave di spesa, chiave privata o Full Viewing Key.

## Errori comuni

- **Supporre che qualsiasi wallet che elenca ZEC possa inviarlo privatamente.** Diversi wallet multi-valuta supportano soltanto il lato transparent di Zcash. Controlla i pool supportati dal wallet prima di affidarti a esso per la privacy. La pagina [Wallet](https://zechub.wiki/using-zcash/wallets) elenca queste informazioni per ciascuna opzione.
- **Prelevare verso un indirizzo transparent e lasciare lì i fondi.** Il prelievo stesso è pubblico e anche ogni movimento successivo da quell'indirizzo resta pubblico. Rendi shielded i fondi una volta ricevuti.
- **Trattare la privacy come qualcosa che si attiva una sola volta.** Ogni transazione è una scelta separata. Inviare shielded oggi non annulla un pagamento transparent effettuato la settimana scorsa.
- **Riutilizzare un indirizzo transparent per tutto.** Poiché l'attività transparent è visibile in modo permanente, un singolo indirizzo riutilizzato collega gradualmente pagamenti che non avevano motivo di essere connessi.
- **Inviare con una commissione predefinita obsoleta.** I wallet che non hanno adottato ZIP 317 potrebbero ancora inviare la vecchia commissione fissa, lasciando una transazione non confermata.
- **Inviare di nuovo prima della scadenza.** Una transazione in attesa può ancora essere confermata fino alla sua scadenza. Verifica lo stato di scadenza prima di creare un altro pagamento.

## Nota

Tieni presente che il modo più sicuro per utilizzare ZEC è usare transazioni shielded ogni volta che mittente, destinatario, wallet e servizio le supportano. Alcuni wallet ed exchange supportano gli [indirizzi unificati](https://electriccoin.co/blog/unified-addresses-in-zcash-explained/#:~:text=The%20unified%20address%20(UA)%20is,within%20the%20broader%20Zcash%20ecosystem.), che possono combinare più tipi di ricevitori Zcash in un unico indirizzo.

## Risorse

- [ZIP 203: Scadenza delle transazioni](https://zips.z.cash/zip-0203)
- [ZIP 317: Meccanismo proporzionale di commissione per i trasferimenti](https://zips.z.cash/zip-0317)
- [ZIP di Zcash](https://zips.z.cash/)

## Pagine correlate

- [Wallet](/using-zcash/wallets) - quali wallet supportano l'invio shielded e quali sono solo transparent
- [Pool Shielded](/using-zcash/shielded-pools) - Sapling e Orchard, i pool in cui risiedono i tuoi fondi shielded
- [Memo](/using-zcash/memos) - messaggi cifrati che possono accompagnare una transazione shielded
- [Indirizzi Transparent degli Exchange](/using-zcash/transparent-exchange-addresses) - indirizzi TEX e perché gli exchange li usano
- [Exchange custodial](/using-zcash/custodial-exchanges) - quali exchange supportano prelievi shielded

## Convertitore da ZEC a ZAT
