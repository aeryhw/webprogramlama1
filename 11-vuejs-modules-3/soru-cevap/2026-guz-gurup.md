> **Uyarı:** Bu içerik, SCÜ Şarkışla UBYO Web Programlama I dersi kapsamında tamamen eğitim amaçlı çevrilmiş ve derlenmiştir. Orijinal dokümantasyon kaynakları (MDN Web Docs, Vue.js, Three.js vb.) kendi orijinal lisanslarına (CC-BY-SA, MIT) tabidir. Bu çalışmanın hiçbir ticari amacı yoktur.

# Vue.js 3: 6 Klasik Soru ve Cevap Havuzu (Pinia Temelleri Dahil)

* **Ders:** Web Programlama I  
* **Bölüm:** SCÜ Şarkışla UBYO — Bilişim Sistemleri ve Teknolojileri  
* **Modül:** 11 — Vue.js Modules (3. Takım)  
* **Konu:** Render Yönetimi, Yaşam Döngüsü ve Durum/State (Koşullu Render, Liste Render, İzleyiciler, Şablon Referansları, Yaşam Döngüsü, Durum Yönetimi)  
* **Takım Adı:** gurup  
* **👥 Takım Üyeleri:**  
  * Emine Avcıbaşı ([@Emine0313](https://github.com/Emine0313))  
  * Elif Yazıcı ([@aeryhw](https://github.com/aeryhw))  
  * İbrahim Kurnaz ([@krnzibrahim](https://github.com/krnzibrahim))  

---

## Soru 1: Bu izleyici neden hiç çalışmıyor? (bozuk kod)

```js
const obj = reactive({ count: 0 })

watch(obj.count, (yeni) => {
  console.log(yeni)
})

obj.count++
```

**Cevap:** `obj.count` yalnızca `0` sayısını iletir, reaktiflik kaybolur. Bu yüzden izleyici hiç tetiklenmez. Doğrusu bir fonksiyon (getter) vermektir:

```js
watch(() => obj.count, (yeni) => {
  console.log(yeni)
})
```

---

## Soru 2: Bir öğeyi koşula göre göstermenin iki yolu arasındaki fark nedir? Hangisi ne zaman seçilir?

**Cevap:** `v-if` öğeyi gerçekten oluşturur ve yok eder. Başlangıçta koşul yanlışsa hiçbir şey çizmez. `v-show` ise öğeyi her zaman çizer, yalnızca CSS ile gizler (`display: none`). `v-if`'in geçiş maliyeti (toggle cost), `v-show`'un başlangıç maliyeti (initial render cost) yüksektir. Sık açılıp kapanan öğelerde `v-show`, koşulun nadiren değiştiği yerlerde `v-if` seçilir.

---

## Soru 3: `v-for` ile liste çizerken her öğeye neden benzersiz bir `key` verilir? `v-if` ile aynı öğede kullanmak neden sorun çıkarır?

**Cevap:** Vue varsayılan olarak öğeleri yerinde günceller (*in-place patch*). Benzersiz ve kararlı bir `key` (örneğin `id`) yoksa liste sırası değişince form girdileri ve alt bileşen durumu yanlış öğeye kayabilir. `v-if`, `v-for`'dan önce değerlendirildiği için döngü değişkenine ulaşamaz ve hata verir. Çözüm, öğeyi `<template v-for>` ile sarmak veya listeyi `computed` ile önceden süzmektir.

---

## Soru 4: Bu kodda bileşen kaldırılınca ne sorun çıkar? (kod okuma)

```vue
<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  setInterval(() => {
    console.log('tik')
  }, 1000)
})
</script>
```

**Cevap:** Bileşen kaldırılsa bile zamanlayıcı çalışmaya devam eder ve bellek sızıntısına (memory leak) yol açar. Temizlik `onUnmounted` içinde yapılmalıdır:

```js
let sayac
onMounted(() => { 
  sayac = setInterval(() => console.log('tik'), 1000) 
})

onUnmounted(() => {
  clearInterval(sayac)
})
```

---

## Soru 5: Birçok bileşenin aynı veriyi kullanması gerekirse ne yapılır? Neden?

**Cevap:** Veriyi ortak bir üst bileşene taşıyıp aşağı aktarmak, bileşen ağacı derinleştikçe zorlaşır (prop drilling). Bunun yerine veri bileşenlerin dışında ortak bir depoda (store) tutulur ve her bileşen oradan okur. Küçük projelerde `reactive()` ile yazılmış basit bir depo yeterlidir. Büyük projelerde ve sunucu tarafı çizimde (SSR) resmi çözüm Pinia'dır; çünkü basit depo istekler arasında durum kirlenmesine (cross-request state pollution) yol açabilir.

---

## Soru 6: Pinia ile bir store nasıl tanımlanır? `storeToRefs` neden gereklidir?

```js
// src/stores/counter.js
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'

export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)
  const doubled = computed(() => count.value * 2)
  function increment() { count.value++ }
  return { count, doubled, increment }
})
```

```vue
<!-- Component.vue -->
<script setup>
import { useCounterStore } from '@/stores/counter'
import { storeToRefs } from 'pinia'

const store = useCounterStore()
const { count, doubled } = storeToRefs(store) // ✅ reaktivite korunur
const { increment } = store                    // fonksiyonlar doğrudan alınır
</script>
```

**Cevap:** Pinia'da store, `defineStore(id, setupFn)` ile tanımlanır; Setup Store sözdizimi `<script setup>` ile birebir aynıdır. Bileşende store'u `const store = useCounterStore()` ile çağırıp state ve computed özelliklerini parçalamak için **`storeToRefs()`** kullanılmalıdır. Doğrudan destructuring (`const { count } = store`) yapılırsa reaktivite kaybolur ve `count` değiştiğinde bileşen yeniden render edilmez. Fonksiyonlar reaktif olmadığı için `storeToRefs` olmadan doğrudan alınabilir. Pinia'nın Vuex'e göre avantajları: mutations yoktur, TypeScript desteği otomatiktir ve DevTools entegrasyonu yerleşiktir.
