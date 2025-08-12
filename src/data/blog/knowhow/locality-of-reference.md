---
author: Furkan Bostancı
pubDatetime: 2025-07-29T19:00:00Z
modDatetime: 2025-07-29T19:00:00Z
title: Locality of Reference ve Performans Optimizasyonları
featured: true
draft: false
tags:
  - Performans
  - Oyun Geliştirme
  - Locality of Reference
  - CPU Cache
  - GPU Cache
  - C++
  - Vertex Buffer
  - ECS
description: >
  Modern CPU tarafındaki cache mekanizmaları, locality of reference kavramı ve oyun motorlarında performans için optimizasyon teknikleri
---

Hızlı çalışması gereken bir yazılım oluşturman gerektiğinde, hızlı ve verimli olması için gerçekten çok fazla çaba harcaman gerekiyor—özellikle oyun motorlarında. Algoritmaları büküp, grafiklerin nasıl çizildiğini iyileştirip, birden fazla CPU çekirdeğini kullanarak işleri hızlandırman gerekebilir.

Ama burada iyi yazılmış programları bile yavaşlatan gizli bir problem var: Bilgisayarlardaki CPU'ların belleği nasıl yönettiği...
İşte burada ```locality of reference``` devreye giriyor ve önemini gösteriyor.

Aslında tüm mesele, kodun bilgisayarın belleğiyle ne kadar verimli iletişim kurduğu ve bunun oyunun akıcı ve uyumlu bir şekilde çalışmasını doğrudan etkilemesi. Buna yeteri kadar dikkat etmediğinde, gerçekten üzerine çok çaba harcadığın optimizasyonlar tamamlanmamış olabilir; örneğin sürekli ana bellekteki veriyi çekerek daha yavaş çalışmasına neden olabilirsin.

Hadi basit bir C++ kod örneğine bakalım. Hayal edelim ki büyük bir grid'imiz var, bu grid oyundaki dünyamızı veya bir texture'ı temsil ediyor. C++ üzerinde 2D array'i ```float grid[HEIGHT][WIDTH]``` şeklinde satır satır bellekte tutabiliriz, bu da tüm sayıların tek bir satırda yan yana tutulduğu anlamına geliyor.

```
// 2D bir array, oyunumuzun dünyasındaki bir grid veya texture
const int WIDTH = 4096;
const int HEIGHT = 4096;
float grid[HEIGHT][WIDTH];

void processGridBadly() {
	// Bu şekilde iterate etmek bellekteki veriye ulaşmak için iyi bir yöntem değil
	for (int j = 0; j < WIDTH; ++j) {
		for (int i = 0; i < HEIGTH; ++i) {
			grid[i][j] *= 0.5f;
		}
	}
}

void processGridWell() {
	// Bu şekilde iterate etmek bellekteki veriye ulaşmak için daha iyi bir yöntem
	for (int j = 0; j < HEIGTH; ++j) {
		for (int i = 0; i < WIDTH; ++i) {
			grid[i][j] *= 0.5f;
		}
	}
}
```

Yukarıdaki iki metod da aynı sayıda işlem yapıyor. Bu yüzden aynı sürede çalışacaklarını varsayabilirsiniz; ancak modern bilgisayarlarda ```processGridWell()``` metodu çok daha hızlı çalışacaktır. Peki neden? Gerçekten arka planda ne oluyor? Bunun cevabı ```locality of reference``` kavramında yatıyor. Oyun motorlarındaki matematik ve grafik üzerinde ne kadar büyük bir etkisi olduğuna bakalım ve ardından oyunlarınızı nasıl daha hızlı ve akıcı hale getirebileceğinizi örneklerle görelim.

### Locality of Reference

