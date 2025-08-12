---
author: Furkan Bostancı
pubDatetime: 2025-07-20T01:37:00Z
modDatetime: 2025-07-20T01:37:00Z
title: Byte Pair Encoding Tokenizer
slug: byte-pair-encoding-tokenizer
featured: true
draft: false
tags:
  - NLP
  - Tokenization
  - Byte Pair Encoding
  - LLM
description: >
  Byte Pair Encoding (BPE) algoritmasının nasıl çalıştığını, avantajlarını ve NLP'de uygulamalarını adım adım açıklıyoruz. Minimal C++ implementasyonu ile BPE’nin mantığını öğrenin.
---

# Giriş

Byte Pair Encoding (BPE) tokenizer NLP'de kullanılan popüler bir metod subword tokenization için. Özellikle vocabulary words (kelime dağarcığı) dışında kalan kelimeler ve vocabulary size (kelime dağarcığı sayısı) boyutunu düşürmede etkili buda büyük çaplı dil modelleri için kritik bir öneme sahip. Bu yazı içinde biz BPE'in arkada hangi mekanik ile çalıştığını inceleyip, avantajlarını ve NLP üzerindeki uygulamalarını keşfedeceğiz.

BPE algoritması OpenAI tarafından yayınlanan GPT-2 paper'ı ve onla ilişkili GPT-2 code'u yayınlanktan sonra popülerleşti. Senrich et al. 2015'te NLP uygulamalarında BPE kullanımına ilişkin orjinal referenas olarak gösterilmektedir. Günümüzde tüm modern LLM'ler (GPT, Llama, Mistral vb...) bu algoritmayı kullanarak tokenizerlarını eğitiyorlar.

# BPE Nasıl çalışır?

BPE algortiması iteratif olarak veri üzerindeki en çok tekrarlanan byte çiftini (veya karakterleri) yeni bir token ile değiştirerek çalışır. Bu adım önceden tanımlanmış vocab size limitine ulaşana kadar yada daha fazla byte çifti kalmayana kadar devam eder. Bu tokenization yöntemi gelen yazı verilerini daha yoğun bir şekilde temsil etmenize olanak sağlar ve bu sayede daha az sayıda token ile daha fazla veri yakalarız.

### Adım adım inceleyelim:

1. Başlarken eğitim verisinde geçen tüm bireysel karakterlerden oluşan bir sözlükle başlayın. İstenilen hedef sözlük boyutunu seçin.

Diyelim ki aşağıdaki gibi encoded edilmiş bir verimiz olsun.

```
aaabdaaabac
```

başlangıçtaki sözlüğümüz (vocabulary) ```{a,b,c,d}```  ve indisler (idx) ```{0,1,2,3}```

2. Byte çiftleri tekrarı: Veri içindeki tekrar eden tüm komşuların sayısının tekrarını sayalım. Aşağıda örnek olarak çiftler ve verilen örnek datadaki tekrar sayıları verilmiştir.

```
aa: 4
ab: 2
ba: 1
ad: 1
da: 1
ac: 1
```

Bu dizide en çok tekrar edilen byte çifti "aa". Bu yüzden en çok tekrar edilen bu veri sözlük üzerinde kullanılmayan bir byte değeri ile değiştirelecek. Örnek olarak bu byte değerini "Z" olarak şeçelim, bu değer sonuç olarak bi sonraki kullanılabilir token indeximiz olarak atanacak. Bu işlem sonucu değiştirilmiş verimiz ve mevcut vocabulary üzerine eklenen değer aşağıdaki gibi olacak.

```
ZabdZabac
-----------------
Z=aa
```

güncellenmiş sözlüğümüz (vocabulary) ```{a,b,c,d,Z}```  ve indisler ```{0,1,2,3,4}```

Aynı işlemi güncellenmiş veri, sözlük ve indis üzerinde devam ederek yineleyelim.
En çok tekrarlanan byte çiftinden biri "ab", şimdi bunu sözlükte olmayan bir byte değeri ile değiştirelim örnek olarak "Y".

```
ZYdZYac
-----------------
Y=ab
Z=aa
```

güncellenmiş sözlüğümüz (vocabulary) ```{a,b,c,d,Z,Y}```  ve indisler ```{0,1,2,3,4,5}```

