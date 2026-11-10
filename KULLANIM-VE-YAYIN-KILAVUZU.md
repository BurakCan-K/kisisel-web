# Burak Can KAHRAMAN Kullanım, İçerik Yönetimi, Vercel Yayın ve Güvenlik Kılavuzu

Bu belge, projeyi daha sonra açtığınızda nereden başlayacağınızı bilmeniz için hazırlanmıştır. Sayfa metinlerini değiştirme, tasarımı düzenleme, yeni sayfa veya blog yazısı ekleme, görsel kullanma, yerel test, GitHub üzerinden sürüm yönetimi, Vercel yayını ve gizli bilgilerin güvenli saklanması konularını kapsar.

> **Teknik sürüm notu:** Proje ilk isteklerde Astro 5 olarak tanımlanmış olsa da güncel `package.json` dosyasında Astro `7.2.3` ve Tailwind CSS `4.3.3` kullanılmaktadır. Bu kılavuz, projenin gerçekten kurulu olan mevcut sürümüne göre yazılmıştır.

## 1. En kısa çalışma akışı

Bir değişiklik yapmak istediğinizde genel sıra şudur:

1. Terminali proje klasöründe açın.
2. `npm run dev` ile siteyi yerelde başlatın.
3. İlgili dosyayı düzenleyin ve tarayıcıda sonucu kontrol edin.
4. `npm run build` ile üretim derlemesini doğrulayın.
5. Değişiklikleri Git ile commit edip GitHub'a gönderin.
6. GitHub'a bağlı Vercel projesi otomatik olarak yeni sürümü yayınlar.

Proje klasörü:

```text
C:\Users\polat\Documents\GitHub\kisisel-web\ecliptic-equinox
```

PowerShell ile başlatma:

```powershell
cd C:\Users\polat\Documents\GitHub\kisisel-web\ecliptic-equinox
npm install
npm run dev
```

Tarayıcı adresi:

```text
http://localhost:4321
```

Terminali kapatmadan sunucuyu durdurmak için `Ctrl + C` kullanın. Projeye özel arka plan modunu tercih ederseniz:

```powershell
npx astro dev --background
npx astro dev status
npx astro dev stop
```

## 2. Proje haritası: Hangi dosya ne işe yarar?

```text
ecliptic-equinox/
├── public/
│   ├── favicon.ico
│   ├── favicon.svg
│   └── og-image.png
├── src/
│   ├── content/
│   │   └── blog/
│   │       └── ilk-yazimiz.md
│   ├── layouts/
│   │   └── BaseLayout.astro
│   ├── pages/
│   │   ├── blog/
│   │   │   ├── index.astro
│   │   │   └── [...slug].astro
│   │   ├── index.astro
│   │   ├── projeler.astro
│   │   ├── hizmetler.astro
│   │   └── iletisim.astro
│   ├── styles/
│   │   └── global.css
│   └── content.config.ts
├── .gitignore
├── astro.config.mjs
├── package.json
└── KULLANIM-VE-YAYIN-KILAVUZU.md
```

### Temel dosyaların görevleri

| Dosya | Görevi | Ne zaman değiştirilir? |
|---|---|---|
| `src/layouts/BaseLayout.astro` | Header, navigasyon, footer ve ortak SEO etiketleri | Site adı, menü, sosyal bağlantılar veya ortak düzen değişince |
| `src/styles/global.css` | Renk değişkenleri, arka plan, font ve global davranış | Marka renkleri veya genel görünüm değişince |
| `src/pages/index.astro` | Ana sayfa | Karşılama, öne çıkan projeler veya ana sayfa hizmetleri değişince |
| `src/pages/projeler.astro` | Proje listesi | Proje eklerken veya proje bilgisi değiştirirken |
| `src/pages/hizmetler.astro` | Hizmet listesi | Hizmet başlığı, açıklaması veya maddeleri değişince |
| `src/pages/iletisim.astro` | İletişim bilgileri ve Formspree formu | E-posta, konum, sosyal hesap veya form kimliği değişince |
| `src/content/blog/*.md` | Blog yazılarının gerçek içeriği | Yazı eklerken veya düzenlerken |
| `src/content.config.ts` | Blog yazılarında zorunlu metadata şeması | Yeni bir frontmatter alanı eklemek istediğinizde |
| `src/pages/blog/index.astro` | Blog listeleme görünümü | Blog kartlarının görünümü değişince |
| `src/pages/blog/[...slug].astro` | Blog detay şablonu | Bütün yazıların ortak okuma görünümü değişince |
| `public/` | Doğrudan yayınlanan statik dosyalar | Görsel, favicon veya sosyal paylaşım görseli eklerken |

