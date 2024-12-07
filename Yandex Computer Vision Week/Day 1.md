**Useful math tricks**
1. Marginalization: $\displaystyle p(x) = \int p(x_z)dz = \int p(x | z)p(z) dz$ 
2. Monte-Carlo Estimation: $\displaystyle \mathbb{E}_{p(x)}[f(x)] \approx \frac{1}{K}\sum_{k=1}^K f(x_k), ~x_k \sim p(x)$
3. Yensen’s Inequality
4. Chain Rule 
5. Markov Chain
6. Log-derivative trick: $\nabla_x p(x) = p(x)\nabla_x \log p(x)$
**Similarity of PDs**
1. KL divergence: $\displaystyle D_{KL}(P||Q)=\int p(x)\log \frac{p(x)}{q(x)}dx$
2. Fisher divergence: $\displaystyle D_{F}(P||Q)=\int p(x) ||\nabla_x\log p(x) - \nabla_x \log q(x)||_2^2$ (???)
# How to represent PD
- Autoregressive (chain rule)
- Normalizing flows (?)
- VAE (?)
- GAN

# KL divergence
Forward:
$$D_{KL} (p_{data}||p_{\theta}) = -H(p_{data}) + H(p_{data}, p_\theta) \to \max$$
$$\Leftrightarrow {\arg \max}_\theta (-H(p_{data}, p_\theta)) \approx Monte Carlo$$
Backward: distribution shift :(
# Latent variable models
![[Pasted image 20241128010514.png]]

# VAE
Encoder-decoder

# Diffusion models
