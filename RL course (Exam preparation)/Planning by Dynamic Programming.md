
### Policy Evaluation

- Want to estimate: $$V_\pi(s) = \mathbb{E}_\pi[R_{t+1} + \gamma V_\pi(S_{t+1}) ~|~ S_t=s]$$
- Can be done iteratively:
	- Initialize $V(s)$ to arbitrary values
	- Iterate $\forall s: V_{k+1}(s) \leftarrow \mathbb{E}_\pi[R_{t+1} + \gamma V_k(S_{t+1}) ~|~ S_t=s]$
	- If $\forall s: V_{k+1}(s) = V_k(s)$, then we have found the right value functions
	- Converges to true value functions under approptiate conditions
- Update in matrix form: $V_{k+1} = R^\pi + \gamma \cdot P^\pi \cdot V_k$

### Policy Improvement

- Once we estimated the value function we can improve the policy by acting greedily under this value function. The value function is guaranteed to improve. Proof:
	- Suppose we have policy $\pi$ with value function $V_\pi$, $Q_\pi$
	- Consider policy $\pi': \pi'(s) = \arg \max_{a} Q_\pi(s, a), ~\forall s$ 
	- We can show that the new value function is greater than the old one: $$\begin{align*}
		V_{\pi}(s) &\leq Q_\pi(s, \pi'(s)) = \mathbb{E}[R_{t+1} + \gamma V_\pi(S_{t+1}) ~|~ S_t = s, A_t = \pi'(s))]\\[0.5em]
		&= \mathbb{E}_{\pi'}[R_{t+1} + \gamma V_\pi(S_{t+1}) ~|~ S_t = s]\\[0.5em]
		&\leq \mathbb{E}_{\pi'}[R_{t+1} + \gamma Q_\pi(S_{t+1}, \pi'(S_{t+1})) ~|~ S_t = s]\\[0.5em]
		&= \mathbb{E}_{\pi'}[R_{t+1} + \gamma R_{t+2} + \gamma^2 V_\pi(S_{t+2}) ~|~ S_t = s]\\[0.5em]
		&\leq \dots \leq \mathbb{E}_{\pi'}[R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \dots ~|~ S_t = s]\\[0.5em]
		&= V_{\pi'}(s)
	\end{align*}$$
- Once we have the better policy, we can run policy evaluation again, then obtain a better policy, and so on

### Value iteration

- We can not wait for convergence of value function for policy evaluation, and instead do $k$ sweeps of policy evaluation
- Extreme case: we do $k=1$ sweep. This is equivalent to updating the value function as follows on each iteration: $$\hat{V}(s) \leftarrow \max_a \mathbb{E}[R_{t+1} + \gamma \hat{V}(S_{t+1}) ~|~ S_t = s]$$
- Then we can say that $\pi^*(s) = \arg \max_a \hat{Q}(s, a)$

### Asynchronous Dynamic Programming

- Instead of evaluating policy in a sweep, we can randomly select a state and modify the value
- Guaranteed to converge if all states have a chance to be selected
- Only stores one instance of value function

### Real-time Dynamic Programming

- We can use agent behaviour to select states
- Algorithm: simulate an episode. After each time step, apply the update

### Limitations

- Have to know transition probability matrix and rewards
- Have to use the values of all successor states for a single update
- If we have large number of states, this is very expensive