## 3. Mevcut metinleri ve bilgileri değiştirme

### 3.1 Site adı, logo metni ve navigasyon

`src/layouts/BaseLayout.astro` dosyasını açın.

Navigasyon bağlantıları dosyanın üst kısmındaki `navigation` dizisindedir:

```ts
const navigation = [
  { label: 'Ana Sayfa', href: '/' },
  { label: 'Projeler', href: '/projeler' },
  { label: 'Hizmetler', href: '/hizmetler' },
  { label: 'Blog', href: '/blog' },
  { label: 'İletişim', href: '/iletisim' },
];
```

- `label`: Menüde görünen metin.
- `href`: Bağlantının gittiği adres.
- Yeni bağlantı eklediğinizde numaralandırma otomatik oluşur.

Logo alanındaki `BURAK CAN/KAHRAMAN`, footer metni, GitHub ve LinkedIn adresleri de aynı dosyadadır. `https://github.com/` gibi genel adresleri kendi profil adreslerinizle değiştirin.

### 3.2 Tasarım renklerini değiştirme

`src/styles/global.css` içindeki değişkenler tasarımın temel paletidir:

```css
:root {
  --paper: #f3f0e8;
  --ink: #171713;
  --muted: #65645d;
  --line: #cfcbc0;
  --signal: #f4511e;
}
```

- `--paper`: Sayfanın kâğıt rengi.
- `--ink`: Ana yazı ve koyu yüzey rengi.
- `--muted`: İkincil metin rengi.
- `--line`: İnce çerçeve ve ayırıcı rengi.
- `--signal`: Turuncu vurgu rengi.

Sayfalarda Tailwind içinde aynı renklerin doğrudan yazıldığı `[#f4511e]`, `[#171713]` gibi sınıflar da vardır. Paleti tamamen değiştirirken bu değerleri proje genelinde arayıp değiştirmeniz gerekir.

VS Code genel arama kısayolu:

```text
Ctrl + Shift + F
```

Örneğin bütün turuncu vurguları bulmak için `#f4511e` aratın.

### 3.3 Ana sayfayı değiştirme

Ana sayfa: `src/pages/index.astro`

Dosyanın üst kısmında `projects` ve `services` dizileri bulunur. İçerik eklemek için mevcut nesnelerden birini kopyalayabilirsiniz:

```ts
{
  title: 'Yeni Proje',
  description: 'Projenin kısa açıklaması.',
  technologies: ['Astro', 'Tailwind CSS'],
  href: '/projeler',
}
```

Hero başlığı `<h1>` etiketinin içindedir. SEO açısından sayfada yalnızca bir `<h1>` bırakın. Alt bölüm başlıklarında `<h2>`, kart başlıklarında `<h3>` kullanın.

### 3.4 Proje ekleme veya değiştirme

Dosya: `src/pages/projeler.astro`

Üst taraftaki `projects` dizisine şu yapıda yeni bir kayıt ekleyin:

```ts
{
  title: 'Proje Adı',
  description: 'Projenin ne yaptığı ve hangi problemi çözdüğü.',
  technologies: ['Astro', 'TypeScript', 'Tailwind CSS'],
  github: 'https://github.com/KULLANICI_ADI/REPO_ADI',
  demo: 'https://proje-adresi.vercel.app',
},
```

