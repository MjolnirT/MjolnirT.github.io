---
title: "Soft Performance Difference Lemma"
date: 2026-02-22 21:00:00 -0700
category: Note
comments: true
share: true
related: false
mathjax: true
classes: wide
---

This note presents the soft performance difference lemma, which is a fundamental result in regularized reinforcement learning. Nowadays, regularized reinforcement learning has become increasingly popular due to its ability to encourage exploration and improve the stability of learning algorithms, such as Soft Actor-Critic (SAC) and Proximal Policy Optimization (PPO), which has been widely used in various applications, including robotics, game playing and natural language processing (ChatGPT). The soft performance difference lemma provides a theoretical foundation for understanding the performance difference between two policies in the regularized setting, which can be used to analyze and design regularized reinforcement learning algorithms.

# 1. Basic Notations

Consider an infinite-horizon Markov Decision Process (MDP) defined by the tuple $$(\mathcal{S}, \mathcal{A}, P, r, \gamma, s_0)$$, where $$\mathcal{S}$$ is the state space, $$\mathcal{A}$$ is the action space, $$P$$ is the transition probability function, $$r$$ is the reward function, $$\gamma \in [0, 1)$$ is the discount factor and $$s_0$$ is a fixed initial state.
A policy $$\pi: \mathcal{S} \rightarrow \Delta(\mathcal{A})$$ maps states to probability distributions over actions.
We omit the dependence of $$s_0$$ for simplicity in the following analysis.
The value function and the Q-function for a policy $$\pi$$ are defined as follows:

$$
\begin{aligned}
    V^\pi(s) &= \mathbb{E}_{ \tau \sim \pi, s_0 = s }\left[ \sum_{t=0}^\infty \gamma^t  r(s_t, a_t) \right] = \mathbb{E}_{a \sim \pi(\cdot | s)} \left[ Q^\pi(s, a) \right],
    \\
    Q^\pi(s, a) &= \mathbb{E}_{ \tau \sim \pi, s_0 = s, a_0 = a }\left[ \sum_{t=0}^\infty \gamma^t  r(s_t, a_t) \right] = r(s, a) + \gamma \mathbb{E}_{s' \sim P(\cdot | s, a)} \left[ V^\pi(s') \right].
\end{aligned}
$$

Consider the visitation distribution of a policy $$\pi$$, denoted as $$d^\pi(s)$$, which represents the discounted state visitation frequency under policy $$\pi$$:

$$
    d^\pi(s) = (1 - \gamma) \sum_{t=0}^\infty \gamma^t \mathbb{E}_{\tau \sim \pi} \left[ \mathbb{P}(s_{t} = s | s_0 = s_0) \right].
$$

The value function can also be expressed in terms of the visitation distribution:

