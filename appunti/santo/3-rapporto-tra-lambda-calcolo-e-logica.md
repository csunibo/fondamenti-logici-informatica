Fissato un linguaggio di programmazione si ha un insieme di tutti i programmi $P$ ($\lambda$ termini). Una **proprietà** è un qualunque sotto insieme di $P$. $x$ ha la proprietà $Q$ sse $x \in Q$. Una proprietà è banale sse $Q = \emptyset \lor Q = P$.
Un esempio è $Q = \{p \in P : p \text{ usa 2 variabili}\}$ nel caso di programma che parla di com'è scritto.
Un esempio è $Q = \{p \in P: p(0)=1 \}$ nel caso di programma che parla di cosa fa sotto forma di funzione. 

Una proprietà $Q$ è **decidibile** sse $\exists p \in P. \forall q \in P. (q \in Q \iff p(q) = \text{true} \land q \notin Q \iff p(q) = \text{false})$.
Ad esempio, con $Q$ è decidibile se $Q= \{p \in P: |p| < 100 \text{ caratteri}\}$

Una proprietà $Q$ è **estensionale** sse $\forall p, q, Q : (\forall i.p(i) = 0 \iff q(i) =0)\implies p \in Q \iff q \in Q$
Dunque non parla dei programmi come sono scritti ma di quello che calcolano. Ad esempio bubble sort e quick sort.
Una proprietà $Q$ è **intenzionale** se non è estensionale.

## Teorema di Rice
$\forall Q.$ se $Q$ non è banale e $Q$ è estensionale, allora $Q$ non è decidibile.
Ad esempio divergere lo è: dato un input il programma non termina. Non è banale perché ci sono programmi che terminano ed è estensionale perché non interessa com'è scritto tale programma.

## Approssimazione
$R$ è un'**approssimazione da dentro** di $Q$ sse $R \subseteq Q$.
$S$ è un'**approssimazione da fuori** di $Q$ sse $Q \subseteq S$.
Supponiamo che $R$ o $S$ siano decidibili e decide da un programma $r$ o $s$.
$\forall p. (r(p) = \text{true} \implies p \in Q) \land (s(p) = \text{false} \implies p \notin Q)$
Nel caso di approssimazione che sta in un'area "grigia": ovvero un po' dentro e un po' fuori, posso allargare l'approssimazione da dentro o stringerla da fuori ma non riuscirò mai una zona di certezza. Un'approssimazione in area grigia non serve a nulla, quindi meglio allargare o stringere.
Dunque dobbiamo cercare sempre approssimazione da dentro oppure da fuori.
Le approssimazioni modulari sono un po' meno precise, ma un buon compromesso.

## Sistema di tipi
È l'implementazione di un programma che decide un'approssimazione da dentro o da fuori in **maniera modulare**.
Molti sistemi di tipo si inseriscono nei linguaggi non tipati, andando a cercare errori gravi nel codice. In C e Java, se si dice che è mal tipato, vuol dire che potrebbe non funzionare a run time.
Il fatto che sia modulare vuol dire che il programma $p$ può essere diviso in $p_1, \dots, p_n$ moduli. Un sistema di tipi è modulare se, per decidere la modalità, analizza un modulo per volta e poi decidere in base a quelli analizzati se è ben tipato o no.
Ad ogni modulo viene associata un'informazione $T_i$ chiamata **tipo**.
$r(p) = \text{true} \iff p \in R \iff \overline{r} (T_1, \dots, T_n)$

Con questo vuol dire che non c'è bisogno di avere a disposizione tutto il codice sorgente. 

Se si ha una situazione gerarchica tale che un modulo è diviso in altri moduli si vede come $p$ è decomposto in $p_1, \dots, p_n$, $p_1$ è decomposto in $p_{11}, \dots, p_{1m}$ e così via. Da questo si potrà trovare, a partire dalle foglie, i vari tipi a salire verso l'alto. Tutto ciò ad arrivare a capire che il tipo $T$ del programma $p$ è ben tipato da dentro o da fuori.

## $\lambda$ calcolo tipato semplice
Per semplice si intende che si sceglie quello più semplice tra quelli possibili. I tipi sono quelli che si associano dal basso verso l'alto (dal $T_{11m}$ al $T$ per dire).

```
T ::= A | T -> T
```