Davranışını öngörebildiğimiz işlemlerin neden bazı yazım biçimlerinde daha fazla performans gösterdiğini anlamak için modern CPU’ların veriyi nasıl işlediğini bilmek önemli. Bellek erişiminde iki temel prensip vardır: ```temporal locality``` ve ```spatial locality```. Bu konseptler, CPU’nun küçük ama çok hızlı bellek alanlarında (cache) veriyi nasıl sakladığını ve yazılımın bir sonraki adımda hangi veriye ihtiyaç duyabileceğini nasıl tahmin ettiğini açıklar.

İlk olarak ```temporal locality```’den bahsedelim. Bu prensip, eğer bir veri parçası kullanılıyorsa, aynı parçanın yakın gelecekte tekrar kullanılma olasılığı çok yüksektir. Bunu şöyle düşünebiliriz: alet çantanızdan spesifik bir aleti seçtiniz; biraz sonra muhtemelen o aleti yeniden kullanacaksınız. CPU’lar da son kullanılan verileri hızlı çalışan cache’lerinde tutar. Program bu veriyi yeniden istediğinde, eğer cache içindeyse buna ```cache hit``` denir ve bu erişim neredeyse anlıktır. Eğer veri cache’de değilse, ```cache miss``` meydana gelir ve CPU, bu veriyi yavaş olan ana bellekte arar, bu da erişim süresini artırır.

Aşağıda iyi ve basit ```temporal locality``` kullanımı örnek olarak bulabilirsiniz:

```
// İyi bir temporal locality örneği: 'sum' değişkenine tekrar tekrar erişiliyor

long long calculateSum(const std::vector<int>& data) {
	long long sum = 0; // Her iterasyonda 'sum' değişkenine erişiliyor
	for (int x : data) {
		sum += x; // 'sum' değişkeni sürekli okunup üzerine yazılıyor ve cache üzerinde tutuluyor
	}
	return sum;
}
```

Bu örnekteki sum değişkeni sürekli kullanıldığından CPU onu cache üzerinde tutar. Bu sayede hızlı bir şekilde güncellenir. ```Temporal locality```’nin kötü bir örneğini göstermek zordur çünkü genellikle doğal yazım tarzı zaten bu prensibe uygundur. Ancak uzun süre kullanılmayan bir değişkenin yeniden kullanılması durumunda, bu veri büyük olasılıkla cache’ten çıkarılmış olur --```cache eviction```-- ve erişimde yavaşlama yaşanabilir.

Şimdi bi sonraki arkadaşımıza uğrayalım ```spatial locality```. Bu prensip eğer programınız bir parça veriye ulaşmaya çalışıyor, yüksek olasılıkla veriye ulaşılan yerlere yakın konumlardaki veriye erişlmeye çalışacaktır memory içindeki. Bu önemli bi konsept çünkü CPU veriyi byte by byte çekmez memory'nin tüm parçasını getirir ki bu tipik olarak 64 byte boyutunda ```cache line``` olarak adlandırılır. Özetle davranışı eğer bir memory'den veri çekildiyse yakın zaman o memorydeki diğer byte'larda kullanılacaktır varsayımı yapar.

Hadi şimdi tekrar 2D array örneğinizden ```spatial locality```'i ziyaret edelim:

```
// İyi bir spatial locality örneği (bitişik bellek üzerinden iterasyon)
void processGridWell() {
	for (int i = 0; i < HEIGHT; ++i) {
		for (int j = 0; j < WIDTH; ++j) {
			// Birbirine bitişik sayılara erişim yapılıyor ve değerleri değiştiriliyor
			grid[i][j] *= 0.5f;
		}
	}
}
```

```processGridWell()``` fonskiyonunda dönğü j üzerinden ```grid[i][0]```, ```grid[i][1]```, ```grid[i][2]``` şeklinde devam ediyor. Bu ulaşılan array elelmanları ardışık şekilde tutuluyor memory içinde raw-major array için. ```grid[i][0]``` yüklendiğinden  ```grid[i][0]```, ```grid[i][1]```, ```grid[i][2]``` şeklinde ilerleyen tüm ```cache line``` getirip cacheliyor. Tüm erişelecek veriler ```grid[i][0]```, ```grid[i][1]```, ```grid[i][2]``` ... artık hızlı ```cache hits```lerde. Burada ```grid[i][j]``` ardışık verilere ulaşacak şekilde ayarlanırsa sonuçunda bellekte yan yana tutulduğu için, ```cache line``` sayesinde bu veriler cache’e toplu halde yüklenir. Bu da erişimlerin çoğunun ```cache hit``` ile sonuçlanmasını sağlar.

