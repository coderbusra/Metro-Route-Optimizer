# Metro Rota Optimizasyon Simülasyonu 🚇

Bu proje, metro istasyonlarını graf veri yapısıyla modelleyerek **en az aktarmalı** ve **en hızlı** rotaları bulan bir simülasyon sistemidir. Ankara metrosu örneği üzerinden BFS ve A* algoritmalarını kullanarak gerçekçi rota optimizasyonu sağlar.

---

## 📚 Kullanılan Teknolojiler ve Kütüphaneler

- **Python**: Temel programlama dili.
- **heapq**: 
  - A* algoritmasında öncelik kuyruğu yönetimi için kullanıldı.
  - En düşük maliyetli yolları seçmeyi sağlar.
- **collections**:
  - `defaultdict`: Hatlara göre istasyonları gruplamak için.
  - `deque`: BFS algoritmasında kuyruk yapısı oluşturmak için.
- **Typing**: Kod okunabilirliğini artırmak için tip ipuçları (`List`, `Dict`, `Tuple`).

---

## 🧠 Algoritmaların Çalışma Mantığı

### 1. BFS (Breadth-First Search) Algoritması
- **Nasıl Çalışır?**  
  Başlangıç istasyonundan başlayarak tüm komşu istasyonları keşfeder.  
  `deque` yapısıyla **FIFO (First-In-First-Out)** mantığında çalışır.  
  Her seviyedeki tüm düğümler taranır, bu da en kısa yolu (aktarma sayısı bazında) garantiler.

- **Neden Kullanıldı?**  
  En az aktarma gerektiren rotaları bulmada etkilidir.  
  `Örnek: Kızılay → Ulus → Demetevler` rotası 0 aktarma ile bulunur.
- **Zaman Karmaşıklığı:** O(V + E) (Düğüm ve bağlantılar sayısına bağlı)

### 2. A* Algoritması
- **Nasıl Çalışır?**  
  - **Gerçek Maliyet (`g(n)`):** Mevcut yolun toplam süresi.  
  - **Tahmini Maliyet (`h(n)`):** Hedefe olan Öklid mesafesi veya hat değişim maliyeti.  
  - `heapq` ile öncelik kuyruğunda `f(n) = g(n) + h(n)` değeri en düşük olan yol seçilir.

- **Neden Bu Algoritmalar Kullanıldı?**

  -  BFS, en kısa yolu (aktarma sayısı bazında) garantili olarak bulabildiği için kullanıldı.
  -  A* algoritması**, sürelere dayalı en kısa yolu bulabildiği için tercih edildi. Dijkstra yerine A* seçilmesinin nedeni, heuristic fonksiyonuyla daha hızlı sonuca ulaşabilmesidir.  
    
    - **Zaman Karmaşıklığı:** O(E log V) (Öncelikli kuyruk nedeniyle)
---

## 🧪 Örnek Kullanım ve Test Sonuçları

### Çalıştırma Komutu
Projeyi çalıştırmak için aşağıdaki komutu kullanabilirsiniz:
```bash
python BusraDertli_MetroSimulation.py
```
### 📝 Örnek Kullanım
```bash

print("\n=== Test Senaryoları ===")
    
    # Senaryo 1: AŞTİ'den OSB'ye
    print("\n1. AŞTİ'den OSB'ye:")
    rota = metro.en_az_aktarma_bul("M1", "K4")
    if rota:
        print("En az aktarmalı rota:", " -> ".join(i.ad for i in rota))
    
    sonuc = metro.en_hizli_rota_bul("M1", "K4")
    if sonuc:
        rota, sure = sonuc
        print(f"En hızlı rota ({sure} dakika):", " -> ".join(i.ad for i in rota))
    
    # Senaryo 2: Batıkent'ten Keçiören'e
    print("\n2. Batıkent'ten Keçiören'e:")
    rota = metro.en_az_aktarma_bul("T1", "T4")
    if rota:
        print("En az aktarmalı rota:", " -> ".join(i.ad for i in rota))
    
    sonuc = metro.en_hizli_rota_bul("T1", "T4")
    if sonuc:
        rota, sure = sonuc
        print(f"En hızlı rota ({sure} dakika):", " -> ".join(i.ad for i in rota))
    
    # Senaryo 3: Keçiören'den AŞTİ'ye
    print("\n3. Keçiören'den AŞTİ'ye:")
    rota = metro.en_az_aktarma_bul("T4", "M1")
    if rota:
        print("En az aktarmalı rota:", " -> ".join(i.ad for i in rota))
    
    sonuc = metro.en_hizli_rota_bul("T4", "M1")
    if sonuc:
        rota, sure = sonuc
        print(f"En hızlı rota ({sure} dakika):", " -> ".join(i.ad for i in rota))
    
    # Senaryo 4: Kızılay'dan OSB'ye
    print("\n4. Kızılay'dan OSB'ye:")
    rota = metro.en_az_aktarma_bul("M2", "K4")
    if rota:
        print("En az aktarmalı rota:", " -> ".join(i.ad for i in rota))
    
    sonuc = metro.en_hizli_rota_bul("M2", "K4")
    if sonuc:
        rota, sure = sonuc
        print(f"En hızlı rota ({sure} dakika):", " -> ".join(i.ad for i in rota))
```



### Test Senaryolarından Örnek Çıktılar:
```bash
=== Test Senaryoları ===

1. AŞTİ'den OSB'ye:
En az aktarmalı rota: AŞTİ -> Kızılay -> Kızılay -> Ulus -> Demetevler -> OSB
En hızlı rota (25 dakika): AŞTİ -> Kızılay -> Kızılay -> Ulus -> Demetevler -> OSB

2. Batıkent'ten Keçiören'e:
En az aktarmalı rota: Batıkent -> Demetevler -> Gar -> Keçiören
En hızlı rota (21 dakika): Batıkent -> Demetevler -> Gar -> Keçiören

3. Keçiören'den AŞTİ'ye:
En az aktarmalı rota: Keçiören -> Gar -> Gar -> Sıhhiye -> Kızılay -> AŞTİ
En hızlı rota (19 dakika): Keçiören -> Gar -> Gar -> Sıhhiye -> Kızılay -> AŞTİ

4. Kızılay'dan OSB'ye:
En az aktarmalı rota: Kızılay -> Kızılay -> Ulus -> Demetevler -> OSB
En hızlı rota (20 dakika): Kızılay -> Kızılay -> Ulus -> Demetevler -> OSB

```
---

## 🚀 Projeyi Geliştirme Fikirleri

- **Aktarma Optimizasyonu**
  - en_az_aktarma_bul metodunu, hat değişimlerini sayarak optimize eden bir versiyon ekleme.

- **Grafiksel Arayüz**
  - matplotlib veya PyQt5 ile kullanıcı dostu bir arayüz tasarlama.

- **Gerçek Zamanlı Veri Entegrasyonu**
  - Ankara Metro API'leriyle entegre çalışacak şekilde genişletme.

- **Performans İyileştirmeleri**

  - Büyük veri setleri için NumPy tabanlı optimizasyonlar yapma.

- **Kesik Bağlantı Tespiti**

  - Rota bulunamazsa alternatif yollar öneren bir sistem ekleme.
