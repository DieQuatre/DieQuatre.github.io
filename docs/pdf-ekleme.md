# PDF Ekleme Rehberi (Sohbet + Repo/Site Alternatifi)

Bu sayfa, PDF dosyanı:
1. Bu sohbet ortamına nasıl yükleyeceğini,
2. Yükleme butonu yoksa hangi alternatifleri kullanacağını,
3. PDF’yi repo/site içine nasıl ekleyeceğini
adım adım açıklar.

## 1) Bu sohbete PDF yükleme

1. Mesaj yazma kutusunun yanındaki **ataç / Upload / + / Dosya** ikonunu bul.
2. İkona tıkla ve PDF dosyanı seç.
3. Yükleme bitince mesajı gönder.

> Not: Buton adı platforma göre değişebilir (Upload, Attach, Dosya Ekle vb.).

## 2) Upload butonu görünmüyorsa (Troubleshooting)

Eğer arayüzde dosya yükleme butonu yoksa:

1. Sayfayı yenile ve tekrar kontrol et.
2. Farklı bir tarayıcı veya gizli sekme dene.
3. Kurumsal/ağ engeli ihtimaline karşı farklı ağdan dene.
4. Hâlâ yoksa aşağıdaki alternatiflerden birini kullan:
   - **Drive/OneDrive linki paylaşma:** PDF’yi yükle, paylaşım linki al, buraya yapıştır.
   - **GitHub’a yükleyip link paylaşma:** Repo içinde dosyayı yükle, dosya linkini paylaş.

## 3) GitHub repo’ya PDF yükleme (alternatif yol)

1. Repo ana sayfasına git.
2. **Add file → Upload files** seç.
3. PDF dosyanı bırak/seç.
4. Commit mesajı gir (örn. `docs: add pdf file`).
5. **Commit changes** ile kaydet.

## 4) PDF’yi siteye ekleme (GitHub Pages)

PDF’yi kullanıcıların açabilmesi için en basit yöntem: bir link vermek.

1. PDF’yi repo içine uygun bir klasöre koy (örn. `docs/` veya `assets/pdf/`).
2. İlgili HTML sayfasına link ekle:

```html
<a href="docs/ornek-dosya.pdf" target="_blank" rel="noopener noreferrer">
  PDF'i Aç
</a>
```

İstersen gömme (embed) de yapabilirsin:

```html
<iframe src="docs/ornek-dosya.pdf" width="100%" height="700"></iframe>
```

> Not: Bazı tarayıcılarda embed davranışı değişebilir; link yöntemi daha güvenlidir.
