# Burak Can KAHRAMAN Tasarım Sistemi — AI Prompt Kiti

Bu kit, bu projedeki görsel dili başka web sitelerine ve uygulamalara taşımak için hazırlanmıştır. OpenAI, ChatGPT, Codex, Gemini, Claude ve benzeri kod üreten modellerde kullanılabilir.

Promptlardaki `[KÖŞELİ PARANTEZ]` alanlarını kendi projenize göre değiştirin. Model mevcut bir kod tabanında çalışacaksa prompt ile birlikte ilgili dosyaları veya repo erişimini verin.

## 1. Tasarım DNA'sı

Tasarımın kısa tanımı:

> Bağımsız bir yazılımcının dijital çalışma defteri: Swiss editorial düzen, teknik saha notu estetiği, sıcak kâğıt yüzeyi, siyah mürekkep, güvenlik turuncusu, monospace mikro metinler ve belirgin numaralandırma.

Bu sistem “minimal” görünür ancak jenerik değildir. Karakterini şu karşıtlıklardan alır:

- Sıcak ve analog zemin / teknik ve dijital bilgi dili
- Çok büyük sans-serif başlık / çok küçük monospace metadata
- Yoğun siyah yüzey / geniş boş alan
- Düz grid / küçük asimetrik hareketler
- İnce çizgiler / tek güçlü turuncu vurgu

## 2. Tasarım tokenları

### Ana renk paleti

| Token | Hex | Kullanım |
|---|---|---|
| Paper | `#F3F0E8` | Ana sayfa zemini |
| Ink | `#171713` | Ana metin, border, koyu yüzeyler |
| Muted | `#65645D` | İkincil metin ve açıklamalar |
| Line | `#CFCBC0` | Hafif ayırıcılar ve pasif border |
| Signal | `#F4511E` | CTA, durum, numara ve vurgu |
| Soft White | `#FFFDF7` | Hover edilen veya yükseltilen yüzey |
| Soft Code | `#DED9CC` | Inline kod ve ikincil dolgu |
| Footer Muted | `#AAA79D` | Koyu yüzeyde ikincil metin |

Önerilen CSS değişkenleri:

```css
:root {
  --paper: #f3f0e8;
  --ink: #171713;
  --muted: #65645d;
  --line: #cfcbc0;
  --signal: #f4511e;
  --soft-white: #fffdf7;
  --soft-code: #ded9cc;
}
```

### Tipografi

- Büyük başlıklar: güçlü grotesk/geometrik sans-serif, `font-black`, sıkı tracking.
- Gövde metni: temiz sans-serif, rahat satır yüksekliği.
- Metadata, etiket, menü, sayaç: monospace, küçük boyut, uppercase, geniş tracking.
- Başlık tracking aralığı: yaklaşık `-0.04em` ile `-0.075em`.
- Gövde satır yüksekliği: `1.7–1.85`.
- Etiketler: `10–12px`, monospace, bold, uppercase.

Harici font yoksa güvenli font yığını:

```css
font-family: Arial, Helvetica, sans-serif;
```

Monospace alanlar:

```css
font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
```

### Ölçü ve yerleşim

- Ana içerik genişliği: `max-width: 64rem` / Tailwind `max-w-5xl`.
- Mobil yatay padding: `20px` / `px-5`.
- Geniş ekran yatay padding: `32px` / `lg:px-8`.
- Ana dikey boşluk: `48–64px`.
- Büyük bölüm aralığı: `96–128px`.
- Teknik grid: `32px × 32px`.
- Border: çoğunlukla `1px solid #171713` veya hafif durumda `#CFCBC0`.
- Köşe yuvarlama: mümkün olduğunca yok; form kontrolü gibi gerekli alanlarda çok düşük.
- Gölge: yumuşak blur yerine sert offset gölge, örneğin `8px 8px 0 #171713`.

### Arka plan grid'i

