Definito da Church, calcolare significa semplificare delle espressioni. Una forma primitiva di calcolo è l'esecuzione di espressioni di somma. Il risultato del calcolo è la massima semplificazione dell'espressione. Si parte dall'idea che tutte le espressioni sono funzioni unarie anonime (1 input e 1 output). Ad ogni modo è un linguaggio Turing completo, dunque permette di definire funzione $n$-arie anche senza usare le chiamate di funzioni (perché sono anonime!), cicli, condizioni o qualsiasi altro costrutto presente in un qualsiasi linguaggio di programmazione.
$$t ::= x\ |\ tt\ |\ \lambda x.t$$
- $t$ è il termine. Spesso si usano $t, s, u, v, M, N, \dots$.
- $x$ è l'occorrenza di una variabile. Spesso si usano $x, y, z, w, \dots$. Dunque si restituisce un valore.
- $t_1t_2$ è la chiamata di funzione, chiamata applicazione. $t_1$ è una funzione unaria con parametro $t_2$. La notazione matematica potrebbe essere $t_1(t_2) \equiv t(t) \equiv tt$.
- $\lambda x.t$ è una funzione anonima, chiamata astrazione. Il parametro formale è $x$ e il corpo è $t$. La notazione matematica è $x \mapsto t \equiv \lambda x.t$ Se usassi un nome di funzione avrei qualcosa come $f(x)=t$ (il nome della funzione in questo caso è $f$). Si usano le parentesi per disambiguare. 

La riscrittura di un termine viene detta **riduzione** e non *semplificazione* perché non defluisce la complessità ad ogni passaggio. La riduzione potrebbe anche complicare l'espressione.

Le variabili sono termini. I termini non sono tutti variabili.

*Esempi*
- $\lambda x.x$ è identità
- $(\lambda x.x)(\lambda y.y)$ risulta $\lambda y.y$
- $\lambda x.y$ risulta sempre $y$

A livello di sintassi si ha la precedenza sull'astrazione
- $\lambda x.xx$ si legge come $\lambda x.(xx)$
e non si ha l'associatività sennò che a sinistra
- $xyz$ si legge $(xy)z$

Una funzione binaria del tipo $f(x,y)=g(x,y)$ può essere vista come una funziona unaria che ritorna una funzione unaria:
$$\lambda x.\lambda y. gxy$$
Questo perché usa l'associatività a sx $(gx)y$.

In questo modo si può passare un solo input, come ad esempio
$$(\lambda x.\lambda y.x+y)2$$
si riduce a
$$\lambda y.2+y$$

e dunque è come avere una nuova funzione che incrementa di $2$ il valore dell'input. Ad esempio con parametro $y=3$ si riduce come:
$$(\lambda y.2+y)3 \hspace{2em}\to\hspace{2em}2+3$$

## Riduzione
Un $\lambda$ termine $t$ si può ridurre ad un altro $t^\prime$ rimpiazzando una chiamata di funzione $(\lambda x.M)N$ con il corpo $M$ dove sostituisco $x$ con $N$.

Ad esempio $(\lambda x.yx)(zz)$ si riduce a $yzz$, o meglio,  $y(zz)$.
### Sostituzione
I nomi dei parametri non sono importanti ma invece quelli delle variabili globali sì. $\lambda x.y$ e $\lambda x.z$ sono programmi diversi.

Il $\lambda x.t$ si ha che $\lambda$ è chiamato **binder**: lega la variabile $x$ al corpo $t$.
Una variabile non legata è **libera**.

$FV(t)$ è l'insieme delle variabili libere di $t$.
- $FV(x) = \{x\}$
- $FV(MN)=FV(M) \cup FV(N)$
- $FV(\lambda x.M)=FV(M) - \{x\}$

Una funzione ricorsiva strutturale esegue ricorsione solo su parti più piccole dell'input, attuabile solo quando si hanno forme finite possibili, come nel lambda calcolo.

