>[!note]
>Datalog è un linguaggio query basato sul Prolog. È un linguaggio basato sulla programmazione logica, che è un paradigma di programmazione basato su delle regole.
>
>Ogni regola è composta da una testa (LHS) e da un corpo (RHS) seguendo la sintassi:
>```datalog
>P :- P1, P2, ..., Pn
>```
>Dove ogni $P$ rappresenta un predicato composto da nome, eventuale negazione e argomenti (possono essere costanti, variabili, simbolo "don't care"). Hanno l'interpretazione che la testa è vera se il corpo è vero.
>
>Ciascuna $n$-upla di una base di dati corrisponde ad un fatto, cioè una regola in cui la testa è un letterale senza variabili, il cui corpo non contiene condizioni ed è omesso.
>
>Per eseguire una query (detta goal in datalog) si usa la sintassi:
>```datalog
>? :- P1, P2, ..., Pn
>```
>Che restituirà un insieme per le variabili trovate. In caso non ci siano variabili un goal restituisce `true` o `false`.

Consideriamo in questa pagina la base di dati contenente le tabelle $\text{Persona}(\underline{\text{Nome}},\text{Età},\text{Sesso})$ e $\text{Genitore}(\underline{\text{Genitore}},\underline{\text{Figlio}})$. Degli esempi di regole possono essere:
```datalog
Padre(X, Y) :- Persona(X, _, 'M'), Genitore(X, Y)
Madre(X, Y) :- Persona(X, _, 'F'), Genitore(X, Y)
```

Un fatto di questa base di dati può essere:
```datalog
Genitore("Carlo", "Antonio")
```

Un esempio di Goal può essere:
```datalog
? :- Padre("Carlo", X)
```

Che restituirà $X=\set{\text{Antonio},\space\text{Gianni}}$.

>[!tip] Corrispondenza con l'algebra relazionale
>Possiamo vedere una corrispondenza tra Datalog e l'algebra relazionale. Le variabili in testa sono l'equivalente delle proiezioni, i predicati sono l'equivalente delle selezioni e l'insieme di predicati è l'equivalente dei join tra loro.
>
>Per esempio l'espressione corrispondente di $\text{Padre}$ in algebra relazionale sarebbe: $$\text{Padre}=\Pi_{1,5}\sigma_{3='\text{M}'}\text{ Persona}\Join_{1 = 4}\text{Genitori}$$

>[!tip] Database estensionale e intensionale
>Definiamo il database estensionale (EDB) come l'insieme delle tabelle presenti nel DB, e il database intensionale (IDB) come l'insieme dei predicati che sono a sinistra in una regola, che sono quindi la conoscenza dedotta a partire da EDB. Si impone che: $$\text{EDB}\cap\text{IDB}=\emptyset$$

>[!tip]
>Nella definizione delle regole si possono usare predicati speciali, come operatori di confronto ($=$, $\neq$, $<$, $\leq$, $>$, $\geq$) e funzioni aritmetiche ($+$, $-$, $*$, $\div$). Tuttavia bisogna stare attenti a non creare "cicli infiniti".

### Negazione in Datalog
>[!note]
>Alcuni letterali del corpo possono essere negati tramite l'espressione `not`. L'uso della negazione in Datalog aumenta il potere espressivo del linguaggio, tuttavia richiede cautela poiché può portare a risultati infiniti.

Senza la negazione Datalog permette di rappresentare gli operatori $\set{\sigma,\Pi,\times,\cup}$, con $P=R\cup S$ rappresentato da:
```datalog
P(X, Y) :- R(X, Y)
P(X, Y) :- S(X, Y)
```

Per ottenere la differenza serve il `not`:
```datalog
P(X, Y) :- R(X, Y), not S(X, Y)
```

>[!tip] Evitare risultati infiniti
>Per evitare risultati infiniti la negazione dev'essere safe, quindi tutte le variabili di un letterale negato devono comparire in un letterale positivo del corpo della regola. Questo esclude regole come:
>```datalog
>S(X, Y) :- R(X), not Q(Y)
>```
>
>Inoltre, la negazione dev'essere stratificata, quindi non ci devono essere cicli di dipendenza tra letterali negati. Questo esclude regole come:
>```datalog
>P(X) :- R(X), not P(x)
>```

### Query ricorsive
>[!note]
>È possibile effettuare query ricorsive, dove il letterale della testa si presenta all'interno del corpo della regola. Per esempio:
>
>```datalog
>Antenato(X, Y) :- Genitore(X, Y)
>Antenato(X, Y) :- Antenato(X, Z), Genitore(Z, Y)
>```
>
>Si può immaginare che questo processo sia eseguito iterativamente: $$\begin{align*}
>\text{Antenato}^{0}&\leftarrow \text{Genitori}\\
>\text{Antenato}^{1}&\leftarrow (\Pi_{1,4}\text{ Antenato}^{0}\Join_{2=1}\text{Genitori})\cup \text{Antenato}^{0}\\
>\text{Antenato}^{2}&\leftarrow (\Pi_{1,4}\text{ Antenato}^{1}\Join_{2=1}\text{Genitori})\cup \text{Antenato}^{1}\\
>\vdots\\
>\text{Antenato}^{n}&\leftarrow (\Pi_{1,4}\text{ Antenato}^{n-1}\Join_{2=1}\text{Genitori})\cup \text{Antenato}^{n-1}
>\end{align*}$$
>Fino a quando $\text{Antenato}^{n}=\text{Antenato}^{n-1}$.

>[!tip] Potere espressivo di Datalog
>Siccome Datalog dispone di query ricorsive, è chiuso transitivamente. Questo non vale per l'algebra relazionale e di conseguenza Datalog ha più potere espressivo dell'algebra relazionale.

