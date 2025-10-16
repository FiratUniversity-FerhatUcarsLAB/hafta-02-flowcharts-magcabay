🔹 BAŞLA
BAŞLA

🔹 1️⃣ Kimlik Doğrulama ve Kullanıcı Yönetimi
Kullanıcıdan TC_No ve Şifre al

Eğer (TC_No ve Şifre Doğru_Mu?) ise
    Kullanıcı_Rolü = Rol_Belirle(Hasta, Doktor, Yönetici)
Aksi halde
    Hatalı_Giriş_Sayısı = Hatalı_Giriş_Sayısı + 1
    Eğer (Hatalı_Giriş_Sayısı >= 3) ise
        Hesap_Kilitlendi_Mesajı_Göster()
        SON
    Aksi halde
        Uyarı_Göster("Hatalı giriş! Lütfen tekrar deneyiniz.")
        Giriş_Ekranına_Dön()
Son

🔹 2️⃣ Rol Belirleme ve Ana Menü
Eğer (Kullanıcı_Rolü == "Hasta") ise
    Hasta_Menüsü_Göster()
Eğer (Kullanıcı_Rolü == "Doktor") ise
    Doktor_Menüsü_Göster()
Eğer (Kullanıcı_Rolü == "Yönetici") ise
    Yönetici_Menüsü_Göster()

🔹 3️⃣ Hasta Menüsü
Hasta_Menüsü:
    1 → Randevu_İşlemleri()
    2 → Tahlil_Sonuçları()
    3 → Profil_Görüntüleme_ve_Güncelleme()
    4 → Geri_Bildirim_Sistemi()
    5 → ÇIKIŞ

🔹 4️⃣ Randevu Alma Modülü
Randevu_İşlemleri():
    Branş_Filtrele()
    Tarih_Veya_Saat_Aralığı_Seç()

    Doktor_Listesini_Getir(Branş, Tarih)
    Uygun_Saatleri_Göster(Doluluk_Oranları_ile)

    Eğer (Seçilen_Saat_Uygun_Mu?) ise
        Randevuyu_Onayla()
        Randevu_Detaylarını_Göster(Doktor, Tarih, Saat, Poliklinik)
        Bildirim_Gönder(SMS veya E-posta)
        Bilgi_Mesajı("Randevunuz başarıyla oluşturuldu.")
    Aksi halde
        Alternatif_Saatleri_Öner()
        Yeni_Seçim_Yap()

    Randevu_Geçmişini_Göster()
    Randevu_İptal_ve_Değişiklik_Seçeneği()
    Menüye_Dön(Hasta_Menüsü)

🔹 5️⃣ Tahlil Sonuçları Modülü
Tahlil_Sonuçları():
    Tahlil_Türü_Seç(Kan, İdrar, MR, BT, vb.)

    Eğer (Tahlil_Kaydı_Var_Mı?) ise
        Eğer (Tahlil_Hazır_Mı?) ise
            Sonuçları_Görselleştir(Grafik, Tablo)
            Doktor_Yorumunu_Göster()
            Sonucu_İndir(PDF, Excel, Görüntü)
        Aksi halde
            Bekleme_Süresi_Tahmini_Göster()
            Bilgi_Mesajı("Tahlil sonucu henüz hazır değil.")
    Aksi halde
        Bilgi_Mesajı("Kayıtlı tahlil bulunamadı.")

    Tahlil_Geçmişini_Göster()
    Menüye_Dön(Hasta_Menüsü)

🔹 6️⃣ Profil Görüntüleme / Güncelleme Modülü
Profil_Görüntüleme_ve_Güncelleme():
    Kişisel_Bilgileri_Göster(Ad, Soyad, Telefon, E-posta)
    Kullanıcı_İsteğine_Göre_Bilgileri_Güncelle()
    Değişiklikleri_Kaydet()
    Onay_Mesajı("Profil bilgileriniz güncellendi.")
    Menüye_Dön(Hasta_Menüsü)

