Facendo prima delle definizioni:
- Un termine $t$ si dice **debolmente normalizzato** quando ha una forma normale. 
- Un termine $t$ si dice **fortemente normalizzante** se $\nexists (t_i)_{i \in \mathbb{N}}: t = t_i \land \forall i.t_i \to_\beta t_{i+1}$
- Un $\lambda$ calcolo si dice avere una proprietà $Q$ sse ogni termine ce l'ha.
Si enuncia il teorema come:
$$\forall \Gamma, M, T. \Gamma \vdash M:T \implies M \text{ è fortemente normalizzante}$$
Quindi non esistono sequenze divergenti.
Dunque si ha la proprietà $Q$ indicibile che è "$M$ fortemente normalizzato"; l'approssimazione da dentro è $\Gamma \vdash M:T$. Però vi sono termini fortemente normalizzanti non tipabili, come $\lambda x.xx$

*Dimostrazione (per induzione ma che poi però non arriva a fine)*
> Visto che l'induzione è logica, per il teorema di [[3. Rapporto tra λ-calcolo e logica#Isomorfismo di Curry-Howard-Kolmogorov]] si vede come qualsiasi cosa dimostrata per induzione la si può fare anche per ricorsione ($\lambda$ calcolo quest'ultimo).

Si procede per induzione sull'albero di prova $\Gamma \vdash M:T$

Caso base: dimostrato mediante caso della variabile.
$$\frac{\begin{array}{@{}c@{}}
(x:T) \in \Gamma
\end{array}}
{
  \Gamma \vdash x:F
}$$
Bisogna dimostrare che $x$ è fortemente normalizzante, che è ovvio perché è una variabile e quindi non ha redex.

Caso $$\frac{\begin{array}{@{}c@{}}
\Gamma, x:T_1 \vdash M:T_2
\end{array}}
{
  \Gamma \vdash \lambda x.M: T_1 \to T_2
  }$$
quello sopra è un albero di prova. Per ipotesi induttiva $M$ è fortemente normalizzante. Bisogna dimostrare come $\lambda x.M$ sia fortemente normalizzante.
Per assurdo si suppone che $\lambda x.M$ non sia fortemente normalizzante, ovvero che $\lambda x.M$ riduci all'infinito. L'unico modo per fare ciò è mediante:
$$\frac{\begin{array}{@{}c@{}}M \to_\beta M_1 \to_\beta M_2 \to_\beta \cdots
\end{array}}
{
  \lambda x.M \to_\beta \lambda x.M_1 \to_\beta \lambda x.M_2 \to_\beta \cdots
  }$$
ma per ipotesi induttiva non possiamo fare ciò perché $M$ è fortemente normalizzante.

Caso $$\frac{\begin{array}{@{}c@{}}
\Gamma \vdash M:T_1 \to T_2\hspace{2em}\Gamma \vdash N:T_2 
\end{array}}
{
  \Gamma \vdash MN:T_2
  }$$
Per ipotesi induttiva $M$ è fortemente normalizzante ($I_1$), N è fortemente normalizzante ($I_2$). Bisogna dimostrare che $MN$ è fortemente normalizzante.
Farlo per assurdo come prima farebbe un po' di confusione perché si dovrebbe estendere $MN$ all'infinito con diversi casi perché vi è la combinazione di $M$ e $N$, e non posso farlo singolarmente perché sono comunque entrambi fortemente normalizzanti.
Visto che $MN$ è un applicazione vuol dire che potrebbe essere un redex. Se $M$ è nella forma $\lambda x.t$ (con $t$ fortemente normalizzante ofc) ma $MN= (\lambda x.t)N \to_\beta t[N/x]$ non c'è garanzia che $t[N/x]$ sia fortemente normalizzante. C'è un controesempio nel ciò: $(\lambda x.xx)(\lambda x.xx)$ diverge ma $\lambda x.xx$ è fortemente normalizzante. 

