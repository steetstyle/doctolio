---
author: Furkan Bostancı
pubDatetime: 2025-07-19T11:45:00Z
modDatetime: 2025-07-19T11:45:00Z
title: Legendre Polinomlarının Negatif Mertebe İlişkisi
slug: legendre-polinomlari-negatif-mertebe-iliskisi
featured: false
draft: false
tags:
  - Matematik
  - Elektromanyetik Teori
  - Legendre Polinomları
description: >
  Legendre polinomlarının negatif mertebe ile pozitif mertebe arasındaki ilişkiyi türev formülleriyle gösteriyoruz.
---

Negatif mertebeli Legendre polinomunu açıkça yazalım:

$$
P_L^{-m}(x) = \frac{1}{2^L L!} (1 - x^2)^{-m/2} \frac{d^{L-m}}{dx^{L-m}} (x^2 - 1)^L
$$

Ayrıca biliyoruz ki:

$$
\frac{d^{L-m}}{dx^{L-m}} (x^2 - 1)^L = \frac{(L - m)!}{(L + m)!} (x^2 - 1)^m \frac{d^{L + m}}{dx^{L + m}} (x^2 - 1)^L
$$

Bu ifadeyi yerine koyarsak:

$$
P_L^{-m}(x) = \frac{1}{2^L L!} (1 - x^2)^{-m/2} \frac{(L - m)!}{(L + m)!} (x^2 - 1)^m \frac{d^{L + m}}{dx^{L + m}} (x^2 - 1)^L
$$

$$(x^2 - 1)^m = (-1)^m (1 - x^2)^m$$ olduğundan:

$$
P_L^{-m}(x) = (-1)^m \frac{(L - m)!}{(L + m)!} \frac{1}{2^L L!} (1 - x^2)^{m - m/2} \frac{d^{L + m}}{dx^{L + m}} (x^2 - 1)^L
$$

Parantez içindeki ifade \(P_L^m(x)\) olduğundan:

$$
\boxed{P_L^{-m}(x) = (-1)^m \frac{(L - m)!}{(L + m)!} P_L^m(x)}
$$
