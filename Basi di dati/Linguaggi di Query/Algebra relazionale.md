>[!note]
>L'algebra relazionale è un linguaggio formale di interrogazione delle basi dei dati. In questo linguaggio esiste un insieme minimo di 5 operazioni che danno l'intero potere espressivo del linguaggio, dove gli operandi sono le tabelle. Le espressioni algebriche esprimono interrogazioni in modo formale, e consentono di estrarre informazioni dai dati.

>[!tip] Operazione di Selezione 
>L'operazione di selezione $\sigma_{\text{Attr='Val'}}\text{Tabella}$, e ci restituisce una tabella con lo stesso schema di $\text{Tabella}$, e con istanze le $n$-uple di $\text{Tabella}$ che soddisfano il predicato di selezione. Il predicato di selezione permette di usare operazioni booleane ($\text{AND}, \text{ OR}, \text{ NOT}$), comparatori ($\text{=},\space\text{!=},\text{ <},\text{ <=},\text{ >},\text{ >=}$), e termini (costanti e attributi).

>[!tip] Operazione di proiezione
>L'operazione di proiezione $\Pi_{\text{Attr1,Attr2}}\text{Tabella}$, che restituisce una tabella con schema gli attributi selezionati, e con istanza la restrizione delle $n$-uple sugli attributi selezionati. L'operatore $*$ restituisce tutte le colonne. Formalmente la proiezione elimina i duplicati, tuttavia informalmente l'eliminazione va richiesta esplicitamente. 

>[!tip] Operazioni di unione, intersezione e differenza
>Gli operatori di unione $\cup$ e intersezione $\cap$ restituiscono rispettivamente l'unione e intersezione delle $n$-uple delle tabelle specificate. È possibile solo se i domini delle tabelle sono ordinatamente dello stesso tipo. Formalmente l'intersezione è ricavabile con la formula: $$R\cap S= R-(R-S)$$
>L'operatore di differenza $-$ restituisce la differenza delle $n$-uple delle tabelle specificate, rimuove quindi le righe della prima tabella in comune con la seconda.

>[!tip] Operazione di prodotto cartesiano
>Il prodotto cartesiano $\times$ restituisce una tabella priva di nome con schema gli attributi delle due tabelle, e con istanza tutte le possibili coppie di $n$-uple delle due tabelle.

>[!tip] Operazione di join
>L'operazione di join $\text{Tabella1}\Join_{\text{Tabella1.attr=Tabella2.attr}}\text{Tabella2}$ è equivalente all'espressione: $$\sigma_\text{Tabella1.attr=Tabella2.attr}\text{Tabella1}\times\text{Tabella2}$$
>Produce quindi una tabella con schema concatenazione degli schemi di $\text{Tabella1}$ e $\text{Tabella2}$, e con istanze le $n$-uple ottenute concatenando le tuple di $\text{Tabella1}$ e $\text{Tabella2}$ che soddisfano il predicato. Dentro l'operazione di join si possono utilizzare tutti i predicati booleani.
>
>Si parla di equi-join quando si fanno solo confronti di uguaglianza, e di join naturale quando un equi-join si fa di tutti gli attributi omonimi, in questo caso si omette il predicato e si elimina la colonna ripetuta.
>
>Nell'operazione di semi-join $\ltimes$ è equivalente alla seguente espressione: $$\Pi_\text{Tabella1.*}(\text{Tabella1}\Join_\text{"condizione"}\text{Tabella2})$$
Anche in questo caso esiste il semi-join naturale.

>[!tip] Assegnamento e ridenominazione
>L'assegnamento è un operazione utilizzata per dare un nome al risultato di un'espressione algebrica, non fa parte delle operazioni algebriche: $$\text{Assegnazione}=\sigma_{\text{Attr='Val'}}\text{Tabella}$$
>La ridenominazione permette di generare una tabella in cui un attributo ha nomi diversi, anche questa non fa parte delle operazioni algebriche: $$\rho_{\text{Rinominato }\leftarrow\text{ Attributo}}\text{Tabella}$$