Questo contro esempio fa vedere come la proprietà di essere fortemente normalizzante non è modulare, mentre il tipaggio lo è. Visto che tipaggio implica normalizzazione forte, vuol dire che deve esserci una proprietà intermedia modulare che approssimi meglio la normalizzazione forte, ovvero ci deve essere l'insieme di tutti i programmi $P$, la proprietà di normalizzazione forte $SN \mathop =^{\text{def}} \{t | t \in P \land t \text{ fortemente normalizzante}\}$ e una proprietà di approssimazione per i tipati $WT \mathop =^{\text{def}} \{t \in P| \exists \Gamma, T. \Gamma \vdash t:T\}$. Bisogna trovare una proprietà che stia in mezzo a $SN$ e $WT$ e questa la si chiama $RED_T = \text{"insieme dei termini riducibili di tipo } T\text{"}$.

## Piano di lavoro 1 (che FALLISCE)
Si definisce $RED_T$ e poi bisogna dimostrare che $WT \subseteq RED_T \subseteq SN$. Si mette una proprietà in mezzo che però non può essere dimostrata la sua intuitività.

I tipi hanno strutture ricorsive quindi, per definire qualcosa sul tipo, si può procedere a definirli ricorsivamente. Quindi la definizione di $RED_T$ avviene con due casi:
$$RED_A \mathop =^\text{def} \{M | \exists \Gamma. \Gamma \vdash M:A \land M \in SN\}$$
($A$ è un tipo di cui non sappiamo nulla)
$(\star)$
$$RED_{T_1 \to T_2} \mathop = ^ \text{def} \{M |\exists \Gamma.\Gamma \vdash M:T_1 \to T_2 \land (\forall N \in RED_{T_1}. MN \in RED_{T_2})\}$$
(In realtà vi è anche una proprietà ridondante che è $M \in SN$)

**Teorema (tentativo)**: $WT \subseteq RED$ ovvero $\forall \Gamma, M, T. \Gamma \vdash M:T \implies M \in RED_T$
Per induzione sull'albero $\Gamma \vdash M: T$

- Caso $$\frac{\begin{array}{@{}c@{}}
\Gamma \vdash M:T_1 \to T_2\hspace{2em}\Gamma \vdash N:T_2 
\end{array}}
{
  \Gamma \vdash MN:T_2
  }$$
	due sotto alberi e dunque due ipotesi. Per ipotesi induttiva $M \in RED_{T_1 \to T_2} (I_1)$ e $N \in RED_{T_1} (I_2)$ Bisogna dimostrare che $MN \in RED_{T_2}$ ma essa è ovvia per $T_1, T_2$ e la definizione di riducibile $T_1 \to T_2 (\star)$.

- Caso $$\frac{\begin{array}{@{}c@{}}
(x:T) \in \Gamma
\end{array}}
{
  \Gamma \vdash x:T
  }$$
	Bisogna dimostrare che $x \in RED_T$. Bisogna dimostrare dunque che se passo in input qualcosa, il valore resti riducibile. Ovvio per il Lemma $CR\ 4$ (una qualunque variabile riducibile per qualsiasi tipo). La sigla $CR$ sta per "candidato di riducibilità", ovvero un insieme di elementi che potrebbe essere riducibile ma che lì in mezzo, effettivamente, ci sono elementi che lo sono.

