# King & Rıfkı Skor Tablosu — Yayınlama Rehberi

Paket adı: `com.batuhansenol.kingpuan` (yayınladıktan sonra değişmez)

## 1. GitHub'a yükle
1. github.com'da **public** bir repo aç: `king-puan` (README ekleme).
2. Zip'i aç, klasörde cmd:
```
cd king-puan
git init
git add .
git commit -m "ilk surum"
git branch -M main
git remote add origin https://github.com/KULLANICI_ADIN/king-puan.git
git push -u origin main
```

## 2. Gizlilik politikası
1. `docs/index.html` içindeki `ILETISIM_EPOSTA` yazısını kendi e-postanla değiştir, push et.
2. Repo > Settings > Pages > Branch: `main`, klasör `/docs` > Save.
3. Adres: `https://KULLANICI_ADIN.github.io/king-puan/` → iki mağazada da bu linki kullan.

## 3. Codemagic
1. codemagic.io'ya GitHub ile gir, **Add application** > `king-puan` reposunu seç > "codemagic.yaml" seç.
2. **Android anahtarı:** Team settings > Code signing identities > Android keystores > **Generate keystore**. Referans adı: `king_keystore`. Oluşan keystore'u indirip sakla — kaybolursa güncelleme çıkamazsın.
3. **Apple anahtarı:** App Store Connect > Users and Access > Integrations > App Store Connect API > **+** (rol: App Manager). `.p8` dosyasını indir, Issuer ID ve Key ID'yi not al.
   Codemagic > Team settings > Integrations > Developer Portal > Connect > adı `king_asc_key` yap, bilgileri gir.

## 4. Google Play
1. Codemagic'te **Android (Google Play)** workflow'unu başlat > bitince `.aab` dosyasını indir.
2. Play Console > Uygulama oluştur (ad: King & Rıfkı Skor Tablosu, Uygulama, Ücretsiz).
3. Doldur: Mağaza girişi (`store/magaza-metinleri.md`, `store/screens-android`, `store/play-icon-512.png`, `store/play-feature-graphic.png`), Gizlilik politikası, Veri güvenliği (veri toplanmıyor), İçerik derecelendirmesi, Hedef kitle (18 altı seçme → daha az soru), Reklam: yok.
4. Test > **Kapalı test** > yeni sürüm > `.aab` yükle > en az **12 test kullanıcısı** ekle (Gmail adresleri) > yayınla.
5. Test kullanıcıları 14 gün boyunca uygulamayı yüklü tutsun. Sonra Panel'den **Üretime erişim başvurusu** yap.

## 5. App Store
1. developer.apple.com > Certificates, IDs & Profiles > Identifiers > **+** > App IDs > Bundle ID: `com.batuhansenol.kingpuan`.
2. App Store Connect > Uygulamalar > **+ Yeni Uygulama** > iOS, ad: King & Rıfkı Skor Tablosu, dil: Türkçe, bu Bundle ID, SKU: `kingpuan`.
3. Codemagic'te **iOS (App Store)** workflow'unu başlat → build otomatik TestFlight'a gider (işlenmesi 10-30 dk).
4. App Store Connect'te doldur: ekran görüntüleri (`store/screens-ios`, 6.9" bölümüne), açıklama, anahtar kelimeler, destek URL'si (gizlilik linki olabilir), App Privacy: **Data Not Collected**, yaş derecelendirme, inceleme notu (`store/magaza-metinleri.md`).
5. Build'i seç > **İncelemeye gönder**.

## Güncelleme çıkarmak
`www/index.html`'i değiştir > `capacitor.config.json` ile aynı klasörde `npx cap sync` > push > Codemagic'te iki workflow'u tekrar çalıştır. `versionName` / `MARKETING_VERSION` (1.0 → 1.1) değiştirmeyi unutma; build numarası otomatik artar.
