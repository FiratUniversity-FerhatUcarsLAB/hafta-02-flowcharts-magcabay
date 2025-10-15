BAŞLA
    YAZ "Dil seçimi: Türkçe / English"
    OKU dil

    YAZ "Kartınızı takınız veya QR ile giriş yapınız"
    OKU giris_tipi

    EĞER giris_tipi geçersiz İSE
        YAZ "Geçersiz giriş! İşlem sonlandırıldı"
        DUR
    DEĞİLSE EĞER giris_tipi = "QR" İSE
        GOTO ANA_MENU
    DEĞİLSE
        // Kart ile giriş
        HATALI_SAYACI ← 0
        DÖNGÜ
            YAZ "PIN giriniz:"
            OKU pin
            EĞER pin doğru İSE
                ÇIK DÖNGÜ
            DEĞİLSE
                HATALI_SAYACI ← HATALI_SAYACI + 1
                EĞER HATALI_SAYACI = 3 İSE
                    YAZ "3 Hatalı PIN! Kart yutuldu"
                    DUR
                DEĞİLSE
                    YAZ "Hatalı PIN. Tekrar deneyiniz."
                BİTİR EĞER
            BİTİR EĞER
        SON DÖNGÜ

ANA_MENU:
    YAZ "Para birimini seçin: TL / USD / EUR"
    OKU para_birimi

    DÖNGÜ
        YAZ "ANA MENÜ Seçenekler:"
        YAZ "1-Bakiye 2-Para Çek 3-Hızlı Çek 4-QR Çek 5-Yatır 6-QR Yatır 7-PIN Değiştir 8-Kredi Kartı Öde 9-Diğer 0-Çıkış"
        OKU secim

        EĞER secim = 1 İSE
            YAZ "Bakiyeniz: ", bakiye, para_birimi

        DEĞİLSE EĞER secim = 2 VEYA secim = 3 VEYA secim = 4 İSE
            YAZ "Çekmek istediğiniz tutarı giriniz:"
            OKU tutar
            EĞER tutar <= 0 İSE
                YAZ "Tutar 0'dan büyük olmalıdır!"
                DEVAM ET DÖNGÜ
            BİTİR EĞER
            EĞER tutar > 7500 İSE
                YAZ "Günlük limit 7500 TL'yi aşıyor!"
                DEVAM ET DÖNGÜ
            BİTİR EĞER
            EĞER tutar > bakiye İSE
                YAZ "Yetersiz bakiye!"
                DEVAM ET DÖNGÜ
            BİTİR EĞER
            EĞER tutar % 20 ≠ 0 İSE
                YAZ "Tutar 20 TL katı olmalıdır!"
                DEVAM ET DÖNGÜ
            BİTİR EĞER
            EĞER ATM_nakit = YETERSİZ İSE
                YAZ "ATM'de yeterli nakit yok!"
                DEVAM ET DÖNGÜ
            BİTİR EĞER
            EĞER banknot_ariza = TRUE İSE
                YAZ "20 TL banknotlarında arıza var, farklı tutar giriniz"
                DEVAM ET DÖNGÜ
            BİTİR EĞER
            bakiye ← bakiye - tutar
            YAZ tutar, para_birimi, " verildi"
        
        DEĞİLSE EĞER secim = 5 VEYA secim = 6 İSE
            YAZ "Yatırmak istediğiniz tutarı giriniz:"
            OKU tutar
            bakiye ← bakiye + tutar
            YAZ tutar, para_birimi, " yatırıldı. Yeni bakiye: ", bakiye

        DEĞİLSE EĞER secim = 7 İSE
            YAZ "Yeni PIN giriniz:"
            OKU yeni_pin
            YAZ "PIN değiştirildi"

        DEĞİLSE EĞER secim = 8 İSE
            YAZ "Kredi kartı ödemesi seçiniz: 1-Asgari 2-Tam 3-Manuel"
            OKU odeme_secimi
            EĞER bakiye ≥ odeme_tutar DEĞİLSE
                YAZ "Yetersiz bakiye"
                DEVAM ET DÖNGÜ
            DEĞİLSE
                bakiye ← bakiye - odeme_tutar
                YAZ "Kredi kartı ödemesi yapıldı. Yeni bakiye: ", bakiye
            BİTİR EĞER

        DEĞİLSE EĞER secim = 9 İSE
            YAZ "Diğer hizmetler seçildi (Detaylar buraya)"

        DEĞİLSE EĞER secim = 0 İSE
            YAZ "Kartınızı almayı unutmayın. İyi günler!"
            DUR

        DEĞİLSE
            YAZ "Geçersiz seçim"

        YAZ "Başka işlem yapmak ister misiniz? (E/H)"
        OKU devam
        EĞER devam = 'H' İSE
            YAZ "Kartınızı almayı unutmayın. İyi günler!"
            DUR
        BİTİR EĞER
    SON DÖNGÜ
BİTİR
