# Burak Can KAHRAMAN — Web Çözümleri ve Portfolyo

Astro ve Tailwind CSS ile hazırlanan Türkçe/İngilizce kişisel web sitesi, portfolyo ve Markdown tabanlı blog.

Kurulum, içerik yönetimi, yeni sayfa ekleme, GitHub/Vercel yayını ve güvenlik için:

**[Ayrıntılı Kullanım ve Yayın Kılavuzu](./KULLANIM-VE-YAYIN-KILAVUZU.md)**

Diğer projelerde aynı görsel dili kullanmak için:

**[AI Tasarım Prompt Kiti](./AI-DESIGN-PROMPT-KITI.md)**

## Hızlı Başlangıç

```powershell
npm install
npm run dev
```

Yerel adres: `http://localhost:4321`

## Proje Yapısı

Aşağıdaki klasörler sitenin sayfalarını, içeriğini ve ortak tasarımını içerir:

```text
/
├── public/                 # Görseller ve favicon
├── src/
│   ├── content/blog/       # Markdown blog yazıları
│   ├── layouts/            # Ortak sayfa düzeni
│   ├── pages/              # Sayfalar ve rotalar
│   └── styles/             # Global tasarım kuralları
├── KULLANIM-VE-YAYIN-KILAVUZU.md
└── package.json
```

## Komutlar

Bütün komutları terminalde proje ana dizinindeyken çalıştırın:

| Komut                     | İşlev                                             |
| :------------------------ | :----------------------------------------------- |
| `npm install`             | Bağımlılıkları kurar                              |
| `npm run dev`             | Yerel geliştirme sunucusunu başlatır              |
| `npm run build`           | Üretim sürümünü `dist/` dizinine oluşturur        |
| `npm run preview`         | Oluşturulan üretim sürümünü yerelde gösterir      |