Ogni espressione dell'algebra relazionale può essere rappresentata in modo grafico da un albero. Rappresenta l'ordine di valutazione degli operandi, dove ogni operatore corrisponde a un nodo.

### Ottimizzazione di query
>[!note]
>Esistono delle equivalenze tra query, tra cui:
>- Eliminazione dei prodotti cartesiani. Se $p$ è una congiunzione di predicati del tipo $\text{Attr comp Attr}$, allora: $$\sigma_{p}\space R\times S\equiv R\Join_{p} S$$
>- Push della selezione rispetto al join. Se $p$ è un predicato che si applica ai soli attributi di $R$, allora: $$\sigma_{p} R\Join_{Q} S\equiv (\sigma_{p}\space R)\Join_{Q} S$$
>- Push della proiezione rispetto al join. Definendo $\text{JR}$ e $\text{JS}$ come gli attributi di $R$ e $S$ necessari a valutare $Q$, e definendo $\text{LR}=L-\text{schema}(S)+\text{JR}$ e $\text{LS}=L-\text{schema}(R)+\text{JS}$, allora: $$\Pi_{L}\space R\Join_{Q} S\equiv \Pi_{L}\space (\Pi_{\text{LR}}\space R)\Join_{Q}(\Pi_{\text{LS}}\space S)$$
>- Idempotenza della selezione. Si ha che per $p= p_{1}\land p_{2}$: $$\sigma_{p} \space R\equiv \sigma_{p_{1}}\sigma_{p_{2}}\space R$$
>- Idempotenza della proiezione. Si ha che per $L_{1}=L$ e $L\subseteq L_{2}$: $$\Pi_{L}\space R\equiv \Pi_{L_{1}}\Pi_{L_{2}}\space R$$
>- Push della selezione rispetto all'unione. Si ha che: $$\sigma_{p}\space R\cup S\equiv (\sigma_{p}\space R)\cup(\sigma_{p}\space S)$$
>- Push della selezione rispetto alla differenza. Si ha che: $$\sigma_{p}\space R-S\equiv (\sigma_{p}\space R)-(\sigma_{p}\space S)$$
>- Push della proiezione rispetto all'unione. Si ha che: $$\Pi_{L}\space R\cup S\equiv (\Pi_{L}\space R)\cup(\Pi_{L}\space S)$$
>- Commutazione di join e unione. Si ha che: $$(R_{1}\cup R_{2})\Join(S_{1}\cup S_{2})\equiv (R_{1}\Join S_{1})\cup(R_{1}\Join S_{2})\cup(R_{2}\Join S_{1})\cup(R_{2}\Join S_{2})$$
>

Oltre a queste esistono delle formule come: $$\begin{align*}
&R\cap R= R\\
&R\cup R= R\qquad &\sigma_{P}\space \emptyset=\emptyset\\
&R-R=\emptyset\qquad &\Pi_{L}\space \emptyset =\emptyset\\
\\
&R\cap (\sigma_{P}\space R)=\sigma_{P}\space R\qquad &R\cup\emptyset=R\\
&R\cup (\sigma_{P}\space R)=R\qquad &R-\emptyset=R\\
&R-(\sigma_{P}\space R)=\sigma_{\lnot P}\space R\qquad &\emptyset-R=\emptyset\\
&&R\cap\emptyset =\emptyset\\
&\sigma_{P_{1}}\space R\cap \sigma_{P_{2}}\space R=\sigma_{P_{1}\land P_{2}}\space R\\
&\sigma_{P_{1}}\space R\cup \sigma_{P_{2}}\space R=\sigma_{P_{1}\lor P_{2}}\space R\qquad&R\times\emptyset=\emptyset\\
&\sigma_{P_{1}}\space R- \sigma_{P_{2}}\space R=\sigma_{P_{1}\land \lnot P_{2}}\space R\qquad &R\Join \emptyset=\emptyset
\end{align*}$$
Informalmente per ottimizzare algebricamente si minimizza la dimensione dei risultati intermedi utilizzando dove possibile le trasformazioni di push, e utilizzando le trasformazioni di idempotenza per generare nuove selezioni e proiezioni.
