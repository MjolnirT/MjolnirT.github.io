---
title: "(Inverse Bandit) Learning from an Exploring Demonstrator: Optimal Reward Estimation for Bandits"
date: 2024-02-28 10:50:00 -0700
category: Note
comments: true
share: true
related: false
mathjax: true

---

This is a recap of the "inverse bandit" problem first proposed in [this paper](https://arxiv.org/pdf/2106.14866.pdf).

## 1. Motivation

In the Reinforcement learning area, most of the time we assume the reward feedback is accessible and we can do Value-Iteration or Policy-Iteration to find the optimal value function. Another approach is that the agent or the designer of the RL algorithm is not able to obtain precise reward feedback from the environment, called **Inverse Reinforcement Learning**. Several approaches can be used to solve this problem. One is to mimic the behavior of an expert. We have an expert to demonstrate how to finish the task with a sequence of near-optimal/optimal actions and let the agent imitate the expert's behavior, which is called **Imitation Learning (IL)**. The advantage of IL is obvious that we do not have to engineer the reward function and avoid the misspecification problem when the reward function is not aligned with our expectations. However, the policy learned from IL is hard to surpass the expert's demonstration. Also, by only observing the expert's policy we may not obtain a uniquely identifiable reward function since there might be infinitely many reward functions that can explain the expert's policy. Also, we can learn to construct reward functions through the interaction with the environment, called **Reward Modeling**. 

However, the authors in this paper believe that the optimal policy leaks less information about the suboptimal actions and the learning agent cannot learn those suboptimal actions enough to rebuild the reward function. Then [guo2022learning](https://arxiv.org/pdf/2106.14866.pdf) introduces the "inverse bandit" problem that forces the agent to learn from the learning trajectory of another bandit algorithm.

### 1.1 The "Inverse Bandit" Problem

Suppose that the agent can observe a demonstration which is sequence of decisions made by another bandit algorithm, $$\{ I_t \}_{t=1}^T$$, from the demonstration the agent expects to estimate the expected rewards $$\{ \mu_i \}_{i\in[K]}$$ in a $$K$$-armed bandit setting. Basically, the reward estimation can be viewed as a mapping from $$\{ I_t \}_{t=1}^T$$ $$ to $$\{ \hat\{mu}_i \}_{i\in[K]}$$


$$
\text{Reward estimation :} \{ I_t \}_{t=1}^T \to\{ \hat{\mu}_i \}_{i\in[K]}
$$


A good estimation can minimize the misspecification error $$\mathbb{E}[ \mid \hat{\mu}_i - \mu_i \mid]$$

### 1.2 The Base Line - Information-theoretic Lower Bound

**Theorem 1** *For every $$K$$-armed Bernoulli bandit instance $$\mathcal{M}$$ satisfying $$\max_{i\in[K]} \mid \mu_i - \frac{1}{2} \mid \leq \frac{1}{4}$$ and for each suboptimal arm $$i\neq i^\star$$, the following is true. Suppose that the demonstrator employs algorithm $$\mathbb{A}$$, and let $$\mathbb{E}[N^{\mathbb{A}}_{t, T}]$$ denote the expected number of times arm $$i$$ is pulled by $$\mathbb{A}$$ when presented the instance $$\mathcal{M}$$. Then there exists an instance $$\mathcal{M}'$$ such that for any reward estimation procedure having knowledge of $$\mu^\star$$ and mappling $$\{I_t\}_{t=1}^T \to \{ \hat{\mu}_i \}_{i\in[K], i \neq i^\star}$$,*


$$
\max_{\tilde{\mathcal{M}} \in \{ \mathcal{M}, \mathcal{M}' \}} \mathbb{E}[|\hat{\mu}_i - \mu_i(\tilde{\mathcal{M}})|]
\geq
\frac{1}{16} \cdot \left( \frac{1}{\mathbb{E}[N^{\mathbb{A}}_{t, T}]} \wedge 1 \right)
$$