Bu işlemden sonra elimizde tekrar eden bir byte çifiti kaldığından bu işlemden sonra encoding işlmemimiz burada durabilir. Alternatif olarak buradaki işlemimize elimizde bir byte çifti kalmayana kadarda devam edebiliriz. "ZY" yi X ile değitirerekte devam edelim.

```
XdXac
-----------------
X=ZY | idx: 6
Y=ab | idx: 5
Z=aa | idx: 4
```

güncellenmiş sözlüğümüz (vocabulary) ```{a,b,c,d,Z,Y,X}```  ve indisler ```{0,1,2,3,4,5,6}```

Artık elimizdeki veri daha fazla sıkıştırılamaz çünkü daha fazla birden fazla tekrar edene bir byte çifti bulunmuyor.

Encode ettiğimiz datayı decode etmek için, aynı işlemleri ters sırada uygulayabiliriz bu sayede elimize başlangıçta verilen veri geçmiş olur tekrardan.

### Ardışık Çiftleri Sayma

Yazacağımız ilke fonksiyon ```get_consecutive_pair_count```, bir tam sayı listesi alır (tokenları temsil eden) ve bir sözlük içerisinde ardışık çiftlerin tekrarlanma sayılarını döndürür.

```
#include <iostream>
#include <vector>
#include <map>
#include <utility>

// Bir listedeki ardışık (consecutive) sayı çiftlerini sayan fonksiyon
std::map<std::pair<int, int>, int> get_consecutive_pair_count(
    const std::vector<int>& ids,
    std::map<std::pair<int, int>, int> counts = {}
) {
    // ids içindeki ardışık eleman çiftlerini dolaş
    for (size_t i = 0; i + 1 < ids.size(); ++i) {
        std::pair<int, int> pair = {ids[i], ids[i + 1]};
        counts[pair]++; // mevcutsa artır, yoksa 1 yap
    }
    return counts;
}

int main() {
    std::vector<int> ids = {1, 2, 3, 1, 2};
    auto result = get_consecutive_pair_count(ids);

    // Sonuçları yazdır
    for (const auto& entry : result) {
        std::cout << "(" << entry.first.first << ", "
                  << entry.first.second << "): "
                  << entry.second << "\n";
    }

    return 0;
}
```

### Ardışık Çiftleri Birleştirme

İkinci fonskiyon ```merge``` parametre olarak bir tam sayı listesi, birleştilecek tam sayı çifti, ve bu tam sayı çiftinin ile değiştirilecek yeni bir tam sayı alır ve bu değerlerle birleştirilmiş bir liste döndürür.

```
#include <iostream>
#include <vector>

// Bir listedeki tüm ardışık "pair" çiftlerini tek bir yeni "idx" değeriyle birleştirir
std::vector<int> merge(const std::vector<int>& ids, std::pair<int, int> pair, int idx) {
    std::vector<int> merged_ids;
    size_t i = 0;

    while (i < ids.size()) {
        // Eğer ids[i] ve bir sonraki eleman pair ile eşleşiyorsa, idx eklenir ve iki eleman atlanır
        if (i < ids.size() - 1 && ids[i] == pair.first && ids[i + 1] == pair.second) {
            merged_ids.push_back(idx);
            i += 2;
        } else {
            // Aksi halde mevcut elemanı ekle
            merged_ids.push_back(ids[i]);
            i += 1;
        }
    }

    return merged_ids;
}

int main() {
    std::vector<int> ids = {1, 2, 3, 1, 2};
    std::pair<int, int> pair = {1, 2};
    int idx = 4;

    std::vector<int> result = merge(ids, pair, idx);

    // Sonucu yazdır
    for (int id : result) {
        std::cout << id << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

### Tüm parçaları birleştirelim

Artık basit bir sınıf oluşturarak BPE algoritmasını uygulayabilriz. Başlangıç değerlerini hazırlama, eğitme ve endocing, decoding işlemleri ile birlikte.


```
class BasicTokenizer {
public:
    BasicTokenizer() {
        // Başlangıçta 256 byte'lık bir vocab
        build_vocab();
    }