*Esempio*
$$FV(\lambda x.xy(\lambda y.yz))$$
si vede come il termine più interno $\lambda y.yz$ ha una $y$ legata. In quello più esterno si ha $\lambda x.xy(..)$ con $x$ legata.
Più in dettaglio si avrà
$$\begin{equation}\begin{split}FV(\lambda x.xy(\lambda y.yz))&=FV(xy(\lambda y.yz))-\{x\}\\
&=\Big(FV(xy) \cup FV(\lambda y.yz)\Big)-\{x\}\\
&=\Bigg(FV(xy) \cup \Big(FV(yz)-\{y\}\Big)\Bigg)-\{x\}\\
&=\Bigg(FV(x)\cup FV(y) \cup \Big(FV(y) \cup FV(z) - \{y\}\Big)\Bigg)-\{x\}\\
&=\Bigg(\{x\}\cup\{y\}\cup \Big(\{y\}\cup\{z\}-\{y\}\Big)\Bigg)-\{x\}\\
&=\{y\}\cup\{z\}\\
&=\{y,z\}
\end{split}\end{equation}$$

Si può, più semplicemente, guardare il legame nel $\lambda$ ed evitare tutta l'espressione sopra. Essere legato fa riferimento all'occorrenza, non al nome della variabile in sé.
#### $\alpha$ conversione
Due $\lambda$ termini $t_1$ e $t_2$ sono $\alpha$ convertibili se si può ottenere l'uno dall'altro ridenominando le sole variabili legate in modo che le occorrenze legate di una variabile in una corrispondenza lo sia anche nell'altra. Idem per le variabili libere.
$\alpha$ equivalenza è una relazione simmetrica, riflessiva e transitiva.

Ad esempio $\lambda x.\lambda y.xyz \equiv_\alpha \lambda x.\lambda w.xwz$
perché i legami $\lambda y$ e $y$ sono nella medesima posizione di $\lambda w$ e $w$.
Invece $\not \equiv_\alpha \lambda x.\lambda z.xzz$
Oppure $\not \equiv_\alpha \lambda x.\lambda y.yxz$
Oppure $\not \equiv_\alpha \lambda x.\lambda y.xyw$


La sostituzione "classica" avviene sostituendo in $M$ un termine $N$ al posto della variabile $x$, scritto come $M\{N/x\}$.

Ad esempio
$(\lambda x.xy)\{zz/y\}=\lambda x.x(zz)$
però in
$(\lambda x.xy)\{xx/y\}\neq\lambda x.x(xx)$
ci sono alterazioni nel senso di valori legati. Però con $\alpha$-conversione si potrebbe avere:
$(\lambda x.xy)\{z/x\}\equiv_\alpha (\lambda z.zy)\{xx/y\}=\lambda z.z(xx)$

Vi sono diversi casi, formalmente:
- $x\{N/x\}=N$
- $y\{N/x\}=y$
- $(t_1t_2)\{N/x\}=t_1\{N/x\}t_2\{N/x\}$
- $(\lambda x.M)\{N/x\}=\lambda x.M$
- $(\lambda y.M)\{N/x\}=\lambda z.M\{z/y\}\{N/x\}$ per $z \not \in FV(M) \cup FV(N)$

$z$ è **fresca** se non è mai stata utilizzata. È detta **sufficientemente fresca** se $\not \in FV(M) \cup FV(N)$. Se è fresca, lo è anche sufficientemente.
Chiaramente è più semplice prenderne una fresca.
### $\beta$ riduzione
$t_1 \to_\beta t_2 \iff$ ottengo $t_2$ da $t_1$ rimpiazzando da qualche parte in $t_1$ il **redex** $(\lambda x.M)N$ col ridotto $M\{N/x\}$.

Ad esempio $\lambda x.(\lambda y.yx)x \to_\beta \lambda x.xx$ dove è stato ridotto il redex $(\lambda y.xy)x$

La relazione binaria $\to_\beta$ è definita mediante un **sistema di inferenza**, un sistema stile deduzione naturale che si possono comporre fra di loro.

![[20230921195155.png|200]]

Il primo è un assioma: non ha ipotesi (premesse) ma solo la conclusione.

Ad esempio si ha
![[20230921195527.png|200]]
col primo assioma e le altre varie successioni.

$t_1 \to_\beta^n t_{n+1}$ ($t_1$ si riduce in $n$ passi a $t_{n+1}$) $\iff t_1\to_\beta t_2 \to_\beta \dots \to_\beta t_{n+1}$
Formalmente:
- $t\to_\beta^0 t$
- $t\to_\beta^{n+1}t^{\prime\prime} \iff t \to_\beta t^\prime$ e $t^\prime \to_\beta^n t^{\prime\prime}$