$A, B, \cdots$ sono variabili di tipo. Intuizione bool, int, string, $\cdots$.
$T \to T$ sono tipo delle funzioni con un certo input/output. 

Ad esempio, il tipo $A \to B$ è il tipo delle funzioni che dato un input $A$ restituisco un output di tipo $B$.

"$\to$" è associativo a destra. Dunque $A \to B \to C \equiv A \to (B \to C)$.

Un termine può avere più tipi. $\lambda x.y$ è un esempio del perché abbiamo un linguaggio modulare, visto che il codice $y$ è esterno.

### Contesto
```
Γ ::= | Γ, x : T
```

quindi $\Gamma$ è vuota oppure gli si associa la variabile $x$ al tipo. Non si hanno termini.

Ad esempio `x: A, y: B, z: A -> B`.
Come ipotesi si ha che in $\Gamma$ nessuna variabile è ripetuta.
$(x:T) \in \Gamma$ vuol dire che `Γ = ..., x : T, ...`.

### Judgement di tipaggio $\Gamma \vdash t: T$
È una relazione ternania. Si legge "in $\Gamma, t$ ha tipo $T$", quindi $t$ ha tipo $T$ sotto l'ipotesi $\Gamma$.
Definisco $\Gamma \vdash t: T$ attraverso un sistema di inferenza.

1. Termine
$$
\frac{\begin{array}{@{}c@{}}
(x:T)\in \Gamma
\end{array}}
{
  \Gamma \vdash x:T
}$$
2. Applicazione
$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash M : T_1 \to T_2\hspace{2em}\Gamma \vdash N: T_1
\end{array}}
{
  \Gamma \vdash MN: T_2
}$$
3. Astrazione
$$
\frac{\begin{array}{@{}c@{}}
\Gamma, x: T_1 \vdash M: T_2
\end{array}}
{
  \Gamma \vdash \lambda x.M: T_1 \to T_2
}$$

Ad esempio
$$
\cfrac{\begin{array}{@{}c@{}}
\cfrac{\begin{array}{@{}c@{}}

\cfrac{\begin{array}{@{}c@{}}
(f: A \to A) \to B) \in \Gamma
\end{array}}
{
f: (A\to A)\to B, x: A \to A \vdash f(A \to A)\to B
}
\hspace{2em}
\cfrac{\begin{array}{@{}c@{}}
(x: A \to A) \in \Gamma
\end{array}}
{
f: (A\to A)\to B, x: A \to A \vdash x, A \to B
}
\end{array}}
{
f: (A\to A)\to B, x: A \to A \vdash fx:B
}
\end{array}}
{
f:(A \to A)\to B \vdash \lambda x.fx:(A \to A) \to B
}
$$

Ad esempio, uno non ben tipato
$$\cfrac{\begin{array}{@{}c@{}}
\cfrac{\begin{array}{@{}c@{}}
\cfrac{\begin{array}{@{}c@{}}
(x: T_3?\to T_4?) \in \Gamma
\end{array}}
{
x: T_3? \to T_4? \vdash x: T_3?\to T_4?
}
\hspace{2em}
\dfrac{\begin{array}{@{}c@{}}
(x:T_3?) \in x:T_3?\to T_4?
\end{array}}
{
  x:T_3?\to T_4? \vdash x :T_3?
}
\end{array}}
{
x:T_3?\to T_4? \vdash xx : T_2?
}
\end{array}}
{
\vdash \lambda x.xx
}$$
quello più a destra $(x:T_3?) \in x:T_3?\to T_4?$ sarebbe vero solo se $T_3? = T_3? \to T_4?$ ma sintatticamente non possono esserlo. $\lambda x.xx$ on ha la proprietà "MISTERIOSA".


## Isomorfismo
La corrispondenza che si ha con $\lambda$ calcolo si chiama isomorfismo: si cambia da una form all'altra senza perdere informazioni. Il fatto che abbiamo lo stesso risultato, non vuol dire che le facciano nel medesimo modo.

### Isomorfismo di Curry-Howard-Kolmogorov

| $\lambda$ calcolo         | Logica     |
|--------------|-----------|
| Tipo |        Formula |
| Termini | Prove|
| Costruttore di tipo  |  Connettivo |
| Costruttore di termini | Passi di prova |
| Variabili libere/legate | Ipotesi globali/locali (quelle locali sono denotate anche come "scaricate")|
| Type checking | Proof checking |
| Type inaditation (dato $\Gamma, T$ cerco un $t$ tc. $\Gamma \vdash t:T$) | Ricerca di prove |
| Riduzione | Normalizzazione di prove |

