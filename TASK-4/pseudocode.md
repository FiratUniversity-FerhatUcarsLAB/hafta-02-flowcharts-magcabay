ALGORITMA UniversiteDersKayitSistemi
    BAŞLA
    GİRİŞ EKRANI GÖSTER
    KULLANICI_ROLÜ = RolKontrol()

    EĞER KULLANICI_ROLÜ = "Öğrenci" İSE
        // --- ENGEL VE DERS SEÇİM SÜRECİ ---
        EĞER MaliEngelVarMi(Öğrenci) İSE
            MESAJ("Mali veya idari engel bulundu! Kayıt yapılamaz.")
            BİTİR
        SON

        DersSepeti ← BOŞ

        TEKRAR
            DersListesi = DersleriListele(Filtre, Arama, Müfredat)
            SEÇİM = KullanıcıEylemi()

            EĞER SEÇİM = "Ekle" İSE
                Ders = DersSec()
                EĞER NOT OnKosulSaglandiMi(Ders, Öğrenci) İSE
                    MESAJ("Ön koşul sağlanmadı!")
                    DEVAM ET
                SON

                EĞER ZamanCakismasiVarMi(Ders, DersSepeti) İSE
                    MESAJ("Zaman çakışması var!")
                    DEVAM ET
                SON

                EĞER KrediToplami(DersSepeti + Ders) > 35 İSE
                    MESAJ("Kredi limiti aşıldı!")
                    DEVAM ET
                SON

                EĞER KontenjanMusaitMi(Ders) DEĞİLSE
                    EĞER KullanıcıOnayi("Yedek listeye eklensin mi?") = EVET İSE
                        YedekListeyeEkle(Ders, Öğrenci)
                    SON
                    DEVAM ET
                SON

                DersSepeti ← DersSepeti + Ders
                MESAJ("Ders sepete eklendi.")

            EĞER SEÇİM = "Çıkar" İSE
                DersSepeti ← DersSepeti - DersSec()
                MESAJ("Ders sepetinden çıkarıldı.")

        TA Kİ KullanıcıKaydıTamamla() = EVET OLANA KADAR

        // --- KONTROLLER VE KAYIT ---
        EĞER GPA(Öğrenci) < 2.5 İSE
            DURUM ← "Onay Bekliyor"
            DanismanBilgilendir(Öğrenci)
        DEĞİLSE
            DURUM ← "Kesinleşti"
        SON

        KayitKaydet(Öğrenci, DersSepeti, DURUM)
        KayitOzetiGoster(Öğrenci, DersSepeti, DURUM)

    EĞER KULLANICI_ROLÜ = "Danışman" İSE
        BekleyenKayitlar = BekleyenKayitlariGor()
        SEÇİLEN = KayitSec(BekleyenKayitlar)
        KARAR = OnaylaVeyaReddet(SEÇİLEN)
        DurumGuncelle(SEÇİLEN, KARAR)
        MESAJ("İşlem tamamlandı.")

    EĞER KULLANICI_ROLÜ = "Yönetici" İSE
        RaporlariGoster()
        DersKontenjanlariniGuncelle()
        SistemLoglariniIncele()
    SON

    MESAJ("Kayıt süreci tamamlandı.")
BİTİR

