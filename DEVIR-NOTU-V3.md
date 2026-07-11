# Kelimece — Proje Devir Notu v3

Yeni sohbete başlarken: bu dosyayı ekle (veya repo klasörünü bağla, dosya repoda) ve şunu yaz:
**"Kelimece projesine devam edelim, devir notu v3 repoda."**
Claude'a repo klasörünü bağlamayı unutma: `Desktop/kelimece/kelimece`.

## Proje nedir?

Tek dosyalık HTML kelime öğrenme uygulaması (framework yok, saf HTML+CSS+JS). Ana dil Türkçe; hedef diller: EN, DE, ES, FR, IT. Veri `localStorage` ("kelimece_v4"). Yayın: GitHub (https://github.com/ebruerbas/kelimece) + GitHub Pages (ebruerbas.github.io/kelimece). **Vercel YOK** (v2 notundaki bilgi eskiydi). Push'u kullanıcı yapar; Claude commit'e kadar getirir.

Kimlik: oyunlaştırmaya kaçmadan "her gün 10 dakikaya değer" hissettirmek. Hedef kitle: sıfırdan öğrenenler değil, **geliştirmek isteyenler**. Rozet/lig/puan yok; Mece var.

## Maskot: MECE 🧡

Turuncu kuş (gövde #f4a950, koyu #d9722a, gaga #e8622c; tüyler pembe #ff8fa3, sarı #ffd166, teal #5ec9c0). Adı "keli-MECE"nin içinden. Kişilik: sıcak destekçi + muzip.

- Üst menü logosu: rozet yüz SVG (`.brand-bird`), marka "keli<span>mece</span>" — "mece" gaga turuncusu (`--mece`)
- Ana sayfa: kitap üstünde detaylı illüstrasyon (`.hero-illo`) + konuşma balonu (`meceLine()`: kutlama > özlem > seri > günün lafı, 7 laf havuzu)
- Mimikler: `birdFace(mood, size)` — happy/cheer/oops/party; quiz geri bildiriminde, sonuç ekranında, placement testinde
- Tüy koleksiyonu: her 20 öğrenilen kelime (PV dahil) kuyruğa tüy ekler, maks 12, yelpaze açılır (`renderMeceTail`, `featherCount`); kuşa dokununca durum modalı
- Favicon (SVG data-URI) + apple-touch-icon.png + icon-192/512.png (indigo zeminli yüz)

## Bu oturumda yapılanların tamamı

**Öğrenme sistemi:** Akıllı tekrar (`p.tough`: yanlış +1 ağırlık maks 5, doğru -1, sıfırda düşer; Tekrar Quizi zorları önceler, ana sayfa kartı sayıyı gösterir). Örnek cümleler İngilizce'de 320/320 tam (SENTENCES; fill sorusu her 4 sorudan biri). Seviye detayında öğrenilmemiş kelimelerin anlamı gizli (•••). Seviye tema isimleri (`LEVEL_NAMES` 16 ad: İlk Kelimeler → Usta İşi).

**Phrasal Verbs modülü (yeni katman):** İngilizce'ye özel, Seviye 8 kilidi (`PV_UNLOCK_LEVEL`). 120 fiil × 6 blok (`PHRASALS`: [tr, en, cümle, cümleTR]; `PV_NAMES`). **Offset mimarisi:** indeksler `PV_BASE`(100000)+sıra; `wTr/wFor/levelOf/isDone/pickDistractors/sentenceFor` offset tanır → quiz motoru, akıllı tekrar, favoriler, tüyler PV'yi ayrımsız taşır. İlerleme `p.pv.learned`'da ayrı. Ekranlar: scr-pv (6 karo) → scr-leveldetail paylaşımlı (`pvMode` bayrağı, `ldBack()`). KURAL: cümlelerde boşluk daima yalın hâle denk gelir (___s/___ed/___ing yasak — çok kelimeli fiilde bozuk cevap üretir).

**Mece kişilik paketi:** balon, mimikler, kutlamalar, seri affı (haftada 1, tek günlük boşluk; `p.lastAmnesty`), placement testine Mece eşliği, "Biliyorum" ilk kullanım açıklaması (`S.knowInfoSeen`), dil bitirme finali (`p.finaleShown`, 320/320'de bir kez).

**Güvence:** JSON yedekle/geri yükle (ana sayfa altı düğmeler), PWA (manifest.json + sw.js — **index network-first** ki güncellemeler önbelleğe takılmasın; statikler cache-first), günlük aktivite kaydı (`p.log[gün]={q,ok,l}` + `logDay()`), aksan toleransı zaten `norm()`'daydı.

**Platform:** Karanlık tema (sistem + manuel anahtar: 🌓Sistem/☀️Açık/🌙Koyu döngüsü, `kelimece_theme` ayrı localStorage anahtarı, `data-theme` attribute + `:root:not([data-theme="light"])` deseni), 44pt dokunma hedefleri, VoiceOver aria-label'ları, tüm font-size'lar rem. Favoriler (`p.favs`, ★ sekmesi + satır yıldızları). İstatistik ekranı (scr-stats: gün/başarı/7 gün grafiği/en uzun seri `p.bestStreak`/tüy).

**Düzeltilen buglar:** dokunmatikte yapışan hover (tüm :hover'lar `@media (hover:hover)` içinde), karanlık modda input yazısı siyah (color: var(--text) + color-scheme), theme-color meta çift (light/dark media).

## Teknik notlar

- Test yöntemi: jsdom (`npm install jsdom` /tmp'de) + `w.eval()` (top-level `let` window'a bağlanmaz, eval şart). Her özellik uçtan uca test edildi.
- Profil alan ekleme güvenli (toughOf/pvOf/favsOf/logDay deseni: yoksa oluştur) — storage v4 korunuyor. v5 SADECE WORDS sırası değişirse gerekir.
- `norm()` baştaki "to " atar → fill/typed'da "go" = "to go", "give up" öbekleri de kabul.
- grep ile test çıktısı filtrelerken " at " kalıbına dikkat (cümle içindekileri de siler).

## Bilinçli yapılmayacaklar (değişmedi)

Hesap/sunucu yok; rozet/lig/puan yok; 5 dilde cümle üretimi yok (kalite denetlenemez — DE/ES/FR/IT cümleleri bilinçli beklemede); ses efekti yok.

## Sıradaki adaylar (karar verilmedi)

- Collocations modülü (make/do/take kalıpları) — PV tutarsa ikinci parkur
- Falou araştırmasından: telaffuz alıştırması (SpeechRecognition) — beklenti düşük tutulursa; Falou'nun 1 numaralı şikâyeti bu teknolojinin güvenilmezliği
- Kelime arama; "bugün bitti" ekranından seviye quizine yönlendirme
- ROADMAP.md repoda — tüm fazlar işaretli durumda
