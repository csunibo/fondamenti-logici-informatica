Nei linguaggi di programmazione vi è costrutto esplicito di ricorsione.

$$f:=T:M\hspace{4em}\text{Presa una funzione dichiarata al top-level avere tipo }T\text{ e corpo }M$$

è zucchero sintattico per il costrutto
$$f:=(\nu f:T.M)\hspace{4em}\text{termine che dichiara una funzione ricorsiva di tipo }T\text{ che, nel corpo, può richiamarsi usando il nome }f$$

- $f$ è il nome top-level.
- $\nu$ è il binder di punto fisso. 
- $(\nu f:T.M)$ è il corpo della funzione.

Questo che segue è usato per tipare funzioni divergenti, non presente nel $\lambda$ calcolo.
$$
\frac{\begin{array}{@{}c@{}}
\Gamma, f:T \vdash M:T
\end{array}}
{
\Gamma \vdash (\nu f:T.M):T
}
$$

Ad esempio:
$$\cfrac{\begin{array}{@{}c@{}}

\cfrac{\begin{array}{@{}c@{}}

\cfrac{\begin{array}{@{}c@{}}
f: \mathbb{N} \to \mathbb{N}, x: \mathbb{N} \vdash f: \mathbb{N} \to N \hspace{2em} f:\mathbb{N} \to \mathbb{N}, x: \mathbb{N} \vdash x:\mathbb{N}
\end{array}}
{
f: \mathbb{N}\to\mathbb{N}, x: \mathbb{N} \vdash fx:\mathbb{N}
}

\end{array}}
{
f: \mathbb{N}\to\mathbb{N} \vdash \lambda x:\mathbb{N}.fx: \mathbb{N}\to\mathbb{N}
}

\end{array}}
{
\vdash (\nu f: \mathbb{N}\to\mathbb{N}.\lambda x:\mathbb{N}.fx):\mathbb{N}\to\mathbb{N}
}$$

tale regola implica la non consistenza del sistema logico.
$$
\frac{\begin{array}{@{}c@{}}
\vdash \nu f:T \to \perp.\lambda x:T.fx:T \to \perp
\end{array}}
{
\vdash (\nu f:T \to \perp.\lambda x:T.fx)I:\perp
}
$$

Un corollario sulle osservazioni precedenti:
$$Y = \lambda f. (\lambda x.f(xx))(\lambda x.f(xx))$$
non è tipabile.