#  wpfthreads
Richiesta
Si vuole visualizzare un conteggio sfruttando le istruzioni multithreads messe a disposizione dal linguaggio C#.
 
Si prevedano quindi tre oggetti TextBlock WPF che visualizzano l'avanzamento di tre contatori a velocità diverse.
 
Il primo scatta ogni ms e conta fino a 5000 (durata totale 5 secondi).
Il secondo scatta ogni 10ms e conta fino a 500 (durata totale 5 secondi).
Il terzo scatta ogni 100ms e conta fino a 50 (durata totale 5 secondi).

Si preveda un quarto TextBlock che visualizzi il totale dei tre contatori (alla fine visualizzerà 5.550)

Incremento del contatore a schermo
Se vogliamo stampare e vedere a schermo un numero che continua ad incrementarsi dobbiamo utilizzare la funzione "sleep", che addormenta per un tot di tempo il sistema. Questa azione non è comunque consigliata ed è quindi meglio creare un processo separato su cui spostare il thread lento.

Problema degli oggetti del thread principale
Quando andiamo ad eseguire il programma ci troveremo però un problema davanti:
Il thread chiamante non riesce ad accedere agli oggetti di proprietà del thread principale . Per consentire la comunicazione in modo sicuro con il thread principale verrà utilizzato il Dispatcher. In più introduciamo una variabile TotalCount su cui ogni Thread dovrà avere accesso per incrementare il suo valore.

Andiamo a parlare ora di alcduni elementi fondamentali nel multithreading: i semafori:
I semafori sono costrutti di basso livello del sistema operativo, che lavorano utilizzando due istruzioni principali:
1. Segnale : Serve a segnalare al sistema che un insieme di istruzioni è stato eseguito con successo, oppure per segnalare la terminazione di un thread.
2. Attesa: E' una procedura bloccante che mette in attesa il sistema finché tutte le istruzioni interessate non finiscono la loro esecuzione.

Problema della sincronia di terminazione
Durante l'esecuzione del programma ci troveremo di fronte ad un problema: non tutti i contatori finiscono nello stesso momento.
Questo è dovuto ad un problema delle API di Windows che non riesce a mettere in pausa il Thread per 1ms preciso nel contatore da 5000 non rispettato, bensì riesce solamente a rappresentare un'approssimazione di attorno ai 15ms.
Per risolverlo utilizzeremo una lambda expression, il cui metodo girerà in loop con una pausa di 1ms nel mezzo e una volta che il contatore raggiunge il valore prestabilito terminerà il timer tramite il suo id e segnalerà al Semaforo la sua terminazione.

