
### Policy Based RL

- Instead of approximating the value function, we directly optimize the policy: $$V_\theta(s) \approx V_\pi(s, \theta) \to \pi_\theta(s, a) = P(a ~|~ s, \theta)$$
- Learning only the policy without the value function
- Advantages:
	- True objective
	- Simpler
	- Better convergence
	- Effective in contiunuous action spaces
	- Can learn stochastic policies
- Disadvantages:
	- Converge to local optimum
	- May be inefficient

### Stochastic Policies

- Better for not fully observable MDPs
- Can use gradients
- The policy is then a probability distribution $\pi(a ~|~ s, \theta)$  
	- Deterministic continuous policy: $\pi_\theta(s) \to a$
	- Stochastic continuous policy: $\pi_\theta(s) \to P(\mathbf{d})$, $a \sim P(\mathbf{d})$, for example $\mathbf{d} = (\mu, \sigma)$, $P(\mathbf{d}) = \mathcal{N}(\mu, \sigma^2)$ 
	- Stochastic discrete policy: $\pi_\theta(s) \to P$, $a \sim P$

### Policy Gradient

- Parametrized policy: $\pi(a ~|~ s, \theta)$
- Approximate value function: $$V(s, \theta) = \sum_a \pi(a ~|~ s, \theta) \cdot Q_\pi(s,a)$$
- For observed state $s$ update parameters with gradient ascent: $$\theta \leftarrow \theta + \beta \cdot \frac{\partial V(s, \theta)}{\partial \theta}$$
- Tricky because we do not know $Q_\pi(s, a)$ and it does depend on $\theta$. We can calculate the gradient and pretend that the action-value function is independent of $\theta$ $$
	\begin{align*}
		\frac{\partial V(s, \theta)}{\partial \theta} &= \frac{\partial}{\partial\theta} \sum_a \pi(a ~|~ s, \theta) \cdot Q_\pi(s,a)\\[0.5em]
		&= \sum_a \frac{\partial}{\partial\theta} \pi(a ~|~ s, \theta) \cdot Q_\pi(s,a)\\[0.5em]
		&= \sum_a \pi(a ~|~ s, \theta) \frac{\partial \log \pi(a ~|~ s, \theta)}{\partial\theta}  \cdot Q_\pi(s,a)\\[0.5em]
		&= \mathbb{E}_{a \sim \pi(~\cdot ~|~s, \theta)}\left[\frac{\partial \log \pi(a ~|~ s, \theta)}{\partial\theta}  \cdot Q_\pi(s,a)\right]
	\end{align*}$$
- Therefore, $g(a, \theta) = \frac{\partial \log \pi(a ~|~ s, \theta)}{\partial\theta}  \cdot Q_\pi(s,a)$, where $a\sim\pi(~\cdot ~|~ s, \theta)$, is an unbiased estimate of policy gradient
- Algorithm:
	- Observe state $s_t$
	- Sample action from policy: $a_t\sim\pi(~\cdot ~|~ s_t, \theta)$
	- Approximate $Q_\pi(s_t, a_t)$
	- Calculate gradient $d_{\theta,t} = \frac{\partial \log \pi(a_t ~|~ s_t, \theta)}{\partial\theta}$ 
	- Calculate $g(a_t, \theta_t) = d_{\theta,t} \cdot Q_\pi(s_t,a_t)$
	- Update policy network: $\theta_{t+1} \leftarrow \theta_t +\beta \cdot g(a_t, \theta_t)$

### How to Approximate $Q(s_t, a_t)$

- Option 1: REINFORCE $$Q_\pi(s_t, a_t) = \mathbb{E}_{\tau \sim \pi}[G(\tau)]$$sample trajectories starting from $(s_t, a_t)$ following policy $\pi$ and calculate the return
- Option 2: Actor-Critic

### REINFORCE

- Repeat the following:
	- Sample $N$ trajectories $\tau_i \sim \pi_\theta$
	- Approximate gradient: $$\nabla_\theta J(\theta) \approx \frac{1}{N} \sum_{i=1}^N \left(\sum_{t=1}^T \nabla_\theta \log \pi_\theta(a_t ~|~ s_t)\right) \left(\sum_{t=1}^T R(s_{i, t}, a_{i,t})\right)$$
	- Update policy: $\theta \leftarrow \theta + \beta \cdot \nabla_\theta J(\theta)$
- Logic: increase probability of good episodes, decrease probability of bad episodes
- Alternatively, we can use the following update: $$\nabla_\theta J(\theta) \approx \frac{1}{N} \sum_{i=1}^N \left(\sum_{t=1}^T \nabla_\theta \log \pi_\theta(a_t ~|~ s_t)\sum_{t'=t}^T R(s_{i, t'}, a_{i,t'})\right)$$
- Problem: gradient depends on the magnitude and sign of the reward, which causes high variance
	- Solution: changing the values by something independent of action does not change the gradient (baseline)
	- We can use $(Q_\pi(s, a) - b)$ in the formula for $g(a, \theta)$ and this will not change the optimization problem
	- $b = 0$ – equivalent to ordinary algorithm
	- $b = V_\pi(s_t) = \sum_a \pi(a | s, \theta) \cdot Q_\pi(s, a)$ – advantage
	- $b = \displaystyle \frac{\mathbb{E}_{\tau \sim \pi}[g(\tau)^2 r(\tau)]}{\mathbb{E}_{\tau \sim \pi}[g(\tau)^2]}$ – minimal variance
- Problem: cannot reuse older episodes