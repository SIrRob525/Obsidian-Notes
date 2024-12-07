### Basic setup 

- One state, $n$ possible actions
- Each action brings immediate random reward $r_{k, t} \sim P_k$
- Goal is to maximize expected immediate reward
- Action-value: $$q^*(a)=\mathbb{E}[R_t | A_t = a]$$
- Exploration/exploitation dillema
- Optimal value: $$V^* = \max_a q^*(a)$$
- Regret: $$I^*(a) = \mathbb{E}[V^* - q^*(a)]$$
- Value estimate: $$q^*(a) \approx Q_n(a) = \frac{R_1 + \dots + R_{n-1}}{n-1}$$
- Update rule: $$Q_{n+1}(a) = \frac{R_{n} + (n-1)\cdot Q_{n}(a)}{n} = Q_n(a) + \frac{1}{n}(R_{n} - Q_n(a))$$
### Non-stationary bandits

- Now action values change slowly over time
- Update rule (exponential, recency-weighted average): $$Q_{n+1}(a) = Q_n(a) + \alpha_n (R_n - Q_n(a)), ~\alpha \in (0,1]$$
- Convergence conditions: $$
\begin{align} 
\sum_{i=1}^\infty a_n = \infty\\
\sum_{i=1}^\infty a_n^2 < \infty
\end{align}
$$
- If $a_n = 1/n$ then we have the basic update rule and both conditions are met

### Methods to solve

- Fixed Exploration Period + Greedy
	- Issue: can lock into suboptimal action
	- Issue: works bad for non-stationary bandits
- $\epsilon$-Greedy (constant $\epsilon$)
	- Select random action with probability $\epsilon$
	- Issue: forever explores, which is suboptimal
-  $\epsilon$-Greedy (decaying $\epsilon$)
	- Select random action with probability $\epsilon_n \to 0$
	- Better balance
- Upper confidence bound
	- Idea: the less we explored an action, the more value it can potentially have → we estimte upper confidence bound (from Hoeffding’s Inequality)$$q^*(a) \leq Q_t(a) + U_t(a)$$ $$U_t(a) = c\cdot \sqrt{\frac{\log t}{N_t(a)}}$$
	- Then select an action with highets upper bound
- Bayesian Approach
	- Idea: have a model with parameters $\theta \sim P(\theta)$ (prior). Update the distribution as we observe the data: $$P(\theta | D) = \frac{P(D | \theta) \cdot P(\theta)}{P(D)}$$
	- In case of bandits, we model reward distribution $P(R | H_t)$. For example, we can use Beta-distribution $R_k \sim P(a_k, b_k)$. As we observe the rewards, we update either $a$ or $b$. Then sample actions according to the maximum expected value.
