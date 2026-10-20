<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Trendly - @trendlyorj</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background-color: #0b0f19;
            color: #ffffff;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
        }

        /* Telefon Çerçevesi (Mobil Görünüm) */
        .phone-container {
            width: 100%;
            max-width: 414px;
            height: 100%;
            max-height: 896px;
            background: #0f172a;
            display: flex;
            flex-direction: column;
            position: relative;
            box-shadow: 0 0 40px rgba(168, 85, 247, 0.2);
            overflow: hidden;
        }

        @media (min-width: 415px) {
            .phone-container {
                height: 85vh;
                border-radius: 40px;
                border: 4px solid #1e293b;
            }
        }

        /* Yükleme (Splash) Ekranı */
        .splash-screen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #0b0f19 0%, #1a103c 50%, #0f172a 100%);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 9999;
            transition: opacity 0.5s ease, visibility 0.5s ease;
        }

        .splash-screen.hidden {
            opacity: 0;
            visibility: hidden;
        }

        .splash-logo {
            font-size: 42px;
            font-weight: 900;
            letter-spacing: 1px;
            background: linear-gradient(135deg, #ff758c 0%, #ff7eb3 50%, #b388ff 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 25px;
            animation: pulse 1.5s infinite;
        }

        .loader-bar {
            width: 160px;
            height: 4px;
            background: #1e293b;
            border-radius: 4px;
            overflow: hidden;
            position: relative;
        }

        .loader-progress {
            position: absolute;
            top: 0;
            left: 0;
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, #ff007f, #b388ff);
            border-radius: 4px;
            animation: loadProgress 2s forwards;
        }

        @keyframes loadProgress {
            0% { width: 0%; }
            50% { width: 70%; }
            100% { width: 100%; }
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        /* Üst Header */
        header {
            padding: 18px 20px 12px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: #0f172a;
            z-index: 10;
        }

        .logo-area {
            display: flex;
            align-items: center;
        }

        .logo-text {
            font-size: 24px;
            font-weight: 800;
            letter-spacing: 0.5px;
            background: linear-gradient(135deg, #ff758c 0%, #ff7eb3 50%, #b388ff 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        /* Sağ Üst Profil (TikTok Linki) */
        .profile-badge {
            display: flex;
            align-items: center;
            gap: 8px;
            background: #1e293b;
            padding: 6px 12px;
            border-radius: 20px;
            border: 1px solid #334155;
            box-shadow: 0 0 10px rgba(168, 85, 247, 0.15);
            text-decoration: none;
            cursor: pointer;
            transition: 0.2s;
        }

        .profile-badge:active {
            transform: scale(0.95);
            border-color: #ff007f;
        }

        .profile-avatar {
            width: 22px;
            height: 22px;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .profile-avatar svg {
            width: 18px;
            height: 18px;
            fill: #ffffff;
        }

        .profile-username {
            font-size: 12px;
            font-weight: 600;
            color: #e2e8f0;
            letter-spacing: 0.2px;
        }

        /* Sekme Değiştirme Butonları (4'lü) */
        .tab-container {
            display: flex;
            gap: 4px;
            padding: 0 15px;
            margin-bottom: 15px;
        }

        .tab-btn {
            flex: 1;
            padding: 8px 2px;
            background: #1e293b;
            border: 1px solid #334155;
            color: #94a3b8;
            font-size: 11px;
            font-weight: 600;
            border-radius: 10px;
            cursor: pointer;
            transition: 0.2s;
            text-align: center;
        }

        .tab-btn.active {
            background: linear-gradient(135deg, #ff007f, #7928ca);
            color: #fff;
            border-color: transparent;
            box-shadow: 0 4px 12px rgba(255, 0, 127, 0.3);
        }

        /* Tek İçerik Alanı */
        .content-area {
            flex: 1;
            overflow-y: auto;
            padding: 0 20px 30px 20px;
        }

        /* Arama Kutusu */
        .search-box {
            background: #1e293b;
            border-radius: 14px;
            padding: 12px 16px;
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 12px;
            border: 1px solid #334155;
            position: sticky;
            top: 0;
            z-index: 5;
        }

        .search-box input {
            background: transparent;
            border: none;
            color: #fff;
            font-size: 15px;
            outline: none;
            width: 100%;
        }

        .search-box input::placeholder {
            color: #64748b;
        }

        /* Bilgi Bandı */
        .info-banner {
            background: linear-gradient(135deg, rgba(255, 0, 127, 0.15), rgba(121, 40, 202, 0.15));
            border: 1px solid rgba(255, 0, 127, 0.3);
            padding: 10px 14px;
            border-radius: 12px;
            margin-bottom: 15px;
            font-size: 12px;
            font-weight: 600;
            color: #ff7eb3;
            display: flex;
            flex-direction: column;
            gap: 4px;
        }

        .live-dot {
            width: 8px;
            height: 8px;
            background-color: #22c55e;
            border-radius: 50%;
            box-shadow: 0 0 8px #22c55e;
            display: inline-block;
            margin-right: 6px;
        }

        /* Bölüm Başlığı */
        .section-title {
            font-size: 18px;
            font-weight: 700;
            margin-bottom: 15px;
            color: #fff;
        }

        /* Paket Kartı */
        .combo-card {
            background: linear-gradient(145deg, #1e293b, #0f172a);
            border: 1px solid rgba(255, 117, 140, 0.3);
            border-radius: 16px;
            padding: 16px;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
        }

        .combo-info h4 {
            color: #ff758c;
            font-size: 15px;
            font-weight: 700;
            margin-bottom: 4px;
        }

        .combo-info p {
            color: #94a3b8;
            font-size: 12px;
        }

        .combo-action-btn {
            background: linear-gradient(135deg, #ff007f, #7928ca);
            border: none;
            color: #fff;
            padding: 10px 14px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: 700;
            cursor: pointer;
            box-shadow: 0 4px 12px rgba(255, 0, 127, 0.3);
            transition: 0.2s;
            white-space: nowrap;
        }

        .combo-action-btn:active {
            transform: scale(0.95);
        }

        /* Kartlar */
        .trend-card {
            background: linear-gradient(145deg, #1e293b, #0f172a);
            border: 1px solid #334155;
            border-radius: 16px;
            padding: 16px;
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            transition: 0.2s;
        }

        .trend-card:active {
            transform: scale(0.98);
            border-color: #ff758c;
        }

        .trend-content-left {
            display: flex;
            align-items: center;
            gap: 12px;
            flex: 1;
            cursor: pointer;
        }

        .trend-info h4 {
            font-size: 16px;
            font-weight: 600;
            margin-bottom: 4px;
            color: #ff7eb3;
        }

        .trend-info p {
            font-size: 13px;
            color: #94a3b8;
        }

        .card-right-area {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .trend-badge {
            background: rgba(255, 0, 127, 0.15);
            color: #ff758c;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            white-space: nowrap;
        }

        /* Favori Yıldız Butonu */
        .fav-btn {
            background: #0f172a;
            border: 1px solid #334155;
            color: #64748b;
            width: 32px;
            height: 32px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 16px;
            cursor: pointer;
            transition: 0.2s;
        }

        .fav-btn.active {
            color: #f59e0b;
            border-color: #f59e0b;
            background: rgba(245, 158, 11, 0.1);
        }

        /* Modal */
        .modal {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(5px);
            display: flex;
            justify-content: center;
            align-items: flex-end;
            z-index: 1000;
            opacity: 0;
            pointer-events: none;
            transition: 0.3s;
        }

        .modal.open {
            opacity: 1;
            pointer-events: auto;
        }

        .modal-content {
            background: #0f172a;
            border: 1px solid #334155;
            width: 100%;
            border-radius: 24px 24px 0 0;
            padding: 24px;
            transform: translateY(100%);
            transition: 0.3s cubic-bezier(0.1, 1, 0.1, 1);
        }

        .modal.open .modal-content {
            transform: translateY(0);
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px;
        }

        .modal-header h3 {
            font-size: 20px;
            color: #ff758c;
        }

        .close-btn {
            background: #1e293b;
            border: none;
            color: #fff;
            width: 32px;
            height: 32px;
            border-radius: 50%;
            font-size: 16px;
            cursor: pointer;
        }

        .action-button {
            width: 100%;
            padding: 14px;
            background: linear-gradient(135deg, #ff007f, #7928ca);
            border: none;
            border-radius: 14px;
            color: #fff;
            font-size: 16px;
            font-weight: 700;
            cursor: pointer;
            margin-top: 20px;
            box-shadow: 0 4px 15px rgba(255, 0, 127, 0.3);
            transition: 0.2s;
        }

        .action-button:active {
            transform: scale(0.98);
        }

        .copy-toast {
            position: absolute;
            top: 20px;
            left: 50%;
            transform: translateX(-50%) translateY(-50px);
            background: #22c55e;
            color: #fff;
            padding: 10px 20px;
            border-radius: 30px;
            font-size: 14px;
            font-weight: 600;
            box-shadow: 0 5px 15px rgba(34, 197, 94, 0.4);
            transition: 0.3s cubic-bezier(0.1, 1, 0.1, 1);
            z-index: 2000;
            opacity: 0;
            pointer-events: none;
        }

        .copy-toast.show {
            transform: translateX(-50%) translateY(0);
            opacity: 1;
        }
    </style>
</head>
<body>

    <div class="phone-container">
        <!-- Yükleme Ekranı -->
        <div class="splash-screen" id="splashScreen">
            <div class="splash-logo">Trendly</div>
            <div class="loader-bar">
                <div class="loader-progress"></div>
            </div>
        </div>

        <!-- Kopyalandı Bildirimi -->
        <div class="copy-toast" id="copyToast">✨ İçerik kopyalandı!</div>

        <!-- Header -->
        <header>
            <div class="logo-area">
                <div class="logo-text">Trendly</div>
            </div>
            <a href="https://www.tiktok.com/@trendlyorj" target="_blank" class="profile-badge">
                <div class="profile-avatar">
                    <svg viewBox="0 0 24 24"><path d="M19.59 6.69a4.83 4.83 0 0 1-3.77-4.25V2h-3.45v13.67a2.89 2.89 0 0 1-5.2 1.74 2.89 2.89 0 0 1 2.31-4.64 2.93 2.93 0 0 1 .88.13V9.4a6.84 6.84 0 0 0-1-.05A6.33 6.33 0 0 0 5 20.1a6.34 6.34 0 0 0 10.86-4.43v-7a8.16 8.16 0 0 0 4.77 1.52v-3.4a4.85 4.85 0 0 1-1-.1z"/></svg>
                </div>
                <div class="profile-username">@trendlyorj</div>
            </a>
        </header>

        <!-- İçerik Alanı -->
        <div class="content-area">
            <div class="search-box" id="searchBoxContainer">
                <span>🔍</span>
                <input type="text" id="searchInput" placeholder="Arşivde ara..." onkeyup="filterItems()">
            </div>

            <!-- Bilgi Bandı -->
            <div class="info-banner" id="infoBanner">
                <div><span class="live-dot"></span>Günlük Güncel Arşiv • Yenilendi</div>
                <div style="opacity: 0.9; font-weight: 500;">⚡ En İyi Paylaşım Saati: 19:00 - 23:00</div>
            </div>

            <!-- 4'lü Sekme Butonları -->
            <div class="tab-container">
                <button class="tab-btn active" id="tabHashtag" onclick="switchTab('hashtag')"># Hashtag</button>
                <button class="tab-btn" id="tabMusic" onclick="switchTab('music')">🎵 Müzik</button>
                <button class="tab-btn" id="tabSchedule" onclick="switchTab('schedule')">📅 Saatler</button>
                <button class="tab-btn" id="tabFavs" onclick="switchTab('favs')">⭐ Favoriler</button>
            </div>

            <!-- Hashtag Bölümü (100 Adet) -->
            <div id="hashtagSection">
                <div class="combo-card">
                    <div class="combo-info">
                        <h4>🚀 Viral Hashtag Paketi</h4>
                        <p>Etkileşimi en yüksek 5'li etiket grubunu anında kopyala</p>
                    </div>
                    <button class="combo-action-btn" onclick="copyCombo('#keşfet #fyp #viral #kesfetbeniöneçıkar #trend')">Kopyala</button>
                </div>

                <div class="section-title">Popüler Hashtag Arşivi (100) 📈</div>
                <div id="trendList">
                    <div class="trend-card" data-title="#keşfet"><div class="trend-content-left" onclick="openModal('#keşfet', 'Son 24 saatte 15.6M gönderide kullanıldı.')"><div class="trend-info"><h4>#keşfet</h4><p>15.6M Gönderi • Viral</p></div></div><div class="card-right-area"><div class="trend-badge">Hot</div><button class="fav-btn" onclick="toggleFav(this, '#keşfet')">★</button></div></div>
                    <div class="trend-card" data-title="#fyp"><div class="trend-content-left" onclick="openModal('#fyp', 'Global çapta en çok kullanılan ana akış etiketi.')"><div class="trend-info"><h4>#fyp</h4><p>43.5M Gönderi • Küresel</p></div></div><div class="card-right-area"><div class="trend-badge">Viral</div><button class="fav-btn" onclick="toggleFav(this, '#fyp')">★</button></div></div>
                    <div class="trend-card" data-title="#kesfetbeniöneçıkar"><div class="trend-content-left" onclick="openModal('#kesfetbeniöneçıkar', 'Keşfe düşmek için en çok tercih edilen etiket.')"><div class="trend-info"><h4>#kesfetbeniöneçıkar</h4><p>9.1M Gönderi • Yükselişte</p></div></div><div class="card-right-area"><div class="trend-badge">Trend</div><button class="fav-btn" onclick="toggleFav(this, '#kesfetbeniöneçıkar')">★</button></div></div>
                    <div class="trend-card" data-title="#komedi"><div class="trend-content-left" onclick="openModal('#komedi', 'Eğlence ve mizah içeriklerinin buluşma noktası.')"><div class="trend-info"><h4>#komedi</h4><p>6.4M Gönderi • Mizah</p></div></div><div class="card-right-area"><div class="trend-badge">Eğlence</div><button class="fav-btn" onclick="toggleFav(this, '#komedi')">★</button></div></div>
                    <div class="trend-card" data-title="#trend"><div class="trend-content-left" onclick="openModal('#trend', 'Güncel akımları takip etmek için ideal.')"><div class="trend-info"><h4>#trend</h4><p>11.4M Gönderi • Popüler</p></div></div><div class="card-right-area"><div class="trend-badge">Popüler</div><button class="fav-btn" onclick="toggleFav(this, '#trend')">★</button></div></div>
                    <div class="trend-card" data-title="#viral"><div class="trend-content-left" onclick="openModal('#viral', 'Hızlı izlenme almak isteyenler için.')"><div class="trend-info"><h4>#viral</h4><p>19.5M Gönderi • Yüksek Erişim</p></div></div><div class="card-right-area"><div class="trend-badge">Viral</div><button class="fav-btn" onclick="toggleFav(this, '#viral')">★</button></div></div>
                    <div class="trend-card" data-title="#mizah"><div class="trend-content-left" onclick="openModal('#mizah', 'Gülmek isteyenler için günlük doz.')"><div class="trend-info"><h4>#mizah</h4><p>5.2M Gönderi • Aktif</p></div></div><div class="card-right-area"><div class="trend-badge">Eğlence</div><button class="fav-btn" onclick="toggleFav(this, '#mizah')">★</button></div></div>
                    <div class="trend-card" data-title="#dans"><div class="trend-content-left" onclick="openModal('#dans', 'Trend dans akımları ve koreografiler.')"><div class="trend-info"><h4>#dans</h4><p>7.8M Gönderi • Hareketli</p></div></div><div class="card-right-area"><div class="trend-badge">Akım</div><button class="fav-btn" onclick="toggleFav(this, '#dans')">★</button></div></div>
                    <div class="trend-card" data-title="#muzik"><div class="trend-content-left" onclick="openModal('#muzik', 'Şarkı kapakları ve ses trendleri.')"><div class="trend-info"><h4>#muzik</h4><p>10.3M Gönderi • Sesler</p></div></div><div class="card-right-area"><div class="trend-badge">Müzik</div><button class="fav-btn" onclick="toggleFav(this, '#muzik')">★</button></div></div>
                    <div class="trend-card" data-title="#oyun"><div class="trend-content-left" onclick="openModal('#oyun', 'Oynanış videoları ve yayın kesitleri.')"><div class="trend-info"><h4>#oyun</h4><p>4.6M Gönderi • Gaming</p></div></div><div class="card-right-area"><div class="trend-badge">Gaming</div><button class="
