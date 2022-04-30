# Controllo dell’erogazione di denaro di un bancomat

Il dispositivo da noi progettato consente di controllare l’erogazione di denaro di un bancomat. Tale dispositivo é stato modellato come circuito sequenziale composto da una parte di controllo (FSM) ed un’altra adibita all'elaborazione dei dati (Datapath).
Il circuito ha 4 ingressi nel seguente ordine:

• BANCOMAT_INSERITO (1 bit) • CODICE (4 bit)
• CASH_RICHIESTO (10 bit)
• CASH_DISPONIBILE (16 bit)

Gli output sono nel seguente ordine:
• REINSERIRE_CODICE (1 bit)
• ABILITAZIONE_EROGAZIONE (1 bit)
• BLOCCO_BANCOMAT (1 bit)
• CASH_DA_EROGARE (10bit)

Il meccanismo è guidato come segue:
• Il segnale di ingresso BANCOMAT_INSERITO (da considerare derivante da un circuito esterno che rileva la presenza di un bancomat valido inserito nel macchinario) se uguale a 1 abilita la codifica dei numeri inseriti tramite il segnale di ingresso CODICE. Se uguale a 0, disabilita (pone a zero) tutte le uscite del circuito.
• L'analisi del CODICE inizia soltanto dopo che il BANCOMAT è stato inserito. Non è possibile inserire il BANCOMAT e la prima cifra del codice nello stesso momento.
• Una volta che il bancomat è stato inserito, viene inserito nel circuito il codice di autenticazione tramite il segnale di ingresso CODICE composto da 3 numeri inseriti in 3 istanti consecutivi, con range 0..9, codificati quindi con 4 bit.
• Una volta accertato che la sequenza numerica corrisponde a 5 5 0, il circuito riceve l’ammontare del cash richiesto dall’ingresso CASH_RICHIESTO (ammontare da 0 a 1023 euro, codificato con 10 bit) e attiva il controllo della disponibilità di banconote nella cassaforte, tramite il segnale interno CHECK_DISPONIBILITA (1 bit). 
Il controllo verifica se il cash richiesto è inferiore a 1/4 del cash disponibile in cassaforte, quest’ultimo ricevuto dal segnale di ingresso CASH_DISPONIBILE. Se inferiore allora il circuito abilita il segnale interno CASH_OK (1 bit), il quale fa abilitare il segnale di uscita ABILITAZIONE_EROGAZIONE, e riporta sul segnale di uscita CASH_DA_EROGARE l’importo richiesto. Altrimenti, tutti questi segnali rimangono posti a 0.
3

• Se il codice viene inserito in modo errato, il circuito abilita l’uscita REINSERIRE_CODICE.
• REINSERIRE_CODICE viene alzato solo al termine dell’inserimento dei codici che compongono il pin. Per esempio, se il codice inserito fosse 123, la porta viene messa ad 1 solo al termine dell’inserimento dell’intero codice e non già alla prima cifra inserita.
• Se il codice viene inserito in modo errato per 3 volte consecutive, il circuito abilita l’uscita BLOCCO_BANCOMAT.