🔹 7️⃣ Anket ve Geri Bildirim Sistemi
Geri_Bildirim_Sistemi():
    Bilgi_Mesajı("Hizmet memnuniyetinizi değerlendirin.")

    Anket_Sorularını_Göster():
        1. Randevu alma kolaylığı nasıldı?
        2. Doktor iletişimi ve ilgisi yeterli miydi?
        3. Tahlil sonuçlarına erişim hızından memnun musunuz?
        4. Genel olarak sistemden memnuniyet puanınız (1-5 arası)?

    Puan_ve_Yorum_Al()
    Geri_Bildirimleri_Veritabanına_Kaydet()

    Teşekkür_Mesajı("Geri bildiriminiz için teşekkür ederiz!")

    Menüye_Dön(Hasta_Menüsü)

🔹 8️⃣ Doktor Modülü
Doktor_Menüsü:
    1 → Randevu_Takvimini_Görüntüle()
    2 → Hasta_Tahlillerini_Görüntüle()
    3 → Geri_Bildirim_Sonuçlarını_İncele()
    4 → ÇIKIŞ

Randevu_Takvimini_Görüntüle():
    Günlük_Randevu_Listesini_Göster()
    Randevu_Detaylarını_Düzenle(Gerekirse)
    Menüye_Dön(Doktor_Menüsü)

Hasta_Tahlillerini_Görüntüle():
    Hasta_Tahlil_Listesini_Getir()
    Yorum_Ekle_Veya_Düzenle()
    Menüye_Dön(Doktor_Menüsü)

Geri_Bildirim_Sonuçlarını_İncele():
    Ortalama_Memnuniyet_Puanı_Hesapla()
    Negatif_Yorumları_Filtrele()
    Rapor_Oluştur()
    Menüye_Dön(Doktor_Menüsü)

🔹 9️⃣ Yönetici Modülü
Yönetici_Menüsü:
    1 → Kullanıcı_Hesaplarını_Yönet()
    2 → Sistem_Ayarlarını_Güncelle()
    3 → Geri_Bildirim_Analiz_Raporları()
    4 → Log_Kayıtlarını_Görüntüle()
    5 → ÇIKIŞ

Kullanıcı_Hesaplarını_Yönet():
    Yeni_Hesap_Ekle()
    Mevcut_Hesabı_Düzenle()
    Hesap_Sil()
    Menüye_Dön(Yönetici_Menüsü)

Sistem_Ayarlarını_Güncelle():
    Poliklinik_Listesi_Güncelle()
    Doktor_Atamaları_Yap()
    Sistem_Yedekleme()
    Menüye_Dön(Yönetici_Menüsü)

Geri_Bildirim_Analiz_Raporları():
    Tüm_Anket_Verilerini_Çek()
    Ortalama_Puanları_Hesapla()
    Zaman_Bazlı_Memnuniyet_Grafiği()
    Raporu_Dışa_Aktar(PDF/Excel)
    Menüye_Dön(Yönetici_Menüsü)

🔹 🔗 Veritabanı İşlemleri
Veritabanı:
    Tablolar:
        - Kullanıcılar (TC, Şifre, Rol, Bilgiler)
        - Randevular (Hasta_ID, Doktor, Tarih, Saat, Durum)
        - Tahliller (Hasta_ID, Tür, Durum, Sonuç, Yorum)
        - Geri_Bildirimler (Hasta_ID, Puan, Yorum, Tarih)
        - Loglar (Kullanıcı_ID, İşlem, Zaman)
    
    İşlemler:
        Ekle(), Güncelle(), Sil(), Sorgula()

🔹 🔒 Hata ve Koşul Yönetimi
Eğer (Bağlantı_Hatası) ise
    Uyarı_Göster("Sistem bağlantı hatası. Lütfen tekrar deneyin.")

Eğer (Sunucu_Cevap_Vermiyor) ise
    Sistem_Loguna_Kaydet("Sunucu hatası")
    Bilgi_Mesajı("Geçici bir problem oluştu.")

🔹 🔚 SON
