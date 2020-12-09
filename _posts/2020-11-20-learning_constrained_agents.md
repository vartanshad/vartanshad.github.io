---
title: 'Learning-Constrained Agents'
date: 2020-11-20
permalink: /posts/2020/11/lca/
tags:
  - economics
  - model
---

In this blog post, I explore the concept of a "learning-constrained agent," and set out directions for a future research agenda.

Learning-Constrained Agents
======

In recent days I have been thinking a lot about the idea of agents that 'learn'. Consider the following example: a student is taking an exam the next day, and in the late hours of the night has to cram. At that moment in time, the student understands the subject -- they can, in practical terms, answer any question about the subject, since they have access to books and their class materials. But the next day, the student will not have access to their materials, and so must rely on what they have managed to learn by heart the night before. The question for the student then is: subject to memory and cognitive constraints, what should they learn the night before? How does the student make such a decision? And how can we model this? 

There are two tricky pieces to this problem. First, how do we model the informational (memory) constraints here? And second, how do we model the agent's decision problem? 

In thinking about this problem, I was reminded of Chris Sim's now famous rational inattention model. We *could* use the Sims framework here. The student might have some limited ability to pay attention to a random variable representing the "right" answer. Given that, the student chooses to allocate their attention strategically. My feeling, though, is that this approach is inadequate, because this decision -- to allocate attention -- is made *blindly*: the student does not see the right answer when they choose what to pay attention to. This feels unrealistic, because as we stipulated, a student has access to all information the night before: they can see everything, including information that would have surprised them when they first saw it.

Therefore, to help inform my approach, I use two quantities that are similarly motivated to mutual information. The first is **Kullback-Leibler Divergence** (or relative entropy) which is defined as (in the discrete case):

$$D_{KL}(P| | Q) = \sum_{x\in X}P(x)\log \left(\frac{P(x)}{Q(x)}\right)$$

and is a measure of the distance between the two distributions. The second, which is only a measure for a single distribution, is **Entropy**, which is measured as

$$H(P) = \sum_{x\in X}P(x)\log \left(P(x)\right)$$

which is a measure of the expected amount of 'surprise' contained in the realization of the distribution. So, if one flips a coin with probability $p$ of landing heads, the entropy is highest when $p=0.5$. Note that the entropy of a distribution is at its maximum for a given support when any of the events are equally likely.