```css
body {
  background-color: #f3f0e8;
  background-image:
    linear-gradient(rgba(23, 23, 19, 0.035) 1px, transparent 1px),
    linear-gradient(90deg, rgba(23, 23, 19, 0.035) 1px, transparent 1px);
  background-size: 32px 32px;
}
```

## 3. Değişmez tasarım kuralları

AI'ın tasarımı jenerikleştirmemesi için promptlarda bu kuralları koruyun:

1. Ana zemin saf beyaz değil sıcak kâğıt rengidir.
2. Ana metin saf siyah değil `#171713` mürekkep tonudur.
3. Turuncu yalnızca vurgu için kullanılır; geniş alanları gereksiz yere doldurmaz.
4. Kartlar yuvarlak, gölgeli SaaS kartları gibi görünmez.
5. Bölümler ince çizgiler ve bol boşlukla ayrılır.
6. Büyük başlıklar sıkı tracking ve yüksek görsel ağırlık taşır.
7. Küçük teknik metinlerde monospace, uppercase ve numara kullanılır.
8. CTA sayısı sınırlıdır; birincil CTA koyu, ikincil CTA çerçevelidir.
9. Hover hareketleri küçük ve mekaniktir: `translate`, renk tersleme veya hafif rotasyon.
10. Dekorasyon işlevsel hiyerarşiyi destekler; rastgele şekil eklenmez.
11. Mobil görünüm sonradan küçültülmüş masaüstü değil, bilinçli tek kolon düzenidir.
12. Erişilebilir focus state, semantic HTML ve yeterli kontrast zorunludur.

## 4. Kaçınılacak görünüm

Promptun sonuna gerektiğinde şu negatif kuralları ekleyin:

```text
Kaçın:
- jenerik startup/SaaS landing page görünümü
- mavi-mor gradientler
- glassmorphism ve blur kartlar
- her bileşende büyük border-radius
- yumuşak ve dağınık drop-shadow
- anlamsız 3D objeler
- stok fotoğraf hissi
- aşırı ikon kullanımı
- aynı boyutta kartlardan oluşan monoton grid
- gereksiz animasyon ve ağır JavaScript
- düşük kontrastlı gri metin
- dekoratif ama işlevsiz dashboard grafikleri
```

## 5. Evrensel ana prompt

Aşağıdaki prompt, yeni bir projeyi bu tasarım sisteminde üretmek için temel prompttur:

