<a href="https://github.com/zechub/zechub/edit/main/site/contribute/ZecWeekly_Newsletter.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>

# Newsletter ZecWeekly

ZecWeekly è una newsletter pubblicata ogni domenica mattina. Include tutte le notizie avvenute durante la settimana nell'ecosistema Zcash. Le notizie sono curate settimanalmente dai membri della community e tutti i link rilevanti vengono aggiunti alla newsletter. Iscriviti alla newsletter [qui](https://zechub.substack.com/).

## Contribuisci

I contributi alla newsletter funzionano al meglio quando un collaboratore prepara l'edizione per la settimana corretta, segue il thread corrente relativo alla bounty o al coordinamento e invia la pull request dopo che i link settimanali sono pronti. Non inviare un'edizione futura prima che ZecHub abbia pubblicato o confermato la data per quell'edizione. Le pull request inviate in anticipo spesso non includono gli aggiornamenti di fine settimana, entrano in conflitto con un curatore assegnato o usano la scadenza sbagliata.

### 1. Conferma l'edizione corrente

Prima di iniziare a scrivere:

- Controlla [ZEC Bounties ](https://bounties.zechub.wiki/) per l'attività relativa alla newsletter corrente.
- Attendi di essere assegnato

![ss](/content-images/149a802c-b64f-4969-ad89-e83ffecf568e-d5d8387145.webp)



### 2. Fai un fork del repository

Se sei nuovo su GitHub, usa questo flusso di lavoro:

1. Apri il [repository ZecHub](https://github.com/ZecHub/zechub).
2. Fai clic su **Fork** e crea un fork nel tuo account GitHub.
3. Nel tuo fork, crea un nuovo branch per l'edizione. Un nome di branch chiaro è utile, ad esempio `digest-may-30-2026`.
4. Assicurati che la tua pull request abbia come repository di base `ZecHub/zechub` e come branch di base `main`.

Se usi la riga di comando, lo stesso flusso di lavoro appare così:

```bash
git clone https://github.com/YOUR-USERNAME/zechub.git
cd zechub
git checkout -b digest-month-day-year
```

Sostituisci `YOUR-USERNAME` con il tuo nome utente GitHub. L'URL sopra è un segnaposto e non verrà risolto così com'è scritto.

### 3. Crea il file della newsletter

Usa il [modello della newsletter](https://github.com/ZecHub/zechub/blob/main/newsletter/newslettertemplate.md) come punto di partenza. Le edizioni della newsletter devono essere inserite nella cartella [`newsletter`](https://github.com/ZecHub/zechub/tree/main/newsletter).

Quando crei il file:

- Segui il formato del nome file richiesto dall'issue o usato dalle recenti edizioni accettate.
- Mantieni lo stesso ordine delle sezioni del modello, a meno che l'attività non richieda un formato diverso.
- Aggiungi solo link della settimana pertinente.
- Scrivi una descrizione breve e chiara per ogni link, affinché i lettori capiscano perché è importante.
- Traduci o riassumi in inglese le fonti non in inglese, quando necessario.
- Controlla ogni link prima di aprire la pull request.

### 4. Raccogli i link al momento giusto

ZecWeekly copre normalmente le attività dell'ecosistema Zcash della settimana corrente e viene pubblicata verso la fine della settimana. La tempistica più sicura è:

- Inizia a raccogliere i link dopo la pubblicazione dell'issue o dell'attività relativa alla newsletter corrente.
- Mantieni una bozza mentre la settimana è ancora in corso.
- Invia la pull request vicino alla data di invio richiesta, dopo aver controllato gli aggiornamenti di fine settimana.
- Non inviare la newsletter di una settimana futura prima che esista l'attività per quella data o prima che ZecHub confermi che devi prepararla.

Se un issue indica di inviare entro una data specifica, segui quella data. Se c'è un conflitto tra questa pagina e un issue corrente, segui l'issue corrente.

### 5. Apri la pull request

Quando il file della newsletter è pronto:

1. Effettua il commit delle modifiche nel tuo fork.
2. Apri una pull request in `ZecHub/zechub` sul branch `main`.
3. Usa un titolo che corrisponda all'edizione, ad esempio `Zcash Ecosystem Digest | May 30th`.
4. Collega l'issue nel corpo della pull request affinché i revisori possano collegare il lavoro all'attività.

Esempio di corpo della pull request:

```md
Closes #ISSUE_NUMBER

Summary:
- Adds the Zcash Ecosystem Digest for Month Day.
- Uses the newsletter template and the current issue deadline.
- Checks links and descriptions for the requested week.
```

Dopo aver aperto la pull request, tieni d'occhio i commenti di revisione. Se ZecHub richiede modifiche, aggiorna lo stesso branch invece di aprire una seconda pull request per la stessa edizione.

### Esempi reali

Usa queste pull request della newsletter unite come esempi di invii accettati:

- [Zcash Ecosystem Digest | 11 aprile](https://github.com/ZecHub/zechub/pull/1551)
- [Zcash Ecosystem Digest | 28 marzo](https://github.com/ZecHub/zechub/pull/1544)
- [Zcash Ecosystem Digest | 14 febbraio](https://github.com/ZecHub/zechub/pull/1474)


![Merged ZecWeekly newsletter pull request example](/content-images/9230d68d-6406-4c8a-992c-df84e0d318d8-8893d2de55.webp)

Quando confronti il tuo lavoro con un esempio, concentrati sulla posizione del file, sul formato del titolo, sull'ordine delle sezioni, sulle descrizioni dei link e sul fatto che la pull request rimandi all'attività corretta.

### Errori comuni da evitare

- Aprire una pull request prima che la data dell'edizione o l'attività siano confermate.
- Lavorare su un issue che ha già una pull request collegata.
- Inviare la pull request al proprio fork invece che a `ZecHub/zechub`.
- Usare il nome file sbagliato o inserire il file fuori dalla cartella `newsletter`.
- Copiare una vecchia edizione senza aggiornare ogni data, link e descrizione.
- Aggiungere link della settimana sbagliata.
- Lasciare link non funzionanti, link duplicati o testo segnaposto del modello.
- Aprire una nuova pull request dopo i commenti di revisione invece di aggiornare il branch originale.

### Checklist finale

Prima di richiedere una revisione, conferma che:

- La data dell'issue o dell'attività corrisponde al file della newsletter.
- Nessun'altra pull request aperta copre già lo stesso issue o la stessa edizione.
- Il file si trova nella cartella `newsletter`.
- Le sezioni del modello sono complete.
- Ogni link funziona e ha una descrizione utile.
- Il corpo della pull request collega l'issue corretto.
- Sei disponibile a effettuare modifiche se i revisori ne richiedono.

## Edizioni passate

[Archivio ZecWeekly](https://zechub.substack.com/p/archive)
