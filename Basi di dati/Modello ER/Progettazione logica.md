>[!note]
>La progettazione logica ha come scopo tradurre lo schema concettuale in uno schema logico, scelto all'interno dei modelli logici dei dati. Questi possono essere gerarchici, reticolari, relazionali, orientati agli oggetti o XML.
>
>Lo schema logico è dipendente dal tipo di modello implementato dal DBMS ma non dallo specifico prodotto.
>
>La progettazione logica si articola in:
>1. Eliminazione delle gerarchie
>2. Selezione delle chiavi primarie
>3. Normalizzazione di attributi composti/multipli
>4. Eliminazione di relazioni
>
>Il risultato di questi passaggi è un insieme di tabelle con primary key e foreign key.

>[!tip] Eliminazione delle gerarchie
>Per eliminare alle gerarchie si può ricorrere alla soluzione orizzontale, dove si pone una sola entità in corrispondenza del padre, con l'aggiunta di un attributo `flag` che indica il tipo di istanza, e attributi opzionali dei rispettivi figli.
>![[Pasted image 20260206162208.png|center]]
>
>In alternativa, si può ricorrere alla soluzione verticale, dove si pone $n$ entità corrispondenti agli $n$ figli.
>![[Pasted image 20260206162314.png|center]]

>[!tip] Selezione delle chiavi primarie
>Per scegliere le chiavi primarie generalmente si sceglie l'identificatore univoco più usato, purché semplice. Se questo è troppo complesso, si introduce un codice.
>
>Infine, gli identificatori esterni delle entità deboli vanno resi interni.

>[!tip] Normalizzazione di attributi composti/multipli
>Gli attributi composti vanno resi semplici, usando un solo attributo oppure più attributi.
>
>![[Pasted image 20260206162708.png|center]]
>
>Gli attributi multipli (cardinalità non $(1,1)$) inoltre, vanno gestiti con un'entità separata.
>
>![[Pasted image 20260206164023.png|center]]

>[!tip] Eliminazione di relazioni
>Consideriamo due entità $E_{1}$ e $E_{2}$ con chiavi primarie $k_{1}$ e $k_{2}$.
>
>In una relazione $m:n$ si crea una nuova tabella per l'associazione, con chiavi primarie $k_{1},k_{2}$, che sono anche chiavi esterne per $E_{1}$ ed $E_{2}$. Gli attributi della relazione sono salvati in questa tabella.
>
>In una relazione $1:n$, si inserisce la chiave esterna nella tabella dal lato $n$. Gli attributi della relazione sono salvati nella tabella del lato $n$.
>
>In una relazione $1:1$, in caso di partecipazione totale di un'entità, si inserisce la chiave esterna nella tabella dell'entità con partecipazione totale, mentre in caso di partecipazione parziale di entrambe, si può scegliere una delle due tabelle oppure creare una tabella separata.