```text
Sen kıdemli bir ürün tasarımcısı ve frontend mühendisisin.

[PROJE TÜRÜ] için sade, işlevsel ve ayırt edici bir arayüz oluştur. Görsel yön, “bağımsız bir yazılımcının dijital çalışma defteri” olmalı: Swiss editorial grid, teknik saha notu estetiği, sıcak analog kâğıt yüzeyi ve hassas dijital tipografi.

Tasarım tokenları:
- Paper: #F3F0E8 — ana zemin
- Ink: #171713 — ana metin, border ve koyu yüzey
- Muted: #65645D — ikincil metin
- Line: #CFCBC0 — hafif ayırıcı
- Signal: #F4511E — CTA, numara ve durum vurgusu
- Soft White: #FFFDF7 — yükseltilmiş yüzey

Tipografi:
- Büyük başlıklarda ağır grotesk sans-serif, çok sıkı tracking ve kısa satırlar kullan.
- Gövde metninde rahat satır yüksekliği ve maksimum okunabilirlik sağla.
- Menü, etiket, sayaç ve metadata için küçük bold monospace, uppercase ve geniş tracking kullan.

Yerleşim:
- Ana içerik max-width 64rem olsun.
- 32px teknik grid dokusunu çok düşük opaklıkla uygula.
- Bölümleri yuvarlak kartlar yerine çizgiler, numaralar, asimetrik kolonlar ve geniş boşluklarla ayır.
- Mobilde tek kolon; büyük ekranda editoryal asimetrik grid kullan.
- Birincil CTA mürekkep zeminli; ikincil CTA ince mürekkep çerçeveli olsun.
- Hover durumları küçük mekanik hareketler ve renk tersleme kullansın.

İşlev ve erişilebilirlik:
- Semantic HTML kullan.
- Her sayfada yalnızca bir h1 olsun.
- Başlık sırası bozulmasın.
- Klavye ile erişilebilen belirgin focus-visible stilleri ekle.
- Form alanlarında label, doğru input type ve autocomplete kullan.
- prefers-reduced-motion tercihini dikkate al.
- Mobilde taşma veya yatay scroll oluşturma.

İçerik tonu:
- Kısa, doğrudan, özgüvenli ve teknik.
- Pazarlama klişeleri kullanma.
- Gerçekçi başlık, veri ve durum metinleri kullan; lorem ipsum kullanma.

Kaçın:
- jenerik SaaS tasarımı
- mavi/mor gradient
- glassmorphism
- büyük yuvarlak kartlar
- yumuşak drop-shadow
- gereksiz ikon ve illüstrasyon
- ağır JS ve sırf dekoratif animasyon

Teknik ortam: [FRAMEWORK VE SÜRÜM]
CSS yaklaşımı: [TAILWIND / CSS MODULES / VANILLA CSS]
Proje amacı: [KISA PROJE AÇIKLAMASI]
Hedef kullanıcı: [HEDEF KİTLE]
Gerekli sayfalar: [SAYFA LİSTESİ]
Gerekli işlevler: [İŞLEV LİSTESİ]

Önce mevcut yapıyı incele. Tasarım tokenlarını merkezi tanımla, tekrar eden arayüzleri bileşenleştir, ardından bütün sayfaları tutarlı şekilde uygula. Mevcut işlevleri bozma. Son olarak responsive, erişilebilirlik ve build kontrolü yap.
```

## 6. Mevcut projeyi yeniden tasarlama promptu

```text
Mevcut [FRAMEWORK] projemin işlevlerini ve içerik yapısını koruyarak görsel sistemini yeniden tasarla.

Hedef stil: Swiss editorial + teknik field notes + bağımsız üretici estetiği.

Palet:
#F3F0E8 paper, #171713 ink, #65645D muted, #CFCBC0 line, #F4511E signal, #FFFDF7 soft white.

Şunları yap:
1. Önce proje yapısını, ortak layout'u, global CSS'i ve tekrar eden bileşenleri incele.
2. Renkleri merkezi CSS tokenlarına taşı.
3. Header, navigasyon, footer ve ana içerik genişliğini ortak layout üzerinden düzenle.
4. Büyük editorial başlıkları küçük monospace metadata ile eşleştir.
5. Yuvarlak SaaS kartlarını çizgili, numaralı ve daha düz editoryal yapılara dönüştür.
6. Mobil, tablet ve masaüstü düzenlerini ayrı ayrı düşün.
7. Mevcut route, SEO, form ve veri işlevlerini koru.
8. Semantic HTML, tek h1 ve focus-visible kurallarını uygula.
9. Ağır kütüphane veya gereksiz client-side JavaScript ekleme.
10. Sonunda build/test çalıştır ve değiştirdiğin dosyaları özetle.

Tasarım şu hisleri vermeli: bağımsız, teknik, güvenilir, dokunsal, keskin ve sakin.
Tasarım şu hisleri vermemeli: kurumsal şablon, kripto sitesi, jenerik AI startup veya hazır tema.
```

## 7. Sadece tek sayfa üretme promptu