Henüz canlı demosu yoksa yanlış veya yanıltıcı bir adres vermek yerine bağlantıyı geçici olarak kart şablonundan kaldırın. Gizli bir GitHub reposunun adresini paylaşmak güvenlik açığı oluşturmaz ancak ziyaretçi bağlantıyı açamaz.

### 3.5 Hizmetleri değiştirme

Dosya: `src/pages/hizmetler.astro`

Her hizmet şu alanlara sahiptir:

```ts
{
  number: '04',
  title: 'Yeni Hizmet',
  description: 'Hizmetin ayrıntılı açıklaması.',
  details: ['Birinci çıktı', 'İkinci çıktı', 'Üçüncü çıktı'],
},
```

Numaraları sıralı tutun. `details` dizisine daha fazla madde eklenebilir; görünüm otomatik olarak uyum sağlar.

### 3.6 İletişim bilgileri ve form

Dosya: `src/pages/iletisim.astro`

Değiştirmeniz gereken örnek değerler:

- `merhaba@example.com` → gerçek e-posta adresiniz.
- `İstanbul, Türkiye` → istediğiniz konum bilgisi.
- `socialLinks` dizisi → gerçek GitHub, LinkedIn ve X adresleriniz.
- `YOUR_FORM_ID` → Formspree üzerinde oluşturduğunuz form kimliği.

Form adresi şu şekilde görünmelidir:

```astro
<form action="https://formspree.io/f/GERCEK_FORM_ID" method="POST">
```

Formspree kimliği formun gönderim adresidir; parola gibi değerlendirilmez. Buna rağmen Formspree panelinde spam korumasını, izin verilen domaini ve gönderim limitlerini yapılandırın. Bir e-posta parolasını veya özel API anahtarını HTML formuna kesinlikle yazmayın.

## 4. Yeni normal sayfa ekleme

Astro dosya tabanlı yönlendirme kullanır. `src/pages/deneyim.astro` dosyası oluşturursanız adres otomatik olarak `/deneyim` olur.

Başlangıç şablonu:

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
---

<BaseLayout
  title="Deneyim | Burak Can KAHRAMAN"
  description="Yazılım geliştirme deneyimim ve çalışma geçmişim."
>
  <section aria-labelledby="page-title">
    <p class="font-mono text-[10px] font-bold tracking-[0.18em] text-[#f4511e] uppercase">
      Kariyer / Zaman çizelgesi
    </p>
    <h1
      id="page-title"
      class="mt-3 text-5xl font-black tracking-[-0.06em] text-[#171713] sm:text-7xl"
    >
      Deneyim
    </h1>

    <div class="mt-12 border-t border-[#171713] pt-8">
      <h2 class="text-2xl font-bold text-[#171713]">Bölüm Başlığı</h2>
      <p class="mt-4 max-w-2xl leading-7 text-[#65645d]">
        Sayfa içeriği burada yer alır.
      </p>
    </div>
  </section>