$t\to_\beta^\star t^\prime$ ($t$ riduce in $0+$ passi a $t^\prime$) $\iff \exists n : t \to_\beta^nt^\prime$

Ad esempio.
$$(\lambda x.\lambda y.xy)(\lambda z.z)(\lambda z.z) \to_\beta^3 \lambda z.z$$
$(\lambda x.\lambda y.xy)(\lambda z.z)(\lambda z.z) \to_\beta (\lambda y.(\lambda z.z)y)(\lambda z.z)$ e qui si vede come ci siano due altri redex da fare. In totale $3$.
$\to_\beta (\lambda y.y)(\lambda z.z) \to_\beta \lambda z.z$

#### Forme normali
Una forma canonica è quando si ha un insieme di rappresentazioni e se ne battezza una come rappresentazione di riferimento. Quella normale è quando, dato un insieme di rappresentazioni, si prende una canonica riscritta all'ennesimo.

$t$ è una forma normale $t \not \to_\beta \iff \not \exists t^\prime : t \to_\beta t^\prime$
$t$ ha forma normale $t^\prime \iff t \to _\beta^\star t^\prime \land t^\prime \not \to_\beta$
$t$ ha una forma normale (o può convergere) $\iff \exists t^\prime : t$ ha forma normale $t^\prime$

#### Non determinismo
La relazione $\to_\beta$ è non deterministica quando da uno stato si può transitare in un altro differente. Dunque $t : \exists t_1, t_2, t_1 \neq t_2$ con $t \to_\beta t_1$ e $t \to_\beta t_2$

Ad esempio
$(\lambda x.y)((\lambda z.z)w) \to_\beta (\lambda x.y)w$
$(\lambda x.y)((\lambda z.z)w) \to_\beta y$

Non si specifica l'ordine in cui applicare i redex. Potenzialmente, nei sistemi non deterministici, si potrebbero avere risultati diversi in base alla strada presa. In questo caso in ambedue casi si arriva al redex
$(\lambda x.y)w \to_\beta y$

Strade diverse non portano, per forza, a forme normali. Ma se arrivo ad una forma normale, sono sicuro che sarà uguale a quella in cui arriveranno tutti.

---

Un linguaggio Turing completo dovrebbe avere tipi di dato, scelta (if-else) e ripetizione (while, ricorsione). Il linguaggio SQL non è Turing-completo, ma poi gli altri più famosi sì. Basta avere i numeri naturali e codificare tutto il resto con essi.

I linguaggi funzionali non modificano memoria e usano funzioni ricorsive.

Il $\lambda$ calcolo non ha dati, scelta (if-else) e non può avere ricorsione dato che le funzioni sono anonime.

---

## Paradosso di Russell
L'assioma di comprensione (inconsistente) definisce, data una proprietà $P$, l'esistenza $\{X : P(X)\}$ e si ha $\forall y : y \in \{X | P(X)\} \iff P(Y)$.

Russell però definisce
$$X \mathop =^\text{def} \{Y | Y \not \in Y\}$$
quindi l'insieme non appartiene a se stesso. Ma $X \in X?$ Sì, ma $\iff X \not \in X$.

O meglio,
$$X \in X \iff \neg (X\in X) \iff \neg (\neg(X \in X)) \iff \dots \iff \neg(\dots\neg(X \in X))) \iff \dots$$

Questo lo si ottiene senza ricorsione.

In $\lambda$ calcolo è tutta una funzione, dunque si può passare una funzione a se stessa (nel medesimo modo in cui nella teoria degli insiemi lo si fa con gli insiemi).

| Tutto è un insieme  | Tutto è una funzione  |
|---|---|
|$X \in X$   | $xx$  |
| $\neg$  | $f$  |
| $X \not \in X$  |  $f(xx)$ |

Nel primo caso:
- Assioma di comprensione: da $P(Y)$ ricava $\{Y:P(Y)\}$
- $\{Y : Y \not \in Y \} \in \{Y : Y \not \in Y\} \iff \neg (\{Y : Y \not \in Y \} \in \{Y : Y \not \in Y\})$

Nel secondo caso:
- $\lambda$ astrazione: da $M$ ricava $\lambda y.M$
- $(\lambda y.f(yy))(\lambda y.f(yy)) \to_\beta f((\lambda y.f(yy))(\lambda y.f(yy)))$