```text
[SAYFA ADI] sayfasını aşağıdaki tasarım sistemiyle oluştur:

- Sıcak kâğıt zemin #F3F0E8
- Mürekkep #171713
- İkincil metin #65645D
- Çizgiler #CFCBC0
- Tek vurgu rengi #F4511E
- Çok büyük, sıkı tracking'li sans-serif h1
- Küçük uppercase monospace bölüm etiketi
- Yuvarlak kart yerine border, numara ve editorial grid
- Mobilde tek kolon, geniş ekranda asimetrik düzen
- Bir sayfada tek h1
- Semantic HTML ve görünür focus state

Sayfanın amacı: [AMAÇ]
Zorunlu bölümler: [BÖLÜMLER]
Birincil CTA: [CTA]
Veri/içerik: [İÇERİK]
Teknik ortam: [FRAMEWORK]

Çıktıyı doğrudan çalışan kod olarak üret. Jenerik placeholder metin kullanma. Mevcut layout ve tasarım tokenlarını yeniden kullan.
```

## 8. Landing page promptu

```text
[ÜRÜN/HİZMET] için editorial ve teknik karakterli responsive bir landing page oluştur.

Hero:
- küçük monospace durum etiketi
- çok büyük, 2–4 satırlık güçlü h1
- başlığın tek kelimesinde #F4511E vurgu
- kısa açıklama
- koyu birincil CTA ve çerçeveli ikincil CTA
- sağ tarafta küçük teknik bilgi/status paneli

Devamında:
- numaralı özellik veya proje kayıtları
- metrikleri tablo/çizgi sistemiyle göster
- müşteri yorumu varsa büyük editorial alıntı olarak göster
- final CTA'yı turuncu geniş yüzey üzerinde kullan

Renkler: #F3F0E8, #171713, #65645D, #CFCBC0, #F4511E.
Kart tabanlı jenerik SaaS düzeni kullanma. Gradient, glassmorphism ve stok görsel kullanma.

Teknik ortam: [FRAMEWORK]
İçerik: [ÜRÜN BİLGİLERİ]
```

## 9. Dashboard / web uygulaması promptu

Bu stil yalnızca portfolyo için değildir. Dashboard'a uyarlarken okunabilirliği koruyun:

```text
[UYGULAMA ADI] için işlevsel bir dashboard oluştur. Burak Can KAHRAMAN editorial field-notes tasarım sistemini veri yoğun bir ürüne uyarla.

Kurallar:
- Ana zemin #F3F0E8, panel yüzeyi #FFFDF7.
- Sidebar veya üst navigasyon #171713 olabilir.
- Kritik durum ve seçili öğe için #F4511E kullan.
- Tablo satırlarını ince #CFCBC0 çizgilerle ayır.
- Büyük radius ve yumuşak gölge kullanma.
- Metriklerde büyük sans-serif sayı + küçük monospace label kullan.
- Grafiklerde ink, muted ve signal dışına çıkma.
- Yoğun bilgiyi boşluk, çizgi ve tipografiyle organize et.
- Durumları yalnızca renkle anlatma; metin veya sembol de ekle.
- Klavye navigasyonu, loading, empty, error ve success durumlarını tasarla.

Ana görev: [KULLANICI GÖREVİ]
Gerekli modüller: [MODÜLLER]
Veri yapısı: [VERİ]
Teknik ortam: [FRAMEWORK]
```

## 10. Blog / dokümantasyon promptu

```text
[BLOG/DOKÜMANTASYON ADI] için yüksek okunabilirlikli editorial bir içerik sistemi oluştur.

Liste görünümü:
- yazıları kart yığını yerine numaralı kayıt satırları olarak göster
- tarih monospace ve uppercase
- başlık ağır sans-serif
- açıklama muted
- etiketler ince border'lı küçük monospace pill/label

Detay görünümü:
- içerik kolonunu yaklaşık 45–48rem ile sınırla
- h1 büyük ancak mobilde taşmayacak şekilde clamp kullan
- h2/h3 için güçlü hiyerarşi kur
- inline code için #DED9CC, code block için #171713 zemin kullan
- blockquote solunda #F4511E çizgi kullan
- linklerde turuncu underline kullan
- geri dönüş bağlantısını küçük monospace göster

SEO başlığı, description, canonical ve sosyal paylaşım metadata'sını unutma.
Her sayfada tek h1 kullan.

İçerik kaynağı: [MARKDOWN / CMS / API]
Teknik ortam: [FRAMEWORK]
```