</BaseLayout>
```

Ardından `BaseLayout.astro` içindeki `navigation` dizisine ekleyin:

```ts
{ label: 'Deneyim', href: '/deneyim' },
```

Kontrol listesi:

- Sayfada tam olarak bir `<h1>` bulunmalı.
- `title` ve `description` özgün olmalı.
- Bölümler `<section>`, bağımsız içerikler `<article>` ile işaretlenmeli.
- Görsellerde anlamlı `alt` metni olmalı.
- Bağlantı metni “buraya tıkla” yerine hedefi anlatmalı.
- Mobil görünüm Tailwind `sm:`, `md:` ve `lg:` sınıflarıyla kontrol edilmeli.

### Alt dizinde sayfa ekleme

`src/pages/hakkimda/ekipman.astro` şu adresi üretir:

```text
/hakkimda/ekipman
```

Bu durumda layout import yolu iki seviye çıkmalıdır:

```astro
import BaseLayout from '../../layouts/BaseLayout.astro';
```

## 5. Blog yazısı ekleme ve yönetme

Astro Content Collections, benzer yapıdaki Markdown içeriklerini doğrulamak, sorgulamak ve render etmek için kullanılır. Bu projede blog yazıları `src/content/blog/` altında tutulur ve `src/content.config.ts` şemasıyla kontrol edilir.

### 5.1 Yeni yazı oluşturma

`src/content/blog/` altında küçük harfli, boşluksuz ve Türkçe karakter içermeyen bir dosya oluşturun:

```text
astro-performans-rehberi.md
```

Dosyanın tam örneği:

````markdown
---
title: "Astro ile Web Performansını İyileştirme"
description: "Astro projelerinde daha hızlı sayfalar oluşturmak için uygulanabilir teknikler."
pubDate: 2026-08-20
tags: ["Astro", "Performans", "Frontend"]
---

Giriş paragrafı burada yer alır.

## İlk Bölüm

Normal paragraf metni.

### Alt Başlık

- Birinci madde
- İkinci madde

```ts
const message = 'Kod örneği';
```

[Astro dokümantasyonu](https://docs.astro.build)
````

Dosya adı URL olur:

```text
src/content/blog/astro-performans-rehberi.md
→ /blog/astro-performans-rehberi/
```

### 5.2 Frontmatter alanları

| Alan | Zorunlu | Biçim | Açıklama |
|---|---:|---|---|
| `title` | Evet | Metin | Yazının sayfa ve SEO başlığı |
| `description` | Evet | Metin | Liste kartı ve SEO açıklaması |
| `pubDate` | Evet | `YYYY-MM-DD` | Sıralama ve yayın tarihi |
| `tags` | Hayır | Metin dizisi | Konu etiketleri; verilmezse boş dizi |

Bir alan eksik veya yanlış türdeyse `npm run build` hata vererek hatalı içeriğin yayınlanmasını engeller.

### 5.3 Yazı sıralaması

`src/pages/blog/index.astro` içindeki şu kod yazıları yeniden eskiye sıralar:

```ts
const posts = (await getCollection('blog')).sort(
  (a, b) => b.data.pubDate.valueOf() - a.data.pubDate.valueOf(),
);
```

Sıralamayı değiştirmek istemiyorsanız bu bölüme dokunmayın.

### 5.4 Blog görseli ekleme

Görseli örneğin şu konuma koyun:

```text
public/blog/astro-performans.webp
```

Markdown içinde:

```markdown
![Astro performans ölçüm ekranı](/blog/astro-performans.webp)
```

Öneriler:

- Fotoğraflarda WebP veya AVIF kullanın.
- Dosya adlarında boşluk ve Türkçe karakter kullanmayın.
- Görseli gereksiz yüksek çözünürlükte yüklemeyin.
- Başkasına ait görselleri lisansını kontrol etmeden kullanmayın.
- `alt` metninde görselin içeriğini kısaca anlatın.

### 5.5 Taslak yazıları saklama

Mevcut sistem `src/content/blog/` altındaki bütün `.md` dosyalarını yayınlar. Henüz yayınlamak istemediğiniz yazılar için en güvenli ve basit yöntem, dosyayı bu dizinin dışında yerel bir taslak klasöründe tutmaktır. Örneğin:

```text
C:\Users\polat\Documents\Blog-Taslaklari\yeni-yazi.md
```

Hazır olduğunda dosyayı `src/content/blog/` içine kopyalayın. Taslağı repo içinde tutmak istiyorsanız ileride şemaya `draft: boolean` alanı ve sayfalara filtre eklenmelidir. Sadece frontmatter'a `draft: true` yazmak mevcut kodda yayını durdurmaz.

## 6. Markdown dosyaları Vercel'de nasıl saklanır?

### Kısa cevap

Blog Markdown dosyalarının asıl saklama yeri **GitHub reposudur**. Vercel kalıcı içerik editörü veya dosya deposu olarak kullanılmaz. Her deploy sırasında Vercel GitHub'dan projeyi çeker, Markdown dosyalarını okur ve statik HTML sayfalar üretir.

Bu projede akış şöyledir:

```text
Yerel .md dosyası
      ↓ git commit / push
