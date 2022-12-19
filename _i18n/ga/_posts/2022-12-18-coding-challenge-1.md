---
title: 'Díorthaigh feidhm a ionsamhlaíonn bonn neamhchlaonta - Dúshlán Códaithe 1'
date: 2022-12-18
permalink: /posts/2022/12/coding-challenge-1/
tags:
  - coding challenge
  - biased
  - simulate
  - coin
  - unbiased
---

> Bíodh `toss_biased` ina fheidhm a thuganns ceann (H) or cruit (T) le dóchúlacht $p$ agus $q$ faoi sheach, áit a bhfuil $p \neq q$, .i. tá an bonn claonta. <br/><br/>
Díorthaigh feidhm a ionsamhlaíonn bonn neamhchlaonta.

Cuimhnigh ar dhá bhonn neamhspleácha

$$
B^{(1)} \sim \text{ toss_biased() } \\
B^{(2)} \sim \text{ toss_biased() }
\notag
$$

Ina theannta sin, cuimhnigh ar na péirí teagmhas a leanas

$$
\begin{align}
\{H^{(1)}H^{(2)}\}, \{T^{(1)}T^{(2)}\} \notag \\
\{H^{(1)}T^{(2)}\}, \{T^{(1)}H^{(2)}\} \notag
\end{align} \notag
$$

áit arb iad $H^{(i)}, T^{(i)}$ na teagmhais a aschuireann bonn $B^{(i)}$ $H$ nó $T$.

Mar gheall ar neamhspleáchas, is féidir an dóchúlacht a ríomh le riail an toraidh $P(B^{(1)}, B^{(2)})=P(B^{(1)})P(B^{(2)})$. Ag seo a gcomhdóchúlachtaí

| $P$       | $H^{(1)}$ | $T^{(1)}$ |
| ----      | --------- | --------- |
| $H^{(2)}$ | $p^2$     | $pq$      |
| $T^{(2)}$ | $pq$      | $q^2$     |

Anois tabhair faoi deara go bhfuil $P(H^{(1)},T^{(2)})=P(T^{(1)},H^{(2)})$. Saothróidh muid an t-airí seo mar bhunús don réiteach ar an dúshlán seo. Bíodh

$$
\bar{H}=\{H^{(1)}T^{(2)}\}\\
\bar{T}=\{T^{(1)}H^{(2)}\}
\notag
$$

mar thorthaí féidearthacha an bhoinn nua $\bar{B}$. Is léir ón airí thuas go bhfuil

$$P(\bar{H}) = P(\bar{T}) \notag$$

agus is bonn neamhchlaonta é $\bar{B}$ dá réir. Mar aon leis, cuimhnigh ar na tacair a leanas

$$
J=\{H^{(1)}T^{(2)},T^{(1)}H^{(2)}\}=\{\bar{H},\bar{T}\}\\
K=\{H^{(1)}H^{(2)},T^{(1)}T^{(2)}\}
\notag
$$

Tabhair faoi deara go bhfuil $r:=P(J)=2pq$ agus $s:=P(K)=p^2+q^2$. Féadfaimis na teagmhais thuas a bhreacadh mar chrann thíos
<div style="text-align: center; padding-bottom: 16px;">
<img src="/images/posts/coding-challenge-1/tree-graph.gv.svg?raw=true" alt="Léaráid Mharkov den phróiseas togartha" />
</div>

Is é $J$ an t-aon teagmhas ina bhfáightear na teagmhais $\bar{H}$ nó $\bar{T}$ agus tá muid ag iarraidh go dtarlóidh teagmhas $J$ lena thoradh neamhchlaonta a fháil. Beidh orainn an próiseas a reáchtáil go hathchúrsach le go dtarlóidh $J$. Dá dhá bhrí sin, is dual dúinn cuimhniú ar an gcrann mar phróiseas Mharkov $X_n$

<div style="text-align: center; padding-bottom: 16px;">

<img src="/images/posts/coding-challenge-1/markov-graph.gv.svg?raw=true" alt="Léaráid Mharkov den phróiseas togartha" />
$
P = \begin{bmatrix}
    r & s \\
    0 & 1
\end{bmatrix}
$
</div>

Tabhair faoi deara go bhfuil $P(T_J = k) = s^{k-1}r$. Ríomhadh muid $T_J$ go dtí go mbeidh $X_n = K$ i gcás $n \geq 1$ eicínt. Is í an dóchúlacht go mbaintear $K$ amach agus $n$ ag teannadh $n \to \infty$, .i. go mbainfear $K$ amach thar éis céimeanna éigríche, ná:

$$
\begin{align}
P(T_J \geq 0) &= \sum^{\infty}_{k=1} P(T_J = k)\notag \\
&= \sum^{\infty}_{k=1} s^{k-1}r \notag \\
&= \frac{r}{1-s} \notag \\
&= \frac{2pq}{1-p^2-q^2} \notag \\
&= 1 \notag 
\end{align} \notag
$$

Dhá bhrí sin, tarlaíonn teagmhas $J$ thar éis oiread céimeanna éigríche. Ós go dtarlaíonn teagmhas $J$ beagnach go cinnte, cuimhníodh muid ar an bhfeidhm a leanas

```
unbiased_toss() {
  b1 = toss_biased()
  b2 = toss_biased()

  if b1 == b2 {
    # Teagmhas K
    # Fós ag fanacht go dtarlóidh an teagmhas J a bhfuil toradh neamhchlaonta air. 
    # Cuirfidh muid unbiased_toss i gcrích aríst.
    return unbiased_toss()
  } else {
    # Teagmhas J
    # Toradh neamhchlaonta a thabhairt.
    if (b1 == H) {
      return H_unbiased
    } else {
      return T_unbiased
    }
  }
}
```