# İstatistiksel Fizik Ödev 2

# Genel Bilgiler

### İç enerji 

İç enerji deki değişim:

>$$dU = TdS - PdV $$

denklemimizin bize anlatmak istediği olay iç enerjide ki değişimin sadece entropi(S) ve hacimin(V) herhangi birinde değişim olduğunda iç enerjide değişim olacağını belirtiyor.

### Entalpi

Entalpi tersinir izobarik süreçler için sistem tarafından emilen ısıyı gösterir.

>$$H = U + PV $$

Entalpideki değişim miktarı:

>$$dH = TdS - PdV + PdV + VdP $$

>$$dH = TdS + VdP $$

### Tam Diferansiyel Denklemler

F(x, y) tam diferansiyel bir denklem olup aşağıdaki forma sahip olsun.

>$$F(x, y) = Adx + Bdy $$

eğer tam bir diferansiyelse aşağıdaki kısmi türev formları birbirlerine eşittir.

>$$(\frac{\partial{A}}{\partial{y}})_x = (\frac{\partial{B}}{\partial{x}})_y $$

#### Soru  1

Sabit sıcaklıkta olduğu için sıcaklıtaki değişim miktarı 0 bu yüzden başlangıçtaki sıcaklık ile en son durumdaki sıcaklık birbirne eşittir.
$$ T_i = T_s $$

Sistem üzerinde üzerinde iş yapılmıyor ve V hacminden 3V hacmine aniden çıkarılıyor.
Gazdaki entropi değişimini bulmamız istiyor, entropi bir durum fonksiyonudur ve durum fonskiyonu olması dolayısıyla yoldan bağımsızdır.
Sistemde sıcaklık değişimi olmadığı için izotermal bir süreç bundan dolayı aklımıza hemen izotermal süreçlerde ideal gazdaki iç enerjinin değişiminin 0 olduğu aklımıza gelmeli.

$$ 0 =  TdS - PdV $$

$$ dS = \frac{P}{T}dV = \frac{R}{V}dV $$

Gazın Entropisindeki degişimi hesaplamak için integral dS in integralini almamız gerekli o yüzden:

$$ \Delta{S}_{Gaz} = \int^{3V}_{V}{\frac{R}{V}dV} = R ln(3)$$ 

sistem izotermal olduğundan dışındaki sistem varsa bile ısı alışverişi olmadığından sistemin toplam entropisindeki değişim sıfır olacaktır.



#### Soru  2
Helmholtz potansiyeli sabit sıcaklıkta sistemden alabileceğimiz maksimum iş miktarını temsil edebilir.

Helmholtz potansiyeli şu şekilde tanımlanır:

$$ F = U  - TS $$

Helmholtz potansiyelindeki değişim miktarı:

>$$dF = dU - SdT - TdS = TdS - PdV - SdT - TdS $$

>$$dF = -SdT -PdV $$

Entropiyi Helmholtz potansiyeli cinsinden yazmak istersek, hacimi sabit bırakıp sıcaklığa göre kısmı türevini alıp -1 ile çarmamız gerekli:

>$$S = -(\frac{\partial{F}}{\partial{T}})_V  $$

Basıncı Helmholtz potansiyeli cinsinden yazmak istersek, Sıcaklığı sabit bırakıp hacime göre kısmı türevini alıp -1 ile çarmamız gerekli:

>$$V = -(\frac{\partial{F}}{\partial{V}})_T  $$

#### Soru  3
Gibbs potansiyeli bir sistemin sabit basınç ve sıcaklık altında üretebildiği kullanılabilir iş miktarını temsil edebilir.

Gibbs potansiyeli şu şekilde tanımlanır:

$$ G = H - TS $$

Gibbs potansiyelindeki değişim miktarı:

>$$dG = dH - SdT - TdS   = TdS + VdP - SdT - TdS $$

>$$dG = -SdT -VdP $$

Entropiyi Gibbs potansiyeli cinsinden yazmak istersek, basıncı sabit bırakıp zamana göre kısmı türevini alıp -1 ile çarmamız gerekli:

>$$S = -(\frac{\partial{G}}{\partial{T}})_P  $$

Hacimi Gibbs potansiyeli cinsinden yazmak istersek, Sıcaklığı sabit bırakıp basınca göre kısmı türevini alıp -1 ile çarmamız gerekli:

>$$V = -(\frac{\partial{G}}{\partial{P}})_T  $$

#### Soru  4
Maxwell denklemlerini türetirken yukarıda bahsettiğimi tam difernasel denklem özelliğini kullanacağız.

İç enerjideki değişimin formülü elimizde var yukarıdaki tam diferansiyel özelliğini kullanırsak:

>$$(\frac{\partial{T}}{\partial{V}})_S =  - (\frac{\partial{P}}{\partial{S}})_V $$

Entalpi deki değişimin formülünü kullanıp yukarıdaki tam diferansiyel özelliğini kullanırsak:

>$$(\frac{\partial{T}}{\partial{P}})_S =   (\frac{\partial{V}}{\partial{S}})_P $$

Helmholtz potansiyel değişimin formülünü kullanıp yukarıdaki tam diferansiyel özelliğini kullanırsak:

>$$(\frac{\partial{S}}{\partial{V}})_T =   (\frac{\partial{P}}{\partial{T}})_V $$

Gibbs potansiyel değişimin formülünü kullanıp yukarıdaki tam diferansiyel özelliğini kullanırsak:

>$$(\frac{\partial{S}}{\partial{P}})_T =  -(\frac{\partial{V}}{\partial{T}})_P $$