Aşağıda kötü bir ```spatial locality``` örneği yer alıyor:

```
// Kötü spatial locality (array elemanlarına büyük stride aralıkları ile ulaşmak)
void processGridBadly() {
	for (int j = 0; j < WIDTH; ++j) {
		for (int i = 0; i < HEIGHT; ++i) {
			// Array elemanlarına ulaşırken birbirnden uzak ardışık olmayan şekilde memoryden ulaşıyor
			grid[i][j] *= 0.5.f;
		}
	}
}
```

```processGridBadly()``` fonskiyonu içerisinde döngü boyunca ```i``` üzerinden ilerliyor. Çünkü örnekteki array row-major bir array ve ```grid[0][j]```, ```grid[1][j]``` elemanları ardışık bir şekilde birbirini izlemiyor bunlar tamamen farklı satırlardaki veriler. Her ```grid[i][j]``` elemanına erişiminde olası bi ihtimalle yeni bi ```cache miss```'e hit edecektir cache'e yüklenmeyen bir ```cache line``` olduğu için, bu şüreçte CPU sürekli ```cache line``` çekmeye çalıştığından kayda değer bir yavaşlama gözlemleyebiliriz.

### AoS vs SoA ve ECS

Matematik çok temel bi konu oyun motorlarından konuşurken. Az önce örneklendirdiğimiz basit grid örneği bile, veriye erişimi belirleme yöntemimizin performansa nasıl büyük bir  etki gösterebileceğini farkettiriyor ```locality of reference``` yüzünden. Bu bizi çok tercih edilen ve önemli bi tasarım seçimine getiriyor oyun geliştirmede. Memory üzeirinde veriyi nasıl dağıtmalısın, özellikle ```Array of Structs (SoA)``` ve ```Struct of Arrays(SoA)```. Burada yapılan şeçim direk olarak verinin nasıl saklandığını ve CPU'nun cache'ini verimli bir şekilde kullanıldığını etkiliyor.

Bir parçacık sistemi oluşturduğumuzu düşünelim, her bir parçacığın konumu, hızı ve kalan bir zamanı olsun.

```Array of Structs (SoA)``` yaklaşımında bir array veya liste oluştururuz tüm elemanları ```struct```veya ```object``` olan ve parçacığı temsil eden.
Bu da tek bir parçacıkta ki verinin (konumu, hızı ve kalan yaşam süresi) ardışık olarak tutulduğu anlamına geliyor.

```
struct ParticleAoS {
	glm::vec3 position;
	glm::vec3 velocity;
	float life;
}

std::vector<ParticleAoS> particleAoS;

void updateParticleAoS(float deltaTime) {
	for (auto& p : particlesAoS) {
		// Konumunu, hızını ve kalan yaşam süresini güncelle
		p.positon += p.velocity * deltaTime;
		p.life -= deltaTime;
	}
}
```

Tek bir parçacık için ```Array of Structs (SoA)``` yöntemi çok kullanışlı gözüküyor çünkü ne zaman bu parçacığın bir verisine ulaşmak istediğimizde bütün elemanları (konumu, hızı ve yaşam süresi) CPU tarafından getirilip aynı zamanda cache'ine yükleniyor çünkü tüm veri yanyana olacak şeklide memory'de duruyor.

Kod üzerinde sürekli eriştiğin veri yanyana ise tek bir parçacık için bu mükemmel bişi. Örnek olarak eğer fonksiyonun konumunu, hızını ve yaşam süresini sürekli güncelliyorsa ```Array of Structs (SoA)``` erişim durumumuz için mükemmel bir ```spatial locality``` sağlıyor. Parçacık için bir veriye ihtiyacın olduğunda yüm veri ```cache hit``` üzerinden geliyor.

