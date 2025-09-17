>[!note]
>Per interrogare le basi di dati si usano tradizionalmente linguaggi programmativi, come SQL o QBE. Esistono anche dei linguaggi formali come l'algebra relazionale, il calcolo relazionale e la programmazione logica per interrogare le basi di dati.

### Algebra relazionale
>[!note]
>L'algebra relazionale è un linguaggio formale di interrogazione delle basi dei dati. In questo linguaggio esiste un insieme minimo di 5 operazioni che danno l'intero potere espressivo del linguaggio, dove gli operandi sono le tabelle. Le espressioni algebriche esprimono interrogazioni in modo formale, e consentono di estrarre informazioni dai dati.
>

L'operazione di selezione $\sigma_{\text{Attr='Val'}}\text{Tabella}$, e ci restituisce una tabella con lo stesso schema di $\text{Tabella}$, e con istanze le $n$-uple di $\text{Tabella}$ che soddisfano il predicato di selezione. Il predicato di selezione permette di usare operazioni booleane ($\text{AND}, \text{ OR}, \text{ NOT}$), comparatori ($\text{=},\space\text{!=},\text{ <},\text{ <=},\text{ >},\text{ >=}$), e termini (costanti e attributi).

L'operazione di proiezione $\Pi_{\text{Attr1,Attr2}}\text{Tabella}$, che restituisce una tabella con schema gli attributi selezionati, e con istanza la restrizione delle $n$-uple sugli attributi selezionati. L'operatore $*$ restituisce tutte le colonne. Formalmente la proiezione elimina i duplicati, tuttavia informalmente l'eliminazione va richiesta esplicitamente. 

Gli operatori di unione $\cup$ e intersezione $\cap$ restituiscono rispettivamente l'unione e intersezione delle $n$-uple delle tabelle specificate. È possibile solo se i domini delle tabelle sono ordinatamente dello stesso tipo. Formalmente l'intersezione è ricavabile con la formula: $$R\cap S= R-(R-S)$$
L'operatore di differenza $-$ restituisce la differenza delle $n$-uple delle tabelle specificate, rimuove quindi le righe della prima tabella in comune con la seconda.

Il prodotto cartesiano $\times$ restituisce una tabella priva di nome con schema gli attributi delle due tabelle, e con istanza tutte le possibili coppie di $n$-uple delle due tabelle.

L'operazione di join $\text{Tabella1}\Join_{\text{Tabella1.attr=Tabella2.attr}}\text{Tabella2}$ è equivalente all'espressione: $$\sigma_\text{Tabella1.attr=Tabella2.attr}\text{Tabella1}\times\text{Tabella2}$$
Produce quindi una tabella con schema concatenazione degli schemi di $\text{Tabella1}$ e $\text{Tabella2}$, e con istanze le $n$-uple ottenute concatenando le tuple di $\text{Tabella1}$ e $\text{Tabella2}$ che soddisfano il predicato. Dentro l'operazione di join si possono utilizzare tutti i predicati booleani.

Si parla di equi-join quando si fanno solo confronti di uguaglianza, e di join naturale quando un equi-join si fa di tutti gli attributi omonimi, in questo caso si omette il predicato e si elimina la colonna ripetuta.

Nell'operazione di semi-join $\ltimes$ è equivalente alla seguente espressione: $$\Pi_\text{Tabella1.*}(\text{Tabella1}\Join_\text{"condizione"}\text{Tabella2})$$
Anche in questo caso esiste il semi-join naturale.

>[!tip] Assegnamento e ridenominazione
>L'assegnamento è un operazione utilizzata per dare un nome al risultato di un'espressione algebrica, non fa parte delle operazioni algebriche: $$\text{Assegnazione}=\sigma_{\text{Attr='Val'}}\text{Tabella}$$
>La ridenominazione permette di generare una tabella in cui un attributo ha nomi diversi, anche questa non fa parte delle operazioni algebriche: $$\rho_{\text{Rinominato }\leftarrow\text{ Attributo}}\text{Tabella}$$