Per essere Turing-completi si deve ripetere lo stesso codice più volte. Ma secondo quanto sopra, non si ripete il codice bensì lo copia e lo esegue. Dunque, per essere Turing-completi, è *sufficiente* che lo ripeta più volte. Mi basta eseguire una nuova copia del codice $\lambda x.f(xx)$

---

Sia l'insieme $A$ e una funzione $f$ con $\text{dom} f = A$ e $\text{cod}f = A$. $x$ è un **punto fisso** di $f \iff x = f(x)$.
Un esempio il valore assoluto $| . |$ ha infiniti punti fissi su $\mathbb{Z}$.
$x \mapsto x+1$ non ha punti fissi.
In $\lambda$ calcolo $t$ è punto fisso di $f \iff f(t) =_\beta t$, dove l'uguaglianza è una chiusura riflessiva, simmetrica e transitiva di $\to_\beta$.

**Teorema**
In $\lambda$ calcolo ogni termine $M$ ha almeno un punto fisso.

**Dimostrazione**
$(\lambda x.M(xx))(\lambda x.M(xx))$ è un punto fisso di $M$. Infatti
$$(\lambda x.M(xx))(\lambda x.M(xx)) \to M((\lambda x.M(xx))(\lambda x.M(xx)))$$

A differenza delle funzioni matematiche, potrebbe non terminare.


**Definizione**
$Y$ è un operatore di punto di fisso $\iff \forall M: YM$ è un punto fisso di $M$.


**Teorema**
$$Y \mathop =^\text{def} \lambda f.(\lambda x.f(xx))(\lambda x.f(xx))$$
è un operatore di punto fisso. In matematica il calcolo è più complesso e diverso per ogni funzione; qui basta mettere in prefisso $\lambda f.$

**Dimostrazione**
$$YM= (\lambda f.(\lambda x.f(xx))(\lambda x.f(xx)))M \to_\beta (\lambda x.M(xx))(\lambda x.M(xx))$$
che è un punto fisso di $M$.

Si può ottenere il più piccolo programma divergente, che non finisce mai di ridurre:
$$(\lambda x.xx)(\lambda x.xx) \to_\beta (\lambda x.xx)(\lambda x.xx) \to_\beta \dots$$

---

Preso un codice in OCaml come
```ocaml
let rec f n =
	match n with
		| 0 => 0
		| S m => if even(S m) then S m + f m else f m
```

che ha pattern-matching, ricorsione e condizione if-else. Come si scrive in $\lambda$ calcolo?

Scrivendo la funzione con un funtore non ricorsivo. Nel $\lambda$ calcolo un funtore è una funzione che restituisce una funzione: tecnicamente lo è tutto. 
```ocaml
let F : (nat -> nat) -> (nat -> nat) =
  λf.
	λn.
	  match n with
		| 0 => 0
		| S m => if even(S m) then S m + f m else f m
```

in $\lambda$ calcolo sarebbe
$f = YF$ dove $Y$ è un punto fisso.
$$f = YF \to_\beta F(YF)=Ff$$

Quindi una qualsiasi funzione si trasforma mettendo tutto il corpo dopo $Y$.

```ocaml
Y
  (λf.
	λn.
	  match n with
		| 0 => 0
		| S m => if even(S m) then S m + f m else f m)
```

le modifiche sono locali: stesso codice di prima ma con piccole cose. Dunque il $\lambda$ calcolo permette di codificare le cose con poche modifiche. Con le macchine di Turing ci sarebbe un bel po' di lavoro dietro per farne il porting.

*Esempio*
```ocaml
let rec fact =
  λn.
  match n with
    | 0 => 1
    | S m => m * fact n
```

che in $\lambda$ calcolo diverrebbe
```
Y
  (λfact.
    λn.
    match n with
	  | 0 => 1
	  | S m => m * fact m)
```

si ha che
$$\text{fact}(\text{S } 0) = Y(\lambda \text{fact}.\lambda n. \cdots)(\text{S } 0)\to_\beta$$
$$(\lambda \text{fact}.\cdots)(Y(\lambda \text{fact}.\cdots))(\text{S }0)\to_\beta$$
$$\star^1\hspace{2em}(\lambda n.\text{match n with 0 => 1 | S n => S n * }Y(\lambda \text{fact}.\cdots)n)(\text{S }0) \to_\beta^\star$$
$$\text{S }0\text{ * }Y(\lambda \text{fact}.\cdots)0\to_\beta^\star$$
$$\text{S }0 \text{ * }1 \to_\beta^\star$$
$$\text{S }0$$

