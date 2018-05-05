---
layout: post
title: "A surprisingly uniform body of water"
date: 2018-05-05 12:00:00 -0500
permalink: /brews/:title
---

Consider the following problem.

We have a stream of incoming data \$$\mathbb{S} = X_1, X_2, \dots, X_N$$.
\$$N$$ is *not* known in advance (i.e. \$$N$$ is random) and is
potentially very very big.

The task is to draw a sample $S \subseteq \mathbb{S}$ of size $k \ge 1$
such that $P(X_i \in S) = \min (k / N, 1)$
for any $i \in \{1, \dots, N\}$.


To be clear, here $k \ll N$, and $N$ could be so large that we cannot
simply store all of the elements of $\mathbb{S}$ in memory *first*
and *then* perform the sampling once the stream ends.
He-he!

Amazingly, there exists a statistimagical algorithm that allows us to
draw such a sample.
The problem is often referred to as *reservoir sampling*.

Here is a viable strategy.

We start by saving the first $k$ elements of the stream $\mathbb{S}$
in an array as they come.
If we denote with $S^i$ the sample after we observed $X_i$, the $i$-th
element of $\mathbb{S}$, then we can write $P(X_i \in S^i) = 1$
for any $i \le k$.
Furthermore, note that $S^1 \subseteq S^2 \subseteq \dots \subseteq S^k$
and that $S = S^N$.

- If the stream ends before or exactly when we reach the $k$-th element,
  i.e. $N \le k$, we simply end up sampling the whole stream:
  $S = \mathbb{S}$ and $P(X_i \in S) = 1$
  for all $i \in \{1, \dots, k\}$.

- If $N > k$, then we proceed as follows:

  for $i > k$,

  - with probability $k / i$,

    1. save $X_i$, the $i$-th element of the stream

    2. randomly select one of the elements of $S^i$

    3. pop the randomly selected element out of $S^i$

    4. fill the empty slot of $S^i$ with $X_i$

    5. this modified version of $S^i$ becomes $S^{i+1}$.

  - with probability $1 - k / i$, ignore $X_i$ and set $S^{i + 1} = S^i$.

To convince ourselves that this actually generates a sample such that
$P(X_i \in S) = \min (k / N, 1)$ for $i \in \{1, \dots, N\}$,
it is enough to use an induction argument.
  
Let's start by considering iteration $k + 1$: at this stage,
the probability that $X_j$ for $j \le k$ is retained in $S^{k+1}$
is the probability of the event "$X_{k+1}$ is discarded or $X_{k+1}$ is
selected but $X_j$ is not popped out":
$$
\begin{align}
P(X \in S^{k+1}) & =
1 - \frac{k}{k + 1} + \frac{k}{k + 1}\frac{k-1}{k} \\
& = \frac{1}{k + 1} + \frac{k-1}{k+1} \\
& = \frac{k}{k+1}
\end{align}
$$
Of course, $P(X_{k+1} \in S^{k+1}) = k / (k + 1)$ by design.

Now, let's assume that the above also holds for a generic $j > k + 1$,
i.e. $P(X_i \in S^j) = k / j$ for every $i \le j$.
Then, for $j + 1$, we have that

- $P(X_{j+1} \in S^{j+1}) = k / (j + 1)$ by design

- For $i \le j$,
  $$
  \begin{align}
  & P(X_i \in S^{j + 1}) = \\
  & = P(X_i \in S^j) \left(
  P(X_{j+1} \text{ is discarded }) +
  P(X_i \text{ is not swapped with } X_{j+1} \mid X_i \in S^j)
  \right) \\
  & = \frac{k}{j}\left(
  1 - \frac{k}{j + 1} + 
  \frac{k}{j + 1}\frac{k - 1}{k}
  \right) \\ 
  & = \frac{k}{j}\left(
  \frac{j + 1 - k}{j+1} + \frac{k - 1}{j + 1}
  \right) \\
  & = 
  \frac{k}{j}\frac{j}{j + 1} \\
  & = \frac{k}{j + 1}
  \end{align}
  $$

[Here](https://github.com/mattiaciollaro/reservoir) you can find a
Python 3 implementation of this procedure.
