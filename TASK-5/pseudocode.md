BASLA
  SISTEM_BASLAT()
  ALARM_DURUMU <- "KAPALI"
  ALARM_SEVIYESI <- 0
  
  DONGU DOGRU    // Ana döngü 7/24 çalışır
    // 1. Tüm sensörleri oku
    SENSOR_VERILERI <- SENSOR_OKU_TUMU()
    
    // 2. Tehdit tespiti
    HAREKET_VAR_MI <- SENSOR_VERILERI.hareket
    KAPI_ACIK_MI <- SENSOR_VERILERI.kapi
    PENCERE_ACIK_MI <- SENSOR_VERILERI.pencere
    CAM_KIRILDI_MI <- SENSOR_VERILERI.cam
    
    // 3. Tehdit seviyesini belirle
    EGER HAREKET_VAR_MI VEYA KAPI_ACIK_MI VEYA PENCERE_ACIK_MI VEYA CAM_KIRILDI_MI ISE
        // Farklı sensör kombinasyonlarına göre seviye belirleme
        EGER CAM_KIRILDI_MI ISE
            ALARM_SEVIYESI <- 3      // Yüksek tehdit
        ISE EGER HAREKET_VAR_MI VE KAPI_ACIK_MI ISE
            ALARM_SEVIYESI <- 2      // Orta tehdit
        ISE
            ALARM_SEVIYESI <- 1      // Düşük tehdit
        BITIR
        ALARM_DURUMU <- "AKTIF"
    ISE
        ALARM_DURUMU <- "KAPALI"
        ALARM_SEVIYESI <- 0
    BITIR
    
    // 4. Alarm verme ve bildirim gönderme
    EGER ALARM_DURUMU = "AKTIF" ISE
        KAMERA_AKTIVE_ET()
        SIREN_CALISTIR()
        
        // Tehdit seviyesine göre farklı bildirim türleri
        EGER ALARM_SEVIYESI = 1 ISE
            BILDIRIM_GONDER("Düşük Tehdit: Sistem küçük bir hareket algıladı.", ["App"])
        ISE EGER ALARM_SEVIYESI = 2 ISE
            BILDIRIM_GONDER("Orta Seviye Alarm: Kapı/Pencere hareketi algılandı.", ["SMS", "App"])
        ISE EGER ALARM_SEVIYESI = 3 ISE
            BILDIRIM_GONDER("YÜKSEK SEVİYE ALARM: Cam kırılması veya izinsiz giriş!", ["SMS", "App", "Email"])
        BITIR
    BITIR
    
    // 5. Alarm sıfırlama kontrolü
    EGER ALARM_DURUMU = "AKTIF" ISE
        EGER SIFIRLAMA_KOMUTU_GELDI_MI() = TRUE ISE
            ALARM_DURUMU <- "KAPALI"
            ALARM_SEVIYESI <- 0
            SIREN_DURDUR()
            KAMERA_DURDUR()
            BILDIRIM_GONDER("Alarm sıfırlandı. Sistem normale döndü.", ["App", "Email"])
        BITIR
    BITIR
    
    // 6. Kısa bekleme (örnek: 2 saniye)
    BEKLE(2 saniye)
    
  BITIR_DONGU

BITIR
