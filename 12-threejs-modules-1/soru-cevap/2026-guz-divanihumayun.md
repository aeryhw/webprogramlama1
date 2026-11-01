> **Uyarı:** Bu içerik, SCÜ Şarkışla UBYO Web Programlama I dersi kapsamında tamamen eğitim amaçlı çevrilmiş ve derlenmiştir. Orijinal dokümantasyon kaynakları (MDN Web Docs, Vue.js, Three.js vb.) kendi orijinal lisanslarına (CC-BY-SA, MIT) tabidir. Bu çalışmanın hiçbir ticari amacı yoktur.

---

# Soru-Cevap: 02 - Three.js Temelleri ve Sahne Grafiği

**Takım:** Dîvân-ı Hümâyun
**Dönem:** 2026 Güz  
**Konu:** Three.js, Geometriler ve Sahne Grafiği (Scene Graph)

---

### Soru 1 — WireframeGeometry ve Nokta Sayısı

**Soru:** Temel Geometriler (Primitives) bölümünde tek bir üçgen (3 nokta) bir `WireframeGeometry` nesnesine aktarıldığında, oluşan yeni geometride kaç nokta ve kaç çizgi parçası bulunur?

**Cevap:**

* **Çizgi Parçası Sayısı:** 3 çizgi parçası.
* **Nokta Sayısı:** 6 nokta (Her bir çizgi parçası bağımsız 2 noktadan oluştuğu için `3 x 2 = 6` nokta).

---

### Soru 2 — Canvas Boyutları ve Aspect Ratio

**Soru:** "Fundamentals" bölümündeki ilk "Hello Cube" örneğinde kullanılan `<canvas>` etiketinin varsayılan piksel boyutu (genişlik x yükseklik) ve buna bağlı olarak hesaplanan varsayılan aspect (en-boy oranı) değeri kaçtır?

**Cevap:**

* **Varsayılan Piksel Boyutu:** `300x150` pikseldir.
* **En-Boy Oranı (Aspect Ratio):** `2` olarak hesaplanır (`300 / 150 = 2`).

---

### Soru 3 — Sahne Grafiği ve Ölçekleme Hiyerarşisi

**Soru:** Sahne Grafiği (Scene Graph) bölümündeki güneş sistemi örneğinde, Dünya (`earthMesh`) doğrudan Güneş'in (`sunMesh`) alt elemanı (child) yapıldığında Dünya'nın boyutu ve Güneş'e olan uzaklığı neden 5 katına çıkmıştır? Bunu çözmek için kodda hangi boş düğüm (node) oluşturulmuştur?

**Cevap:**

1. **Nedeni:** `sunMesh` nesnesinin ölçeği (scale) 5 katına (`5, 5, 5`) ayarlandığı için kendi yerel alanını da (local space) 5 kat büyütmüştür. Ebeveyn-çocuk (parent-child) hiyerarşisinde çocuk düğüm ebeveynin ölçeğini devraldığından Dünya'nın boyutu ve mesafesi de 5 katına çıkmıştır.
2. **Çözüm:** `sunMesh` yerine ebeveyn olarak ölçeği büyütülmemiş, görünmez ve boş bir `solarSystem` adında `Object3D` düğümü oluşturulmuş; hem Güneş hem de Dünya bu düğümün altına eklenmiştir.

---

### Soru 4 — Render Sıralaması ve Derinlik Testi

**Soru:** Aynı düğüme hem `AxesHelper` hem de `GridHelper` eklenen `AxisGridHelper` sınıfında, ızgaranın eksenlerin üzerine çizilip onları kapatmasını önlemek için `AxesHelper` ve `GridHelper` nesnelerine sırasıyla hangi `renderOrder` değerleri verilmiştir?

**Cevap:**

* **AxesHelper `renderOrder` Değeri:** `2`
* **GridHelper `renderOrder` Değeri:** `1`
* **Ek Bilgi:** Nesnelerin/kürelerin içine gömüldüklerinde bile görünür kalmalarını sağlamak için `depthTest = false` ayarlanmıştır.

---

### Soru 5 — TextGeometry ve Pivot Noktası

**Soru:** `TextGeometry` örneğinde oluşturulan `'three.js'` metninin varsayılan olarak sol kenarından değil de tam kendi merkezi etrafında dönmesini sağlamak için sınırlayıcı kutunun (boundingBox) merkezi alındıktan sonra konuma hangi matematiksel metot ve parametre uygulanmıştır?

**Cevap:**

* **Uygulanan Metot ve Parametre:** `multiplyScalar(-1)` metodu uygulanmıştır.
* **Açıklama:** Bu işlem ile sınırlayıcı kutunun merkez vektörü tersine çevrilerek metin merkeze ötelenmiş, ardından sıfırlanmayı önlemek adına bir `Object3D` ebeveyn düğümüne bağlanarak dönme hareketi sağlanmıştır.