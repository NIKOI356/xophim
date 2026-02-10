<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>XoPhim - Phim hay cả rổ</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --bg-body: #14171c;
            --bg-nav: #1a1d23;
            --text-main: #ffffff;
            --text-dim: #a0a0a0;
            --accent: #f1c40f; /* Màu vàng icon */
            --search-bg: #252931;
            --mobile-card-bg: #3b4b7a; /* Màu xanh đặc trưng trên mobile */
            --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--bg-body);
            color: var(--text-main);
            overflow-x: hidden;
        }

        /* --- HEADER DESKTOP --- */
        header {
            background-color: transparent; 
            height: 70px;
            display: flex;
            align-items: center;
            padding: 0 20px;
            position: fixed; 
            width: 100%;
            top: 0;
            z-index: 1000;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            transition: var(--transition);
        }

        header.scrolled {
            background-color: rgba(26, 29, 35, 0.95);
            backdrop-filter: blur(10px);
            border-bottom: 1px solid #2d323a;
        }

        .container {
            width: 100%;
            max-width: 1600px;
            margin: 0 auto;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .logo-wrapper {
            display: flex;
            align-items: center;
            text-decoration: none;
            color: white;
            min-width: 150px;
            transition: var(--transition);
        }

        .logo-wrapper:hover {
            transform: scale(1.05);
        }

        .logo-icon {
            font-size: 32px;
            color: var(--accent);
            margin-right: 10px;
        }

        .logo-text h1 {
            font-size: 22px;
            line-height: 1;
        }

        .logo-text span {
            font-size: 11px;
            color: var(--text-dim);
        }

        .search-container {
            background: var(--search-bg);
            border-radius: 6px;
            display: flex;
            align-items: center;
            padding: 8px 15px;
            margin: 0 20px;
            flex: 0 1 350px;
            transition: var(--transition);
            border: 1px solid transparent;
        }

        .search-container:focus-within {
            border-color: var(--accent);
            box-shadow: 0 0 10px rgba(241, 196, 15, 0.2);
        }

        .search-container i {
            color: var(--text-dim);
            margin-right: 10px;
        }

        .search-container input {
            background: transparent;
            border: none;
            color: white;
            outline: none;
            width: 100%;
            font-size: 14px;
        }

        .nav-main {
            display: flex;
            align-items: center;
            gap: 18px;
        }

        .nav-link {
            text-decoration: none;
            color: #ddd;
            font-size: 14px;
            font-weight: 500;
            white-space: nowrap;
            transition: var(--transition);
            position: relative;
            padding: 10px 0; 
            cursor: pointer;
        }

        .nav-link:hover {
            color: var(--accent);
            transform: translateY(-2px);
        }

        .nav-link i {
            font-size: 10px;
            margin-left: 3px;
        }

        .dropdown {
            position: relative;
        }

        .dropdown::after {
            content: "";
            position: absolute;
            top: 100%;
            left: 0;
            width: 100%;
            height: 20px;
            display: block;
        }

        .dropdown-content {
            display: none;
            position: absolute;
            top: 100%;
            left: 50%;
            transform: translateX(-50%) translateY(10px);
            background: #1a1d23;
            min-width: 450px;
            padding: 20px;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
            border-radius: 8px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.8);
            margin-top: 15px;
            opacity: 0;
            transition: var(--transition);
            border: 1px solid #333;
            z-index: 1001; 
        }

        .dropdown.open .dropdown-content {
            display: grid;
            opacity: 1;
            transform: translateX(-50%) translateY(0);
        }

        .dropdown-content a {
            color: #ccc;
            text-decoration: none;
            font-size: 13px;
            padding: 8px;
            border-radius: 4px;
            transition: var(--transition);
            display: block;
            opacity: 1 !important; 
            transform: none !important;
        }

        .dropdown-content a:hover {
            color: var(--accent);
            background: rgba(255,255,255,0.05);
            padding-left: 12px;
        }

        .badge-new {
            background: #f39c12;
            color: #000;
            font-size: 9px;
            padding: 1px 4px;
            border-radius: 3px;
            font-weight: bold;
            margin-right: 4px;
        }

        .user-action button {
            background: #f0f0f0;
            color: #333;
            border: none;
            padding: 8px 18px;
            border-radius: 20px;
            font-weight: bold;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: var(--transition);
        }

        .user-action button:hover {
            background: var(--accent);
            transform: scale(1.05);
        }

        .btn-mobile {
            display: none;
            font-size: 22px;
            cursor: pointer;
            transition: var(--transition);
        }

        .btn-mobile:active {
            transform: scale(0.8);
        }

        .mobile-search-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--bg-body);
            z-index: 3000;
            display: none;
            padding: 15px;
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .mobile-search-overlay.active {
            display: block;
        }

        .search-mobile-header {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .search-mobile-bar {
            background: var(--search-bg);
            border-radius: 6px;
            display: flex;
            align-items: center;
            padding: 10px 15px;
            flex-grow: 1;
        }

        .search-mobile-bar input {
            background: transparent;
            border: none;
            color: white;
            outline: none;
            width: 100%;
            font-size: 16px;
        }

        .close-search-mobile {
            font-size: 24px;
            color: #ff6b6b;
            cursor: pointer;
        }

        .mobile-nav-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.85);
            z-index: 2000;
            display: none;
            align-items: flex-start;
            justify-content: center;
            padding-top: 40px;
            backdrop-filter: blur(5px);
        }

        .mobile-nav-overlay.active {
            display: flex;
        }

        .mobile-menu-card {
            background-color: var(--mobile-card-bg);
            width: 90%;
            max-width: 400px;
            border-radius: 15px;
            padding: 40px 25px 30px;
            position: relative;
            max-height: 85vh;
            overflow-y: auto;
            transform: translateY(-20px);
            transition: var(--transition);
        }

        .mobile-nav-overlay.active .mobile-menu-card {
            transform: translateY(0);
        }

        .close-mobile {
            position: absolute;
            top: 15px;
            left: 20px;
            font-size: 28px;
            color: #ff6b6b;
            cursor: pointer;
            transition: var(--transition);
        }

        .close-mobile:hover {
            transform: rotate(90deg);
        }

        .mobile-user-btn {
            background: #f0f0f0;
            color: #333;
            width: 100%;
            padding: 12px;
            border-radius: 25px;
            border: none;
            font-weight: bold;
            margin-bottom: 30px;
            font-size: 16px;
            transition: var(--transition);
        }

        .mobile-user-btn:active {
            transform: scale(0.95);
            background: var(--accent);
        }

        .mobile-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        .mobile-grid a, .mobile-collapse-btn {
            color: white;
            text-decoration: none;
            font-size: 16px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: none;
            border: none;
            width: 100%;
            text-align: left;
            cursor: pointer;
            padding: 5px 0;
            transition: var(--transition);
        }

        .mobile-grid a:active, .mobile-collapse-btn:active {
            color: var(--accent);
            padding-left: 10px;
        }

        .mobile-collapse-content {
            max-height: 0;
            overflow: hidden;
            grid-column: span 2;
            background: rgba(0,0,0,0.2);
            padding: 0 10px;
            border-radius: 8px;
            margin-top: -10px;
            transition: max-height 0.4s ease-out, padding 0.3s ease;
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 0;
            opacity: 0;
        }

        .mobile-collapse-content.active {
            max-height: 500px; 
            padding: 15px 10px;
            opacity: 1;
            gap: 10px;
        }

        .mobile-collapse-content a {
            font-size: 14px;
            color: #ddd;
            padding: 8px 0;
            opacity: 0;
            transform: translateX(-10px);
            transition: all 0.3s ease;
        }

        .mobile-collapse-content.active a {
            opacity: 1;
            transform: translateX(0);
        }

        /* --- HERO BANNER --- */
        .hero-slider {
            position: relative;
            width: 100%;
            height: 55vh; 
            overflow: hidden;
            background: #000;
        }

        .slider-container {
            display: flex;
            width: 100%;
            height: 100%;
            transition: transform 0.8s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .slide-item {
            min-width: 100%;
            height: 100%;
            position: relative;
        }

        .slide-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .slide-overlay {
            position: absolute;
            inset: 0;
            background: linear-gradient(to right, rgba(20,23,28,0.9) 20%, transparent 100%);
            display: flex;
            align-items: center;
            padding: 0 10%;
        }

        .slide-content {
            max-width: 600px;
            animation: slideUp 0.8s ease forwards;
        }

        @keyframes slideUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .slide-content h2 { font-size: 36px; margin-bottom: 10px; line-height: 1.2; }
        .slide-meta { display: flex; gap: 8px; margin-bottom: 20px; flex-wrap: wrap; }
        .meta-item { background: rgba(255,255,255,0.15); padding: 4px 12px; border-radius: 4px; font-size: 13px; border: 1px solid rgba(255,255,255,0.2); }
        .imdb-tag { color: var(--accent); border-color: var(--accent); font-weight: bold; }
        .slide-desc { color: #ccc; line-height: 1.6; margin-bottom: 30px; display: -webkit-box; -webkit-line-clamp: 3; -webkit-box-orient: vertical; overflow: hidden; font-size: 14px; }

        .btn-play-hero { background: var(--accent); color: black; border: none; width: 50px; height: 50px; border-radius: 50%; font-size: 20px; cursor: pointer; transition: 0.3s; margin-right: 20px; }
        .btn-play-hero:hover { transform: scale(1.1); }

        .slider-thumbnails {
            position: absolute;
            bottom: 30px;
            right: 5%;
            display: flex;
            gap: 10px;
            z-index: 10;
        }

        .thumb-item {
            width: 80px; height: 45px; border-radius: 6px; overflow: hidden; cursor: pointer; border: 2px solid transparent; opacity: 0.6; transition: 0.3s;
        }

        .thumb-item.active { border-color: var(--accent); opacity: 1; transform: scale(1.1); }
        .thumb-item img { width: 100%; height: 100%; object-fit: cover; }

        /* --- PHẦN QUAN TÂM --- */
        .interest-section { max-width: 1600px; margin: 40px auto; padding: 0 20px; }
        .interest-section h3 { font-size: 20px; margin-bottom: 20px; }
        .interest-grid { display: flex; overflow-x: auto; gap: 15px; padding-bottom: 10px; scroll-snap-type: x mandatory; }
        .interest-grid::-webkit-scrollbar { display: none; }
        .interest-card { min-width: 160px; height: 100px; border-radius: 12px; padding: 15px; display: flex; flex-direction: column; justify-content: space-between; transition: 0.3s; cursor: pointer; text-decoration: none; color: white; scroll-snap-align: start; flex-shrink: 0; }
        .interest-card:hover { transform: translateY(-5px); filter: brightness(1.1); }
        .interest-card span { font-size: 18px; font-weight: bold; }
        .interest-card i { font-size: 12px; margin-left: 5px; }

        /* --- DANH SÁCH PHIM QUỐC GIA --- */
        .movie-list-section { max-width: 1600px; margin: 50px auto; padding: 0 20px; }
        .list-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
        .list-header h3 { font-size: 20px; border-left: 4px solid var(--accent); padding-left: 15px; }
        .movie-scroll { display: flex; overflow-x: auto; gap: 15px; padding-bottom: 15px; scroll-snap-type: x mandatory; }
        .movie-scroll::-webkit-scrollbar { height: 4px; background: #252931; }
        .movie-scroll::-webkit-scrollbar-thumb { background: var(--accent); border-radius: 4px; }
        
        .movie-item { 
            min-width: 160px; 
            max-width: 160px; 
            flex-shrink: 0; 
            scroll-snap-align: start; 
            cursor: pointer; 
            transition: 0.3s; 
        }
        .movie-item:hover { transform: translateY(-8px); }
        .movie-thumb { 
            position: relative; 
            border-radius: 8px; 
            overflow: hidden; 
            aspect-ratio: 2/3; 
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
        }
        .movie-thumb img { width: 100%; height: 100%; object-fit: cover; }
        .movie-badge { position: absolute; top: 8px; left: 8px; background: rgba(0,0,0,0.75); color: var(--accent); padding: 2px 6px; font-size: 9px; border-radius: 4px; font-weight: bold; }
        .movie-title { margin-top: 10px; font-weight: 500; font-size: 14px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }

        /* --- TOP 10 XÉO XÉO STYLE --- */
        .top10-item {
            min-width: 200px; max-width: 200px; position: relative; margin-right: 35px; flex-shrink: 0; scroll-snap-align: start; cursor: pointer; perspective: 1000px;
        }
        .top10-thumb {
            position: relative; border-radius: 12px; overflow: hidden; aspect-ratio: 2/3; box-shadow: -10px 10px 20px rgba(0,0,0,0.6); transform: rotateY(-15deg) skewY(2deg); transition: 0.5s ease; border-left: 2px solid rgba(255,255,255,0.1); z-index: 5;
        }
        .top10-item:hover .top10-thumb { transform: rotateY(0) skewY(0) scale(1.05); box-shadow: 0 10px 30px rgba(241, 196, 15, 0.3); }
        .top10-number {
            position: absolute; bottom: -15px; left: -15px; font-size: 110px; font-weight: 900; color: var(--accent); font-style: italic; text-shadow: 5px 5px 15px rgba(0,0,0,0.9); line-height: 1; z-index: 1; 
            -webkit-text-stroke: 1px rgba(255,255,255,0.2);
        }
        .top10-info { margin-top: 15px; padding-left: 15px; }

        /* --- POPUP CHI TIẾT --- */
        .popup-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.95); z-index: 5000; display: none; overflow-y: auto; padding: 20px; backdrop-filter: blur(5px); }
        .popup-content { max-width: 850px; margin: 20px auto; background: var(--bg-nav); border-radius: 15px; display: grid; grid-template-columns: 220px 1fr; gap: 25px; padding: 25px; position: relative; box-shadow: 0 20px 50px rgba(0,0,0,0.5); }
        .popup-poster img { width: 100%; height: auto; border-radius: 10px; box-shadow: 0 5px 15px rgba(0,0,0,0.5); }
        .close-popup { position: absolute; top: 15px; right: 20px; font-size: 24px; cursor: pointer; color: #ff4d4d; transition: 0.2s; }
        .close-popup:hover { transform: rotate(90deg); }
        .lang-selector { display: flex; gap: 10px; margin-top: 20px; margin-bottom: 10px; }
        .lang-btn { background: #252931; border: 1px solid #444; color: white; padding: 5px 15px; border-radius: 20px; cursor: pointer; font-size: 12px; }
        .lang-btn.active { background: var(--accent); color: black; border-color: var(--accent); font-weight: bold; }
        .episode-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(80px, 1fr)); gap: 8px; margin-top: 10px; max-height: 200px; overflow-y: auto; padding-right: 5px; }
        .ep-btn { background: #252931; border: 1px solid #333; color: white; padding: 8px; border-radius: 4px; cursor: pointer; font-size: 12px; transition: 0.2s; }
        .ep-btn:hover { border-color: var(--accent); color: var(--accent); }
        .btn-view-now { background: var(--accent); color: black; border: none; padding: 12px 30px; border-radius: 25px; font-weight: bold; cursor: pointer; margin-top: 20px; display: inline-flex; align-items: center; gap: 10px; transition: 0.3s; }
        .btn-view-now:hover { transform: scale(1.05); filter: brightness(1.1); }

        /* --- RESPONSIVE --- */
        @media (max-width: 768px) {
            .nav-main, .search-container, .user-action, .slider-thumbnails { display: none; }
            .btn-mobile { display: block; }
            .hero-slider { height: 300px; } 
            .slide-content h2 { font-size: 24px; }
            .slide-desc { display: none; }
            .popup-content { grid-template-columns: 1fr; margin: 0; padding: 20px; text-align: center; }
            .popup-poster { max-width: 140px; margin: 0 auto; } 
            .movie-item { min-width: 140px; max-width: 140px; } 
            .top10-number { font-size: 70px; bottom: -10px; left: -10px; }
            .top10-item { min-width: 150px; }
            .lang-selector { justify-content: center; }
        }
    </style>
</head>
<body>

<header id="mainHeader">
    <div class="container">
        <div class="btn-mobile" onclick="toggleMenu('mobileMenu')">
            <i class="fas fa-bars"></i>
        </div>

        <a href="#" class="logo-wrapper">
            <div class="logo-icon"><i class="fas fa-play-circle"></i></div>
            <div class="logo-text">
                <h1>XoPhim</h1>
                <span>Phim hay cả rổ</span>
            </div>
        </a>

        <div class="search-container">
            <i class="fas fa-search"></i>
            <input type="text" placeholder="Tìm kiếm phim, diễn viên">
        </div>

        <nav class="nav-main">
            <a href="#" class="nav-link">Chủ Đề</a>
            <div class="dropdown">
                <a class="nav-link" onclick="handleDropdownClick(event)">Thể loại <i class="fas fa-caret-down"></i></a>
                <div class="dropdown-content">
                    <a href="#">Chính kịch</a><a href="#">Khoa Học</a><a href="#">Tình Cảm</a><a href="#">Cổ Trang</a>
                    <a href="#">Tâm Lý</a><a href="#">Bí Ẩn</a><a href="#">Hành Động</a><a href="#">Hình Sự</a>
                    <a href="#">Hài Hước</a><a href="#">Phiêu Lưu</a><a href="#">Gia Đình</a><a href="#">Kinh Dị</a>
                </div>
            </div>
            <a href="#" class="nav-link">Phim Lẻ</a>
            <a href="#" class="nav-link">Phim Bộ</a>
            <a href="#" class="nav-link">Xem Chung</a>
            <div class="dropdown">
                <a class="nav-link" onclick="handleDropdownClick(event)">Quốc gia <i class="fas fa-caret-down"></i></a>
                <div class="dropdown-content" style="min-width: 200px; grid-template-columns: 1fr;">
                    <a class="dropdown-item" href="#">Trung Quốc</a>
                    <a class="dropdown-item" href="#">Âu Mỹ</a>
                    <a class="dropdown-item" href="#">Hàn Quốc</a>
                    <a class="dropdown-item" href="#">Thổ Nhĩ Kỳ</a>
                    <a class="dropdown-item" href="#">Anh</a>
                    <a class="dropdown-item" href="#">Canada</a>
                    <a class="dropdown-item" href="#">Pháp</a>
                    <a class="dropdown-item" href="#">Bỉ</a>
                    <a class="dropdown-item" href="#">Nhật Bản</a>
                    <a class="dropdown-item" href="#">Thái Lan</a>
                </div>
            </div>
            <a href="#" class="nav-link">Diễn Viên</a>
            <a href="#" class="nav-link">Lịch Chiếu</a>
            <a href="#" class="nav-link"><span class="badge-new">NEW</span> Reviews</a>
        </nav>

        <div class="user-action">
            <button><i class="fas fa-user"></i> Thành viên</button>
        </div>

        <div class="btn-mobile" onclick="toggleMenu('mobileSearch')">
            <i class="fas fa-search"></i>
        </div>
    </div>
</header>

<section class="hero-slider">
    <div class="slider-container" id="heroSlider"></div>
    <div class="slider-thumbnails" id="heroThumbs"></div>
</section>

<section class="interest-section">
    <h3>Bạn đang quan tâm gì?</h3>
    <div class="interest-grid">
        <a href="#" class="interest-card" style="background: #3b5bdb;"><span>Chính kịch</span><div class="sub-text">Xem chủ đề <i class="fas fa-chevron-right"></i></div></a>
        <a href="#" class="interest-card" style="background: #20c997;"><span>Hài Hước</span><div class="sub-text">Xem chủ đề <i class="fas fa-chevron-right"></i></div></a>
        <a href="#" class="interest-card" style="background: #748ffc;"><span>Hành Động</span><div class="sub-text">Xem chủ đề <i class="fas fa-chevron-right"></i></div></a>
        <a href="#" class="interest-card" style="background: #9775fa;"><span>Hình Sự</span><div class="sub-text">Xem chủ đề <i class="fas fa-chevron-right"></i></div></a>
        <a href="#" class="interest-card" style="background: #ff922b;"><span>Tình Cảm</span><div class="sub-text">Xem chủ đề <i class="fas fa-chevron-right"></i></div></a>
        <a href="#" class="interest-card" style="background: #fa5252;"><span>Bí ẩn</span><div class="sub-text">Xem chủ đề <i class="fas fa-chevron-right"></i></div></a>
        <a href="#" class="interest-card" style="background: #252931; align-items: center; justify-content: center;"><span>+15 chủ đề</span></a>
    </div>
</section>

<section class="movie-list-section" id="koreanSection">
    <div class="list-header"><h3>Phim Hàn Quốc mới</h3></div>
    <div class="movie-scroll" id="koreanList"></div>
</section>

<section class="movie-list-section" id="chineseSection">
    <div class="list-header"><h3>Phim Trung Quốc mới</h3></div>
    <div class="movie-scroll" id="chineseList"></div>
</section>

<section class="movie-list-section" id="usukSection">
    <div class="list-header"><h3>Phim US-UK mới</h3></div>
    <div class="movie-scroll" id="usukList"></div>
</section>

<section class="movie-list-section" id="cinemaSection">
    <div class="list-header"><h3>Phim Điện Ảnh Mới Coóng <i class="fas fa-chevron-circle-right" style="font-size: 16px; margin-left: 5px;"></i></h3></div>
    <div class="movie-scroll" id="cinemaList"></div>
</section>

<section class="movie-list-section" id="top10Section" style="padding-bottom: 50px;">
    <div class="list-header"><h3>Top 10 Phim Lẻ Hay Nhức Nách <i class="fas fa-fire" style="color: #e67e22; margin-left: 5px;"></i></h3></div>
    <div class="movie-scroll" id="top10List" style="padding: 20px;"></div>
</section>

<div class="popup-overlay" id="moviePopup">
    <div class="popup-content" id="popupContent"></div>
</div>

<div class="mobile-search-overlay" id="mobileSearch">
    <div class="search-mobile-header">
        <div class="search-mobile-bar">
            <input type="text" id="mobileSearchInput" placeholder="Tìm kiếm phim, diễn viên">
        </div>
        <div class="close-search-mobile" onclick="toggleMenu('mobileSearch')">
            <i class="fas fa-times"></i>
        </div>
    </div>
</div>

<div class="mobile-nav-overlay" id="mobileMenu">
    <div class="mobile-menu-card">
        <div class="close-mobile" onclick="toggleMenu('mobileMenu')">&times;</div>
        
        <button class="mobile-user-btn"><i class="fas fa-user"></i> Thành viên</button>
        
        <div class="mobile-grid">
            <a href="#">Chủ Đề</a>
            <button class="mobile-collapse-btn" onclick="toggleCollapse('colGenre')">
                Thể loại <i class="fas fa-caret-down"></i>
            </button>
            <div id="colGenre" class="mobile-collapse-content">
                <a href="#">Chính kịch</a><a href="#">Khoa Học</a>
                <a href="#">Tình Cảm</a><a href="#">Cổ Trang</a>
                <a href="#">Tâm Lý</a><a href="#">Bí Ẩn</a><a href="#">Hành Động</a><a href="#">Hình Sự</a>
                <a href="#">Hài Hước</a><a href="#">Phiêu Lưu</a><a href="#">Gia Đình</a><a href="#">Kinh Dị</a>
            </div>

            <a href="#">Phim Lẻ</a>
            <a href="#">Phim Bộ</a>
            <a href="#">Xem Chung</a>

            <button class="mobile-collapse-btn" onclick="toggleCollapse('colCountry')">
                Quốc gia <i class="fas fa-caret-down"></i>
            </button>
            <div id="colCountry" class="mobile-collapse-content">
                <a href="#">Trung Quốc</a>
                <a href="#">Âu Mỹ</a>
                <a href="#">Hàn Quốc</a>
                <a href="#">Thổ Nhĩ Kỳ</a>
                <a href="#">Anh</a>
                <a href="#">Canada</a>
                <a href="#">Pháp</a>
                <a href="#">Bỉ</a>
                <a href="#">Nhật Bản</a>
                <a href="#">Thái Lan</a>
            </div>

            <a href="#">Diễn Viên</a>
            <a href="#">Lịch Chiếu</a>
            <a href="#" style="grid-column: span 2;">
                <span><span class="badge-new">NEW</span> Reviews</span>
            </a>
        </div>
    </div>
</div>

<script>
    const API_NEW = 'https://phimapi.com/danh-sach/phim-moi-cap-nhat?page=1';
    const API_DETAIL = 'https://phimapi.com/phim/';
    let currentSlide = 0;
    let slideCount = 0;

    async function initSite() {
        try {
            const res = await fetch(API_NEW);
            const data = await res.json();
            const items = data.items.slice(0, 6);
            slideCount = items.length;

            const slider = document.getElementById('heroSlider');
            const thumbs = document.getElementById('heroThumbs');

            slider.innerHTML = items.map((m, i) => `
                <div class="slide-item">
                    <img src="${m.thumb_url}" alt="${m.name}">
                    <div class="slide-overlay">
                        <div class="slide-content">
                            <div class="slide-meta">
                                <span class="meta-item imdb-tag">IMDb 0.0</span>
                                <span class="meta-item">${m.year}</span>
                                <span class="meta-item">Hoàn tất</span>
                            </div>
                            <h2>${m.name}</h2>
                            <p class="slide-desc">${m.origin_name} - Xem ngay tại XoPhim.</p>
                            <div class="btn-group">
                                <button class="btn-play-hero" onclick="showMovieDetail('${m.slug}')"><i class="fas fa-play"></i></button>
                                <span style="font-weight:bold; color:white; cursor:pointer;" onclick="showMovieDetail('${m.slug}')">Xem ngay</span>
                            </div>
                        </div>
                    </div>
                </div>
            `).join('');

            thumbs.innerHTML = items.map((m, i) => `
                <div class="thumb-item ${i===0?'active':''}" onclick="goToSlide(${i})">
                    <img src="${m.poster_url}" alt="thumb">
                </div>
            `).join('');

            setInterval(() => {
                currentSlide = (currentSlide + 1) % slideCount;
                goToSlide(currentSlide);
            }, 4000);

            loadCountryPhim('han-quoc', 'koreanList');
            loadCountryPhim('trung-quoc', 'chineseList');
            loadCountryPhim('au-my', 'usukList');
            loadCinemaPhim();
            loadTop10LePhim();

        } catch (e) { console.error("Lỗi API", e); }
    }

    async function loadTop10LePhim() {
        try {
            const res = await fetch(`https://phimapi.com/v1/api/danh-sach/phim-le?limit=10`);
            const data = await res.json();
            const items = data.data.items;
            document.getElementById('top10List').innerHTML = items.map((m, i) => `
                <div class="top10-item" onclick="showMovieDetail('${m.slug}')">
                    <span class="top10-number">${i + 1}</span>
                    <div class="top10-thumb">
                        <img src="https://phimimg.com/${m.poster_url}" alt="" style="width:100%; height:100%; object-fit:cover;">
                    </div>
                    <div class="top10-info">
                        <div class="top10-title">${m.name}</div>
                        <div class="top10-sub">${m.year} • FullHD</div>
                    </div>
                </div>
            `).join('');
        } catch (e) { console.error(e); }
    }

    async function loadCinemaPhim() {
        try {
            const res = await fetch(`https://phimapi.com/v1/api/danh-sach/phim-le?limit=12`);
            const data = await res.json();
            const items = data.data.items;
            document.getElementById('cinemaList').innerHTML = items.map(m => `
                <div class="movie-item" onclick="showMovieDetail('${m.slug}')">
                    <div class="movie-thumb">
                        <img src="https://phimimg.com/${m.poster_url}" alt="">
                        <span class="movie-badge">${m.quality || 'FullHD'}</span>
                    </div>
                    <div class="movie-title">${m.name}</div>
                </div>
            `).join('');
        } catch (e) { console.error(e); }
    }

    async function loadCountryPhim(slug, elementId) {
        try {
            const res = await fetch(`https://phimapi.com/v1/api/quoc-gia/${slug}?limit=12`);
            const data = await res.json();
            const items = data.data.items;
            document.getElementById(elementId).innerHTML = items.map(m => `
                <div class="movie-item" onclick="showMovieDetail('${m.slug}')">
                    <div class="movie-thumb">
                        <img src="https://phimimg.com/${m.poster_url}" alt="">
                        <span class="movie-badge">P.Đề / T.Minh</span>
                    </div>
                    <div class="movie-title">${m.name}</div>
                </div>
            `).join('');
        } catch (e) { console.error(e); }
    }

    async function showMovieDetail(slug) {
        try {
            const res = await fetch(API_DETAIL + slug);
            const data = await res.json();
            const movie = data.movie;
            const episodeServers = data.episodes; 

            const popup = document.getElementById('moviePopup');
            const content = document.getElementById('popupContent');
            
            const langButtons = episodeServers.map((server, idx) => 
                `<button class="lang-btn ${idx === 0 ? 'active' : ''}" onclick="switchLang(this, '${idx}')">${server.server_name}</button>`
            ).join('');

            const firstServerEpisodes = episodeServers[0].server_data;
            const episodeList = firstServerEpisodes.map(ep => `<button class="ep-btn">${ep.name}</button>`).join('');

            content.innerHTML = `
                <span class="close-popup" onclick="closePopup()">&times;</span>
                <div class="popup-poster"><img src="${movie.poster_url}"></div>
                <div>
                    <h2 style="font-size:24px; margin-bottom:10px;">${movie.name}</h2>
                    <div class="slide-meta">
                        <span class="meta-item imdb-tag">IMDb ${movie.tmdb.vote_average || '0.0'}</span>
                        <span class="meta-item">${movie.year}</span>
                        <span class="meta-item">${movie.quality}</span>
                    </div>
                    <p style="color:#aaa; line-height:1.6; margin-bottom:15px; font-size:13px;">${movie.content.substring(0, 250)}...</p>
                    
                    <div class="lang-selector">${langButtons}</div>
                    
                    <div id="epContainer" class="episode-grid">${episodeList}</div>
                    
                    <button class="btn-view-now">
                        <i class="fas fa-play"></i> Xem Ngay
                    </button>
                </div>
            `;
            
            window.currentMovieEpisodes = episodeServers;

            popup.style.display = 'block';
            document.body.style.overflow = 'hidden';
        } catch (e) { console.error(e); }
    }

    window.switchLang = function(btn, serverIdx) {
        document.querySelectorAll('.lang-btn').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        
        const serverData = window.currentMovieEpisodes[serverIdx].server_data;
        const newEpisodes = serverData.map(ep => `<button class="ep-btn">${ep.name}</button>`).join('');
        document.getElementById('epContainer').innerHTML = newEpisodes;
    };

    function closePopup() {
        document.getElementById('moviePopup').style.display = 'none';
        document.body.style.overflow = 'auto';
    }

    function goToSlide(i) {
        currentSlide = i;
        const slider = document.getElementById('heroSlider');
        if(slider) { slider.style.transform = `translateX(-${i * 100}%)`; }
        document.querySelectorAll('.thumb-item').forEach((t, idx) => {
            t.classList.toggle('active', idx === i);
        });
    }

    initSite();

    window.addEventListener('scroll', () => {
        const header = document.getElementById('mainHeader');
        if (window.scrollY > 50) { header.classList.add('scrolled'); } 
        else { header.classList.remove('scrolled'); }
    });

    function handleDropdownClick(event) {
        event.preventDefault();
        const parent = event.currentTarget.closest('.dropdown');
        document.querySelectorAll('.dropdown').forEach(d => { if (d !== parent) d.classList.remove('open'); });
        parent.classList.toggle('open');
    }

    function toggleMenu(id) {
        const menu = document.getElementById(id);
        menu.classList.toggle('active');
        if(id === 'mobileSearch' && menu.classList.contains('active')) {
            setTimeout(() => { document.getElementById('mobileSearchInput').focus(); }, 100);
        }
    }

    function toggleCollapse(id) {
        const content = document.getElementById(id);
        content.classList.toggle('active');
        const btn = content.previousElementSibling;
        const icon = btn.querySelector('i');
        if(content.classList.contains('active')) {
            icon.style.transform = "rotate(180deg)";
            icon.style.color = "var(--accent)";
            const links = content.querySelectorAll('a');
            links.forEach((link, index) => { link.style.transitionDelay = (index * 0.05) + "s"; });
        } else {
            icon.style.transform = "rotate(0deg)";
            icon.style.color = "white";
        }
    }

    window.onclick = function(event) {
        const menuOverlay = document.getElementById('mobileMenu');
        const searchOverlay = document.getElementById('mobileSearch');
        const popup = document.getElementById('moviePopup');
        if (event.target == menuOverlay) menuOverlay.classList.remove('active');
        if (event.target == searchOverlay) searchOverlay.classList.remove('active');
        if (event.target == popup) closePopup();
        if (!event.target.closest('.dropdown')) {
            document.querySelectorAll('.dropdown').forEach(d => d.classList.remove('open'));
        }
    }
</script>
</body>
</html>
