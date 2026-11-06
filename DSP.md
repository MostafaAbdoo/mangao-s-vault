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
- $\sin(\theta + 2\pi) = +\sin \theta$
- $\cos(\theta + 2\pi) = +\cos \theta$
- $e^{ix} = \cos x + i \sin x$
- $e^{-ix} = \cos(-x) + i \sin(-x) = \cos x - i \sin x$
- $\cos x = \operatorname{Re}\{e^{ix}\} = \frac{e^{ix} + e^{-ix}}{2}$
- $\sin x = \operatorname{Im}\{e^{ix}\} = \frac{e^{ix} - e^{-ix}}{2i}$
- $\cos(A)\sin(B) = \frac{1}{2} \left[ \sin(B + A) + \sin(B - A) \right]$
![[Pasted image 20261030195305.png]]
![[Pasted image 20261030200214.png]]

Scaling:- 
Case 1: If a > 1, the signal z(t) is a compressed version of x(t).
Case 2: If a < 1, the signal z(t) is an expanded (stretched) version of x(t).

• **Sampling** is used to convert a continuous-time signal into a discrete-time signal. 
• **Quantization** is used to convert a discrete-time signal into a digital signal.
The quantization errors can be reduced by increasing k (number of bits, or
levels)
$$F_s = \frac{1}{T_s}$$
$$|\Delta_{error}| \le \frac{\Delta}{2}$$
$$y(t) = A \sin(2\pi F t + \theta) + D$$
$$x(t) = A \cos(\omega t + \theta)$$
$$f = \frac{F}{F_s}$$
Normalized Frequency only in discrete signals
$$\omega_0 = 2\pi f = \frac{2\pi F}{F_s}$$
**Discrete Angular Frequency**: $\omega_0$

$$F_s \ge 2 \cdot F_{\max}$$
$$F_{\text{fold}} = \frac{F_s}{2}$$
Any analog signal has to be a periodic signal
![[Pasted image 20261030210519.png]]
![[Pasted image 20261030211825.png]]

![[Pasted image 20261031064947.png]]
![[Pasted image 20261031072703.png]]
![[Pasted image 20261031072814.png]]
![[Pasted image 20261031073218.png]]
![[Pasted image 20261031074114.png]]
for continues it's always periodic

---
## Direct Method
𝑘𝑚𝑖𝑛 = 𝑥𝑚𝑖𝑛 + ℎ𝑚𝑖𝑛 , 𝑘𝑚𝑎𝑥 = 𝑥𝑚𝑎𝑥 + ℎ𝑚𝑎𝑥 
$y[n] = \sum_{k=-\infty}^{\infty} x[k] \cdot h[n - k]$                  n -> 𝑘𝑚𝑖𝑛 to 𝑘𝑚𝑎𝑥
## Table Method
![[Pasted image 20261106191151.png]]
## Array Method
![[Pasted image 20261106191213.png]]


---
𝑐𝑜𝑚𝑝𝑙𝑒𝑥 𝑛𝑢𝑚𝑏𝑒𝑟 = 𝑧 = cos 𝜃 + 𝑖 sin 𝜃, Exponential Signals are complex
𝑎𝑠𝑠𝑢𝑚𝑖𝑛𝑔 𝑟 = 1
 note: 𝑑/𝑑𝑥 𝑒2𝑥 = $2𝑒^2𝑥$   
 ![[Pasted image 20261106200552.png]]
 ![[Pasted image 20261106205828.png]]