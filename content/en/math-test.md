---
date: "2026-05-14"
type: "post"
math: true
title: "Math test"
categories:
  - "Notes 筆記"
---

# Derivatives and Differences
| $f(x)$ | $f'(x)$ | $\Delta F(x)$ |
| -- | -- | -- |
| constant $k$ | 0 | 0 |
| $x^n$ | $nx^{n-1}$ |  |
| $e^x$ | $e^x$ | $e^x(e^h-1)$ |
| $\ln x$ | $\frac{1}{x}, x > 0$ | $\ln(1+\frac{h}{x})$ |
| $a^x (a > 0)$ | $a^x\ln a$ | $a^x(a^h-1)$ |
| $a^{bx} (a > 0)$ | $ba^{bx}\ln a$ | $a^{bx}(a^{bh}-1)$ |
| $\log_a (bx) (a, b > 0, a \ne 1)$ | $\frac{1}{x\ln a}$ | $\log_a(1+\frac{h}{x})$ |

| $f(x)$ | $f'(x)$ | $\Delta F(x)$ |
| -- | -- | -- |
| $\sin x$ | $\cos x$ | $2\sin(\frac{ah}{2})\cos(a(x+\frac{h}{2}))$ |
| $\cos x$ | $-\sin x$ | $-2\sin(\frac{ah}{2})\sin(a(x+\frac{h}{2}))$ |
| $\tan x$ | $\sec^2 x$ |  |
| $\cot x$ | $-\csc^2 x$ |  |
| $\sec x$ | $\sec x\tan x$ |  |
| $\csc x$ | $-\csc x\cot x$ |  |

| $f(x)$ | $f'(x)$ |
| -- | -- |
| $\sin^{-1}x$ | $\frac{1}{\sqrt{1-x^2}}$ |
| $\cos^{-1}x$ | $-\frac{1}{\sqrt{1-x^2}}$ |
| $\tan^{-1}x$ | $\frac{1}{1+x^2}$ |
| $\cot^{-1}x$ | $-\frac{1}{1+x^2}$ |
| $\sec^{-1}x$ | $\frac{1}{\|x\|\sqrt{x^2-1}}$ |
| $\csc^{-1}x$ | $-\frac{1}{\|x\|\sqrt{x^2-1}}$ |

| $f(x)$ | $f'(x)$ | $\Delta F(x)$ |
| -- | -- | -- |
| $\sinh x$ | $\cosh x$ | $2\sinh(\frac{ah}{2})\cosh(a(x+\frac{h}{2}))$ |
| $\cosh x$ | $\sinh x$ | $2\sinh(\frac{ah}{2})\sinh(a(x+\frac{h}{2}))$ |
| $\tanh x$ | $\mathrm{sech}^2 x$ |  |
| $\coth x$ | $-\mathrm{csch}^2 x$ |  |
| $\mathrm{sech} x$ | $-\mathrm{sech} x\tanh x$ |  |
| $\mathrm{csch} x$ | $-\mathrm{csch} x\coth x$ |  |

# Antiderivatives
| $f(x)$ | $F(x)$ |
| -- | -- |
| constant $k$ | $kx+C$ |
| $x^n$ | $\frac{x^{n+1}}{n+1}+C, n \ne -1$ |
| $\frac{1}{x}$ | $\ln\|x\|+C$ |  |
| $e^x$ | $e^x+C$ |  |
| $\ln x$ | $x\ln x-x+C$ |  |
| $a^x$ | $\frac{a^x}{\ln a}+C$ |
| $\log_a x$ | $\ln a(x\ln x-x)+C$ |  |

| $f(x)$ | $F(x)$ |
| -- | -- |
| $\sin x$ | $-\cos x+C$ |
| $\sin^2 x$ | $\frac{1}{2}-\frac{1}{4}\sin 2x+C$ |
| $\cos x$ | $\sin x+C$ |
| $\cos^2 x$ | $\frac{1}{2}+\frac{1}{4}\sin 2x+C$ |
| $\tan x$ | $\ln\|\sec x\|+C$ |
| $\cot x$ | $\ln\|\sin x\|+C$ |
| $\sec x$ | $\ln\|\sec x + \tan x\|+C$ |
| $\sec^2 x$ | $\tan x+C$ |
| $\sec^3 x$ | $\frac{1}{2}(\sec x\tan x + \ln\|\sec x + \tan x\|)+C$ |
| $\csc x$ | $\ln\|\csc x - \cot x\|+C$ |
| $\csc^2 x$ | $-\cot x+C$ |
| $\csc^3 x$ | $\frac{1}{2}(-\csc x\cot x + \ln\|\csc x - \cot x\|)+C$ |

