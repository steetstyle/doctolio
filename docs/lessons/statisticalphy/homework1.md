# Istatistiksel Fizik Odev 1

> ##### Her zaman doğru: 
1. $$  dU = ¯dQ + ¯dW \hspace{1cm}$$ 
2. $$ ¯dQ ≤ T dS, ¯dW ≥ −p dV \hspace{1cm}$$
3. $$ dU = TdS − p dV \hspace{1cm} $$

> ##### Sadece Tersinebilir süreçler için:
1. $$ d¯Q = T dS \hspace{1cm}  $$ 
2. $$ d¯W = −p dV \hspace{1cm} $$ 

> ##### Kullanilan kısmı Türev özelliği
>$$
\left( \frac{\partial x}{\partial y} \right)_{\mathrlap{Z}} = - \left( \frac{\partial x}{\partial z} \right)_{\mathrlap{y}} \left( \frac{\partial z}{\partial y} \right)_{\mathrlap{x}} 
$$

### Maxwellin birinci yasasından
Birinci yasadan sunu hatırlayalım

>$$ dU = ¯dQ + ¯dW \newline $$

Sadece tersinebilir süreçler için iki tane denklemimiz var

> $$
d¯Q = TdS  \newline
$$
> $$ d¯W = −pdV. $$

Bunlarin ikisini birbirine kombinlersek aşağıdaki denklemi elde ediyoruz.

> $$ dU = TdS − pdV $$

Bu denklemleri kullanırken sadece tersinebilir süreçler için belirttiğimiz denklemlri kullanıyoruz fakat fonksiyonlar durumdan bağımsızdır ve bu yüzden yoldan da bağımsız olduğundan tersinebilir olmayan süreçler içinde kullanılabilir. Tersinebilir olmayan süreçler için  ¯dQ ≤ T dS  ve ayrıca  ¯dW ≥ −p dV yi kullanabiliriz ama dQ yu cok küçük alırsak ve dW ise cok büyük olursa  tersinebilir durumlarda da kullanabiliriz. bu nedenle bir üstte ki bulduğumuz denklemi sağlanan koşullarda her şekilde kullanabiliriz.

Bu denklem S ve V nin değişimine göre iç enerjideki değişimin ne kadar olduğuna dair bir ipucu veriyor. Boylece U fonskiyonumuz S ve V değişkenleri cinsinden dogal değişkenler olarak adlandırabiliriz. Bu iki degişkenin sistemin boyutuna göre olçeklenirler. Ama T ve P sistemin boyutuna göre ölçeklenmezler daha çok kuvvete benzerler.

#### Soru 1
> $$ dS =  \frac{dU - pdV}{T} $$
> - V yi sabit birakırsak dV sıfır olur ve eşitiliğin U ya göre kısmını alır isek
> $$ \left( \frac{\partial S}{\partial U} \right)_{\mathrlap{V}} = \frac{1}{T}  $$
> ardindan V yi sabit birakip U nun S'ye göre kısmı turevini alır isek
> $$ \left( \frac{\partial U}{\partial S} \right)_{\mathrlap{V}} = T  $$
> bulunan iki eşitliği birbiri ile carpar isek
> $$ T \frac{1}{T} = 1  $$
> buluruz.

#### Soru 2
> $$ dV =  - \frac{dU - TdS}{p} $$
> - U yi sabit bırakırsak dU sıfır olur ve eşitiliğin V'ye göre kısmını alır isek
> $$ \left( \frac{\partial S}{\partial V} \right)_{\mathrlap{U}} = - \frac{p}{T}  $$
> ardindan V yi sabit birakip U nun S'ye göre kısmı turevini alır isek
> $$ \left( \frac{\partial V}{\partial S} \right)_{\mathrlap{U}} = - \frac{T}{p} $$
> bulunan iki eşitliği birbiri ile carpar isek
> $$ - \frac{p}{T} - \frac{T}{p} = 1  $$
> buluruz.

#### Soru  3
> Matematiksel olarak biraz oynayıp dU formuyla karşılaştırarak:
>- dU da  V yi sabit birakip S'ye göre kısmı turev alır isek dV sıfır olacagindan alttaki eşitliği elde ederiz.
>
>    $$
>    \left( \frac{\partial U}{\partial S} \right)_{\mathrlap{V}} = T \newline
>    $$
>- bu seferde S yi sabit birakip V'ye göre kısmı turev alır isek dS sıfır olacagindan alttaki eşitliği elde ederiz.
>
>    $$
>    - \left( \frac{\partial U}{\partial V} \right)_{\mathrlap{S}} = p \newline
>    $$
>Yukarıda buldugumuz 1. denklemi tersine cevirir isek aşağıdaki denklemi elde ederiz.
>    $$
>    \frac{1}{T} = \left( \frac{\partial S}{\partial U} \right)_{\mathrlap{V}}
>    $$
>P ve 1/T yi birbiri ile carpar isek ve yukaridaki kısmı Turev ozelligini uygularsak
>    $$
>    \frac{p}{T} =  - \left( \frac{\partial U}{\partial V} \right)_{\mathrlap{S}}  \left( \frac{\partial S}{\partial U} \right)_{\mathrlap{V}} = \left( \frac{\partial S}{\partial V} \right)_{\mathrlap{U}}$$
>olduğunu görürüz 