GitHub reposu
      ↓ Vercel otomatik build
Herkese açık statik HTML blog sayfası
```

Vercel üzerinde çalışan derlemenin dosya sistemine sonradan `.md` dosyası yazmayı denemeyin. Yeni yazı eklemek için Markdown dosyasını yerelde veya GitHub üzerinden ekleyip yeni deploy oluşturmalısınız.

### Public repo mu private repo mu?

- **Public repo:** Markdown kaynakları ve site kodu GitHub'da herkes tarafından görülebilir.
- **Private repo:** Kaynak `.md` dosyalarını ve kodu yalnızca erişim verdiğiniz kişiler görür. Vercel'e GitHub reposunu okuma izni vererek private repo da yayınlayabilirsiniz.
- Her iki durumda da Vercel'de yayınlanan blog yazısının üretilmiş HTML içeriği ziyaretçilere açıktır.

Private repo, yayımlanmış içeriği gizlemez; yalnızca kaynak kodun GitHub'daki görünürlüğünü sınırlar.

### Markdown içinde ne saklanabilir?

Markdown blog dosyalarında şunlar bulunabilir:

- Herkese açık yazı metni
- Başlık ve açıklama
- Etiketler
- Herkese açık görsel yolları
- Herkese açık bağlantılar

Şunları Markdown içine **kesinlikle yazmayın**:

- Parola
- API anahtarı veya token
- Veritabanı bağlantı adresi
- TC kimlik numarası veya özel müşteri bilgisi
- Özel e-posta konuşmaları
- Formspree, GitHub veya Vercel hesap erişim tokenları
- Henüz duyurulmaması gereken ticari bilgi

Unutmayın: Bir gizli bilgiyi daha sonra dosyadan silmek yeterli olmayabilir; değer Git geçmişinde eski commit içinde kalabilir.

## 7. Ortam değişkenleri ve gizli bilgiler

### 7.1 Yerel geliştirme

Gerekirse proje kökünde `.env.local` oluşturun:

```env
API_SECRET=gercek-gizli-deger
PUBLIC_SITE_URL=http://localhost:4321
```

Bu projedeki `.gitignore`, `.env`, `.env.local`, `.env.production` gibi bütün `.env*` dosyalarını Git dışında bırakır. Yalnızca gerçek değer içermeyen `.env.example` dosyasına izin verir.

Örnek dosya:

```env
API_SECRET=
PUBLIC_SITE_URL=
```

Astro'da `PUBLIC_` ile başlayan değişkenler tarayıcı tarafına açılabilir. Bu nedenle parola, özel token ve gizli anahtarlarda `PUBLIC_` ön eki kullanmayın.

Statik site açısından daha önemli kural şudur: Bir gizli değeri sayfanın HTML çıktısına yazdırırsanız değişkenin adı gizli olsa bile sonuç ziyaretçiye görünür. Gizli değişkenleri yalnızca güvenilir sunucu tarafı işlemlerde kullanın.

### 7.2 Vercel üzerinde

Vercel Dashboard içinde:

```text
Project → Settings → Environment Variables
```

Gizli değeri buraya ekleyin; kaynak kod içine yazmayın. Ortamı doğru seçin:

- `Production`: `main` dalından yayınlanan canlı site.
- `Preview`: diğer Git dallarının önizlemeleri.
- `Development`: yerel Vercel geliştirme ortamı.

API anahtarı gibi gerçek sırları eklerken Vercel'deki **Sensitive** seçeneğini etkinleştirin. Sensitive değerler daha sonra panelde okunamaz biçimde saklanır. Değişken güncellemesi eski deployment'lara otomatik uygulanmaz; yeni deployment gerekir.

### 7.3 Yanlışlıkla secret commit edilirse

1. Önce ilgili anahtarı hizmet sağlayıcısının panelinden iptal edin veya döndürün.
2. Yeni anahtarı yalnızca Vercel Environment Variables bölümüne ekleyin.
3. Dosyadan silip yeni commit oluşturun.
4. Gerekirse Git geçmişinden de temizleyin.
5. GitHub secret scanning uyarılarını kontrol edin.

Sadece commit'i silmek veya repoyu private yapmak, sızmış anahtarı tekrar güvenli hâle getirmez. Öncelik anahtarı iptal etmektir.

## 8. Git ve GitHub ile güvenli değişiklik akışı

### Basit akış

```powershell
git status
git add .
git commit -m "Blog yazısı ekle"
git push
```

`git add .` çalıştırmadan önce `git status` çıktısını okuyun. `.env.local`, kişisel not, büyük video veya parola dosyası görünüyorsa commit etmeyin.

### Daha güvenli branch ve Vercel Preview akışı

Canlı siteyi doğrudan değiştirmemek için:

```powershell
git switch -c guncelleme/yeni-blog-yazisi
```

Değişiklikleri yaptıktan sonra:

```powershell
npm run build
git add .
git commit -m "Yeni blog yazısı ekle"
git push -u origin guncelleme/yeni-blog-yazisi
```

Vercel bu dal için ayrı bir Preview URL oluşturur. Önizlemeyi kontrol ettikten sonra GitHub'da Pull Request açıp `main` dalına birleştirin. `main` güncellendiğinde production deploy oluşur.

## 9. Vercel'e ilk yayın

### 9.1 Ön koşullar

- Kod GitHub reposuna gönderilmiş olmalı.
- `npm run build` yerelde başarılı olmalı.
- Örnek iletişim değerleri gerçek bilgilerle değiştirilmiş olmalı.
- Formspree kullanılacaksa `YOUR_FORM_ID` değiştirilmiş olmalı.

### 9.2 Vercel Dashboard adımları

1. Vercel hesabınıza GitHub ile giriş yapın.
2. **Add New → Project** seçin.
3. GitHub'daki `kisisel-web` reposunu bulun ve **Import** seçin.
4. Ayarları kontrol edin:

| Ayar | Değer |
|---|---|
| Framework Preset | `Astro` |
| Root Directory | Repo kökü ise boş bırakın |
| Install Command | Otomatik veya `npm install` |
| Build Command | `npm run build` |
| Output Directory | `dist` |
| Production Branch | `main` |

5. Gerekli ortam değişkenleri varsa ekleyin.
6. **Deploy** seçin.

Bu proje statik Astro sitesi olduğu için Vercel adapter zorunlu değildir. Server-side rendering, Vercel Functions veya Vercel Image Optimization gibi sunucu özellikleri eklerseniz adapter gerekebilir.

### 9.3 Sonraki yayınlar

GitHub bağlantısından sonra:

```powershell
git add .
git commit -m "Site içeriğini güncelle"
git push
```

`main` dalına yapılan push yeni production deploy başlatır. Başka dallar Preview deployment oluşturur.

### 9.4 Yayın hatası olursa

Vercel Dashboard → Project → Deployments → başarısız deployment → Build Logs yolunu izleyin.

Sık nedenler:

- Markdown frontmatter alanının eksik olması.
- Tarihin yanlış yazılması.
- Astro veya TypeScript sözdizimi hatası.
- Eksik environment variable.
- Büyük/kırık dosya yolu.
- Dosya adında büyük-küçük harf farkı. Windows bunu bazen fark etmez, Linux tabanlı build ortamı fark eder.

Önce yerelde aynı kontrolü yapın:

```powershell
npm run build
```

## 10. SEO ve domain sonrası yapılacaklar

Özel domain bağlandıktan sonra:

1. `astro.config.mjs` içinde `site` adresi tanımlamayı değerlendirin.
2. Sayfalara gerçek canonical URL aktarın.
3. `public/og-image.png` sosyal paylaşım görselini güncel tutun.
4. Favicon dosyalarını kişisel marka ile değiştirin.
5. Sayfa başlıklarını benzersiz tutun.
6. Her sayfada tek `<h1>` kullanın.
7. İleride sitemap için `@astrojs/sitemap` eklenebilir.
8. `robots.txt` ve Google Search Console kurulabilir.

`public/` altındaki bir dosyanın yolu kökten başlar:

```text
public/og-image.png → /og-image.png
public/cv.pdf        → /cv.pdf
```

`public/` dizinine koyduğunuz her dosyanın doğrudan internetten erişilebilir olacağını varsayın. Gizli belgeyi `public/` içine koymayın.

## 11. Yedekleme ve geri alma

### GitHub ana yedektir, fakat tek yedek olmak zorunda değildir

- Düzenli olarak GitHub'a push edin.
- Önemli içeriklerin ayrıca yerel veya güvenilir bulut yedeğini tutun.
- Blog görsellerinin orijinallerini ayrı bir arşivde saklayın.
- Repo erişiminde iki faktörlü doğrulamayı etkinleştirin.

### Son commit'i inceleme

```powershell
git log --oneline -10
git show COMMIT_KIMLIGI
```

### Güvenli geri alma

Canlıya giden hatalı commit'i geçmişi silmeden geri almak için:

```powershell
git revert COMMIT_KIMLIGI
git push
```

Bu işlem hatalı değişikliğin tersini yeni bir commit olarak oluşturur. Paylaşılan geçmişte `git reset --hard` kullanmayın.

Vercel Dashboard üzerinden önceki başarılı deployment'a geçici rollback de yapılabilir; kalıcı çözüm için Git tarafındaki hatalı değişikliği de düzeltin.

## 12. Yayın öncesi güvenlik ve kalite kontrol listesi

Her production yayından önce:

- [ ] `npm run build` başarılı.
- [ ] `git status` içinde `.env*`, parola veya özel belge yok.
- [ ] Markdown frontmatter alanları doğru.
- [ ] Taslak yazı yanlışlıkla `src/content/blog/` altında değil.
- [ ] İletişim e-postası ve sosyal bağlantılar doğru.
- [ ] `YOUR_FORM_ID` örnek değeri kaldırıldı veya form geçici olarak kapatıldı.
- [ ] Proje/demo bağlantıları gerçek hedeflere gidiyor.
- [ ] Görseller sıkıştırılmış ve alt metinleri var.
- [ ] Her sayfada bir `<h1>` var.
- [ ] Mobil görünüm kontrol edildi.
- [ ] Gizli değerler Vercel Environment Variables içinde.
- [ ] Gizli değerlerde `PUBLIC_` ön eki kullanılmadı.
- [ ] `public/` içinde özel veya kişisel belge yok.
- [ ] Vercel Preview adresi kontrol edildi.
- [ ] `main` dalına yalnızca doğrulanmış değişiklik birleştirildi.

## 13. Yeni özellik eklerken izlenecek yöntem

Yeni bir fikir geldiğinde doğrudan birçok dosyayı değiştirmek yerine şu sırayı izleyin:

1. Özelliğin hangi sayfada görüneceğini belirleyin.
2. Sadece içerik değişiyorsa mevcut veri dizisini veya Markdown dosyasını kullanın.
3. Aynı arayüz birden fazla yerde kullanılacaksa `src/components/` altında bileşen oluşturun.
4. Gizli veya kalıcı veri gerekip gerekmediğini belirleyin.
5. Önce yerelde küçük bir sürüm oluşturun.
6. Mobil, klavye ve okunabilirlik kontrolü yapın.
7. `npm run build` çalıştırın.
8. Ayrı branch'e push edip Vercel Preview'da deneyin.
9. Sonra `main` dalına alın.

### Ne zaman bileşen oluşturulmalı?

Örneğin aynı proje kartını ana sayfada ve projeler sayfasında kullanacaksanız:

```text
src/components/ProjectCard.astro
```

Basit bileşen yapısı:

```astro
---
interface Props {
  title: string;
  description: string;
}

