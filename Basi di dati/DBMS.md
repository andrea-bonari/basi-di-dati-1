>[!note]
>I DataBase Management System (DBMS) sono software che permettono di creare e gestire basi di dati in modo organizzato ed efficiente. I DBMS devono permettere la condivisione dei dati e garantire la loro qualità, garantendo inoltre la loro efficienza, sicurezza e robustezza.
>
>Per farlo ogni dato, a prescindere dalle applicazioni dalle quali viene utilizzato, compare una sola volta. Questo è fatto per eliminare inutili ridondanze, e migliorare la consistenza dei dati.
>
>I DBMS funzionano definendo la struttura generale dei dati (schema) e successivamente definendo le specifiche operazioni sui dati. I dati in sé, dentro lo schema, sono detti istanza.
>

Il DBMS utilizza due linguaggi:
- DDL (Data Definition Language): Per manipolare lo schema.
- DML (Data Manipulation Language): Per manipolare le istanze.

### Modello relazionale
>[!note]
>Il modello relazionale è un modello di dati dove sono rappresentati come tabelle, e le associazioni tra dati sono ottenute associando valori di attributi in tabelle diverse.
>
>Dal punto di vista matematico si ha che il grado della relazione è il numero di domini $D$, La cardinalità della relazione è il numero di $n$-uple, e l'attributo è il nome dato al dominio di una relazione.

>[!tip] Relazione matematica
>Una relazione su $D_{1},\cdots, D_{n}$ è un sottoinsieme $R$ tale che: $$R\subseteq D_{1}\times\cdots\times D_{n}$$
>Questa è detta relazione $n$-aria. Nel caso reale si ha che $\#(R)\neq+\infty$.

I riferimenti fra dati in relazioni diverse sono rappresentati per mezzo di valori dei domini che compaiono nelle $n$-uple.

Questa organizzazione delle tabelle permette:
- Indipendenza dalle strutture fisiche che possono cambiare anche dinamicamente.
- Possibilità di rappresentare solo ciò che è rilevante dal punto di vista dell'utente.
- Portabilità dei dati da un sistema all'altro.

Talvolta è possibile che le informazioni in una istanza siano incomplete, in questo caso si utilizza il valore `NULL`. È tuttavia necessario imporre restrizioni sulla presenza di valori nulli.

>[!tip] Vincoli di integrità
>I vincoli di integrità escludono istanze in quanto non rappresenta correttamente .
>I vincoli di integrità si distinguono in vincoli intrarelazionali e vincoli interrelazionali.
>
>Tra i vincoli intrarelazionali troviamo il vincolo di $n$-upla, cioè una condizione sui valori di ciascuna $n$-upla, indipendentemente dalle altre $n$-uple.

>[!tip] Chiavi
>Le chiavi sono un sottoinsieme degli attributi dello schema che hanno la proprietà di unicità (non esistono due $n$-uple con chiave uguale) e minimalità (sottraendo un qualunque attributo alla chiave si perde la proprietà di unicità).
>
>Se il sottoinsieme non è minimo si parla di superchiave. In questo caso una delle chiavi è detta primaria mentre le altre sono secondarie.
>
>In presenza di valori nulli, i valori degli attributi che formano la chiave non permettono di identificare le $n$-uple come desiderato, per questo la presenza di valori nulli nelle chiavi deve essere limitate.

