BAŞLA

// --- 1. Kimlik Doğrulama ---
TEKRAR
    TC_No ← Kullanıcıdan_TC_Gir()
    Eğer Kimlik_Dogrula(TC_No) ise
        Devam Et
    değilse
        Yaz("Geçersiz TC Kimlik Numarası. Tekrar deneyin.")
SONSUZA_KADAR

// --- 2. Poliklinik Seçimi ---
Poliklinikler ← Poliklinik_Listesini_Getir()
Poliklinik_Secimi ← Kullanıcıdan_Poliklinik_Sec(Poliklinikler)

// --- 3. Doktor Listesi Gösterme ve Seçme ---
Doktorlar ← Doktor_Listesi_Getir(Poliklinik_Secimi)
Doktor_Secimi ← Kullanıcıdan_Doktor_Sec(Doktorlar)

// --- 4. Uygun Saatleri Gösterme ve Seçim ---
TEKRAR
    Saatler ← Uygun_Saatleri_Getir(Doktor_Secimi)
    Eğer Saatler boş ise
        Yaz("Bu doktor için uygun saat yok. Lütfen başka bir doktor seçin.")
        Doktor_Secimi ← Kullanıcıdan_Doktor_Sec(Doktorlar)
    değilse
        Saat_Secimi ← Kullanıcıdan_Saat_Sec(Saatler)
        ÇIKIŞ
SONSUZA_KADAR

// --- 5. Randevu Onayı ---
Yaz("Seçiminiz: ", Poliklinik_Secimi, Doktor_Secimi, Saat_Secimi)
Onay ← Kullanıcıdan_Onay_Al()
Eğer Onay = "Evet" ise
    Randevu_Kaydet(TC_No, Doktor_Secimi, Saat_Secimi)
    SMS_Gonder(TC_No, "Randevunuz başarıyla oluşturuldu: " & Saat_Secimi)
    Yaz("Randevunuz oluşturuldu ve SMS gönderildi.")
değilse
    Yaz("Randevu iptal edildi.")

BAŞLA

// --- 1. Kimlik Doğrulama ---
TEKRAR
    TC_No ← Kullanıcıdan_TC_Gir()
    Eğer Kimlik_Dogrula(TC_No) ise
        Devam Et
    değilse
        Yaz("Geçersiz TC Kimlik Numarası. Lütfen tekrar deneyin.")
SONSUZA_KADAR

// --- 2. Tahlil Varlığı Kontrolü ---
Tahliller ← Tahlil_Kaydi_Sorgula(TC_No)

Eğer Tahliller boş ise
    Yaz("Sistemde kayıtlı bir tahlil bulunamadı.")
    GİT → SON
değilse
    Devam Et

// --- 3. Sonuç Hazır mı? Kontrolü ---
Sonuc ← Tahlil_Sonuc_Kontrol(TC_No)

Eğer Sonuc = "Hazır" ise
    Yaz("Tahlil sonuçlarınız hazır.")
    Sonuc_Verisi ← Tahlil_Sonucunu_Getir(TC_No)
    Yaz("Sonuç:", Sonuc_Verisi)

    // --- 4. PDF İndirme Seçeneği ---
    İndirilsinMi ← Kullanıcıdan_Soru("Sonucu PDF olarak indirmek ister misiniz? (E/H)")

    Eğer İndirilsinMi = "E" ise
        PDF_Olustur_ve_Indir(Sonuc_Verisi)
        Yaz("PDF indirildi.")
    değilse
        Yaz("PDF indirme iptal edildi.")

değilse
    Yaz("Tahlil sonucunuz henüz hazır değil. Lütfen daha sonra tekrar deneyiniz.")

// --- 5. Sistem Sonu ---
SON:
Yaz("İyi günler.")
BİTİR


// --- 6. Sistem Sonu ---
Yaz("İyi günler.")
BİTİR