In $\star^1$ si potrebbe però espandere la parte $Y(\lambda \text{fact}.\cdots)$ e dunque avere
$$(\lambda n.\text{match n with 0 => 1 | S n => S n * }(\lambda n.\text{match n with 0 => 1 | S n => S n * }Y(\lambda \text{fact}.\cdots)n)(\text{S }0) \to_\beta^\star$$
però così si avrebbe un'espansione all'infinito.

---

Un tipo di dato algebrico, sempre in OCaml, può essere definito come:

```ocaml
type B = true : B | false : B
(** Es: true: B *)
```
Il valore booleano, definito come B, può avere solo due valori ed essi sono del tipo booleano.
B è il nome del tipo, true/false sono costruttori.

Un esempio, simile alle `enum` in Rust, i semi delle carte:
```ocaml
type Seme = Cuori : Seme | Quadri : Seme | Picche : Seme | Fiori: Seme
```

Oppure i numeri naturali, in cui `0` è una possibile forma dei numeri naturali. Il simbolo `S` non è un successore diretto, bensì è come se fosse una funzione che prende un numero naturale e ritorna un altro numero naturale. Dunque è ricorsivo in questo caso.
```ocaml
type N = 0 : N | S : N -> N
(** Es: S (S 0) : N *)
```

O una coppia di numeri naturali
```ocaml
type N2 = Pair : N -> N -> N2
(** Es: Pair 3 5 : N2 *)
```

Oppure un parametrico, come una Lista di valori `T`.  `List` è il tipo parametrico e `T` il parametro del tipo. La lista è vuota `[]` o una "cons" `(::)` in cui ha una testa e una coda. Una lista ha due elementi dunque: testa `T` e coda `List T`.

```ocaml
type List T = [] : List T | (::) : T -> List T -> List T
(** Es: (S 0) :: 0 :: [] : List N 
La testa è  (S 0)
La coda è   0 :: []
la coda a sua volta ha
testa       0
coda        []
---
Altro esempio è
1 :: (2 :: (3 :: []))
*)
```

Si possono fare delle ottimizzazione in memoria: i valori piccoli numerici possono andare scritti come bit dato che occupano poco spazio; la lista vuota può semplicemente puntare a `nil`.

Un esempio di struttura albero può essere definito come
```ocaml
type Tree1 T = X : Tree1 T | O : Tree1 T -> T -> Tree1 T -> Tree1 T
```

![[20230928215254.png|150]]

```ocaml
type Tree2 T = □ : T -> Tree2 T | O : Tree2 T -> Tree2 T -> Tree2 T
```

![[20230928215745.png|150]]

un tipo di dato algebrico può essere confrontato col pattern-matching.
```ocaml
match x with
  | ki xi ... xn => Mi
```
dove $k_i$ è il nome dell'$i$-esimo costruttore; $x_i \dots x_n$ sono variabili, una per ogni argomento del costruttore, che funzionano come variabili legate; $M_i$ è il codice da eseguire che può usare le variabili $x_i \dots x_n$.
Esempi dai tipi definiti prima:

- `b: B`
```ocaml
match b with
  | true => M1
  | false => M2
```

- calcolare la somma di una lista di `N`
```ocaml
let rec sum l =
  match l with
    | [] => 0
    | x :: l => x + sum l
```

qui `x` è `N`, mentre `l` è `List N`. Con un esempio di `l` con valore iniziale `5 :: (2 :: [])` si avrà un heap tipo:

![[20230928223745.png|300]]

si hanno un numero finito di `if-and-else` e di defer. 

Il $\lambda$ calcolo è un linguaggio di programmazione unitipato. Non ha tipi, però si può fare un ponte con la logica.

Presa come esempio un tipo lista
```ocaml
type List T = [] : List T | (::) T -> List T -> List T
```

```ocaml
let rec sum =
  λl.
    match l with
      | [] => 0
      | x::l => x + sum l
```

Introduciamo il concetto di **interfaccia**: un insieme di funzioni. Una nuova lista significa definire tre nuovi costrutti: empty, cons e pattern-matching.

$$\text{empty : List T}$$
$$\text{cons : T} \to \text{List T} \to \text {List T}$$
$$\text{match}_\text{List} : \forall \text{X.List T} \to \text{X} \to \text{(T} \to \text{List T} \to \text{X) }\to \text{X}$$


si def. come $\text{match}_\text{List}$ perché ogni match è diverso per ogni tipo.

```ocaml
let rec sum =
  λl.
    match_List
    l
    0
    (λx.λl. x + sum l)
```

Il costrutto di pattern-matching è considerato una volta sola. I linguaggi che hanno la possibilità di essere più flessibili nel fare questo matching lo fanno in fase di compilazione.
$$\text{match}_\text{List} \text{ empty M}_\text{e} \text{ M}_\text{c} \to_\beta^\star \text{M}_\text{e}$$
$\text{M}_\text{e}$ corrisponde a cosa restituire quando la lista è vuota ($0$ in questo caso)
$\text{M}_\text{c}$ corrisponde a $\text{(T}\to \text{List T}\to \text{X)}$

Passo una funzione che in un qualche modo codifica il dato e un’altra funzione estrae questo dato codificato.
$$\text{match}_\text{List} \text{(cons M}_\text{x} \text{ M}_\text{l}\text{) M}_\text{e}\text{ M}_\text{c} \to_\beta^\star \text{M}_\text{c}\text{ M}_\text{x} \text{ M}_\text{l}$$
Con $\to_\beta^\star$ diciamo che c’è una strada che porta a quel risultato in maniera deterministica, ma non c’è solo quella.

Prendere un termine astratto su delle variabili e poi metterlo in altre funzioni sono teoremi di riduzione semantica: "dato un insieme di ipotesi $\Gamma$ e un’ipotesi $X$ e da lì si dimostra una formula $f$" è equivalente a dire che "a partire da $\Gamma$ si dimostra $X \to f$".

Da un punto di vista logico si può riscrivere

$$\text{match}_\text{List}\text{ : List T} \to \forall \text{X. X} \to \text{(T }\to \text{List T}\to\text{ X)}\to \text{X}$$

$$\text{List T} \mathop=^\text{def} \forall \text{X.X(T }\to\text{ List T}\to \text{X)}\to \text{X}$$

Il tipo è ricorsivo.

Il pattern-matching in questo caso (in realtà sempre) è la funzione identità.
$$\text{match}_\text{List} \mathop=^\text{def} \lambda x.x$$

$$\text{empty: List T} = \forall \text{X: X}\to\text{(T}\to \text{List T} \to\text{ X)} \to \text{X} \mathop=^\text{def} \lambda e. \lambda c. e$$
$e$ è $X$, $c$ è $\text{T} \to \text{List T} \to \text{X}$

È una funzione con due input, quindi sono due $\lambda$. 

Poi si ha
$$\text{empty} \mathop=^\text{def} \lambda e.\lambda c.e$$

$$\text{match}_\text{List}\text{ empty M}_\text{e}\text{ M}_\text{c}$$
$$= (\lambda x.x)(\lambda e. \lambda c.e) \text{M}_\text{e}\text{ M}_\text{c}$$
$$\to_\beta (\lambda e. \lambda c.e)\text{M}_\text{e}\text{ M}_\text{c}$$
$$\to_\beta \text{M}_\text{e}$$

Invece si ha che

$$\text{cons: T} \to \text{List T} \to \text{List T = T}\to\text{ List T} \to \forall X.X \to\text{ (T} \to \text{List T} \to\text{ X)}\to \text{X} \mathop=^\text{def} \lambda x. \lambda l. \lambda e. \lambda c. cxl$$

$x$ è $\text{T}$, $l$ è $\text{List T}$, $e$ è $\text{X}$, $c$ è $\text{T} \to \text{List T} \to \text{X}$

$$\text{match}_\text{List}\text{ (cons M}_\text{x}\text{ M}_\text{c}\text{)M}_\text{e}\text{ M}_\text{c}$$
$$=(\lambda x.x)\text{(cons M}_\text{x}\text{ M}_\text{l}\text{)M}_\text{e}\text{ M}_\text{c}$$
$$\to_\beta\text{cons M}_\text{x}\text{ M}_\text{l}\text{ M}_\text{e}\text{ M}_\text{c}$$
$$=(\lambda x.\lambda l.\lambda e.\lambda c.cxl)\text{M}_\text{x}\text{ M}_\text{l}\text{ M}_\text{e}\text{ M}_\text{c}$$
$$\to_\beta^4\text{M}_\text{c}\text{ M}_\text{x}\text{ M}_\text{l}$$

Dunque, ignorando i tipi che tanto non servono ad eccezione di guida per la dimostrazione, si hanno che i 3 sono formati come segue
![[20231002204734.png]]

Riprendendo l'esempio del tipo booleano
```ocaml
type B = true : B | false : B
```

$$\text{match}_\text{B} \mathop = ^\text{def} \lambda x.x$$
$$\text{true} \mathop = ^\text{def} \lambda t.\lambda e. t$$
$$\text{false} \mathop = ^\text{def} \lambda t.\lambda e. e$$

$$\text{match}_\text{B}\text{ true M}_\text{t}\text{ M}_\text{e}=(\lambda x.x)\text{ true M}_\text{t}\text{ M}_\text{e}$$
$$\to_\beta\text{ true M}_\text{t}\text{ M}_\text{e}=(\lambda t.\lambda e.t)\text{M}_\text{t}\text{ M}_\text{e}$$
$$\to_\beta \text{M}_\text{t}$$

Riprendendo l'esempio dei numeri naturali
```ocaml
type N = 0 : N | S : N -> N
```

$$\text{match}_\text{N} \mathop = ^\text{def} \lambda x.x$$
$$\text{0} =\lambda z.\lambda s.z$$
$$\text{S} =\lambda n.\lambda z.\lambda s.sn$$
$$\text{match}_\text{N}\text{ (S N) M}_\text{z}\text{ M}_\text{s}$$
$$\to_\beta\text{S N M}_\text{z}\text{ M}_\text{s}$$
$$=(\lambda n.\lambda z.\lambda s.sn)\text{ N M}_\text{z}\text{ M}_\text{s}$$
$$\to_\beta^3\text{M}_\text{s}\text{ N}$$

Riprendendo l'esempio dei semi
```ocaml
type Seme = Cuori : Seme | Quadri : Seme | Picche : Seme | Fiori : Seme
```
Tutti hanno $0$ argomenti ma ci sono $4$ costruttori.

$$\text{match}_\text{Seme} \mathop = ^\text{def} \lambda x.x$$
$$\text{cuori}\mathop = ^\text{def} \lambda c. \lambda q.\lambda p.\lambda f.c$$
$$\text{quadri}\mathop = ^\text{def} \lambda c. \lambda q.\lambda p.\lambda f.q$$
$$\text{picche}\mathop = ^\text{def} \lambda c. \lambda q.\lambda p.\lambda f.p$$
$$\text{fiori}\mathop = ^\text{def} \lambda c. \lambda q.\lambda p.\lambda f.f$$

### Assegnazione
```
var a = 2;
f(x, y) {
	var z = 2;
	x = z * y;
	z = y + a;
	a = 3;
	x = x + g();
	return x + z;

	g() {
		z = z+1;
		return 3;
	}
}
```

nel $\lambda$ calcolo e programmazione funzionale non si può mutare nulla: se voglio comunicare un cambiamento, si da un output in più. 
Trasformare in un linguaggio funzionale uno snippet scritto in linguaggio imperativo vuol dire esplicitare tutto ciò che dipende e cambia una funzione.

```
f(x, y, a) {
	// x viene usata, dunque deve restituire il valore nuovo
	// y no, quindi non ritorna
	// a sì, quindi ritorna
	// z sì ma è locale, quindi non ritorna

	var z = 2;
	var x' = z * y;
	var z' = y + a;
	var a' = 3;
	var (z'', res) = g(z');
	var x'' = x' + res;
	return (a', x'', x''+z'')

	g(z) {
		var z' = z + 1;
		return (z', 3);
	}
}
```

visto che non esiste l'assegnazione di variabile, una soluzione è quella di espandere le variabili.

```
var x = 4;
...
g(x);
```

diviene
```
// g(x){4/x}
...
g(4);
```

oppure si può scrivere un redex
$(\lambda x.\cdots.g(x))4$

in programmazione funzionale può esser fatto come
```ocaml
let x = 4 in g(x)
```

### Cicli
```
var x = 10;
var res = 0;
while_ x > 0 do
	res = res + x;
	x = x - 1;
done
```

diviene
```ocaml
let rec while_ x res =
	if x > 0 then
		while_ (x-1) (res+x)
	else
		(x, res)
```

### Record
```c
struct person {
	name = "Claudio"
	age = 47
	city = "Bologna"
}

if person.age > 20 then
	return person.name
```

come sono messi in memoria potrebbe essere semplice zucchero sintattico per $n$-uple, in questo caso triple perché ha 3 campi.

```ocaml
type Person = mk : string -> int -> string -> Person
```

la funzione `age` diviene solo un modo per estrarre valore. Idem per le altre due.

```ocaml
age p =
	match p with
		mk name age city => age

if age person > 20 then
	person.name
```

### OOP
```
object person {
	age = 41
	name = "Claudio"
	grow(n) {
		if self.age + n > 100 then
			self.die()
		else
			self.age = self.age + n
	}
	die() {
		self.name = "RIP"
	}
}
```

In questo linguaggio si hanno gli oggetti, non classi.

```
struct person {
	age = 41
	name = "Claudio"
	grow =
		λself.λn
			if self.age + n > 100 then
				// passa come parametro l'oggetto stesso
				self.die self
			else
				person { 
					age = self.age + n
					name = self.name
					grow = self.grow
					die = self.die
				}
}
```

### Eccezioni
Il controllo del programma può passare all'istruzione successiva o può saltare ad una computazione completamente diversa.

```ocaml
type e = E1 : N -> e | E2 : string -> e | E3 : N -> N -> e
```

si definiscono due costrutti nuovi

`throw e`

```
try M with
	| E1 x => M1n
	| E2 x => M2
	| E3 x y => M3
```

```
try
	( if even(4) then
		true
	else
		throw (E1 7) ) or false
with
	| E1 x => even(x)
	| E2 x => false
	| E3 x y => true
```

prima viene eseguito il codice dentro `if`.
$\to^\star$
```
try true
	with .. | .. | ..
		=> true
```

ma invece con
```
try
	( if even(5) then
		true
	else
		throw (E1 7) ) or false
with
	| E1 x => even(x)
	| E2 x => false
	| E3 x y => true
```
$\to$
```
try throw(E1 7) or false
with
	| E1 x => even(x)
	| E2 x => false
	| E3 x y => true
```

non si passa il controllo al `false` bensì ad un eccezione che poi viene gestita grazie al `try ... with` e dunque viene eseguito `E1 x => even(x)`.

$$M : B \lor N \lor \text{string} \lor (N \times N)$$
$$M : I \to O_1 \lor O_2 \lor \cdots \lor O_n$$
ma in $\lambda$ calcolo si ha solo
$$M: I_1 \to I_2 \to \cdots \to I_k \to O \cong I_1 \land I_2 \land \cdots \land I_k \to O$$

Ricordando che
$\neg F \equiv F \to \perp$
$\neg (P \lor Q) \equiv \neg P \land \neg Q$
$\neg (P \land Q) \equiv \neg P \lor \neg Q$
$\neg \neg F \equiv F$
$F_1 \land F_2 \to G \equiv F_1 \to F_2 \to G$

si ha che
$$\begin{equation}\begin{split}
I \to O_1 \lor O_2 &\equiv I \to \neg \neg (O_1 \lor O_2)\\
&\equiv I \to \neg (\neg O_1 \land \neg O_2)\\
&\equiv I \to ((O_1 \to \perp) \land (O_2 \to \perp)\to\perp)\\
&\equiv I \to (O_1 \to \perp) \to (O_2 \to \perp) \to \perp 
\end{split}\end{equation}$$
si deve invocare una delle due funzioni $(O_1 \to \perp)$ oppure $(O_2 \to \perp)$ dove, nel codice, ci sono eccezioni.

```
f : Z -> Z                 or Z             or string
      'term. con succ.'   'throw neg.'     'throw toobig'
=
	λx.
		( if x < 0 then
			throw (negative x)
		else if x > 10 then
			throw (toobig "reduce")
		else
			x ) * 10
```

```
f : Z -> (Z -> ⟂) -> (Z -> ⟂) -> (string -> ⟂) -> ⟂
=
	λn.λk_{return}.λk_{e1}.λk_{e2}.
		if x < 0 then
			k_{e1} x
		else if x > 10 then
			k_{e2} "reduce"
		else
			k_{return} x * 10
```

è una trasformazione globale del codice. Dunque
```
try
	f 2
with
	| E1 x => x+2
	| E2 x => 0
```

che è
$f(\lambda x.x)(\lambda x.x+2)(\lambda x.0)$

