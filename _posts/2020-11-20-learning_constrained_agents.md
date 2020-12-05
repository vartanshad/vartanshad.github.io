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
 
There are two tricky pieces to this problem. First, how do we model the informational (memory) constraints here? And second, how do we model the agent's decision problem? 

In thinking about this problem, I was reminded of Chris Sim's now famous rational inattention model. We *could* use the Sims framework here. The student might have some limited ability to pay attention to a random variable representing the "right" answer. Given that, the student chooses to allocate their attention strategically. My feeling, though, is that this approach is inadequate, because this decision -- to allocate attention -- is made *blindly.*: the student does not see the right answer when they choose what to pay attention to. This feels unrealistic, because as we stipulated, a student has access to all information the night before: they can see everything, including information that would have surprised them when they first saw it.

In recent days I have been thinking a lot about the idea of agents that 'learn'. Consider the following example: a student is taking an exam the next day, and in the late hours of the night has to cram. At that moment in time, the student understands the subject -- they can, in practical terms, answer any question about the subject, since they have access to books and their class materials. But the next day, the student will not have access to their materials, and so must rely on what they have managed to learn by heart the night before. The question for the student then is: subject to memory and cognitive constraints, what should they learn the night before? How does the student make such a decision? And how can we model this? 

Kullback-Leibler Divergence is defined as (in the discrete case):

$$D_{KL}(P| | Q) = \sum_{x\in X}P(x)\log \left(\frac{P(x)}{Q(x)}\right)$$

My proposal: a multiself approach 
---------------------------

I propose the following setup. There are two selves: the student the night before, and the student the day of the exam. The student the night observes all information, including the realization of a variable $Y$ which we call the exam's 'right answers'. $Y$ is distributed according to distribution $Q$. However, they have a limited ability to retain information. Specifically, the student on the night before can only communicate to their future self the outcome of a variable $X$, with distribution $P$. And they face the constraint that

$$D_{KL}(P| | Q)< \kappa$$



