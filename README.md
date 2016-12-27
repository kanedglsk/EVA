---
title: "Extreme Value Analysis"
author: "Shikun Li"
output:
  html_document:
    number_sections: yes
    toc: true
    theme: "sandstone"
---
How do we predict rare events of extreme values, such as floods(high rainfall), tsunami(high sea level) and anomalous traffic, when there are very few such observations? What do 100-year floor, 50-year wave and  200-day outbreak mean? 
Successful predictions of these rare events is important to prevent loss.

Simplest case: $x_1, x_2, x_3, ... x_n \stackrel{i.i.d.}\sim F$. Require accurate inference on the tail of $F$.


# Block maxima
## Definition
$X_1, X_2, X_3, ... X_n \stackrel{i.i.d.}\sim F$ and define:
$$M_n=max\{X_1, X_2, X_3, ... X_n\}$$
Then the distribution of $M_n$ is
$$Pr\{M_n < z\} = (F(z))^n$$

## Fisher–Tippett–Gnedenko theorem
If there exist sequences of constants $a_n > 0$ and $b_n \in \mathbb{R}$ such that, as $n \rightarrow \infty$,
$$Pr\{(M_n - b_n)/a_n \leq z \} \rightarrow G(z)$$
then
$$G(z) \propto exp[-(1+\xi z)^{-1/\xi}]$$
For some non-degenerate distribution, $G$ belongs to one of the following:

Gumbel:
$$\begin{equation} G(z)= exp\{-exp(-(\frac{z-b}{a}))\}
    \end{equation}, z \in \mathbb{R}$$

Weibull:
$$\begin{equation} G(z)=
    \begin{cases}
      exp\{-exp(-(\frac{z-b}{a}))\} & z < b \\
      1 & z  \geq b
    \end{cases}
    \end{equation}$$
    
Frechet:
$$\begin{equation} G(z)=
    \begin{cases}
      0 & z \leq b \\
      exp\{-(\frac{z-b}{a})^{-a}\} & z > b
    \end{cases}
    \end{equation}$$




*Illustrating the theorm by simulation:*
```{r, message=FALSE}
library(extRemes)
## block size
n <- 10
original_mean <- 5
original_sd <- 2

## Create a series of maxima
series_length <- 1000
maxima <- c()
for (i in 1:series_length) {
  maxima <- c(maxima, max(rnorm(n = n, mean = original_mean, sd = original_sd)))
}
plot(maxima, main = "Simulated maxima of normal variables", type = "l")
fit <- fevd(maxima, type = "Gumbel")
plot(fit, type = "density", main = "Empirical density vs estimated Gumbel distribution")
```

## Quantiles and return levels
In terms of quantiles, take $0 < p < 1$ and define
$$z_p = \mu - \frac{\sigma}{\xi}[1 - \{-log(1-p)\}^{-\xi}]$$
where $G(z_p) = 1 - p$.

In extreme value terminology, $z_p$ is the return level associated with the return period $1/p$.

For annual maxima of rainfall, $z_p$ is the amount of annual maximum rainfall with probability of occurrence $1-p$ in any given year, and $1/p$ is the recurrence interval in years.

# Peaks over thresholds
