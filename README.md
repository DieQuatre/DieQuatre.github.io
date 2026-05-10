📚 Kişisel Ders Notları Web Sitesi

Bu proje, kişisel ders notlarımı (özellikle Makine Öğrenimi ve Diferansiyel Denklemler) barındırmak için oluşturulmuş basit, statik bir web sitesidir.

🚀 Proje Hakkında

Bu site, üniversite ders notlarımı dijital ve düzenli bir ortamda toplamak amacıyla oluşturulmuştur.
Sitenin tasarımında, ADHD (DEHB) dostu bir arayüz hedeflenmiştir. Göz yormayan karanlık mod (dark mode) ve dikkati dağıtmayan odaklı bir yapı (belirgin mor butonlar gibi) kullanılmıştır. Temel amaç, hem benim hem de benzer ihtiyaçları olan diğer öğrencilerin notlara kolayca odaklanabilmesidir.

✨ Temel Özellikler

ADHD Dostu Arayüz: Göz yormayan karanlık mod ve dikkat dağıtmayan minimal tasarım.
Basit Navigasyon: Dersler ve sınav türleri (Vize, Final, Quiz) arasında kolay geçiş.
Tamamen Statik: Sunucu gerektirmeden, GitHub Pages üzerinden ücretsiz yayınlanabilir.
Sıfır Bağımlılık: Sadece HTML ve CSS kullanılmıştır.

💻 Kullanılan Teknolojiler

Proje, gereksiz karmaşıklıktan kaçınarak en temel web teknolojileriyle oluşturulmuştur:
HTML5: Sayfa içeriğini ve yapısını oluşturmak için.
CSS3: Karanlık mod, renkler, butonlar ve tüm stillendirme için (<style> etiketleri içinde).

📁 Dosya Yapısı

Sitenin yapısı, hangi dosyanın ne işe yaradığını gösterecek şekilde basit tutulmuştur:

├── 📄 index.html                (Ana Sayfa - Ders seçimi)
│
├── 📄 diferansiyel-denklemler.html (Diferansiyel D. Menüsü - Vize/Final seçimi)
├── 📄 diferansiyel-vize.html       (Diferansiyel D. Vize notları)
├── 📄 diferansiyel-final.html      (Diferansiyel D. Final notları)
│
├── 📄 machine-learning.html     (Machine Learning Menüsü - Vize/Final seçimi)
├── 📄 machine-learning-vize.html  (ML Vize notları)
├── 📄 machine-learning-final.html (ML Final notları)
│
└── 📄 README.md                 (Bu dosya)

📜 Lisans

Bu proje MIT Lisansı altındadır.

## PDF Dosyası Ekleme ve Sitede Gösterme (GitHub Pages)

PDF dosyalarını proje içinde `assets/pdfs/` klasöründe tutabilirsiniz. Bu repo içinde klasörü hazır tutmak için `.gitkeep` dosyası kullanılır.

### 1) GitHub Web Arayüzü ile PDF ekleme

1. Repoya girin ve `assets/pdfs/` klasörünü açın.
2. **Add file → Upload files** seçin.
3. PDF dosyanızı (ör. `notlar.pdf`) yükleyin.
4. Commit mesajı girip değişikliği kaydedin.

### 2) Git ile PDF ekleme

```bash
git add assets/pdfs/notlar.pdf
git commit -m "PDF notu eklendi"
git push
```

### 3) PDF'e link verme (önerilen)

Herhangi bir HTML sayfasına aşağıdaki bağlantıyı ekleyebilirsiniz:

```html
<a href="/assets/pdfs/notlar.pdf" target="_blank" rel="noopener">
  PDF notunu aç
</a>
```

### 4) PDF'i sayfaya gömme (isteğe bağlı)

```html
<iframe
  src="/assets/pdfs/notlar.pdf"
  width="100%"
  height="700"
  style="border: 1px solid #444;"
>
</iframe>
```

> Not: Bazı mobil tarayıcılarda `iframe` ile PDF görüntüleme sınırlı olabilir. Böyle durumlarda link verme yöntemi daha uyumludur.

✨ Sonsöz

Her zaman yanımda olan Gemini ve KBI'a teşekkür ederim 💕
