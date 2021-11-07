# İstatistiksel Fizik Ödev 1

> ##### Her Zaman Doğru: 
1. $$  dU = ¯dQ + ¯dW \hspace{1cm}$$ 
2. $$ ¯dQ ≤ T dS, ¯dW ≥ −p dV \hspace{1cm}$$
3. $$ dU = TdS − p dV \hspace{1cm} $$

> ##### Sadece Tersinebilir Süreçler için:
1. $$ d¯Q = T dS \hspace{1cm}  $$ 
2. $$ d¯W = −p dV \hspace{1cm} $$ 

> ##### Kullanılan Kısmi Türev Özelliği:
>$$
\left( \frac{\partial x}{\partial y} \right)_{\mathrlap{Z}} = - \left( \frac{\partial x}{\partial z} \right)_{\mathrlap{y}} \left( \frac{\partial z}{\partial y} \right)_{\mathrlap{x}} 
$$

### Maxwell' in birinci yasası
Birinci yasadan şunu hatırlayalım:

>$$ dU = ¯dQ + ¯dW \newline $$

Sadece tersinebilir süreçler için iki tane denklemimiz var:

> $$
d¯Q = TdS  \newline
$$
> $$ d¯W = −pdV. $$

Bunların ikisini birbirine kombinlersek aşağıdaki denklemi elde ediyoruz:

> $$ dU = TdS − pdV $$

Bu denklemleri sadece tersinebilir süreçler için kullanabiliyoruz fakat fonksiyonlar durumdan bağımsızdır ve bu yüzden yoldan da bağımsız olduğundan tersinebilir olmayan süreçler için de kullanılabilir. Tersinebilir olmayan süreçler için  ¯dQ ≤ T dS  ve ayrıca  ¯dW ≥ −p dV 'yi kullanabiliriz ama dQ 'yu çok küçük alırsak ve dW çok büyük olursa  tersinebilir durumlarda da kullanabiliriz. Bu nedenle bir üstteki bulduğumuz denklemi, sağlanan koşullarda her şekilde kullanabiliriz.

Bu denklem S ve V 'nin değişimine göre iç enerjideki değişimin ne kadar olduğuna dair bir ipucu veriyor. Böylece U fonskiyonumuz S ve V değişkenleri cinsinden doğal değişkenler olarak adlandırılabilir. Bu iki değişken, sistemin boyutuna göre ölçeklenir. Ama T ve P sistemin boyutuna göre ölçeklenmez, daha çok kuvvete benzerler.

#### Soru 1
> $$ dS =  \frac{dU - pdV}{T} $$
> - V 'yi sabit kabul edersek dV sıfır olur ve entropi ifadesini bir tarafta bırakıp iki tarafın V 'ye göre kısmi türevini alırsak
> $$ \left( \frac{\partial S}{\partial U} \right)_{\mathrlap{V}} = \frac{1}{T}  $$
> ardından V 'yi sabit bırakıp U 'nun S 'ye göre kısmi türevini alırsak
> $$ \left( \frac{\partial U}{\partial S} \right)_{\mathrlap{V}} = T  $$
> bulunan iki eşitliği birbiri ile çarparsak
> $$ T \frac{1}{T} = 1  $$
> buluruz.

#### Soru 2
> $$ dV =  - \frac{dU - TdS}{p} $$
> - U 'yu sabit kabul edersek dU sıfır olur ve eşitliğin S 'nin V 'ye göre kısmi türevini alırsak:
> $$ \left( \frac{\partial S}{\partial V} \right)_{\mathrlap{U}} = - \frac{p}{T}  $$
> ardından U 'yu sabit kabul edip V 'nin S'ye göre kısmi türevini alırsak:
> $$ \left( \frac{\partial V}{\partial S} \right)_{\mathrlap{U}} = - \frac{T}{p} $$
> bulunan iki eşitliği birbiri ile çarparsak:
> $$ - \frac{p}{T} - \frac{T}{p} = 1  $$
> buluruz.

#### Soru  3
> Matematiksel olarak biraz oynayıp dU formuyla karşılaştırarak:
>- dU 'da  V 'yi sabit kabul edip U 'nun S'ye göre kısmi türevini alır isek dV sıfır olacağından alttaki eşitliği elde ederiz.
>
>    $$
>    \left( \frac{\partial U}{\partial S} \right)_{\mathrlap{V}} = T \newline
>    $$
>- bu sefer de S 'yi sabit kabul edip U 'nun V 'ye göre kısmi türevini alır isek dS sıfır olacağından alttaki eşitliği elde ederiz.
>
>    $$
>    - \left( \frac{\partial U}{\partial V} \right)_{\mathrlap{S}} = p \newline
>    $$
>Yukarıda bulduğumuz 1. denklemi tersine çevirir isek aşağıdaki denklemi elde ederiz.
>    $$
>    \frac{1}{T} = \left( \frac{\partial S}{\partial U} \right)_{\mathrlap{V}}
>    $$
>P ve 1/T 'yi birbiri ile çarpar isek ve yukarıdaki kısmi türev özelliğini uygularsak
>    $$
>    \frac{p}{T} =  - \left( \frac{\partial U}{\partial V} \right)_{\mathrlap{S}}  \left( \frac{\partial S}{\partial U} \right)_{\mathrlap{V}} = \left( \frac{\partial S}{\partial V} \right)_{\mathrlap{U}}$$
>olduğunu görürüz 
