$\lambda x.xx$ è non tipato nel $\lambda$ calcolo tipato semplice.
$\lambda x.xx$ è tipato nel $\lambda$ calcolo con polimorfismo uniforme.

Esempio d'uso con funzione identità:
$$
\cfrac{\begin{array}{@{}c@{}}

\cfrac{\begin{array}{@{}c@{}}

\cfrac{\begin{array}{@{}c@{}}


(x: \forall A.A \to A )\in \Gamma

\end{array}}
{

\cfrac{\begin{array}{@{}c@{}}

x:\forall A.A \to A \vdash x:\forall A. A \to A
\end{array}}
{
x: \forall A.A \to A \vdash x : (\forall A. A \to A) \to (\forall A. A \to A)
}\forall_e

}
\hspace{2em}

\cfrac{\begin{array}{@{}c@{}}

(x:\forall A.A \to A) \in \Gamma
\end{array}}
{
x: \forall A:A \to A \vdash x:\forall A.A\to A
}

\end{array}}
{
x:\forall A.A \to A \vdash xx : \forall A.A\to A
}

\end{array}}
{
\vdash \lambda x.xx:(\forall A.A \to A)\to(\forall A.A\to A)
}
$$

1. Il $\lambda$ calcolo con polimorfismo uniforme è un'approssimazione migliore della proprietà della normalizzazione forte.
2. $\lambda x.xx$ mostra che non è sempre possibile monomorfizzare programmi che usano il polimorfismo $\implies$ abbiamo incrementato la potenza espressiva.
