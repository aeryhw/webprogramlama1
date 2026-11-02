# Biyom Atlası | Vue 3 Canlı Görsel Sunum Uygulaması

> **Ders:** Web Programlama I  
> **Kurum:** SCÜ Şarkışla UBYO — Bilişim Sistemleri ve Teknolojileri  
> **Takım Adı:** gurup  
> **Modül:** 11 — Vue.js Modules (3. Takım)  
> **Kapsam:** Render Yönetimi, Yaşam Döngüsü ve Durum/State  
> **👥 Takım Üyeleri:**  
> - Emine Avcıbaşı ([@Emine0313](https://github.com/Emine0313))  
> - Elif Yazıcı ([@aeryhw](https://github.com/aeryhw))  
> - İbrahim Kurnaz ([@krnzibrahim](https://github.com/krnzibrahim))  

---

## 🌍 Uygulama Hakkında

**Biyom Atlası**, Vue 3 CDN sürümü kullanılarak geliştirilmiş, sıfır kurulum gerektiren etkileşimli bir sunum uygulamasıdır.

Modül 11 kapsamındaki 6 temel Vue.js kavramı canlı arayüz üzerinde gösterilmektedir:

| # | Konu | Vue 3 Özelliği |
|---|------|----------------|
| 📍 1 | Koşullu Render | `v-show` / `v-if` |
| 📍 2 | Liste Render Etme | `v-for` + `computed` filtre |
| 📍 3 | İzleyiciler | `watch()` + debounce mantığı |
| 📍 4 | Şablon Referansları | `ref` + DOM ölçümü |
| 📍 5 | Yaşam Döngüsü | `onMounted` / `onUpdated` / `onUnmounted` |
| 📍 6 | Durum Yönetimi | `Vue.reactive()` singleton store |

---

## 🚀 Çalıştırma Talimatı

Uygulama **sıfır bağımlılıkla** çalışır — Node.js veya npm gerektirmez.

### Yöntem 1: Doğrudan Tarayıcıda (En Kolay)

`index.html` dosyasına çift tıklayarak Chrome, Edge veya Firefox'ta açın.

> **Not:** `vue.global.prod.js` dosyası yerel CDN yedek dosyasıdır. İnternet bağlantısı varsa uygulama otomatik olarak `unpkg.com` üzerinden Vue'yu yükler; yoksa yerel dosyayı kullanır.

### Yöntem 2: Yerel HTTP Sunucusu (Önerilen)

```bash
# Python ile
python -m http.server 8000

# veya Node.js kuruluysa
npx serve .
```

Tarayıcıda `http://localhost:8000` adresine gidin.

---

## 📁 Klasör İçeriği

```
uygulama/
├── index.html          ← Ana uygulama (Vue 3 CDN)
├── vue.global.prod.js  ← Vue 3 yerel yedek (internet yoksa kullanılır)
└── README.md           ← Bu dosya
```

---

## 📌 Teknik Notlar

- **Vue sürümü:** Vue 3 (`vue.global.prod.js` — Production build)
- **Bağımlılık:** Yok (CDN + yerel yedek)
- **Tarayıcı uyumluluğu:** Chrome 90+, Edge 90+, Firefox 88+, Safari 14+
- **`prefers-reduced-motion`:** Desteklenir — animasyonlar erişilebilirlik ayarına göre devre dışı kalır
