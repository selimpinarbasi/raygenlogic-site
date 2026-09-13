# raygenlogic-site

Ray Gen Logic'in tanıtım sitesi. Statik HTML/CSS/JS, build adımı yok. GitHub Pages üzerinden `raygenlogic.com`e yayınlanıyor (main'e her push otomatik deploy eder), önünde Cloudflare var.

## Dokunulmaması gereken dosyalar

- `privacy/index.html` — gizlilik politikası, App Store/Play Store formlarına giriyor. İçeriği asla değiştirme, yalnızca tasarım/stil.
- `support/index.html` — destek sayfası, aynı şekilde mağaza formlarında.
- `CNAME` — domain bağlantısı, silinirse `raygenlogic.com` kırılır.

## config/v1.json

Bu dosya artık Ed25519 ile **imzalı bir zarf**: `{"payload": {...}, "sig": "..."}`. GitHub'da elle düzenlenmez — imza, payload'ın byte'larına bağlı olduğu için tek bir boşluk/satır sonu farkı bile imzayı geçersiz kılar.

**Güncelleme akışı** (oyun reposunda, `raygenlogic/tools/remote-config-sign.py`):

```
tools/remote-config-sign.py sign --key ~/raygen-config-private.pem --payload payload.json --out v1.json
```

Bu komutun ürettiği `v1.json` çıktısı **olduğu gibi, byte byte** buraya (`config/v1.json`) kopyalanır — JSON'u yeniden biçimlendirme, pretty-print etme, tek bir alanı bile elle değiştirme. Kopyalama sonrası kaynakla hedefin hash'i (`shasum -a 256`) karşılaştırılarak doğrulanmalı.

`payload` alanlarının anlamı:

| Alan | Anlamı |
|---|---|
| `payload.version` | Şema sürümü, istemci uyumluluğu için |
| `payload.ads.realUnitsEnabled` | `false` iken test reklam birimleri kullanılır (gerçek AdMob ID'leri yerine) |
| `payload.ads.interstitialEnabled` / `.rewardedEnabled` | O reklam türünü tamamen açar/kapatır |
| `payload.offers.starterPackEnabled` / `.adFreeOfferEnabled` | İlgili mağaza teklifinin görünürlüğü |
| `payload.minSupportedBuild.android` / `.ios` | Bu build numarasının altındaki istemcilere "güncelle" uyarısı |
| `payload.message` | `null` değilse istemcide gösterilecek genel duyuru metni |
| `sig` | `payload`in Ed25519 imzası (base64) — özel anahtar bu repoda hiç bulunmaz |

Canlı adres: `https://raygenlogic.com/config/v1.json` — `access-control-allow-origin: *` ile servis ediliyor, istemci doğrudan fetch edebilir. Cache `max-age=600` (GitHub Pages varsayılanı, dosya bazında özelleştirilemiyor) — istemci tarafı `?v=<dakika kovası>` ile bayatlığı 1 dakikayla sınırlıyor.

## app-ads.txt

`https://raygenlogic.com/app-ads.txt` — Google AdMob için IAB app-ads.txt beyanı. Yalnızca reklam sağlayıcı değişirse güncellenir.
