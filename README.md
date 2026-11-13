# 🃏 Pis 7'li Online — Kurulum

**Gerçek zamanlı, 2–4 kişilik, üyelik gerektirmeyen iskambil oyunu.**

Tüm oyuncular kendi adını yazar. Bir oyuncu 6 haneli oda kodu oluşturur, arkadaşları aynı koda girer. Oda sahibi oyunu başlatır. Kimse rakiplerinin kartlarını göremez; hamle kuralları Supabase veritabanı tarafından kontrol edilir. WebSocket bildirimleri kullanılır; bağlantı sorunlarında 3,5 saniyede bir yenileme yedeği vardır.

## Kurulum (3 adım)

**1. Supabase'i hazırla**

- [supabase.com](https://supabase.com/) üzerinden bir proje oluştur (istersen mevcut projen de olur; burada yalnızca `p7_*` adlı nesneler oluşturulur).
- **SQL Editor** → **New query** → paket içindeki `supabase.sql` dosyasının tüm içeriğini yapıştır → **Run**.
- **Project Settings → API / Connect** ekranından **Project URL** ve **publishable** (`sb_publishable_...`) veya eski **anon** anahtarını al.

**2. config.js dosyasını düzenle**

```js
window.P7_CONFIG = {
  SUPABASE_URL: 'https://GERCEK-PROJE-REF.supabase.co',
  SUPABASE_KEY: 'sb_publishable_GERCEK_ANAHTAR'
};
```

**Önemli:** `service_role` / `secret` anahtarlarını asla HTML/JS içinde yayınlama. Publishable/anon anahtarı tarayıcıda kullanılmak üzere tasarlanmıştır.

**3. GitHub Pages'e yayınla**

- GitHub'da yeni depo aç, örneğin `pis-7li-online`.
- `index.html`, `styles.css`, `app.js`, `config.js`, `manifest.webmanifest`, `assets/` klasörünü **deponun ana dizinine** yükle.
- `supabase.sql` ve `README.md` dosyalarını da depoya koyabilirsin (gizli anahtar içermezler).
- GitHub deposunda **Settings → Pages → Deploy from a branch → main → /(root) → Save**.
- GitHub Pages adresini arkadaşlarına gönder. Bir telefon **Oda Oluştur**, diğer telefon **Odaya Katıl** yaparak denesin.

Siteyi ayarlar olmadan açarsan **Örnek oyun masasını gör** düğmesiyle tasarımı inceleyebilirsin; bu yalnızca görsel önizlemedir, çevrimiçi oyun değildir.

## Oyun kuralları

| Kart | Etki |
|---|---|
| 7 | Sonraki oyuncuya +3 kart; başka bir 7 ile ceza +6, +9... birikir. |
| J (Vale) | Atan oyuncu sinek, karo, kupa veya maça seçer. |
| A (As) | Aynı oyuncu aynı türden kart veya başka As atarak devam eder. Uygun kart yoksa 1 kart çeker. |
| 10 | Oyunun yönünü tersine çevirir. |

Normal turda aynı tür veya aynı değer atılır, Vale herhangi bir kartın üzerine atılabilir. Oynanabilir kart yoksa 1 çekilir; gelen kart uygunsa atılabilir, değilse otomatik sıra geçer. Oyuncu çektiği kart uygunsa **Pas Geç** seçeneğine de basabilir. **As, tek başına son kart olarak eli bitirmez**; takip kartı kuralı devam eder. Diğer kartlarla ilk elini boşaltan kazanır. 52 kartlık tek deste kullanılır.

> Henüz konuşulmamış kurallara ilk sürüm için şu varsayılanları koyduk: 1 desteyle oynama, ilk bitirenin kazanması, son kart uyarısının otomatik olması ve As'tan sonra tek normal kart oynama. İstenirse değiştirilebilir.

## Sorun giderme

- **"Kurulum gerekli"** görünürse `config.js` URL / anahtar değerlerini kontrol et.
- **"Could not find function" / "function not found"**: SQL Editor'de tüm `supabase.sql` dosyasının başarıyla çalıştığına bak; Supabase API şema önbelleğinin yenilenmesini kısa süre sonra tekrar dene.
- **"Failed to fetch"**: URL'nin doğru olduğunu, projenin aktif olduğunu ve cihazda internet bulunduğunu kontrol et.
- **Oyun güncellenmiyor:** WebSocket kesilirse 3,5 saniyelik otomatik yenileme devreye girer; ayrıca sayfayı yenileyebilirsin. Aynı tarayıcı odasına geri bağlanır.
- **GitHub 404:** `index.html` dosyası ana dizinde olmalı ve Pages doğru `main` / `root` üzerinde olmalı.

## Güvenlik ve kapsam

- `p7_rooms` tablosunda RLS etkindir ve tarayıcı rollerine doğrudan tablo okuma/yazma izni verilmez. Oyun sunucu tarafındaki izin kontrollü RPC işlemleriyle çalışır.
- Oda bağlantısına sahip herkes, oda başlamadan önce isim yazarak katılabilir. Bu bir **arkadaş grubu** oyunudur; kimlik doğrulamalı/ödüllü/halka açık turnuvalar için daha sıkı yetkilendirme ve istek hızı sınırlaması gerekir.
- Oda açıkken tarayıcı kapatılırsa kullanıcı yeniden bağlanabilir. **Çıkış** düğmesiyle ayrılmak koltuğu boşaltır. Oyun süresince tarayıcı verilerini silmek yeniden bağlanma anahtarını kaybettirir.
- Bu uygulama mevcut AKTİVA veya Mahalle Platformu dosyalarını ve tablolarını değiştirmez.

## Dosyalar

- `index.html` — uygulama giriş sayfası
- `styles.css` — masaüstü/mobil tasarım
- `app.js` — kullanıcı arayüzü, kart seçimi, WebSocket ve oda akışı
- `config.js` — kullanıcı tarafından girilecek Supabase Project URL ve publishable/anon key
- `supabase.sql` — oda, gizli eller ve server-side oyun kuralları
- `manifest.webmanifest`, `assets/` — telefonda ana ekrana ekleme ve simgeler
