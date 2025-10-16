BAŞLA SISTEM

    // --- KULLANICI GİRİŞİ ---
    BAŞLA KULLANICI_GİRİŞİ
        deneme_sayısı ← 0
        
        DÖNGÜ deneme_sayısı < 3 İSE
            KULLANICIDAN eposta VE şifre AL
            EĞER (eposta VE şifre DOĞRU) İSE
                GİRİŞ_BAŞARILI ← DOĞRU
                ÇIKIŞ DÖNGÜDEN
            DEĞİLSE
                deneme_sayısı ← deneme_sayısı + 1
                EKRANA "Hatalı giriş! Tekrar deneyin." YAZ
            BİTİR EĞER
        BİTİR DÖNGÜ

        EĞER GİRİŞ_BAŞARILI DEĞİLSE
            EKRANA "3 deneme hakkınız doldu. Hesabınız kilitlendi." YAZ
            PROGRAMI SONLANDIR
        BİTİR EĞER
    BİTİR KULLANICI_GİRİŞİ


    // --- ÜRÜN SEÇİMİ VE SEPET YÖNETİMİ ---
    BAŞLA SEPET_ISLEMLERI
        SEPET ← BOŞ

        DÖNGÜ KULLANICI ÜRÜN EKLEMEK İSTİYORSA
            KULLANICIDAN ürün_kodu VE adet AL
            STOK_MIKTARI ← STOK_SORGULA(ürün_kodu)

            EĞER STOK_MIKTARI >= adet İSE
                ÜRÜNÜ_SEPETE_EKLE(ürün_kodu, adet)
                EKRANA "Ürün sepete eklendi." YAZ
            DEĞİLSE
                EKRANA "Yetersiz stok!" YAZ
            BİTİR EĞER

            KULLANICIYA "Başka ürün eklemek istiyor musunuz?" SOR
        BİTİR DÖNGÜ
    BİTİR SEPET_ISLEMLERI


    // --- İNDİRİM KODU KONTROLÜ ---
    BAŞLA INDIRIM_KODU
        KULLANICIYA "İndirim kodunuz var mı?" SOR
        EĞER EVET İSE
            KULLANICIDAN indirim_kodu AL
            GEÇERLİ ← KOD_KONTROL(indirim_kodu)
            EĞER GEÇERLİ İSE
                INDIRIM_ORANI ← KODDAN_ORAN_AL(indirim_kodu)
                SEPET_TOPLAMI ← SEPET_TOPLAMI * (1 - INDIRIM_ORANI)
                EKRANA "İndirim uygulandı." YAZ
            DEĞİLSE
                EKRANA "Kod geçersiz veya süresi dolmuş!" YAZ
            BİTİR EĞER
        BİTİR EĞER
    BİTİR INDIRIM_KODU


    // --- KARGO HESAPLAMA ---
    BAŞLA KARGO_HESAPLAMA
        KULLANICIDAN TESLİMAT_ADRESİ AL
        KARGO_TUTARI ← HESAPLA_KARGO(SEPET_AGIRLIGI, TESLİMAT_ADRESİ)

        EĞER SEPET_TOPLAMI >= 1000 TL İSE
            KARGO_TUTARI ← 0
            EKRANA "Kargo ücretsiz!" YAZ
        DEĞİLSE
            EKRANA "Kargo ücreti: " + KARGO_TUTARI YAZ
        BİTİR EĞER
    BİTİR KARGO_HESAPLAMA


    // --- ÖDEME AŞAMASI ---
    BAŞLA ODEME
        TOPLAM_TUTAR ← SEPET_TOPLAMI + KARGO_TUTARI

        KULLANICIYA "Ödeme yöntemini seçin: 1-Kredi Kartı, 2-Kapıda Ödeme" SOR
        EĞER SEÇİM = 1 İSE
            KULLANICIDAN kart_bilgileri AL
            BANKA_ONAY ← KART_ONAYLA(kart_bilgileri, TOPLAM_TUTAR)

            EĞER BANKA_ONAY = DOĞRU İSE
                EKRANA "Ödeme başarılı." YAZ
                SİPARİŞ_KAYDET()
            DEĞİLSE
                EKRANA "Ödeme başarısız! Lütfen tekrar deneyin." YAZ
            BİTİR EĞER

        DEĞİLSE EĞER SEÇİM = 2 İSE
            EKRANA "Kapıda ödeme seçildi. Siparişiniz hazırlanıyor." YAZ
            SİPARİŞ_KAYDET()
        BİTİR EĞER
    BİTİR ODEME


    // --- SİPARİŞ ONAYI VE TAKİP ---
    BAŞLA SIPARIS_ONAY
        SIPARIS_NO ← OLUŞTUR_SIPARIS_NUMARASI()
        EKRANA "Siparişiniz oluşturuldu. Numara: " + SIPARIS_NO YAZ
        EPOSTA_GONDER(kullanici, SIPARIS_NO)
    BİTİR SIPARIS_ONAY

BİTİR SISTEM
