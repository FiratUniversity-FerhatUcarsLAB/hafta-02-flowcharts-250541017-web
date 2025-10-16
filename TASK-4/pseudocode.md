BAŞLA

    // 1. GİRİŞ EKRANI
    ÖĞRENCİ_NO, ŞİFRE al
    EĞER (öğrenci_no ve şifre doğru) İSE
        "Giriş başarılı" yaz
    DEĞİLSE
        "Hatalı giriş, tekrar deneyin" yaz
        GİRİŞ ekranına dön
    SON

    // 2. DERS LİSTESİ GÖRÜNTÜLEME
    DERS_LISTESİ göster

    // 3. DERS SEÇİM DÖNGÜSÜ
    DÖNGÜ her DERS için DERS_LISTESİ içinde

        // 3.1 Kontenjan kontrolü
        EĞER (DERS.kontenjan > 0) İSE
            // 3.2 Ön koşul kontrolü
            EĞER (öğrenci tüm ön koşulları sağlamış mı?) İSE
                // 3.3 Zaman çakışması kontrolü
                EĞER (zaman_çakışması yok mu?) İSE
                    // 3.4 Kredi limiti kontrolü
                    EĞER (toplam_kredi + DERS.kredi ≤ 35) İSE
                        // 3.5 Danışman onayı kontrolü
                        EĞER (öğrenci_GPA < 2.5) İSE
                            "Danışman onayı bekleniyor" yaz
                            danışman_onayı ← danışman_onayı_al()
                            EĞER (danışman_onayı = ONAY) İSE
                                DERS kayda ekle
                            DEĞİLSE
                                "Onay reddedildi, ders eklenmedi" yaz
                            SON
                        DEĞİLSE
                            DERS kayda ekle
                        SON
                    DEĞİLSE
                        "Kredi limiti aşılıyor, ders eklenemedi" yaz
                    SON
                DEĞİLSE
                    "Zaman çakışması tespit edildi, ders eklenemedi" yaz
                SON
            DEĞİLSE
                "Ön koşul dersi eksik, ders eklenemedi" yaz
            SON
        DEĞİLSE
            "Kontenjan dolu, ders eklenemedi" yaz
        SON

    DÖNGÜ SONU

    // 4. DERS SİLME (isteğe bağlı)
    EĞER (öğrenci ders silmek istiyor mu?) İSE
        DERS seç ve sil
    SON

    // 5. KAYIT ÖZETİ VE ONAY
    kayıt_özeti_göster()
    EĞER (öğrenci onaylıyor mu?) İSE
        "Kayıt tamamlandı" yaz
    DEĞİLSE
        "Kayıt düzenleniyor" yaz
        Geri dön -> DERS_LISTESİ
    SON

BİTİR
