---
author: Furkan Bostancı
pubDatetime: 2021-07-19T12:15:00Z
modDatetime: 2021-07-19T12:15:00Z
title: Legendre Polinomlarının Ortogonalite İntegrali
featured: false
draft: false
tags:
  - Matematik
  - Elektromanyetik Teori
  - Legendre Polinomları
description: >
  Legendre polinomlarının ortogonalite integralini türev formüllerini kullanarak adım adım hesaplıyoruz.
---

Legendre polinomlarının türev formüllerini kullanarak aşağıdaki integrali hesaplayınız:

$$
\int_{-1}^1 \left[P_L^m(x)\right]^2 dx
$$

Burada $P_L^m(x)$, asosiye Legendre polinomlarını temsil etmektedir.

---

## Çözüm

Öncelikle $P_L^m(x)$ için iki farklı formülü yazalım:

$$
P_L^m(x) = \frac{1}{2^L L!} (1 - x^2)^{m/2} \frac{d^{L + m}}{dx^{L + m}} (x^2 - 1)^L
$$

$$
P_L^m(x) = \frac{(-1)^m}{2^L L!} \frac{(L + m)!}{(L - m)!} (1 - x^2)^{-m/2} \frac{d^{L - m}}{dx^{L - m}} (x^2 - 1)^L
$$

Bu iki formülü çarparsak:

$$
[P_L^m(x)]^2 = \frac{(-1)^m (L + m)!}{(2^L L!)^2 (L - m)!} \frac{d^{L + m}}{dx^{L + m}} (x^2 - 1)^L \frac{d^{L - m}}{dx^{L - m}} (x^2 - 1)^L
$$

Bunu $x \in [-1, 1]$ aralığında integre edelim:

$$
\int_{-1}^1 [P_L^m(x)]^2 dx = \frac{(-1)^m (L + m)!}{(2^L L!)^2 (L - m)!} \int_{-1}^1 \frac{d^{L + m}}{dx^{L + m}} (x^2 - 1)^L \frac{d^{L - m}}{dx^{L - m}} (x^2 - 1)^L dx
$$

Parçalı integrasyon $\int u \, v = uv - \int u \, dv$ yöntemi ile sınır terimleri $(x^2 - 1) = 0$ olduğu için kaybolur ve her adımda $(-1)$ çarpanı eklenir. $m$ kez tekrar ettiğimizde:

$$
\int_{-1}^1 \frac{d^{L + m}}{dx^{L + m}} (x^2 - 1)^L \frac{d^{L - m}}{dx^{L - m}} (x^2 - 1)^L dx = \int_{-1}^1 (-1)^m \frac{d^L}{dx^L} (x^2 - 1)^L \frac{d^L}{dx^L} (x^2 - 1)^L dx
$$

Tüm ifadeleri yerine koyarsak:

$$
\int_{-1}^1 [P_L^m(x)]^2 dx = \frac{1}{(2^L L!)^2} \frac{(L + m)!}{(L - m)!} \int_{-1}^1 \frac{d^L}{dx^L} (x^2 - 1)^L \frac{d^L}{dx^L} (x^2 - 1)^L dx
$$

$$
= \frac{(L + m)!}{(L - m)!} \int_{-1}^1 [P_L(x)]^2 dx
$$

Ortonormalite özelliğinden biliyoruz ki:

$$
\int_{-1}^1 [P_L(x)]^2 dx = \frac{2}{2L + 1}
$$

Sonuç olarak:

$$
\boxed{\int_{-1}^1 [P_L^m(x)]^2 dx = \frac{2}{2L + 1} \frac{(L + m)!}{(L - m)!}}
$$