const { title, description } = Astro.props;
---

<article class="border-b border-[#171713] py-6">
  <h2 class="text-2xl font-bold text-[#171713]">{title}</h2>
  <p class="mt-3 text-[#65645d]">{description}</p>
</article>
```

Kullanımı:

```astro
---
import ProjectCard from '../components/ProjectCard.astro';
---

<ProjectCard title="Proje Adı" description="Kısa açıklama" />
```

## 14. Resmî başvuru kaynakları

- [Astro Content Collections](https://docs.astro.build/en/guides/content-collections/)
- [Astro Markdown içeriği](https://docs.astro.build/en/guides/markdown-content/)
- [Astro sayfa ve routing](https://docs.astro.build/en/guides/routing/)
- [Astro'yu Vercel'de yayınlama](https://vercel.com/docs/frameworks/frontend/astro)
- [Vercel Git deployments](https://vercel.com/docs/git)
- [Vercel Environment Variables](https://vercel.com/docs/environment-variables)
- [Vercel Sensitive Environment Variables](https://vercel.com/docs/environment-variables/sensitive-environment-variables)
- [GitHub'da hassas verileri koruma](https://docs.github.com/en/code-security/secret-scanning/introduction/about-secret-scanning)

## 15. Türkçe ve İngilizce içerik yönetimi

Türkçe varsayılan dildir ve `/` altında yayınlanır. İngilizce sayfalar `/en/` altında bulunur. Header içindeki `TR` / `EN` bağlantısı ziyaretçiyi aynı sayfanın diğer dildeki karşılığına götürür.

Ortak arayüz metinleri ve bütün sayfa içerikleri şu dosyada tutulur:

```text
src/i18n/config.ts
```

Dosyada `tr` ve `en` olmak üzere iki ayrı sözlük vardır. Bir Türkçe metni değiştirdiğinizde aynı alanın İngilizce karşılığını da güncelleyin. Tasarım bileşenleri `src/components/pages/` altında ortak olduğu için HTML ve Tailwind sınıflarını iki defa düzenlemeniz gerekmez.

Rota eşlemesi de `src/i18n/config.ts` içindeki `routes` nesnesindedir:

```text
/hizmetler  ↔ /en/services
/projeler   ↔ /en/projects
/iletisim   ↔ /en/contact
/blog       ↔ /en/blog
```

Yeni bir blog yazısında dil ve çeviri eşlemesi zorunludur:

```yaml
lang: "tr"
translationKey: "benzersiz-yazi-anahtari"
```

İngilizce karşılığı `src/content/blog/en/` altında bulunmalı, `lang: "en"` kullanmalı ve aynı `translationKey` değerini taşımalıdır. İngilizce çevirisi olmayan Türkçe yazılar İngilizce blog listesinde gösterilmez.

Yeni normal bir sayfa eklerken hem Türkçe hem İngilizce route wrapper dosyası oluşturun ve ortak sayfa bileşenini iki rotada farklı `lang` değeriyle çağırın. Sayfa başlıkları, açıklamalar, form etiketleri, tarih biçimleri, navigasyon ve footer otomatik olarak seçilen dile göre değişir.

---

Bu kılavuzdaki en önemli iki kural:

1. **Blog yazısı gibi herkese açık içerikleri GitHub'daki Markdown dosyalarında saklayın ve Git üzerinden yayınlayın.**
2. **Parola, token ve API anahtarlarını hiçbir zaman Markdown, Astro, `public/` veya Git geçmişine koymayın; Vercel Environment Variables kullanın.**
