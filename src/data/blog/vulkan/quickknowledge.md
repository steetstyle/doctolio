---
author: Furkan Bostancı
pubDatetime: 2025-07-20T13:00:00Z
modDatetime: 2025-07-20T13:00:00Z
title: (Vulkan Serisi) 2 - Giriş
slug: vulkan-serisi/giris
featured: false
draft: false
tags:
  - Vulkan
  - Grafik Programlama
  - GLM
  - GLFW
description: >
  Vulkan API’nin temel prensiplerini, avantajlarını, dezavantajlarını ve uygulama adımlarını anlatan rehber.
---

## Önsöz

Bu yazı size **Vulkan API**’sini kullanmanın temelini ve prensiplerini anlatacaktır. Vulkan, modern grafik kartları için düşük seviyeli kontrol ve daha az sürücü sürprizi sağlayan, Khronos Group tarafından geliştirilen bir grafik API’sidir. Bu arayüz sayesinde uygulamalar, **OpenGL** veya **Direct3D** gibi API’lere kıyasla daha fazla soyutlama yaparak daha yüksek performans elde edebilir.

Vulkan’ın arkasındaki fikirler **Direct3D** ve **Metal** ile benzerlik göstermektedir; fakat Vulkan **cross-platform** olduğu için çok daha geniş bir işletim sistemi yelpazesinde çalıştırma olanağı sunar.

Ancak bu avantajların bir bedeli vardır: Daha fazla performans, API’nin tüm detaylarının uygulama tarafından manuel olarak yönetilmesi anlamına gelir. Bu sebeple Vulkan, yüksek performanslı grafikler konusunda hevesli ve çaba harcamaya istekli geliştiricileri hedefler. Eğer oyun geliştirmeye odaklıysanız, OpenGL veya Direct3D gibi API’leri kullanmak hâlâ geçerli bir seçenek olmaya devam etmektedir. Alternatif olarak, **Unreal Engine** veya **Unity** gibi oyun motorları üzerinden Vulkan kullanımını da tercih edebilirsiniz.

### Bu yazıyı verimli kullanabilmeniz için gerekli ön bilgi ve araçlar:
- Vulkan (NVIDIA, AMD, Intel, Apple Silicon) ile uyumlu bir grafik kartı ve sürücü
- C++ deneyimi (RAII ve initializer list’lere aşinalık)
- C++17 özelliklerini destekleyen bir derleyici (Visual Studio 2017+, GCC 7+ veya Clang 5+)
- 3D bilgisayar grafikleriyle ilgili temel bilgi

Bu yazıda **OpenGL** veya **Direct3D**’ye hâkim olmanız beklenmemektedir, fakat 3D grafiklerin bilgisayarda nasıl çizdirildiğine dair temel bilgi yararlı olacaktır. Perspektif projeksiyon gibi konular detaylı olarak ele alınmayacak, bunun yerine gerekli olduğunda öğrenebileceğiniz kaynaklar önerilecektir.

C yerine C++ kullanılması tavsiye edilmektedir; çünkü mantık ve kaynak yaşam sürelerini yönetmek için **sınıflar** ve **RAII** gibi C++ özellikleri kullanılacaktır. Ancak C dilini tercih edenler, kendi lineer cebir kütüphanelerini ve bellek yönetimini sağlamakla yükümlüdür.  

Vulkan API ile çalışırken **orijinal C API’si** kullanılacaktır; fakat C++ kullanan geliştiriciler, hataları önlemeye yardımcı olan **Vulkan-Hpp binding**’lerinden de yararlanabilirler.

---

## Yazı İçeriğinin Dizaynı

İlk olarak Vulkan’ın genel özelliklerine ve nasıl çalıştığına değinecek, ardından ekrana bir üçgen çizeceğiz. Gerçekleştireceğiniz bu küçük adımlar ilerleyen bölümlerde daha anlamlı hâle gelecek ve tüm yapbozun birleştirilmesine yardımcı olacaktır.

Daha sonra, lineer cebir işlemleri için **GLM**, pencere oluşturma için **GLFW**, ve geliştirme ortamı için **Vulkan SDK** kullanılacaktır. Bu yazıda bu araçların Windows (Visual Studio) veya Ubuntu Linux (GCC) üzerinde kurulumları anlatılmayacaktır; detaylar için [bu bağlantıya](https://vulkan-tutorial.com/Development_environment) bakabilirsiniz.

### Uygulama Aşamaları

Ekrana üçgen çizdirmek için gerekli temel bileşenler aşağıdaki adımlarla Vulkan programına entegre edilecektir:

- Yeni bir konseptin amacı açıklanacaktır.
- Programımıza entegre etmek için gerekli **API çağrıları** yapılacaktır.
- Tekrarlayan işlemleri soyutlamak için **helper fonksiyonlar** oluşturulacaktır.

Her bölüm, bir öncekinin üzerine inşa edilecek şekilde tasarlanmıştır. Bununla birlikte, herhangi bir bölümü bağımsız olarak okuyabilir; belirli bir konuya referans olarak kullanabilirsiniz.

### Vulkan API Dokümantasyonu

Yazıda geçen tüm Vulkan türleri (*types*) ve fonksiyonları, ilgili **API dokümantasyonu**na bağlantılarla yönlendirilmiştir. Vulkan nispeten yeni bir API olduğundan dokümantasyonda sık güncellemeler olabilir. Geri bildirimlerinizi [Khronos Group’un resmi deposu](https://github.com/KhronosGroup/Vulkan-Docs) üzerinden iletebilirsiniz.

Vulkan, diğer grafik API’lerine göre donanım üzerinde daha düşük seviyeli kontrol sunar. Basit işlemler bile çok sayıda parametre yönetimi gerektirir; bu nedenle tekrarları azaltmak için **helper fonksiyonlar** kullanılacaktır.

### Kod Yapısı ve İlerleme

Her bölümde, o ana kadar tamamlanmış kodun bağlantısı paylaşılacaktır. Kod yapısı hakkında emin olunmadığında veya hata alındığında referans kodlar incelenerek karşılaştırma yapılabilir. Kodlar, farklı marka ve modeldeki grafik kartlarında test edilmiştir.

---

## Sonuç

Vulkan hâlen gelişmekte olan bir API’dir; burada anlatılan yöntemler zamanla evrilebilir ve en iyi uygulamalar henüz tam olarak standartlaşmamıştır.
