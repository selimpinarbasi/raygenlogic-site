# raygenlogic-site

Ray Gen Logic'in tanıtım sitesi. Statik HTML/CSS/JS, build adımı yok. GitHub Pages üzerinden `raygenlogic.com`e yayınlanıyor (main'e her push otomatik deploy eder), önünde Cloudflare var.

## Dokunulmaması gereken dosyalar

- `privacy/index.html` — gizlilik politikası, App Store/Play Store formlarına giriyor. İçeriği asla değiştirme, yalnızca tasarım/stil.
- `support/index.html` — destek sayfası, aynı şekilde mağaza formlarında.
- `CNAME` — domain bağlantısı, silinirse `raygenlogic.com` kırılır.

## config/v1.json

Bu dosyayı **Selim GitHub üzerinden doğrudan düzenler** (repo'yu GitHub'da açıp dosyayı edit edip commit etmesi yeterli — kod değişikliği gerekmez). Oyun istemcileri bunu çalışma zamanında okuyup davranış değiştiriyor:

| Alan | Anlamı |
|---|---|
| `version` | Şema sürümü, istemci uyumluluğu için |
| `ads.realUnitsEnabled` | `false` iken test reklam birimleri kullanılır (gerçek AdMob ID'leri yerine) |
| `ads.interstitialEnabled` / `ads.rewardedEnabled` | O reklam türünü tamamen açar/kapatır |
| `offers.starterPackEnabled` / `offers.adFreeOfferEnabled` | İlgili mağaza teklifinin görünürlüğü |
| `minSupportedBuild.android` / `.ios` | Bu build numarasının altındaki istemcilere "güncelle" uyarısı |
| `message` | `null` değilse istemcide gösterilecek genel duyuru metni |

Canlı adres: `https://raygenlogic.com/config/v1.json` — `access-control-allow-origin: *` ile servis ediliyor, istemci doğrudan fetch edebilir.

## app-ads.txt

`https://raygenlogic.com/app-ads.txt` — Google AdMob için IAB app-ads.txt beyanı. Yalnızca reklam sağlayıcı değişirse güncellenir.
