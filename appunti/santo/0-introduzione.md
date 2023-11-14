Un **formalismo** è una descrizione matematica rigorosa su un ragionamento, dunque può essere in diversi modi: espressioni algebriche, grafici, o altro.
Un **formalismo di calcolo** risponde alla domanda su cosa voglia dire "computare". 

Alcuni esempi di questi ultimi sono la macchina di Turing, il $\lambda$ calcolo, un linguaggio di programmazione. Vi sono diversi causa la **tesi di Church-Turing** la quale è una congettura: *Ogni funzione calcolabile da un formalismo di calcolo sufficientemente espressivo è calcolabile da una macchina di Turing e viceversa*, dunque ogni formalismo può essere calcolato usando la macchina di Turing. Tutti sono equivalenti. Tutti i formalismi non possono calcolare *tutti i problemi matematici*. Si sceglie il formalismo in base ai pro/cons: ad esempio la scelta di un linguaggio è dato da quanto è veloce a compilare o dalla semplicità nella sintassi.

## Macchina di Turing
Come si calcola dal punto di vista meccanico? Si usa un supporto fisico per mantenere i dati, fatto da celle discrete (eg: cella della lavagna) contenenti finite informazioni. Il supporto fisico è infinito perché, sennò, avremmo un numero finito di combinazioni di dati e non risolvere ogni problema possibile. In probabilità usiamo il concetto di input che può essere infinito, anche se realmente non è nel calcolatore.
Nel nastro vi è una testina che legge la cella muovendosi in essa verso dx o sx. In base allo stato della macchina decide se scrivere o leggere l'informazione. Meccanicamente non potrebbe implementare infiniti stati.

Definita con una tupla $(A, Q, q_0, q_f, \delta)$ dove
$A \neq \emptyset$ chiamato alfabeto composto da un numero finito di simboli.
$Q \neq \emptyset$ è l'insieme degli stati finiti.
$q_0 \in Q$ è lo stato in cui si trova inizialmente la macchina.
$q_f \in Q$ è lo stato in cui si trova la macchina quando finisce.
$\delta$ è una funzione con $dom(\delta) = A \times Q$ e $cod(\delta) = A \times Q \times \{L, R\}$ che definisce cosa fa la macchina quando arriva in una cella. $L$ ed $R$ sono, rispettivamente, *left* e *right*, ovvero il movimento della testina che è in 1 dimensione.

Uno stato è definito come $(\alpha, i, q)$ dove
$\alpha : \mathbb{Z} \to A$ è una funzione che definisce il nastro infinito. $\alpha(k)=a \iff k$-esima cella del nastro contiene il simbolo $a$. 
$i \in \mathbb{Z}$ è la posizione della testina sul nastro. Testina posizionata sulla $i$-esima cella di contenuto $\alpha(i)$.
$q \in Q$ è lo stato corrente.

La macchina finisce in uno stato non finale $(\alpha^\prime, i^\prime, q^\prime)$ se:
- $\delta(\alpha(i), q)=(a,q^\prime,x)$. La testina legge il contenuto $\alpha(i)$ della corrente $i$. Lo stato corrente di aggiorna da $q$ a $q^\prime$.
- $\alpha^\prime(i)=a$ e $\alpha^\prime(n)=\alpha(n)$ per $n \neq i$ la testina sovrascrive il valore della cella con $a$
- $i^\prime=i+1$ se $x=R$
  $i^\prime=i-1$ se $x =L$
  si muove a dx o sx a seconda del valore di $x$.


*Esempio*: confrontare due numeri espressi in base 1: si scrive $1$ n-volte in base a che numero voler scrivere. $1=1, 2=11, 3=111,...$
$A = \{b, 1, 0, \$\}$
$b = \text{"blank"}$ significa che è vuoto.
$\$$ usato per separare i numeri.
l'alfabeto è spesso allargato.
...
Lo spazio occupato dall'output è $1$ perché non si calcola lo spazio dell'input e si scrive una sola cella per l'output.

---

## Differenze tra macchine di Turing e $\lambda$ calcolo
A livello di complessità
- nelle macchine di Turing ogni passo (tempo) e cella (spazio) sono costanti $O(1)$: questo perché anche lo stato della macchina è finito. Dunque tempo costante + scrittura/lettura; ogni cella ha un'unità di spazio fissa. Ottimo per studio di complessità.
- nel $\lambda$ calcolo ogni passo implementato *naive* ha un costo di $O(n^2)$ che dipende dalla dimensione dell'espressione che si vuole semplificare. Un'implementazione di questo tipo diviene più complessa con un tempo non ben definito.

- Le m. T. sono imperative ma non nel senso di linguaggio di prog. imperativo.
- Il $\lambda$ calcolo è alla base di ogni linguaggio di prog. funzionale.

- Le m. T. sono non composizionali: bisogna farne una ad-hoc ogni volta.
- Il $\lambda$ calcolo è composizionale: ogni funzione può essere decomposta per risolvere problemi simili. 

- Le m. T. sono a basso livello. Diviene difficile implementare costrutti e meccanismi.
- Il $\lambda$ calcolo ad alto, non ci sono strutture dati vincolanti. Molto facile implementare costrutti e meccanismi di diversi linguaggi di programmazione.

- Testare m. T. è difficile perché non vi è una definizione ricorsiva.
- Il $\lambda$ calcolo ha il concetto di ricorsione.

Un $\lambda$ calcolo è una controparte computazione della logica. La corrispondenza tra $\lambda$ calcolo e logica permette di fare logica $\leftrightarrow$ matematica $\leftrightarrow$ ling. programmazione.


