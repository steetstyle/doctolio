# Yazı İçeriğinin Dizaynı

Başlarken Vulkanın genel özelliklerini nasıl çalıştığına değinip başlangıçta beraber ekrana bir üçgen çizdireceğiz. Gerçekleştireceğiniz tüm bu küçük adımların ilerde daha anlamlı geleceğini temin ederim. Bu küçük adımlar ileride tüm yapbozu birleştirmemize yardımcı olacaktır. Daha sonra, lineer cebir işlemleri için GLM kütühanesi ve pencere oluşturma için GLFW ve Vulkan SDK ile geliştirme ortamını kuracağız. Yazıda, bunların Windows'ta Visual Studio ile ve Ubuntu Linux'ta GCC ile nasıl kurulacağını ele alınmayacaktır. Bu kurulum aşamalarını [bu link](https://vulkan-tutorial.com/Development_environment) üzerinden takip edip gerekli aşamaları gerçekleştirdikten sonra yazının geri kalanını okumaya devam edebilirsiniz.

Bundan sonra ekrana üçgen çizdirmek için gerekli tüm basit komponentleri Vulkan programımıza uygulayacağız. Her bölüm kabaca şu aşamaları takip edecektir:
- Yeni bir konsept ve amacını belirteceğiz.
- Programımıza entegre etmek için gerekli API çağrılarını kullanacağız.
- Soyutlamamıza yardımcı Helper fonsiyonlar yazacağız.

Yazılan her bölüm bir önceki aşamayı takip edecektir, ayrıca her bir bölümü ayrı bir yazıymış gibide okuyabilirsiniz Vulkanda belirli bir konuda bilgi sahibi olmak için. Bu da demek oluyor ki yazıyı öğrenmek için referans olarak alabilirsiniz. Tüm Vulkan typeları ve fonksiyonları api dokümasyonunda ki ilgili yere linklenecektir onlara tıklayıp daha fazla öğrenebilirsiniz. Vulkan diğerlerine kıyasla çok yeni bir api, o yüzden api dokümasyonunda çok fazla değişiklik olabilir. [Bu repo](https://github.com/KhronosGroup/Vulkan-Docs) üzerinden geri dönüşlerinizi commit olarak iletebilirsiniz.

Daha önceden bahsettiğimiz gibiö Vulkan grafik kartımız üzerinde daha fazla kontrol sahibi olmamamız için diğer apilere kıyasla daha çok parametre içeriyor. Bu yüzden basit operasyonlar örnek olarak sürekli aynı işlemlerin tekrarlanmasına yol açacaktır. O yüzden Helper fonksiyonlar üzerinden gitmeye çalışacağız.

Her bir bölümde o konuya kadar tamamlanmış kodun linki eklenecektir. Yazdığınız kodun yapısı hakkında bir şüphe varsa veya hata alıyorsanız kodu inceleyebilir ve karşılaştırabilirsiniz. Kodlar farklı markaların grafik kartlarıyla denenmiştir.

Vulkan hala çok yeni bir API'dir ve bir aşamayı uygularken henüz en iyi yöntemler henüz tam olarak oturmamıştır. 

