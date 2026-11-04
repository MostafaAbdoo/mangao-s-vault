- **Unit Sample (or Unit Impulse)**
$$\delta[n] = \begin{cases} 1, & n = 0 \\ 0, & n \neq 0 \end{cases}$$
- **Unit Step (or Heaviside Step Function)**
$$u[n] = \begin{cases} 1, & n \ge 0 \\ 0, & n < 0 \end{cases}$$
- **Unit Ramp**
$$r[n] = \begin{cases} n, & n \ge 0 \\ 0, & n < 0 \end{cases}$$
**To remember**
- $\sin(\pi - \theta) = \sin\theta$
- $\cos(\pi - \theta) = -\cos\theta$
- $\tan(\pi - \theta) = -\tan\theta$
- $\sin(2\pi - \theta) = -\sin\theta$
- $\cos(2\pi - \theta) = \cos\theta$
- $\tan(2\pi - \theta) = -\tan\theta$
- $\sin^2 x + \cos^2 x = 1$
- $\sin^2 x = \frac{1 - \cos(2x)}{2}$
- $\cos^2 x = \frac{1 + \cos(2x)}{2}$
- $\sin(2x) = 2\sin x \cos x$
![[Pasted image 20261030195305.png]]
![[Pasted image 20261030200214.png]]

$$F_s = \frac{1}{T_s}$$
$$|\Delta_{error}| \le \frac{\Delta}{2}$$
$$y(t) = A \sin(2\pi F t + \theta) + D$$
$$x(t) = A \cos(\omega t + \theta)$$
$$f = \frac{F}{F_s}$$
$$\omega_0 = 2\pi f = \frac{2\pi F}{F_s}$$
**Discrete Angular Frequency**: $\omega_0$

$$F_s \ge 2 \cdot F_{\max}$$
$$F_{\text{fold}} = \frac{F_s}{2}$$
![[Pasted image 20261030210519.png]]
![[Pasted image 20261030211825.png]]
![[Pasted image 20261031064808.png]]
![[Pasted image 20261031064947.png]]
![[Pasted image 20261031072703.png]]
![[Pasted image 20261031072814.png]]
![[Pasted image 20261031073218.png]]
![[Pasted image 20261031074114.png]]
for continues it's always periodic

---
- **Static (Memoryless) System:** The output at any given time depends _only_ on the input at that exact same time instant (e.g., $y[n] = 2x[n]$). It has no memory of past inputs or anticipation of future ones.
- **Dynamic (Memory) System:** The output depends on past inputs, current inputs, and/or future inputs (e.g., $y[n] = x[n] + x[n-1]$). It requires memory elements (like delay units) to store past values.
- **Causal System:** The output at the current time depends _only_ on the present and/or past inputs. It never depends on future inputs
- **Non-Causal System:** The output depends on future input values
- **Time-Invariant System:** A time shift in the input signal results in an identical time shift in the output signal. If an input $x[n]$ produces $y[n]$, then a delayed input $x[n-k]$ will produce the exact same output delayed by $k$, i.e., $y[n-k]$. The system's characteristics do not change over time
- **Time-Variant System:** The system's behavior or parameters change with time. A delay in the input does _not_ result in a simple identical delay in the output (e.g., $y[n] = n \cdot x[n]$).
- **Linear System:** Satisfies both the **superposition principle** (additivity) and the **scaling property** (homogeneity). If input $x_1[n]$ gives output $y_1[n]$ and $x_2[n]$ gives $y_2[n]$, then a linear combination of inputs $a x_1[n] + b x_2[n]$ will produce the exact same combination of outputs $a y_1[n] + b y_2[n]$.
- **Nonlinear System:** Does not obey the superposition or scaling rules (e.g., systems involving squaring $y[n] = x^2[n]$, products, or constants like $y[n] = x[n] + 5$). Doubling the input does not necessarily double the output.


any scaling is time variant
Nonlinear System (due to a constant offset)
![[Pasted image 20261031080459.png]]
![[Pasted image 20261031080710.png]]