Both these measures have desirable information-theoretic properties. I will not go into this in detail, but see [here](https://en.wikipedia.org/wiki/Entropy_(information_theory)) and [here](https://en.wikipedia.org/wiki/Relative_entropy). Besides having some desirable mathematical properties, these measures have interpretations in terms of quantifying flows of information. I will outline two different versions of this idea.

Rational Learning-Constrained Agents: A Multiself Approach 
---------------------------

I propose the following setup. There are two selves: the student the night before (**the present student**), and the student the day of the exam (**the future student**). The student the night observes all information, including the realization of a variable $Y$ which we call the exam's 'right answers'. Call this realization $y$. $Y$ is distributed according to distribution $Q$, which represents the student's prior -- effectively, a representation of the information they have already internalized. However, they have a limited ability to retain information. Specifically, the present student can only communicate to their future self the outcome of a variable $X$, with a distribution $P$, which has the same support as $Q$. The present student chooses the distribution of $P$, subject to the following entropy constraint:

$$H(P)> \kappa$$

Where $\kappa$ is some exogenous informational/cognitive constraint.[^1] $X$ represents, in a way, the 'memory' of the student. Here, memory acts as a garbled version of reality, which we take here to mean that it is a random variable with a certain amount of entropy. One can also think of this as adding noise to the present student's signal, where we derive a constraint for how much the student can limit this noise that works universally across all information structures.

Therefore, our constraint is on how informative this signal can be. When $\kappa$ is very low, the distribution $P$ can be designed by the student so that some outcomes are very unlikely and unlikely, making the signal very informative.

Our story does not end here. The future student sees $X$ and must make some decision (such as what to write on the exam). Note that what $P$ is chosen by the student can (and generally will) depend on the realization that they saw. Therefore, call the present student's strategy $P(y)$. The future student has a conjecture about this strategy, $\tilde{P(y)}$, and forms a posterior -- via Bayes' rule -- based on the signal they saw and the conjecture they held. In equilibrium, this conjecture is correct; the present student indeed finds it optimal to play $\tilde{P(y)}$.

**A Simple Example**

The following example will not necessarily provide new insights, but hopefully will help readers grasp the above concepts. Consider that present student observes variable $Y$, with support $\lbrace 0,1 \rbrace$. Call this realization $y$.

In everyday terms, this would mean that the student sees the answer to the test the day before, and it is either $0$ or $1$ with equal probability.

Unfortunately, due to some truly abysmal memory, any signal present student chooses to send to future student contains some amount of entropy of at least $\kappa$. This signal $X$ has the same support as $Y$: $\lbrace0,1\rbrace$. Future student observes the realization of $X$, and chooses an action $a \in \lbrace0,1 \rbrace$ such that student's utility is $1$ if $a$ matches $y$, and $0$ otherwise.

We make an assumption that $P(X=z\vertY=z)>P(X=z\vertY\neq z)$ for $z\in \lbrace0,1 \rbrace$. In this case, the present student's decision effectively boils down to a decision of $p$, where $p = P(X = 1)$. [^2] 

Given the simple nature of the problem here, we make the following guess about equilibrium (that will prove to be correct): the agent will set $p$ as high as $\kappa$ allows if $y=1$, and as low as possible if $p$. Specifically, given our constraint, this means that

$$p \log \left(p\right) + 1-p log  \left(1-p\right)= \kappa$$

There are two values that meet such a standard. Call these $p_{l}(\kappa)$ and $p_{h}(\kappa)$. Now, if future student indeed believes these are the probabilities that present student will choose they form the following posterior beliefs:


$$
P(Y = 1|X=1) = \frac{P(X = 1 | Y = 1)P(Y=1)   }{P(X=1) } = \frac{0.5p_h}{0.5p_h + 0.5 p_l}
$$

and

$$
P(Y = 1|X=0) = \frac{P(X = 0 | Y = 1)P(Y=1)   }{P(X=0) } = \frac{0.5(1-p_h)}{0.5(1-p_h) + 0.5(1-p_l)}
$$



Semi-Behavioral Learning-Constrained Agents
------------

We might prefer a different approach. The above model might become intractable, since it involves us finding a fixed point when we are dealing with large strategy spaces (in our case, spaces over distributions). We might also not necessarily believe that the future student does the kind of complicated equilibrium inference that they do in the multiself approach. They might have to simply 'do' with the memory that they have internalized, engaging their [System 1 cognition](https://books.google.ca/books?id=ZuKTvERuPG8C&redir_esc=y).

In that sense, future student might be *mechanical*. So we posit the following. Present student, as before, sees the random variable $Y$ (which has distribution $Q$ which is also the student's prior). They then directly choose the future student's posterior, $P$, subject to the constraint that

$$
D_{KL}(P||Q) <\kappa
$$

That is, the posterior can differ from the prior only by some amount bounded by $\kappa$, where Kullback-Leibler Divergence is the relevant measure of distance. The future student, being 'semi-behavioral' in that they simply optimize taking their posterior as given (and don't play the kind of multiself equilibrium described above), chooses their action to maximize their expected utility if their beliefs about the true outcome were indeed $Q$.

Some interesting differences arise in this version. You can get truly non-Bayesian updating, and in some cases the student might prefer to 'fool' future student to get better decisions. But that will come in the next part.





[^1]: We can also model this in terms of a cost to utility $\kappa(H(P))$.

[^2]: Technically, present student can use the state $0$ to communicate to future student that $1$ is the right answer, and vice verse. We rule out such equilibria by assumption, but it is true that there is nothing privileged about the labels on the support set.