```Struct of Arrays (SoA)``` yaklaşımında ise tam tersine veriyi componente göre organize ederiz. Tam parçacık listesi yerine parçacıklarla alakalı tüm bilglieri ayrı ayrı listelerde barındırırız bütün konuları, bütün hızları, bütün yaşam sürelerini ayrı array veya listelerde tutmak şeklinde.

```
struct ParticleSoA {
	std::vector<glm::vec3> positions;
	std::vector<glm::vec3> velocities;
	std::vector<float> lives;
};

ParticleSoA particlesSoA;

void updateParticlePositionsSoA(float deltaTime) {
	for (size_t i = 0; i < particlesSoA.positions.size(); ++i) {
		// Sadece konum ve hıza erişiyoruz ilgili konumu verisini güncellemek için
		particlesSoA.positions[i] += particlesSoA.velocities[i] * deltaTime;
	}
}
```

```Struct of Arrays (SoA)``` yaklaşımı parçacıkların bi parça verisi kullanırken gerçekten parlıyor. Örnek verelim bir tane fonksiyonunuz var parçacığın sadece konumu güncelleiyor hıza bağımlı bir şekilde (bknz... ```updateParticlePositionsSoA```), ```Struct of Arrays (SoA)``` yaklaşımını kullanırken konum ve hız arrayleri CPU'nun cacheine yükleniyor ```lives``` verisi fonskiyon içerisinde herhangi bir spesifik task için kullanılmadığı için CPU cacheinin dışında kalıyor. Gerçekleşen bu davranış cache space ve memory bandwith üzerinde çok değerli daha fazla kullanılabilir bir kaynak bırakıyor, bu nedenle CPU ihtiyaç duymadığı verileri getirmeyip gereksiz zaman harcamıyor.

