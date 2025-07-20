---
author: Furkan Bostancı
pubDatetime: 2025-07-20T12:00:00Z
modDatetime: 2025-07-20T12:00:00Z
title: (Vulkan Serisi) 1 - İçerik Dizaynı
slug: vulkan-serisi/icerik-dizayni
featured: false
draft: false
tags:
  - Vulkan
  - Grafik Programlama
  - GLM
  - GLFW
description: >
  Vulkan API’ye giriş, üçgen çizdirme örneği, GLM, GLFW ve Vulkan SDK kullanımı üzerine detaylı rehber.
---

Başlarken **Vulkan**’ın genel özelliklerine ve nasıl çalıştığına değinecek; başlangıçta beraber ekrana bir üçgen çizeceğiz. Bu aşamada gerçekleştireceğiniz küçük adımlar, ilerleyen bölümlerde daha anlamlı hale gelecek ve tüm yapbozu birleştirmemize yardımcı olacaktır.

Daha sonra, lineer cebir işlemleri için **GLM** kütüphanesini, pencere oluşturma için **GLFW**’yi ve gerekli geliştirme araçları için **Vulkan SDK**’yı kullanarak geliştirme ortamını kuracağız. Yazıda, bu araçların Windows’ta Visual Studio veya Ubuntu Linux’ta GCC ile nasıl kurulacağı ele alınmamaktadır. İlgili kurulum aşamalarını [bu link](https://vulkan-tutorial.com/Development_environment) üzerinden takip ederek tamamlayabilirsiniz.

## Uygulama Aşamaları

Ekrana üçgen çizdirmek için gerekli tüm temel bileşenler, Vulkan programımıza aşağıdaki adımlarla entegre edilecektir:

- Yeni bir konsept tanıtılacak ve amacı açıklanacaktır.
- Programımıza entegre etmek için gerekli **API çağrıları** uygulanacaktır.
- Tekrarlayan işlemleri soyutlamak için **helper fonksiyonlar** geliştirilecektir.

Her bölüm, kendisinden önceki aşamayı takip edecek şekilde tasarlanmıştır. Bununla birlikte, her bir bölümü bağımsız olarak da okuyabilirsiniz; böylece belirli bir konuda bilgi sahibi olmak isteyenler için referans niteliği taşır.

## Vulkan API Dokümantasyonu

Yazıda geçen tüm Vulkan türleri (*types*) ve fonksiyonları ilgili **API dokümantasyonuna** bağlantılarla yönlendirilmiştir. Vulkan nispeten yeni bir API olduğu için dokümantasyonda sık güncellemeler yaşanabilir. Geri bildirimlerinizi [Khronos Group’un resmi deposu](https://github.com/KhronosGroup/Vulkan-Docs) üzerinden iletebilirsiniz.

Vulkan, diğer grafik API’lerine kıyasla donanım üzerinde daha fazla düşük seviye kontrol sağlamaktadır. Bunun sonucu olarak, basit işlemler bile çok sayıda parametre yönetimi gerektirir ve tekrarlayan kod parçalarına yol açar. Bu nedenle, soyutlama için **helper fonksiyonlar** kullanılacaktır.

## Kod Yapısı ve İlerleme

Her bölümde, o ana kadar tamamlanmış kodun bağlantısı paylaşılacaktır. Kod yapısı hakkında emin olunmadığında veya hata alındığında, referans kodlar incelenerek karşılaştırma yapılabilir. Kodlar farklı marka ve modeldeki grafik kartlarında test edilmiştir.

Vulkan hâlâ gelişmekte olan bir API’dir; bu nedenle burada açıklanan yöntemler zamanla evrilebilir. En iyi uygulamalar henüz tam olarak standartlaşmamıştır.