## 11. Bileşen üretme promptu

```text
[BİLEŞEN ADI] bileşenini mevcut Burak Can KAHRAMAN tasarım sistemine uygun üret.

Tasarım bağlamı:
- Paper #F3F0E8
- Ink #171713
- Muted #65645D
- Line #CFCBC0
- Signal #F4511E
- Soft White #FFFDF7
- sans-serif ana tipografi + monospace metadata
- düz border, minimum radius, sert ve küçük hover hareketi

Bileşenin amacı: [AMAÇ]
Props/veri: [ALANLAR]
Durumlar: default, hover, focus-visible, active, disabled, loading, empty, error.
Responsive davranış: [DAVRANIŞ]

Semantic HTML kullan. Erişilebilir isim, label ve klavye etkileşimi ekle. Var olan tokenları kullan; yeni rastgele renk veya spacing üretme. Ağır bağımlılık ekleme.
```

## 12. AI ile tasarım denetimi promptu

Bir modelden kod yazmasını değil, mevcut arayüzü değerlendirmesini istediğinizde:

```text
Bu arayüzü Burak Can KAHRAMAN editorial field-notes tasarım sistemi açısından denetle.

Kontrol et:
1. Palet dışı veya gereksiz renk var mı?
2. Turuncu vurgu aşırı mı kullanılmış?
3. Jenerik SaaS kartı, gradient, glassmorphism veya fazla radius var mı?
4. Büyük başlık ile monospace metadata karşıtlığı yeterli mi?
5. Bölüm ritmi, boşluk ve çizgi hiyerarşisi tutarlı mı?
6. Mobilde taşma, sıkışma veya okunabilirlik sorunu var mı?
7. Focus-visible, kontrast, label ve semantic HTML doğru mu?
8. Sayfada tek h1 var mı ve başlık sırası mantıklı mı?
9. Gereksiz JS, animasyon veya bağımlılık var mı?
10. Tasarım başka sayfalarda aynı sistem gibi devam ediyor mu?

Önce bulguları önem sırasıyla yaz. Ardından yalnızca gerekli değişiklikleri öner. Mevcut çalışan işlevleri değiştirme. Görsel farklılık uğruna erişilebilirliği düşürme.
```

## 13. İçerik ve mikro metin promptu

```text
[PROJE/ÜRÜN] için Burak Can KAHRAMAN tasarım diline uygun Türkçe arayüz metinleri üret.

Ton:
- kısa, doğrudan, sakin ve teknik
- özgüvenli fakat abartısız
- pazarlama klişelerinden uzak
- “devrim niteliğinde”, “benzersiz çözüm”, “geleceği şekillendir” gibi boş ifadeler kullanma
- aktif fiil ve somut çıktı kullan

Üret:
- 5 hero başlığı
- her başlık için 1 kısa açıklama
- 3 birincil CTA ve 3 ikincil CTA
- 5 monospace mikro etiket/status metni
- empty, loading, success ve error mesajları

Hedef kitle: [KİTLE]
Ürün açıklaması: [AÇIKLAMA]
```

## 14. Görsel / Open Graph promptu

ImageGen, Gemini görsel modeli veya başka bir görsel üretici için:

```text
Use case: social preview / Open Graph card
Aspect ratio: 1200×630 landscape

Create a bold editorial graphic for [BRAND/PROJECT].

Visual direction:
- Swiss editorial design mixed with independent technical field notes
- warm paper background #F3F0E8
- near-black ink #171713
- safety orange #F4511E
- subtle 32px technical grid and tactile printed texture
- oversized geometric sans-serif headline
- compact monospace metadata
- sharp alignment, generous negative space, thumbnail legibility

Exact text:
"[MARKA]"
"[BAŞLIK]"
"[KONUM VEYA METADATA]"

Constraints:
- reproduce all Turkish characters exactly
- exact text only, no invented words
- no people, devices or third-party logos
- no gradients, glossy 3D, stock imagery or blue tech aesthetic
- no mockup frame and no watermark
```