Escludendo i tipi si può cambiare forma.
$$\cfrac{\begin{array}{@{}c@{}}
\cfrac{\begin{array}{@{}c@{}}
\cfrac{\begin{array}{@{}c@{}}

\cfrac{\begin{array}{@{}c@{}}
(A \to A \to B) \in \Gamma
\end{array}}
{
  \Gamma \vdash A \to A \to B
}
\cfrac{\begin{array}{@{}c@{}}
A \in \Gamma
\end{array}}
{
  \Gamma \vdash A
}
\end{array}}
{
  A \to A \to B, A \vdash A \to B
}

\cfrac{\begin{array}{@{}c@{}}
A \in \Gamma
\end{array}}
{
  A \to A \to B, A \vdash A
}
\end{array}}
{
  A \to A \to B, A \vdash B
}
\end{array}}
{
  A \to A \to B \vdash A \to B 
} \implies_i$$
$$\cfrac{\begin{array}{@{}c@{}}
\cfrac{\begin{array}{@{}c@{}}
\cfrac{\begin{array}{@{}c@{}}

\cfrac{\begin{array}{@{}c@{}}
(f:A \to A \to B) \in \Gamma
\end{array}}
{
  \Gamma \vdash f:A \to A \to B
}
\cfrac{\begin{array}{@{}c@{}}
(x:A) \in \Gamma
\end{array}}
{
  \Gamma \vdash x:A
}
\end{array}}
{
  f:A \to A \to B, x:A \vdash fxA \to B
}

\cfrac{\begin{array}{@{}c@{}}
(x:A) \in \Gamma
\end{array}}
{
  f:A \to A \to B, x:A \vdash x:A
}
\end{array}}
{
  f:A \to A \to B, x:A \vdash fxx:B
}
\end{array}}
{
  f:A \to A \to B \vdash \lambda x.fxx A \to B 
} \implies_i$$
il quale diviene, semplicemente
$$\lambda x.fxx$$

($\Pi$ sono alberi di prova)
![[20231012212318.png|500]]

Questo isomorfismo è capace di scalare. Se si prova ad aggiungere altri connettivi, come ad esempio, l'AND.

$$F ::= \dots | F_1 \land F_2$$
$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash F_1 \hspace{2em} \Gamma \vdash F_2
\end{array}}
{
  \Gamma \vdash F_1 \land F_2
}\land_i
$$
$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash F_1 \land F_2
\end{array}}
{
  \Gamma \vdash F_1 
}\land_{e_1}
$$
$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash F_1 \land F_2
\end{array}}
{
  \Gamma \vdash F_2 
}\land_{e_2}
$$
 $$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash F_1 \land F_2\hspace{2em}\Gamma,F_1,F_2 \vdash  F
\end{array}}
{
  \Gamma \vdash F 
}\land_e
$$

ma lato $\lambda$ calcolo si avrà
$$T ::= \dots | T \times T$$
$$t ::= \dots | \langle t, t\rangle | t.1 | t.2| \text{match }t\text{ with }\langle x_1, x_2\rangle \implies t$$
quindi si aggiungono le tuple nel linguaggio di programmazione.
$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash M_1:T_1 \hspace{2em} \Gamma \vdash M_2:T_2
\end{array}}
{
  \Gamma \vdash \langle M_1,M_2\rangle:T_1 \times T_2
}
$$
$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash C:T_1 \times T_2
\end{array}}
{
  \Gamma \vdash C.1:T_1
}
\hspace{2em}
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash C:T_1 \times T_2
\end{array}}
{
  \Gamma \vdash C.2:T_2
}
$$
$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash C: T_1 \times T_2 \hspace{2em} \Gamma, x_1:T_1,x_2:T_2 \vdash M:T
\end{array}}
{
  \Gamma \vdash \text{match }c\text{ with }\langle x_1, x_2\rangle \implies M:T
}
$$
![[20231012213740.png|500]]
![[20231012213814.png|500]]

Estendendo la logica aggiungendo $T$, ovvero il *TOP*, in cui è sempre vero.
$$F ::= \dots | T$$
$$
\frac{\begin{array}{@{}c@{}}
\end{array}}
{
  \Gamma \vdash T
}T_i
$$
$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash T\hspace{2em}\Gamma \vdash F
\end{array}}
{
  \Gamma \vdash F
}T_e
$$
 
