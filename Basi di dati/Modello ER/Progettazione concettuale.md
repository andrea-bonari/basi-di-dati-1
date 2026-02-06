>[!note]
>La progettazione di un DB si articola nelle fasi di progettazione concettuale, logica e fisica.
>
>La progettazione concettuale ha lo scopo di tradurre il risultato dell'analisi dei requisiti in una descrizione formale, indipendente dal DBMS e dal modello logico. La descrizione formale è espressa in uno schema concettuale, costruito utilizzando un modello concettuale dei dati.
>
>Per la progettazione concettuale ci serviamo del modello ER.

Nei modelli concettuali facciamo uso di astrazioni, cioè la capacità di evidenziare caratteristiche comuni ad insiemi di oggetti. Di base abbiamo tre astrazioni: classificazione, aggregazione e generalizzazione.

La classificazione è la capacità di definire classi di oggetti o fatti del mondo reale.

L'aggregazione è la costruzione di una classe complessa aggregando classi più semplici (componenti).

Infine la generalizzazione stabilisce legami di sottoinsieme fra classi, dove tutte le proprietà definite per la superclasse sono ereditate dalla sottoclasse.

### Modello Entità-Relazione
>[!note]
>Il modello Entità-Relazione (ER) è lo standard industriale di buona parte delle metodologie e degli strumenti per il progetto concettuale di basi di dati.
>
>Definiamo un'entità come una classe di oggetti o fatti rilevanti per l'applicazione. Ogni entità è caratterizzata da un nome. Queste sono rappresentate con un quadrato.
>
>Definiamo poi una relazione come un'aggregazione di entità di interesse per l'applicazione. Ogni istanza di una associazione è una $n$-upla tra istanze di entità. Ogni associazione è caratterizzata da un nome. Queste sono rappresentate con un rombo.
>
>Definiamo infine gli attributi come caratteristiche delle entità e delle associazioni di interesse per l'applicazione. Anche questi sono caratterizzati da un nome, e rappresentati da un pallino collegato ad una entità o una relazione.

>[!tip] Cardinalità delle associazioni
>Per cardinalità si intende un vincolo sul numero di istanze di associazione cui ciascuna istanza di entità deve partecipare. È una coppia $(\text{MIN-CARD},\text{MAX-CARD})$. Le più comuni sono $1:1$, $1:n$ e $n:m$. È possibile inoltre specificare l'opzionalità indicando $\text{MIN-CARD}=0$.

È possibile effettuare auto-associazioni, cioè associazioni aventi come partecipanti istanze provenienti dalla stessa entità.

>[!tip] Attributi
>Gli attributi possono essere scalari oppure multipli (indicati con $(m:n)$ che ne esprime la cardinalità e opzionalità). Inoltre si può indicare una composizione se a loro si collegano altri attributi.
>![[Pasted image 20260206153019.png|center]]
>
>Se un attributo è un identificatore non modificabile lo indichiamo con un pallino pieno.
>
>Se questo è una composizione di due attributi facciamo passare la sua linea attraverso tali attributi.
>
>![[Pasted image 20260206153158.png|center]]
>
>

>[!tip] Entità deboli
>Le entità deboli possono esistere se e solo se sono presenti entità "forti" da cui queste dipendono. Infatti, in caso di eliminazione dell'istanza "forte" di riferimento, le istanze deboli collegate devono essere eliminate.
>
>Si identifica tra le chiavi deboli una chiave parziale, che è associata con la chiave di un'altra entità tramite relazione, detta identificante.
>
>Per essere univocamente identificata, l'entità debole deve partecipare alla relazione identificante con cardinalità $(1,1)$, cioè per ogni istanza dell'entità debole deve esistere esattamente un'istanza dell'entità proprietaria che la identifica.
>
>Un identificazione esterna può coinvolgere anche più entità proprietarie. Inoltre, un'entità proprietaria può essere a sua volta debole (purché non si formino cicli di entità deboli).
>![[Pasted image 20260206155254.png|center]]