İşte bu yüzden bu yapılar ```Entity-Component-System (ECS)``` mimarisi gibi popüler be modern ve yüksek performanslı oyun motorlarınında kullanılıyor. Bu yazıda ```Entity-Component-System (ECS)``` için detaylı bi izaha girmeyeceğiz (Daha detaylı açıklamasını buradan bulabilirsin [ECS](https://levelup.gitconnected.com/fast-ecs-from-scratch-in-rust-for-your-game-engine-d7de8f23cd4a)), burada önemli olan ```locality of reference```yi kullanmak neden iyi bi getirisi olabiliyor. ```Entity-Component-System (ECS)``` doğası gereği ```Struct of Arrays (SoA)``` tarzı bi veri düzenini kullanımını teşvik ediyor. Örneğin, ne mzaman ```Movement System```tüm konuları hıza bağlı bi şekilde güncellemek isterse ```PositionComponent```ve ```VelocityComponent``` ardışık memory blokları sayesinde iterate edebilir. Buradaki can alıcı ve en önemli avantah bu ayrım sayesinde sistem sadece işlemi yapmak için gerekli spesifik veriyi o işlemde CPU cache'ine yükler. Diğer herhangi bir bileşen ```RenderComponent```veya ```HealthComponent``` tamamen ```Movement System```tarafından göz ardı edildiğinden bu veriler hiç zaman değerli CPU cache'ine yüklenmez. Bu ```cache pollution```'a engel olarak sadece gerekli ve ilgili verilerin cache üzerinde doldurulmasına ve yüksek verimli bi işleme yol açar.

Ancak şunu belirtmekte fayda var ```Entity-Component-System (ECS)``` gerçek uygulamalrda parçalı tasarımı teşvik ederken, bazı durumlarda sürekli grup olarak beraber çekilen veriyi tek bir component üzerinde birleştirmek güzel bi fikir olabilir. Örneğin ```PositionComponent```ve ```VelocityComponent``` genellikle her zaman beraber okunur ve yazılır ```Movement System```tarafından bu yüzden bu verileri birleştirip tek bir ```KinematicsComponent``` yapmak daha verimli olabilir.

```
struct KinematicsComponent {
	float position[3];
	float velocity[3];
};
```

Bu pratik, önde gelen oyun motorları ve çatıları altında sıklıkla göreceğiz bir yaklaşımdır. Örnek olarak ```Unity’s Data-Oriented Technology Stack (DOTS)```'ı düşünün. Onların ```Entity-Component-System (ECS)``` implementatsyonu konum, rotasyon ve ölçek verilerinin barındıran ```LocalTransform``` componentine sahip. Kesinlikle bu veriler farklı componentlere ayırılabilir ama bunları beraber şekilde tamamen mantıklı bi tasarım şekliyle 3 parça veriyi tek bir olarak neredeyse her zaman aynı anda erişildiğinden tek bir bileşen olarak uygulamışlar çünkü herhangi bir mekansal dönüşüm uygulamadığımızda her bu 3 veriye ulaşıyoruz genellikle.

### Vertex Buffer Data Layout

Verinizi Vertex Buffer üzerinde konumlandırma stratejiniz GPU'nun internal cache'i üzerindeki verimliliğini kayda değer miktarda etkiliyor. İki tane temel yaklaşımımız var, bunlar Interleaved ve Seperated layoutlar

``Interleaved Vertex Data Layout``` yaklaşımında tüm özellikler tek bir vertex(position, normal, texture kordinatları ve dahası) olarak ardışık şekilde memory'de tutuluyor. Bu yaklaşım konseptsel olarak daha önce CPU tarafında bahsettiğimiz ```Àrray of Structs (AoS)``` yaklaşımına benziyor. Her bir ```struct```ın bu bağlamda tam bir ```vertex``` olarak ilgili şekilde toplanmış olarak.


```
struct VertexInterleaved {
	glm::vec3 position;
	glm::vec2 normal;
	glm::vec2 textCoord;
};

std::vector<VertexInterleaved> interleavedVertices;
```

Bu layout özellikle ```vertex shader```'ının tüm ```vertex```'in özelliklerine ulaşmak istediğinde çok iyi bir performans sunar. Örneğin GPU ne zaman ```vertex```in konumunu çekmek istese yüksek olasılıkla normal ve texture kordinatlarınıda beraberinde aynı ```cache line``` üzerinde çeker çünkü bu ulaşılmak istenen veri paketlenmiş bir şekilde tutuluyor. Bu yaklaşım genellikle genel rendering işlemleri için daha kolay yönetilmesi yüzünden tüm ```vertex``` özelliklerini sürekli kullanırken tercih edilir.

Alternatif olarak ```Seperated Vertex Data Layout``` yönteminde ise her bir özellik tipi kendi ayrı, ardışık dizilerinde saklanır. Örneğin her ```vertex``` konumları bir array içinde, tüm normaller bir başkası içinde, tüm texture kordinatları bir başka array'in içinde şeklinde bir veri düzeninde gider. Bu yaklaşımı direkt olarak daha önce bahsettiğimiz ```Structs of Arrays (SoA)``` ile eşleştirebiliriz.

```
struct MeshDataSeperated {
	std::vector<glm::vec3> positions;
	std::vector<glm::vec3> normals;
	std::vector<glm::vec2> texCoords;
};

MeshDataSeperated seperatedMeshData;
```
Bu layout özellikle bir çok spesifik özellikler çok fazla ```vertices```üzerinden gerekliyse üstün bir ```spatial locality```sağlar. Örneğin ```render pass``` üzerinde sadece ```positions``` okuyorsun ```shadow map generation``` veya ```depth only pass```yaparken GPU bu durumda sadece ```positions``` arrayini çekebilir. Bu yöntem kesinlikle ```seperated layout```lar için yararlı bi kullanım alanı, ```normal```leri ve ```texCoords```u cache'e yükelemekten kaçınmak için. Buna karşın kesinlikle daha fazla memory bandwith kullanımına sahip oluyoruz ama pazarlıksız bir takas olmaz, yönetimi biraz daha karmaşıklaşabiliyor özellikle birbirinden uzak bufferların senkronize olması gerekirken.

### Index Buffer Order

Vertex verilerini nasıl düzelendiğimizin ötesinde bunları hangi sıra ile üçlerin çizilmesine talimat vermek bile rendering performansını önemli ölçüde etkiliyor bu yüzden ```index buffer```kullanmak rendering performansı önemli ölçüde arttırıyor. Bunun birincil nedeni ```GPU Vertex Cache```.

Modern GPUlar küçük ama yüksek hızlı cacheler ile özellikle donatılmıştır vertices sonuçlarını önceki hesaplanmasını tamamlamış ```vertex shader``` üzerinden sonuçlarını kaydederek. ```Vertex shader``` belirli ```vertex```ler ile işini bitirdiğinde hesaplanan değerler geçiçi olarak cache üzerinde tutulur. Buradaki önemli avantaj aynı ```vertex``` yakın zamanda tekrardan başka bir üçgen için kullanılacak ve işlenmiş veri hala özelliştirilmiş cache içerisinde ve GPU bu veriyi önceden hesaplanmış bu verilerden getirirken çok hızlı bir şekilde getirebiliyor. Bu demek oluyorki GPU hesaplanması gereken ```vertex shader```'ın spesifik ```vertex```inin tekrar hesaplanmasını bypass ediyor ki buda perfomans olarak kazanç olarak kaydediliyor çünkü ```vertex shader``` hesaplamaları genel hesaplanılabilirlik kaynağı olarak yoğun bir şekilde kaynak harcarlar.

Buradaki ana optimizasyon stratejisi üçgenleri sıralamadan oluşuyor sonuç olarak bunlar ```index buffer``` indislerine karşılık geliyor.
Sık yapılan işlemlerde çok kullanılan verticesleri kullanarak. Bu yaklaşım GPU ```vertex cache```ine yüksek olasılıkla ```cache hit``` yapacak şekilde maksimize ediyor olur. GPU bi seri üçheni işlerken eğer ```vertex``` ardarda çizilen üçgenlerde kullanılıyorsa bu işleme alınan veri hazır bir şekilde cache üzerinde tutulur. Bu yöntem verimliliğine GPUnın  paylaşılan ```vertex```ler için önceden hesaplanmış sonuçlardan üzerinden ulaşmaşıyla gerçekleşir bu sayede sadece yeni karşılaşılan ```vertex``` ler için bu hesaplamalar yapılır. Buna karşın eğer üçgenler rastegele bi şekilde sıralanmış ise önceden hesaplanmış bir ```vertex```in cache evicted(cache üzerinden çıkarılmış) olmasına neden olabileceğinden tekrar kullanılmadan önce tüm bu hesaplamaların tekrar en baştan hesaplanması gerekebilir .

```
// Orijinal indisler basit bir mesh için
// 0---1
// | / |
// 2---3
std::vector<unsigned int> originalIndices = {
	1, 0, 2 // Birinci üçgen
	1, 3, 2 // İkinci üçgen
	// Bu sıralama cache için ideal olmayabilir
	// Eğer 1, 3, 2 verileri 0, 1, 2 den uzak ise memor üzerinde
};

// Optimize edilmiş indisler aynı dörtgen için, yeniden sıralandı daha iyi bir cache locality için
// Algoritma en son kullanılan vertexlerin (0, 1, 2) sıcak bi şekilde cache üzerinde kalmasını sağlıyor.
std::vector<unsigned int> optimizedIndices = {
	0, 1, 2,
	1, 2, 3
	// Yeniden sıralanmış 1 ve 2 paylaşılacak şekilde ilk üçgen ile birlikte
};
```

Cache üzerindeki verimliliği arttırmak için geliştiriciler genellikle ```vertex index```lerinin sırasını optimize etmek için algortimalar kullanırlar. Bu araçlar genellikle offline olarak ```asset pipeline``` içinde çalıştırılır, saf mesh verisini GPU içerisinde daha arkadaş canlısı olarak dönüştürülmesine yardımcı olarak.

Bu algoritmalardan en çok kullanılanlardan biri ```Tom Forsyth's Linear-Speed Vertex Cache Optimization```. Bu algoritma çok büyük mesh verileri üzerinde üçgen sayısını lineer bir şekilde ölçekleyerek verimli çalışacak şekilde tasarlanmış düşünülmüş ki buda modern oyunlarda karmaşık modeller için günümüzde hayati bir öneme sahip.

Forsyth algortimasının kalbinde yatan yakşalım greedy(açgözlülük) tabanlı dinamik skor sistemi. Basitleştirilmiş GPU ```vertex cache```ini(Son kullanılanlara göre, ilk giren ilk çıkar vb. yöntemlerle) "hot" veya "cold" olarak olduğunu simule eder. Her bir ```vertex``` simule edilmiş cache üzerinden yakınlığına bağlı ve en çok kullanılanlar en çok skoru alacak şekilde bi puan alırlar. Kalan üçgenelere yine küçük bi destek vererek ```cache eviction```  işleminden önce yapılmaları sağlanılmaya çalışılır. Üçgenler skoru 3 adet onu oluşturan ```vertex```lerin skorlarına bağlı olarak ağırlıklandırılır. Algoritma sürekli olarak çizilmeyen en yüksek skorlu üçgeni bularak tekrarlanır ve bu indisler optimize edilmiş çıktı listesine eklenir ve bunun sonunda simule edilmiş cache üzerinde güncelleme yapar ve ```vertex```skorlarını etkiler. Bu greedy(açgözlülük) bazlı yaklaşım algoritmanın her zaman anlık olarak en yararlı olabilecek üçgeni seçmesine yardım olmasından emin olurarak yavaşlatan ve kompleks şeçimlerden GPU'nun kaçınmasını teşvik eder.

Tom Forsyth algoritmasına ek olarak, ```vertex cache``` optimizasyonuna kayda değer bir katkısı olan Tipsify da vardır. Bu, üçgen listelerini optimize etmeyi vaat eden farklı bir stratejidir. Forsyth algoritması, tüm üçgenleri sırayla skorlayıp en iyi bir sonraki üçgeni kalan kümeden seçerek ilerlerken, Tipsify ise ```vertex-centric "fanning out"``` yaklaşımını benimser. Tipik olarak bir ```vertex``` seçilir ve bu ```vertex```i kullanan tüm üçgenler işlenir. Ardından, etkili bir şekilde ```fanning out``` yöntemi uygulanır. Yani köşenin etrafındaki üçgenler tüketildikten sonra, komşu ```vertex```lerden birine geçilerek aynı işlem tekrarlanır. Bu yerel gruplama, ```cache locality``` sağlanmasına yardımcı olur.

Tipsify’ı özel kılan özelliklerden biri de sıkça kullanılan vertex cache optimizasyonunu, ```overdraw reduction``` (fazla çizim azaltımı) ile birleştirmesidir. ```Overdraw```, aynı pikselin birden fazla üçgen tarafından tekrar tekrar çizilmesiyle oluşur ve bu durum ```GPU cycle```'larını tüketir. Tipsify, bu tür gereksiz piksel hesaplamalarını en aza indirmek için üçgenleri, görünme olasılığı yüksek olan geometrileri önceliklendirerek sıralar.

Tipsify, aynı zamanda hızlı ve ölçeklenebilir olmasıyla bilinir; özellikle büyük meshler üzerinde etkili çalışır. Hatta AMD'nin Triangle Order Optimization Tool (Tootle) sistemine entegre edilmiştir. Bu da Tipsify’ı, özellikle AMD ekosisteminde popüler bir tercih haline getiriyor.