e lato $\lambda$ calcolo si avrà
$$T ::= \dots | \mathbb{1}$$
$\mathbb{1}$ è il costrutto *unit* (in Haskell è `()`, in C è `void`, in Python è `None`, ...).
$$t ::= \dots | ()| \text{let }()=t\text{ in }t$$

$$
\frac{\begin{array}{@{}c@{}}
\end{array}}
{
  \Gamma \vdash (): \mathbb{1}
}
$$
$$
\frac{\begin{array}{@{}c@{}}
M \vdash M: \mathbb{1}\hspace{2em}\Gamma \vdash N: T
\end{array}}
{
  \Gamma \vdash \text{match } M \text{ with }()\implies N: T
}
$$

![[20231012215021.png|500]]

Estendendo la logica aggiungendo $B$, ovvero il *BOTTOM*, in cui è sempre falso. Si dimostra con *ex falso sequitur quodlibet* (dal falso sempre qualunque cosa vera).
$$F ::= \dots | \perp$$

$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash \perp
\end{array}}
{
  \Gamma \vdash F
}
$$

e lato $\lambda$ calcolo si avrà
$$T ::= \dots | \mathbb{0}$$
$$t ::= \dots | \text{abort}(t)$$
esempio del codice "mai eseguito". Non si hanno regole di riscrittura perché un programma che esegue un abort, termina lì.

$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash M:\perp
\end{array}}
{
  \Gamma \vdash \text{abort}(M):T
}
$$

Estendendo la logica aggiungendo l'or.
$$F ::= \dots | F_1 \lor F_2$$

$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash F_1
\end{array}}
{
  \Gamma \vdash F_1\lor F_2
}\lor_{i_1}
$$
$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash F_2
\end{array}}
{
  \Gamma \vdash F_1\lor F_2
}\lor_{i_2}
$$
$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash F_1 \lor F_2 \hspace{2em}\Gamma,F_1\vdash F\hspace{2em}\Gamma,F_2\vdash F
\end{array}}
{
  \Gamma \vdash F
}\lor_e
$$

e lato $\lambda$ calcolo si avrà un unione disgiunta, dunque sempre dato algebrico.
$$T ::= \dots | T_1+T_2$$
$$t ::= \dots | \underline{L}(t)|\underline{R}(t)|\text{match }$$
$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash M_1:T_1
\end{array}}
{
  \Gamma \vdash \underline{L}(M_1):T_1+T_2
}
$$
$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash M_2:T_2
\end{array}}
{
  \Gamma \vdash \underline{R}(M_2):T_1+T_2
}
$$
$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash M:T_1+T_2\hspace{2em}\Gamma,x_1:T_1\vdash N_1:T \hspace{2em} \Gamma, x_2: T_2 \vdash N_2:T
\end{array}}
{
  \Gamma \vdash \text{match }M\text{ with }\underline{L}(x_1) \implies N_1 |\underline{R}(x_2)\implies N_2:T
}
$$


## Teorema di consistenza della logica proposizionale
$$\nvdash \perp$$
*Dimostrazione*
Si dimostra per assurdo. Assumo che $\vdash \perp$
Per Curry-Howard $\exists M. \vdash M : \perp$
Sia $M$ t.c. $\vdash M:\perp$
Sia $N$ la forma normale di $M$ che esiste per il teorema di normalizzazione forte, si ha $\vdash N: \perp$
Il $\perp$ non ha regole di introduzione, dunque neanche $N$ non lo è; non è una variabile perché non è un'ipotesi; dunque dovrebbe essere una regola di eliminazione (cioè $N$ è un pattern matching, come $N = \text{match } M \text{ with }\cdots$). 
$M$ non è una variabile per lo stesso motivo di $N$; non è una regola di introduzione perché se lo fosse ci sarebbe una match su una regola di introduzione e dunque sarebbe un redex, ma una forma normale non ha una riduzione ($N$ è normale); dunque è anch'essa una regola di eliminazione, ma così all'infinito dunque, perché sarebbe un match di un'altra regola di eliminazione e così via. ASSURDO perché manca il caso base di fine di questo loop. Quindi $\nvdash \perp$.
