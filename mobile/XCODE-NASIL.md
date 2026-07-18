# Kelimece'yi iPhone'da deneme (Xcode)

## Gerekenler
- Mac'te Xcode kurulu (App Store'dan, ücretsiz)
- Apple ID (ücretsiz hesap yeterli, geliştirici üyeliği şart değil)
- iPhone + kablo

## Adımlar
1. Xcode'u aç → "Open a project or file" → `Desktop/kelimece/kelimece/mobile/ios/App/App.xcodeproj` seç.
2. Soldaki gezginde en üstteki mavi **App** simgesine tıkla → ortada **Signing & Capabilities** sekmesi →
   **Team** açılırından Apple ID'ni seç ("Add an Account" ile giriş yapabilirsin).
   Bundle Identifier çakışma hatası verirse sonuna adını ekle: `com.ebru.kelimece1` gibi.
3. iPhone'u kabloyla bağla; telefonda "Bu bilgisayara güven" de.
   Telefonda geliştirici modu istenirse: Ayarlar → Gizlilik ve Güvenlik → Geliştirici Modu → Aç (telefon yeniden başlar).
4. Xcode'un üst ortasındaki aygıt seçiciden kendi iPhone'unu seç → ▶ (Run) düğmesine bas.
5. İlk açılışta telefon "güvenilmeyen geliştirici" derse: Ayarlar → Genel → VPN ve Aygıt Yönetimi → Apple ID'ne güven.

## Güncelleme akışı
Uygulamanın kendisi `index.html`'de yaşıyor. Ben her değişiklikten sonra `mobile/www` +
`mobile/ios/.../public` klasörlerini eşitleyip commit'liyorum — sen sadece Xcode'da tekrar ▶'a
basarsın. (Elle eşitlemek istersen: `cd mobile && npx cap sync ios`)

## Not
- Ücretsiz Apple ID ile kurulan uygulama 7 gün sonra açılmaz olur; Xcode'dan tekrar yüklemen yeterli.
- App Store'a koymak istersek yıllık $99 geliştirici üyeliği gerekir — o aşamada konuşuruz.
