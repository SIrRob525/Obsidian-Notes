### Generalized Policy Iteration

- Policy evaluation: any algorithm (MC, TD)
- Policy improvement: greedy
	- If we use state-values, greedy improvement requires knowledge of the MDP (rewards and transition probabilities)$$\pi'(s) \leftarrow \arg \max_a R(s, a) + \gamma  \sum_{s'} P(s' ~|~ s, a) V(s')$$
	- If we use action-values, we do not have this problem $$\pi'(s) \leftarrow \arg \max_a Q(s, a)$$
- Problems:
	- Exploring starts is difficult
	- Policy evaluation requires infinite number of episodes
		- Solution: we do not have to wait for convergence of policy evaluation (similar to value iteration)
	- Once we use a determenistic greedy policy, we lose the oportunity to explore other states: exploration-exploitation dillema

### Monte Carlo with 𝜖-Greedy Exploration

- Instead of a greedy policy, we can use $\epsilon$-greedy with respect to the previous policy $\pi$
- This is an on-policy algorithm, since we estimate the value with the same policy we try to optimize
- Converges to the optimal greedy policy if $\epsilon_k = 1/k \to 0$ (Greedy in the Limit with Infinite Exploration)

### Off-policy methods

- Use behavioral policy $\mu$ to generate episodes and evaluate the target policy $\pi$
- Advantages:
	- Can learn from observation (e.g. $\pi_b$ is expert policy)
	- Can reuse experience from old policies
	- Can learn about greedy policy while following exploratory policy

### Off-policy MC

- Its nessesary to use importance sampling when calculating returns to evaluate the target policy, because the trajectories were sampled from a different distribution $$\mathbb{E}_{x \sim P} [f(x)] = \int f(x) P(x) dx = \int f(x) \frac{P(x)}{Q(x)} Q(x) dx = \mathbb{E}_{x \sim Q}\left[ \frac{P(x)}{Q(x)} f(x)\right]$$$$\mathbb{E}_{\tau \sim \pi}[G(\tau)] = \mathbb{E}_{\tau \sim \mu}\left[ \frac{\pi(\tau)}{\mu(\tau)} G(\tau)\right] \approx \frac{1}{n} \sum_i \frac{\pi(\tau_i)}{\mu(\tau_i)} G(\tau_i)$$
- Importance sampling ratio: $$\rho_t^{T-1} = \prod_{k=t}^{T-1}\frac{\pi(A_k ~|~ S_k)}{\mu(A_k ~|~ S_k)}$$
- Estimate: $$V(s) = \frac{\sum_{t\in\mathcal{T}(s)}\rho_t^{T(t)}G_t}{|\mathcal{T}(s)|}$$where $\mathcal{T}(s)$ is a set of time steps in which $s$ was visited (different for first-visit and every-visit methods), $T(t)$ is the next terminal state after time-step $t$
- The variance of ordinary importance-sampling ratio is high (since the denominator can be very small)
- Weighted importance sampling: $$V(s) = \frac{\sum_{t\in\mathcal{T}(s)}\rho_t^{T(t)}G_t}{\sum_{t\in\mathcal{T}(s)}\rho_t^{T(t)}}$$
- This avoids high variance problem but is biased since $\mathbb{E}[V(s)] = V_\mu(s)$ instead of $V_\pi(s)$ 

### SARSA: On-Policy TD

- On-policy TD with $\epsilon$-greedy policy improvement
- Policy evaluation: $Q \approx Q_\pi$ through updates $$Q(s, a) \leftarrow Q(s, a) + \alpha(R(s, a) + \gamma Q(s', a') - Q(s, a))$$
- Policy improvement: $\epsilon$-greedy
- Converges to optimal policy under the following conditions: 
	- GLIE ($\epsilon_k \to 0$)
	- Robbins-Monro: $\sum_{t=1}^\infty \alpha_t = \infty$, $\sum_{t=1}^\infty \alpha_t^2 < \infty$ 
- Can also do $n$-step SARSA and $\lambda$-SARSA

### Q-leaning: Off-Policy TD

- Possible approach: use behavioral policy $\mu$ and weight TD target by importance sampling ratio: $$V(S_t) \leftarrow V(S_t) + \alpha \left( \frac{\pi(A_t ~|~ S_t)}{\mu(A_t ~|~ S_t)} (R_{t+1} + \gamma V(S_{t+1})) - V(S_t)\right)$$
- Q-learning uses action-values and therefore does not use importance sampling
- Algorithm: 
	- Sample action using behavioral policy $A_t \sim \mu(\cdot ~|~ S_t)$
	- Take action $A_t$, observe reward $R_{t+1}$ and next state $S_{t+1}$ 
	- Sample action $A’ \sim \pi(\cdot ~|~ S_{t+1})$ 
	- Update: $$Q(S_t, A_t) \leftarrow Q(S_t, A_t) + \alpha(R_{t+1} + \gamma Q(S_{t+1}, A') - Q(S_t, A_t))$$
	- Alternatively: $$Q(S_t, A_t) \leftarrow Q(S_t, A_t) + \alpha(R_{t+1} + \gamma \max_a Q(S_{t+1}, a) - Q(S_t, A_t))$$
### Expected SARSA

- Instead of a single next state, we can use the expectation over states in update: $$Q(s, a) \leftarrow Q(s, a) + \alpha(R(s, a) + \gamma \mathbb{E}_{a' \sim \pi} [Q(s', a')] - Q(s, a))$$
- Performs better (since uses information about all possible actions) but costs more (since we have to evaluate the Q-funtion more times)
- If $\pi$ is determenistic greedy policy, this is equivalent to Q-learning

### Q-learning Variants

- Ordinary Q-learning suffers from maximization bias, since $\mathbb{E}[\max_i \mu_i] \geq \max_i \mathbb{E}[\mu_i]$. Therefore, high Q-values may inflate infinitely
- Solution: Double Q-learning:
	- Train two value functions $Q_1$, $Q_2$
	- On every step, select one value function randomly, sample best next action from it, but use the other value function in update. For example: $$Q_1(S_t, A_t) \leftarrow Q_1(S_t, A_t) + \alpha(R_{t+1} + \gamma Q_2(S_{t+1}, \arg \max_a Q_1(S_{t+1}, a))- Q_1(S_t, A_t))$$
- This helps avoid maximization bias

