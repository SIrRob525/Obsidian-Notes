
### Monte-Carlo Learning

- Goal: learn value function $V_\pi$ from episodes under policy $\pi$
- Idea: sample many episodes. Calculate the value function in state $s$ as the average of returns after visiting state $s$.
	- First-visit: only consider the first visit to $s$ from each episode
	- Every-visit: consider all visits to $s$ from each episode
- Update rule: $$V(S_t) \leftarrow V(S_t) + \alpha (G_t - V(S_t))$$
- Converges asymptotically
- Limitations:
	- All episodes must terminate
	- All states must be visited
		- Exploring starts: initialize starting state and action randomly with non-zero probability for each pair
	- High variance

### Temporal Difference

- Idea: initialize the value function arbitrarily and update using Bellman equation with observed rewards $$V(S_t) \leftarrow V(S_t) + \alpha (R_{t+1} + \gamma V_{t+1}(S_{t+1}) - V(S_t))$$
- Advantages: 
	- Can learn from incomplete or infinite episodes
	- Low variance
- Limitations:
	- All states must be visited
	- Bias from initial values is possible

### MC vs TD

- MC has high variance, TD has low variance
- MC has no bias, TD may have some bias
- MC learns only from complete episodes, TD can learn from incomplete episodes
- TD exploits Markov property → more efficient in Markov environments
- MC does not exploit Markov property → more efficient in non-Markov environments

### TD($\lambda$)

- Use $n$-step return for update: $$G^{(n)}_t = R_{t+1} + \gamma R_{t+2} + \dots + \gamma^{n-1} R_{t+n} + \gamma^n V(S_{t+n})$$$$V(S_t) \leftarrow V(S_t) + \alpha (G^{(n)}_{t} - V(S_t))$$
- MC is TD for $n = \infty$
- TD for $n > 1$ may perform better than both TD and MC
- We can also use $\lambda$-return, defined as $$G^\lambda_t = (1-\lambda)\sum_{n=1}^\infty \lambda^{n-1} G_t^{(n)}$$which we call TD($\lambda$) algorithm