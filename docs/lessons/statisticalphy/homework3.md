# İstatistiksel Fizik Ödev 3 

## Soru 
$$ T_1 + T_2 = T = sabit $$
$$ \mu_1 + \mu_2 = \mu = sabit $$
$$ V_1 + V_2 = V = sabit $$
$$ dF = dF_1 + dF_2 = 0 $$

şartından sistemin sağlaması gereken özellikleri bulunuz. 

## Cevap

$$ \boxed{1} \rightarrow \boxed{2} $$

İç enerjideki değişimi bulmak için 

$$ dU =  TdS - pdV + \mu dN $$

formulunden entropi degisimini cekelim

$$ dS = \frac{dU}{T} + \frac{pdV}{T} - \frac{\mu dN}{T} $$ 

$$ \lparen \frac{\partial S}{\partial U} \rparen_{N,V} = \frac{1}{T} $$

$$ \lparen \frac{\partial S}{\partial V} \rparen_{N,U} = \frac{p}{T} $$

$$ \lparen \frac{\partial S}{\partial N} \rparen_{U,V} = - \frac{\mu}{T} $$

Şimdi iki sistem düşünelim ve bu sistemler birbirleriyle etkileşime girsinler. Sistemin denge durumunu belirlemek için termodinamiğin ikinci kanunu kullanabiliriz.

$$ F = U - TS $$

$$ dF = TdS - pdV + \mu dN - TdS - SdT $$ 

$$ dF =  \boxed{\mu dN - SdT - pdV} $$

$$ dF = dF_1 + dF_2 $$
iki sistem dışarı ile izole olduğunu varsayalım bu durumda dN parçacık değişimi diğer sisteme geçeceğinden tüm sistemdeki toplam parçacık sayısı sabit kalacağından toplam dN sıfır olacaktır. Bu mantığı sıcaklık, iç nerjideki değişim ve hacime de uygular isek.

$$ T_1 + T_2 = T = sabit $$
$$ \mu_1 + \mu_2 = \mu = sabit $$
$$ V_1 + V_2 = V = sabit $$

---

$$ dT_1 + dT_2 = dT = 0 $$
$$ d\mu_1 + d\mu_2 = d\mu = 0 $$
$$ dV_1 + dV_2 = 0 $$

---
Helmholtz daki entropi değişkenini bulmak için iç enerjideki değişimden entorpi değişimini çekelim.

$$ dS = \lparen \frac{\partial S}{\partial U} \rparen_{N,V} (dU) + \lparen \frac{\partial S}{\partial V} \rparen_{N,U} (dV) - \lparen \frac{\partial S}{\partial N} \rparen_{U,V} (dN) $$ 

$$ dS = \lparen \frac{1}{T_1} - \frac{1}{T_2}  \rparen (dU_1) + \lparen \frac{p_1}{T_1} - \frac{p_2}{T_2}  \rparen (dV_1) - \lparen \frac{\mu_1}{T_1} - \frac{\mu_2}{T_2}  \rparen (dN_1) $$

ve bu durumda iki sistemden birinin iç enerji, parçacık sayısı, hacim değişim sıfır olmadığı için yanındaki katsayılar sıfır olmalı

$$ \frac{1}{T_1} - \frac{1}{T_2} = 0 $$

$$ \frac{p_1}{T_1} - \frac{p_2}{T_2} = 0 $$

$$ \frac{\mu_1}{T_1} - \frac{\mu_2}{T_2} = 0 $$

Sistem durgun durumda iken helmholtz fonksiyonunun değişimi sıfır olmalı buradan çıkaracağımız sonuç toplam entropinin sabit olacağı.Çünkü sistem dışarıyla izole ve dışarıyla herhangi bir takas yapmıyor. Bu yüzdendir ki aşağıdaki koşulları sağlaması gereklidir.

---

$$ T_1 = T_2 $$

$$ p_1 = p_2 $$

$$ \mu_1 = \mu_2 $$

$$ S_1 = S_2 $$