| $f(x)$ | $F(x)$ |
| -- | -- |
| $\sin^{-1}x$ | $x\sin^{-1}x+\sqrt{1-x^2}+C$ |
| $\cos^{-1}x$ | $x\cos^{-1}x-\sqrt{1-x^2}+C$ |
| $\tan^{-1}x$ | $x\tan^{-1}x-\frac{1}{2}\ln\|1+x^2\|+C$ |
| $\cot^{-1}x$ | $x\cot^{-1}x+\frac{1}{2}\ln\|1+x^2\|+C$ |

| $f(x)$ | $F(x)$ |
| -- | -- |
| $\sinh x$ | $\cosh x+C$ |
| $\cosh x$ | $\sinh x+C$ |
| $\tanh x$ | $\ln\cosh x+C$ |
| $\mathrm{sech}^2 x$ | $\tanh x+C$ |
| $\mathrm{csch}^2 x$ | $-\coth x+C$ |
| $\mathrm{sech} x\tanh x$ | $-\mathrm{sech} x+C$ |
| $\mathrm{csch} x\coth x$ | $-\mathrm{csch} x+C$ |

Define $n^{\underline k} = \frac{n!}{(n-k)!} = n(n-1)\cdots(n-k+1)$.
| $f(x)$ | $F(x)$ |
| -- | -- |
| $e^{a x}\sin bx$ | $\frac{e^{a x}}{a^2+b^2}\left(a\sin bx-b\cos bx\right)+C$ |
| $e^{a x}\cos bx$ | $\frac{e^{a x}}{a^2+b^2}\left(a\cos bx+b\sin bx\right)+C$ |
| $x^n \sin ax$ | $\sum_{k=1}^{n+1}\left(\frac{n^{\underline k-1}x^{n+1-k}}{a^k} \sin\left(x+\frac{\pi}{2}(k-1)\right)\right)$ |
| $x^n \cos ax$ | $\sum_{k=1}^{n+1}\left(\frac{n^{\underline k-1}x^{n+1-k}}{a^k} \sin\left(x+\frac{\pi}{2}(k-1)\right)\right)$ |

# Transforms
| $f(t)$ | $\mathcal{F}\{f(t)\}=F(\omega)$ | $\mathcal{L}\{\}=F(s)$ | $Z\{[x]\}=X[z]$ |
| -- | -- | -- | -- |
| $f(at)$ | $\frac{1}{\|a\|}F(\frac{\omega}{a})$ |  |  |
| $f(t-T)$ | $e^{-i\omega T}F(\omega)$ | $e^{-sT}F(s)$ | $z^{-T}X[z]$ |
| $e^{at}x(t)$ | $F(\omega-a)$ | $F(s-a)$ |  |
| constant $k$ | $2k\pi\delta(\omega)$ | $\frac{k}{s}$ | $\frac{kz}{z-1}$ |
| $t$ |  | $\frac{1}{s^2}$ |  |
| $t^n$ |  | $\frac{n!}{s^{n+1}}$ |  |
| $\delta(t)$ | 1 | 1 | 1 |
| $u(t)$ | $\pi\delta(\omega)+\frac{1}{i\omega}$ | $\frac{1}{s}$ | $\frac{z}{z-1}$ |
| $e^{-at}u(t)$ | $\frac{1}{a+i\omega}$ |  |  |
| $te^{-at}u(t)$ | $\frac{1}{(a+i\omega)^2}$ |  |  |
| $\mathrm{rect}(\frac{t}{\tau})$ | $\tau\mathrm{sinc}(\frac{\omega\tau}{2}) = \frac{2}{\omega}\sin(\frac{\omega\tau}{2})$ |  |  |
| $\sin\omega t$ | $\frac{\pi}{i}(\delta(\omega-\omega_0)-\delta(\omega+\omega_0))$ | $\frac{\omega}{s^2+\omega^2}$ | $\frac{z\sin\omega t}{z^2-2z\cos\omega t+1}$ |
| $\cos\omega t$ | $\pi(\delta(\omega-\omega_0)+\delta(\omega+\omega_0))$ | $\frac{s}{s^2+\omega^2}$ | $\frac{z^2-z\cos\omega t}{z^2-2z\cos\omega t+1}$ |
