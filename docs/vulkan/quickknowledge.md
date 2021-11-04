
# Önsöz
Bu yazı size Vulkan apisini kullanmanın temelini ve prensiplerini anlatacaktır. Vulkan modern grafik kartları için daha çok soyutlama katmanı sağlayan Khronos Group tarafından geliştirilen bir grafik kartı apisidir. Bu interface aracılığıyla uygulamamızda OpenGL veya Direct3D gibi apilere kıyasla daha çok soyutlama yapabileceğimizden daha fazla performans ve daha az supriz sürücü ile karşılaştırackatır. Vulkanın arkasındaki fikirler Direct3D ve Metal inkine benzer fakat Vulkan Cross-platform olduğu için aynı anda daha fazla işletim sisteminde çalıştırmamıza olanak sağlar.


Ancak bu avantajlar tabiki doğası gereği "price you pay" yani meali ne kadar ödersen okadar karşılığını alırsın. Bu da demek oluyor ki diğer grafik apilere kıyasla daha çok çalışmamız anlamına geliyor. API ile ilgili her ayrıntının uygulamamız tarafından en baştan ayarlanması gerekiyor.

Buradan anlamanız gereken mesaj Vulkan herkes için olmadığıdır. Burada hedeflenen kitle yüksek performanslı grafikler konusunda hevesli ve biraz çalışmaya istekli programcıları hedefliyor. Bilgisayar grafikleri yerine oyun geliştirmeyle daha çok ilgileniyorsanız OpenGL veya Direct3D gibi apileri kullanmayıda tercih edebilirsiniz yakın zamanda deprecated olmayacağının garantisini verebilirim. Diğer bir şeçenekse Unreal Engine ve Unity gibi oyun motorlarında Vulkanı kullanmak.

Bu yazıyı verimli bir şekilde kendi lehinize kullanmanızö bilmeniz veyahut aşağıdakilere sahip olmanız işe yaracaktır:

- Vulkan (NVIDIA, AMD, Intel, Apple Silicon (Veya Apple M1)) ile uyumlu bir grafik kartı ve sürücü
- C++ ile deneyim (RAII ile aşinalık, Initializer Lists)
- C++ 17 özelliklerinin yeterli desteğine sahip bir derleyici (Visual Studio 2017+, GCC 7+ veya Clang 5+)
- 3D bilgisayar grafikleriyle ilgili biraz deneyim

Bu yazıda OpenGL veya Direct3D gibi kütüphanelerini önceden kullandığınızı veya bildiğinizi varsaymayacağız ancak 3D grafiklerin bilgisayarda nasıl çizdirildiğine dair biraz bilgi sahibi olmanızı gerektirir. Çünkü örnek olarak perspektif projeksiyonunun arkasındaki matematiği açıklamayacağız bunun yerine bunları ögrenebileceğiniz kaynaklar bulup tekrardan gelmenizi öneririz.

İsterseniz C++ yerine C kullanabilirsiniz ancak farklı bir lineer cebir kütüphanesi kullanmanız gerekecek ve kod yapılanması konusunda kendi başınıza olacaksınız. Mantık ve kaynak yaşam sürelerini düzenlemek için sınıflar ve RAII gibi C++ özelliklerini kullanacağız

Diğer programlama dillerini kullanan geliştiricilerin takip etmesini kolaylaştırmak ve temel API ile biraz deneyim kazanmak için Vulkan ile çalışmak için orijinal C API'sini kullanacağız. Ancak C ++ kullanıyorsanız, bazı kirli işleri soyutlayan ve belirli hata sınıflarını önlemeye yardımcı olan daha yeni Vulkan-Hpp bindinglerini kullanmayı tercih edebilirsiniz.