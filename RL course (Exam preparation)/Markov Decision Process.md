
### Markov Process

- Some distribution of states which satisfies $$P(S_t | H_t) = P(S_t | S_{t-1})$$which means that the state captures all relevant information from the history and is a sufficient statistic of the future.
- Markov process is defined by a finite set of states $\mathcal{S}$ and transition probability matrix $\mathcal{P}: \mathcal{P}_{ss'} = P(s' | s)$

### Markov Reward Process

- Additionally, we define a reward function $R(s)$ and discount factor $\gamma \in [0,1]$
- Return: $$G_t = \sum_{k=0}^\infty \gamma^k R_{t+k+1}$$where $R_i = R(S_i)$ and $S_i$ satisfy the Markov property.
- Value function: $$V(s) = \mathbb{E}[G_t | S_t = s]$$
- Bellman equation: $$V(s) = R_t + \gamma \cdot \mathbb{E}[V(S_{t+1}) ~|~ S_t = s]$$
- Bellman equation (matrix form): $$V = R + \gamma \cdot P \cdot V$$
- This is a linear equation → we can solve with $O(N^3)$ complexity.

### Markov Desicion Process

- Additionaly, we define finite set of actions $\mathcal{A}$
- $\mathcal{P}$ is now defined as $\mathcal{P}_{s,a,s'} = P(s' ~|~ S_t=s, A_t = a)$. Reward function is also based on action: $R(s, a)$
- Policy is a distribution of actions over states: $$\pi(a | s) = P(A_t=a | S_t = s)$$
- Value function: $$V_\pi(s) = \mathbb{E}_\pi [G_t ~|~ S_t = s]$$
- Action-value function: $$Q_\pi(s, a) = \mathbb{E}_\pi[G_t ~|~ S_t=s, A_t = a]$$
- Relation: $$V_\pi(s) = \sum_{a} \pi(a ~|~ s) \cdot Q_\pi(s, a)$$ $$Q_\pi(s, a) = R(s, a) + \gamma \sum_{s'} P(s' ~|~ s, a) \cdot V_\pi(s')$$
- Bellman equation: $$V_\pi(s) = \mathbb{E}_\pi[R_t + \gamma V_\pi(S_{t+1}) ~|~ S_t = s, A_t = a]$$$$Q_\pi(s, a) = R(s, a) + \gamma \mathbb{E}_\pi[Q_\pi(s', a') ~|~ S_t = s, A_t = a]$$
- Can also be solved in matrix form

### Optimal Policies and Optimal Value Functions

- Optimal value function: $$V^*(s) = \max_\pi V_\pi(s)$$
- Optimal action-value function: $$Q^*(s, a) = \max_\pi Q_\pi(s, a)$$
- Comparing policies: $$\pi' \geq \pi \Leftrightarrow V_{\pi'}(s) \geq V_{\pi}(s) ~\forall s $$
- Optimal policy always exists and achieves optimal value and action-value function in all states
- Optimal policy can be found by solving Bellman equations: $$V_\pi(s) = \max_a\left[R_t + \gamma V_\pi(S_{t+1}) ~|~ S_t = s, A_t = a\right]$$$$Q_\pi(s, a) = R(s, a) + \gamma \max_{a'}\left[Q_\pi(s', a') ~|~ S_t = s, A_t = a\right]$$
