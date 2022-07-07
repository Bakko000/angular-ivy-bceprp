# Progetto Sviluppo dei Servizi Web AA 2021/2022

# Descrizione
Il progetto riguarda la creazione delle principali funzionalità di una applicazione per la prenotazione di posti in un teatro.
L'applicazione fornisce all'utente la possibilità di accedere alle prenotazione di un certo spettacolo, visualizzare i posti disponibili nel teatro (platea e palchi), scegliere un posto, associargli il proprio nominativo, e ricevere conferma dell'avvenuta prenotazione. L'utente può fare funzione di biglietteria, prenotando in sequenza biglietti per gli utenti.
In questa versione è possibile creare un nuovo teatro scegliendo il numero di posti per il teatro e, nell'interfaccia di prenotazione, prenotare "rapidamente" con un solo click il posto desiderato.

# Codice
Il linguaggio utilizzato è TypeScript. Per l'architettura <i>three-tier</i> che rispetta il paradigma MVC, si sfrutta il framework Angular13.

# Anteprima e link al deploy
https://myproject-72e14.firebaseapp.com/?17704

# Struttura
Il progetto si articola in 4 componenti compreso quello principale, ed un servizio.

<b>db.service</b> è il servizio che costruisce Observables di richieste tramite HTTP (POST e GET) al database di tipo <i>KvaaS</i>;


<b>app.component</b> è il componente principale, contiene il form di "login" per accedere ad un certo spettacolo, verificando l'esistenza della chiave ad esso associata. Inoltre contiene la classe per l'oggetto Teatro. Da questo componente si passano diverse variabili, di cui restano in ascolto gli altri. La verifica dell'esistenza della chiave all'interno della sessione è fondamentale per la visualizzazione dei successivi template. A sua volta app.component contiene altri 3 componenti:

<b>nominativo.component</b> è il componente che si occupa del passaggio del nome dell'utente che sta prenotando, dei cui cambi di valore restano in ascolto gli altri componenti ed il componente stesso (tramite @Input). In base al valore di questa variabile si mostra o meno il template corrispettivo, e si passa allo step successivo nel template di prenotazione.component.  Nominativo permette anche il reset del teatro, che comporta lo svuotamento dei posti e il reset delle dimensioni a quelle originali (se aveva dimensioni diverse). A tal fine viene istanziato un nuovo oggetto della classe Teatro richiamata da app.component.

<b>prenotazione.component</b> è il componente che si occupa dell'effettiva prenotazione, viene mostrato il teatro tramite la subscribe di getTheatre (dal servizio) e la scomposizione della stringa così ottenuta. Il click del posto genera un evento che viene gestito dalla funzione selezionaPosto(), quest ultimo tramite la subscribe al metodo setTheatre (dal servizio) invia la nuova stringa aggiornata, dimensioni del teatro incluse. Alla fine si mostra il template del componente nominativo per permettere la prenotazione di altri posti e viene mostrato il messaggio di avvenuta prenotazione.

<b>nuovoteatro.component</b> è il componente che si occupa dell'aggiunta di un nuovo teatro. Si concede la possibilità di configurarne i posti (chiamate <i>dimensioni</i>, le file restano invece fisse), un certo numero per la platea e per i palchi. <b>Se il teatro viene successivamente resettato le dimensioni diventano di default: 10 posti platea, 6 posti palchi</b>. La variabile newtheatre passata tra componenti e la variabile clicked permettono di gestire la visualizzazione del  template. Alla fine della creazione viene visualizzato un messaggio ed in esso fornita la nuova chiave. La creazione di un nuovo teatro avviene tramite la subscribe al metodo newKey(), il recupero delle dimensioni inserite nel form dall'utente (tramite ngModel), ed il loro successivo passaggio ad una nuova istanza dell'oggetto della classe Teatro che configura così il nuovo teatro. L'inserimento nel database avviene tramite la subscribe al metodo newTheatre che passa la stringa composita (dimensioni+platea+palchi) alla nuova chiave.


