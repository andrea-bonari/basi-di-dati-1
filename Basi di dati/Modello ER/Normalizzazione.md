>[!note]
>Può capitare che, per carenze di specifiche, o errori di schematizzazione, siano presenti delle relazioni con anomalie. Per evitarle utilizziamo il processo di normalizzazione.
>
>Questo processo viene svolto generalmente durante la fase di progettazione logica.

### Dipendenze funzionali
>[!note]
>Una dipendenza funzionale è un vincolo di integrità per il modello relazionale. 
>
>Facendo un esempio, dall'osservazione delle relazioni osserviamo che ogni volta che in una tupla compare un certo impiegato, lo stipendio è sempre lo stesso. Pertanto, il valore dell'impiegato determina il valore dello stipendio, cioè esiste una funzione che associa ad ogni valore nel dominio impiegato uno ed un solo valore nel dominio stipendio.
>
>Formalmente, data una relazione $R$ definita su uno schema $S(X)$, e due sottoinsiemi di attributi $Y$ e $Z$ non vuoti di $X$, esiste una dipendenza funzionale $Y\to Z$, se, per ogni coppia di tuple $t_{1}$ e $t_{2}$ aventi lo stesso valore di $Y$, risulta che hanno lo stesso valore di $Z$
>
>Questo concetto generalizza il vincolo di chiave.

Per non avere anomalie, è necessario avere che l'antecedente della dipendenza funzionale deve essere una chiave.

È possibile eliminare dipendenze parziali scomponendo la tabella in più tabelle.
Se la relazione originale è ricostruibile con il `join`, la decomposizione è corretta e si dice essere senza perdita.

Abbiamo che le ridondanze e le anomalie sono causate da dipendenze $X\to Y$ tali che $X$ non contiene la chiave della relazione. 

>[!tip] Forma normale di Boyce Codd (BCNF)
>Una relazione $R$ è in forma normale (Boyce e Codd) se, per ogni dipendenza $X\to Y$ in $R$, $X$ contiene una chiave $K$ di $R$ ($X$ è superchiave).
>
>Questa forma normale non è sempre raggiungibile.

>[!tip] Qualità delle decomposizioni
>Le decomposizioni dovrebbero sempre soddisfare le proprietà di decomposizione senza perdita e conservazione delle dipendenze:
>- La decomposizione senza perdita assicura che l'informazione della relazione originale possa essere ricostruita a partire dall'informazione contenuta nelle relazioni decomposte
>- La conservazione delle dipendenze garantisce che le relazioni decomposte abbiano la stessa capacità di rappresentare i vincoli di integrità della relazione originale e quindi di impedire gli aggiornamenti illegali.

### Terza forma normale
>[!note]
>Una relazione $R$ è in terza forma normale (3FN) se, per ogni dipendenza funzionale (non banale) $X\to Y$ definita su di essa, almeno una delle seguenti condizioni è verificata:
>- $X$ contiene una chiave $K$ di $R$
>- $Y$ è contenuto in almeno una chiave di $R$
>
>A differenza della BCNF ha il vantaggio che si può sempre ottenere preservando tutte le dipendenze.

Per decomporre in 3FN si crea una relazione per ogni gruppo di dipendenza funziona che hanno lo stesso lato sinistro (determinante), e si inserisce nello schema corrispondente gli attributi coinvolti in almeno una dipendenza funzionale del gruppo.

Ad esempio, se le dipendenze funzionali identificate sullo schema $R(\underline{AB}CDEFG)$ sono: $$AB\to CD,\quad AB\to E,\quad C\to F, \quad F\to G$$
Si genereranno gli schemi $R_{1}(\underline{AB}CDE)$, $R_{2}(\underline{C}F)$, $R_{3}(\underline{F}G)$.
Se due o più determinanti si determinano reciprocamente, si fondono gli schemi.

Alla fine del processo si verifica che esiste uno schema la cui chiave è anche chiave dello schema originario (se non esiste la si crea).

>[!tip] Altre forme normali
>Una relazione è in 1FN se e solo se ciascun attributo è definito su un dominio con valori atomici. La relazione ha una chiave: ogni attributo non chiave dipende funzionalmente dalla chiave.
>
>Una relazione è in 2FN se è in 1FN e inoltre tutti gli attributi non chiave dipendono funzionalmente dall'intera chiave. Tale normalizzazione elimina le dipendenze parziali.
>
>Da queste due definizioni possiamo dire che una relazione è in 3FN se è in 2FN e inoltre tutti gli attributi non-chiave dipendono direttamente dalla chiave (cioè non possiede attributi non-chiave che dipendono da altri attributi non-chiave). Tale normalizzazione elimina le dipendenze transitive.

La normalizzazione elimina le anomalie, tuttavia può appesantire l'esecuzione di certe operazioni. Bisogna valutare in base alla frequenza con cui i dati vengono modificati, e sull'incisione dell'occupazione di memoria della ridondanza.
