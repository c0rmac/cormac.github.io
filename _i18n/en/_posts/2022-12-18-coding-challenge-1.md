---
title: 'Derive a function that simulates an unbiased coin toss - Coding Challenge 1'
date: 2022-12-18
permalink: /posts/2022/12/coding-challenge-1/
tags:
  - coding challenge
  - biased
  - simulate
  - coin
  - unbiased
---

> Suppose toss_biased is a function which returns Heads (H) or Tails (T) with probability $p$ and $q$, $$ p \neq q $$, ie. the toss is biased. <br/><br/>
Derive a function that simulates an unbiased coin toss

Consider two independent bases:

$$
B^{(1)} \sim \text{ toss_biased() } \\
B^{(2)} \sim \text{ toss_biased() }
$$

In addition, consider the following pairs of events:

$$
\{H^{(1)}H^{(2)}\}, \{T^{(1)}T^{(2)}\}, \{H^{(1)}T^{(2)}\}, \{T^{(1)}H^{(2)}\}
$$

where $H^{(i)}, T^{(i)}$ are the events that coin $B^{(i)}$ outputs $H$ or $T$.

Due to independence, the probability can be calculated by the product rule $P(B^{(1)}=b^{(1)}, B^{(2)}=b^{(2)}) =P(B^{(1)}=b^{(1)})P(B^{(2)}=b^{(2)})$. The following are the joint probabilities:

| $P$       | $H^{(1)}$ | $T^{(1)}$ |
| ----      | --------- | --------- |
| $H^{(2)}$ | $p^2$     | $pq$      |
| $T^{(2)}$ | $pq$      | $q^2$     |

Now note that $P(H^{(1)},T^{(2)})=P(T^{(1)},H^{(2)})$. We will exploit this property to answer this challenge. Let:

$$
\bar{H}=\{H^{(1)}T^{(2)}\}\\
\bar{T}=\{T^{(1)}H^{(2)}\}
$$

be the possible outcomes of the new coin $\bar{B}$. It is clear from the above property that:

$$P(\bar{H}) = P(\bar{T})$$

and $\bar{B}$ is therefore an unbiased coin. Also, consider the following sets:

$$
J=\{H^{(1)}T^{(2)},T^{(1)}H^{(2)}\}=\{\bar{H},\bar{T}\}\\
K=\{H^{(1)}H^{(2)},T^{(1)}T^{(2)}\}
$$

Note that $r:=P(J)=2pq$ and $s:=P(K)=p^2+q^2$. We may plot the above events as a tree below:
<div style="text-align: center; padding-bottom: 16px;">
<img src="/images/posts/coding-challenge-1/tree-graph.gv.svg?raw=true" alt="Léaráid Mharkov den phróiseas togartha" />
</div>

$J$ is the only event in which the events $\bar{H}$ or $\bar{T}$ are attained and we want event $J$ to occur to obtain its unbiased result. We will need to run the process recursively for $J$ to occur. Therefore, its natural to consider the above tree diagram as a Markov process $X_n$:

<div style="text-align: center; padding-bottom: 16px;">

<img src="/images/posts/coding-challenge-1/markov-graph.gv.svg?raw=true" alt="Léaráid Mharkov den phróiseas togartha" />
$
P = \begin{bmatrix}
    r & s \\
    0 & 1
\end{bmatrix}
$
</div>

Note that $P(T_J = k) = s^{k-1}r$. We shall calculate $T_J$ until $X_n = K$ for some $n \geq 1$. The probability that $K$ is reached as $n \to \infty$, i.e. that $K$ will be reached after infinite steps, is:

$$
P(T_J \geq 0) =  \sum^{\infty}_{k=1} P(T_J = k) 
= \sum^{\infty}_{k=1} s^{k-1}r
= \frac{r}{1-s} = \frac{2pq}{1-p^2-q^2} = 1
$$

Therefore, event $J$ occurs after an infinite number of steps. Given that $J$ occurs almost certainly, we shall consider the following function:

```
unbiased_toss() {
  b1 = toss_biased()
  b2 = toss_biased()

  if b1 == b2 {
    # Teagmhas K
    # Still waiting for the event J to occur which has an unbiased result.
     # We will execute unbiased_toss again.
    return unbiased_toss()
  } else {
    # Teagmhas J
    # Give an unbiased result.
    if (b1 == H) {
      return H_unbiased
    } else {
      return T_unbiased
    }
  }
}
```