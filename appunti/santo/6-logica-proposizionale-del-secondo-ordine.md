$$F ::= \cdots | \forall A. F$$
$A$ è una variabile proposizionale, qualcosa che può essere vero/falso.
(nella logica del primo ordine si ha $F ::= \cdots | \forall x.F | P^n(x_1, \dots, x_n)$ dove $x$ è una variabile di termine, elemento del dominio del discorso e $P^n(x_1, \dots, x_n)$ è un predicato, come l'essere pari o dispari).

Un esempio di logica proposizionale del primo ordine è
$$\forall x.x \leq x$$

Un esempio di logica proposizionale del secondo ordine è
$$\forall A,B,C.(A \land B \to C) \to \neg C \to \neg (A \land B)$$
Le regole qui sono

$$\frac{\begin{array}{@{}c@{}}
\Gamma \vdash F[B/A]
\end{array}}
{
  \Gamma \vdash \forall A.F
} \forall_i\hspace{2em}\text{ dove }B\text{ è una variabile fresca non usata in }\Gamma$$
$$\frac{\begin{array}{@{}c@{}}
\Gamma \vdash \forall A.F
\end{array}}
{
  \Gamma \vdash F[G/A]
} \forall_e$$

$$
\cfrac{\begin{array}{@{}c@{}}

\cfrac{\begin{array}{@{}c@{}}

\cfrac{\begin{array}{@{}c@{}}

(\forall A. (A \to B)) \in \forall A. (A \to B)

\end{array}}
{
\forall A. (A \to B) \vdash \forall A.(A \to B)
}

\end{array}}
{
\forall A. (A \to B) \vdash (D \to D) \to B
}\forall_e

\end{array}}
{
\forall A. (A \to B) \vdash \forall C. (C \to C) \to B
}\forall_i
$$

## Polimorfismo uniforme o generico o template
$$T::= \cdots | \forall A.T$$
o anche, come usata in molti linguaggi di programmazione
$$T::= \cdots | \langle A\rangle T$$

$$\frac{\begin{array}{@{}c@{}}
\Gamma \vdash M:T[B/A]
\end{array}}
{
  \Gamma \vdash M:\forall A.T
} \forall_i$$
$$\frac{\begin{array}{@{}c@{}}
\Gamma \vdash M:\forall A.T
\end{array}}
{
  \Gamma \vdash M:T[T^\prime/A]
} \forall_e$$
che poi, quest'ultimo, $M:T[T^\prime /A]$ è $M\langle T^\prime\rangle$

Questo, alla fine di tutto, è il $\forall$ della logica proposizionale del secondo ordine.

## Esistenza
$$F::= \cdots | \exists A.F$$
essendo al secondo ordine, vuol dire che $A$ è una variabile proposizionale.

$$\frac{\begin{array}{@{}c@{}}
\Gamma \vdash F[G/A]
\end{array}}
{
  \Gamma \vdash \exists A.F
} \exists_i$$
$G$ è completamente variabile.

$$\frac{\begin{array}{@{}c@{}}
\Gamma \vdash \exists A.F\hspace{2em}\Gamma,F[B/A] \vdash G
\end{array}}
{
  \Gamma \vdash G
} \exists_e$$
Nel medesimo modo fatto per $\forall$ si prende una variabile nuova di cui non si sa assolutamente nulla (fresca: non usata in $\Gamma$ e $G$).

La medesima cosa ma per Curry-Howard sarà
$$T::= \cdots | \exists A.T$$
$$t ::= \cdots | \text{open }t\text{ as } x \text{ in } t$$

che nei linguaggi di programmazione lo si può trovare come "interfaccia" o "tipo di dato astratto" o "classe" o "mixin" o "modulo" o "trait".

### Tipo di dato astratto
Un tipo di dato astratto è un tipo per il quale non viene data l'implementazione ma solo la sua interfaccia come insieme di segnature di funzioni.
*Esempio di stack di interi con tipo di dato astratto*
```
module stack
	type stack
	fn empty : stack
	fn push : stack × Z -> stack
	fn pop : stack -> 1 + Z × stack
end

---

open stack

(λx.λs.
	push ❬x, s❭
	) 2 empty

---

module instance stack
	type stack = array❬Z❭ × N
	fn empty = ❬[], 0❭
	fn push ❬x, s❭ = ❬s.1[s.2<-x], x.2+1❭
end

---

// Curry-Howard ottenuto
∃ stack.stack × (stack × Z -> stack) × (stack -> 1 + Z × stack)
open M as f in ... f.1 ... f.2.1 ... f.2.2 ...
				  empty     push      pop
```

$$\frac{\begin{array}{@{}c@{}}
\Gamma \vdash M:F[G/A]
\end{array}}
{
  \Gamma \vdash M:\exists A.F
}$$
nei linguaggi di programmazione questo sarebbe la "module instance" sopra.

```
module instance
	type A = F
	M:F[G/A]
end
```

$$\frac{\begin{array}{@{}c@{}}
\Gamma \vdash M:\exists A.F\hspace{2em}\Gamma,f : F[B/A] \vdash N: G
\end{array}}
{
  \Gamma \vdash \text{open }M\text{ as } f \text{ in } N :G
}$$

in questo caso si ha che il primo implementa $M$ senza conoscere $N$, mentre il secondo implementa $N$ senza conoscere $M$. La parte sotto fa il lavoro del linker.

Questo si dimostra con varie regole:

$$\cfrac{\begin{array}{@{}c@{}}

\cfrac{\begin{array}{@{}c@{}}
\Gamma \vdash M:F[T/A]
\end{array}}
{\Gamma \vdash M
}\exists_i
\hspace{2em}\Gamma,f : F[B/A] \vdash N: G
\end{array}}
{
  \Gamma \vdash \text{open }M\text{ as } f \text{ in } N :G
}\exists_e \to \frac{\begin{array}{@{}c@{}}


\end{array}}
{
\Gamma \vdash N[M/f]:G
}$$
da una parte abbiamo implementazione del modulo + linker e dall'altra il codice senza usare moduli.