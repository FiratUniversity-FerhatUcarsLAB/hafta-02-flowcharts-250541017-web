ALGORİTMA ATM_ParaCekme

BAŞLA

    bakiye ← 5000
    günlük_limit ← 2000
    deneme_sayısı ← 0
    max_deneme ← 3
    doğru_PIN ← 1234

    // --- PIN DOĞRULAMA ---
    TEKRAR ET
        EKRANA "Lütfen PIN giriniz:" YAZ
        GİRİŞ PIN

        EĞER PIN = doğru_PIN İSE
            EKRANA "PIN doğru, hoş geldiniz!" YAZ
            ÇIKIŞ YAP (PIN döngüsünden)
        DEĞİLSE
            deneme_sayısı ← deneme_sayısı + 1
            EKRANA "Hatalı PIN! Kalan deneme: " + (max_deneme - deneme_sayısı) YAZ
        BİTİŞEĞER
    DENEME SÜRESİ dolana kadar (deneme_sayısı < max_deneme)

    EĞER deneme_sayısı = max_deneme İSE
        EKRANA "Kart bloke edildi. Lütfen bankanızla iletişime geçiniz." YAZ
        BİTİR
    BİTİŞEĞER


    // --- PARA ÇEKME İŞLEMİ ---
    DEVAM ← "E"

    TEKRAR ET
        EKRANA "Çekmek istediğiniz tutarı giriniz:" YAZ
        GİRİŞ tutar

        // 1. Tutar kontrolü
        EĞER tutar % 20 ≠ 0 İSE
            EKRANA "Tutar 20 TL’nin katı olmalıdır." YAZ
            DEVAM ET (başa dön)
        BİTİŞEĞER

        // 2. Günlük limit kontrolü
        EĞER tutar > günlük_limit İSE
            EKRANA "Günlük limit aşıldı! Limit: " + günlük_limit YAZ
            DEVAM ET
        BİTİŞEĞER

        // 3. Bakiye kontrolü
        EĞER tutar > bakiye İSE
            EKRANA "Yetersiz bakiye! Mevcut bakiye: " + bakiye YAZ
            DEVAM ET
        BİTİŞEĞER

        // Tüm kontroller başarılıysa işlem yap
        bakiye ← bakiye - tutar
        günlük_limit ← günlük_limit - tutar
        EKRANA tutar + " TL verildi." YAZ
        EKRANA "Kalan bakiye: " + bakiye YAZ

        // Başka işlem isteği
        EKRANA "Başka bir işlem yapmak ister misiniz? (E/H)" YAZ
        GİRİŞ DEVAM
    DENEME SÜRESİ dolana kadar (DEVAM = "E" veya DEVAM = "e")

    EKRANA "Kartınızı almayı unutmayınız. İyi günler!" YAZ

BİTİR
