---
title: "(TRAVEL) Provably Efficient Learning of Transferable Rewards"
date: 2024-09-23 21:00:00 -0700
category: Note
comments: true
share: true
related: false
mathjax: true
---

This is a recap of the regert analysis in [(TRAVEL) Provably Efficient Learning of Transferable Rewards](https://proceedings.mlr.press/v139/metelli21a.html).

## 1. Motivation
Inverse reinforcement learning tackles the challenge of deriving a reward function from observations of an expert's policy within a Markov Decision Process (MDP) framework. Typically, we operate under the assumption that the underlying MDP model is not known. To determine a viable reward function, it may be necessary to first reconstruct the transition model.

The performance of rebuilt reward can be measured in different ways:
- 1. **Policy difference** If we only care about the similarity of learnt policy induced by the rebuilt reward function to the expert policy, the problem is a Behavior Cloning problem while BC does not require to rebuilding reward function.
- 2. **Value difference** If we only care about the empirical performance of induced policy by the rebuilt reward, we measure the value function.
- 3. **Reward difference** We can measure the difference between the rebuilt function and the underlying reward function, under some strong assumptions of reward uniqueness.

Since we need to rebuilt reward function based on rebuilt transition model, a natural question is 

**Q1: How does the error on the rebuilt transition model and on the expert's policy propagate to the recovered reward?**

After reconstructing the reward function, we are also concerned with its performance, specifically the value of the induced policy compared to the expert policy. Furthermore, the policy's value should be evaluated in a different environment, as the reconstructed transition model differs from the underlying transition model, leading us to the second question.

**Q2: How does the error on the recovered reward affect the performance of the policy learned in a different environment?**

In the work of this paper (TRAVEL), it makes contributions summarized 

1. (Answer to Q1) Deriving an error bound on the recovered reward caused by the estimation of the transition model and of the expert's policy.
2. (Answer to Q2) Deriving an bound on the induced policy in the target environment (known) from demonstrations on the source environment (unknow).
3. (Sample complexity analysis) Based on 1. and 2., they apply a sample complexity analysis by considering a uniform sampling strategy for the IRL problem.
4. (Algorithm) Proposing a new algorithm caleed *Transferable Reward ActiVEirL* (TRAVEL) that adapts the sampling strategy to the features of the problem.

The Inverse RL algorithm is a mapping from the problem space $\mathfrak{P}$ to the induced reward space $R_{\mathfrak{P}}$. We require that for any reward function from $R_{\mathfrak{P}}$, the expert policy should always be optimal in the given environment $M$. However, due to the ambiguity of the IRL problem, the solution of an IRL algorithm might not be unique, which means that $R_{\mathfrak{P}}$ might not be a single-element set.

## 2. Measuring Rewards in the Feasible Set
**Lemma 3.2** (Feasible Reward Set Explicit) *Let $\mathfrak{P}=(\mathcal{M}, \pi^E)$ be an IRL problem. Let $r\in \mathbb{R}^{\mathcal{S} \times \mathcal{A}}$, then $r$ is a feasible reward, i.e., $r\in R_{\mathfrak{P}}$ if and only if there exist $\zeta \in R^{\mathcal{S} \times \mathcal{A}}_{\geq 0}$ and $V \in \mathbb{R}^{\mathcal{S}}$ such that:*
$$
    r = - \bar{B}^{\pi^E} \zeta + (E - \gamma P) V.
$$


**Theorem 3.1** (Error Propagation) *Let $\mathfrak{P} = (\mathcal{M}, \pi^E)$ and $\hat{\mathfrak{P}} = (\hat{\mathcal{M}}, \hat{\pi}^E)$ be two IRL problems. Then, for any $r \in R_{\mathfrak{P}}$ such that $r=-\bar{B} ^{\pi^E}\zeta + (E-\gamma P)V$ and $||r||_\infty \leq R_{\max}$ there exists $|hat{r}\in R_{\mathfrak{P}}$ such that element-wise it holds that*
$$
    |r - \hat{r}| \leq \hat{B}^{\pi^E} B^{\hat{\pi}^E} \zeta + \gamma |(P - \hat{P})V|
$$
*Furthermore, $||\zeta||_\infty \leq \frac{R_{\max}}{1-\gamma}$ and $||V||_{\infty} \leq \frac{R_{\max}}{1-\gamma}$.*

On the RHS, the first term quantifies the error brought by the misspecification of the expert policy $\pi^E$ and the estimated policy $\hat{\pi}^E$. The second term represents the error due to the difference between the true transition model $P$ and the estimated transition model $\hat{P}$. The second term discribes the error from the estimation on the transition model.

## 3. Transferring Rewards

In this section, the author is going to address the challenge of transferring rewards. Specifically, suppose that there is an **unknown source** environment $\mathcal{M}$ and a **known target** environment $\mathcal{M}'$, and a reward function $r^E$ such that its induced policy $\pi^E$ is optimal in $\mathcal{M}$ and $(\pi')^E$ is optimal in $\mathcal{M}'$. If we can solve the source IRL problem $\mathfrak{P}=(\mathcal{M}, \pi^E)$ to derive a rebuilt reward $R_{\mathfrak{P}}$, the rebuilt reward does not necessarily have its induced optimal policy $\pi^E_{R_{\mathfrak{P}}}$ equal to $\pi^E$. Equivalently, the rebuilt reward $R_{\mathfrak{P}}$ might not be equal to $r^E$ due to the ambiguity. To address this additional ambiguity issue, the authors introduces the following assumption

**Assumption 4.1** *Let $\mathfrak{P}=(\mathcal{M}, \pi^E)$ and $\mathfrak{P}'=(\mathcal{M}', (\pi')^E)$ be the source and target IRL problems. The corresponding feasible sets satisfy $\mathcal{R}_{\mathfrak{P}} \subseteq \mathcal{R}_{\mathfrak{P}'}$.*

The intuition behind Assumption 4.1 is that once we observe the expert's behavior in the source environment, we can conclude that the action taken by the expert is optimal, but we do not know how much it differs from other suboptimal actions.

## 4. Learning Transferable Rewards with a Generative Model

## 5. Active Learning of Transferable Rewards

