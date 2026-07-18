# İçerik Denetim Görevi — Kelimece

> Bu dosya Claude Code'a verilmek üzere hazırlandı. Kullanım: proje klasöründe
> `claude` başlat, "DENETIM-GOREVI.md dosyasını oku ve uygula" de.

## Bağlam
Kelimece, tek dosyalık (index.html) bir kelime öğrenme PWA'sı. Veriler `index.html` içinde:
- `WORDS`: 700 satır, her satır `[tr, en, de, es, fr, it]`. 20 kelime = 1 seviye, 35 seviye,
  zorluk artarak gider. `LEVEL_NAMES` her seviyenin temasını verir.
- `SENTENCES`: Türkçe anahtar → `{ en: ["___ içeren İngilizce cümle", "Türkçe çevirisi"] }`.
  Yaklaşık 1000 kayıt (kelimeler + phrasal verb'ler). Şimdilik yalnız İngilizce cümle var.
- `PHRASALS`: `[tr, en, örnek cümle (___'li), TR çevirisi]` — 120 phrasal verb.

Kullanıcı şikayeti: "cümle yapıları ve bazı yerler çok saçma" — amaç, tüm içeriği taze gözle
okuyup doğallık/doğruluk sorunlarını bulmak ve DÜZELTMEK.

## Denetim kriterleri
Her SENTENCES kaydı için:
1. **Doğallık:** İngilizce cümle ana dili İngilizce birinin kuracağı bir cümle mi?
   Yapay/ders kitabı kokan, garip kolokasyonlu cümleleri yeniden yaz.
2. **Dil bilgisi:** `___` yerine hedef kelime konunca cümle dilbilgisel olarak kusursuz mu?
   (Tekil/çoğul, fiil çekimi, artikel uyumu — kelime WORDS'teki YALIN haliyle konur, cümle
   buna göre kurulmuş olmalı. Çekim gerektiren cümleyse cümleyi değiştir, kelimeyi değil.)
3. **Çeviri sadakati:** TR cümle, EN cümlenin doğal Türkçesi mi? Birebir kelime çevirisi değil,
   doğal Türkçe olmalı; ama anlam kaymamalı.
4. **Anlam netliği:** Cümle, hedef kelimenin EN YAYGIN anlamını mı örnekliyor? (ör. "dil"
   için tongue/language karışıklığı — WORDS'teki karşılıkla tutarlı olmalı.)
5. **Uzunluk:** Seviye tespitindeki "cümle dizme" sorusu cümleleri kelimelere bölüyor;
   3-9 kelimelik cümleler ideal. 10+ kelimelik cümleleri mümkünse kısalt (zorunlu değil,
   uzun kalan cümle otomatik olarak o formatta kullanılmıyor).

Her WORDS satırı için (6 dil):
6. **Çeviri doğruluğu:** de/es/fr/it karşılıkları doğru ve en yaygın karşılık mı?
   Yanlış/nadir karşılıkları düzelt. Almanca isimlerde baş harf büyük olmalı.
7. **Seviye uygunluğu:** Kelime bariz yanlış seviyedeyse (ör. Seviye 2'de akademik kelime)
   NOT AL ama taşıma yapma (seviye taşımak ilerleme verilerini bozar) — rapora yaz.
8. **TR çakışması:** Aynı Türkçe karşılık birden fazla kelimede varsa not al (quiz eş anlam
   filtresi bunlara dayanıyor; norm() küçük harfe çevirir, birebir eşleşme sayılır).

## Çalışma yöntemi
- Seviye seviye ilerle (35 seviye × 20 kelime). Her seviyeyi bitirince düzeltmeleri uygula.
- Düzeltmeleri doğrudan `index.html` içinde yap. Veri formatını (dizi yapısı, anahtarlar)
  DEĞİŞTİRME — sadece içerik metinlerini düzelt.
- SENTENCES anahtarı Türkçe karşılıktır; WORDS'te TR karşılığı değiştirirsen SENTENCES
  anahtarını da senkron güncelle (yoksa `sentenceFor` cümleyi bulamaz!).
- Her 5 seviyede bir commit at. Commit mesajı Türkçe, ne düzeltildiğini özetle.

## Test (her commit öncesi zorunlu)
```bash
cd /tmp && npm i jsdom --no-audit --no-fund 2>/dev/null
node - <<'EOF'
const { JSDOM } = require('jsdom');
const fs = require('fs');
const html = fs.readFileSync(process.env.HOME+'/Desktop/kelimece/kelimece/index.html','utf8');
const dom = new JSDOM(html,{runScripts:'dangerously',url:'https://x.test/',
  beforeParse(w){ w.matchMedia=()=>({matches:false,addEventListener(){},addListener(){}}); w.scrollTo=()=>{}; }});
const w = dom.window;
let fail = 0;
const WORDS = w.eval('WORDS'), SENTENCES = w.eval('SENTENCES'), LEVEL_COUNT = w.eval('LEVEL_COUNT');
// 1. yapı: her satır 6 eleman, boş yok
WORDS.forEach((r,i)=>{ if(r.length!==6||r.some(x=>!x||!x.trim())){ console.log('BOZUK SATIR',i,r); fail++; } });
// 2. kelime sayısı 20'nin katı
if(WORDS.length % 20 !== 0){ console.log('KELİME SAYISI 20 KATI DEĞİL:', WORDS.length); fail++; }
// 3. her kelimenin EN cümlesi var ve ___ içeriyor
WORDS.forEach(r=>{ const s=SENTENCES[r[0]];
  if(!s||!s.en){ console.log('CÜMLE YOK:',r[0]); fail++; }
  else if(!s.en[0].includes('___')){ console.log('___ YOK:',r[0]); fail++; }
  else if(!s.en[1]||!s.en[1].trim()){ console.log('TR CÜMLE YOK:',r[0]); fail++; }
});
// 4. uygulama açılıyor, quiz başlıyor
w.document.getElementById('inp-name').value='T'; w.obPick('en');
w.document.getElementById('btn-start-test').click(); w.saPick('some');
if(!w.document.getElementById('scr-test').classList.contains('active')){ console.log('QUIZ AÇILMADI'); fail++; }
console.log(fail ? 'SONUÇ: '+fail+' HATA' : 'SONUÇ: TEMİZ');
process.exit(fail?1:0);
EOF
```

## Çıktı
Denetim bitince repo köküne `DENETIM-RAPORU.md` yaz:
- Kaç cümle yeniden yazıldı (seviye bazında kısa döküm)
- Kaç çeviri düzeltildi (dil bazında)
- Taşınması önerilen kelimeler (madde 7) ve TR çakışmaları (madde 8) listesi
- Karar gerektiren belirsiz durumlar

## Yapma
- Veri şemasını, fonksiyonları, UI'ı değiştirme — bu görev yalnız içerik metinleri.
- Kelime ekleme/çıkarma yapma (700 sayısı ve seviye yapısı sabit).
- `git push` deneme (kullanıcı kendisi push'luyor).