- Caso $$\frac{\begin{array}{@{}c@{}}
\Gamma, x:T_1 \vdash M:T_2
\end{array}}
{
  \Gamma \vdash \lambda x.M : T_1 \to T_2
  }$$
	per ipotesi induttiva $M \in RED_{T_2}$. Bisogna dimostrare che $\lambda x.M \in RED_{T_1 \to T_2}$ ovvero $\exists \Gamma. \lambda x.M:T_1 \to T_2$ (ovvio perché è nell'ipotesi) $\land \forall N \in RED_{T_1}.(\lambda x.M)N \in RED_{T_2}$.
	Per dimostrare ciò serve:
	- (OK) Caso particolare $CR\ 3$: $(\lambda x.M)N \to t \in RED_{T_2} \implies (\lambda x.M)N \in RED_{T_2}$
	- (FALLISCE) $M \in RED_{T_2} \implies M [N/x] \in RED_{T_2}$

## Piano di lavoro 2
1. Definito $RED_T$.
2. si identificano le proprietà dei candidati $CR\ 1$ - $CR\ 3$.
3. si dimostrano $CR\ 1$ - $CR\ 3$ per $RED_T$.
4. si generalizza l'enunciato che i ben tipati sono sotto insieme dei riducibili $WT \subseteq RED_T$ usando $CR\ 1$ - $CR\ 3$.
5. si dimostra che i riducibili sono fortemente normalizzanti $RED_T \subseteq SN$ usando $CR\ 1$.

**Definizione (termine neutrale)**
Un termine $M$ è neutrale quando i redex di $N[M/x]$ sono redex di $M$ o di $N$ (non ne ho creati di nuovi). 

È molto generale perché questa definizione vale anche per altre tipologie, come ad esempio, le coppie.
Un termine neutrale è una $\lambda$ applicazione. Ad esempio, $(MN)$ è neutrale perché se $R[(MN)/x]$ contiene un redex della forma $(\lambda z.U)W$ allora non può essere stato creato perché vuol dire che prima c'era qualcosa del tipo $(\lambda z.U)x$ ma quindi vuol dire che non è stato creato perché prima era un redex. Oppure anche qualcosa del tipo $xR$ non avrebbe creato un redex nuovo.

**Teorema** 
$M$ è neutrale sse $M$ non è una $\lambda$ astrazione.

Un termine non neutrale è $\lambda x.M$, poiché $(zy)[(\lambda x.M)/z]=(\lambda x.M)y$ che è un redex ma $(\lambda x.M)y \notin \lambda x.M$ e $(\lambda x.M)y \notin (zy)$ quindi ho creato un nuovo redex! 

### Candidati di reducibilità

- $CR\ 1: RED_T \subseteq SN$
- $CR\ 2: \forall M, N. M \in RED_T \land M \to_\beta^\star N \implies N \in RED_T$
- $CR\ 3: \forall M. (M \text{ neutrale} \land (\forall N. M \to_\beta N \implies N \in RED_T) \implies M \in RED_T)$

Il fatto che sto a dire che $M \to_\beta N \land N \in RED_T$ sto dicendo che se ho $M$ che è ben tipato, una volta ridotto a $N$, anch'esso è ben tipato. Questa proprietà non vale nei sistemi di tipo, dunque non è una proprietà generale.

- $CR\ 4$ (Corollario di $CR\ 3$): $\forall T. x \in RED_T$ e si ha una dimostrazione ovvia per $CR\ 3$: $x$ è neutrale perché non è una $\lambda$ astrazione e poi vedo che in qualunque modo lo muovo, ho comunque qualcosa riducibile, ma $x$ essendo una variabile normale, non posso muoverla in alcun modo! Banalmente $x$ è riducibile su qualunque termine.

**Teorema** 
$\forall T. CR\ 1(T) \land CR\ 2(T) \land CR\ 3(T)$ 

*Dimostrazione*
Si dimostra per induzione mutua sui 3 candidati di riducibilità su $T$.

Caso $A$:
	devo dimostrare 
	- $CR\ 1(A): RED_A \subseteq SN$ ovvero $\{M | \exists \Gamma. \Gamma \vdash M:A \land M \in SN\} \subseteq SN$ (ovvio)
	- $CR\ 2(A):\forall M,N. M \in RED_A \land M \to_\beta^\star N \implies N \in RED_A$ 
		Proprietà ovvia per la proprietà di fortemente normalizzante. Se ci fosse un cammino infinito da $N$ allora ci sarebbe anche da $M$.  
	- $CR\ 3(A): \forall M. (M \text{ neutrale} \land (\forall N. M \to_\beta N \implies N \in RED_A) \implies M \in RED_A)$ (ovvio) 
		Considero tutti i modi di poter fare un passo da $M$. Se tutti i passi sono riducibili (e dunque fortemente normalizzanti dato che $RED_A \subseteq SN$) vuol dire che da qualunque passo io vada, $M$ non avrà comunque un cammino infinito.
Caso $T_1 \to T_2$:
	per ipotesi induttiva, $CR\ 1(T_1) \land CR\ 2(T_1) \land CR\ 3(T_1)$ e $CR\ 1(T_2) \land CR\ 2(T_2) \land CR\ 3(T_2)$.
	bisogna dimostrare
	- $CR\ 1(T_1 \to T_2): RED_{T_1 \to T_2} \subseteq SN$ ovvero $\{M | \exists \Gamma. \Gamma \vdash M:T_1\to T_2 \land \forall N \in RED_{T_1}.MN \in RED_{T_2}\} \subseteq SN$
	  Poiché per ipotesi induttiva vale $CR\ 3(T_1)$ allora vale anche $CR\ 4(T_1)$, ovvero $x \in RED_{T_1}$.
	  Fisso $M$ t.c. $\exists \Gamma, \Gamma \vdash M:T_1 \to T_2$ e $H = (\forall N \in RED_{T_1}. MN \in RED_{T_2})$ e dimostro $M \in SN$.
	  Da $H$ e da $x \in RED_{T_1}$ ho $Mx \in RED_{T_2}$.
	  Per ipotesi induttiva $CR\ 1(T_2)$ si ha $Mx \in SN$ quindi $M \in SN$ ed è ovvio perché se $M \to \cdots$ allora anche $Mx \to \cdots$, ed è impossibile.
	- $CR\ 2(T_1 \to T_2): \forall M, N. M \in RED_{T_1 \to T_2} \land M \to_\beta^\star N \implies N \in RED_{T_1 \to T_2}$.
	  Fisso $M, N$ t.c. $M \in RED_{T_1 \to T_2}\ (H_1)$ e $M \to_\beta^\star N\ (H_2)$. Dimostro che $N \in RED_{T_1 \to T_2} = \{U | \exists \Gamma. \Gamma \vdash U:T_1 \to T_2 \land \forall W \in RED_{T_1}. UW \in RED_{T_2}\}$ ovvero
	  1. dimostro che $\exists \Gamma. \Gamma \vdash N:T_1 \to T_2$. 
		 Preso $H_1$ e quindi da $H_2$ e *subject reduction* (se è ben tipato, rimane ben tipato, una proprietà dei sistemi di tipo) è ovvio.	
	  2. dimostro che $\forall W \in RED_{T_1}. NW \in RED_{T_2}$.
	     Fisso $W \in RED_{T_1}$ e dimostro che $NW \in RED_{T_2}$.
	     Per ipotesi induttiva vale $CR\ 2(T_2)$ ovvero $\forall V, V^\prime. V \in RED_{T_1}. V \to V^\prime \implies V^\prime \in RED_{T_2}$ e per $H_1$, $MW \in RED_{T_2}$ poiché $M \to_\beta^\star N$ per $H_2$ si ha $MW \to_\beta^\star NW$, quindi $NW \in RED_{T_2}$
	- $CR\ 3(T_1 \to T_2): \forall M. (M \text{ neutrale} \land (\forall N. M \to_\beta N \implies N \in RED_{T_1 \to T_2}) \implies M \in RED_{T_1 \to T_2})$
		 Fisso $M$ t.c. $M$ è neutrale $(H_1)$ e $\forall N.M \to_\beta N\implies N \in RED_{T_1 \to T_2}\ (H_2)$. Dimostro che $M \in RED_{T_1 \to T_2}$ ovvero
		 1. dimostro che $\exists \Gamma. \Gamma \vdash N:T_1 \to T_2$ (OMESSA, non facile, usa l'ipotesi di neutralità)
		    *Esempio* $(\lambda x.y)(\lambda z.zz)$ non è neutrale e non è tipato, ma $(\lambda x.y)(\lambda z.zz) \to_\beta y$ è ben tipato.
		 2. dimostro $\forall U. U \in RED_{T_1} \implies MU \in RED_{T_2}$.
		    Fisso $U$ t.c. $U \in RED_{T_1}\ (H_3)$ e dimostro che $MU \in RED_{T_2}$.
			> Considerando un albero finitely branching $T$ (= un nodo ha un numero finito di figli) e senza rami infiniti. Usando l'**assioma della scelta** vale che $\exists \nu(T) \in \mathbb{N}. \text{ nessun ramo è più lungo di }\nu(T)$
		    Si procede su $\nu(T)$ per dimostrare che $MU \in RED_{T_2}$
		    - caso base: $\nu(U)=0$ ovvero $U \not\to$
		      usando l'ipotesi induttiva su $CR\ 3(T_2)$ mi riduco a dimostrare che $MU \to V \implies V \in RED_{T_2}$.
		      Sia $V$ t.c. $MU \to_\beta V$ ci sono due possibilità:
		      1. $$\frac{\begin{array}{@{}c@{}}
M \to_\beta M^\prime
\end{array}}
{
  MU \to_\beta M^\prime U =V
}$$
				  per $H_2$, $M^\prime \in RED_{T_1 \to T_2}$ quindi $M^\prime U \in RED_{T_2}$ quindi $V \in RED_{T_2}$
			  2.  $MU \to_\beta V$ in quanto $M$ è una $\lambda$ astrazione, ma impossibile per $H_1$, che dice come $M$ sia neutrale, pertanto non può essere una $\lambda$ astrazione.
			- caso induttivo: $\nu(U)=n+1$ e sia $U \to_\beta U^\prime$ t.c. $\nu(U^\prime)=n$
			  Per ipotesi induttiva, se $MU^\prime \in RED_{T_2}\ (II)$. Bisogna dimostrare che $MU \in RED_{T_2}$
			  Per $CR\ 3(T_2)$ mi riduco a dimostrare che $MU \to V \implies V \in RED_{T_2}$. Ci son 3 casi, con i primi due uguali a quelli di prima.
			  3. $$\frac{\begin{array}{@{}c@{}}
U \to_\beta U^\prime
\end{array}}
{
  MU \to_\beta MU^\prime
}$$
				  per $(II)$, $MU^\prime \in RED_{T_2}$


q.e.d.

---

Ricapitolando:
Il teorema si può enunciare come
$$\forall \Gamma, M, T. \text{dato }\{N_i | (x_i: T_i) \in \Gamma, N_i \in RED_{T_i}\} \text{ si ha }\Gamma \vdash M:T \implies M[\vec{N_i}/\vec{x_i}] \in RED_{T}$$

Il corollario è
$$\forall \Gamma, M, T. \Gamma \vdash M:T \implies M \in RED_T$$
*Dimostrazione del corollario*
Per avere $M$ in $M[N_i/x_i]$ bisogna scegliere $N_i = x_i$. Ma esso dev'essere $RED_{T_i}$ ed è vero per $CR\ 4$.

*Dimostrazione del teorema per induzione*
- Caso $$\frac{\begin{array}{@{}c@{}}
(x_j:T_j) \in \Gamma
\end{array}}
{
  \Gamma \vdash x_j:T_j
  }$$
  Bisogna dimostrare che $x_j[\vec{N_i}/\vec{x_i}] \in RED_{T_j} = N_j \in RED_{T_j}$ ed è ovvio perché sono stati scelti gli $N_i$ come riducibili di $RED_{T_i}$.
  
- Caso $$\frac{\begin{array}{@{}c@{}}
\Gamma \vdash M:T_1 \to T_2\hspace{2em}\Gamma \vdash N:T_2 
\end{array}}
{
  \Gamma \vdash MN:T_2
  }$$
  Per ipotesi induttive $M[\vec{N_i}/\vec{x_i}] \in RED_{T_1 \to T_2}$ $(II_1)$ e $N[\vec{N_i}/\vec{x_i}] \in RED_{T_1}$ $(II_2)$.
  Bisogna dimostrare che $(MN)[\vec{Ni}/\vec{x_i}] \in RED_{T_2} = M[\vec{Ni}/\vec{x_i}]N[\vec{Ni}/\vec{x_i}]$ che è ovvio per $(II_1), (II_2)$ e definizione di $RED_{T_1 \to T_2}$.

- Caso $$\frac{\begin{array}{@{}c@{}}
\Gamma, x_{n+1}:T_{n+1} \vdash M:T
\end{array}}
{
  \Gamma \vdash \lambda x_{n+1}.M : T_{n+1} \to T
  }$$
  dove $n=|\Gamma|$.
  Per ipotesi induttiva $\forall N_{n+1} \in RED_{T_{n+1}}: M[\vec{N_i}/\vec{x_i}] \in RED_T$.
  Bisogna dimostrare che $(\lambda x_{n+1}.M)[\vec{N_i}/\vec{x_i}] \in T_{n+1} \to T$ ovvero 
  1. $\exists \Gamma. \Gamma \vdash (\lambda x_{n+1}.M)[\vec{N_i}/\vec{x_i}] : T_{n+1}\to T$ che è ovvio da ipotesi $\Gamma \vdash \lambda x_{n+1}.M : T_{n+1}\to T$, per $N_i:T_i$ e per un lemma
  2. $\forall N_{n+1} \in RED_{T_{n+1}}. (\lambda x_{n+1}.M)[\vec{N_i}/\vec{x_i}] \in RED_{T_{n+1}}$.
     Fissato $N_{n+1} \in RED_{T_{n+1}}$ $(H$) si può sfruttare $CR\ 3$ poiché si ha neutrale $(\lambda x_{n+1}.M)[\vec{N_i}/\vec{x_i}]N_{n+1}$. 
     (Sfruttare $CR\ 3$ vuol dire considerare tutti i possibili casi per la riduzione: se si riducesse il redex si avrebbe esattamente l'ipotesi induttiva perché si otterrebbe $M[\vec{N_i}/\vec{x_i}] \in RED_T$).
     Poiché $N_{n+1} \in RED_{T_{n+1}}$ per $(H)$ e poiché $M[\vec{N_i}/\vec{x_i}]=M[\vec{N_i}/\vec{x_i}; x_{n+1}/x_{n+1}] \in RED_{T_{n+1}} \implies \exists \nu(N_{n+1}), \nu(M[\vec{N_i}/\vec{x_i}])$.
     Per dimostrare che $\forall M, \vec{N_i}, N_{n+1}$ vale $\lambda x_{n+1}.M[\vec{N_i}/\vec{x_i}]N_{n+1} \to^\star U \in RED_T$ si procede per induzione su $\nu(N_{n+1})+ \nu(M[\vec{N_i}/\vec{x_i}])$.
     - Caso $$\frac{\begin{array}{@{}c@{}}
M[\vec{N_i}/\vec{x_i}]\to W
\end{array}}
{
  (\lambda x_{n+1}.M)[\vec{N_i}/\vec{x_i}]N_{n+1}\to_\beta (\lambda x_{n+1}.W)N_{n+1}
  }$$
	 Poiché $M[\vec{N_i}/\vec{x_i}]\to_\beta W$ si ha $\nu(M[\vec{N_i}/\vec{x_i}])=\nu(W)+1$ e si conclude per l'ipotesi induttiva.
     
     - Caso  $$\frac{\begin{array}{@{}c@{}}
N_{n+1}\to W
\end{array}}
{
  (\lambda x_{n+1}.M)[\vec{N_i}/\vec{x_i}]N_{n+1}\to_\beta (\lambda x_{n+1}.M)[\vec{N_i}/\vec{x_i}]W
  }$$
	 Analogo a quello sopra perché $\nu(N_{n+1})=\nu(W)+1$ e si conclude per l'ipotesi induttiva.
	  
     - Caso $$\frac{\begin{array}{@{}c@{}}

\end{array}}
{
  (\lambda x_{n+1}.M)[\vec{N_i}/\vec{x_i}]N_{n+1}\to_\beta M[\vec{N_i}/\vec{x_i}; N_{n+1}/x_{n+1}]
  }$$
       ed è valida per $II$.
