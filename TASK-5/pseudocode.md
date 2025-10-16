# --- Başlangıç ---
SISTEM_AKTIF = True
ALARM_ACTIVE = False
ALARM_LEVEL = 0  # 0: yok, 1: düşük, 2: orta, 3: yüksek

while SISTEM_AKTIF:  # 7/24 Sürekli Döngü
    # --- 1. Sensör Verisi Toplama ---
    motion = read_motion_sensors()            # Hareket sensörleri
    door_window = read_contact_sensors()     # Kapı/pencere sensörleri
    smoke = read_smoke_sensors()             # Duman sensörleri
    break_glass = read_break_glass_sensors() # Cam kırılma sensörleri
    camera_status = read_camera_status()     # Kamera durumu

    # --- 2. Veri Analizi ve Karar ---
    threat_score = compute_threat_score(motion, door_window, smoke, break_glass)

    if threat_score == 0:
        # Tehdit yoksa, alarm aktif değilse bekle ve tekrar oku
        if ALARM_ACTIVE:
            if alarm_reset_command_received():
                reset_alarm()
                ALARM_ACTIVE = False
                ALARM_LEVEL = 0
            else:
                continue
        else:
            wait(poll_interval)
            continue

    # --- 3. Yanlış Alarm Filtresi ---
    owner_home = is_owner_home()  # GPS, Wi-Fi, Manuel Mod
    if owner_home:
        log("Potansiyel tehdit, ev sahibi evde → yanlış alarm")
        notify_owner_app("Hareket algılandı, evdeysen dikkate alma")
        wait(poll_interval)
        continue

    # --- 4. Alarm Süreci ---
    if threat_score < LOW_THRESHOLD:
        ALARM_LEVEL = 1
    elif threat_score < HIGH_THRESHOLD:
        ALARM_LEVEL = 2
    else:
        ALARM_LEVEL = 3

    if ALARM_LEVEL >= 2 and not camera_status.active:
        activate_cameras()

    activate_siren_if_needed(ALARM_LEVEL)
    ALARM_ACTIVE = True

    message = format_alarm_message(ALARM_LEVEL, motion, door_window, smoke, break_glass, timestamp())
    send_notification_sms(owner_phone, message)
    send_notification_app(owner_userid, message)
    send_notification_email(owner_email, message)

    # --- 5. Alarm Kontrol Döngüsü ---
    while ALARM_ACTIVE:
        if alarm_reset_command_received():
            reset_alarm()
            ALARM_ACTIVE = False
            ALARM_LEVEL = 0
            break
        elif threat_persists():
            escalate_if_needed(ALARM_LEVEL)  # Gerekirse polis bildirimi, ek kamera
            wait(secondary_poll_interval)
        else:
            graceful_deactivate(ALARM_LEVEL)
            ALARM_ACTIVE = False
            ALARM_LEVEL = 0
            break

    # --- 6. Döngüye Dön ---
    wait(poll_interval)  # Ana döngü tekrar başlar