>[!tip] Gerarchie di generalizzazione
>Una gerarchia di generalizzazione è un legame tra un'entità padre $E$ ed alcune entità figlie $E_{1},E_{2},\cdots,E_{n}$ tale per cui:
>- Ogni istanza di $E_{k}$ è anche istanza di $E$
>- Un'istanza di $E$ può essere un'istanza di $E_{k}$
>
>In questa associazione di generalizzazione/specializzazione, le entità figlio ereditano le proprietà dell'entità padre.
>
>![[Pasted image 20260206155607.png|center]]
>
>Le gerarchie possono essere:
>- Totali (T): ogni istanza dell'entità padre deve far parte di almeno una delle entità figlie
>- Parziali (P): le istanze dell'entità padre possono far parte di una delle entità figlie
>Inoltre possono essere:
>- Esclusive (E): ogni istanza dell'entità padre non può far parte di più di una delle entità figlie
>- Overlapping (O): ogni istanza dell'entità padre può far parte di più delle entità figlie
>
>Il tipologia di gerarchi si identifica con una tupla.

### Strategie di progetto concettuale
>[!note]
>Lo sviluppo dello schema si può eseguire seguendo le strategie top-down e bottom-up.
>
>Nella strategia top-down si procede per raffinamenti a partire da una descrizione che comprende tutta la realtà di interesse, mentre nella strategia bottom-up si disegnano separatamente aspetti della realtà e poi li si integra costruendo un unico schema.

>[!tip] Strategia top-down "pura"
>A partire dalle specifiche si costruisce uno schema iniziale. A partire da esso si arriva per raffinamenti successivi a schemi intermedi e poi allo schema finale.
>
>I raffinamenti prevedono l'uso di trasformazioni elementari (primitive) che operano sul singolo concetto per descriverlo con maggior dettaglio.
>
>Questa strategia ha come vantaggio la possibilità di descrivere inizialmente lo schema trascurando i dettagli, precisandoli poi gradualmente, tuttavia non va bene per applicazioni complesse perché è difficile avere una visione globale precisa iniziale di tutte le componenti del sistema.

>[!tip] Strategia bottom-up "a macchia d'olio"
>Nella strategia a macchia d'olio le specifiche nascono progressivamente, affrontando i requisiti fino al massimo dettaglio e "avanzando" per sottoproblemi.
>
>La tecnica è adatta a tradurre pian piano una descrizione testuale in un diagramma. Pur essendo bottom-up il progettista analizza le specifiche in modo "stratificato" e le aggiunge progressivamente ad un unico schema, perciò i conflitti sono meno probabili.

>[!tip] Strategia mista
>La strategia mista è adatta a progetti ampi. Si suddividono le specifiche in parti. Le varie parti si realizzano in top-down, mentre l'integrazione tra queste si sviluppa in bottom-up.
>
>Questo approccio permette la collaborazione tra più progettisti in sezioni parziali, tuttavia c'è il rischio di conflitti e difficoltà di integrazione. Per ovviare il problema è possibile sviluppare un piccolo schema scheletro dei concetti principali in modo top-down e attenersi alle scelte presenti nello schema scheletro in tutti gli altri schemi.

### Qualità di uno schema concettuale
>[!note]
>Uno schema concettuale ha come qualità: completezza, correttezza, leggibilità, minimalità e auto-esplicatività.
>

Per quanto riguarda la completezza e correttezza, è necessario assicurarsi (per la completezza) che i dati consentano di eseguire tutte le applicazione e (per la correttezza) che sia possibile popolare la base di dati anche con informazione parzialmente incompleta durante le fasi iniziali della sua evoluzione.

La leggibilità è più un aspetto grafico, è consigliabile disegnare su una griglia.

Per quanto riguarda la minimalità, è necessario individuare e documentare le ridondanze, senza necessariamente eliminarle. Si pone un attenzione particolare ai cicli, che potrebbero indicare dei possibili conflitti nei dati
### Post-processing
>[!note]
>Dopo la creazione del diagramma ER si passa all'ultimo passo di post-processing, dove si verifica che:
>- Tutte le entità hanno un identificatore
>- Tutte le associazioni hanno cardinalità ben definita
>- Tutte le entità sono significative (consentono di rappresentare più di un attributo o siano collegate ad altre entità tramite associazioni)
>- Le generalizzazioni sia utili (consentano di ereditare proprietà)
>- I cicli (se presenti) siano ben documentati

