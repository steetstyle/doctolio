---
author: Furkan Bostancı
pubDatetime: 2021-07-19T12:00:00Z
modDatetime: 2021-07-19T12:00:00Z
title: Legendre Polinomları Türev İlişkisi
featured: false
draft: false
tags:
  - Matematik
  - Elektromanyetik Teori
  - Legendre Polinomları
description: >
  Legendre polinomlarının türev ifadelerini Leibniz kuralı ile adım adım türetiyoruz.
---

Başlangıç ifademiz:

$$
\frac{d^{L-m}}{dx^{L-m}}(x^2 - 1)^{L} = \frac{d^{L-m}}{dx^{L-m}}(x-1)^{L}(x+1)^{L}
$$

Leibniz kuralı, herhangi bir \(n\)-kat türevlenebilir fonksiyon \(f\) ve \(g\) için:

$$
(fg)^{(n)} = \sum_{m=0}^n \frac{n!}{m!(n-m)!} f^{(n-m)} g^{(m)}
$$

Ayrıca şunu hatırlayalım:

$$
\frac{d^n}{dx^n}(x-a)^l = \frac{l!}{(l-n)!}(x-a)^{l-n}
$$

\(n > l\) için türev sıfırdır.

Leibniz kuralını uygulayalım:

$$
\frac{d^{L-m}}{dx^{L-m}}(x-1)^{L}(x+1)^{L} =
\sum_{k=0}^{L-m} \frac{(L-m)!}{k!(L-m-k)!} \frac{d^{L-m-k}}{dx^{L-m-k}} (x-1)^L \frac{d^k}{dx^k} (x+1)^L
$$

Türevleri açarsak:

$$
= \sum_{k=0}^{L-m} \frac{(L-m)!}{k!(L-m-k)!} \frac{L!}{(m+k)!}(x-1)^{m+k} \frac{L!}{(L-k)!}(x+1)^{L-k}
$$

Toplama sınırlarını yeniden düzenleyip çarpanları ayırırsak:

$$
\frac{d^{L-m}}{dx^{L-m}}(x^2 - 1)^L = \frac{(L-m)!}{(L+m)!}(x^2 - 1)^m \sum_{k=0}^{L} \frac{(L+m)!}{k!(L+m-k)!} \frac{d^{L+m-k}}{dx^{L+m-k}}(x-1)^L \frac{d^k}{dx^k}(x+1)^L
$$

Bu, Leibniz kuralının ters uygulanmasıyla şu hale gelir:

$$
\frac{d^{L-m}}{dx^{L-m}}(x^2 - 1)^L = \frac{(L-m)!}{(L+m)!} (x^2 - 1)^m \frac{d^{L+m}}{dx^{L+m}}(x^2 - 1)^L
$$

Sonuç olarak:

$$
\boxed{\frac{d^{L-m}}{dx^{L-m}} (x^2 - 1)^L = \frac{(L-m)!}{(L+m)!} (x^2 - 1)^m \frac{d^{L+m}}{dx^{L+m}} (x^2 - 1)^L}
$$
