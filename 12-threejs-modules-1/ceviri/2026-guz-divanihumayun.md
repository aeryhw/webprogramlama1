# three.js Kılavuzu

## İçindekiler

- [1. Installation (Kurulum)](#1-installation-kurulum)
  - [Project structure (Proje yapısı)](#project-structure-proje-yapısı)
  - [Seçenek 1: NPM ve bir derleme aracıyla kurulum](#seçenek-1-npm-ve-bir-derleme-aracıyla-kurulum)
    - [NPM Geliştirme (Development)](#npm-geliştirme-development)
    - [NPM Yayın (Production)](#npm-yayın-production)
  - [Seçenek 2: CDN'den içe aktarma](#seçenek-2-cdnden-içe-aktarma)
    - [CDN Geliştirme (Development)](#cdn-geliştirme-development)
    - [Daha fazla yerel sunucu (More local servers)](#daha-fazla-yerel-sunucu-more-local-servers)
    - [CDN Yayın (Production)](#cdn-yayın-production)
  - [Addons (Eklentiler)](#addons-eklentiler)
  - [Next Steps (Sonraki adımlar)](#next-steps-sonraki-adımlar)
- [2. Creating a Scene (Sahne oluşturma)](#2-creating-a-scene-sahne-oluşturma)
  - [Before we start (Başlamadan önce)](#before-we-start-başlamadan-önce)
  - [Creating the scene (Sahneyi oluşturma)](#creating-the-scene-sahneyi-oluşturma)
  - [Rendering the scene (Sahneyi render etme)](#rendering-the-scene-sahneyi-render-etme)
  - [Animating the cube (Küpü canlandırma)](#animating-the-cube-küpü-canlandırma)
  - [The result (Sonuç)](#the-result-sonuç)
- [3. Fundamentals (Temeller)](#3-fundamentals-temeller)
  - [Three.js ve WebGL](#threejs-ve-webgl)
  - [Bir three.js uygulamasının yapısı](#bir-threejs-uygulamasının-yapısı)
  - [Three.js'i yükleme ve canvas hazırlama](#threejsi-yükleme-ve-canvas-hazırlama)
  - [Kamera ve frustum](#kamera-ve-frustum)
  - [Scene, geometri, malzeme ve Mesh](#scene-geometri-malzeme-ve-mesh)
  - [Render döngüsü ve animasyon](#render-döngüsü-ve-animasyon)
  - [Aydınlatma ekleme](#aydınlatma-ekleme)
  - [Üç küp ile örnek](#üç-küp-ile-örnek)
  - [es6 modules, three.js ve klasör yapısı](#es6-modules-threejs-ve-klasör-yapısı)
- [4. Primitives (Temel Geometriler)](#4-primitives-temel-geometriler)
  - [Temel geometrilere giriş](#temel-geometrilere-giriş)
  - [EdgesGeometry ve WireframeGeometry](#edgesgeometry-ve-wireframegeometry)
  - [Arka plan rengi ve kamera ayarları](#arka-plan-rengi-ve-kamera-ayarları)
  - [addObject ve createMaterial fonksiyonları](#addobject-ve-creatematerial-fonksiyonları)
  - [addSolidGeometry ve kutu örneği](#addsolidgeometry-ve-kutu-örneği)
  - [Özel durum: TextGeometry](#özel-durum-textgeometry)
  - [Özel durum: çizgi tabanlı geometriler](#özel-durum-çizgi-tabanlı-geometriler)
  - [Points ve PointsMaterial](#points-ve-pointsmaterial)
  - [Alt bölme ayarları](#alt-bölme-ayarları)
- [5. Scene Graph (Sahne Grafiği)](#5-scene-graph-sahne-grafiği)
  - [Sahne grafiği nedir?](#sahne-grafiği-nedir)
  - [Güneş sistemi örneği](#güneş-sistemi-örneği)
  - [Yardımcılar: AxesHelper ve GridHelper](#yardımcılar-axeshelper-ve-gridhelper)
  - [AxisGridHelper sınıfı ve lil-gui](#axisgridhelper-sınıfı-ve-lil-gui)
  - [Tank örneği](#tank-örneği)
  - [Sonuç ve değerlendirme](#sonuç-ve-değerlendirme)
- [6. Özet](#özet)

---

## 1. Installation (Kurulum)

### Project structure (Proje yapısı)

Her three.js projesinin web sayfasını tanımlayan en az bir HTML dosyasına ve three.js kodunuzu çalıştıran bir JavaScript dosyasına ihtiyacı vardır. Aşağıdaki yapı ve isimlendirme tercihleri zorunlu değildir, ancak bu rehber boyunca tutarlılık için kullanılacaktır.

- `index.html`

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="utf-8">
      <title>My first three.js app</title>
      <style>
        body { margin: 0; }
      </style>
    </head>
    <body>
      <script type="module" src="/main.js"></script>
    </body>
  </html>
  ```

- `main.js`

  ```javascript
  import * as THREE from 'three';

  ...
  ```

- `public/`
  - `public/` klasörü bazen "static" (statik) klasör olarak da adlandırılır, çünkü içindeki dosyalar web sitesine değiştirilmeden gönderilir. Dokular, ses ve 3D modeller genellikle buraya konur.

Temel proje yapısını oluşturduğumuza göre, projeyi yerel olarak çalıştırmak ve bir web tarayıcısı üzerinden erişmek için bir yönteme ihtiyacımız var. Kurulum ve yerel geliştirme, npm ve bir derleme aracıyla ya da three.js'i bir CDN'den içe aktararak yapılabilir. Her iki seçenek de aşağıdaki bölümlerde açıklanmıştır.

### Seçenek 1: NPM ve bir derleme aracıyla kurulum

#### NPM Geliştirme (Development)

[npm paket kayıt defterinden](https://www.npmjs.com/) kurulum yapmak ve bir [derleme aracı](https://eloquentjavascript.net/10_modules.html#h_zWTXAU93DC) kullanmak, çoğu kullanıcı için önerilen yaklaşımdır — projenizin ihtiyaç duyduğu bağımlılık sayısı arttıkça, statik barındırmanın kolayca çözemeyeceği sorunlarla karşılaşma olasılığınız da artar. Bir derleme aracıyla, yerel JavaScript dosyalarını ve npm paketlerini içe aktarmak import map kullanmadan, kutudan çıktığı gibi çalışmalıdır.

1. [Node.js](https://nodejs.org/)'i kurun. Bağımlılıkları yönetmek ve derleme aracımızı çalıştırmak için buna ihtiyacımız olacak.
2. Proje klasörünüzde bir [terminal](https://www.joshwcomeau.com/javascript/terminal-for-js-devs/) kullanarak three.js'i ve bir derleme aracı olan [Vite](https://vitejs.dev/)'i kurun. Vite geliştirme sırasında kullanılacaktır, ancak nihai web sayfasının parçası değildir. Başka bir derleme aracını tercih ederseniz sorun değil — [ES Modüllerini](https://eloquentjavascript.net/10_modules.html#h_zWTXAU93DC) içe aktarabilen modern derleme araçlarını destekliyoruz.

   ```bash
   # three.js
   npm install --save three

   # vite
   npm install --save-dev vite
   ```

   _Kurulum projeme node_modules/ ve package.json ekledi. Bunlar nedir?_
   _three.js'i TypeScript ile kullanma_

3. Terminalinizden şunu çalıştırın:

   ```bash
   npx vite
   ```

   _npx nedir?_

4. Her şey yolunda gittiyse, terminalinizde `http://localhost:5173` gibi bir URL göreceksiniz ve web uygulamanızı görmek için bu URL'yi açabilirsiniz.

Sayfa boş olacak — bir [sahne oluşturmaya](https://threejs.org/manual/en/creating-a-scene.html) hazırsınız.

Devam etmeden önce bu araçlar hakkında daha fazla bilgi edinmek isterseniz şunlara bakın:

- [three.js journey: Local Server](https://threejs-journey.com/lessons/local-server) (Yerel Sunucu)
- [Vite: Command Line Interface](https://vitejs.dev/guide/cli.html) (Komut Satırı Arayüzü)
- [MDN: Package management basics](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Understanding_client-side_tools/Package_management) (Paket yönetimi temelleri)

#### NPM Yayın (Production)

Daha sonra, web uygulamanızı yayına almaya hazır olduğunuzda, Vite'a bir üretim derlemesi çalıştırmasını söylemeniz yeterlidir — `npx vite build`. Uygulamanın kullandığı her şey derlenecek, optimize edilecek ve `dist/` klasörüne kopyalanacaktır. Bu klasörün içeriği web sitenizde barındırılmaya hazırdır.

### Seçenek 2: CDN'den içe aktarma

#### CDN Geliştirme (Development)

Derleme araçları olmadan kurulum, yukarıda verilen proje yapısında bazı değişiklikler gerektirecektir.

1. `main.js` içinde `'three'` (bir npm paketi) adresinden kod içe aktardık ve web tarayıcıları bunun ne anlama geldiğini bilmez. `index.html` içine, paketin nereden alınacağını tanımlayan bir [import map](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type/importmap) eklememiz gerekecek. Aşağıdaki kodu `<head></head>` etiketinin içine, stillerden sonra koyun.

   ```html
   <script type="importmap">
   {
     "imports": {
       "three": "https://cdn.jsdelivr.net/npm/three@<version>/build/three.module.js",
       "three/addons/": "https://cdn.jsdelivr.net/npm/three@<version>/examples/jsm/"
     }
   }
   </script>
   ```

   `<version>` yerine three.js'in gerçek bir sürümünü, örneğin `"v0.149.0"` gibi, yazmayı unutmayın. En güncel sürüm [npm sürüm listesinde](https://www.npmjs.com/package/three?activeTab=versions) bulunabilir.

2. Ayrıca bu dosyaları web tarayıcısının erişebileceği bir URL'de barındırmak için bir yerel sunucu çalıştırmamız gerekecek. Teknik olarak bir HTML dosyasına çift tıklayıp tarayıcınızda açmak mümkün olsa da, daha sonra uygulayacağımız önemli özellikler, güvenlik nedenleriyle sayfa bu şekilde açıldığında çalışmaz.

   [Node.js](https://nodejs.org/)'i kurun, ardından projenin dizininde bir yerel sunucu başlatmak için [serve](https://www.npmjs.com/package/serve)'i çalıştırın:

   ```bash
   npx serve .
   ```

3. Her şey yolunda gittiyse, terminalinizde `http://localhost:3000` gibi bir URL göreceksiniz ve web uygulamanızı görmek için bu URL'yi açabilirsiniz.

Sayfa boş olacak — bir [sahne oluşturmaya](https://threejs.org/manual/en/creating-a-scene.html) hazırsınız.

Başka birçok yerel statik sunucu da mevcut — bazıları Node.js yerine farklı diller kullanır, bazıları ise masaüstü uygulamalarıdır. Hepsi temelde aynı şekilde çalışır ve aşağıda birkaç alternatif sunduk.

#### Daha fazla yerel sunucu (More local servers)

**Command Line (Komut Satırı)**

Komut satırı yerel sunucuları bir terminal penceresinden çalışır. İlgili programlama dilinin önce kurulması gerekebilir.

- `npx http-server` (Node.js)
- `npx five-server` (Node.js)
- `python -m SimpleHTTPServer` (Python 2.x)
- `python -m http.server` (Python 3.x)
- `php -S localhost:8000` (PHP 5.4+)

**GUI (Grafik Arayüz)**

Grafik arayüzlü yerel sunucular bilgisayarınızda bir uygulama penceresi olarak çalışır ve bir kullanıcı arayüzüne sahip olabilir.

- [Servez](https://greggman.github.io/servez)

**Code Editor Plugins (Kod Editörü Eklentileri)**

Bazı kod editörlerinde, istek üzerine basit bir sunucu başlatan eklentiler bulunur.

- Visual Studio Code için [Five Server](https://marketplace.visualstudio.com/items?itemName=yandeu.five-server)
- Visual Studio Code için [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)
- Atom için [Live Server](https://atom.io/packages/atom-live-server)

#### CDN Yayın (Production)

Web uygulamanızı yayına almaya hazır olduğunuzda, kaynak dosyalarınızı web barındırma sağlayıcınıza gönderin — hiçbir şeyi derlemeye gerek yok. Bu takasın dezavantajı, import map'i uygulamanızın ihtiyaç duyduğu tüm bağımlılıklarla (ve bağımlılıkların bağımlılıklarıyla!) güncel tutmaya dikkat etmeniz gerekmesidir. Bağımlılıklarınızı barındıran CDN geçici olarak kapanırsa, web siteniz de çalışmayı durduracaktır.

**ÖNEMLİ:** Tüm bağımlılıkları three.js'in aynı sürümünden ve aynı CDN'den içe aktarın. Farklı kaynaklardan dosyaları karıştırmak, kodun mükerrer eklenmesine ya da uygulamanın beklenmedik şekillerde bozulmasına neden olabilir.

### Addons (Eklentiler)

Kutudan çıktığı haliyle three.js, bir 3D motorunun temellerini içerir. Kontroller, yükleyiciler ve son işleme efektleri gibi diğer three.js bileşenleri [`addons/`](https://github.com/mrdoob/three.js/tree/dev/examples/jsm) dizininin parçasıdır. Eklentilerin ayrıca kurulması gerekmez, ancak ayrıca içe aktarılmaları gerekir.

Aşağıdaki örnek, three.js'in `OrbitControls` ve `GLTFLoader` eklentileriyle nasıl içe aktarılacağını gösterir. Gerektiğinde bu, her eklentinin belgelerinde veya örneklerinde de belirtilecektir.

```javascript
import * as THREE from 'three';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';

const controls = new OrbitControls( camera, renderer.domElement );
const loader = new GLTFLoader();
```

three.js için bazı mükemmel üçüncü taraf projeler de mevcut. Bunların ayrıca kurulması gerekir — bkz. [Libraries and Plugins](https://threejs.org/manual/en/libraries-and-plugins.html) (Kütüphaneler ve Eklentiler).

### Next Steps (Sonraki adımlar)

Artık bir [sahne oluşturmaya](https://threejs.org/manual/en/creating-a-scene.html) hazırsınız.

Kaynak: [three.js kılavuzu – Installation](https://threejs.org/manual/en/installation.html). Türkçe çeviri.

---

## 2. Creating a Scene (Sahne oluşturma)

Bu bölümün amacı three.js'e kısa bir giriş yapmaktır. Dönen bir küp içeren bir sahne kurarak başlayacağız. Takılırsanız ve yardıma ihtiyaç duyarsanız diye sayfanın altında çalışan bir örnek verilmiştir.

### Before we start (Başlamadan önce)

Henüz yapmadıysanız, [Kurulum kılavuzunu (Installation guide)](https://threejs.org/docs/#manual/en/introduction/Installation) inceleyin. Aynı proje yapısını (`index.html` ve `main.js` dahil) kurduğunuzu, three.js'i yüklediğinizi ve ya bir derleme aracı (build tool) çalıştırdığınızı ya da CDN ve import map'ler ile yerel bir sunucu kullandığınızı varsayacağız.

### Creating the scene (Sahneyi oluşturma)

three.js ile bir şey görüntüleyebilmek için üç şeye ihtiyacımız var: sahneyi kamera ile render edebilmemiz için sahne (scene), kamera (camera) ve render edici (renderer).

**main.js**

```javascript
import * as THREE from 'three';

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );

const renderer = new THREE.WebGLRenderer();
renderer.setSize( window.innerWidth, window.innerHeight );
document.body.appendChild( renderer.domElement );
```

Burada neler olduğunu biraz açıklayalım. Sahneyi, kameramızı ve render ediciyi kurmuş olduk.

three.js'te birkaç farklı kamera vardır. Şimdilik bir `PerspectiveCamera` kullanalım.

İlk öznitelik görüş alanıdır (field of view). FOV, herhangi bir anda ekranda görülen sahne kapsamıdır. Değer derece cinsindendir.

İkincisi en-boy oranıdır (aspect ratio). Neredeyse her zaman elementin genişliğinin yüksekliğine bölümünü kullanmak istersiniz; aksi halde eski filmleri geniş ekran bir televizyonda oynattığınızdaki sonucu alırsınız - görüntü sıkışmış görünür.

Sonraki iki öznitelik yakın ve uzak kırpma düzlemleridir (near and far clipping plane). Bunun anlamı, kameradan `far` değerinden daha uzakta ya da `near` değerinden daha yakında olan nesnelerin render edilmeyeceğidir. Şimdilik bunun için endişelenmenize gerek yok, ancak daha iyi performans için uygulamalarınızda başka değerler kullanmak isteyebilirsiniz.

Sırada render edici var. Render edici örneğini oluşturmanın yanı sıra, uygulamamızı hangi boyutta render etmesini istediğimizi de ayarlamamız gerekir. Uygulamamızla doldurmak istediğimiz alanın genişlik ve yüksekliğini kullanmak iyi bir fikirdir - bu durumda tarayıcı penceresinin genişliği ve yüksekliği. Performans yoğun uygulamalarda `setSize`'a `window.innerWidth/2` ve `window.innerHeight/2` gibi daha küçük değerler de verebilirsiniz; bu, uygulamanın dörtte bir boyutta render edilmesini sağlar.

Uygulamanızın boyutunu koruyup daha düşük çözünürlükte render etmek isterseniz, `setSize`'ı `updateStyle` olarak (üçüncü argüman) `false` ile çağırarak bunu yapabilirsiniz. Örneğin, `<canvas>`'ınızın genişliği ve yüksekliği %100 ise, `setSize(window.innerWidth/2, window.innerHeight/2, false)` uygulamanızı yarı çözünürlükte render edecektir.

Son olarak, render edici elementini HTML belgemize ekliyoruz. Bu, render edicinin sahneyi bize göstermek için kullandığı bir `<canvas>` elementidir.

"Hepsi güzel, peki söz verdiğin küp nerede?" Şimdi ekleyelim.

```javascript
const geometry = new THREE.BoxGeometry( 1, 1, 1 );
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
const cube = new THREE.Mesh( geometry, material );
scene.add( cube );

camera.position.z = 5;
```

Bir küp oluşturmak için bir `BoxGeometry`'ye ihtiyacımız var. Bu, küpün tüm noktalarını (vertices) ve dolgusunu (faces) içeren bir nesnedir. Bunu ileride daha ayrıntılı inceleyeceğiz.

Geometrinin yanı sıra, onu renklendirmek için bir malzemeye (material) ihtiyacımız var. Three.js birkaç malzeme ile birlikte gelir, ancak şimdilik `MeshBasicMaterial` ile devam edeceğiz. Tüm malzemeler, kendilerine uygulanacak özelliklerden oluşan bir nesne alır. İşleri çok basit tutmak için yalnızca yeşil olan `0x00ff00` değerinde bir `color` özniteliği veriyoruz. Bu, CSS'te ya da Photoshop'ta renklerin çalışma biçimiyle aynıdır (hex renkler).

İhtiyacımız olan üçüncü şey bir `Mesh`. Mesh, bir geometri alıp ona bir malzeme uygulayan, sonra sahnemize ekleyip serbestçe hareket ettirebileceğimiz bir nesnedir.

Varsayılan olarak `scene.add()` çağırdığımızda, eklenen şey (0,0,0) koordinatlarına eklenir. Bu, hem kameranın hem de küpün birbirinin içinde olmasına neden olur. Bunu önlemek için kamerayı biraz uzaklaştırmamız yeterlidir.

### Rendering the scene (Sahneyi render etme)

Yukarıdaki kodu daha önce oluşturduğumuz `main.js` dosyasına kopyalarsanız hiçbir şey göremezsiniz. Bunun nedeni henüz gerçekten hiçbir şeyi render etmiyor olmamızdır. Bunun için render ya da animasyon döngüsü (animation loop) denen şeye ihtiyacımız var.

```javascript
function animate( time ) {
  renderer.render( scene, camera );
}
renderer.setAnimationLoop( animate );
```

Bu, render edicinin ekran her yenilendiğinde sahneyi çizmesini sağlayan bir döngü oluşturur (tipik bir ekranda bu saniyede 60 kez anlamına gelir). Tarayıcıda oyun yazmaya yeniyseniz "neden sadece bir `setInterval` oluşturmuyoruz?" diyebilirsiniz. Mesele şu ki - yapabilirdik, ancak `WebGLRenderer` içinde dahili olarak kullanılan `requestAnimationFrame`'in bir dizi avantajı vardır. Belki de en önemlisi, kullanıcı başka bir tarayıcı sekmesine geçtiğinde durmasıdır; böylece kullanıcının değerli işlem gücü ve pil ömrü boşa harcanmaz.

### Animating the cube (Küpü canlandırma)

Yukarıdaki tüm kodu başlamadan önce oluşturduğunuz dosyaya eklerseniz, yeşil bir kutu görmelisiniz. Şimdi onu döndürerek her şeyi biraz daha ilginç hale getirelim.

Aşağıdaki kodu `animate` fonksiyonunuzdaki `renderer.render` çağrısının hemen üstüne ekleyin:

```javascript
cube.rotation.x = time / 2000;
cube.rotation.y = time / 1000;
```

Bu, her karede çalıştırılacaktır (normalde saniyede 60 kez) ve küpe güzel bir dönme animasyonu verecektir. Temelde, uygulama çalışırken hareket etmesini ya da değişmesini istediğiniz her şey animasyon döngüsünden geçmek zorundadır. Elbette oradan başka fonksiyonlar da çağırabilirsiniz, böylece yüzlerce satırlık bir `animate` fonksiyonuyla karşılaşmazsınız.

### The result (Sonuç)

Tebrikler! İlk three.js uygulamanızı tamamladınız. Basit, ama bir yerden başlamak gerekir.

Kodun tamamı aşağıda ve düzenlenebilir bir [canlı örnek (live example)](https://jsfiddle.net/zycqb61k/) olarak mevcuttur. Nasıl çalıştığını daha iyi anlamak için üzerinde oynayın.

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>My first three.js app</title>
    <style>
      body { margin: 0; }
    </style>
  </head>
  <body>
    <script type="module" src="/main.js"></script>
  </body>
</html>
```

**main.js**

```javascript
import * as THREE from 'three';

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );

const renderer = new THREE.WebGLRenderer();
renderer.setSize( window.innerWidth, window.innerHeight );
renderer.setAnimationLoop( animate );
document.body.appendChild( renderer.domElement );

const geometry = new THREE.BoxGeometry( 1, 1, 1 );
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
const cube = new THREE.Mesh( geometry, material );
scene.add( cube );

camera.position.z = 5;

function animate( time ) {

  cube.rotation.x = time / 2000;
  cube.rotation.y = time / 1000;

  renderer.render( scene, camera );

}
```

---

## 3. Fundamentals (Temeller)

Bu, three.js hakkındaki makale serisinin ilk makalesidir. [Three.js](https://threejs.org), 3B içeriği bir web sayfasına mümkün olduğunca kolay getirmeye çalışan bir 3B kütüphanedir.

### Three.js ve WebGL

Three.js sıklıkla WebGL ile karıştırılır; çünkü çoğu zaman, ama her zaman değil, three.js 3B çizmek için WebGL kullanır. [WebGL, yalnızca noktaları, çizgileri ve üçgenleri çizen çok düşük seviyeli bir sistemdir](https://webglfundamentals.org). WebGL ile yararlı bir şey yapmak genellikle epeyce kod gerektirir ve three.js tam da burada devreye girer. Sahneler, ışıklar, gölgeler, malzemeler, dokular, 3B matematik gibi, WebGL'i doğrudan kullanacak olsaydınız kendinizin yazmak zorunda kalacağı her şeyi o halleder.

Bu eğitimler JavaScript'i zaten bildiğinizi varsayar ve çoğunlukla ES6 stilini kullanır. Bilmenizin beklendiği şeylerin kısa bir listesi için [buraya bakın](https://threejs.org/manual/en/prerequisites.html). three.js'i destekleyen tarayıcıların çoğu otomatik güncellenir, dolayısıyla çoğu kullanıcı bu kodu çalıştırabilmelidir. Bu kodu gerçekten eski tarayıcılarda çalıştırmak isterseniz [Babel](https://babeljs.io) gibi bir transpiler'a bakın. Elbette gerçekten eski tarayıcı kullanan kişilerin makineleri muhtemelen three.js'i çalıştıramaz.

Çoğu programlama dilini öğrenirken insanların yaptığı ilk şey, bilgisayara "Hello World!" yazdırmaktır. 3B için en yaygın ilk işlerden biri 3B bir küp yapmaktır. O halde "Hello Cube!" ile başlayalım.

### Bir three.js uygulamasının yapısı

Başlamadan önce size bir three.js uygulamasının yapısı hakkında fikir vermeye çalışalım. Bir three.js uygulaması, bir sürü nesne oluşturmanızı ve bunları birbirine bağlamanızı gerektirir.

- Bir `Renderer` var. Bu, tartışmasız three.js'in ana nesnesidir. Bir `Renderer`'a bir `Scene` ve bir `Camera` verirsiniz; o da 3B sahnenin kameranın görüş hacmi (frustum) içinde kalan kısmını 2B bir görüntü olarak bir canvas'a render eder (çizer).
- Bir sahne grafiği (scenegraph) var; bu, ağaca benzer bir yapıdır ve bir `Scene` nesnesi, birden çok `Mesh` nesnesi, `Light` nesneleri, `Group`, `Object3D` ve `Camera` nesneleri gibi çeşitli nesnelerden oluşur. Bir `Scene` nesnesi sahne grafiğinin kökünü tanımlar ve arka plan rengi ile sis gibi özellikleri içerir. Bu nesneler hiyerarşik, ebeveyn/çocuk ağaç benzeri bir yapı tanımlar ve nesnelerin nerede göründüğünü ve nasıl yönlendirildiğini temsil eder. Çocuklar, ebeveynlerine göre konumlandırılır ve yönlendirilir. Örneğin bir arabanın tekerlekleri arabanın çocukları olabilir; böylece arabanın nesnesini hareket ettirmek ve yönlendirmek tekerlekleri de otomatik olarak hareket ettirir. Bu konuda daha fazlasını [sahne grafikleri hakkındaki makalede](https://threejs.org/manual/en/scenegraph.html) okuyabilirsiniz.

  Diyagramda `Camera`'nın yarısının sahne grafiğinin içinde, yarısının dışında olduğuna dikkat edin. Bu, three.js'te diğer nesnelerin aksine bir `Camera`'nın çalışmak için sahne grafiğinde olmak zorunda olmadığını temsil eder. Diğer nesneler gibi, başka bir nesnenin çocuğu olan bir `Camera` da ebeveyn nesnesine göre hareket eder ve yönlenir. Bir sahne grafiğine birden çok `Camera` nesnesi koymanın bir örneği, [sahne grafikleri makalesinin](https://threejs.org/manual/en/scenegraph.html) sonunda yer alır.

- `Mesh` nesneleri, belirli bir `Geometry`'nin belirli bir `Material` ile çizilmesini temsil eder. Hem `Material` nesneleri hem de `Geometry` nesneleri birden çok `Mesh` nesnesi tarafından kullanılabilir. Örneğin farklı konumlarda iki mavi küp çizmek için her küpün konumunu ve yönelimini temsil etmek üzere iki `Mesh` nesnesine ihtiyacımız olur. Bir küpün köşe noktası verisini tutmak için yalnızca bir `Geometry`, mavi rengi belirtmek için de yalnızca bir `Material` yeterli olur. Her iki `Mesh` nesnesi de aynı `Geometry` nesnesine ve aynı `Material` nesnesine başvurabilir.
- `Geometry` nesneleri; küre, küp, düzlem, köpek, kedi, insan, ağaç, bina vb. gibi bir geometri parçasının köşe noktası verisini temsil eder. Three.js birçok türde yerleşik geometri ilkeli (primitive) sağlar. Ayrıca özel geometri oluşturabilir ya da geometriyi dosyalardan yükleyebilirsiniz.
- `Material` nesneleri, geometriyi çizmek için kullanılan yüzey özelliklerini temsil eder; kullanılacak renk ve ne kadar parlak olduğu gibi şeyleri içerir. Bir `Material` ayrıca bir ya da daha fazla `Texture` nesnesine de başvurabilir; bunlar örneğin bir görüntüyü geometrinin yüzeyine sarmak için kullanılabilir.
- `Texture` nesneleri genellikle ya görüntü dosyalarından yüklenen, ya bir canvas'tan üretilen, ya da başka bir sahneden render edilen görüntüleri temsil eder.
- `Light` nesneleri farklı türde ışıkları temsil eder.

Bütün bunlar göz önüne alındığında, şuna benzeyen en küçük "Hello Cube" kurulumunu yapacağız.

### Three.js'i yükleme ve canvas hazırlama

Önce three.js'i yükleyelim

```html
<script type="module">
import * as THREE from 'three';
</script>
```

Script etiketine `type="module"` koymanız önemlidir. Bu, three.js'i yüklemek için `import` anahtar sözcüğünü kullanmamızı sağlar. r147 sürümünden itibaren three.js'i düzgün şekilde yüklemenin tek yolu budur. Modüllerin, ihtiyaç duydukları diğer modülleri kolayca içe aktarabilme avantajı vardır. Bu, bağımlı oldukları ek script'leri elle yüklemek zorunda kalmamızı sağlar.

Sıradaki ihtiyacımız bir `<canvas>` etiketi, o halde...

```html
<body>
  <canvas id="c"></canvas>
</body>
```

Three.js'ten o canvas'a çizmesini isteyeceğiz, bu yüzden onu bulmamız gerekiyor.

```html
<script type="module">
import * as THREE from 'three';

function main() {
  const canvas = document.querySelector('#c');
  const renderer = new THREE.WebGLRenderer({antialias: true, canvas});
  ...
</script>
```

Canvas'ı bulduktan sonra bir `WebGLRenderer` oluşturuyoruz. Renderer, sağladığınız tüm veriyi alıp canvas'a render etmekten sorumlu olan şeydir.

Burada bazı sıra dışı ayrıntılar olduğunu unutmayın. three.js'e bir canvas vermezseniz o sizin için bir tane oluşturur, ancak sonra onu belgenize eklemeniz gerekir. Nereye ekleneceği kullanım durumunuza göre değişebilir ve kodunuzu değiştirmeniz gerekir; bu yüzden ben three.js'e bir canvas vermeyi biraz daha esnek buluyorum. Canvas'ı herhangi bir yere koyabilirim ve kod onu bulur; oysa canvas'ı belgeye ekleyen kodum olsaydı, kullanım durumum değiştiğinde muhtemelen o kodu değiştirmem gerekirdi.

### Kamera ve frustum

Sonra bir kameraya ihtiyacımız var. Bir `PerspectiveCamera` oluşturacağız.

```javascript
const fov = 75;
const aspect = 2;  // the canvas default
const near = 0.1;
const far = 5;
const camera = new THREE.PerspectiveCamera(fov, aspect, near, far);
```

`fov`, görüş alanı (field of view) sözcüklerinin kısaltmasıdır. Bu durumda dikey boyutta 75 derecedir. three.js'teki açıların çoğunun radyan cinsinden olduğuna, ancak perspektif kameranın nedense dereceyi aldığına dikkat edin.

`aspect`, canvas'ın görüntü en-boy oranıdır. Ayrıntılara başka bir makalede gireceğiz, ancak varsayılan olarak bir canvas 300x150 pikseldir ve bu da en-boy oranını 300/150, yani 2 yapar.

`near` ve `far`, kameranın önünde render edilecek alanı temsil eder. Bu aralığın öncesindeki ya da sonrasındaki her şey kırpılır (çizilmez).

Bu dört ayar bir "frustum" tanımlar. Frustum, tepesi kesilmiş bir piramide benzeyen bir 3B şeklin adıdır. Başka bir deyişle "frustum" sözcüğünü küre, küp, prizma, frustum gibi başka bir 3B şekil olarak düşünün.

Yakın ve uzak düzlemlerin yüksekliği görüş alanı tarafından belirlenir. Her iki düzlemin genişliği ise görüş alanı ve en-boy oranı tarafından belirlenir.

Tanımlanan frustum'un içindeki her şey çizilir. Dışındaki hiçbir şey çizilmez.

Kamera varsayılan olarak -Z ekseni boyunca, +Y yukarı olacak şekilde bakar. Küpümüzü başlangıç noktasına koyacağız, bu yüzden bir şey görebilmek için kamerayı başlangıç noktasından biraz geri taşımamız gerekir.

```javascript
camera.position.z = 2;
```

Hedeflediğimiz şey şu: kameramız z = 2'de ve -Z ekseni boyunca bakıyor. Frustum'umuz kameranın önünden 0,1 birim ileride başlıyor ve kameranın önünde 5 birime kadar gidiyor. Yukarıdan baktığımızda görüş alanı en-boy oranından etkilenir. Canvas'ımız yüksekliğinin iki katı genişliğinde olduğundan, canvas boyunca görüş alanı, dikey görüş alanı olan belirttiğimiz 75 dereceden çok daha geniş olacaktır.

### Scene, geometri, malzeme ve Mesh

Sonra bir `Scene` yapıyoruz. three.js'te bir `Scene`, bir tür sahne grafiğinin köküdür. three.js'in çizmesini istediğiniz her şeyin sahneye eklenmesi gerekir. Sahnelerin nasıl çalıştığına dair daha fazla ayrıntıyı gelecekteki bir makalede ele alacağız.

```javascript
const scene = new THREE.Scene();
```

Sırada bir kutunun verisini içeren `BoxGeometry` oluşturuyoruz. Three.js'te görüntülemek istediğimiz hemen her şey, 3B nesnemizi oluşturan köşe noktalarını tanımlayan bir geometriye ihtiyaç duyar.

```javascript
const boxWidth = 1;
const boxHeight = 1;
const boxDepth = 1;
const geometry = new THREE.BoxGeometry(boxWidth, boxHeight, boxDepth);
```

Ardından temel bir malzeme oluşturup rengini ayarlıyoruz. Renkler, standart CSS tarzı 6 haneli hex renk değerleri kullanılarak belirtilebilir.

```javascript
const material = new THREE.MeshBasicMaterial({color: 0x44aa88});
```

Sonra bir `Mesh` oluşturuyoruz. three.js'te bir `Mesh`, üç şeyin birleşimini temsil eder:

1. Bir `Geometry` (nesnenin şekli)
2. Bir `Material` (nesnenin nasıl çizileceği: parlak mı mat mı, hangi renk, hangi doku(lar) uygulanacak vb.)
3. O nesnenin sahnedeki konumu, yönelimi ve ölçeği, ebeveynine göre. Aşağıdaki kodda bu ebeveyn sahnedir.

```javascript
const cube = new THREE.Mesh(geometry, material);
```

Ve son olarak bu mesh'i sahneye ekliyoruz

```javascript
scene.add(cube);
```

Daha sonra renderer'ın `render` fonksiyonunu çağırıp ona sahneyi ve kamerayı vererek sahneyi render edebiliriz

```javascript
renderer.render(scene, camera);
```

[Çalışan örneği ayrı bir pencerede aç](https://threejs.org/manual/examples/fundamentals.html)

Bunun 3B bir küp olduğunu anlamak biraz zor, çünkü ona doğrudan -Z ekseni boyunca bakıyoruz ve küpün kendisi eksenlerle hizalı olduğundan yalnızca tek bir yüz görüyoruz.

### Render döngüsü ve animasyon

Onu dönerken canlandıralım; umarım bu, 3B çizildiğini netleştirir. Canlandırmak için `requestAnimationFrame` kullanarak bir render döngüsü içinde render edeceğiz.

İşte döngümüz

```javascript
function render(time) {
  time *= 0.001;  // convert time to seconds

  cube.rotation.x = time;
  cube.rotation.y = time;

  renderer.render(scene, camera);

  requestAnimationFrame(render);
}
requestAnimationFrame(render);
```

`requestAnimationFrame`, tarayıcıya bir şeyi canlandırmak istediğiniz yönünde yapılan bir istektir. Ona çağrılacak bir fonksiyon verirsiniz. Bizim durumumuzda bu fonksiyon `render`'dır. Tarayıcı fonksiyonunuzu çağırır ve sayfanın görüntülenmesiyle ilgili herhangi bir şeyi güncellerseniz tarayıcı sayfayı yeniden render eder. Bizim durumumuzda three'nin `renderer.render` fonksiyonunu çağırıyoruz; o da sahnemizi çizer.

`requestAnimationFrame`, fonksiyonumuza sayfanın yüklenmesinden bu yana geçen süreyi iletir. Bu süre milisaniye cinsinden iletilir. Saniyelerle çalışmayı çok daha kolay buluyorum, bu yüzden burada onu saniyeye çeviriyoruz.

Ardından küpün X ve Y dönüşünü geçerli zamana ayarlıyoruz. Bu dönüşler radyan cinsindendir. Bir dairede 2 pi radyan vardır, dolayısıyla küpümüz her eksende yaklaşık 6,28 saniyede bir kez tam dönmelidir.

Sonra sahneyi render ediyoruz ve döngümüzü sürdürmek için başka bir animasyon karesi istiyoruz.

Döngünün dışında, döngüyü başlatmak için `requestAnimationFrame`'i bir kez çağırıyoruz.

[Çalışan örneği ayrı bir pencerede aç](https://threejs.org/manual/examples/fundamentals-not-as-flat.html)

### Aydınlatma ekleme

Biraz daha iyi, ama 3B'yi görmek hâlâ zor. Yardımcı olacak şey biraz aydınlatma eklemek, o halde bir ışık ekleyelim. three.js'te birçok ışık türü var; bunları gelecekteki bir makalede ele alacağız. Şimdilik bir yönlü ışık (directional light) oluşturalım.

```javascript
const color = 0xFFFFFF;
const intensity = 3;
const light = new THREE.DirectionalLight(color, intensity);
light.position.set(-1, 2, 4);
scene.add(light);
```

Yönlü ışıkların bir konumu ve bir hedefi vardır. İkisi de varsayılan olarak 0, 0, 0'dır. Bizim durumumuzda ışığın konumunu -1, 2, 4 olarak ayarlıyoruz; yani kameramızın biraz solunda, üstünde ve arkasında. Hedef hâlâ 0, 0, 0 olduğundan ışık başlangıç noktasına doğru parlayacaktır.

Malzemeyi de değiştirmemiz gerekiyor. `MeshBasicMaterial` ışıklardan etkilenmez. Onu ışıklardan etkilenen bir `MeshPhongMaterial` ile değiştirelim.

```javascript
-const material = new THREE.MeshBasicMaterial({color: 0x44aa88});  // greenish blue
+const material = new THREE.MeshPhongMaterial({color: 0x44aa88});  // greenish blue
```

Yeni program yapımız: `WebGLRenderer`, `PerspectiveCamera`, `Scene`, `Mesh`, `Light`, `BoxGeometry`, `MeshPhongMaterial`.

[Çalışan örneği ayrı bir pencerede aç](https://threejs.org/manual/examples/fundamentals-with-light.html)

Artık 3B olduğu oldukça açık olmalı.

### Üç küp ile örnek

Sırf eğlence olsun diye 2 küp daha ekleyelim.

Her küp için aynı geometriyi kullanacağız, ancak her küpün farklı bir renk olabilmesi için farklı bir malzeme yapacağız.

Önce belirtilen renkte yeni bir malzeme oluşturan bir fonksiyon yapacağız. Ardından bu fonksiyon, belirtilen geometriyi kullanarak bir mesh oluşturur, onu sahneye ekler ve X konumunu ayarlar.

```javascript
function makeInstance(geometry, color, x) {
  const material = new THREE.MeshPhongMaterial({color});

  const cube = new THREE.Mesh(geometry, material);
  scene.add(cube);

  cube.position.x = x;

  return cube;
}
```

Sonra onu 3 farklı renk ve X konumuyla 3 kez çağıracağız ve `Mesh` örneklerini bir dizide saklayacağız.

```javascript
const cubes = [
  makeInstance(geometry, 0x44aa88,  0),
  makeInstance(geometry, 0x8844aa, -2),
  makeInstance(geometry, 0xaa8844,  2),
];
```

Son olarak render fonksiyonumuzda 3 küpü de döndüreceğiz. Her biri için biraz farklı bir dönüş hesaplıyoruz.

```javascript
function render(time) {
  time *= 0.001;  // convert time to seconds

  cubes.forEach((cube, ndx) => {
    const speed = 1 + ndx * .1;
    const rot = time * speed;
    cube.rotation.x = rot;
    cube.rotation.y = rot;
  });

  ...
```

ve işte bu.

[Çalışan örneği ayrı bir pencerede aç](https://threejs.org/manual/examples/fundamentals-3-cubes.html)

Bunu yukarıdan görünüşlü diyagramla karşılaştırırsanız beklentilerimizle uyuştuğunu görebilirsiniz. X = -2 ve X = +2'deki küpler kısmen frustum'umuzun dışındadır. Canvas boyunca görüş alanı çok aşırı olduğu için biraz abartılı şekilde bozulmuş (çarpık) da görünürler.

Programımızın artık şu yapısı var: `WebGLRenderer`, `PerspectiveCamera`, `Scene`, 3 adet `Mesh`, `Light`, `BoxGeometry` ve 3 adet `MeshPhongMaterial`.

Gördüğünüz gibi aynı `BoxGeometry`'ye başvuran 3 `Mesh` nesnemiz var. Her `Mesh`, her küpün farklı bir renkte olabilmesi için benzersiz bir `MeshPhongMaterial`'e başvuruyor.

Umarım bu kısa giriş işlere başlamaya yardımcı olur. Sırada, kodumuzu birden çok duruma uyum sağlayabilecek şekilde duyarlı (responsive) hale getirmeyi ele alacağız.

### es6 modules, three.js ve klasör yapısı

r147 sürümünden itibaren three.js'i kullanmanın tercih edilen yolu es6 modülleri ve import map'lerdir.

es6 modülleri bir script içinde `import` anahtar sözcüğüyle ya da satır içi olarak bir `<script type="module">` etiketi aracılığıyla yüklenebilir. İşte bir örnek

```html
<script type="module">
import * as THREE from 'three';

...

</script>
```

Oradaki `'three'` belirleyicisine dikkat edin. Onu olduğu gibi bırakırsanız büyük olasılıkla hata verir. Tarayıcıya three.js'i nerede bulacağını söylemek için bir import map kullanılmalıdır

```html
<script type="importmap">
{
  "imports": {
    "three": "./path/to/three.module.js"
  }
}
</script>
```

Yol belirleyicisinin yalnızca `./` ya da `../` ile başlayabileceğini unutmayın.

`OrbitControls.js` gibi eklentileri (addons) içe aktarmak için şunu kullanın

```javascript
import {OrbitControls} from 'three/addons/controls/OrbitControls.js';
```

Eklentileri import map'e şu şekilde eklemeyi unutmayın

```html
<script type="importmap">
{
  "imports": {
    "three": "./path/to/three.module.js",
    "three/addons/": "./different/path/to/examples/jsm/"
  }
}
</script>
```

Bir CDN de kullanabilirsiniz

```html
<script type="importmap">
{
  "imports": {
    "three": "https://cdn.jsdelivr.net/npm/three@<version>/build/three.module.js",
    "three/addons/": "https://cdn.jsdelivr.net/npm/three@<version>/examples/jsm/"
  }
}
</script>
```

Sonuç olarak, three.js'i kullanmanın önerilen yolu şudur

```html
<script type="importmap">
{
  "imports": {
    "three": "./path/to/three.module.js",
    "three/addons/": "./different/path/to/examples/jsm/"
  }
}
</script>

<script type="module">
import * as THREE from 'three';
import {OrbitControls} from 'three/addons/controls/OrbitControls.js';

...

</script>
```

---

## 4. Primitives (Temel Geometriler)

### Temel geometrilere giriş

Bu makale, three.js hakkındaki bir makale serisinin parçasıdır. İlk makale temel kavramlar (fundamentals) hakkındaydı. Henüz okumadıysanız oradan başlamak isteyebilirsiniz.

Three.js çok sayıda temel geometriye (primitive) sahiptir. Temel geometriler, genellikle çalışma zamanında (runtime) bir dizi parametre ile oluşturulan 3 boyutlu şekillerdir.

Bir dünya küresi için küre (sphere) veya 3 boyutlu bir grafik çizmek için birden fazla kutu (box) gibi yapılar için temel geometrileri kullanmak yaygındır. Özellikle 3B dünyasına adım atarken ve deneyler yaparken temel geometrilerden yararlanmak oldukça sık tercih edilir. Çoğu 3B uygulama için ise bir sanatçının Blender, Maya veya Cinema 4D gibi bir 3B modelleme programında hazırladığı modelleri kullanmak daha yaygındır. Bu serinin ilerleyen bölümlerinde, çeşitli 3B modelleme programlarından veri oluşturmayı ve bunları yüklemeyi ele alacağız. Şimdilik mevcut temel geometrilerin bazılarını inceleyelim.

Aşağıdaki temel geometrilerin birçoğu, parametrelerinin bazıları veya tamamı için varsayılan değerlere sahiptir; böylece ihtiyaçlarınıza göre az ya da çok parametre belirleyerek kullanabilirsiniz.

### EdgesGeometry ve WireframeGeometry

**EdgesGeometry**

Girdi olarak başka bir geometriyi alan ve yalnızca yüzeyler arasındaki açı belirli bir eşik değerinden (threshold) büyük olduğunda kenarlar oluşturan yardımcı bir nesnedir. Örneğin, en üstteki kutuya bakarsanız, kutuyu oluşturan her bir üçgeni gösteren ve her yüzeyin ortasından geçen bir çizgi görürsünüz. Bunun yerine bir EdgesGeometry kullanıldığında bu orta çizgiler kaldırılır. `thresholdAngle` (eşik açısı) değerini ayarladığınızda, bu eşiğin altında kalan kenarların kaybolduğunu göreceksiniz.

**WireframeGeometry**

Verilen geometrideki her bir kenar için tek bir çizgi parçası (2 nokta) içeren bir geometri oluşturur. Bu yardımcı nesne olmasaydı, WebGL genellikle her çizgi parçası için 2 nokta gerektirdiğinden sık sık eksik veya fazla kenarlarla karşılaşırdınız. Örneğin, elinizde yalnızca tek bir üçgen olsaydı sadece 3 nokta bulunurdu. Bunu `wireframe: true` özelliğine sahip bir materyal kullanarak çizmeye çalışsaydınız yalnızca tek bir çizgi elde ederdiniz. Bu üçgen geometrisini bir WireframeGeometry nesnesine aktarmak ise 6 nokta kullanarak 3 çizgi parçasına sahip yeni bir geometri oluşturacaktır.

Özel geometri (custom geometry) oluşturmayı başka bir makalede ele alacağız. Şimdilik her temel geometri türünü oluşturan bir örnek yapalım. Önceki makaledeki örneklerle başlayacağız.

### Arka plan rengi ve kamera ayarları

Sayfanın üst kısımlarına yakın bir yerde arka plan rengini ayarlayalım:

```javascript
const scene = new THREE.Scene();
+ scene.background = new THREE.Color(0xAAAAAA);
```

Bu, three.js'e açık gri bir renkle temizleme yapmasını söyler.

Tüm nesneleri görebilmemiz için kameranın konumunun değişmesi gerekir.

```javascript
-const fov = 75;
+const fov = 40;
const aspect = 2;  // the canvas default
const near = 0.1;
-const far = 5;
+const far = 1000;
const camera = new THREE.PerspectiveCamera(fov, aspect, near, far);
-camera.position.z = 2;
+camera.position.z = 120;
```

### addObject ve createMaterial fonksiyonları

Bir x, y konumu ile bir Object3D alan ve nesneyi sahneye ekleyen `addObject` adında bir fonksiyon ekleyelim.

```javascript
const objects = [];
const spread = 15;

function addObject(x, y, obj) {
  obj.position.x = x * spread;
  obj.position.y = y * spread;

  scene.add(obj);
  objects.push(obj);
}
```

Ayrıca rastgele renkli bir materyal oluşturan bir fonksiyon yapalım. Color sınıfının, bir rengi ton (hue), doygunluk (saturation) ve parlaklığa (luminance) göre ayarlamanıza olanak tanıyan bir özelliğini kullanacağız.

Ton (hue), renk çemberi etrafında 0 ile 1 arasında değişir; kırmızı 0, yeşil 0.33 ve mavi 0.66 noktasındadır. Doygunluk (saturation), 0 ile 1 arasında değer alır; 0 renksiz (gri), 1 ise en doygun halidir. Parlaklık (luminance), 0 ile 1 arasında değişir; 0 siyah, 1 beyaz ve 0.5 maksimum renk yoğunluğudur. Başka bir deyişle, parlaklık 0.0'dan 0.5'e doğru gittikçe renk siyahtan seçilen tona doğru ilerler. 0.5'ten 1.0'a doğru gittikçe ise renk tondan beyaza doğru ilerler.

```javascript
function createMaterial() {
  const material = new THREE.MeshPhongMaterial({
    side: THREE.DoubleSide,
  });

  const hue = Math.random();
  const saturation = 1;
  const luminance = .5;
  material.color.setHSL(hue, saturation, luminance);

  return material;
}
```

Ayrıca materyale `side: THREE.DoubleSide` parametresini geçtik. Bu, Three.js'e bir şekli oluşturan üçgenlerin her iki yüzünü de çizmesini söyler. Küre veya küp gibi içi dolu bir şekil için üçgenlerin arka yüzlerini çizmenin genellikle bir anlamı yoktur, çünkü bu yüzlerin hepsi şeklin içine bakar. Ancak bizim durumumuzda, `PlaneGeometry` ve `ShapeGeometry` gibi 2 boyutlu olan ve dolayısıyla içi bulunmayan birkaç nesne çiziyoruz. `side: THREE.DoubleSide` ayarını yapmazsak, arka yüzlerinden bakıldığında bu nesneler kaybolur.

`side: THREE.DoubleSide` ayarlanmadığında çizim işleminin daha hızlı olduğunu belirtmeliyim; bu nedenle ideal olarak bu ayarı yalnızca gerçekten ihtiyacı olan materyallere uygulamalıyız. Fakat bu örnekte çok fazla nesne çizmediğimiz için bunu dert etmemizi gerektirecek bir sebep yok.

### addSolidGeometry ve kutu örneği

İçine bir geometri aktardığımız, `createMaterial` aracılığıyla rastgele renkli bir materyal oluşturan ve bunu `addObject` ile sahneye ekleyen `addSolidGeometry` adında bir fonksiyon yapalım.

```javascript
function addSolidGeometry(x, y, geometry) {
  const mesh = new THREE.Mesh(geometry, createMaterial());
  addObject(x, y, mesh);
}
```

Artık bunu oluşturduğumuz temel geometrilerin büyük bir kısmı için kullanabiliriz. Örneğin bir kutu (box) oluştururken:

```javascript
{
  const width = 8;
  const height = 8;
  const depth = 8;
  addSolidGeometry(-2, -2, new THREE.BoxGeometry(width, height, depth));
}
```

Yukarıdaki desende öne çıkan birkaç dikkate değer istisna bulunmaktadır.

### Özel durum: TextGeometry

Bunların en büyüğü muhtemelen `TextGeometry`dir. Metin için bir ağ (mesh) oluşturabilmesi için önce 3B yazı tipi verisini yüklemesi gerekir. Bu veri eşzamansız (asynchronous) olarak yüklendiğinden, geometriyi oluşturmaya çalışmadan önce yüklenmesini beklememiz gerekir. Yazı tipi yükleme işlemini Promise yapısına dönüştürerek (promisifying) bunu çok daha kolay hale getirebiliriz. Bir `FontLoader` ve ardından tamamlandığında (resolve) bize yazı tipini verecek bir Promise döndüren `loadFont` adında bir fonksiyon oluşturuyoruz. Sonrasında `doit` adında async bir fonksiyon oluşturup `await` kullanarak yazı tipini yüklüyoruz. Ve son olarak geometriyi oluşturup sahneye eklemek için `addObject` fonksiyonunu çağırıyoruz.

```javascript
{
  const loader = new FontLoader();
  // promisify font loading
  function loadFont(url) {
    return new Promise((resolve, reject) => {
      loader.load(url, resolve, undefined, reject);
    });
  }

  async function doit() {
    const font = await loadFont('resources/threejs/fonts/helvetiker_regular.typeface.json');  /* threejs.org: url */
    const geometry = new TextGeometry('three.js', {
      font: font,
      size: 3.0,
      depth: .2,
      curveSegments: 12,
      bevelEnabled: true,
      bevelThickness: 0.15,
      bevelSize: .3,
      bevelSegments: 5,
    });
    const mesh = new THREE.Mesh(geometry, createMaterial());
    geometry.computeBoundingBox();
    geometry.boundingBox.getCenter(mesh.position).multiplyScalar(-1);

    const parent = new THREE.Object3D();
    parent.add(mesh);

    addObject(-1, -1, parent);
  }
  doit();
}
```

Başka bir fark daha var. Metni kendi merkezi etrafında döndürmek istiyoruz, ancak Three.js varsayılan olarak metni dönme merkezi sol kenarında olacak şekilde oluşturur. Bu durumu çözmek için Three.js'ten geometrinin sınırlayıcı kutusunu (bounding box) hesaplamasını isteyebiliriz. Ardından sınırlayıcı kutunun `getCenter` metodunu çağırıp buna ağımızın (mesh) konum nesnesini aktarabiliriz. `getCenter`, kutunun merkezini bu konuma kopyalar. Ayrıca konum nesnesini geri döndürdüğü için, tüm nesneyi dönme merkezi tam ortasına gelecek şekilde konumlandırmak adına `multiplyScalar(-1)` metodunu çağırabiliriz.

Eğer daha sonra önceki örneklerdeki gibi doğrudan `addSolidGeometry` fonksiyonunu çağırsaydık, bu durum konumu tekrar sıfırlayarak değiştirecekti ki bu da istediğimiz bir şey değil. Bu yüzden, bu senaryoda Three.js sahne grafiğinin (scene graph) standart düğümü olan bir `Object3D` oluşturuyoruz. Mesh de zaten `Object3D` sınıfından türetilmiştir. Sahne grafiğinin nasıl çalıştığını başka bir makalede ele alacağız. Şimdilik, tıpkı DOM düğümlerinde olduğu gibi alt elemanların (children) üst elemanlarına (parent) göre çizildiğini bilmek yeterlidir. Bir `Object3D` oluşturup ağımızı onun bir alt elemanı yaparak, `Object3D` nesnesini istediğimiz yere konumlandırabilir ve daha önce ayarladığımız merkez kaymasını (offset) koruyabiliriz. Bunu yapmasaydık, metin merkezinin dışından (ekseni kaymış şekilde) dönerdi.

### Özel durum: çizgi tabanlı geometriler

Diğer istisnalar ise `EdgesGeometry` ve `WireframeGeometry` için çizgi tabanlı 2 örnektir. Bu örnekler `addSolidGeometry` fonksiyonunu çağırmak yerine, şu şekilde görünen `addLineGeometry` fonksiyonunu çağırırlar:

```javascript
function addLineGeometry(x, y, geometry) {
  const material = new THREE.LineBasicMaterial({color: 0x000000});
  const mesh = new THREE.LineSegments(geometry, material);
  addObject(x, y, mesh);
}
```

Siyah bir `LineBasicMaterial` oluşturur ve ardından Three.js'in çizgi parçaları (her parça için 2 nokta) çizdiğinizi anlamasına yardımcı olan, `Mesh` için bir sarıcı (wrapper) görevi gören `LineSegments` nesnesini oluşturur.

Temel geometrilerin her biri, oluşturma sırasında iletebileceğiniz çeşitli parametrelere sahiptir. Bunları burada tekrarlamak yerine hepsi için dokümantasyona bakmak en doğrusudur. Her bir şeklin yanındaki bağlantılara tıklayarak doğrudan o şeklin doküman sayfasına da gidebilirsiniz.

### Points ve PointsMaterial

Yukarıdaki kalıplara pek uymayan bir diğer sınıf çifti daha vardır. Bunlar `PointsMaterial` ve `Points` sınıflarıdır. `Points`, bir `BufferGeometry` alması bakımından yukarıdaki `LineSegments` nesnesine benzer; ancak çizgiler yerine her bir köşede (vertex) noktalar çizer. Bunu kullanabilmek için, noktaların ne kadar büyük olacağını belirleyen bir `size` parametresi alan `PointsMaterial` materyalini de nesneye iletmeniz gerekir.

```javascript
const radius = 7;
const widthSegments = 12;
const heightSegments = 8;
const geometry = new THREE.SphereGeometry(radius, widthSegments, heightSegments);
const material = new THREE.PointsMaterial({
    color: 'red',
    size: 0.2,     // in world units
});
const points = new THREE.Points(geometry, material);
scene.add(points);
```

Noktaların kameraya olan uzaklıklarından bağımsız olarak aynı boyutta kalmasını istiyorsanız, `sizeAttenuation` özelliğini `false` olarak ayarlayarak devre dışı bırakabilirsiniz.

```javascript
const material = new THREE.PointsMaterial({
    color: 'red',
    sizeAttenuation: false,
    size: 3,       // in pixels
-   size: 0.2,     // in world units
});
...
```

### Alt bölme ayarları

Ele alınması önemli olan bir diğer konu da neredeyse tüm şekillerin kaç parçaya bölüneceğine (subdivide) dair çeşitli ayarlara sahip olmasıdır. Küre geometrileri buna iyi bir örnek olabilir. Küreler, etrafında kaç bölme ve yukarıdan aşağıya kaç bölme yapılacağına dair parametreler alır.

---

## 5. Scene Graph (Sahne Grafiği)

Bu makale, three.js hakkındaki bir makale serisinin parçasıdır. İlk makale [three.js temel kavramları](https://threejs.org/manual/#fundamentals) hakkındaydı. Henüz okumadıysanız oradan başlamayı düşünmek isteyebilirsiniz.

### Sahne grafiği nedir?

Three.js'in çekirdeğini, tartışmasız sahne grafiği (scene graph) oluşturur. Bir 3B motorunda sahne grafiği, bir graftaki düğümlerin (nodes) hiyerarşisidir ve her düğüm yerel bir alanı (local space) temsil eder.

Bu biraz soyut bir kavram olduğundan birkaç örnek vermeye çalışalım.

Örneklerden biri güneş sistemi; güneş, dünya ve ay olabilir.

Dünya, Güneş'in etrafında döner. Ay ise Dünya'nın etrafında döner. Ay, Dünya çevresinde dairesel bir hareket izler. Ay'ın bakış açısından bakıldığında, kendisi Dünya'nın "yerel alanında" (local space) dönmektedir. Güneş'e göre hareketi spirograf benzeri karmaşık bir eğri oluştursa da, Ay'ın bakış açısından sadece Dünya'nın yerel alanı etrafında dönmeyi dikkate alması yeterlidir.

Başka bir açıdan düşünecek olursak, Dünya üzerinde yaşayan sizlerin ne Dünya'nın kendi ekseni etrafında dönmesi ne de Güneş etrafındaki turu hakkında düşünmeniz gerekir. Yürürken, araç kullanırken, yüzerken veya koşarken sanki Dünya hiç hareket etmiyor ya da dönmüyormuş gibi hareket edersiniz. Güneş'e kıyasla Dünya'nın etrafında saatte yaklaşık 1000 mil (1600 km) hızla dönerken ve Güneş etrafında saatte yaklaşık 67.000 mil (107.000 km) hızla savrulurken bile Dünya'nın "yerel alanında" yürür, sürer, yüzer, koşar ve yaşarsınız. Güneş sistemindeki konumunuz yukarıdaki Ay'ın konumuna benzer, ancak bunu dert etmenize gerek yoktur. Siz sadece Dünya'ya ve onun "yerel alanına" göre olan konumunuzla ilgilenirsiniz.

### Güneş sistemi örneği

Adım adım ilerleyelim. Güneş, Dünya ve Ay'ı içeren bir diyagram yapmak istediğimizi hayal edin. Sadece bir küre oluşturup bunu başlangıç noktasına (origin) yerleştirerek Güneş ile başlayacağız. Not: Güneş, Dünya ve Ay'ı bir sahne grafiğinin nasıl kullanılacağını göstermek amacıyla bir örnek olarak kullanıyoruz. Elbette gerçek Güneş, Dünya ve Ay fizikten yararlanır; ancak bizim amacımız doğrultusunda bir sahne grafiği ile bunu taklit edeceğiz.

**Güneş ve nokta ışık**

```javascript
// an array of objects whose rotation to update
const objects = [];

// use just one sphere for everything
const radius = 1;
const widthSegments = 6;
const heightSegments = 6;
const sphereGeometry = new THREE.SphereGeometry(
    radius, widthSegments, heightSegments);

const sunMaterial = new THREE.MeshPhongMaterial({emissive: 0xFFFF00});
const sunMesh = new THREE.Mesh(sphereGeometry, sunMaterial);
sunMesh.scale.set(5, 5, 5);  // make the sun large
scene.add(sunMesh);
objects.push(sunMesh);
```

Çok düşük poligonlu bir küre kullanıyoruz; ekvatoru etrafında yalnızca 6 alt bölme (subdivision) var. Bu sayede dönme hareketini görmek kolaylaşıyor.

Aynı küreyi her şey için tekrar kullanacağımızdan, Güneş ağı (mesh) için ölçeği (scale) 5 katına ayarlayacağız.

Ayrıca Phong materyalinin `emissive` (yayılan ışık) özelliğini sarı olarak ayarlıyoruz. Bir Phong materyalinin emissive özelliği, temelde yüzeye hiçbir ışık vurmadığında çizilecek renktir. Işık bu rengin üzerine eklenir.

Sahnenin merkezine tek bir nokta ışık (point light) da yerleştirelim. Nokta ışıklar hakkındaki detaylara daha sonra değineceğiz, ancak şimdilik basitçe ifade etmek gerekirse bir nokta ışık, tek bir noktadan yayılan ışığı temsil eder.

```javascript
{
  const color = 0xFFFFFF;
  const intensity = 500;
  const light = new THREE.PointLight(color, intensity);
  scene.add(light);
}
```

```javascript
const camera = new THREE.PerspectiveCamera(fov, aspect, near, far);
camera.position.set(0, 50, 0);
camera.up.set(0, 0, 1);
camera.lookAt(0, 0, 0);
```

Önceki örneklerden uyarlanan işleme döngüsünde (render loop), `objects` dizimizdeki tüm nesneleri şu kodla döndürüyoruz:

```javascript
objects.forEach((obj) => {
  obj.rotation.y = time;
});
```

`sunMesh` nesnesini `objects` dizisine eklediğimiz için dönecektir.

**Dünya'yı ekleme ve Güneş'in alt elemanı yapma**

Şimdi Dünya'yı ekleyelim.

```javascript
const earthMaterial = new THREE.MeshPhongMaterial({color: 0x2233FF, emissive: 0x112244});
const earthMesh = new THREE.Mesh(sphereGeometry, earthMaterial);
earthMesh.position.x = 10;
scene.add(earthMesh);
objects.push(earthMesh);
```

Mavi bir materyal oluşturuyoruz, ancak siyah arka planda belirgin şekilde görünebilmesi için ona az miktarda yayılan mavi (emissive blue) renk veriyoruz.

`earthMesh` nesnesini oluşturmak için aynı `sphereGeometry` geometrisini yeni mavi `earthMaterial` materyalimizle birlikte kullanıyoruz. Bunu Güneş'in 10 birim soluna konumlandırıp sahneye ekliyoruz. `objects` dizimize eklediğimiz için bu da dönecektir.

Hem Güneş'in hem de Dünya'nın kendi etrafında döndüğünü görebilirsiniz; ancak Dünya, Güneş'in etrafında dönmüyor. Dünya'yı Güneş'in bir alt elemanı (child) yapalım:

```javascript
-scene.add(earthMesh);
+sunMesh.add(earthMesh);
```

`earthMesh` nesnesini `sunMesh` nesnesinin bir alt elemanı (child) yaptık. `sunMesh` nesnesinin ölçeği `sunMesh.scale.set(5, 5, 5)` koduyla 5 katına ayarlanmıştı. Bu durum, `sunMesh` nesnesinin yerel alanının (local space) 5 kat daha büyük olduğu anlamına gelir. Bu alana koyulan her şey 5 ile çarpılacaktır. Yani Dünya artık 5 kat daha büyüktür ve Güneş'e olan uzaklığı da (`earthMesh.position.x = 10`) 5 katına çıkmıştır.

**solarSystem düğümü (Object3D)**

Bu durumu düzeltmek için boş bir sahne grafiği düğümü (scene graph node) ekleyelim. Hem Güneş'i hem de Dünya'yı bu düğümün alt elemanı (child) yapacağız.

```javascript
+const solarSystem = new THREE.Object3D();
+scene.add(solarSystem);
+objects.push(solarSystem);

const sunMaterial = new THREE.MeshPhongMaterial({emissive: 0xFFFF00});
const sunMesh = new THREE.Mesh(sphereGeometry, sunMaterial);
sunMesh.scale.set(5, 5, 5);
-scene.add(sunMesh);
+solarSystem.add(sunMesh);
objects.push(sunMesh);

const earthMaterial = new THREE.MeshPhongMaterial({color: 0x2233FF, emissive: 0x112244});
const earthMesh = new THREE.Mesh(sphereGeometry, earthMaterial);
earthMesh.position.x = 10;
-sunMesh.add(earthMesh);
+solarSystem.add(earthMesh);
objects.push(earthMesh);
```

Burada bir `Object3D` oluşturduk. Tıpkı bir `Mesh` gibi bu da sahne grafiğinde bir düğümdür; ancak bir `Mesh` nesnesinden farklı olarak hiçbir materyale veya geometriye sahip değildir. Yalnızca yerel bir alanı (local space) temsil eder.

Hem `sunMesh` hem de `earthMesh`, `solarSystem` düğümünün birer alt elemanıdır (child). Her 3 düğüm de döndürülmektedir ve artık `earthMesh`, `sunMesh` nesnesinin bir alt elemanı olmadığı için ölçeği 5 katına çıkmamaktadır.

**earthOrbit ve Ay (moonOrbit)**

Aynı düzeni uygulamaya devam ederek bir de Ay ekleyelim.

```javascript
+const earthOrbit = new THREE.Object3D();
+earthOrbit.position.x = 10;
+solarSystem.add(earthOrbit);
+objects.push(earthOrbit);

const earthMaterial = new THREE.MeshPhongMaterial({color: 0x2233FF, emissive: 0x112244});
const earthMesh = new THREE.Mesh(sphereGeometry, earthMaterial);
-earthMesh.position.x = 10; // note that this offset is already set in its parent's THREE.Object3D object "earthOrbit"
-solarSystem.add(earthMesh);
+earthOrbit.add(earthMesh);
objects.push(earthMesh);

+const moonOrbit = new THREE.Object3D();
+moonOrbit.position.x = 2;
+earthOrbit.add(moonOrbit);

+const moonMaterial = new THREE.MeshPhongMaterial({color: 0x888888, emissive: 0x222222});
+const moonMesh = new THREE.Mesh(sphereGeometry, moonMaterial);
+moonMesh.scale.set(.5, .5, .5);
+moonOrbit.add(moonMesh);
+objects.push(moonMesh);
```

Yine görünmez sahne grafiği düğümleri (scene graph nodes) ekledik. İlki, `earthOrbit` adında bir `Object3D` düğümüdür ve hem `earthMesh` hem de yine yeni bir düğüm olan `moonOrbit` bunun alt elemanı olarak eklenmiştir. Ardından `moonMesh` nesnesini `moonOrbit` düğümüne ekledik.

### Yardımcılar: AxesHelper ve GridHelper

Sahne grafiğindeki düğümleri görselleştirmek için genellikle bir şeyler çizmek yararlı olur. Three.js, buna yardımcı olmak için bazı yararlı yardımcılara (helpers) sahiptir.

Bunlardan biri `AxesHelper` olarak adlandırılır. Yerel X, Y ve Z eksenlerini temsil eden 3 çizgi çizer. Oluşturduğumuz her düğüme bir tane ekleyelim.

```javascript
// add an AxesHelper to each node
objects.forEach((node) => {
  const axes = new THREE.AxesHelper();
  axes.material.depthTest = false;
  axes.renderOrder = 1;
  node.add(axes);
});
```

Bizim durumumuzda, kürelerin içinde kalsalar bile eksenlerin görünmesini istiyoruz. Bunu yapmak için materyallerinin `depthTest` özelliğini `false` olarak ayarlıyoruz; bu da başka bir şeyin arkasında çizilip çizilmediklerini kontrol etmeyecekleri anlamına gelir. Ayrıca, tüm kürelerden sonra çizilmeleri için `renderOrder` değerlerini 1 olarak ayarlıyoruz (varsayılan değer 0'dır). Aksi takdirde bir küre bunların üzerine çizilebilir ve onları kapatabilir.

Üst üste binen 2 çift eksen olduğu için bazılarını görmek zor olabilir. Hem `sunMesh` hem de `solarSystem` aynı konumdadır. Benzer şekilde `earthMesh` ve `earthOrbit` de aynı konumda bulunur. Her düğüm için bunları açıp kapatmamıza olanak tanıyacak bazı basit kontroller ekleyelim. Bu sırada, `GridHelper` adında başka bir yardımcı daha ekleyelim. Bu yardımcı, X,Z düzleminde 2 boyutlu bir ızgara oluşturur. Varsayılan olarak ızgara 10x10 birim büyüklüğündedir.

Ayrıca three.js projelerinde çok popüler olan `lil-gui` adında bir arayüz kütüphanesi kullanacağız. lil-gui bir nesneyi ve o nesne üzerindeki bir özellik adını alır; özelliğin türüne bağlı olarak bu özelliği değiştirmek için otomatik bir kullanıcı arayüzü oluşturur.

Her düğüm için hem bir `GridHelper` hem de bir `AxesHelper` oluşturmak istiyoruz. Her düğüm için bir etikete ihtiyacımız var, bu yüzden eski döngüyü kaldırıp her düğüme yardımcıları eklemek için belirli bir fonksiyonu çağırmaya geçeceğiz.

```javascript
-// add an AxesHelper to each node
-objects.forEach((node) => {
-  const axes = new THREE.AxesHelper();
-  axes.material.depthTest = false;
-  axes.renderOrder = 1;
-  node.add(axes);
-});

+function makeAxisGrid(node, label, units) {
+  const helper = new AxisGridHelper(node, units);
+  gui.add(helper, 'visible').name(label);
+}
+
+makeAxisGrid(solarSystem, 'solarSystem', 25);
+makeAxisGrid(sunMesh, 'sunMesh');
+makeAxisGrid(earthOrbit, 'earthOrbit');
+makeAxisGrid(earthMesh, 'earthMesh');
+makeAxisGrid(moonOrbit, 'moonOrbit');
+makeAxisGrid(moonMesh, 'moonMesh');
```

### AxisGridHelper sınıfı ve lil-gui

`makeAxisGrid` fonksiyonu, lil-gui'yi memnun etmek için oluşturacağımız bir sınıf olan `AxisGridHelper` nesnesini üretir. Yukarıda belirtildiği gibi lil-gui, bir nesnenin adlandırılmış özelliğini değiştiren bir arayüzü otomatik olarak oluşturacaktır. Özelliğin türüne bağlı olarak farklı bir kullanıcı arayüzü sunar. Biz bir onay kutusu (checkbox) oluşturmasını istediğimizden bool (mantıksal) türünde bir özellik belirtmemiz gerekir. Ancak hem eksenlerin hem de ızgaranın tek bir özelliğe bağlı olarak görünmesini veya kaybolmasını istiyoruz; bu nedenle bir özellik için alıcı (getter) ve ayarlayıcı (setter) metodlarına sahip bir sınıf oluşturacağız. Bu sayede lil-gui'nin tek bir özelliği değiştirdiğini sanmasını sağlarken, arka planda bir düğüme ait hem `AxesHelper` hem de `GridHelper` nesnelerinin `visible` (görünürlük) özelliğini güncelleyebiliriz.

```javascript
// Turns both axes and grid visible on/off
// lil-gui requires a property that returns a bool
// to decide to make a checkbox so we make a setter
// and getter for `visible` which we can tell lil-gui
// to look at.
class AxisGridHelper {
  constructor(node, units = 10) {
    const axes = new THREE.AxesHelper();
    axes.material.depthTest = false;
    axes.renderOrder = 2;  // after the grid
    node.add(axes);

    const grid = new THREE.GridHelper(units, units);
    grid.material.depthTest = false;
    grid.renderOrder = 1;
    node.add(grid);

    this.grid = grid;
    this.axes = axes;
    this.visible = false;
  }
  get visible() {
    return this._visible;
  }
  set visible(v) {
    this._visible = v;
    this.grid.visible = v;
    this.axes.visible = v;
  }
}
```

Dikkat edilmesi gereken bir husus, eksenlerin ızgaradan sonra çizilmesini sağlamak için `AxesHelper` nesnesinin `renderOrder` değerini 2, `GridHelper` nesnesininkini ise 1 olarak ayarlamış olmamızdır. Aksi takdirde ızgara, eksenlerin üzerine çizilip onları kapatabilirdi.

`solarSystem` seçeneğini açtığınızda, Dünya'nın tam da yukarıda ayarladığımız gibi merkezden 10 birim uzakta olduğunu göreceksiniz. Dünya'nın `solarSystem` düğümünün yerel alanında (local space) nasıl yer aldığını görebilirsiniz. Benzer şekilde, `earthOrbit` seçeneğini açtığınızda Ay'ın, `earthOrbit` yerel alanının merkezinden tam 2 birim uzakta olduğunu göreceksiniz.

Arabanın gövdesini (body) hareket ettirirseniz, tüm tekerlekler de onunla birlikte hareket eder. Gövdenin tekerleklerden bağımsız olarak yaylanmasını (bounce) isterseniz, gövdeyi ve tekerlekleri arabanın şasisini temsil eden bir "şasi" (frame) düğümüne bağlayabilirsiniz (parent).

Bir insan için sahne grafiğinin (scene graph) epey karmaşık bir hâl aldığını görebilirsiniz. Hatta yukarıdaki sahne grafiği basitleştirilmiş bir örnektir. Örneğin, her bir parmağı (en az 28 düğüm daha) ve her bir ayak parmağını (yine bir 28 düğüm daha) ek olarak yüz, çene, gözler ve belki daha fazlasını kapsayacak şekilde genişletebilirsiniz.

### Tank örneği

Şimdi yarı karmaşık bir sahne grafiği oluşturalım. Bir tank yapacağız. Tankın 6 tekerleği ve bir kulesi (turret) olacak. Tank bir yolu takip edecek. Etrafta hareket eden bir küre bulunacak ve tank bu küreyi hedef alacak.

Tüm bu düğümlerin kurulumunu görmek için koda bakın.

**Hedef (target) düğümleri**

Tankın nişan aldığı hedef için, yukarıdaki `earthOrbit` düğümüne benzer şekilde dönen bir `targetOrbit` (`Object3D`) bulunur. `targetOrbit` düğümünün bir alt elemanı olan `targetElevation` (`Object3D`), `targetOrbit` düğümünden bir öteleme (offset) ve temel bir yükseklik sağlar. Buna bağlı olan bir diğer `Object3D` düğümü `targetBob` ise `targetElevation` düğümüne göre yalnızca yukarı ve aşağı sallanır (bob). Son olarak, kendi etrafında döndürdüğümüz ve renklerini değiştirdiğimiz basit bir küp olan `targetMesh` yer alır.

```javascript
// move target
targetOrbit.rotation.y = time * .27;
targetBob.position.y = Math.sin(time * 2) * 4;
targetMesh.rotation.x = time * 7;
targetMesh.rotation.y = time * 13;
targetMaterial.emissive.setHSL(time * 0.3 % 1, 1, .25);
targetMaterial.color.setHSL(time * 0.3 % 1, 1, .25);
```

**Tankın yol boyunca hareketi (SplineCurve)**

Tank için, altındaki tüm nesneleri birlikte hareket ettirmek amacıyla `tank` adında bir `Object3D` düğümü kullanılır. Kod, bir `SplineCurve` (eğri) kullanır ve bu eğri boyunca konum bilgisi talep edebilir. 0.0 değeri eğrinin başlangıcını, 1.0 değeri ise eğrinin sonunu temsil eder. Kod önce tankı yerleştireceği mevcut konumu ister, ardından eğri üzerinde biraz daha ilerideki bir konumu sorgulayarak `Object3D.lookAt` yöntemiyle tankın yüzünü o yöne çevirir.

```javascript
const tankPosition = new THREE.Vector2();
const tankTarget = new THREE.Vector2();

...

// move tank
const tankTime = time * .05;
curve.getPointAt(tankTime % 1, tankPosition);
curve.getPointAt((tankTime + 0.01) % 1, tankTarget);
tank.position.set(tankPosition.x, 0, tankPosition.y);
tank.lookAt(tankTarget.x, 0, tankTarget.y);
```

**Taret ve taret kamerası**

Tankın üzerindeki taret (turret), `tank` düğümünün bir alt elemanı (child) olduğu için otomatik olarak tankla birlikte hareket eder. Tareti hedefe yöneltmek için yapmamız gereken tek şey hedefin dünya konumunu (world position) almak ve ardından yeniden `Object3D.lookAt` yöntemini kullanmaktır.

```javascript
const targetPosition = new THREE.Vector3();

...

// face turret at target
targetMesh.getWorldPosition(targetPosition);
turretPivot.lookAt(targetPosition);
```

Taret kamerası (`turretCamera`), `turretMesh` nesnesinin bir alt elemanı (child) olduğu için taretle birlikte hem yukarı-aşağı hareket eder hem de döner. Bu kamerayı doğrudan hedefe nişan alacak şekilde ayarlıyoruz.

```javascript
// make the turretCamera look at target
turretCamera.lookAt(targetPosition);
```

**Hedef kamerası**

Ayrıca `targetBob` düğümünün bir alt elemanı olan bir `targetCameraPivot` vardır; yani hedefle birlikte süzülür. Bunu da geriye, tanka doğru yöneltiyoruz. Amacı, `targetCamera` kamerasının hedefin kendisinden bir miktar uzakta konumlanabilmesini sağlamaktır. Kamerayı bunun yerine `targetBob` düğümünün bir alt elemanı yapıp doğrudan kameranın kendisini yöneltmiş olsaydık, kamera hedefin içinde kalırdı.

```javascript
// make the targetCameraPivot look at the tank
tank.getWorldPosition(targetPosition);
targetCameraPivot.lookAt(targetPosition);
```

**Tekerleklerin döndürülmesi**

Son olarak, tüm tekerlekleri döndürüyoruz.

```javascript
wheelMeshes.forEach((obj) => {
  obj.rotation.x = time * 3;
});
```

**Kameralar arasında geçiş**

Kameralar için, başlatma sırasında açıklamalarıyla birlikte 4 kameranın tümünü içeren bir dizi hazırlıyoruz.

```javascript
const cameras = [
  { cam: camera, desc: 'detached camera', },
  { cam: turretCamera, desc: 'on turret looking at target', },
  { cam: targetCamera, desc: 'near target looking at tank', },
  { cam: tankCamera, desc: 'above back of tank', },
];

const infoElem = document.querySelector('#info');
```

Son olarak, tüm kameralar arasında işleme (render) anında sırayla geçiş yapıyoruz.

```javascript
const camera = cameras[time * .25 % cameras.length | 0];
infoElem.textContent = camera.desc;
```

### Sonuç ve değerlendirme

Umarım bu açıklama, sahne grafiklerinin (scene graphs) nasıl çalıştığı ve bunları nasıl kullanabileceğiniz konusunda faydalı bir fikir vermiştir. `Object3D` düğümleri oluşturmak ve nesneleri bunlara bağlamak (parenting), Three.js gibi bir 3B motoru verimli bir şekilde kullanmanın en önemli adımlarından biridir.

Çoğu zaman bir nesneyi istediğiniz gibi hareket ettirmek ve döndürmek için karmaşık matematiksel hesaplamalar gerekiyormuş gibi görünebilir. Örneğin, bir sahne grafiği kullanmadan Ay'ın hareketini veya araba tekerleklerinin gövdeye göre konumunu hesaplamak oldukça karmaşıktır; ancak bir sahne grafiği kullandığınızda bu işlemler son derece kolaylaşır.

---

## 6. Özet

**Three.js'in yapısı (Architecture):** Three.js bir web sayfasına 3B içerik eklemeyi kolaylaştıran bir kütüphanedir. Temel mimarisi bir Renderer (Çizici) ve bir Scene Graph (Sahne Grafiği) üzerine kuruludur.

- **Renderer:** Sahneyi (Scene) ve Kamerayı (Camera) alarak 2B bir görüntüyü ekrandaki `<canvas>` etiketine çizer.
- **Scene Graph:** Nesnelerin ebeveyn-çocuk hiyerarşisini tutan ağaç yapısıdır. Sahne; Camera, Mesh (Ağ), Light (Işık) ve Group gibi nesneleri barındırır.
- **Mesh:** Bir geometriden (Geometry - köşe noktaları/şekil) ve bir malzemeden (Material - renk, kaplama, parlaklık) oluşan 3B nesnedir.

**Temel kurulum ve kullanım (Setup & Usage):**

- ES6 modülleri (`import * as THREE from 'three';`) ve HTML5 `<canvas>` etiketi kullanılarak başlatılır.
- `WebGLRenderer` oluşturulup ilgili canvas'a bağlanır.
- Perspektif kamera (`PerspectiveCamera`) tanımlanırken görüş alanı (fov), en-boy oranı (aspect), yakın (near) ve uzak (far) kırpma düzlemleri ayarlanır.
- Sahneye nesne (`BoxGeometry` ve `MeshBasicMaterial` ile küp) eklenip kameranın durduğu konum (`position.z`) ayarlanarak render döngüsü çalıştırılır.

Özetle Three.js; WebGL'in karmaşık yapısını soyutlayarak sahne, kamera, nesne geometrisi, malzeme ve ışık nesnelerini kolayca bir araya getirip tarayıcıda 3B sahneler oluşturmayı sağlar.