$$
    \begin{aligned}
    V^\pi(s_0) &= \frac{1}{1 - \gamma} \mathbb{E}_{s \sim d^\pi, a \sim \pi(\cdot | s)} [r(s, a)],
    \\
    Q^\pi(s, a) &= r(s, a) + \gamma \mathbb{E}_{s' \sim P(\cdot | s, a)} \left[ \frac{1}{1 - \gamma} \mathbb{E}_{s'' \sim d^\pi, a'' \sim \pi(\cdot | s'')} [r(s'', a'')] \right].
    \end{aligned}
$$

# 2. Potential Difference Lemma
Potential Difference Lemma is a fundamental result in reinforcement learning that relates the value functions of two policies. It states that for any two policies $$\red{\pi}$$ and $$\green{\pi'}$$, the difference in their value functions can be expressed as:

$$
\begin{aligned}
V^\red{\pi}(s_0) - V^{\green{\pi'}}(s_0) &= \frac{1}{1 - \gamma} \left( \mathbb{E}_{s \sim d^\red{\pi}, a \sim \red{\pi}(\cdot | s)} [r(s, a)] - \mathbb{E}_{s \sim d^{\green{\pi'}}, a \sim \green{\pi'}(\cdot | s)} [r(s, a)] \right) \\
&= \frac{1}{1 - \gamma} \mathbb{E}_{s \sim d^\red{\pi}, a \sim \red{\pi}(\cdot \mid s)} \left[ Q^{\green{\pi'}}(s, a) - V^{\green{\pi'}}(s) \right] \\
&= \frac{1}{1 - \gamma} \mathbb{E}_{s \sim d^\red{\pi}, a \sim \red{\pi}(\cdot \mid s)} \left[ A^{\green{\pi'}}(s, a) \right],
\end{aligned}
$$

where $$A^{\green{\pi'}}(s, a) = Q^{\green{\pi'}}(s, a) - V^{\green{\pi'}}(s)$$ is the advantage function of policy $$\green{\pi'}$$.
We omit the proof of the potential difference lemma, which can be found in many standard reinforcement learning textbooks and blogs (e.g., [Wen Sun's blog](https://wensun.github.io/CS4789_data/PDL.pdf)).

To understand the potential difference lemma, we can interpret it as follows: the difference in value functions between two policies can be expressed as the expected advantage of one policy over the other, weighted by the visitation distribution of the first policy.

# 3. Entropy-Regularized Setting

In the regularized reinforcement learning setting, we often consider a regularized objective function that includes an additional regularization term to encourage certain properties in the learned policy. Specifically, we consider the entropy-regularized objective function defined as:

$$
J(\pi) = \mathbb{E}_{s \sim d^\pi, a \sim \pi(\cdot | s)} [r(s, a)] + \lambda \mathbb{E}_{s \sim d^\pi} [\mathcal{H}(\pi(\cdot | s))],
$$

where $$\mathcal{H}(\pi(\cdot | s))$$ is the entropy of the policy at state $$s$$, and $$\lambda > 0$$ is the regularization coefficient.
The value function and the Q-function for the regularized objective can be defined as:

$$
\begin{aligned}
    V^\pi_{\lambda}(s) 
    &= \mathbb{E}_{ \tau \sim \pi, s_0 = s }\left[ \sum_{t=0}^\infty \gamma^t  (r(s_t, a_t) - \lambda \log \pi(a_t | s_t)) \right]
    = \mathbb{E}_{a \sim \pi(\cdot | s)} \left[ Q^\pi_{\lambda}(s, a) - \lambda \log \pi(a | s) \right]
    \\
    Q^\pi_{\lambda}(s, a)
    &= \mathbb{E}_{ \tau \sim \pi, s_0 = s, a_0 = a }\left[ \sum_{t=0}^\infty \gamma^t  (r(s_t, a_t) - \lambda \log \pi(a_t | s_t)) \right] 
    = r(s, a) + \gamma \mathbb{E}_{s' \sim P(\cdot | s, a)} \left[ V^\pi_{\lambda}(s') \right]
\end{aligned}
$$

From the above definitions, we can see that the regularized value function $$V^\pi_{\lambda}(s)$$ and Q-function $$Q^\pi_{\lambda}(s)$$ can be expressed as the sum of the unregularized value function $$V^\pi(s)$$ and Q-function $$Q^\pi_{\lambda}(s, a)$$ plus the entropy regularization term. Both regularization terms represent the expected discounted entropy of the policy along the trajectory, while the one of value function is conditioned on the initial state, while the one of Q-function is conditioned on both the initial state and action. 
This difference reflects the fundamental difference between the value function and the Q-function, which the value function is only dependent on the state and the Q-function is shifted by one step and is dependent on both the state and action.

# 4. Soft Performance Difference Lemma
The soft performance difference lemma states that for any two policies $$\red{\pi}$$ and $$\green{\pi'}$$, the difference in their soft value functions can be expressed as:

$$
\begin{aligned}
    V^\red{\pi}_{\lambda}(s) - V^{\green{\pi'}}_{\lambda}(s)
    &= 
    \frac{1}{1 - \gamma} \mathbb{E}_{s \sim d^\red{\pi}, a \sim \red{\pi}(\cdot \mid s)} \left[ 
    Q^{\green{\pi'}}_{\lambda}(s, a) - V^{\green{\pi'}}_{\lambda}(s) - \lambda \log \red{\pi}(a | s) \right] \\
    &= \frac{1}{1 - \gamma} \mathbb{E}_{s \sim d^\red{\pi}, a \sim \red{\pi}(\cdot \mid s)} \left[ A^{\green{\pi'}}_{\lambda}(s, a) + \lambda \log \frac{\green{\pi'}(a | s)}{\red{\pi}(a | s)} \right],
\end{aligned}
$$

where $$A^{\green{\pi'}}_{\lambda}(s, a) = Q^{\green{\pi'}}_{\lambda}(s, a) - V^{\green{\pi'}}_{\lambda}(s) - \lambda \log \green{\pi'}(a \mid s)$$ is the soft advantage function of policy $$\green{\pi'}$$.

## Proof of the Soft Performance Difference Lemma

The soft performance difference lemma can be considered as a comparison between two policies in terms of their expected returns. We introduce the intermediate policies $$\pi^t$$ where the first $$t$$ steps follow policy $$\pi$$ and the remaining steps follow policy $$\pi'$$. By defining the intermediate policies as follows:

$$\pi^t = (\underbrace{\red{\pi}, \red{\pi}, \cdots, \red{\pi}}_{t \text{ times}}, \underbrace{\green{\pi'}, \green{\pi'}, \cdots, \green{\pi'}}_{\text{remainding times}})$$

Based on the definition of the intermediate policies, we can see that $$\pi^0 = \green{\pi'}$$ and $$\pi^\infty = \red{\pi}$$.
Then the difference in value functions can be expressed as a telescoping sum:

$$
V^\red{\pi}_{\lambda}(s_0) - V^\green{\pi'}_{\lambda}(s_0) = \sum_{t=0}^\infty \left( V^{\pi^{t+1}}_{\lambda}(s_0) - V^{\pi^{t}}_{\lambda}(s_0) \right).
$$

Recall the definition of the value function, we can express the value functions of each intermediate policy as follows:

$$
\begin{aligned}
&\quad V^{\pi^{t}}_{\lambda}(s_0)
\\
&= \mathbb{E}_{\tau \sim \pi^{t}, s_0} \left[ \sum_{k=0}^\infty \gamma^k (r(s_k, a_k) - \lambda \log \pi^{t+1}(a_k | s_k)) \right]
\\
&= \mathbb{E}_{\tau \sim \pi^{t}, s_0} \left[ \mathbb{E}_{\tau \sim \pi^{t+1}, s_k=s_t} \left[ \sum_{k=0}^\infty \gamma^k (r(s_k, a_k) - \lambda \log \pi^{t+1}(a_k | s_k)) \mid s_1, a_t \ldots, s_t, a_t \right] \right]
\\
&= \mathbb{E}_{\tau \sim \pi^{t}, s_0} \left[ \sum_{k=0}^t \gamma^k (r(s_k, a_k) - \lambda \log \pi^{t}(a_k | s_k)) + \gamma^{t} \mathbb{E}_{\tau \sim \pi^{t}, s_{t}} \left[ \sum_{k=0}^\infty \gamma^k (r(s_{k+t}, a_{k+t}) - \lambda \log \pi^{t}(a_{k+t} | s_{k+t})) \mid s_1, a_t \ldots, s_t, a_t \right] \right]
\\
&= \mathbb{E}_{\tau \sim \pi^{t}, s_0} \left[ \sum_{k=0}^t \gamma^k (r(s_k, a_k) - \lambda \log \pi^{t}(a_k | s_k)) + \gamma^{t} V^{\pi^{t}}_{\lambda}(s_{t}) \right]
\\
&= \mathbb{E}_{\tau \sim \red{\pi}, s_0} \left[ \sum_{k=0}^t \gamma^k (r(s_k, a_k) - \lambda \log \red{\pi}(a_k | s_k)) + \gamma^{t} V^{\green{\pi'}}_{\lambda}(s_{t}) \right]
\end{aligned}
$$

The above derivation is based on decoupling the trajectory into two parts: executing policy $$\red{\pi}$$ in the first $$t$$ steps and executing policy $$\green{\pi'}$$ in the remaining steps.
To achieve this decoupling, we apply the law of total expectation to condition on the state and action at the first $$t$$ steps (second equation), and then we can use value function of $$\green{\pi'}$$ to express the expected return of the remaining steps (forth equation).
After the decoupling, we switch the expectation from the intermediate policy $$\pi^t$$ to the policy $$\red{\pi}$$, which is because the first $$t$$ steps of the intermediate policy $$\pi^t$$ are the same as policy $$\red{\pi}$$, and the remaining steps do not affect the expectation of the first $$t$$ steps (last equation).

We apply some iterative expectation 
then the difference in value functions between two consecutive intermediate policies can be expressed as:
$$
\begin{aligned}
&\quad V^{\pi^{t+1}}_{\lambda}(s_0) - V^{\pi^{t}}_{\lambda}(s_0) \\
&= \mathbb{E}_{\tau \sim \red{\pi}, s_0} \left[ \sum_{k=0}^{t+1} \gamma^k (r(s_k, a_k) - \lambda \log \red{\pi}(a_k | s_k)) + \gamma^{t+1} V^{\green{\pi'}}_{\lambda}(s_{t+1}) \right]
\\
&\quad - \mathbb{E}_{\tau \sim \red{\pi}, s_0} \left[ \sum_{k=0}^t \gamma^k (r(s_k, a_k) - \lambda \log \red{\pi}(a_k | s_k)) + \gamma^{t} V^{\green{\pi'}}_{\lambda}(s_{t}) \right]
\\
&= \mathbb{E}_{\tau \sim \red{\pi}, s_0} \left[ \gamma^t (r(s_t, a_t) - \lambda \log \red{\pi}(a_t | s_t) + \gamma V^{\green{\pi'}}_{\lambda}(s_{t+1}) - V^{\green{\pi'}}_{\lambda}(s_{t})) \right]
\end{aligned}
$$

Summing over all $$t$$, the difference in value functions becomes:

$$
\begin{aligned}
&\quad V^\red{\pi}_{\lambda}(s_0) - V^\green{\pi'}_{\lambda}(s_0) \\
&= \sum_{t=0}^\infty \mathbb{E}_{\tau \sim \red{\pi}, s_0} \left[ \gamma^t (r(s_t, a_t) - \lambda \log \red{\pi}(a_t | s_t) + \gamma V^{\green{\pi'}}_{\lambda}(s_{t+1}) - V^{\green{\pi'}}_{\lambda}(s_{t})) \right]
\\
&= \frac{1}{1 - \gamma} \mathbb{E}_{s \sim d^\red{\pi}, a \sim \red{\pi}(\cdot \mid s)} \left[ r(s, a) - \lambda \log \red{\pi}(a | s) + \gamma \mathbb{E}_{s' \sim P(\cdot | s, a)} \left[ V^{\green{\pi'}}_{\lambda}(s') \right] - V^{\green{\pi'}}_{\lambda}(s) \right]
\\
&= \frac{1}{1 - \gamma} \mathbb{E}_{s \sim d^\red{\pi}, a \sim \red{\pi}(\cdot \mid s)} \left[ Q^{\green{\pi'}}_{\lambda}(s, a) - V^{\green{\pi'}}_{\lambda}(s) - \lambda \log \red{\pi}(a | s) \right]
\\
&= \frac{1}{1 - \gamma} \mathbb{E}_{s \sim d^\red{\pi}, a \sim \red{\pi}(\cdot \mid s)} \left[ A^{\green{\pi'}}_{\lambda}(s, a) + \lambda \log \frac{\green{\pi'}(a | s)}{\red{\pi}(a | s)} \right]
\end{aligned}
$$

In the second equation, we use the definition of the visitation distribution to express the expectation over trajectories as an expectation over states and actions. In the third equation, we use the definition of the regularized Q-function to express the expected return in terms of the Q-function and value function. Finally, in the last equation, we rearrange the terms to express the difference in value functions in terms of the soft advantage function and the KL divergence between the two policies.

## Cite this page

If you would like to cite this page, you can use the following citation:

```
@misc{qin2026softperformance,
  author = {Hao Qin},
  title  = {Soft Performance Difference Lemma},
  year   = {2026},
  url    = {https://mjolnirt.github.io/note/soft-performance-difference-lemma/}
}
```