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
            --accent-2: #00f2fe;
            --accent-3: #ff2a4b;
            --light-alpha: 0.16;
        }

        body.theme-boxing { --gold:#ffb703; --gold-glow:rgba(255,183,3,.55); --accent-2:#ff2a4b; --accent-3:#00f2fe; --border-grid:rgba(255,183,3,.20); --gold-r:255; --gold-g:183; --gold-b:3; }
        body.theme-relax { --gold:#57e8ff; --gold-glow:rgba(87,232,255,.40); --accent-2:#7c8cff; --accent-3:#8affc1; --border-grid:rgba(87,232,255,.18); --gold-r:87; --gold-g:232; --gold-b:255; }
        body.theme-rgb { --gold:#ff4fd8; --gold-glow:rgba(255,79,216,.48); --accent-2:#00f2fe; --accent-3:#7cff00; --border-grid:rgba(255,79,216,.22); --gold-r:255; --gold-g:79; --gold-b:216; }
        body.theme-night { --gold:#9b7cff; --gold-glow:rgba(155,124,255,.45); --accent-2:#00d9ff; --accent-3:#ff4f9a; --border-grid:rgba(155,124,255,.20); --gold-r:155; --gold-g:124; --gold-b:255; }
        body.theme-champion { --gold:#ffd166; --gold-glow:rgba(255,209,102,.58); --accent-2:#00e5ff; --accent-3:#ff496c; --border-grid:rgba(255,209,102,.22); --gold-r:255; --gold-g:209; --gold-b:102; }
        body.theme-custom .marquee-wrapper { background:linear-gradient(90deg, color-mix(in srgb, var(--gold) 25%, #050509), var(--gold), color-mix(in srgb, var(--accent-2) 45%, #050509)); background-size:200% 100%; animation:customFlow 6s ease infinite; }
        body.theme-custom .rule-card:hover { box-shadow:0 0 26px var(--gold-glow), inset 0 0 14px var(--gold-glow); }
        @keyframes customFlow { 0%,100% { background-position:0% 50%; } 50% { background-position:100% 50%; } }
        body.theme-gold { --gold:#ffb703; --gold-glow:rgba(255,183,3,.55); --accent-2:#ff2a4b; --accent-3:#00f2fe; --border-grid:rgba(255,183,3,.20); --gold-r:255; --gold-g:183; --gold-b:3; }


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

        body::before {
            content: ""; position: fixed; inset: -25%; pointer-events: none; z-index: 0;
            background: conic-gradient(from 90deg at 50% 50%, var(--accent-2), transparent 18%, var(--gold-glow), transparent 42%, var(--accent-3), transparent 72%, var(--accent-2));
            filter: blur(90px); opacity: .14; animation: ambientLight 18s ease-in-out infinite alternate;
        }
        @keyframes ambientLight {
            0% { transform: rotate(0deg) scale(1); opacity: .10; }
            50% { transform: rotate(18deg) scale(1.08); opacity: .19; }
            100% { transform: rotate(-12deg) scale(1.02); opacity: .13; }
        }
        body.theme-rgb::before { animation: rgbAura 7s linear infinite; opacity: .22; }
        @keyframes rgbAura { to { transform: rotate(360deg) scale(1.08); filter: blur(85px) hue-rotate(360deg); } }

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
            opacity: 0.22; filter: blur(18px); animation: sweepLight 15s ease-in-out infinite;
        }
        @keyframes sweepLight {
            0%, 100% { transform: translateX(0) rotate(-8deg); }
            50% { transform: translateX(150vw) rotate(8deg); }
        }

        .corner-glow { position: fixed; width: 280px; height: 280px; border-radius: 50%; z-index: 0; filter: blur(62px); opacity: 0.28; animation: cornerPulse 7s ease-in-out infinite; }
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
        .settings-body { padding: 11px 13px 13px; }
        .settings-row { display: flex; flex-direction: column; gap: 6px; margin-bottom: 11px; }
        .settings-row span.label { font-size: 0.68rem; font-weight: 800; text-transform: uppercase; color: var(--text-sub); letter-spacing: 1.2px; }

        .style-grid { display:grid; grid-template-columns:repeat(2, minmax(0,1fr)); gap:8px; }
        .style-btn { position:relative; overflow:hidden; min-height:58px; border:1px solid var(--border-grid); border-radius:11px; background:rgba(255,255,255,.045); color:#fff; cursor:pointer; padding:8px 9px; text-align:left; transition:.28s ease; }
        .style-btn::before { content:""; position:absolute; inset:-40%; background:linear-gradient(120deg, transparent 35%, rgba(255,255,255,.18), transparent 65%); transform:translateX(-70%) rotate(8deg); transition:.55s ease; }
        .style-btn:hover::before { transform:translateX(70%) rotate(8deg); }
        .style-btn:hover { transform:translateY(-2px); border-color:var(--gold); box-shadow:0 0 18px var(--gold-glow); }
        .style-btn.active { border-color:var(--gold); background:linear-gradient(135deg, rgba(255,255,255,.10), rgba(255,255,255,.035)); box-shadow:0 0 18px var(--gold-glow), inset 0 0 14px var(--gold-glow); }
        .style-btn .style-icon { display:block; font-size:1.2rem; line-height:1; margin-bottom:4px; }
        .style-btn .style-name { display:block; font-size:.72rem; font-weight:900; letter-spacing:.7px; text-transform:uppercase; }
        .style-btn .style-sub { display:block; margin-top:2px; font-size:.58rem; color:var(--text-sub); }
        .style-boxing { --style-a:#ffb703; --style-b:#ff2a4b; } .style-relax { --style-a:#57e8ff; --style-b:#7c8cff; }
        .style-rgb { --style-a:#ff4fd8; --style-b:#00f2fe; } .style-night { --style-a:#9b7cff; --style-b:#00d9ff; }
        .style-champion { --style-a:#ffd166; --style-b:#00e5ff; }
        .style-btn .style-icon { color:var(--style-a); text-shadow:0 0 12px var(--style-a); }


        .custom-color-box { padding: 8px; border: 1px solid var(--border-grid); border-radius: 10px; background: rgba(255,255,255,.035); }
        .custom-color-controls { display:flex; align-items:center; gap:7px; }
        .custom-color-input { width:42px; height:34px; padding:0; border:1px solid var(--border-grid); border-radius:8px; background:transparent; cursor:pointer; }
        .custom-hex { flex:1; min-width:0; height:34px; border:1px solid var(--border-grid); border-radius:8px; background:rgba(255,255,255,.05); color:#fff; padding:0 9px; font:700 .78rem 'Montserrat',sans-serif; outline:none; }
        .custom-hex:focus { border-color:var(--gold); box-shadow:0 0 12px var(--gold-glow); }
        .custom-color-hint { display:block; margin-top:5px; color:var(--text-sub); font-size:.58rem; }
        .custom-apply { width:100%; margin-top:7px; border:1px solid var(--gold); border-radius:8px; padding:6px 8px; background:rgba(255,255,255,.05); color:#fff; font-weight:800; cursor:pointer; transition:.25s; }
        .custom-apply:hover { background:var(--gold); color:#080808; box-shadow:0 0 15px var(--gold-glow); }

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
        section { margin-bottom: 16px; }

        .section-header { display: flex; align-items: center; gap: 7px; margin-bottom: 7px; border-bottom: 2px solid var(--border-grid); padding-bottom: 3px; }
        .section-header h2 { font-family: 'Teko', sans-serif; font-size: 2.1rem; text-transform: uppercase; letter-spacing: 1px; }
        .header-line { height: 4px; width: 25px; background: var(--gold); box-shadow: 0 0 12px var(--gold-glow); border-radius: 2px; }

        .rules-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 7px; align-items: start; }
        
        /* КАРТОЧКИ: плотные, компактные, без лишних вертикальных отступов */
        .rule-card {
            background: var(--bg-card); 
            border: 1px solid var(--border-grid); 
            border-radius: 14px; 
            padding: 9px 11px;
            backdrop-filter: blur(16px); 
            display: flex; 
            flex-direction: column; 
            justify-content: flex-start;
            align-self: start;
            transition: transform .32s cubic-bezier(.2,.8,.2,1), border-color .28s ease, box-shadow .32s ease, background .32s ease; 
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

        .card-top { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 2px; gap: 5px; }
        .card-title { font-size: 1.02rem; font-weight: 800; line-height: 1.15; }

        .badge-penalty { font-size: 0.68rem; font-weight: 900; padding: 3px 8px; border-radius: 5px; text-transform: uppercase; white-space: nowrap; }
        .badge-penalty.warn { background: rgba(255, 183, 3, 0.15); color: var(--gold); border: 1px solid var(--gold); }
        .badge-penalty.danger { background: rgba(255, 42, 75, 0.15); color: var(--red); border: 1px solid var(--red); }
        .badge-penalty.info { background: rgba(0, 242, 254, 0.15); color: var(--cyan); border: 1px solid var(--cyan); }

        /* Текст внутри карточек без пустых разрывов строк */
        .card-desc { color: var(--text-sub); font-size: 0.84rem; line-height: 1.22; display: flex; flex-direction: column; gap: 0; }
        .card-desc strong { color: #fff; }

        .bans-flex { display: grid; grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); gap: 6px; }
        
        /* БАН-БОКСЫ: компактные, равномерная подсветка */
        .ban-box {
            background: rgba(255, 42, 75, 0.06); 
            border: 1px solid rgba(255, 42, 75, 0.25); 
            border-radius: 8px;
            padding: 7px 8px; 
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
            font-size: 0.82rem; box-shadow: 0 0 20px var(--gold-glow); opacity: 0; transition: transform .32s cubic-bezier(.2,.8,.2,1), border-color .28s ease, box-shadow .32s ease, background .32s ease;
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
        footer span { color: var(--gold); font-weight: 800; }

        /* Единый свет + микро-анимации для всего интерфейса */
        .promo-banner, .status-badge, .search-input, .nav-link, .rule-card, .ban-box, footer {
            box-shadow: 0 0 0 1px rgba(255,255,255,.015), 0 10px 30px rgba(0,0,0,.18), 0 0 24px color-mix(in srgb, var(--gold) 12%, transparent);
        }
        .promo-banner { animation: floatPanel 6s ease-in-out infinite; }
        .status-badge { animation: statusPulse 3s ease-in-out infinite; }
        .nav-link { position:relative; overflow:hidden; }
        .nav-link::after { content:""; position:absolute; left:-120%; bottom:0; width:90%; height:2px; background:linear-gradient(90deg,transparent,var(--gold),transparent); transition:.45s; }
        .nav-link:hover::after, .nav-link.active-link::after { left:115%; }
        .search-input { animation: searchGlow 5s ease-in-out infinite; }
        .rule-card, .ban-box { animation: cardEnter .65s both; animation-delay:calc(var(--i, 0) * 55ms); }
        .rule-card:nth-child(2), .ban-box:nth-child(2) { --i:1; } .rule-card:nth-child(3), .ban-box:nth-child(3) { --i:2; }
        .rule-card:nth-child(4), .ban-box:nth-child(4) { --i:3; } .rule-card:nth-child(5), .ban-box:nth-child(5) { --i:4; }
        .rule-card::after, .ban-box::after { content:""; position:absolute; inset:-60% -25%; background:linear-gradient(105deg, transparent 42%, rgba(255,255,255,.09) 50%, transparent 58%); transform:translateX(-80%) rotate(8deg); animation:shineSweep 7s ease-in-out infinite; pointer-events:none; }
        .section-header .header-line { animation:linePulse 2.5s ease-in-out infinite; }
        .main-badge, .promo-btn, #scrollTop { animation:accentPulse 3.2s ease-in-out infinite; }
        .rope-line.r1, .rope-line.r3 { animation:ropeGlow 2.8s ease-in-out infinite alternate; }
        .reveal-ready { opacity:0; transform:translateY(12px); }
        .reveal-ready.visible { opacity:1; transform:translateY(0); transition:opacity .65s ease, transform .65s cubic-bezier(.2,.8,.2,1); }
        .rule-card.reveal-ready.visible, .ban-box.reveal-ready.visible { animation:cardEnter .65s both; }
        @keyframes cardEnter { from { opacity:0; transform:translateY(14px) scale(.985); } to { opacity:1; transform:translateY(0) scale(1); } }
        @keyframes shineSweep { 0%,55% { transform:translateX(-85%) rotate(8deg); } 75%,100% { transform:translateX(85%) rotate(8deg); } }
        @keyframes linePulse { 0%,100% { width:25px; box-shadow:0 0 8px var(--gold-glow); } 50% { width:45px; box-shadow:0 0 20px var(--gold-glow); } }
        @keyframes accentPulse { 0%,100% { filter:drop-shadow(0 0 0 transparent); } 50% { filter:drop-shadow(0 0 10px var(--gold-glow)); } }
        @keyframes statusPulse { 0%,100% { transform:translateY(0); } 50% { transform:translateY(-2px); } }
        @keyframes searchGlow { 0%,100% { box-shadow:0 0 0 rgba(0,0,0,0); } 50% { box-shadow:0 0 22px color-mix(in srgb, var(--gold) 12%, transparent); } }
        @keyframes floatPanel { 0%,100% { transform:translateY(0); } 50% { transform:translateY(-3px); } }
        @keyframes ropeGlow { from { opacity:.55; } to { opacity:.95; } }
        body.theme-rgb .marquee-wrapper, body.theme-rgb .promo-btn { background-size:300% 300%; animation:rgbFlow 5s ease infinite; }
        body.theme-rgb .rule-card, body.theme-rgb .section-header h2, body.theme-rgb h1 span { text-shadow:0 0 16px var(--gold-glow); }
        @keyframes rgbFlow { 0% { background-position:0% 50%; } 50% { background-position:100% 50%; } 100% { background-position:0% 50%; } }
        body.theme-relax .promo-banner { animation:relaxFloat 8s ease-in-out infinite; }
        @keyframes relaxFloat { 0%,100% { transform:translateY(0) rotate(0); } 50% { transform:translateY(-2px) rotate(.15deg); } }
        @media (prefers-reduced-motion: reduce) {
            *, *::before, *::after { animation-duration:.01ms !important; animation-iteration-count:1 !important; transition-duration:.01ms !important; }
        }

        @media (max-width: 480px) {
            h1 { font-size: 2.4rem; }
            .loader-text { font-size: 1.5rem; }
            .settings-dropdown { width: 240px; }
        }
    </style>
</head>
<body class="theme-boxing">

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
                    <span class="label">Стиль арены</span>
                    <div class="style-grid" id="styleGrid">
                        <button class="style-btn style-boxing active" onclick="setTheme('theme-boxing', this)"><span class="style-icon">🥊</span><span class="style-name">Бокс</span><span class="style-sub">Ринг • золото • красный</span></button>
                        <button class="style-btn style-relax" onclick="setTheme('theme-relax', this)"><span class="style-icon">🌊</span><span class="style-name">Relax</span><span class="style-sub">Спокойный холодный свет</span></button>
                        <button class="style-btn style-rgb" onclick="setTheme('theme-rgb', this)"><span class="style-icon">🌈</span><span class="style-name">RGB</span><span class="style-sub">Динамический неон</span></button>
                        <button class="style-btn style-night" onclick="setTheme('theme-night', this)"><span class="style-icon">🌌</span><span class="style-name">Night</span><span class="style-sub">Фиолетовый • кибер</span></button>
                        <button class="style-btn style-champion" onclick="setTheme('theme-champion', this)"><span class="style-icon">🏆</span><span class="style-name">Champion</span><span class="style-sub">Премиум • пояс • арена</span></button>
                    </div>
                </div>
                <div class="settings-row">
                    <span class="label">Свой цвет</span>
                    <div class="custom-color-box">
                        <div class="custom-color-controls">
                            <input class="custom-color-input" id="customColor" type="color" value="#ffb703" oninput="syncCustomColor(this.value)">
                            <input class="custom-hex" id="customHex" type="text" value="#ffb703" maxlength="7" placeholder="#RRGGBB" oninput="syncCustomHex(this.value)">
                        </div>
                        <span class="custom-color-hint">Выбери любой цвет для подсветки сайта</span>
                        <button class="custom-apply" onclick="applyCustomColor()">Применить цвет</button>
                    </div>
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
            <a href="#no-show" class="nav-link">10. Вылеты / Неявка</a>
            <a href="#title-fights" class="nav-link">11. Титульник / Дивизионы</a>
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

        <section id="no-show">
            <div class="section-header">
                <div class="header-line"></div>
                <h2>10. Вылеты / Неявка</h2>
            </div>
            <div class="rules-grid">
                <div class="rule-card danger-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">10.1 Проблемы со связью и вылеты</div>
                        <span class="badge-penalty danger">5 минут</span>
                    </div>
                    <div class="card-desc">
                        <span>Если прямо посреди матча у бойца оборвалось соединение или вылетела игра, включается счётчик: у него есть ровно <strong>5 минут</strong> на немедленное возвращение.</span>
                        <span>Если боец не возвращается в установленный срок, поединок может быть аннулирован либо рефери может присудить <strong>технический нокаут (ТКО)</strong>.</span>
                    </div>
                </div>

                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">10.2 Неявка обоих бойцов</div>
                        <span class="badge-penalty warn">10 минут</span>
                    </div>
                    <div class="card-desc">
                        <span>Рефери вправе отменить бой, если <strong>оба бойца не явились в течение 10 минут</strong> с момента начала ожидания.</span>
                    </div>
                </div>

                <div class="rule-card searchable" onclick="copyCardText(this)">
                    <div class="card-top">
                        <div class="card-title">10.3 Возвращение после выхода из боя</div>
                        <span class="badge-penalty warn">Отстранение</span>
                    </div>
                    <div class="card-desc">
                        <span>Если один из бойцов покинул бой, но затем вернулся, поединок должен быть продолжен примерно с теми же результатами, которые были до выхода.</span>
                        <span>При отказе соблюдать это требование рефери вправе назначить <strong>временное отстранение</strong>.</span>
                    </div>
                </div>
            </div>
        </section>

        <section id="title-fights">
            <div class="section-header">
                <div class="header-line"></div>
                <h2>11. Титульник / Смена дивизионов</h2>
            </div>
            <div class="rules-grid">
                <div class="rule-card searchable">
                    <div class="card-top">
                        <div class="card-title">11.1 Смена дивизиона и путь наверх</div>
                        <span class="badge-penalty info">Переход</span>
                    </div>
                    <div class="card-desc">
                        <span>При сильном доминировании администрация может принудительно перевести бойца в более высокий дивизион.</span>
                        <span>В обычном порядке для перехода наверх необходимо завоевать чемпионский пояс текущего дивизиона и провести минимум <strong>одну успешную защиту</strong>.</span>
                        <span>Если боец прошёл в дивизион по рангу, но не способен показывать там результат, он может быть возвращён на ступень ниже. Данное правило <strong>временно не применяется к H.C.L</strong>.</span>
                        <span>Если рейтинг бойца достиг необходимого значения для следующего дивизиона, он может попросить о переводе. Если рефери видят, что рейтинг уже соответствует нужному дивизиону, они вправе перевести бойца без дополнительных переговоров.</span>
                    </div>
                </div>

                <div class="rule-card searchable">
                    <div class="card-top">
                        <div class="card-title">11.2 Защита титула чемпиона</div>
                        <span class="badge-penalty info">Регламент</span>
                    </div>
                    <div class="card-desc">
                        <span>🟢 <strong>H.C.L:</strong> защита титула — каждую неделю.</span>
                        <span>🟡 <strong>T.C.L:</strong> защита титула — каждые 2 недели.</span>
                        <span>🟠 <strong>O.C.L:</strong> защита титула — каждые 2,5 недели.</span>
                    </div>
                </div>

                <div class="rule-card searchable">
                    <div class="card-top">
                        <div class="card-title">11.3 Формат титульных боёв</div>
                        <span class="badge-penalty info">BO3</span>
                    </div>
                    <div class="card-desc">
                        <span>Титульные противостояния проводятся в формате <strong>BO3 — до двух побед (3 боя максимум)</strong>.</span>
                        <span>Игрок может сменить стиль только после поражения. Победитель обязан сохранять текущий стиль до своего проигрыша.</span>
                        <span>Остальные правила проведения боя остаются такими же, как в обычных боях.</span>
                    </div>
                </div>

                <div class="rule-card searchable">
                    <div class="card-top">
                        <div class="card-title">11.4 Наблюдение и фолы</div>
                        <span class="badge-penalty warn">Контроль</span>
                    </div>
                    <div class="card-desc">
                        <span>За титульным боем наблюдает один рефери высшей категории (<strong>главный рефери</strong>) либо <strong>два рефери обычной категории</strong>.</span>
                        <span>После каждого боя (не каждого раунда) счётчик фолов <strong>аннулируется</strong>.</span>
                        <span>В зависимости от количества и характера фолов рефери вправе изменить итог конкретного боя.</span>
                    </div>
                </div>

                <div class="rule-card danger-card searchable">
                    <div class="card-top">
                        <div class="card-title">11.5 Право на вызов чемпиона</div>
                        <span class="badge-penalty danger">ТОП-5</span>
                    </div>
                    <div class="card-desc">
                        <span>Бросить вызов действующему чемпиону могут только бойцы из <strong>Топ-5 рейтинга</strong> своего дивизиона.</span>
                        <span><strong>Топ-1</strong> получает эксклюзивную привилегию: чемпион обязан принять его вызов безоговорочно.</span>
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

        const themeClasses = ['theme-boxing', 'theme-relax', 'theme-rgb', 'theme-night', 'theme-champion', 'theme-custom'];

        function setTheme(themeName, element) {
            themeClasses.forEach(t => document.body.classList.remove(t));
            document.body.classList.add(themeName);
            document.querySelectorAll('.style-btn').forEach(btn => btn.classList.remove('active'));
            if (element) element.classList.add('active');
            localStorage.setItem('ocl_theme', themeName);
        }

        function normalizeHex(value) {
            value = String(value || '').trim();
            if (!value.startsWith('#')) value = '#' + value;
            return /^#[0-9a-fA-F]{6}$/.test(value) ? value.toLowerCase() : null;
        }
        function hexRgb(hex) {
            const n = parseInt(hex.slice(1), 16);
            return { r:(n>>16)&255, g:(n>>8)&255, b:n&255 };
        }
        function applyCustomHex(hex) {
            const rgb = hexRgb(hex);
            document.body.style.setProperty('--gold', hex);
            document.body.style.setProperty('--gold-glow', `rgba(${rgb.r},${rgb.g},${rgb.b},.48)`);
            document.body.style.setProperty('--border-grid', `rgba(${rgb.r},${rgb.g},${rgb.b},.20)`);
            document.body.style.setProperty('--accent-2', hex);
            document.body.style.setProperty('--accent-3', hex);
            document.body.style.setProperty('--gold-r', rgb.r);
            document.body.style.setProperty('--gold-g', rgb.g);
            document.body.style.setProperty('--gold-b', rgb.b);
            themeClasses.forEach(t => document.body.classList.remove(t));
            document.body.classList.add('theme-custom');
            document.querySelectorAll('.style-btn').forEach(btn => btn.classList.remove('active'));
            document.getElementById('customColor').value = hex;
            document.getElementById('customHex').value = hex;
            localStorage.setItem('ocl_custom_color', hex);
            localStorage.setItem('ocl_theme', 'theme-custom');
        }
        function syncCustomColor(value) { document.getElementById('customHex').value = value; }
        function syncCustomHex(value) {
            const hex = normalizeHex(value);
            if (hex) document.getElementById('customColor').value = hex;
        }
        function applyCustomColor() {
            const hex = normalizeHex(document.getElementById('customHex').value);
            if (hex) applyCustomHex(hex);
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
            const savedCustom = localStorage.getItem('ocl_custom_color');
            if (savedCustom && normalizeHex(savedCustom)) {
                applyCustomHex(normalizeHex(savedCustom));
            }
            const savedTheme = localStorage.getItem('ocl_theme');
            if (savedTheme && savedTheme !== 'theme-custom' && themeClasses.includes(savedTheme)) {
                themeClasses.forEach(t => document.body.classList.remove(t));
                document.body.classList.add(savedTheme);
                document.querySelectorAll('.style-btn').forEach(btn => btn.classList.remove('active'));
                const activeStyle = document.querySelector(`[onclick*="'${savedTheme}'"]`);
                if (activeStyle) activeStyle.classList.add('active');
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

        const revealTargets = document.querySelectorAll('.section-header, .promo-banner, .status-badge, .rule-card, .ban-box, footer');
        const revealObserver = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('visible');
                    revealObserver.unobserve(entry.target);
                }
            });
        }, { threshold: .08 });
        revealTargets.forEach(el => { el.classList.add('reveal-ready'); revealObserver.observe(el); });
    </script>
</body>
</html>
