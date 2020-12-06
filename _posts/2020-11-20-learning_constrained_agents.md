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

To help inform my approach, I use two quantities that are similarly motivated to mutual information. The first is **Kullback-Leibler Divergence** which is defined as (in the discrete case):

$$D_{KL}(P| | Q) = \sum_{x\in X}P(x)\log \left(\frac{P(x)}{Q(x)}\right)$$

and is a measure of the distance between the two distributions. The second is **entropy** which is measured as

$$
H(P) = \sum_{x\in X}P(x)\log \left(\frac{P(x)}\right)$$
$$

My proposal: a multiself approach 
---------------------------

I propose the following setup. There are two selves: the student the night before (the present student), and the student the day of the exam (the future student). The student the night observes all information, including the realization of a variable $Y$ which we call the exam's 'right answers'. Call this realization $y$. $Y$ is distributed according to distribution $Q$, which represents the student's prior -- effectively, a representation of the information they have already internalized. However, they have a limited ability to retain information. Specifically, the present student can only communicate to their future self the outcome of a variable $X$, with a distribution $P$, which has the same support as $Q$. The present student chooses the distribution of $P$, subject to the following constraint:

$$D_{KL}(P| | Q)< \kappa$$

Where $\kappa$ is some exogenous informational/cognitive constraint. That is, the variable that the future student $X$ sees has a distribution that can only differ from the student's prior by some amount bounded by the Kullback-Leibler constraint. $X$ represents, in a way, the 'memory' of the student. Here, memory acts as a garbled version of reality. Our constraint is on how much more informative this garbled version can be relative to the student's prior.

Our story does not end here. Note that what $P$ is chosen by the agent can (and generally will) depend on the realization that the student

An alternate version: entropy
-----------------

The previous version compared the distribution of the signal that the student can send themselves to the prior distribution that they held before they saw any information. However, another, possibly cleaner version is in terms of entropy:

