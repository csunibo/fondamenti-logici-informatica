Si ha solo il connettivo di implicazione ($\to$).

```
F ::= A | F -> F
```

dove $A$ è una variabile proposizionale (rappresenta un valore vero o falso).
dove $F \to F$ è un'implicazione materiale "se ... allora ...".

$\to$ è associativo a destra. Ad esempio $A \to B \to A \equiv A \to (B \to A)$.

I contesti di ipotesi
```
Γ ::= | Γ, F
```

in cui si suppone che $F$ valga.

Un judgement di derivazione logica è definita come $\Gamma \vdash F$, ovvero "dall'ipotesi $\Gamma$ riesco a dimostrare $F$". Un modo per definire le regole di derivazione sono chiamate **deduzione naturale**.

$$
\frac{\begin{array}{@{}c@{}}
F \in \Gamma
\end{array}}
{
  \Gamma \vdash F
}
$$
$$
\frac{\begin{array}{@{}c@{}}
\Gamma \vdash F_1 \to F_2\hspace{2em}\Gamma \vdash F_1
\end{array}}
{
  \Gamma \vdash F_2
}
$$
quest'ultimo è chiamato *modus ponens* o $\to_e$
$$
\frac{\begin{array}{@{}c@{}}
\Gamma, F_1 \vdash F_2
\end{array}}
{
  \Gamma \vdash F_1 \to F_2
}
$$
che è anche scritto come $\to_i$

Queste 3 regole sono le medesime usate per definire le regole del $\lambda$ calcolo tipato.

Ad esempio
$$
\cfrac{\begin{array}{@{}c@{}}
\cfrac{\begin{array}{@{}c@{}}
\cfrac{\begin{array}{@{}c@{}}


\cfrac{\begin{array}{@{}c@{}}
\cfrac{\begin{array}{@{}c@{}}
A \to B \to C \in \Gamma
\end{array}}
{
 A \to B \to C,B,A \vdash A \to B \to C 
}
\cfrac{\begin{array}{@{}c@{}}
A \in \Gamma
\end{array}}
{
A \to B \to C,B,A \vdash A
}


\end{array}}
{
 A \to B \to C, B, A \vdash  B \to C
}

\cfrac{\begin{array}{@{}c@{}}
B \in \Gamma
\end{array}}
{
A \to B \to C,B,A \to B
}


\end{array}}
{
 A \to B \to C, B, A \vdash  C
}
\end{array}}
{
  A \to B \to C, B \vdash A \to C
}
\end{array}}
{
  A \to B \to C \vdash B \to A \to C
}
$$
