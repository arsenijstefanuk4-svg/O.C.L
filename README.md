<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>O.C.L — Ouroboros Community League Arena</title>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;700;900&family=Teko:wght@500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-dark: #030407;
            --bg-card: rgba(13, 16, 28, 0.72);
            --gold: #ffb703;
            --gold-glow: rgba(255, 183, 3, 0.5);
            --gold-r: 255; --gold-g: 183; --gold-b: 3;
            --red: #ff2a4b;
            --red-glow: rgba(255, 42, 75, 0.5);
            --cyan: #00f2fe;
            --text-main: #f0f4f8;
            --text-sub: #98a2b8;
            --border-grid: rgba(255, 183, 3, 0.15);
            --canvas-tan: #7a5a3a;
        }

        body.theme-gold    { --gold: #ffb703; --gold-glow: rgba(255, 183, 3, 0.5);  --border-grid: rgba(255, 183, 3, 0.15); --gold-r:255; --gold-g:183; --gold-b:3; }
        body.theme-cyan     { --gold: #00f2fe; --gold-glow: rgba(0, 242, 254, 0.5);  --border-grid: rgba(0, 242, 254, 0.15); --gold-r:0; --gold-g:242; --gold-b:254; }
        body.theme-red       { --gold: #ff2a4b; --gold-glow: rgba(255, 42, 75, 0.5);  --border-grid: rgba(255, 42, 75, 0.15); --gold-r:255; --gold-g:42; --gold-b:75; }
        body.theme-purple   { --gold: #c026d3; --gold-glow: rgba(192, 38, 211, 0.5); --border-grid: rgba(192, 38, 211, 0.15); --gold-r:192; --gold-g:38; --gold-b:211; }
        body.theme-green    { --gold: #00e676; --gold-glow: rgba(0, 230, 118, 0.5);  --border-grid: rgba(0, 230, 118, 0.15); --gold-r:0; --gold-g:230; --gold-b:118; }
        body.theme-orange   { --gold: #ff6d00; --gold-glow: rgba(255, 109, 0, 0.5);  --border-grid: rgba(255, 109, 0, 0.15); --gold-r:255; --gold-g:109; --gold-b:0; }
        body.theme-blue     { --gold: #2979ff; --gold-glow: rgba(41, 121, 255, 0.5); --border-grid: rgba(41, 121, 255, 0.15); --gold-r:41; --gold-g:121; --gold-b:255; }
        body.theme-pink     { --gold: #ff4dab; --gold-glow: rgba(255, 77, 171, 0.5); --border-grid: rgba(255, 77, 171, 0.15); --gold-r:255; --gold-g:77; --gold-b:171; }
        body.theme-custom   { }

        * { box-sizing: border-box; margin: 0; padding: 0; }
        html { scroll-behavior: smooth; }

        body {
            font-family: 'Montserrat', sans-serif;
            background-color: var(--bg-dark);
            color: var(--text-main);
            line-height: 1.45;
            overflow-x: hidden;
            position: relative;
            background-image:
                radial-gradient(ellipse 70% 45% at 50% -5%, var(--gold-glow) 0%, transparent 60%),
                radial-gradient(ellipse 55% 40% at 105% 105%, rgba(255, 42, 75, 0.14) 0%, transparent 55%),
                radial-gradient(ellipse 40% 30% at -5% 60%, rgba(0, 242, 254, 0.06) 0%, transparent 60%),
                linear-gradient(to right, rgba(255, 255, 255, 0.025) 1px, transparent 1px),
                linear-gradient(to bottom, rgba(255, 255, 255, 0.025) 1px, transparent 1px);
            background-size: 100% 100%, 100% 100%, 100% 100%, 42px 42px, 42px 42px;
        }

        body::after {
            content: ""; position: fixed; inset: 0; pointer-events: none; z-index: 1;
            background: radial-gradient(ellipse 90% 80% at 50% 45%, transparent 55%, rgba(0,0,0,0.55) 100%);
        }

        body.no-motion *, body.no-motion *::before, body.no-motion *::after {
            animation: none !important; transition: none !important; scroll-behavior: auto !important;
        }

        #progress-bar {
            position: fixed; top: 0; left: 0; height: 3px;
            background: linear-gradient(90deg, var(--red), var(--gold), var(--cyan));
            width: 0%; z-index: 10000; box-shadow: 0 0 14px var(--gold-glow);
            transition: width 0.12s ease-out;
        }

        .ring-decor { position: fixed; inset: 0; z-index: 0; pointer-events: none; overflow: hidden; }
        .canvas-floor {
            position: absolute; left: 0; right: 0; bottom: 0; height: 45vh;
            background: radial-gradient(ellipse 90% 100% at 50% 100%, rgba(122, 90, 58, 0.10) 0%, transparent 70%);
            opacity: 0.8;
        }
        .ring-ropes-top, .ring-ropes-bottom {
            position: fixed; left: 0; width: 100%; height: 18px; z-index: 1;
            display: flex; flex-direction: column; justify-content: space-between; opacity: 0.55;
        }
        .ring-ropes-top { top: 0; }
        .ring-ropes-bottom { bottom: 0; transform: rotate(180deg); }
        .rope-line { height: 3px; width: 100%; border-radius: 3px; }
        .rope-line.r1 { background: linear-gradient(90deg, var(--red), transparent, var(--red)); box-shadow: 0 0 8px var(--red-glow); }
        .rope-line.r2 { background: linear-gradient(90deg, var(--text-main), transparent, var(--text-main)); opacity: 0.3; }
        .rope-line.r3 { background: linear-gradient(90deg, var(--gold), transparent, var(--gold)); box-shadow: 0 0 8px var(--gold-glow); }

        .spotlight-sweep {
            position: fixed; top: -30%; left: -20%; width: 55%; height: 160%; z-index: 0;
            background: radial-gradient(ellipse at center, var(--gold-glow) 0%, transparent 65%);
            opacity: 0.14; filter: blur(14px); animation: sweepLight 20s ease-in-out infinite;
        }
        @keyframes sweepLight {
            0%, 100% { transform: translateX(0) rotate(-8deg); }
            50% { transform: translateX(150vw) rotate(8deg); }
        }

        .corner-glow { position: fixed; width: 280px; height: 280px; border-radius: 50%; z-index: 0; filter: blur(70px); opacity: 0.22; animation: cornerPulse 7s ease-in-out infinite; }
        .corner-glow.cg-tl { top: -90px; left: -90px; background: var(--red); }
        .corner-glow.cg-br { bottom: -90px; right: -90px; background: var(--gold); animation-delay: 3.2s; }
        @keyframes cornerPulse { 0%, 100% { opacity: 0.14; transform: scale(1); } 50% { opacity: 0.28; transform: scale(1.12); } }

        .floating-particles { position: fixed; inset: 0; z-index: 0; }
        .p-ember {
            position: absolute; bottom: -5%; font-size: 1.1rem; opacity: 0;
            animation: emberRise linear infinite; color: var(--gold); filter: drop-shadow(0 0 6px var(--gold-glow));
        }
        @keyframes emberRise {
            0% { transform: translateY(0) translateX(0) rotate(0deg); opacity: 0; }
            10% { opacity: 0.5; }
            50% { transform: translateY(-52vh) translateX(var(--drift, 12px)) rotate(180deg); }
            90% { opacity: 0.3; }
            100% { transform: translateY(-102vh) translateX(0) rotate(360deg); opacity: 0; }
        }

        .customizer-panel { position: fixed; top: 15px; right: 15px; z-index: 9999; }
        .settings-toggle-btn {
            width: 44px; height: 44px; border-radius: 50%;
            background: rgba(13, 16, 28, 0.92); border: 1.5px solid var(--gold);
            color: var(--gold); font-size: 1.25rem; cursor: pointer;
            display: flex; align-items: center; justify-content: center;
            box-shadow: 0 4px 20px rgba(0,0,0,0.5), 0 0 16px var(--gold-glow); transition: 0.3s ease;
        }
        .settings-toggle-btn:hover { transform: rotate(90deg) scale(1.08); }

        .settings-dropdown {
            position: absolute; top: 54px; right: 0; width: 270px;
            background: rgba(11, 13, 24, 0.97); border: 1px solid var(--border-grid);
            border-radius: 16px; backdrop-filter: blur(16px);
            box-shadow: 0 14px 40px rgba(0,0,0,0.65);
            opacity: 0; visibility: hidden; transform: translateY(-10px) scale(0.97);
            transition: 0.25s cubic-bezier(0.4,0,0.2,1); max-height: 80vh; overflow-y: auto;
        }
        .settings-dropdown.open { opacity: 1; visibility: visible; transform: translateY(0) scale(1); }
        .settings-head { display: flex; align-items: center; justify-content: space-between; padding: 14px 16px; border-bottom: 1px solid var(--border-grid); }
        .settings-head strong { font-family: 'Teko', sans-serif; font-size: 1.25rem; letter-spacing: 1px; color: var(--gold); text-transform: uppercase; }
        .settings-close { background: none; border: none; color: var(--text-sub); font-size: 1.1rem; cursor: pointer; padding: 2px 6px; border-radius: 6px; }
        .settings-body { padding: 14px 16px 16px; }
        .settings-row { display: flex; flex-direction: column; gap: 9px; margin-bottom: 16px; }
        .settings-row span.label { font-size: 0.68rem; font-weight: 800; text-transform: uppercase; color: var(--text-sub); letter-spacing: 1.2px; }

        .color-dots { display: flex; gap: 9px; flex-wrap: wrap; }
        .dot-btn { width: 24px; height: 24px; border-radius: 50%; border: 2px solid transparent; cursor: pointer; transition: 0.2s; flex-shrink: 0; }
        .dot-btn:hover { transform: scale(1.18); }
        .dot-btn.active { border-color: #fff; transform: scale(1.14); box-shadow: 0 0 10px currentColor; }
        .dot-gold { background: #ffb703; } .dot-cyan { background: #00f2fe; } .dot-red { background: #ff2a4b; } .dot-purple { background: #c026d3; }
        .dot-green { background: #00e676; } .dot-orange { background: #ff6d00; } .dot-blue { background: #2979ff; } .dot-pink { background: #ff4dab; }

        .custom-color-row { display: flex; align-items: center; gap: 8px; }
        .swatch-native { -webkit-appearance: none; appearance: none; width: 34px; height: 34px; flex-shrink: 0; border: 2px solid rgba(255,255,255,0.15); border-radius: 9px; cursor: pointer; background: none; padding: 0; }
        .swatch-native::-webkit-color-swatch-wrapper { padding: 0; }
        .swatch-native::-webkit-color-swatch { border: none; border-radius: 7px; }
        .hex-input {
            flex: 1; min-width: 0; background: rgba(255,255,255,0.05); border: 1px solid var(--border-grid);
            border-radius: 8px; padding: 8px 10px; color: #fff; font-size: 0.82rem; font-weight: 700;
            font-family: 'Montserrat', monospace; outline: none;
        }
        .hex-input.invalid { border-color: var(--red); }
        .hex-hint { font-size: 0.68rem; color: var(--text-sub); margin-top: 1px; }

        .toggle-row { display: flex; align-items: center; justify-content: space-between; }
        .switch { position: relative; width: 44px; height: 24px; flex-shrink: 0; }
        .switch input { opacity: 0; width: 0; height: 0; }
        .slider-track { position: absolute; cursor: pointer; inset: 0; background: rgba(148,163,184,0.25); border-radius: 24px; transition: 0.3s; }
        .slider-track::before { content: ""; position: absolute; height: 18px; width: 18px; left: 3px; bottom: 3px; background: #fff; border-radius: 50%; transition: 0.3s; }
        .switch input:checked + .slider-track { background: var(--gold); box-shadow: 0 0 10px var(--gold-glow); }
        .switch input:checked + .slider-track::before { transform: translateX(20px); }

        #loader {
            position: fixed; inset: 0; background: #020203; z-index: 99999;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            transition: opacity 0.5s ease, visibility 0.5s; overflow: hidden;
        }
        .loader-ropes { position: absolute; left: 0; width: 100%; height: 14px; opacity: 0.7; }
        .loader-ropes.top { top: 0; } .loader-ropes.bottom { bottom: 0; }
        .loader-bg-glow { position: absolute; width: 500px; height: 500px; border-radius: 50%; background: radial-gradient(circle, var(--gold-glow) 0%, transparent 70%); filter: blur(22px); }
        .loader-round { font-family: 'Teko', sans-serif; font-size: 1.1rem; letter-spacing: 4px; color: var(--red); text-transform: uppercase; margin-bottom: 6px; }
        .punch-stage { position: relative; width: 220px; height: 160px; display: flex; align-items: center; justify-content: center; }
        .heavy-bag { font-size: 4.5rem; position: relative; z-index: 1; transform-origin: top center; animation: bagSwing 0.6s ease-in-out infinite alternate; }
        .glove-left, .glove-right { position: absolute; font-size: 3rem; z-index: 2; top: 35px; }
        .glove-left { left: 0; animation: punchLeft 0.6s ease-in-out infinite; }
        .glove-right { right: 0; transform: scaleX(-1); animation: punchRight 0.6s ease-in-out infinite 0.3s; }
        .impact-spark { position: absolute; width: 30px; height: 30px; border-radius: 50%; background: var(--gold); box-shadow: 0 0 25px var(--gold); opacity: 0; z-index: 3; animation: sparkFlash 0.6s infinite; }
        @keyframes bagSwing { 0% { transform: rotate(-8deg); } 100% { transform: rotate(8deg); } }
        @keyframes punchLeft { 0%, 100% { transform: translateX(0) rotate(-10deg); } 50% { transform: translateX(55px) rotate(15deg); } }
        @keyframes punchRight { 0%, 100% { transform: scaleX(-1) translateX(0) rotate(-10deg); } 50% { transform: scaleX(-1) translateX(55px) rotate(15deg); } }
        @keyframes sparkFlash { 0%, 40%, 60%, 100% { opacity: 0; transform: scale(0.5); } 50% { opacity: 1; transform: scale(1.4); } }
        .loader-bell { font-size: 1.6rem; margin-top: 6px; }
        .loader-bell.ding { animation: bellDing 0.5s ease; }
        @keyframes bellDing { 0%, 100% { transform: rotate(0); } 20% { transform: rotate(-25deg); } 40% { transform: rotate(20deg); } 60% { transform: rotate(-12deg); } 80% { transform: rotate(8deg); } }
        .loader-text { font-family: 'Teko', sans-serif; font-size: 2.2rem; letter-spacing: 3px; color: var(--gold); margin-top: 14px; text-align: center; padding: 0 20px; }
        .loader-fight { font-family: 'Teko', sans-serif; font-size: 4rem; letter-spacing: 6px; color: var(--red); opacity: 0; transform: scale(0.6); }
        .loader-fight.show { animation: fightFlash 0.6s ease forwards; }
        @keyframes fightFlash { 0% { opacity: 0; transform: scale(0.4) rotate(-5deg); } 60% { opacity: 1; transform: scale(1.15) rotate(2deg); } 100% { opacity: 1; transform: scale(1) rotate(0); } }
        .loader-timer { font-size: 0.9rem; color: var(--text-sub); margin-top: 6px; font-weight: 700; }

        .promo-banner {
            background: linear-gradient(135deg, rgba(255, 183, 3, 0.12), rgba(0, 242, 254, 0.12));
            border: 2px dashed var(--gold); border-radius: 18px; padding: 20px; text-align: center;
            margin: 20px auto 30px; max-width: 850px; box-shadow: 0 0 25px var(--gold-glow);
            position: relative; z-index: 2; backdrop-filter: blur(10px);
        }
        .promo-banner h3 { font-family: 'Teko', sans-serif; font-size: 2rem; color: var(--gold); text-transform: uppercase; margin-bottom: 6px; }
        .promo-banner p { font-size: 0.95rem; color: var(--text-sub); margin-bottom: 12px; }
        .promo-links { display: flex; justify-content: center; gap: 15px; flex-wrap: wrap; }
        .promo-btn {
            background: linear-gradient(45deg, var(--gold), #ff9800); color: #000; font-weight: 800;
            padding: 10px 22px; border-radius: 30px; text-decoration: none; font-size: 0.9rem;
            box-shadow: 0 0 15px var(--gold-glow); transition: 0.3s ease; display: inline-flex; align-items: center; gap: 8px;
        }
        .promo-btn.chat { background: linear-gradient(45deg, var(--cyan), #2979ff); color: #fff; }
        .promo-btn:hover { transform: translateY(-3px) scale(1.05); }

        .marquee-wrapper {
            background: linear-gradient(90deg, #100003, var(--red), var(--gold), #100003);
            color: #fff; font-weight: 900; font-size: 0.95rem; text-transform: uppercase;
            letter-spacing: 2px; padding: 10px 0; overflow: hidden; white-space: nowrap;
            width: 100vw; position: relative; left: 50%; transform: translateX(-50%);
            display: flex; border-bottom: 1px solid var(--gold); z-index: 2;
        }
        .marquee-content { display: flex; flex-shrink: 0; white-space: nowrap; animation: marquee 16s linear infinite; }
        .marquee-item { padding-right: 50px; }
        @keyframes marquee { 0% { transform: translateX(0%); } 100% { transform: translateX(-50%); } }

        .status-container { display: flex; justify-content: center; margin-top: 20px; position: relative; z-index: 2; }
        .status-badge {
            display: flex; align-items: center; gap: 10px;
            background: rgba(15, 18, 32, 0.85); border: 1px solid var(--gold);
            padding: 8px 18px; border-radius: 50px; box-shadow: 0 0 20px var(--gold-glow); backdrop-filter: blur(12px);
        }
        .radar-dot { width: 10px; height: 10px; border-radius: 50%; }
        .radar-dot.open { background: #00e676; box-shadow: 0 0 12px #00e676; }
        .radar-dot.closed { background: var(--red); box-shadow: 0 0 12px var(--red); }

        header { text-align: center; padding: 15px 15px; position: relative; z-index: 2; }
        .main-badge {
            display: inline-block; background: linear-gradient(45deg, var(--red), #ff5252); color: #fff;
            font-family: 'Teko', sans-serif; font-size: 1.3rem; font-weight: 700;
            padding: 2px 16px; border-radius: 4px; text-transform: uppercase; letter-spacing: 2px;
            box-shadow: 0 0 15px var(--red-glow); transform: skewX(-8deg); margin-bottom: 10px;
        }
        h1 { font-family: 'Teko', sans-serif; font-size: 3.4rem; line-height: 0.95; text-transform: uppercase; letter-spacing: 2px; }
        h1 span { color: var(--gold); text-shadow: 0 0 20px var(--gold-glow); }

        .search-wrapper { width: 100%; max-width: 650px; margin: 15px auto 0; padding: 0 10px; }
        .search-input {
            width: 100%; background: rgba(15, 18, 32, 0.9); border: 2px solid var(--border-grid);
            padding: 12px 18px; border-radius: 12px; color: #fff; font-size: 0.95rem; font-weight: 600; outline: none; transition: 0.3s;
        }
        .search-input:focus { border-color: var(--gold); box-shadow: 0 0 25px var(--gold-glow); }
        .search-input::placeholder { color: var(--text-sub); }

        .nav-scroller { display: flex; gap: 8px; overflow-x: auto; padding: 15px 10px 5px; scrollbar-width: none; justify-content: center; flex-wrap: wrap; }
        .nav-link {
            background: rgba(20, 25, 45, 0.7); border: 1px solid var(--border-grid); color: var(--text-sub);
            padding: 6px 14px; border-radius: 8px; font-weight: 700; font-size: 0.75rem; text-decoration: none;
            text-transform: uppercase; transition: 0.2s;
        }
        .nav-link:hover, .nav-link.active-link { border-color: var(--gold); color: #fff; background: rgba(255,255,255,0.05); }

        .container { width: 100%; max-width: 1250px; margin: 25px auto; padding: 0 15px; position: relative; z-index: 2; }
        section { margin-bottom: 35px; }

        .section-header { display: flex; align-items: center; gap: 10px; margin-bottom: 16px; border-bottom: 2px solid var(--border-grid); padding-bottom: 6px; }
        .section-header h2 { font-family: 'Teko', sans-serif; font-size: 2.1rem; text-transform: uppercase; letter-spacing: 1px; }
        .header-line { height: 4px; width: 25px; background: var(--gold); box-shadow: 0 0 12px var(--gold-glow); border-radius: 2px; }

        .rules-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(310px, 1fr)); gap: 16px; }
        
        /* КАРТОЧКИ: плотные, компактные, без лишних вертикальных отступов */
        .rule-card {
            background: var(--bg-card); 
            border: 1px solid var(--border-grid); 
            border-radius: 14px; 
            padding: 16px 18px;
            backdrop-filter: blur(16px); 
            display: flex; 
            flex-direction: column; 
            justify-content: space-between;
            transition: all 0.3s ease; 
            cursor: pointer; 
            position: relative; 
            overflow: hidden;
            box-shadow: 0 0 12px rgba(255, 183, 3, 0.12), inset 0 0 8px rgba(255, 183, 3, 0.04);
        }
        .rule-card::before {
            content: ""; position: absolute; top: 0; left: 0; right: 0; height: 3px;
            background: linear-gradient(90deg, var(--gold), transparent 70%); opacity: 0.7;
        }
        .rule-card:hover { 
            transform: translateY(-4px); 
            border-color: var(--gold); 
            box-shadow: 0 0 22px var(--gold-glow), inset 0 0 12px var(--gold-glow); 
        }
        .rule-card.punch { animation: cardPunch 0.3s ease; }
        @keyframes cardPunch { 0% { transform: scale(1); } 50% { transform: scale(0.97); } 100% { transform: scale(1); } }
        
        .rule-card.danger-card { 
            border-color: rgba(255, 42, 75, 0.3); 
            box-shadow: 0 0 12px rgba(255, 42, 75, 0.12), inset 0 0 8px rgba(255, 42, 75, 0.04);
        }
        .rule-card.danger-card::before { background: linear-gradient(90deg, var(--red), transparent 70%); }
        .rule-card.danger-card:hover { 
            border-color: var(--red); 
            box-shadow: 0 0 22px rgba(255, 42, 75, 0.35), inset 0 0 12px rgba(255, 42, 75, 0.1); 
        }

        .card-top { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 8px; gap: 8px; }
        .card-title { font-size: 1.05rem; font-weight: 800; line-height: 1.2; }

        .badge-penalty { font-size: 0.68rem; font-weight: 900; padding: 3px 8px; border-radius: 5px; text-transform: uppercase; white-space: nowrap; }
        .badge-penalty.warn { background: rgba(255, 183, 3, 0.15); color: var(--gold); border: 1px solid var(--gold); }
        .badge-penalty.danger { background: rgba(255, 42, 75, 0.15); color: var(--red); border: 1px solid var(--red); }
        .badge-penalty.info { background: rgba(0, 242, 254, 0.15); color: var(--cyan); border: 1px solid var(--cyan); }

        /* Текст внутри карточек без пустых разрывов строк */
        .card-desc { color: var(--text-sub); font-size: 0.88rem; display: flex; flex-direction: column; gap: 6px; }
        .card-desc strong { color: #fff; }

        .bans-flex { display: grid; grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); gap: 10px; }
        
        /* БАН-БОКСЫ: компактные, равномерная подсветка */
        .ban-box {
            background: rgba(255, 42, 75, 0.06); 
            border: 1px solid rgba(255, 42, 75, 0.25); 
            border-radius: 8px;
            padding: 10px 10px; 
            text-align: center; 
            font-weight: 800; 
            color: #ff6b81; 
            font-size: 0.82rem; 
            transition: 0.3s;
            box-shadow: 0 0 8px rgba(255, 42, 75, 0.1);
        }
        .ban-box:hover { 
            background: rgba(255, 42, 75, 0.2); 
            transform: translateY(-3px); 
            box-shadow: 0 0 16px rgba(255,42,75,0.3); 
        }

        #toast {
            position: fixed; bottom: 25px; left: 50%; transform: translateX(-50%) translateY(100px);
            background: var(--gold); color: #000; padding: 8px 20px; border-radius: 30px; font-weight: 800;
            font-size: 0.82rem; box-shadow: 0 0 20px var(--gold-glow); opacity: 0; transition: all 0.3s ease;
            z-index: 10000; pointer-events: none;
        }
        #toast.show { transform: translateX(-50%) translateY(0); opacity: 1; }

        #scrollTop {
            position: fixed; bottom: 20px; right: 20px; width: 44px; height: 44px; background: var(--gold);
            color: #000; border: none; border-radius: 50%; cursor: pointer; display: none; align-items: center;
            justify-content: center; font-weight: 900; font-size: 1.2rem; z-index: 999; box-shadow: 0 0 16px var(--gold-glow); transition: 0.2s;
        }
        #scrollTop:hover { transform: scale(1.1); }

        footer { text-align: center; padding: 25px 15px; border-top: 1px solid var(--border-grid); color: var(--text-sub); font-size: 0.82rem; position: relative; z-index: 2; }
        footer span { color: var(--gold); font-weight: 800; }

        @media (max-width: 480px) {
            h1 { font-size: 2.4rem; }
            .loader-text { font-size: 1.5rem; }
            .settings-dropdown { width: 240px; }
        }
    </style>
</head>
<body class="theme-gold">

    <div class="ring-decor" aria-hidden="true">
        <div class="canvas-floor"></div>
        <div class="ring-ropes-top"><div class="rope-line r1"></div><div class="rope-line r2"></div><div class="rope-line r3"></div></div>
        <div class="ring-ropes-bottom"><div class="rope-line r1"></div><div class="rope-line r2"></div><div class="rope-line r3"></div></div>
        <div class="spotlight-sweep"></div>
        <div class="corner-glow cg-tl"></div>
        <div class="corner-glow cg-br"></div>
        <div class="floating-particles" id="particles"></div>
    </div>

    <div class="customizer-panel">
        <button class="settings-toggle-btn" id="settingsBtn" onclick="toggleSettings()" title="Настройки">⚙</button>
        <div class="settings-dropdown" id="settingsDropdown">
            <div class="settings-head">
                <strong>Настройки</strong>
                <button class="settings-close" onclick="toggleSettings(false)" title="Закрыть">✕</button>
            </div>
            <div class="settings-body">
                <div class="settings-row">
                    <span class="label">Подсветка</span>
                    <div class="color-dots" id="colorDots">
                        <div class="dot-btn dot-gold active" onclick="setTheme('theme-gold', this)" title="Золотой неон"></div>
                        <div class="dot-btn dot-cyan" onclick="setTheme('theme-cyan', this)" title="Киберпанк синий"></div>
                        <div class="dot-btn dot-red" onclick="setTheme('theme-red', this)" title="Кровавый красный"></div>
                        <div class="dot-btn dot-purple" onclick="setTheme('theme-purple', this)" title="Неоновый фиолетовый"></div>
                        <div class="dot-btn dot-green" onclick="setTheme('theme-green', this)" title="Ринг зелёный"></div>
                        <div class="dot-btn dot-orange" onclick="setTheme('theme-orange', this)" title="Нокаут оранжевый"></div>
                        <div class="dot-btn dot-blue" onclick="setTheme('theme-blue', this)" title="Ледяной синий"></div>
                        <div class="dot-btn dot-pink" onclick="setTheme('theme-pink', this)" title="Неоновый розовый"></div>
                    </div>
                </div>
                <div class="settings-row">
                    <span class="label">Свой цвет</span>
                    <div class="custom-color-row">
                        <input type="color" class="swatch-native" id="customSwatch" value="#ffb703" oninput="applyCustomColor(this.value)">
                        <input type="text" class="hex-input" id="customHex" value="#ffb703" maxlength="7" placeholder="#ffb703" oninput="onHexTyped(this.value)">
                    </div>
                    <span class="hex-hint" id="hexHint">Введите HEX-код, например #ffb703</span>
                </div>
                <div class="settings-row toggle-row">
                    <span class="label">Анимации</span>
                    <label class="switch">
                        <input type="checkbox" id="motionToggle" checked onchange="toggleMotion(this.checked)">
                        <span class="slider-track"></span>
                    </label>
                </div>
            </div>
        </div>
    </div>

    <div id="loader">
        <div class="loader-ropes top"><div class="rope-line r1"></div><div class="rope-line r3"></div></div>
        <div class="loader-ropes bottom"><div class="rope-line r1"></div><div class="rope-line r3"></div></div>
        <div class="loader-bg-glow"></div>
        <div class="loader-round">Выход на ринг</div>
        <div class="punch-stage">
            <div class="glove-left">🥊</div>
            <div class="heavy-bag">🥊</div>
            <div class="glove-right">🥊</div>
            <div class="impact-spark"></div>
        </div>
        <div class="loader-bell" id="loaderBell">🔔</div>
        <div class="loader-text" id="loaderText">ПОДГОТОВКА АРЕНЫ O.C.L...</div>
        <div class="loader-fight" id="loaderFight">БОЙ!</div>
        <div class="loader-timer" id="loadTimer">Загрузка: 5 сек</div>
    </div>

    <div id="progress-bar"></div>
    <div id="toast">Правило скопировано в буфер!</div>

    <div class="marquee-wrapper">
        <div class="marquee-content">
            <span class="marquee-item">⚡ ПРАВИЛА БОЁВ Ouroboros Community League (O.C.L) • ОФИЦИАЛЬНЫЙ РЕГЛАМЕНТ • СОБЛЮДАЙТЕ ПРАВИЛА ЛИГИ • ⚡</span>
            <span class="marquee-item">⚡ ПРАВИЛА БОЁВ Ouroboros Community League (O.C.L) • ОФИЦИАЛЬНЫЙ РЕГЛАМЕНТ • СОБЛЮДАЙТЕ ПРАВИЛА ЛИГИ • ⚡</span>
        </div>
    </div>

    <header>
        <div class="status-container">
            <div class="status-badge">
                <div id="radarDot" class="radar-dot"></div>
                <span id="radarText" style="font-weight:800; font-size:0.75rem; text-transform:uppercase;">Проверка арены...</span>
            </div>
        </div>

        <div style="margin-top:15px;">
            <div class="main-badge">Ouroboros Community League</div>
            <h1>Правила боёв <span>O.C.L</span></h1>
        </div>

        <div class="promo-banner">
            <h3>Сообщество Ouroboros Community League</h3>
            <p>Присоединяйся к нашему официальному каналу и общайся в комьюнити-чате!</p>
            <div class="promo-links">
                <a href="https://t.me/OCLleague" target="_blank" class="promo-btn">📢 Telegram Канал</a>
                <a href="https://t.me/OCLleagechat" target="_blank" class="promo-btn chat">💬 Чат Лиги</a>
            </div>
        </div>

        <div class="search-wrapper">
            <input type="text" id="searchInput" class="search-input" placeholder="⚡ Поиск правил (пассив, демпси, багоюз, бекдеш, бан стили...)" oninput="searchRules()">
        </div>

        <div class="nav-scroller" id="navScroller">
            <a href="#pd" class="nav-link">1. Пассив</a>
            <a href="#bugs" class="nav-link">2. Багоюз</a>
            <a href="#combos" class="nav-link">3. Слоу клики</a>
            <a href="#skating" class="nav-link">4. С-кейтинг</a>
            <a href="#dd" class="nav-link">5. Дабл деш</a>
            <a href="#distance" class="nav-link">6. Дистанция</a>
            <a href="#ref" class="nav-link">7. Реферство</a>
            <a href="#disputes" class="nav-link">8. Оспоры</a>
            <a href="#respect" class="nav-link">9. Неуважение</a>
            <a href="#bans" class="nav-link">Бан стили</a>
        </div>
    </header>

    <main class="container">

        <section id="pd">
            <div class="section-header">
                <div class="header-line"></div>
                <h2>1. Пассив</h2>
            </div>
            <div class="rules-grid">
                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">1.1 ПД Фишинг</div>
                        <span class="badge-penalty info">Определение</span>
                    </div>
                    <div class="card-desc">
                        <span>ПД Фишинг — намеренное прекращение ударов/взаимодействия для идеального уклонения.</span>
                        <span>При спамящих комбо соперника можно сделать два уклона. Ждать удара можно максимум <strong>3 секунды</strong>.</span>
                        <span>Попытка удара, контрудар или способность сбрасывают таймер. Использование эмоций = бездействие.</span>
                    </div>
                </div>

                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">1.2 Пассив в конце боя для ульты</div>
                        <span class="badge-penalty danger">2 Фола</span>
                    </div>
                    <div class="card-desc">
                        <span>За пассив в конце боя с целью накопить/дать ульту выдается сразу 2 фола.</span>
                    </div>
                </div>

                <div class="rule-card danger-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">1.3 Подряд идущие ПД</div>
                        <span class="badge-penalty warn">Фол</span>
                    </div>
                    <div class="card-desc">
                        <span>Если вы делаете 2 ПД подряд, не начав атаку самостоятельно (даже в тайминг), за это дается фол.</span>
                    </div>
                </div>
            </div>
        </section>

        <section id="bugs">
            <div class="section-header">
                <div class="header-line"></div>
                <h2>2. Багоюз</h2>
            </div>
            <div class="rules-grid">
                <div class="rule-card danger-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">2.1 Парирование ульты</div>
                        <span class="badge-penalty danger">ТКО / 1.5 Фол</span>
                    </div>
                    <div class="card-desc">
                        <span>Прожатие блока под летящую ульту, когда ульта решающая для боя — карается ТКО.</span>
                        <span>Если ульта не решающая — дается <strong>1.5 фола</strong>.</span>
                    </div>
                </div>

                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">2.2 Нелегальный стаггеринг</div>
                        <span class="badge-penalty warn">Пред ➔ Фол</span>
                    </div>
                    <div class="card-desc">
                        <span>Задержка удара M1, становящегося неуклоняемым и притягивающего игрока вопреки кадрам уклона.</span>
                        <span>Первое нарушение — устное предупреждение, далее — фол. Обычный стаггеринг разрешен.</span>
                    </div>
                </div>
            </div>
        </section>

        <section id="combos">
            <div class="section-header">
                <div class="header-line"></div>
                <h2>3. Слоуклики</h2>
            </div>
            <div class="rules-grid">
                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">3.1 Правила применения</div>
                        <span class="badge-penalty warn">Фол</span>
                    </div>
                    <div class="card-desc">
                        <span>Медленные M1 разрешены только после попадания под ультимейт (не после способностей).</span>
                        <span>Разрешено использовать слоуклики только для <strong>ОДНОЙ СЕРИИ УДАРОВ</strong>, превышение ведет к фолу.</span>
                    </div>
                </div>

                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">3.2 Исключения</div>
                        <span class="badge-penalty info">Особые стили</span>
                    </div>
                    <div class="card-desc">
                        <span>Для стиля Крюк (corkscrew) слоуклики после ультимейта запрещены.</span>
                        <span>Для Айрон Фиста после ультимейта разрешено делать <strong>два комбо слоуклика</strong>.</span>
                    </div>
                </div>
            </div>
        </section>

        <section id="skating">
            <div class="section-header">
                <div class="header-line"></div>
                <h2>4. С-кейтинг и БД</h2>
            </div>
            <div class="rules-grid">
                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">4.1 С-кейтинг и бекдеш</div>
                        <span class="badge-penalty warn">Фол</span>
                    </div>
                    <div class="card-desc">
                        <span>Отход назад с зажатой клавишей С или стиком вниз (БД). Разрешен после удара/комбо по врагу.</span>
                        <span>Пропуск двух действий противника при отходе назад без атак — фол.</span>
                    </div>
                </div>

                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">4.2 Условия разрешений</div>
                        <span class="badge-penalty warn">Пред ➔ Фол</span>
                    </div>
                    <div class="card-desc">
                        <span>Скейт и бекдеш разрешены, если по сопернику прошел хотя бы один удар или серия.</span>
                    </div>
                </div>

                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">4.3 Против Демпси и Шотгана</div>
                        <span class="badge-penalty info">Особые правила</span>
                    </div>
                    <div class="card-desc">
                        <span>Против Демпси фишить можно, но С-скейтить нельзя. Против Шотгана разрешен бекдеш на способность.</span>
                    </div>
                </div>
            </div>
        </section>

        <section id="dd">
            <div class="section-header">
                <div class="header-line"></div>
                <h2>5. Дабл деш</h2>
            </div>
            <div class="rules-grid">
                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">5.1 Регламент дешей</div>
                        <span class="badge-penalty warn">1-2 Фола</span>
                    </div>
                    <div class="card-desc">
                        <span>Дабл деш (ДД) запрещен от финта, но разрешен от обычных ударов. Трипл деш запрещен в любой форме.</span>
                        <span>За запрещенный ДД — <strong>1 фол</strong>, за 3 деша и более — <strong>2 фола</strong>.</span>
                    </div>
                </div>
            </div>
        </section>

        <section id="distance">
            <div class="section-header">
                <div class="header-line"></div>
                <h2>6. Дистанция</h2>
            </div>
            <div class="rules-grid">
                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">6.1 Правила дистанции и байтов</div>
                        <span class="badge-penalty warn">Фол</span>
                    </div>
                    <div class="card-desc">
                        <span>Дистанцию держать можно. Байт на промах: если враг дважды промахнулся на вашей дистанции, а вы не ответили ударом — фол.</span>
                    </div>
                </div>
            </div>
        </section>

        <section id="ref">
            <div class="section-header">
                <div class="header-line"></div>
                <h2>7. Реферство</h2>
            </div>
            <div class="rules-grid">
                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">7.1 Поведение рефери</div>
                        <span class="badge-penalty warn">Выговор</span>
                    </div>
                    <div class="card-desc">
                        <span>Рефери обязан адекватно общаться с бойцами без оскорблений. За нарушение выдается выговор.</span>
                    </div>
                </div>

                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">7.2 Полномочия и съемка</div>
                        <span class="badge-penalty info">Обязанности</span>
                    </div>
                    <div class="card-desc">
                        <span>Запрещено вымогать деньги за проведение боев. Рефери обязан вести видеозапись каждого боя для разбора спорных моментов.</span>
                    </div>
                </div>
            </div>
        </section>

        <section id="disputes">
            <div class="section-header">
                <div class="header-line"></div>
                <h2>8. Оспоры боев</h2>
            </div>
            <div class="rules-grid">
                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">8.1 Порядок оспаривания</div>
                        <span class="badge-penalty warn">Выговор рефери</span>
                    </div>
                    <div class="card-desc">
                        <span>Оспорить бой можно при наличии видеозаписи у бойца или через главного рефери. Без записи рефери получает выговор.</span>
                    </div>
                </div>
            </div>
        </section>

        <section id="respect">
            <div class="section-header">
                <div class="header-line"></div>
                <h2>9. Кара за неуважение</h2>
            </div>
            <div class="rules-grid">
                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">9.1 Отмена заявок</div>
                        <span class="badge-penalty warn">Отстранение</span>
                    </div>
                    <div class="card-desc">
                        <span>Отмена заявки боя после поражения карается временным отстранением от боев.</span>
                    </div>
                </div>

                <div class="rule-card danger-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">9.2 Токсичность</div>
                        <span class="badge-penalty danger">Пред ➔ Фол</span>
                    </div>
                    <div class="card-desc">
                        <span>Вызывающее поведение карается предупреждением (первый раз), затем фолом.</span>
                        <span>Запрещены токсичные эмоции (эмоция L и пригибание Савамуры).</span>
                    </div>
                </div>
            </div>
        </section>

        <section id="bans">
            <div class="section-header">
                <div class="header-line"></div>
                <h2>Бан стили</h2>
            </div>
            <div class="bans-flex">
                <div class="ban-box searchable">slugger</div>
                <div class="ban-box searchable">hawk</div>
                <div class="ban-box searchable">white ash</div>
                <div class="ban-box searchable">shotgun</div>
                <div class="ban-box searchable">wolf</div>
                <div class="ban-box searchable">hammer</div>
                <div class="ban-box searchable">switch hit (без сбития ульты)</div>
                <div class="ban-box searchable">chronos</div>
                <div class="ban-box searchable">bullet</div>
                <div class="ban-box searchable">supernova</div>
                <div class="ban-box searchable">deimos</div>
                <div class="ban-box searchable">hitman</div>
                <div class="ban-box searchable">all shinies</div>
                <div class="ban-box searchable">exclusive styles</div>
            </div>
        </section>

    </main>

    <button id="scrollTop" onclick="window.scrollTo({top:0, behavior:'smooth'})">↑</button>

    <footer>
        <p>Официальный регламент соревновательной лиги <span>Ouroboros Community League (O.C.L)</span> &copy; 2026</p>
    </footer>

    <script>
        let timeLeft = 5;
        const timerElement = document.getElementById('loadTimer');
        const loaderText = document.getElementById('loaderText');
        const loaderBell = document.getElementById('loaderBell');
        const loaderFight = document.getElementById('loaderFight');
        const loaderStages = ["ПОДГОТОВКА АРЕНЫ O.C.L...", "СЕКУНДАНТЫ ГОТОВЯТ УГЛЫ РИНГА...", "СУДЬЯ ПРОВЕРЯЕТ ПЕРЧАТКИ...", "ПОСЛЕДНИЕ ИНСТРУКЦИИ БОЙЦАМ..."];

        const countdown = setInterval(() => {
            timeLeft--;
            if (timeLeft > 0) {
                timerElement.textContent = `Загрузка: ${timeLeft} сек`;
                loaderText.textContent = loaderStages[(5 - timeLeft) % loaderStages.length];
            } else {
                clearInterval(countdown);
                timerElement.textContent = `Гонг!`;
                loaderBell.classList.add('ding');
                loaderText.textContent = "РИНГ ГОТОВ";
                loaderFight.classList.add('show');
                setTimeout(() => {
                    const loader = document.getElementById('loader');
                    loader.style.opacity = '0';
                    setTimeout(() => loader.style.visibility = 'hidden', 500);
                }, 450);
            }
        }, 1000);

        function checkArenaStatus() {
            const now = new Date();
            const utc = now.getTime() + (now.getTimezoneOffset() * 60000);
            const msk = new Date(utc + (3600000 * 3));
            const hours = msk.getHours();

            const dot = document.getElementById('radarDot');
            const text = document.getElementById('radarText');

            if (hours >= 12 && hours < 22) {
                dot.className = 'radar-dot open';
                text.textContent = 'Арена открыта (12:00 - 22:00 МСК)';
                text.style.color = '#00e676';
            } else {
                dot.className = 'radar-dot closed';
                text.textContent = 'Арена закрыта (Открытие в 12:00 МСК)';
                text.style.color = 'var(--red)';
            }
        }
        checkArenaStatus();
        setInterval(checkArenaStatus, 30000);

        function searchRules() {
            const query = document.getElementById('searchInput').value.toLowerCase().trim();
            const items = document.querySelectorAll('.searchable');
            items.forEach(item => {
                const text = item.textContent.toLowerCase();
                item.style.display = text.includes(query) ? "" : "none";
            });
        }

        function copyCardText(card) {
            const title = card.querySelector('.card-title') ? card.querySelector('.card-title').innerText : 'Бан стиль';
            const desc = card.querySelector('.card-desc') ? card.querySelector('.card-desc').innerText : card.innerText;
            const textToCopy = `📌 [O.C.L Rule] ${title}: ${desc}`;

            card.classList.remove('punch');
            void card.offsetWidth;
            card.classList.add('punch');

            navigator.clipboard.writeText(textToCopy).then(() => {
                const toast = document.getElementById('toast');
                toast.classList.add('show');
                setTimeout(() => toast.classList.remove('show'), 2000);
            }).catch(() => {});
        }

        function spawnParticles() {
            const wrap = document.getElementById('particles');
            const symbols = ['🥊', '⭐', '💥', '🔥'];
            for (let i = 0; i < 8; i++) {
                const el = document.createElement('div');
                el.className = 'p-ember';
                el.textContent = symbols[i % symbols.length];
                el.style.left = (Math.random() * 96 + 2) + '%';
                el.style.setProperty('--drift', (Math.random() * 40 - 20) + 'px');
                el.style.animationDuration = (16 + Math.random() * 12) + 's';
                el.style.animationDelay = (Math.random() * 14) + 's';
                el.style.fontSize = (0.75 + Math.random() * 0.85) + 'rem';
                wrap.appendChild(el);
            }
        }
        spawnParticles();

        const themeClasses = ['theme-gold', 'theme-cyan', 'theme-red', 'theme-purple', 'theme-green', 'theme-orange', 'theme-blue', 'theme-pink', 'theme-custom'];

        function clearInlineTheme() {
            document.body.style.removeProperty('--gold');
            document.body.style.removeProperty('--gold-glow');
            document.body.style.removeProperty('--border-grid');
        }

        function setTheme(themeName, element) {
            clearInlineTheme();
            themeClasses.forEach(t => document.body.classList.remove(t));
            document.body.classList.add(themeName);
            document.querySelectorAll('.dot-btn').forEach(btn => btn.classList.remove('active'));
            if (element) element.classList.add('active');
            localStorage.setItem('ocl_theme', themeName);
            localStorage.removeItem('ocl_custom_color');
        }

        function hexToRgb(hex) {
            const m = /^#?([a-f\d]{3}|[a-f\d]{6})$/i.exec(hex.trim());
            if (!m) return null;
            let h = m[1];
            if (h.length === 3) h = h.split('').map(c => c + c).join('');
            const num = parseInt(h, 16);
            return { r: (num >> 16) & 255, g: (num >> 8) & 255, b: num & 255 };
        }

        function applyCustomColor(hex) {
            const rgb = hexToRgb(hex);
            if (!rgb) return;
            const normalized = '#' + [rgb.r, rgb.g, rgb.b].map(v => v.toString(16).padStart(2, '0')).join('');

            themeClasses.forEach(t => document.body.classList.remove(t));
            document.body.classList.add('theme-custom');
            document.body.style.setProperty('--gold', normalized);
            document.body.style.setProperty('--gold-glow', `rgba(${rgb.r}, ${rgb.g}, ${rgb.b}, 0.5)`);
            document.body.style.setProperty('--border-grid', `rgba(${rgb.r}, ${rgb.g}, ${rgb.b}, 0.15)`);

            document.querySelectorAll('.dot-btn').forEach(btn => btn.classList.remove('active'));
            document.getElementById('customSwatch').value = normalized;
            document.getElementById('customHex').value = normalized;
            document.getElementById('customHex').classList.remove('invalid');
            document.getElementById('hexHint').textContent = 'Применён свой цвет ' + normalized;

            localStorage.setItem('ocl_custom_color', normalized);
            localStorage.setItem('ocl_theme', 'theme-custom');
        }

        function onHexTyped(value) {
            const hint = document.getElementById('hexHint');
            const input = document.getElementById('customHex');
            let v = value.trim();
            if (v && !v.startsWith('#')) v = '#' + v;
            if (hexToRgb(v)) {
                input.classList.remove('invalid');
                applyCustomColor(v);
            } else {
                input.classList.add('invalid');
                hint.textContent = 'Неверный формат. Пример: #ffb703';
            }
        }

        function toggleSettings(force) {
            const dropdown = document.getElementById('settingsDropdown');
            if (typeof force === 'boolean') {
                dropdown.classList.toggle('open', force);
            } else {
                dropdown.classList.toggle('open');
            }
        }
        document.addEventListener('click', (e) => {
            const panel = document.querySelector('.customizer-panel');
            const dropdown = document.getElementById('settingsDropdown');
            if (dropdown.classList.contains('open') && !panel.contains(e.target)) {
                dropdown.classList.remove('open');
            }
        });

        function toggleMotion(enabled) {
            document.body.classList.toggle('no-motion', !enabled);
            localStorage.setItem('ocl_motion', enabled ? 'on' : 'off');
        }

        window.addEventListener('DOMContentLoaded', () => {
            const savedTheme = localStorage.getItem('ocl_theme');
            const savedCustom = localStorage.getItem('ocl_custom_color');

            if (savedTheme === 'theme-custom' && savedCustom) {
                applyCustomColor(savedCustom);
            } else if (savedTheme && themeClasses.includes(savedTheme)) {
                themeClasses.forEach(t => document.body.classList.remove(t));
                document.body.classList.add(savedTheme);
                document.querySelectorAll('.dot-btn').forEach(btn => btn.classList.remove('active'));
                const activeDot = document.querySelector(`[onclick*="'${savedTheme}'"]`);
                if (activeDot) activeDot.classList.add('active');
            }

            const savedMotion = localStorage.getItem('ocl_motion');
            const prefersReduced = window.matchMedia && window.matchMedia('(prefers-reduced-motion: reduce)').matches;
            const motionOn = savedMotion ? savedMotion === 'on' : !prefersReduced;
            document.getElementById('motionToggle').checked = motionOn;
            document.body.classList.toggle('no-motion', !motionOn);
        });

        const navLinks = Array.from(document.querySelectorAll('.nav-link'));
        const sections = navLinks.map(l => document.querySelector(l.getAttribute('href')));
        function updateActiveNav() {
            let current = sections[0];
            const y = window.scrollY + 120;
            sections.forEach(sec => { if (sec && sec.offsetTop <= y) current = sec; });
            navLinks.forEach(l => l.classList.toggle('active-link', current && l.getAttribute('href') === '#' + current.id));
        }

        window.onscroll = () => {
            const winScroll = document.documentElement.scrollTop;
            const height = document.documentElement.scrollHeight - document.documentElement.clientHeight;
            const scrolled = height > 0 ? (winScroll / height) * 100 : 0;
            document.getElementById("progress-bar").style.width = scrolled + "%";
            document.getElementById("scrollTop").style.display = winScroll > 300 ? "flex" : "none";
            updateActiveNav();
        };
        updateActiveNav();
    </script>
</body>
</html>