    void train(const std::string& text, int vocab_size, bool verbose = false) {
        if (vocab_size < 256) {
            throw std::runtime_error("Vocab size en az 256 olmalı");
        }

        int num_merges = vocab_size - 256;

        // Metni byte listesine dönüştür
        std::vector<int> ids(text.begin(), text.end());

        // Başlangıç vocab (0-255)
        std::map<int, std::vector<unsigned char>> vocab;
        for (int i = 0; i < 256; ++i) {
            vocab[i] = {static_cast<unsigned char>(i)};
        }

        // Merge işlemleri
        for (int i = 0; i < num_merges; ++i) {
            auto pair_counts = get_consecutive_pair_count(ids);

            // En çok tekrar eden çifti bul
            auto most_common_pair = std::max_element(
                pair_counts.begin(), pair_counts.end(),
                [](const auto& a, const auto& b) {
                    return a.second < b.second;
                })->first;

            int idx = 256 + i; // Yeni token id
            ids = merge(ids, most_common_pair, idx);

            merges[most_common_pair] = idx;
            vocab[idx] = concat_bytes(vocab[most_common_pair.first], vocab[most_common_pair.second]);

            if (verbose) {
                std::cout << "[" << i + 1 << " / " << num_merges << "] "
                          << "Birleştirildi (" << most_common_pair.first << ", "
                          << most_common_pair.second << ") -> " << idx
                          << " count=" << pair_counts[most_common_pair] << "\n";
            }
        }

        this->vocab = vocab;
    }

    std::string decode(const std::vector<int>& ids) const {
        std::vector<unsigned char> text_bytes;
        for (int id : ids) {
            auto it = vocab.find(id);
            if (it != vocab.end()) {
                text_bytes.insert(text_bytes.end(), it->second.begin(), it->second.end());
            }
        }
        return std::string(text_bytes.begin(), text_bytes.end());
    }

    std::vector<int> encode(const std::string& text) const {
        std::vector<int> ids(text.begin(), text.end()); // ham byte'lar

        while (ids.size() >= 2) {
            auto stats = get_consecutive_pair_count(ids);

            // en düşük merge indexine sahip çifti bul
            auto pair = *std::min_element(
                stats.begin(), stats.end(),
                [this](const auto& a, const auto& b) {
                    return get_merge_index(a.first) < get_merge_index(b.first);
                }).first;

            // merge yoksa çık
            if (merges.find(pair) == merges.end()) {
                break;
            }

            int idx = merges.at(pair);
            ids = merge(ids, pair, idx);
        }

        return ids;
    }

private:
    std::map<std::pair<int, int>, int> merges; // (int, int) -> int
    std::map<int, std::vector<unsigned char>> vocab; // id -> bytes

    void build_vocab() {
        vocab.clear();
        for (int i = 0; i < 256; ++i) {
            vocab[i] = {static_cast<unsigned char>(i)};
        }
    }

    int get_merge_index(const std::pair<int, int>& pair) const {
        auto it = merges.find(pair);
        if (it != merges.end()) {
            return it->second;
        }
        return std::numeric_limits<int>::max(); // inf
    }

    static std::vector<unsigned char> concat_bytes(
        const std::vector<unsigned char>& a,
        const std::vector<unsigned char>& b) {
        std::vector<unsigned char> result = a;
        result.insert(result.end(), b.begin(), b.end());
        return result;
    }
};

int main() {
    // Tokenizer oluştur
    BasicTokenizer tokenizer;

    // Eğitim verisi
    std::string text = "hello hello world";

    // Tokenizer'ı eğit (örnek: vocab boyutu 270)
    tokenizer.train(text, 270, true);

    // Metni encode et
    std::vector<int> encoded = tokenizer.encode("hello world");
    std::cout << "\nEncoded IDs: ";
    for (int id : encoded) {
        std::cout << id << " ";
    }
    std::cout << "\n";

    // Encode edilen ID'leri decode et
    std::string decoded = tokenizer.decode(encoded);
    std::cout << "Decoded text: " << decoded << "\n";

    return 0;
}
```

Sade anlaşılır anlatımı için [Byte Pair Endocing Tokenizer - Ando.ai](https://blog.ando.ai/posts/bpe-tokenizer/)'a teşekkürler