## 15. Hızlı tek-paragraf prompt

Ayrıntılı kitin tamamını yapıştırmak istemediğinizde:

```text
[PROJE] arayüzünü Swiss editorial ve bağımsız geliştirici field-notes estetiğinde tasarla: sıcak kâğıt #F3F0E8, mürekkep #171713, muted #65645D, çizgi #CFCBC0 ve yalnızca vurgu için güvenlik turuncusu #F4511E kullan. Büyük sıkı tracking'li sans-serif başlıkları küçük uppercase monospace metadata ve numaralarla eşleştir. 32px düşük opaklıklı teknik grid, ince border, geniş boşluk, asimetrik kolon ve küçük mekanik hover hareketleri kullan. Yuvarlak SaaS kartı, gradient, glassmorphism, yumuşak gölge, stok görsel ve gereksiz animasyondan kaçın. Responsive, semantic, klavye erişilebilir ve tek h1'li çalışan [FRAMEWORK] kodu üret.
```

## 16. Promptu kullanma yöntemi

En iyi sonuç için modele tek seferde belirsiz şekilde “bu renklerle site yap” demek yerine şu sırayı uygulayın:

1. **Bağlam verin:** Projenin amacı, kullanıcıları ve teknik ortamı.
2. **Ana promptu verin:** Bölüm 5 veya 6.
3. **Gerçek içerik verin:** Sayfalar, metinler, veri alanları.
4. **Sınırları belirtin:** Mevcut işlevler, dokunulmaması gereken dosyalar, bağımlılık kısıtı.
5. **Çalışan ilk sürümü isteyin:** Bütün sistemi tek seferde soyut olarak tartışmak yerine uygulanmış kod alın.
6. **Denetim promptunu çalıştırın:** Bölüm 12 ile sonuçları kontrol ettirin.
7. **Tek hedefli revizyon yapın:** “Sadece mobil header'ı düzelt” gibi sınırlandırın.

### OpenAI / Codex için öneri

```text
Önce repoyu ve mevcut kuralları incele. Dosyaları doğrudan düzenle, mevcut mimariyi koru, build çalıştır ve sonucu dosya bazında özetle.
```

### Gemini için öneri

```text
Eklediğim proje dosyalarını kaynak gerçek olarak kabul et. Önce değiştireceğin dosyaları ve koruyacağın işlevleri listele; ardından eksiksiz dosya içeriklerini veya uygulanabilir patch üret. Açıklama yerine çalışan kodu önceliklendir.
```

### Yalnızca fikir / mockup için

```text
Kod üretme. Önce bilgi mimarisi, görsel hiyerarşi, responsive davranış ve bileşen listesini tanımla. Her tasarım kararını bu paletteki bir token ve işlevsel gerekçeyle ilişkilendir.
```

## 17. Projeye göre özelleştirme alanları

Bu sistemi başka projeye taşırken aşağıdakiler değişebilir:

- Marka adı ve monogram
- Signal rengi — tek vurgu rengi olarak kalmalı
- İçerik genişliği
- Grid ölçüsü
- Sans-serif font ailesi
- Ürün tipine göre yoğunluk

Şunları mümkün olduğunca koruyun:

- sıcak paper / koyu ink karşıtlığı
- monospace metadata dili
- editorial numaralandırma
- çizgi tabanlı bölüm ayrımı
- düşük radius ve kontrollü gölge
- geniş boşluk ve güçlü tipografik hiyerarşi

Bu unsurlar birlikte kullanıldığında tasarım “aynı aileden” görünür; yalnızca hex kodlarını taşımak aynı sonucu vermez.
