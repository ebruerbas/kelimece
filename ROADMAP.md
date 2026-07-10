# Kelimece Yol Haritası

Üç değerlendirme oturumunun (geliştirici+kullanıcı, oyun geliştiricisi gözüyle kullanıcılar, Apple inceleyicileri) birleşik sonucu. Temmuz 2026.

Uygulamanın kimliği: oyunlaştırmaya kaçmadan "her gün 10 dakikaya değer" hissettirmek. Rozet/lig/puan yok; Mece var.

## Faz 1 — Güvence (teknik temel)

- [x] JSON yedekle / geri yükle (ilerleme tek kopyada ve kırılgan — en büyük risk)
- [x] PWA: manifest + service worker (telefona kurulum, çevrimdışı, kalıcı depolama)
- [x] Günlük aktivite kaydını başlat (ileriki istatistik ekranının hammaddesi — kayıt birikmeden grafik olmaz)
- [x] Yazma sorularında aksan toleransı (über=uber, café=cafe)

## Faz 2 — His ve kimlik

- [x] Seviyelere tema isimleri (Seviye 4 → "Duygular"; sıfır içerik maliyeti, harita hissi)
- [x] Doğru/yanlış anına mikro animasyon (prefers-reduced-motion uyumlu)
- [x] Seri affı: haftada 1 kaçan gün telafisi (Mece'nin karakteriyle uyumlu)
- [x] Seviye testine Mece eşliği (ilk 2 dakikada karakter sahnede olsun)
- [x] "Biliyorum" düğmesine ilk kullanım açıklaması
- [x] alert() yerine uygulamanın kendi modalı

## Faz 3 — Platform cilası

- [ ] Karanlık tema
- [ ] Dokunma hedefleri 44pt (mini ses düğmeleri, üst haplar)
- [ ] VoiceOver: 🔊 düğmelerine aria-label, ••• için erişilebilir metin
- [ ] Dinamik yazı boyutuna saygı (px → rem geçişi)

## Faz 4 — Derinlik ve içerik

- [x] Mece tüy koleksiyonu (öğrenilen kelime = tüy; kalıcı, yarışsız ilerleme)
- [ ] Örnek cümleleri İngilizce'de tüm seviyelere yay (diğer diller: kalite riski nedeniyle beklet)
- [ ] Dil bitirme finali + yeni dile davet
- [ ] Favori kelimeler
- [ ] İstatistik ekranı (Faz 1'deki kayıt biriktikten sonra)

## Bilinçli olarak yapılmayacaklar

- Hesap sistemi / sunucu (tek dosya + cihazda veri kimliğin parçası; yedekleme bunu telafi eder)
- Rozet, lig, puan, can tarzı oyunlaştırma
- 5 dilde cümle üretimi (gramer doğruluğu tek kişilik projede denetlenemez)
- Ses efektleri (varsayılan kapalı ses kimse tarafından açılmaz — tartışıldı, vazgeçildi)
