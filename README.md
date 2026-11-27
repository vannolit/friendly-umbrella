<!DOCTYPE html>
<html lang="lo" class="h-full bg-slate-900">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#1e3a8a">
    <title>ລະບົບບໍລິຫານພະນັກງານ - ລັດວິສາຫະກິດ (ECI)</title>

    <!-- Favicon / App Icon (Dynamic via JS) -->
    <link id="appFavicon" rel="icon" type="image/svg+xml" href="">
    <link id="appleTouchIcon" rel="apple-touch-icon" href="">

    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- QR Code Generator & Scanner -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/jsqr@1.4.0/dist/jsQR.js"></script>
    <!-- FontAwesome Icons -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/js/all.min.js"></script>
    <!-- Google Font (Noto Sans Lao) -->
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+Lao:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    
    <!-- Firebase SDK (Compat) -->
    <script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-database-compat.js"></script>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: { lao: ['"Noto Sans Lao"', 'sans-serif'] },
                    colors: { laoBlue: '#1e3a8a', laoGold: '#f59e0b' },
                    keyframes: {
                        pulseGlow: {
                            '0%, 100%': { transform: 'scale(1)', boxShadow: '0 0 25px rgba(245, 158, 11, 0.35)' },
                            '50%': { transform: 'scale(1.04)', boxShadow: '0 0 42px rgba(245, 158, 11, 0.7)' },
                        },
                        loadBar: { '0%': { width: '0%' }, '55%': { width: '72%' }, '100%': { width: '100%' } },
                        scanLine: { '0%, 100%': { top: '12%' }, '50%': { top: '84%' } }
                    },
                    animation: {
                        pulseGlow: 'pulseGlow 2.2s ease-in-out infinite',
                        loadBar: 'loadBar 1.8s cubic-bezier(0.4, 0, 0.2, 1) forwards',
                        scanLine: 'scanLine 2.4s ease-in-out infinite',
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Noto Sans Lao', sans-serif;
            background: radial-gradient(1200px 600px at 10% -10%, #2563eb 0%, transparent 60%),
                        radial-gradient(900px 500px at 100% 110%, #4338ca 0%, transparent 55%), #0f1f5c !important;
            -webkit-font-smoothing: antialiased;
            letter-spacing: .005em;
            padding: 0 !important;
            display: block !important;
            -webkit-text-size-adjust: 100%;
        }
        * { scrollbar-width: thin; scrollbar-color: #cbd5e1 transparent; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }

        /* ===================== RESPONSIVE: MOBILE / TABLET / DESKTOP ===================== */
        :root { --safe-t: env(safe-area-inset-top, 0px); --safe-b: env(safe-area-inset-bottom, 0px); }
        html, body { height: 100%; }
        #appShell {
            width: 100%; max-width: none; height: 100vh; height: 100dvh; margin: 0 auto;
            border: 0; border-radius: 0;
            background: linear-gradient(180deg, #f8fafc 0%, #eef2f9 100%);
        }
        #appShell .fake-icons { display: none; }
        #statusBar { padding: calc(var(--safe-t) + 6px) 16px 6px !important; background: #0b1b4d !important; }
        #appHeader {
            background: linear-gradient(135deg, #1e3a8a 0%, #1d4ed8 60%, #4338ca 100%) !important;
            box-shadow: 0 8px 24px -12px rgba(15, 23, 42, .6);
        }
        #adminToggleBtn { box-shadow: 0 6px 16px -4px rgba(245, 158, 11, .55); }
        #appMain { padding-bottom: calc(96px + var(--safe-b)) !important; }
        #bottomNav { padding-bottom: calc(10px + var(--safe-b)); }
        #adminTabBar { position: sticky; top: 0; z-index: 30; }
        #adminTabBar button { min-height: 54px; }
        #adminTabBar button i { font-size: 17px; }
        #adminTabBar button span:not(#leaveBadge) { font-size: 10px; line-height: 1.1; text-align: center; }
        #leaveBadge:not(.hidden) { display: flex; }
        /* ---- Employee side: floating bottom nav + launcher tabs ---- */
        #bottomNav { left: 10px !important; right: 10px !important; bottom: calc(8px + var(--safe-b)) !important; padding: 8px 6px !important; border: 0 !important; border-radius: 28px; background: rgba(255, 255, 255, .94) !important; -webkit-backdrop-filter: blur(14px); backdrop-filter: blur(14px); box-shadow: 0 18px 40px -12px rgba(15, 23, 42, .5), 0 0 0 1px rgba(148, 163, 184, .2); }
        #bottomNav button { flex: 1; gap: 4px; color: #64748b !important; position: relative; }
        #bottomNav button > svg, #bottomNav button > i { width: 40px; height: 40px; border-radius: 15px; background: var(--g); color: #fff; opacity: .9; box-shadow: 0 8px 14px -8px rgba(15, 23, 42, .6); transition: transform .2s, box-shadow .2s, opacity .2s; }
        #bottomNav button > svg { padding: 11px; }
        #bottomNav button > i { display: flex; align-items: center; justify-content: center; font-size: 16px; }
        #bottomNav button span { font-size: 10px !important; }
        #bottomNav button.text-blue-900 { color: #1e3a8a !important; }
        #bottomNav button.text-blue-900 > svg, #bottomNav button.text-blue-900 > i { opacity: 1; transform: translateY(-5px) scale(1.1); box-shadow: 0 0 0 3px #fff, 0 0 0 5px #1e3a8a, 0 12px 18px -8px rgba(30, 58, 138, .75); }
        #bottomNav button.text-blue-900 span { margin-top: -3px; }
        #bottomNav button.text-amber-600 { color: #b45309 !important; }
        #bottomNav button:nth-child(1) { --g: linear-gradient(135deg, #2563eb,#4f46e5); }
        #bottomNav button:nth-child(2) { --g: linear-gradient(135deg, #0d9488,#059669); }
        #bottomNav button:nth-child(3) { --g: linear-gradient(135deg, #e11d48,#f43f5e); }
        #bottomNav button:nth-child(4) { --g: linear-gradient(135deg, #f59e0b,#ea580c); }
        #subTabCheckin, #subTabDuty, #subTabLeave { animation: fadeUp .28s ease both; }
        @keyframes fadeUp { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: none; } }
        @media (max-width: 640px) {
            #dashTabs { gap: 6px 4px !important; padding: 12px 6px 10px !important; border-radius: 26px !important; background: linear-gradient(135deg, #ffffff 0%, #eef2ff 100%) !important; box-shadow: 0 14px 30px -14px rgba(30, 58, 138, .45); }
            #dashTabs button { flex-direction: column !important; gap: 6px !important; padding: 4px 0 !important; background: transparent !important; box-shadow: none !important; color: #475569 !important; font-size: 10.5px; font-weight: 600 !important; }
            #dashTabs button > svg, #dashTabs button > i { width: 46px; height: 46px; border-radius: 16px; background: var(--g); color: #fff; box-shadow: 0 8px 16px -8px rgba(15, 23, 42, .55); transition: transform .18s, box-shadow .18s; }
            #dashTabs button > svg { padding: 12px; }
            #dashTabs button > i { display: flex; align-items: center; justify-content: center; font-size: 19px; }
            #dashTabs button.text-blue-900 { color: #1e3a8a !important; font-weight: 800 !important; }
            #dashTabs button.text-blue-900 > svg, #dashTabs button.text-blue-900 > i { transform: scale(1.1); box-shadow: 0 0 0 3px #fff, 0 0 0 5px #1e3a8a, 0 10px 18px -8px rgba(30, 58, 138, .7); }
            #dashTabs button.text-blue-900::after { content: ''; width: 18px; height: 3px; border-radius: 3px; background: #f59e0b; }
            #dashTabs button:nth-child(1) { --g: linear-gradient(135deg, #2563eb,#4f46e5); }
            #dashTabs button:nth-child(2) { --g: linear-gradient(135deg, #0d9488,#059669); }
            #dashTabs button:nth-child(3) { --g: linear-gradient(135deg, #e11d48,#f43f5e); }
        }
        /* ---- Mobile admin menu: app-launcher style ---- */
        @media (max-width: 640px) {
            #adminTabBar { gap: 6px 4px; padding: 12px 6px 10px; border-radius: 26px; background: linear-gradient(135deg, #ffffff 0%, #eef2ff 100%); box-shadow: 0 14px 30px -14px rgba(30, 58, 138, .45); }
            #adminTabBar button { background: transparent !important; box-shadow: none !important; color: #475569 !important; min-height: 0; padding: 4px 0 !important; gap: 6px !important; font-weight: 600 !important; }
            #adminTabBar button > svg, #adminTabBar button > i { width: 46px; height: 46px; border-radius: 16px; background: var(--g); color: #fff; box-shadow: 0 8px 16px -8px rgba(15, 23, 42, .55); transition: transform .18s, box-shadow .18s; }
            #adminTabBar button > svg { padding: 12px; }
            #adminTabBar button > i { display: flex; align-items: center; justify-content: center; font-size: 19px; }
            #adminTabBar button span:not(#leaveBadge) { font-size: 10.5px; }
            #adminTabBar button.bg-blue-900 { color: #1e3a8a !important; font-weight: 800 !important; }
            #adminTabBar button.bg-blue-900 > svg, #adminTabBar button.bg-blue-900 > i { transform: scale(1.1); box-shadow: 0 0 0 3px #fff, 0 0 0 5px #1e3a8a, 0 10px 18px -8px rgba(30, 58, 138, .7); }
            #adminTabBar button.bg-blue-900::after { content: ''; width: 18px; height: 3px; border-radius: 3px; background: #f59e0b; margin-top: -1px; }
            #leaveBadge { top: -2px !important; right: 6px !important; box-shadow: 0 0 0 2px #fff; }
            #adminTabBar button:nth-child(1) { --g: linear-gradient(135deg, #2563eb,#4f46e5); }
            #adminTabBar button:nth-child(2) { --g: linear-gradient(135deg, #f59e0b,#ea580c); }
            #adminTabBar button:nth-child(3) { --g: linear-gradient(135deg, #0d9488,#059669); }
            #adminTabBar button:nth-child(4) { --g: linear-gradient(135deg, #e11d48,#f43f5e); }
            #adminTabBar button:nth-child(5) { --g: linear-gradient(135deg, #7c3aed,#a855f7); }
            #adminTabBar button:nth-child(6) { --g: linear-gradient(135deg, #0284c7,#06b6d4); }
            #adminTabBar button:nth-child(7) { --g: linear-gradient(135deg, #d97706,#facc15); }
            #adminTabBar button:nth-child(8) { --g: linear-gradient(135deg, #475569,#64748b); }
        }

        /* Cards, Inputs & Interactive Polish */
        #appMain .bg-white.rounded-3xl, #appMain .bg-white.rounded-2xl {
            border: 1px solid rgba(148, 163, 184, .24) !important;
            border-radius: 22px !important;
            box-shadow: 0 4px 20px -8px rgba(30, 58, 138, .14) !important;
        }
        #appMain .bg-gradient-to-br, #appMain .bg-gradient-to-r {
            border-radius: 22px !important;
            box-shadow: 0 18px 36px -18px rgba(30, 58, 138, .65) !important;
        }
        #appShell input:not([type=checkbox]):not([type=radio]), #appShell select, #appShell textarea {
            border-radius: 14px !important;
            transition: border-color .15s, box-shadow .15s, background .15s;
        }
        #appShell input:focus, #appShell select:focus, #appShell textarea:focus {
            outline: none;
            border-color: #2563eb !important;
            background: #fff !important;
            box-shadow: 0 0 0 4px rgba(37, 99, 235, .15);
        }
        #appShell button { transition: transform .12s, box-shadow .15s, background-color .15s, filter .15s; }
        #appShell button:active { transform: scale(.97); }
        #appShell button:hover { filter: brightness(1.03); }

        /* Shift Radio Card Active State */
        .shift-card {
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .shift-card:has(input:checked) {
            transform: translateY(-2px);
            box-shadow: 0 8px 18px -6px rgba(15, 23, 42, 0.18);
        }

        @keyframes fadeUp { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: none; } }
        #appMain > section:not(.hidden), #secDashboard > div:not(.hidden), #secAdmin > div:not(.hidden) { animation: fadeUp .28s ease both; }

        /* Phone: full screen, 16px inputs so iOS does not zoom */
        @media (max-width: 767px) {
            input:not([type=checkbox]):not([type=radio]), select, textarea { font-size: 16px !important; }
        }
        /* Floating bottom nav on phone/tablet */
        @media (max-width: 1023px) {
            #bottomNav:not(.hidden) {
                left: 12px; right: 12px; bottom: calc(10px + var(--safe-b));
                padding: 8px 6px; border: 1px solid rgba(148, 163, 184, .3); border-radius: 24px;
                background: rgba(255, 255, 255, .92); backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px);
                box-shadow: 0 16px 40px -12px rgba(15, 23, 42, .4);
            }
            #bottomNav button { padding: 6px 12px; border-radius: 16px; }
            #bottomNav button.text-blue-900 { background: #eff6ff; }
        }
        /* Tablet: single column centred */
        @media (min-width: 768px) and (max-width: 1023px) {
            #appShell { max-width: 760px; box-shadow: 0 0 60px rgba(0,0,0,.35); }
        }
        /* Desktop: sidebar + wide balanced workspace */
        @media (min-width: 1024px) {
            #appShell { max-width: none; display: grid; grid-template-columns: 260px minmax(0, 1fr); grid-template-rows: auto auto minmax(0, 1fr); }
            #statusBar, #appHeader { grid-column: 1 / -1; }
            #statusBar { padding: 5px 32px !important; }
            #appHeader { padding: 12px 32px !important; }
            #appMain { grid-column: 2; grid-row: 3; padding: 24px 32px !important; min-height: 0; }
            #appShell:has(#bottomNav.hidden) #appMain { grid-column: 1 / -1; }
            #bottomNav:not(.hidden) {
                grid-column: 1; grid-row: 3; position: static;
                display: flex; flex-direction: column; justify-content: flex-start; align-items: stretch;
                gap: 6px; padding: 16px; border-top: 0; border-right: 1px solid #e2e8f0;
                background: rgba(255, 255, 255, .85);
            }
            #bottomNav button { flex-direction: row; gap: 12px; padding: 12px 14px; border-radius: 14px; justify-content: flex-start; }
            #bottomNav button:hover { background: #eff6ff; }
            #bottomNav button.text-blue-900 { background: #eff6ff; box-shadow: inset 3px 0 0 #1d4ed8; }
            #bottomNav button span { margin: 0 !important; font-size: 14px !important; }
            #bottomNav button i { width: 22px; text-align: center; }
            #appMain > section { max-width: 1320px; margin-left: auto; margin-right: auto; }
            #secLogin, #secAdminPin { max-width: 480px; }
            #toastBanner { max-width: 520px; margin: 0 auto; }
            /* bigger text for desktop reading */
            #appShell .text-\[9px\] { font-size: 11px !important; }
            #appShell .text-\[10px\] { font-size: 12px !important; }
            #appShell .text-\[11px\] { font-size: 13px !important; }
            #appShell .text-xs { font-size: 14px !important; }
            #appShell .text-sm { font-size: 16px !important; }
            #appShell .text-base { font-size: 17px !important; }
            /* employee dashboard: profile left, content right */
            #secDashboard:not(.hidden) { display: grid; grid-template-columns: 360px minmax(0, 1fr); gap: 22px; align-items: start; }
            #secDashboard > * { margin: 0 !important; }
            #secDashboard > :nth-child(1), #secDashboard > #postLoginAdBanner { grid-column: 1; }
            #dashTabs { display: none; }
            #subTabCheckin, #subTabDuty, #subTabLeave { grid-column: 2; grid-row: 1 / span 3; max-width: 920px; }
            /* admin workspace */
            #adminTabBar button { flex-direction: row !important; justify-content: center; gap: 8px !important; padding: 12px 8px !important; }
            #adminTabEmployees:not(.hidden), #adminTabDuties:not(.hidden) { display: grid; grid-template-columns: minmax(320px, 420px) minmax(0, 1fr); gap: 16px; align-items: start; }
            #adminTabScanner:not(.hidden), #adminTabAds:not(.hidden), #adminTabBackup:not(.hidden) { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 16px; align-items: start; }
            #adminTabEmployees > *, #adminTabDuties > *, #adminTabScanner > *, #adminTabAds > *, #adminTabBackup > * { margin: 0 !important; }
            #adminEmpList, #adminLogList, #adminLeaveList, #adminDutyList, #adminAdList { max-height: calc(100vh - 360px) !important; min-height: 160px; }
        }

        /* ===================== ADMIN BALANCE ===================== */
        #secAdmin > .grid-cols-4 > div { display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 64px; }
        @media (max-width: 479px) {
            #secAdmin > .grid-cols-4 { grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 8px; }
            #secAdmin > .grid-cols-4 > div { min-height: 72px; }
            #secAdmin > .grid-cols-4 strong { font-size: 22px !important; }
        }
        @media (min-width: 1024px) {
            #secAdmin { max-width: 1200px !important; }
            #secAdmin > .grid-cols-4 { gap: 16px; }
            #secAdmin > .grid-cols-4 > div { padding: 18px !important; min-height: 96px; border-radius: 20px; }
            #secAdmin > .grid-cols-4 strong { font-size: 30px !important; line-height: 1.1; }
            #adminTabBar { padding: 6px; gap: 6px; }
            #adminTabBar button { min-width: 0; }
            #adminTabScanner:not(.hidden), #adminTabAds:not(.hidden), #adminTabBackup:not(.hidden) { align-items: stretch; }
            #adminTabAds > :last-child { grid-column: 1 / -1; }
            #adminTabEmployees > form { position: sticky; top: 0; }
            #adminLogList, #adminLeaveList, #adminDutyList, #adminAdList { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 10px; align-content: start; }
            #adminLogList > *, #adminLeaveList > *, #adminDutyList > *, #adminAdList > * { margin: 0 !important; }
            #adminTabLogs:not(.hidden), #adminTabLeaves:not(.hidden) { display: block; }
            #adminTabLogs > * + *, #adminTabLeaves > * + * { margin-top: 12px; }
        }
    
        /* ===== Balance pass: phone / tablet / desktop ===== */
        #adminTabBar button:nth-child(1) { --g: linear-gradient(135deg, #2563eb,#4f46e5); }
        #adminTabBar button:nth-child(2) { --g: linear-gradient(135deg, #f59e0b,#ea580c); }
        #adminTabBar button:nth-child(3) { --g: linear-gradient(135deg, #0d9488,#059669); }
        #adminTabBar button:nth-child(4) { --g: linear-gradient(135deg, #e11d48,#f43f5e); }
        #adminTabBar button:nth-child(5) { --g: linear-gradient(135deg, #7c3aed,#a855f7); }
        #adminTabBar button:nth-child(6) { --g: linear-gradient(135deg, #0284c7,#06b6d4); }
        #adminTabBar button:nth-child(7) { --g: linear-gradient(135deg, #d97706,#facc15); }
        #adminTabBar button:nth-child(8) { --g: linear-gradient(135deg, #475569,#64748b); }
        #adminTabBar button span:not(#leaveBadge) { max-width: 100%; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        /* Phone + tablet: evenly spaced floating nav, no lifted tab */
        @media (max-width: 1023px) {
            #bottomNav:not(.hidden) { justify-content: space-around !important; align-items: stretch; gap: 2px; }
            #bottomNav button { flex: 1 1 0 !important; min-width: 0; padding: 2px 0 !important; border-radius: 16px; align-items: center; justify-content: flex-start; background: transparent !important; }
            #bottomNav button.text-blue-900 { background: transparent !important; }
            #bottomNav button.text-blue-900 > svg, #bottomNav button.text-blue-900 > i { transform: scale(1.06) !important; }
            #bottomNav button.text-blue-900 span { margin-top: 0 !important; }
            #bottomNav button span { white-space: nowrap; }
            #appMain { padding-left: 14px; padding-right: 14px; }
        }
        /* Tablet + desktop admin menu: one balanced row of 8 */
        @media (min-width: 641px) {
            #adminTabBar { grid-template-columns: repeat(8, minmax(0, 1fr)); gap: 6px; padding: 8px; }
            #adminTabBar button { min-height: 62px; justify-content: center; }
            #adminTabBar button > svg, #adminTabBar button > i { width: 32px; height: 32px; border-radius: 11px; background: var(--g); color: #fff; box-shadow: 0 6px 12px -7px rgba(15, 23, 42, .6); }
            #adminTabBar button > svg { padding: 8px; }
            #adminTabBar button > i { display: flex; align-items: center; justify-content: center; font-size: 15px; }
            #adminTabBar button.bg-blue-900 { background: #eff6ff !important; color: #1e3a8a !important; box-shadow: inset 0 -3px 0 #1d4ed8 !important; }
            #adminTabBar button.bg-blue-900 > svg, #adminTabBar button.bg-blue-900 > i { box-shadow: 0 0 0 2px #fff, 0 0 0 4px #1d4ed8; }
        }
        /* Desktop: sidebar nav stays a neat list; admin tabs inline */
        @media (min-width: 1024px) {
            #bottomNav:not(.hidden) { left: auto !important; right: auto !important; bottom: auto !important; padding: 16px !important; border: 0 !important; border-right: 1px solid #e2e8f0 !important; border-radius: 0 !important; background: rgba(255, 255, 255, .85) !important; box-shadow: none !important; -webkit-backdrop-filter: none; backdrop-filter: none; justify-content: flex-start !important; gap: 6px; }
            #bottomNav button { flex: none !important; flex-direction: row !important; justify-content: flex-start !important; padding: 10px 14px !important; gap: 12px !important; border-radius: 14px !important; }
            #bottomNav button > svg, #bottomNav button > i { width: 36px; height: 36px; border-radius: 12px; opacity: 1; transform: none !important; box-shadow: 0 6px 12px -7px rgba(15, 23, 42, .6) !important; }
            #bottomNav button > svg { padding: 9px; }
            #bottomNav button > i { font-size: 16px; }
            #bottomNav button.text-blue-900 { background: #eff6ff !important; box-shadow: inset 3px 0 0 #1d4ed8 !important; }
            #bottomNav button.text-blue-900 > svg, #bottomNav button.text-blue-900 > i { box-shadow: 0 6px 12px -6px rgba(30, 58, 138, .7) !important; }
            #adminTabBar button { flex-direction: row; gap: 9px; min-height: 52px; }
            #adminTabBar button span:not(#leaveBadge) { font-size: 13px !important; font-weight: 700; }
            #leaveBadge { top: 4px !important; right: 6px !important; }
        }

        /* ===== Compact employee menus (phone/tablet) ===== */
        @media (max-width: 1023px) {
            #bottomNav:not(.hidden) { left: 14px !important; right: 14px !important; bottom: calc(6px + var(--safe-b)) !important; padding: 5px 4px !important; border-radius: 22px !important; box-shadow: 0 10px 24px -10px rgba(15, 23, 42, .45), 0 0 0 1px rgba(148, 163, 184, .2) !important; }
            #bottomNav button { gap: 2px !important; padding: 1px 0 !important; }
            #bottomNav button > svg, #bottomNav button > i { width: 30px !important; height: 30px !important; border-radius: 11px !important; box-shadow: 0 5px 9px -6px rgba(15, 23, 42, .55) !important; }
            #bottomNav button > svg { padding: 8px !important; }
            #bottomNav button > i { font-size: 13px !important; }
            #bottomNav button span { font-size: 9.5px !important; line-height: 1.1; }
            #bottomNav button.text-blue-900 > svg, #bottomNav button.text-blue-900 > i { transform: scale(1.04) !important; box-shadow: 0 0 0 2px #fff, 0 0 0 3.5px #1e3a8a !important; }
            #appMain { padding-bottom: calc(68px + var(--safe-b)) !important; }
        }
        @media (max-width: 640px) {
            #dashTabs { gap: 4px !important; padding: 8px 4px 6px !important; border-radius: 20px !important; }
            #dashTabs button { gap: 3px !important; padding: 2px 0 !important; font-size: 10px !important; }
            #dashTabs button > svg, #dashTabs button > i { width: 34px !important; height: 34px !important; border-radius: 12px !important; box-shadow: 0 5px 10px -6px rgba(15, 23, 42, .5) !important; }
            #dashTabs button > svg { padding: 9px !important; }
            #dashTabs button > i { font-size: 15px !important; }
            #dashTabs button.text-blue-900 > svg, #dashTabs button.text-blue-900 > i { transform: scale(1.05) !important; box-shadow: 0 0 0 2px #fff, 0 0 0 3.5px #1e3a8a !important; }
            #dashTabs button.text-blue-900::after { width: 14px; height: 2px; }
        }
</style>
</head>
<body class="text-slate-800 flex justify-center items-center min-h-screen p-0 sm:p-4">

    <div id="appShell" class="w-full bg-slate-50 shadow-2xl flex flex-col overflow-hidden relative">
        
        <!-- SPLASH SCREEN -->
        <div id="splashScreen" class="absolute inset-0 z-[100] bg-gradient-to-b from-blue-950 via-blue-900 to-slate-950 flex flex-col items-center justify-between p-8 text-white transition-opacity duration-700">
            <div class="w-full pt-4 flex justify-center">
                <span class="text-[10px] tracking-widest uppercase text-blue-300/80 font-semibold">ECI STAFF & DUTY SYSTEM (ONLINE)</span>
            </div>
            <div class="flex flex-col items-center text-center my-auto">
                <div class="relative mb-6">
                    <div class="absolute -inset-3 rounded-full border border-amber-400/40 animate-ping opacity-30"></div>
                    <div class="w-40 h-40 rounded-full bg-white p-1 border-4 border-amber-400 shadow-2xl animate-pulseGlow overflow-hidden flex items-center justify-center">
                        <img data-company-logo src="" alt="ໂລໂກ້ ລັດວິສາຫະກິດກໍ່ສ້າງ ແລະ ຕິດຕັ້ງໄຟຟ້າ" class="w-full h-full object-cover rounded-full">
                    </div>
                </div>
                <h1 class="text-lg font-extrabold tracking-wide text-white leading-snug">ລັດວິສາຫະກິດກໍ່ສ້າງ ແລະ ຕິດຕັ້ງໄຟຟ້າ</h1>
                <p class="text-xs font-bold text-amber-400 tracking-wider mt-1">ELECTRICAL CONSTRUCTION & INSTALLATION (ECI)</p>
                <div class="mt-3 px-3.5 py-1 rounded-full bg-white/10 border border-white/15 text-[11px] text-blue-100">
                </div>
            </div>
            <div class="w-full max-w-[240px] flex flex-col items-center space-y-2.5 pb-6">
                <div class="w-full h-1.5 bg-blue-950/90 rounded-full overflow-hidden border border-white/10 p-[1px]">
                    <div id="splashProgressBar" class="h-full bg-gradient-to-r from-amber-400 via-yellow-300 to-amber-500 rounded-full animate-loadBar"></div>
                </div>
                <p id="splashStatusText" class="text-[11px] text-blue-200 font-medium flex items-center gap-1.5">
                    <i class="fa-solid fa-circle-notch fa-spin text-amber-400"></i> ກຳລັງເຊື່ອມຕໍ່ Server...
                </p>
            </div>
        </div>

        <!-- Toast Banner -->
        <div id="toastBanner" class="hidden absolute top-24 left-4 right-4 z-[95] bg-slate-900/95 text-white px-4 py-3 rounded-2xl shadow-xl border border-amber-400/40 text-xs flex items-center justify-between transition-all">
            <div class="flex items-center space-x-2.5">
                <i id="toastIcon" class="fa-solid fa-circle-check text-emerald-400 text-base"></i>
                <span id="toastMsg" class="leading-snug font-medium">ແຈ້ງເຕືອນ</span>
            </div>
            <button onclick="hideToast()" class="text-slate-400 hover:text-white ml-2"><i class="fa-solid fa-xmark"></i></button>
        </div>

        <!-- Post-Login Popup Advertisement Modal -->
        <div id="adPopupModal" class="hidden absolute inset-0 z-[90] bg-slate-950/80 backdrop-blur-sm flex items-center justify-center p-5">
            <div class="bg-white w-full rounded-3xl overflow-hidden shadow-2xl border-2 border-amber-400 animate-[fadeIn_0.25s_ease-out]">
                <div class="relative">
                    <img id="adPopupImg" src="" alt="ໂຄສະນາ" class="w-full h-48 object-cover bg-blue-950">
                    <span id="adPopupBadge" class="absolute top-3 left-3 px-3 py-1 rounded-full bg-amber-400 text-blue-950 text-[10px] font-extrabold shadow">📢 ປະກາດສຳຄັນ</span>
                    <button onclick="closeAdPopup()" class="absolute top-3 right-3 w-8 h-8 rounded-full bg-black/60 hover:bg-black/80 text-white text-xs flex items-center justify-center">
                        <i class="fa-solid fa-xmark"></i>
                    </button>
                </div>
                <div class="p-5 space-y-2.5">
                    <h3 id="adPopupTitle" class="text-base font-extrabold text-slate-900 leading-snug">ຫົວຂໍ້ໂຄສະນາ</h3>
                    <p id="adPopupDesc" class="text-xs text-slate-600 leading-relaxed">ລາຍລະອຽດປະກາດ</p>
                    <div class="pt-2 flex gap-2">
                        <a id="adPopupLink" href="#" target="_blank" class="hidden flex-1 py-2.5 bg-amber-400 hover:bg-amber-500 text-blue-950 rounded-xl text-xs font-extrabold text-center shadow">
                            <i class="fa-solid fa-arrow-up-right-from-square mr-1"></i> ເບິ່ງລາຍລະອຽດ
                        </a>
                        <button onclick="closeAdPopup()" class="flex-1 py-2.5 bg-blue-900 hover:bg-blue-800 text-white rounded-xl text-xs font-bold">
                            ຮັບຊາບ / ປິດໜ້າຕ່າງ
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Digital Employee ID Badge Modal -->
        <div id="badgeModal" class="hidden absolute inset-0 z-[85] bg-slate-950/80 backdrop-blur-sm flex items-center justify-center p-5">
            <div class="bg-white w-full rounded-3xl overflow-hidden shadow-2xl border-2 border-amber-400">
                <div class="bg-gradient-to-r from-blue-950 via-blue-900 to-indigo-900 text-white p-4 text-center relative">
                    <button onclick="closeBadgeModal()" class="absolute top-3 right-3 w-8 h-8 rounded-full bg-white/15 hover:bg-white/25 text-white text-xs flex items-center justify-center"><i class="fa-solid fa-xmark"></i></button>
                    <div class="flex items-center justify-center gap-2">
                        <img data-company-logo src="" alt="ECI" class="w-7 h-7 rounded-full object-cover border border-amber-400 bg-white">
                        <p class="text-[10px] text-amber-300 font-bold tracking-wider">ລັດວິສາຫະກິດກໍ່ສ້າງ ແລະ ຕິດຕັ້ງໄຟຟ້າ (ECI)</p>
                    </div>
                    <h3 class="text-sm font-extrabold mt-0.5">ບັດປະຈຳຕົວພະນັກງານດິຈິຕອນ</h3>
                </div>
                <div class="p-5 flex flex-col items-center text-center space-y-3">
                    <div class="w-16 h-16 rounded-2xl bg-amber-400 text-blue-950 font-extrabold text-2xl flex items-center justify-center shadow-md border-2 border-blue-900" id="badgeAvatar">ສ</div>
                    <div>
                        <h4 id="badgeName" class="text-base font-extrabold text-slate-900">ຊື່ພະນັກງານ</h4>
                        <span id="badgeCode" class="inline-block mt-0.5 px-3 py-0.5 rounded-full bg-blue-900 text-amber-300 text-xs font-bold">ECI-135</span>
                    </div>
                    <div class="w-full bg-slate-50 rounded-2xl p-3 text-left text-xs space-y-1.5 border border-slate-200/80">
                        <div class="flex justify-between"><span class="text-slate-500">ພະແນກ:</span><span id="badgeDept" class="font-bold text-slate-800 text-right">-</span></div>
                        <div class="flex justify-between"><span class="text-slate-500">ຕຳແໜ່ງ:</span><span id="badgePos" class="font-bold text-slate-800 text-right">-</span></div>
                        <div class="flex justify-between"><span class="text-slate-500">ເບີໂທ:</span><span id="badgePhone" class="font-bold text-slate-800 text-right">-</span></div>
                    </div>
                    <div class="p-3 bg-white rounded-2xl border-2 border-dashed border-blue-200 flex flex-col items-center">
                        <div id="personalQrBox" class="p-1 bg-white"></div>
                        <p class="text-[10px] text-slate-500 mt-2">ສະແກນ QR ນີ້ທີ່ຈຸດກວດກາ Admin ເພື່ອບັນທຶກເວລາ</p>
                    </div>
                    <button onclick="closeBadgeModal()" class="w-full py-2.5 bg-blue-900 hover:bg-blue-800 text-white rounded-xl text-xs font-bold">ປິດບັດພະນັກງານ</button>
                </div>
            </div>
        </div>

        <!-- Scan Success Confirmation Modal (ສະແກນ QR จุดตรวจ Admin ສຳເລັດ) -->
        <div id="scanResultModal" class="hidden absolute inset-0 z-[92] bg-slate-950/85 backdrop-blur-sm flex items-center justify-center p-5">
            <div class="bg-white w-full rounded-3xl overflow-hidden shadow-2xl border-2 border-emerald-400">
                <div id="scanResultHeader" class="bg-gradient-to-r from-emerald-600 to-teal-700 text-white p-5 text-center relative">
                    <div id="scanResultIcon" class="w-16 h-16 mx-auto mb-2 rounded-full bg-white text-emerald-600 flex items-center justify-center text-3xl shadow-lg animate-bounce">
                        <i class="fa-solid fa-circle-check"></i>
                    </div>
                    <h3 id="scanResultTitle" class="text-base font-extrabold">ສະແກນ QR ຈຸດກວດສຳເລັດ!</h3>
                    <p id="scanResultSub" class="text-[11px] text-emerald-100 mt-0.5">ລະບົບໄດ້ບັນທຶກເວລາ ແລະ ພິກັດ GPS ແລ້ວ</p>
                </div>
                <div class="p-5 space-y-3 text-xs">
                    <div id="scanSuccessDetails" class="bg-slate-50 rounded-2xl p-3.5 space-y-2 border border-slate-200/80">
                        <div class="flex justify-between items-center"><span class="text-slate-500">ພະນັກງານ:</span><strong id="resEmpName" class="text-slate-900 text-right">-</strong></div>
                        <div class="flex justify-between items-center"><span class="text-slate-500">ຊ່ວງເວລາ:</span><span id="resShiftBadge" class="px-2 py-0.5 rounded-full text-[10px] font-bold bg-emerald-100 text-emerald-800">-</span></div>
                        <div class="flex justify-between items-center"><span class="text-slate-500">ເວລາບັນທຶກ:</span><strong id="resTime" class="text-blue-900 font-extrabold">-</strong></div>
                        <div class="flex justify-between items-start gap-2"><span class="text-slate-500 shrink-0">ຈຸດສະແກນ Admin:</span><strong id="resCheckpoint" class="text-emerald-700 text-right">-</strong></div>
                        <div class="flex justify-between items-center"><span class="text-slate-500">ທີ່ຢູ່ GPS:</span><span id="resGps" class="text-slate-700 font-medium text-[11px]">-</span></div>
                        <div id="resPhotoWrap" class="hidden pt-2 border-t border-slate-200 flex items-center gap-2.5">
                            <img id="resPhotoImg" src="" alt="ຮູບຖ່າຍຢືນຢັນ" class="w-14 h-14 rounded-xl object-cover border border-slate-300">
                            <span class="text-[10px] text-slate-500">ຮູບຖ່າຍຢືນຢັນຕົວຕົນຕອນສະແກນ QR</span>
                        </div>
                    </div>
                    <div id="scanErrorDetails" class="hidden bg-rose-50 border border-rose-200 rounded-2xl p-3.5 text-center space-y-1.5">
                        <p class="text-rose-800 font-bold text-xs">QR Code ນີ້ບໍ່ແມ່ນປ້າຍຈຸດສະແກນຂອງ Admin!</p>
                        <p id="scanErrorRaw" class="text-[10px] text-rose-600 break-all bg-white p-2 rounded-xl border border-rose-100">ລະຫັດທີ່ສະແກນ: -</p>
                        <p class="text-[11px] text-slate-600 pt-1">ກະລຸນານຳກ້ອງໄປສະແກນປ້າຍ QR ທີ່ Admin ປິ້ນຕິດໄວ້ຢູ່ຈຸດກວດກາຂອງບໍລິສັດ.</p>
                    </div>
                    <div class="flex gap-2">
                        <button id="scanRetryBtn" type="button" onclick="closeScanResultModal(true)" class="hidden flex-1 py-2.5 bg-amber-400 hover:bg-amber-500 text-blue-950 rounded-xl text-xs font-extrabold">
                            <i class="fa-solid fa-camera mr-1"></i> ສະແກນໃໝ່ອີກຄັ້ງ
                        </button>
                        <button type="button" onclick="closeScanResultModal(false)" class="flex-1 py-2.5 bg-blue-900 hover:bg-blue-800 text-white rounded-xl text-xs font-bold">
                            ຕົກລົງ / ປິດໜ້າຕ່າງ
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Printable Admin QR Poster Preview Modal -->
        <div id="printPosterModal" class="hidden absolute inset-0 z-[94] bg-slate-950/85 backdrop-blur-sm flex items-center justify-center p-4">
            <div class="bg-white w-full rounded-3xl overflow-hidden shadow-2xl border-2 border-blue-900 flex flex-col max-h-[92%]">
                <div class="bg-blue-950 text-white px-4 py-3 flex items-center justify-between">
                    <span class="text-xs font-bold text-amber-300"><i class="fa-solid fa-print mr-1.5"></i> ຕົວຢ່າງປ້າຍ QR ສຳລັບປິ້ນຕິດຈຸດສະແກນ</span>
                    <button type="button" onclick="closePrintPosterModal()" class="w-7 h-7 rounded-full bg-white/15 hover:bg-white/25 text-white text-xs flex items-center justify-center"><i class="fa-solid fa-xmark"></i></button>
                </div>
                <div id="printablePosterArea" class="p-5 overflow-y-auto text-center space-y-3 bg-white">
                    <div class="border-4 border-blue-900 rounded-3xl p-4 space-y-2.5 bg-gradient-to-b from-blue-50/60 to-white">
                        <div class="flex items-center justify-center gap-2.5">
                            <img data-company-logo src="" alt="ECI" class="w-12 h-12 rounded-full object-cover border-2 border-amber-400 bg-white">
                            <div class="text-left">
                                <p class="text-xs font-extrabold text-blue-950 leading-tight">ລັດວິສາຫະກິດກໍ່ສ້າງ ແລະ ຕິດຕັ້ງໄຟຟ້າ</p>
                                <p class="text-[10px] font-bold text-amber-600">ELECTRICAL CONSTRUCTION & INSTALLATION (ECI)</p>
                            </div>
                        </div>
                        <div class="bg-blue-900 text-white py-1.5 px-3 rounded-xl">
                            <p class="text-[10px] text-amber-300 font-bold">ຈຸດສະແກນບັນທຶກເວລາເຮັດວຽກປະຈຳວັນ</p>
                            <h4 id="posterPointTitle" class="text-sm font-extrabold mt-0.5">ECI-35-GATE-01</h4>
                        </div>
                        <div class="py-2 flex flex-col items-center">
                            <div id="posterQrCanvas" class="p-3 bg-white rounded-2xl border-2 border-blue-900 shadow-md inline-block"></div>
                            <p id="posterPayloadCode" class="text-[10px] font-mono text-slate-500 mt-1.5">ECI-CP|ECI-35-GATE-01</p>
                        </div>
                        <div id="posterShiftGrid" class="grid grid-cols-2 gap-1.5 text-[10px] font-bold text-slate-800 bg-amber-50 p-2.5 rounded-2xl border border-amber-200">
                            <div>🌅 ເຂົ້າເຊົ້າ: 08:00</div>
                            <div>☀️ ອອກສວຍ: 12:00</div>
                            <div>🌤️ ເຂົ້າສວຍ: 12:30</div>
                            <div>🌆 ອອກແລງ: 17:00</div>
                        </div>
                        <p class="text-[10px] text-slate-500">ກະລຸນາເປີດແອັບພະນັກງານ ECI ແລ້ວສະແກນ QR ນີ້ທຸກຄັ້ງທີ່ເຂົ້າ-ອອກວຽກ</p>
                    </div>
                </div>
                <div class="p-3 bg-slate-100 border-t border-slate-200 flex gap-2">
                    <button type="button" onclick="triggerNativePrintPoster()" class="flex-1 py-2.5 bg-blue-900 hover:bg-blue-800 text-white rounded-xl text-xs font-extrabold shadow">
                        <i class="fa-solid fa-print mr-1.5"></i> ສັ່ງປິ້ນປ້າຍ QR ທັນທີ (Print)
                    </button>
                    <button type="button" onclick="downloadPosterPng()" class="py-2.5 px-3.5 bg-emerald-600 hover:bg-emerald-700 text-white rounded-xl text-xs font-bold shadow">
                        <i class="fa-solid fa-download mr-1"></i> ບັນທຶກຮູບ
                    </button>
                </div>
            </div>
        </div>

        <!-- Status Bar -->
        <div id="statusBar" class="bg-blue-950 text-white px-6 pt-3 pb-2 flex justify-between items-center text-xs font-semibold z-50">
            <span id="currentTime">08:00</span>
            <span id="cloudStatusDot" class="text-[10px] text-emerald-400 flex items-center gap-1"><i class="fa-solid fa-cloud"></i> Online</span>
            <div class="fake-icons flex items-center space-x-2 text-[10px]">
                <i class="fa-solid fa-signal"></i>
                <i class="fa-solid fa-wifi"></i>
                <i class="fa-solid fa-battery-full text-xs"></i>
            </div>
        </div>

        <!-- Header -->
        <header id="appHeader" class="bg-blue-900 text-white p-3.5 shadow-lg flex items-center justify-between z-40 relative">
            <div class="flex items-center space-x-2.5">
                <div class="w-10 h-10 rounded-full border-2 border-amber-400 bg-white overflow-hidden shadow-sm shrink-0 flex items-center justify-center">
                    <img data-company-logo src="" alt="ECI Logo" class="w-full h-full object-cover">
                </div>
                <div>
                    <h1 class="text-xs font-bold leading-tight">ລັດວິສາຫະກິດກໍ່ສ້າງ ແລະ ຕິດຕັ້ງໄຟຟ້າ</h1>
                    <p id="headerSubtext" class="text-[10px] text-amber-300 font-medium">ລະບົບບໍລິຫານພະນັກງານ ແລະ ເວນຍາມ</p>
                </div>
            </div>
            <div class="flex items-center space-x-1.5">
                <button onclick="replaySplash()" title="ໂຫຼດໃໝ່" class="bg-white/15 hover:bg-white/25 text-white px-2 py-1.5 rounded-xl text-[10px] font-semibold flex items-center transition active:scale-95">
                    <i class="fa-solid fa-rotate-right"></i>
                </button>
                <button onclick="toggleAdminView()" id="adminToggleBtn" class="bg-amber-400 hover:bg-amber-500 text-blue-950 font-bold px-2.5 py-1.5 rounded-xl text-xs flex items-center shadow-md transition-all active:scale-95">
                    <i class="fa-solid fa-user-shield mr-1"></i> Admin
                </button>
            </div>
        </header>

        <!-- Main Content -->
        <main id="appMain" class="flex-1 overflow-y-auto no-scrollbar p-4 pb-24 relative">
            
            <!-- SECTION 1: LOGIN / REGISTER -->
            <section id="secLogin" class="space-y-4">
                <div class="bg-white p-5 rounded-3xl shadow-sm border border-slate-100 text-center">
                    <div class="w-20 h-20 rounded-full bg-white p-0.5 mx-auto mb-2.5 border-2 border-amber-400 shadow-md overflow-hidden flex items-center justify-center">
                        <img data-company-logo src="" alt="ECI" class="w-full h-full object-cover rounded-full">
                    </div>
                    <h2 id="authTitle" class="text-base font-bold text-slate-800">ເຂົ້າສູ່ລະບົບພະນັກງານ</h2>
                    <p id="authSub" class="text-xs text-slate-500 mt-1">ປ້ອນລະຫັດພະນັກງານ ແລະ ລະຫັດຜ່ານເພື່ອເຂົ້າໃຊ້ງານ</p>
                </div>

                <div class="bg-slate-200/70 p-1 rounded-2xl grid grid-cols-2 gap-1 text-xs font-bold">
                    <button type="button" id="tabLogin" onclick="authTab('login')" class="py-2.5 rounded-xl transition">ເຂົ້າສູ່ລະບົບ</button>
                    <button type="button" id="tabSignup" onclick="authTab('signup')" class="py-2.5 rounded-xl transition">ສະໝັກສະມາຊິກ</button>
                </div>

                <form id="loginForm" onsubmit="handleLogin(event)" class="bg-white p-5 rounded-3xl shadow-sm border border-slate-100 space-y-4"></form>
                <form id="signupForm" onsubmit="handleSignup(event)" class="hidden bg-white p-5 rounded-3xl shadow-sm border border-slate-100 space-y-3.5"></form>

                <div class="bg-blue-50/90 border border-blue-200 rounded-2xl p-3.5 flex items-center justify-between">
                    <div class="text-[11px] text-blue-950">
                        <span class="font-bold block">ທົດລອງເຂົ້າລະບົບດ່ວນ:</span>
                        <span class="text-slate-600">ID: ECI-xxx | ຜ່ານ: xxxxxx</span>
                    </div>
                </div>
            </section>

            <!-- SECTION 2: EMPLOYEE DASHBOARD -->
            <section id="secDashboard" class="hidden space-y-3.5">
                <!-- Profile & 4-Period Summary Card -->
                <div class="bg-gradient-to-br from-blue-950 via-blue-900 to-indigo-900 text-white p-4 rounded-3xl shadow-lg relative overflow-hidden border border-white/10">
                    <div class="flex items-center justify-between gap-2">
                        <div class="flex items-center space-x-3 min-w-0">
                            <div class="w-12 h-12 bg-amber-400 text-blue-950 rounded-2xl flex items-center justify-center font-extrabold text-lg shadow-md border-2 border-white/20 shrink-0">
                                <span id="avatarChar">ສ</span>
                            </div>
                            <div class="min-w-0">
                                <h3 id="profileName" class="font-bold text-sm truncate">ຊື່ພະນັກງານ</h3>
                                <p id="profileCode" class="text-[11px] text-blue-200 font-medium">ID: <ECI-13X></ECI-13X></p>
                                <p id="profileDept" class="text-[10px] text-amber-300 truncate">ພະແນກ: -</p>
                                <p id="profilePhone" class="text-[10px] text-slate-300"><i class="fa-solid fa-phone mr-1 text-amber-400"></i> 020 XXXXXXXX</p>
                            </div>
                        </div>
                        <div class="flex flex-col gap-1.5 shrink-0">
                            <button onclick="openBadgeModal()" class="bg-amber-400 hover:bg-amber-500 text-blue-950 px-2.5 py-1.5 rounded-xl text-[10px] font-extrabold flex items-center justify-center gap-1 shadow transition">
                                <i class="fa-solid fa-id-badge"></i> ບັດ QR
                            </button>
                            <button onclick="logout()" class="bg-white/10 hover:bg-white/20 px-2.5 py-1.5 rounded-xl text-white text-[10px] font-semibold flex items-center justify-center gap-1 transition">
                                <i class="fa-solid fa-power-off text-rose-400"></i> ອອກ
                            </button>
                        </div>
                    </div>

                    <!-- 4-Period Attendance Summary Grid -->
                    <div class="mt-3 pt-3 border-t border-white/15">
                        <div class="flex items-center justify-between text-[10px] mb-1.5">
                            <span class="text-blue-200 font-semibold"><i class="fa-regular fa-clock mr-1 text-amber-400"></i>ສະຫຼຸບເວລາສະແກນ 4 ຊ່ວງມື້ນີ້</span>
                            <strong id="todayStatusBadge" class="text-amber-300 font-bold">ຍັງບໍ່ໄດ້ສະແກນ</strong>
                        </div>
                        <div class="grid grid-cols-4 gap-1.5 text-center text-[10px]">
                            <div class="bg-white/10 rounded-xl p-1.5 border border-white/10">
                                <span id="lblSummary1" class="text-blue-200 block text-[9px]">ເຊົ້າ (08:00)</span>
                                <strong id="todaySlot1" class="text-emerald-300 text-xs font-extrabold">--:--</strong>
                            </div>
                            <div class="bg-white/10 rounded-xl p-1.5 border border-white/10">
                                <span id="lblSummary2" class="text-blue-200 block text-[9px]">ພັກສວຍ (12:00)</span>
                                <strong id="todaySlot2" class="text-amber-300 text-xs font-extrabold">--:--</strong>
                            </div>
                            <div class="bg-white/10 rounded-xl p-1.5 border border-white/10">
                                <span id="lblSummary3" class="text-blue-200 block text-[9px]">ເຂົ້າສວຍ (12:30)</span>
                                <strong id="todaySlot3" class="text-sky-300 text-xs font-extrabold">--:--</strong>
                            </div>
                            <div class="bg-white/10 rounded-xl p-1.5 border border-white/10">
                                <span id="lblSummary4" class="text-blue-200 block text-[9px]">ແລງ (17:00)</span>
                                <strong id="todaySlot4" class="text-rose-300 text-xs font-extrabold">--:--</strong>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- POST-LOGIN ADVERTISEMENT / ANNOUNCEMENT BANNER CAROUSEL -->
                <div id="postLoginAdBanner" class="hidden bg-white rounded-3xl shadow-sm border border-amber-200 overflow-hidden relative">
                    <div class="relative h-36 bg-gradient-to-r from-blue-950 to-indigo-900">
                        <img id="adBannerImg" src="" alt="ໂຄສະນາ ECI" class="w-full h-full object-cover opacity-90">
                        <div class="absolute inset-0 bg-gradient-to-t from-slate-950/90 via-slate-950/35 to-transparent"></div>
                        <span id="adBannerBadge" class="absolute top-2.5 left-3 px-2.5 py-0.5 rounded-full bg-amber-400 text-blue-950 text-[10px] font-extrabold shadow">📢 ໂຄສະນາ</span>
                        <div class="absolute top-2.5 right-3 flex items-center gap-1">
                            <button type="button" onclick="prevAdSlide()" class="w-6 h-6 rounded-full bg-black/50 text-white text-[10px] flex items-center justify-center hover:bg-black/75"><i class="fa-solid fa-chevron-left"></i></button>
                            <button type="button" onclick="nextAdSlide()" class="w-6 h-6 rounded-full bg-black/50 text-white text-[10px] flex items-center justify-center hover:bg-black/75"><i class="fa-solid fa-chevron-right"></i></button>
                        </div>
                        <div class="absolute bottom-2.5 left-3 right-3 text-white">
                            <h4 id="adBannerTitle" class="text-xs font-extrabold leading-snug line-clamp-1">ຫົວຂໍ້ໂຄສະນາ</h4>
                            <p id="adBannerDesc" class="text-[10px] text-slate-200 line-clamp-2 mt-0.5">ລາຍລະອຽດປະກາດ</p>
                        </div>
                    </div>
                    <div class="px-3.5 py-2 bg-amber-50/70 flex items-center justify-between">
                        <div id="adBannerDots" class="flex items-center gap-1.5"></div>
                        <button type="button" onclick="openCurrentAdDetail()" class="text-[10px] font-bold text-blue-900 hover:underline flex items-center gap-1">
                            ອ່ານເພີ່ມເຕີມ <i class="fa-solid fa-circle-chevron-right"></i>
                        </button>
                    </div>
                </div>

                <!-- Navigation Tabs -->
                <div id="dashTabs" class="grid grid-cols-3 gap-1.5 p-1 bg-slate-200/80 rounded-2xl text-[11px] font-bold">
                    <button onclick="switchTab('checkin')" id="tabBtnCheckin" class="py-2.5 rounded-xl transition shadow bg-white text-blue-900 flex items-center justify-center gap-1"><i class="fa-solid fa-qrcode"></i> ສະແກນເວລາ</button>
                    <button onclick="switchTab('duty')" id="tabBtnDuty" class="py-2.5 rounded-xl transition text-slate-600 hover:text-slate-900 flex items-center justify-center gap-1"><i class="fa-solid fa-calendar-days"></i> ຕາຕະລາງເວນ</button>
                    <button onclick="switchTab('leave')" id="tabBtnLeave" class="py-2.5 rounded-xl transition text-slate-600 hover:text-slate-900 flex items-center justify-center gap-1"><i class="fa-solid fa-file-pen"></i> ຂໍລາພັກ</button>
                </div>

                <!-- SUB-TAB 1: 4-PERIOD QR CLOCK-IN / OUT -->
                <div id="subTabCheckin" class="space-y-4">
                    <div class="bg-white p-5 rounded-3xl shadow-sm border border-slate-100">
                        <div class="flex items-center justify-between mb-3.5">
                            <h3 class="text-sm font-extrabold text-slate-800 flex items-center gap-2">
                                <span class="w-8 h-8 rounded-xl bg-blue-100 text-blue-700 flex items-center justify-center">
                                    <i class="fa-solid fa-camera"></i>
                                </span>
                                <span>ເລືອກຊ່ວງເວລາສະແກນ QR</span>
                            </h3>
                            <span class="text-[11px] bg-emerald-50 text-emerald-700 border border-emerald-200 px-3 py-1 rounded-full font-bold">
                                ● 4 ຊ່ວງເວລາ
                            </span>
                        </div>
                        
                        <!-- 4 Shift Radio Cards (Touch-Friendly & Balanced) -->
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-2.5 mb-3.5">
                            <label class="shift-card flex items-center gap-3 p-3.5 bg-emerald-50/70 border-2 border-emerald-200 has-[:checked]:border-emerald-600 has-[:checked]:bg-emerald-100/90 rounded-2xl cursor-pointer">
                                <input type="radio" name="checkType" value="IN_MORNING" checked class="w-4 h-4 text-emerald-600 accent-emerald-600 shrink-0">
                                <div class="leading-tight">
                                    <span class="text-xs font-extrabold text-emerald-950 block">🌅 ເຂົ້າວຽກເຊົ້າ</span>
                                    <span id="btnTime1" class="text-[11px] font-bold text-emerald-700">ເວລາ 08:00</span>
                                </div>
                            </label>
                            <label class="shift-card flex items-center gap-3 p-3.5 bg-amber-50/70 border-2 border-amber-200 has-[:checked]:border-amber-600 has-[:checked]:bg-amber-100/90 rounded-2xl cursor-pointer">
                                <input type="radio" name="checkType" value="OUT_NOON" class="w-4 h-4 text-amber-600 accent-amber-600 shrink-0">
                                <div class="leading-tight">
                                    <span class="text-xs font-extrabold text-amber-950 block">☀️ ພັກ/ອອກສວຍ</span>
                                    <span id="btnTime2" class="text-[11px] font-bold text-amber-700">ເວລາ 12:00</span>
                                </div>
                            </label>
                            <label class="shift-card flex items-center gap-3 p-3.5 bg-sky-50/70 border-2 border-sky-200 has-[:checked]:border-sky-600 has-[:checked]:bg-sky-100/90 rounded-2xl cursor-pointer">
                                <input type="radio" name="checkType" value="IN_NOON" class="w-4 h-4 text-sky-600 accent-sky-600 shrink-0">
                                <div class="leading-tight">
                                    <span class="text-xs font-extrabold text-sky-950 block">🌤️ ເຂົ້າວຽກສວຍ</span>
                                    <span id="btnTime3" class="text-[11px] font-bold text-sky-700">ເວລາ 12:30</span>
                                </div>
                            </label>
                            <label class="shift-card flex items-center gap-3 p-3.5 bg-rose-50/70 border-2 border-rose-200 has-[:checked]:border-rose-600 has-[:checked]:bg-rose-100/90 rounded-2xl cursor-pointer">
                                <input type="radio" name="checkType" value="OUT_EVENING" class="w-4 h-4 text-rose-600 accent-rose-600 shrink-0">
                                <div class="leading-tight">
                                    <span class="text-xs font-extrabold text-rose-950 block">🌆 ອອກວຽກແລງ</span>
                                    <span id="btnTime4" class="text-[11px] font-bold text-rose-700">ເວລາ 17:00</span>
                                </div>
                            </label>
                        </div>

                        <!-- Offsite Work Toggle -->
                        <label class="flex items-center justify-between gap-2.5 text-xs bg-purple-50/90 hover:bg-purple-100/70 border border-purple-200 rounded-2xl p-3 mb-3.5 font-bold text-purple-950 cursor-pointer transition">
                            <div class="flex items-center gap-2.5">
                                <input type="checkbox" id="offsiteChk" onchange="stopCam()" class="w-4 h-4 rounded accent-purple-600">
                                <span> ວຽກນອກສະຖານທີ່ (QR · ຖ່າຍຮູບ + ພິກັດ GPS)</span>
                            </div>
                            <span class="text-[10px] bg-purple-200/80 text-purple-900 px-2 py-0.5 rounded-lg shrink-0">Offsite</span>
                        </label>

                        <div id="offsiteNoteBox" class="hidden mb-3.5">
                            <input type="text" id="offsiteNote" class="w-full p-3 bg-slate-50 border border-purple-200 rounded-2xl text-xs" placeholder="ລະບຸສະຖານທີ່/ໜ້າວຽກນອກສະຖານທີ່">
                        </div>

                        <!-- QR Scanner Viewfinder -->
                        <div id="qrIdle" onclick="startCam()" class="relative cursor-pointer overflow-hidden rounded-2xl bg-gradient-to-b from-slate-900 via-blue-950 to-slate-900 h-52 mb-3.5 flex flex-col items-center justify-center border-2 border-blue-900/40 group">
                            <div class="relative w-36 h-36 flex items-center justify-center group-hover:scale-105 transition-transform">
                                <span class="absolute top-0 left-0 w-8 h-8 border-t-4 border-l-4 border-amber-400 rounded-tl-xl"></span>
                                <span class="absolute top-0 right-0 w-8 h-8 border-t-4 border-r-4 border-amber-400 rounded-tr-xl"></span>
                                <span class="absolute bottom-0 left-0 w-8 h-8 border-b-4 border-l-4 border-amber-400 rounded-bl-xl"></span>
                                <span class="absolute bottom-0 right-0 w-8 h-8 border-b-4 border-r-4 border-amber-400 rounded-br-xl"></span>
                                <i class="fa-solid fa-qrcode text-5xl text-white/35"></i>
                                <div class="absolute left-2 right-2 h-0.5 bg-amber-400 shadow-[0_0_10px_#facc15] animate-scanLine"></div>
                            </div>
                            <span class="text-xs text-slate-200 font-semibold mt-3">ແຕະເພື່ອເປີດກ້ອງ ແລະ ສະແກນ QR ຈຸດເຂົ້າ-ອອກວຽກ</span>
                        </div>

                        <div id="camBox" class="hidden relative overflow-hidden rounded-2xl bg-black mb-3.5">
                            <video id="camVid" playsinline muted class="w-full h-56 object-cover"></video>
                            <div id="camFrame" class="absolute inset-0 m-auto w-44 h-44 border-2 border-amber-400 rounded-2xl pointer-events-none"></div>
                            <button type="button" onclick="stopCam()" class="absolute top-2.5 right-2.5 bg-black/70 text-white w-8 h-8 rounded-full text-xs flex items-center justify-center"><i class="fa-solid fa-xmark"></i></button>
                        </div>

                        <!-- GPS & Quick Share -->
                        <div class="text-xs text-slate-600 mb-3.5 bg-slate-50 rounded-2xl p-3 space-y-2.5 border border-slate-200/70">
                            <div class="flex items-center gap-1.5 font-medium">
                                <i class="fa-solid fa-location-dot text-rose-500"></i>
                                <span id="gpsTxt">GPS: ຈະບັນທຶກພິກັດອັດຕະໂນມັດຕອນເຂົ້າ-ອອກວຽກ</span>
                            </div>
                            <div class="grid grid-cols-3 gap-2 text-[11px] font-bold">
                                <button type="button" onclick="sendLocation('share')" class="bg-blue-900 hover:bg-blue-800 text-white rounded-xl py-2.5 flex items-center justify-center gap-1.5 shadow-sm"><i class="fa-solid fa-paper-plane"></i> Google Map</button>
                                <button type="button" onclick="sendLocation('wa')" class="bg-emerald-600 hover:bg-emerald-700 text-white rounded-xl py-2.5 flex items-center justify-center gap-1.5 shadow-sm"><i class="fa-brands fa-whatsapp"></i> WhatsApp</button>
                                <button type="button" onclick="sendLocation('copy')" class="bg-white hover:bg-slate-100 text-slate-800 border border-slate-200 rounded-xl py-2.5 flex items-center justify-center gap-1.5"><i class="fa-solid fa-copy"></i> ສຳເນົາລິ້ງ</button>
                            </div>
                        </div>

                        <!-- Full-Width Primary CTA Button -->
                        <button id="camBtn" onclick="startCam()" class="w-full bg-gradient-to-r from-blue-900 to-blue-700 hover:from-blue-800 hover:to-blue-600 text-white font-extrabold text-sm py-3.5 rounded-2xl shadow-lg shadow-blue-900/25 flex items-center justify-center gap-2 transition active:scale-[0.98]">
                            <i class="fa-solid fa-camera text-amber-300"></i>
                            <span id="camBtnTxt">ສະແກນ QR + ຖ່າຍຮູບ</span>
                        </button>
                        <div class="mt-2.5 grid grid-cols-2 gap-2">
                            <label class="cursor-pointer text-center bg-slate-100 hover:bg-slate-200 text-slate-800 text-[11px] font-bold py-2.5 rounded-xl"><i class="fa-solid fa-image mr-1"></i>ສະແກນຈາກຮູບ<input type="file" accept="image/*" capture="environment" class="hidden" onchange="scanFromImage(this)"></label>
                            <button type="button" onclick="$id('manualBox').classList.toggle('hidden')" class="bg-slate-100 hover:bg-slate-200 text-slate-800 text-[11px] font-bold py-2.5 rounded-xl"><i class="fa-solid fa-keyboard mr-1"></i>ໃສ່ລະຫັດຈຸດ</button>
                        </div>
                        <div id="manualBox" class="hidden mt-2 flex gap-2"><input id="manualCp" placeholder="ເຊັ່ນ ECI-35-GATE-01" class="flex-1 p-2.5 bg-slate-50 border border-slate-200 rounded-xl text-xs"><button type="button" onclick="manualCp()" class="bg-blue-900 text-white px-4 rounded-xl text-xs font-bold">ຢືນຢັນ</button></div>
                    </div>

                    <div class="bg-white p-5 rounded-3xl shadow-sm border border-slate-100">
                        <h3 class="text-xs font-extrabold text-slate-800 mb-3 flex items-center justify-between">
                            <span><i class="fa-solid fa-clock-rotate-left text-blue-600 mr-1.5"></i> ປະຫວັດເຂົ້າ-ອອກວຽກຂອງທ່ານ</span>
                            <span id="myLogCount" class="text-[11px] bg-slate-100 text-slate-600 px-2.5 py-0.5 rounded-full font-bold">0 ລາຍການ</span>
                        </h3>
                        <div id="userLogHistory" class="space-y-2.5 max-h-64 overflow-y-auto text-xs"></div>
                    </div>
                </div>

                <!-- SUB-TAB 2: DUTY ROSTER -->
                <div id="subTabDuty" class="hidden space-y-4">
                    <div class="bg-white p-5 rounded-3xl shadow-sm border border-slate-100">
                        <h3 class="text-xs font-bold text-slate-800 mb-3 flex items-center justify-between">
                            <span><i class="fa-solid fa-calendar-week text-amber-500 mr-1.5"></i> ຕາຕະລາງເວນຍາມ ແລະ ວຽກນອກສະຖານທີ່</span>
                            <span class="text-[10px] text-emerald-600 font-bold">● Real-time</span>
                        </h3>
                        <div class="grid grid-cols-4 gap-1 mb-3">
                            <button onclick="filterMyShift('all')" id="filterAll" class="py-1.5 bg-blue-900 text-white rounded-xl text-[10px] font-bold shadow">ທັງໝົດ</button>
                            <button onclick="filterMyShift('ເຊົ້າ')" id="filterMorning" class="py-1.5 bg-slate-100 text-slate-700 rounded-xl text-[10px] font-medium">ເຊົ້າ</button>
                            <button onclick="filterMyShift('ບ່າຍ')" id="filterAfternoon" class="py-1.5 bg-slate-100 text-slate-700 rounded-xl text-[10px] font-medium">ບ່າຍ</button>
                            <button onclick="filterMyShift('ເດິກ')" id="filterNight" class="py-1.5 bg-slate-100 text-slate-700 rounded-xl text-[10px] font-medium">ເດິກ</button>
                        </div>
                        <div id="myDutyList" class="space-y-2.5 max-h-80 overflow-y-auto"></div>
                    </div>
                </div>

                <!-- SUB-TAB 3: LEAVE REQUEST -->
                <div id="subTabLeave" class="hidden space-y-4">
                    <form onsubmit="handleLeaveSubmit(event)" class="bg-white p-5 rounded-3xl shadow-sm border border-slate-100 space-y-3">
                        <h3 class="text-xs font-bold text-slate-800 border-b pb-2 flex items-center">
                            <i class="fa-solid fa-envelope-open-text text-amber-500 mr-2"></i> ແບບຟອມຂໍລາພັກ
                        </h3>
                        <div>
                            <label class="block text-[11px] font-semibold text-slate-700 mb-1">ປະເພດການລາ <span class="text-red-500">*</span></label>
                            <select id="leaveType" required class="w-full p-2.5 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                                <option value="🌴 ຂໍລາພັກປະຈຳປີ">🌴 ຂໍລາພັກປະຈຳປີ (Annual Leave)</option>
                                <option value="🤒 ຂໍລາປ່ວຍ">🤒 ຂໍລາປ່ວຍ (Sick Leave)</option>
                                <option value="💼 ຂໍລາກິດ">💼 ຂໍລາກິດ (Personal Leave)</option>
                            </select>
                        </div>
                        <div class="grid grid-cols-2 gap-2">
                            <div>
                                <label class="block text-[11px] font-semibold text-slate-700 mb-1">ຕັ້ງແຕ່ວັນທີ <span class="text-red-500">*</span></label>
                                <input type="date" id="leaveStart" required class="w-full p-2.5 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                            </div>
                            <div>
                                <label class="block text-[11px] font-semibold text-slate-700 mb-1">ເຖິງວັນທີ <span class="text-red-500">*</span></label>
                                <input type="date" id="leaveEnd" required class="w-full p-2.5 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                            </div>
                        </div>
                        <div>
                            <label class="block text-[11px] font-semibold text-slate-700 mb-1">ສາເຫດການລາ <span class="text-red-500">*</span></label>
                            <textarea id="leaveReason" rows="2" required class="w-full p-2.5 bg-slate-50 border border-slate-200 rounded-xl text-xs" placeholder="ລະບຸສາເຫດ..."></textarea>
                        </div>
                        <button type="submit" class="w-full bg-amber-400 hover:bg-amber-500 text-blue-950 font-bold py-3 rounded-xl text-xs shadow-md transition">
                            <i class="fa-solid fa-paper-plane mr-1"></i> ສົ່ງໃບລາໃຫ້ຝ່າຍຈັດຕັ້ງ (Admin)
                        </button>
                    </form>
                    <div class="bg-white p-4 rounded-3xl shadow-sm border border-slate-100 space-y-2">
                        <h3 class="text-xs font-bold text-slate-800">ສະຖານະໃບຂໍລາພັກຂອງທ່ານ</h3>
                        <div id="myLeaveList" class="space-y-2"></div>
                    </div>
                </div>
            </section>

            <!-- SECTION 3: ADMIN PIN -->
            <section id="secAdminPin" class="hidden space-y-4">
                <div class="bg-gradient-to-r from-blue-950 to-slate-900 text-white p-5 rounded-3xl shadow-md text-center border border-white/10">
                    <div class="w-14 h-14 mx-auto mb-2 rounded-full bg-amber-400 text-blue-950 flex items-center justify-center text-2xl shadow-lg">
                        <i class="fa-solid fa-user-shield"></i>
                    </div>
                    <h2 class="text-sm font-bold text-amber-400">ລະບົບຈັດການຫຼັງບ້ານ / Admin</h2>
                    <p class="text-[11px] text-slate-300 mt-1">ປ້ອນລະຫັດ PIN 4 ຫຼັກ (ຄ່າເລີ່ມຕົ້ນ: <strong>xxxx</strong>)</p>
                </div>
                <div class="bg-white p-5 rounded-3xl shadow-sm border border-slate-100">
                    <div id="pinDots" class="flex justify-center gap-3 mb-4"></div>
                    <div id="pinPad" class="grid grid-cols-3 gap-2.5 max-w-[240px] mx-auto"></div>
                    <button onclick="toggleAdminView()" class="mt-4 w-full text-[11px] text-slate-500 font-semibold py-2 hover:text-slate-800">ຍົກເລີກ</button>
                </div>
            </section>

            <!-- SECTION 4: ADMIN DASHBOARD -->
            <section id="secAdmin" class="hidden space-y-3.5">
                <div class="bg-gradient-to-r from-blue-950 to-slate-900 text-white p-4 rounded-3xl shadow-md flex items-center justify-between border border-white/10">
                    <div>
                        <h2 class="text-xs font-bold text-amber-400 flex items-center"><i class="fa-solid fa-desktop mr-1.5"></i> ລະບົບຈັດການຫຼັງບ້ານ (Admin)</h2>
                        <p id="syncBadge" class="text-[10px] mt-1 text-emerald-300">● ເຊື່ອມຕໍ່ Cloud Real-time ແລ້ວ</p>
                    </div>
                    <button onclick="toggleAdminView()" class="bg-white/10 text-white px-2.5 py-1.5 rounded-xl text-[10px] font-semibold hover:bg-white/20"><i class="fa-solid fa-xmark mr-1"></i> ປິດ</button>
                </div>

                <div class="grid grid-cols-4 gap-1.5 text-center">
                    <div class="bg-white p-2.5 rounded-2xl border border-slate-100 shadow-sm"><span class="text-[10px] text-slate-500 block">ພະນັກງານ</span><strong id="statTotalEmp" class="text-sm font-extrabold text-blue-900">0</strong></div>
                    <div class="bg-white p-2.5 rounded-2xl border border-slate-100 shadow-sm"><span class="text-[10px] text-slate-500 block">ສະແກນມື້ນີ້</span><strong id="statTodayIn" class="text-sm font-extrabold text-emerald-600">0</strong></div>
                    <div class="bg-white p-2.5 rounded-2xl border border-slate-100 shadow-sm"><span class="text-[10px] text-slate-500 block">ໂຄສະນາ</span><strong id="statActiveAds" class="text-sm font-extrabold text-purple-600">0</strong></div>
                    <div class="bg-white p-2.5 rounded-2xl border border-slate-100 shadow-sm"><span class="text-[10px] text-slate-500 block">ລໍຖ້າລາພັກ</span><strong id="statPendingLeaves" class="text-sm font-extrabold text-amber-600">0</strong></div>
                </div>

                <!-- 6 Admin Navigation Tabs -->
                <div id="adminTabBar" class="grid grid-cols-4 gap-1 bg-white rounded-2xl p-1.5 shadow-md border border-slate-100">
                    <button onclick="switchAdminTab('employees')" id="adminTabEmployeesBtn" class="py-2.5 rounded-xl font-bold bg-blue-900 text-white flex flex-col items-center gap-1 shadow relative"><i class="fa-solid fa-users"></i><span>ພະນັກງານ</span></button>
                    <button onclick="switchAdminTab('finance')" id="adminTabFinanceBtn" class="py-2.5 rounded-xl font-semibold text-slate-600 bg-slate-50 flex flex-col items-center gap-1 relative"><i class="fa-solid fa-coins"></i><span>ການເງິນ</span></button>
                    <button onclick="switchAdminTab('logs')" id="adminTabLogsBtn" class="py-2.5 rounded-xl font-semibold text-slate-600 bg-slate-50 flex flex-col items-center gap-1 relative"><i class="fa-solid fa-list-check"></i><span>ເຂົ້າ-ອອກ</span></button>
                    <button onclick="switchAdminTab('leaves')" id="adminTabLeavesBtn" class="py-2.5 rounded-xl font-semibold text-slate-600 bg-slate-50 flex flex-col items-center gap-1 relative"><i class="fa-solid fa-file-signature"></i><span>ລາພັກ</span><span id="leaveBadge" class="hidden absolute top-0.5 right-1 bg-rose-500 text-white rounded-full min-w-[16px] h-4 px-1 items-center justify-center font-bold" style="font-size:9px">0</span></button>
                    <button onclick="switchAdminTab('scanner')" id="adminTabScannerBtn" class="py-2.5 rounded-xl font-semibold text-slate-600 bg-slate-50 flex flex-col items-center gap-1 relative"><i class="fa-solid fa-qrcode"></i><span>ຈຸດສະແກນ</span></button>
                    <button onclick="switchAdminTab('duties')" id="adminTabDutiesBtn" class="py-2.5 rounded-xl font-semibold text-slate-600 bg-slate-50 flex flex-col items-center gap-1 relative"><i class="fa-solid fa-calendar-check"></i><span>ເວນຍາມ</span></button>
                    <button onclick="switchAdminTab('ads')" id="adminTabAdsBtn" class="py-2.5 rounded-xl font-semibold text-slate-600 bg-slate-50 flex flex-col items-center gap-1 relative"><i class="fa-solid fa-bullhorn"></i><span>ໂຄສະນາ</span></button>
                    <button onclick="switchAdminTab('backup')" id="adminTabBackupBtn" class="py-2.5 rounded-xl font-semibold text-slate-600 bg-slate-50 flex flex-col items-center gap-1 relative"><i class="fa-solid fa-cloud-arrow-down"></i><span>ສຳຮອງ</span></button>
                </div>

                <!-- ADMIN TAB 1: EMPLOYEES -->
                <div id="adminTabEmployees" class="space-y-3">
                    <form id="empForm" onsubmit="handleSaveEmployee(event)" class="bg-white p-4 rounded-3xl shadow-sm border border-slate-100 space-y-2.5">
                        <div class="flex items-center justify-between border-b pb-2">
                            <h3 id="empFormTitle" class="text-xs font-bold text-blue-950 flex items-center"><i class="fa-solid fa-user-plus text-blue-600 mr-1.5"></i> ເພີ່ມ / ບັນທຶກຂໍ້ມູນພະນັກງານໃໝ່</h3>
                            <button type="button" id="cancelEditEmpBtn" onclick="resetEmpForm()" class="hidden text-[10px] text-rose-600 font-bold underline">ຍົກເລີກແກ້ໄຂ</button>
                        </div>
                        <input type="hidden" id="editEmpOriginalCode" value="">
                        <div class="grid grid-cols-2 gap-2">
                            <div><label class="block text-[10px] font-semibold text-slate-700 mb-0.5">ລະຫັດພະນັກງານ *</label><input type="text" id="empCodeInput" required class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs uppercase" placeholder="ECI-136"></div>
                            <div><label class="block text-[10px] font-semibold text-slate-700 mb-0.5">ເບີໂທຕິດຕໍ່ *</label><input type="tel" id="empPhoneInput" required class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs" placeholder="020 XXXXXXXX"></div>
                        </div>
                        <div><label class="block text-[10px] font-semibold text-slate-700 mb-0.5">ຊື່ ແລະ ນາມສະກຸນ *</label><input type="text" id="empNameInput" required class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs" placeholder="ທ້າວ/ນາງ ຊື່ ແລະ ນາມສະກຸນ"></div>
                        <div class="grid grid-cols-2 gap-2">
                            <div>
                                <label class="block text-[10px] font-semibold text-slate-700 mb-0.5">ພະແນກ *</label>
                                <select id="empDeptInput" required class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">

                                </select>
                            </div>
                            <div><label class="block text-[10px] font-semibold text-slate-700 mb-0.5">ຕຳແໜ່ງ *</label><input type="text" id="empPosInput" required class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs" placeholder="ວິຊາການ"></div>
                        </div>
                        <div class="grid grid-cols-2 gap-2">
                            <div><label class="block text-[10px] font-semibold text-slate-700 mb-0.5">ລະຫັດຜ່ານ</label><input type="text" id="empPwInput" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs" placeholder="123456"></div>
                            <div>
                                <label class="block text-[10px] font-semibold text-slate-700 mb-0.5">ສະຖານະ</label>
                                <select id="empStatusInput" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                                    <option value="active"> ປະຈຳການ</option>
                                    <option value="inactive"> ພັກວຽກ/ຢຸດຊົ່ວຄາວ</option>
                                </select>
                            </div>
                        </div>
                        <button type="submit" class="w-full bg-blue-900 hover:bg-blue-800 text-white font-bold py-2.5 rounded-xl text-xs shadow"><i class="fa-solid fa-floppy-disk mr-1"></i> ບັນທຶກຂໍ້ມູນພະນັກງານ</button>
                    </form>

                    <div class="bg-white p-3.5 rounded-3xl shadow-sm border border-slate-100 space-y-2.5">
                        <div class="flex items-center justify-between gap-2">
                            <div class="relative flex-1">
                                <i class="fa-solid fa-magnifying-glass absolute left-3 top-2.5 text-slate-400 text-xs"></i>
                                <input type="text" id="empSearchInput" oninput="renderAdminEmployees()" class="w-full pl-8 pr-3 py-2 bg-slate-50 border border-slate-200 rounded-xl text-xs" placeholder="ຄົ້ນຫາຊື່, ລະຫັດ, ພະແນກ...">
                            </div>
                            <button onclick="exportEmployeesCSV()" class="bg-emerald-600 hover:bg-emerald-700 text-white px-3 py-2 rounded-xl text-[10px] font-bold shrink-0 shadow"><i class="fa-solid fa-file-excel mr-1"></i> Export</button>
                        </div>
                        <div id="adminEmpList" class="space-y-2 max-h-80 overflow-y-auto"></div>
                    </div>
                </div>

                <!-- ADMIN TAB 2: LOGS -->
                <div id="adminTabLogs" class="hidden space-y-2.5">
                    <div class="bg-white p-3 rounded-2xl border border-slate-100 shadow-sm space-y-2">
                        <div class="flex justify-between items-center text-xs">
                            <span class="font-bold text-slate-800">ປະຫວັດການສະແກນເຂົ້າ-ອອກວຽກ (4 ຊ່ວງ)</span>
                            <div class="flex gap-1.5">
                                <button onclick="exportLogsCSV()" class="bg-emerald-600 text-white px-2.5 py-1 rounded-lg text-[10px] font-bold"><i class="fa-solid fa-download mr-1"></i> CSV</button>
                                <button onclick="clearLogs()" class="bg-rose-50 text-rose-600 border border-rose-200 px-2 py-1 rounded-lg text-[10px] font-bold">ລ້າງປະຫວັດ</button>
                            </div>
                        </div>
                        <input type="text" id="logSearchInput" oninput="renderAdminLogs()" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs" placeholder="ຄົ້ນຫາຕາມຊື່ພະນັກງານ, ລະຫັດ ຫຼື ວັນທີ...">
                    </div>
                    <div id="adminLogList" class="space-y-2 max-h-96 overflow-y-auto"></div>
                </div>

                <!-- ADMIN TAB 3: SCANNER, QR & SCHEDULE SETTINGS -->
                <div id="adminTabScanner" class="hidden space-y-3">
                    <!-- 4-Period Shift Schedule Settings -->
                    <div class="bg-white p-4 rounded-3xl shadow-sm border border-slate-100 space-y-2.5">
                        <h3 class="text-xs font-bold text-blue-950 flex items-center justify-between">
                            <span><i class="fa-solid fa-clock text-blue-600 mr-1.5"></i> ກຳນົດເວລາສະແກນ 4 ຊ່ວງປະຈຳວັນ</span>
                            <span class="text-[10px] text-emerald-600 font-bold">ປັບແຕ່ງໄດ້</span>
                        </h3>
                        <div class="grid grid-cols-2 gap-2 text-xs">
                            <div>
                                <label class="block text-[10px] font-semibold text-slate-700 mb-0.5">1. ເຂົ້າວຽກເຊົ້າ</label>
                                <input type="time" id="cfgTime1" value="08:00" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs font-bold">
                            </div>
                            <div>
                                <label class="block text-[10px] font-semibold text-slate-700 mb-0.5">2. ພັກ/ອອກສວຍ</label>
                                <input type="time" id="cfgTime2" value="12:00" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs font-bold">
                            </div>
                            <div>
                                <label class="block text-[10px] font-semibold text-slate-700 mb-0.5">3. ເຂົ້າວຽກສວຍ</label>
                                <input type="time" id="cfgTime3" value="12:30" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs font-bold">
                            </div>
                            <div>
                                <label class="block text-[10px] font-semibold text-slate-700 mb-0.5">4. ອອກວຽກແລງ</label>
                                <input type="time" id="cfgTime4" value="17:00" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs font-bold">
                            </div>
                        </div>
                        <button type="button" onclick="saveShiftTimes()" class="w-full bg-blue-900 hover:bg-blue-800 text-white font-bold py-2 rounded-xl text-xs shadow">
                            <i class="fa-solid fa-floppy-disk mr-1"></i> ບັນທຶກຕາຕະລາງເວລາ 4 ຊ່ວງ
                        </button>
                    </div>

                    <!-- Admin Gate Scanner -->
                    <div class="bg-white p-4 rounded-3xl shadow-sm border border-slate-100 space-y-2.5">
                        <h3 class="text-xs font-bold text-blue-950 flex items-center justify-between">
                            <span><i class="fa-solid fa-camera-retro text-amber-500 mr-1.5"></i> ເຄື່ອງສະແກນບັດພະນັກງານ</span>
                            <span class="text-[10px] bg-blue-50 text-blue-800 px-2 py-0.5 rounded-full font-bold">Admin Gate</span>
                        </h3>
                        <div class="grid grid-cols-2 gap-1.5 text-[10px] font-bold">
                            <label class="flex items-center gap-1.5 p-2 bg-emerald-50 border border-emerald-200 rounded-xl text-emerald-900 cursor-pointer"><input type="radio" name="adminGateType" value="IN_MORNING" checked> 🌅 ເຂົ້າເຊົ້າ</label>
                            <label class="flex items-center gap-1.5 p-2 bg-amber-50 border border-amber-200 rounded-xl text-amber-900 cursor-pointer"><input type="radio" name="adminGateType" value="OUT_NOON"> ☀️ ອອກສວຍ</label>
                            <label class="flex items-center gap-1.5 p-2 bg-sky-50 border border-sky-200 rounded-xl text-sky-900 cursor-pointer"><input type="radio" name="adminGateType" value="IN_NOON"> 🌤️ ເຂົ້າສວຍ</label>
                            <label class="flex items-center gap-1.5 p-2 bg-rose-50 border border-rose-200 rounded-xl text-rose-900 cursor-pointer"><input type="radio" name="adminGateType" value="OUT_EVENING"> 🌆 ອອກແລງ</label>
                        </div>
                        <div id="adminCamBox" class="hidden relative overflow-hidden rounded-2xl bg-black">
                            <video id="adminCamVid" playsinline muted class="w-full h-48 object-cover"></video>
                            <div class="absolute inset-0 m-auto w-36 h-36 border-2 border-amber-400 rounded-2xl pointer-events-none"></div>
                            <button type="button" onclick="stopAdminCam()" class="absolute top-2 right-2 bg-black/70 text-white w-7 h-7 rounded-full text-xs"><i class="fa-solid fa-xmark"></i></button>
                        </div>
                        <button type="button" id="adminScanBtn" onclick="toggleAdminCam()" class="w-full bg-amber-400 hover:bg-amber-500 text-blue-950 font-extrabold py-2.5 rounded-xl text-xs shadow"><i class="fa-solid fa-qrcode mr-1.5"></i> ເປີດກ້ອງສະແກນບັດພະນັກງານ</button>
                    </div>

                    <!-- Point QR Generator & Wall Poster Printer -->
                    <div class="bg-white p-4 rounded-3xl shadow-sm border border-slate-100 space-y-2.5">
                        <div class="flex items-center justify-between">
                            <h3 class="text-xs font-bold text-slate-800"><i class="fa-solid fa-print text-blue-600 mr-1.5"></i> QR Code ຈຸດສະແກນ (ປິ້ນຕິດຝາ)</h3>
                            <span class="text-[9px] bg-emerald-50 text-emerald-700 px-2 py-0.5 rounded-full font-bold">ບັງຄັບກວດສອບ</span>
                        </div>
                        <div class="flex gap-1.5">
                            <input id="newCheckpointName" class="flex-1 p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs" placeholder="ຊື່ຈຸດສະແກນ ເຊັ່ນ: ປະຕູໃຫຍ່ 1, ຊັ້ນ 2...">
                            <button type="button" onclick="addCheckpointPoint()" class="bg-blue-900 text-white px-3 py-2 rounded-xl text-xs font-bold shrink-0"><i class="fa-solid fa-plus mr-1"></i> ເພີ່ມຈຸດ</button>
                        </div>
                        <div id="checkpointListBox" class="space-y-2 max-h-60 overflow-y-auto"></div>
                    </div>

                    <!-- Security & Backup -->
                    <div class="bg-white p-4 rounded-3xl shadow-sm border border-slate-100 space-y-2.5">
                        <h3 class="text-xs font-bold text-slate-800"><i class="fa-solid fa-gear text-slate-600 mr-1.5"></i> ຄວາມປອດໄພ & ສຳຮອງຂໍ້ມູນ</h3>
                        <div class="flex gap-2">
                            <input type="password" id="newAdminPin" maxlength="4" pattern="\d{4}" class="flex-1 p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs" placeholder="ປ່ຽນ Admin PIN ໃໝ່ (4 ຕົວເລກ)">
                            <button type="button" onclick="changeAdminPin()" class="bg-blue-900 text-white px-3 py-2 rounded-xl text-xs font-bold">ບັນທຶກ PIN</button>
                        </div>
                        <div class="grid grid-cols-2 gap-2 pt-1">
                            <button type="button" onclick="exportFullBackup()" class="bg-slate-800 text-white py-2 rounded-xl text-[11px] font-bold"><i class="fa-solid fa-database mr-1"></i> ສຳຮອງຂໍ້ມູນ (JSON)</button>
                            <button type="button" onclick="resetDemoData()" class="bg-rose-50 text-rose-700 border border-rose-200 py-2 rounded-xl text-[11px] font-bold"><i class="fa-solid fa-rotate mr-1"></i> ຄືນຄ່າເລີ່ມຕົ້ນ</button>
                        </div>
                    </div>
                </div>

                <!-- ADMIN TAB 7: BACKUP / RESTORE -->
                <div id="adminTabBackup" class="hidden space-y-3">
                    <div class="bg-white p-4 rounded-3xl shadow-sm border border-slate-100 space-y-3">
                        <h3 class="text-xs font-bold text-slate-800"><i class="fa-solid fa-cloud-arrow-down text-blue-700 mr-1.5"></i> ສຳຮອງຂໍ້ມູນ (Backup)</h3>
                        <p id="backupLastInfo" class="text-[10px] text-slate-500">ຍັງບໍ່ເຄີຍສຳຮອງຂໍ້ມູນໃນເຄື່ອງນີ້</p>
                        <div class="grid gap-2">
                            <button type="button" onclick="exportBackup('full')" class="text-left bg-blue-900 hover:bg-blue-800 text-white p-3 rounded-2xl shadow">
                                <span class="block text-xs font-extrabold"><i class="fa-solid fa-database mr-1.5"></i> ສຳຮອງທັງລະບົບ</span>
                                <span class="block text-[10px] text-blue-200 mt-0.5">ພະນັກງານ, ເຂົ້າ-ອອກ, ລາພັກ, ເວນຍາມ, ໂຄສະນາ, ເວລາ, ຈຸດສະແກນ, ໂລໂກ້</span>
                            </button>
                            <button type="button" onclick="exportBackup('org')" class="text-left bg-emerald-600 hover:bg-emerald-700 text-white p-3 rounded-2xl shadow">
                                <span class="block text-xs font-extrabold"><i class="fa-solid fa-sitemap mr-1.5"></i> ສຳຮອງລະບົບການຈັດຕັ້ງ</span>
                                <span class="block text-[10px] text-emerald-100 mt-0.5">ພະນັກງານ/ພະແນກ/ຕຳແໜ່ງ, ເວນຍາມ, ລາພັກ, ປະຫວັດເຂົ້າ-ອອກ</span>
                            </button>
                            <button type="button" onclick="exportBackup('admin')" class="text-left bg-slate-800 hover:bg-slate-700 text-white p-3 rounded-2xl shadow">
                                <span class="block text-xs font-extrabold"><i class="fa-solid fa-user-shield mr-1.5"></i> ສຳຮອງການຕັ້ງຄ່າ Admin</span>
                                <span class="block text-[10px] text-slate-300 mt-0.5">ໂຄສະນາ, ເວລາ 4 ຊ່ວງ, ຈຸດສະແກນ QR, ໂລໂກ້</span>
                            </button>
                        </div>
                        <p class="text-[10px] text-slate-500 leading-relaxed"> ຄອມ: ບັນທຶກໄຟລ໌ .json ລົງເຄື່ອງ &nbsp;|&nbsp;  ມືຖື: ເປີດໜ້າຕ່າງແບ່ງປັນ (ບັນທຶກລົງ Files / Drive / LINE / WhatsApp). ⚠️ ໄຟລ໌ມີລະຫັດຜ່ານພະນັກງານ ກະລຸນາເກັບໄວ້ໃຫ້ປອດໄພ. Admin PIN ບໍ່ຢູ່ໃນໄຟລ໌ສຳຮອງ.</p>
                    </div>
                    <div class="bg-white p-4 rounded-3xl shadow-sm border border-slate-100 space-y-3">
                        <h3 class="text-xs font-bold text-slate-800"><i class="fa-solid fa-clock-rotate-left text-amber-600 mr-1.5"></i> ກູ້ຄືນຂໍ້ມູນ (Restore)</h3>
                        <input type="file" id="restoreFileInput" accept=".json,application/json,text/plain" class="hidden" onchange="handleRestoreFile(event)">
                        <button type="button" onclick="$id('restoreFileInput').click()" class="w-full bg-amber-400 hover:bg-amber-500 text-blue-950 font-extrabold py-2.5 rounded-xl text-xs shadow"><i class="fa-solid fa-file-import mr-1.5"></i> ເລືອກໄຟລ໌ Backup (.json)</button>
                        <div id="restorePreview" class="hidden space-y-2.5 bg-slate-50 border border-slate-200 rounded-2xl p-3">
                            <div id="restoreSummary" class="text-[11px] text-slate-700 space-y-0.5"></div>
                            <label class="flex items-start gap-2 text-[11px] text-slate-700"><input type="radio" name="restoreMode" value="merge" checked class="mt-0.5"> <span><b>ລວມຂໍ້ມູນ</b> (ແນະນຳ) — ເພີ່ມ/ອັບເດດ ບໍ່ລຶບຂອງເດີມ</span></label>
                            <label class="flex items-start gap-2 text-[11px] text-slate-700"><input type="radio" name="restoreMode" value="replace" class="mt-0.5"> <span><b>ແທນທີ່ທັງໝົດ</b> — ລຶບຂອງເດີມຕາມໝວດໃນໄຟລ໌ (ລະບົບຈະດາວໂຫຼດ Backup ເກົ່າໃຫ້ກ່ອນ)</span></label>
                            <div class="flex gap-2">
                                <button type="button" onclick="applyRestore()" class="flex-1 bg-blue-900 text-white py-2 rounded-xl text-xs font-bold">ຢືນຢັນກູ້ຄືນ</button>
                                <button type="button" onclick="cancelRestore()" class="px-4 bg-white border border-slate-300 text-slate-600 py-2 rounded-xl text-xs font-bold">ຍົກເລີກ</button>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- ADMIN TAB 4: ADVERTISEMENTS & COMPANY LOGO MANAGER -->
                <div id="adminTabAds" class="hidden space-y-3">
                    <!-- Company Logo Changer Card -->
                    <div class="bg-white p-4 rounded-3xl shadow-sm border border-slate-100 space-y-3">
                        <div class="flex items-center justify-between border-b pb-2">
                            <h3 class="text-xs font-bold text-blue-950 flex items-center">
                                <i class="fa-solid fa-shield-cat text-amber-500 mr-1.5"></i> ຈັດການໂລໂກ້ບໍລິສັດ (Company Logo)
                            </h3>
                            <button type="button" onclick="resetCompanyLogo()" class="text-[10px] font-bold text-rose-600 hover:underline">ຄືນຄ່າໂລໂກ້ ECI 35</button>
                        </div>
                        <div class="flex items-center gap-3">
                            <div class="w-16 h-16 rounded-full border-2 border-amber-400 bg-white overflow-hidden shadow shrink-0 flex items-center justify-center">
                                <img data-company-logo src="" alt="Preview Logo" class="w-full h-full object-cover">
                            </div>
                            <div class="flex-1 space-y-1.5">
                                <label class="block w-full text-center py-2 px-3 bg-blue-900 hover:bg-blue-800 text-white rounded-xl text-[11px] font-bold cursor-pointer shadow">
                                    <i class="fa-solid fa-upload mr-1"></i> ອັບໂຫຼດຮູບໂລໂກ້ໃໝ່ຈາກເຄື່ອງ
                                    <input type="file" id="logoFileInput" accept="image/*" onchange="handleLogoFileUpload(event)" class="hidden">
                                </label>
                                <div class="flex gap-1">
                                    <input type="url" id="logoUrlInput" class="flex-1 p-1.5 bg-slate-50 border border-slate-200 rounded-xl text-[10px]" placeholder="ຫຼື ວາງລິ້ງ URL ຮູບໂລໂກ້...">
                                    <button type="button" onclick="handleLogoUrlSave()" class="px-2.5 py-1.5 bg-amber-400 text-blue-950 rounded-xl text-[10px] font-extrabold">ໃຊ້ລິ້ງ</button>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Advertisement Form (Create / Edit) -->
                    <form id="adForm" onsubmit="handleSaveAd(event)" class="bg-white p-4 rounded-3xl shadow-sm border border-slate-100 space-y-2.5">
                        <div class="flex items-center justify-between border-b pb-2">
                            <h3 id="adFormTitle" class="text-xs font-bold text-blue-950 flex items-center">
                                <i class="fa-solid fa-bullhorn text-amber-500 mr-1.5"></i> ເພີ່ມໂຄສະນາ / ປະກາດຫຼັງ Login
                            </h3>
                            <button type="button" id="cancelEditAdBtn" onclick="resetAdForm()" class="hidden text-[10px] text-rose-600 font-bold underline">ຍົກເລີກແກ້ໄຂ</button>
                        </div>
                        <input type="hidden" id="editAdId" value="">
                        <div>
                            <label class="block text-[10px] font-semibold text-slate-700 mb-0.5">ຫົວຂໍ້ໂຄສະນາ / ປະກາດ *</label>
                            <input type="text" id="adTitleInput" required class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs" placeholder="ເຊັ່ນ: ປະກາດສະຫຼອງຄົບຮອບ 35 ປີ ECI...">
                        </div>
                        <div>
                            <label class="block text-[10px] font-semibold text-slate-700 mb-0.5">ລາຍລະອຽດ *</label>
                            <textarea id="adDescInput" rows="2" required class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs" placeholder="ປ້ອນເນື້ອໃນໂຄສະນາ ຫຼື ແຈ້ງການໃຫ້ພະນັກງານຮັບຊາບ..."></textarea>
                        </div>
                        <div class="grid grid-cols-2 gap-2">
                            <div>
                                <label class="block text-[10px] font-semibold text-slate-700 mb-0.5">ປ້າຍກຳກັບ (Badge)</label>
                                <select id="adBadgeInput" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                                    <option value="📢 ໂຄສະນາ">📢 ໂຄສະນາ</option>
                                    <option value="📌 ປະກາດສຳຄັນ">📌 ປະກາດສຳຄັນ</option>
                                    <option value="🎁 ໂປຣໂມຊັນ / ສະຫວັດດີການ">🎁 ສະຫວັດດີການ</option>
                                    <option value="⚡ ຂ່າວສານ ECI">⚡ ຂ່າວສານ ECI</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-[10px] font-semibold text-slate-700 mb-0.5">ຮູບແບບການສະແດງ</label>
                                <select id="adModeInput" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                                    <option value="both"> Banner + Popup ຫຼັງ Login</option>
                                    <option value="banner"> ສະແດງເປັນ Banner ຫຼັງ Login</option>
                                    <option value="popup"> ສະແດງເປັນ Popup ທັນທີ</option>
                                </select>
                            </div>
                        </div>
                        <div class="space-y-1.5">
                            <label class="block text-[10px] font-semibold text-slate-700">ຮູບພາບໂຄສະນາ (ອັບໂຫຼດ ຫຼື ໃສ່ URL)</label>
                            <div class="flex gap-2 items-center">
                                <label class="px-3 py-2 bg-slate-800 text-white rounded-xl text-[10px] font-bold cursor-pointer shrink-0">
                                    <i class="fa-solid fa-image mr-1"></i> ເລືອກຮູບ
                                    <input type="file" id="adImgFileInput" accept="image/*" onchange="handleAdImageUpload(event)" class="hidden">
                                </label>
                                <input type="text" id="adImgUrlInput" class="flex-1 p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs" placeholder="ຫຼື ວາງລິ້ງ URL ຮູບພາບ (ຖ້າມີ)">
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-2">
                            <div>
                                <label class="block text-[10px] font-semibold text-slate-700 mb-0.5">ລິ້ງເພີ່ມເຕີມ (ຖ້າມີ)</label>
                                <input type="url" id="adLinkInput" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs" placeholder="https://...">
                            </div>
                            <div>
                                <label class="block text-[10px] font-semibold text-slate-700 mb-0.5">ສະຖານະ</label>
                                <select id="adActiveInput" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                                    <option value="true"> ເປີດສະແດງໂຄສະນາ</option>
                                    <option value="false"> ປິດຊົ່ວຄາວ</option>
                                </select>
                            </div>
                        </div>
                        <button type="submit" class="w-full bg-amber-400 hover:bg-amber-500 text-blue-950 font-extrabold py-2.5 rounded-xl text-xs shadow">
                            <i class="fa-solid fa-floppy-disk mr-1"></i> ບັນທຶກໂຄສະນາ
                        </button>
                    </form>

                    <!-- List of Managed Advertisements -->
                    <div class="bg-white p-3.5 rounded-3xl shadow-sm border border-slate-100 space-y-2">
                        <div class="flex items-center justify-between">
                            <h4 class="text-xs font-bold text-slate-800">ລາຍການໂຄສະນາທັງໝົດ</h4>
                            <span id="adminAdCount" class="text-[10px] text-slate-500">0 ລາຍການ</span>
                        </div>
                        <div id="adminAdList" class="space-y-2 max-h-72 overflow-y-auto"></div>
                    </div>
                </div>

                <!-- ADMIN TAB 5: DUTIES -->
                <div id="adminTabDuties" class="hidden space-y-3">
                    <form onsubmit="handleAssignDuty(event)" class="bg-white p-4 rounded-3xl shadow-sm border border-slate-100 space-y-2.5">
                        <h3 class="text-xs font-bold text-slate-800 border-b pb-2 flex items-center"><i class="fa-solid fa-circle-plus text-blue-600 mr-2"></i> ມອບໝາຍເວນຍາມ / ວຽກນອກສະຖານທີ່</h3>
                        <div>
                            <label class="block text-[11px] font-semibold text-slate-700 mb-1">ເລືອກພະນັກງານ *</label>
                            <select id="dutyEmpSelect" onchange="syncDutyEmpInput()" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs mb-1.5"></select>
                            <input type="text" id="dutyEmpName" required class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs" placeholder="ຫຼື ພິມຊື່ພະນັກງານໂດຍກົງ">
                        </div>
                        <div class="grid grid-cols-2 gap-2">
                            <div><label class="block text-[11px] font-semibold text-slate-700 mb-1">ວັນທີເຂົ້າເວນ *</label><input type="date" id="dutyDate" required class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs"></div>
                            <div>
                                <label class="block text-[11px] font-semibold text-slate-700 mb-1">ກະເວນ *</label>
                                <select id="dutyShift" required class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                                    <option value="☀️ ເວນເຊົ້າ (08:00 - 12:00)">☀️ ເວນເຊົ້າ (08:00 - 12:00)</option>
                                    <option value="🌤️ ເວນບ່າຍ (12:30 - 17:00)">🌤️ ເວນບ່າຍ (12:30 - 17:00)</option>
                                    <option value="🌙 ເວນເດິກ (17:00 - 08:00)">🌙 ເວນເດິກ (17:00 - 08:00)</option>
                                </select>
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-2">
                            <div>
                                <label class="block text-[11px] font-semibold text-slate-700 mb-1">ປະເພດວຽກ</label>
                                <select id="dutyKind" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                                    <option value="onsite"> ວຽກໃນສະຖານທີ່</option>
                                    <option value="offsite"> ວຽກນອກສະຖານທີ່</option>
                                </select>
                            </div>
                            <div><label class="block text-[11px] font-semibold text-slate-700 mb-1">ຈຸດປະຈຳການ *</label><input type="text" id="dutyLocation" required class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs" placeholder="ປະຕູໃຫຍ່ / ສະຖານີ 35"></div>
                        </div>
                        <button type="submit" class="w-full bg-blue-900 hover:bg-blue-800 text-white font-bold py-2.5 rounded-xl text-xs shadow"><i class="fa-solid fa-floppy-disk mr-1"></i> ບັນທຶກ ແລະ ປະກາດເວນຍາມ</button>
                    </form>
                    <div id="adminDutyList" class="space-y-2 max-h-64 overflow-y-auto"></div>
                </div>

                <!-- ADMIN TAB 6: LEAVES -->
                <!-- ADMIN TAB: FINANCE -->
                <div id="adminTabFinance" class="hidden space-y-3">
                    <div class="bg-white rounded-2xl border border-slate-100 p-2 flex items-center gap-2"><div id="saveStat" class="flex-1 text-[10px] text-slate-500">💾 ກຳລັງກວດການບັນທຶກ...</div>
                        <button onclick="backupFin()" class="bg-slate-800 text-white px-2 py-1.5 rounded-lg text-[10px] font-bold">⬇ ສຳຮອງ</button>
                        <label class="bg-slate-100 text-slate-700 px-2 py-1.5 rounded-lg text-[10px] font-bold cursor-pointer">⬆ ກູ້ຄືນ<input type="file" accept=".json" class="hidden" onchange="restoreFin(this)"></label></div>
                    <div class="grid grid-cols-3 gap-1 bg-white rounded-2xl p-1 border border-slate-100 text-[10px] font-bold">
                        <button onclick="finSub('pay')" id="finBtn_pay" class="py-1.5 rounded-xl">💰 ເງິນເດືອນ</button>
                        <button onclick="finSub('docs')" id="finBtn_docs" class="py-1.5 rounded-xl">📄 ເອກະສານ</button>
                        <button onclick="finSub('led')" id="finBtn_led" class="py-1.5 rounded-xl">📊 ລາຍຮັບ-ຈ່າຍ</button>
                    </div>
                    <!-- PAYROLL (OFFICIAL) -->
                    <div id="finSub_pay" class="space-y-2">
                        <div class="bg-white p-3 rounded-3xl border border-slate-100 space-y-2">
                            <div class="flex gap-2 items-end">
                                <div class="flex-1"><label class="block text-[10px] font-semibold mb-0.5">ງວດເດືອນ</label><input type="month" id="payMonth" onchange="renderPayroll()" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs"></div>
                                <span id="payStatus" class="text-[10px] font-bold px-3 py-2 rounded-xl"></span>
                            </div>
                            <details><summary class="text-[10px] font-bold text-blue-900 cursor-pointer">⚙️ ຕັ້ງຄ່າລະບົບເງິນເດືອນ</summary>
                                <div class="grid grid-cols-2 gap-1.5 mt-2 text-[9px] text-slate-600">
                                    <div class="col-span-2">ຊື່ອົງກອນ<input id="cfgCo" onchange="saveCfg()" class="w-full p-1.5 bg-slate-50 border rounded-lg text-xs"></div>
                                    <div>ປະກັນສັງຄົມ (%)<input type="number" step="0.1" id="cfgSso" onchange="saveCfg()" class="w-full p-1.5 bg-slate-50 border rounded-lg text-xs"></div>
                                    <div>ເພດານຖານປະກັນສັງຄົມ (ກີບ)<input type="number" id="cfgCap" onchange="saveCfg()" class="w-full p-1.5 bg-slate-50 border rounded-lg text-xs"></div>
                                    <div>ມື້ເຮັດວຽກ/ເດືອນ<input type="number" id="cfgDays" onchange="saveCfg()" class="w-full p-1.5 bg-slate-50 border rounded-lg text-xs"></div>
                                    <div>ຊົ່ວໂມງ/ມື້<input type="number" id="cfgHours" onchange="saveCfg()" class="w-full p-1.5 bg-slate-50 border rounded-lg text-xs"></div>
                                    <div>ຕົວຄູນ OT (ເທົ່າ)<input type="number" step="0.1" id="cfgOt" onchange="saveCfg()" class="w-full p-1.5 bg-slate-50 border rounded-lg text-xs"></div>
                                    <div>ຜູ້ຈັດທຳ<input id="cfgS1" onchange="saveCfg()" class="w-full p-1.5 bg-slate-50 border rounded-lg text-xs"></div>
                                    <div>ຫົວໜ້າບັນຊີ<input id="cfgS2" onchange="saveCfg()" class="w-full p-1.5 bg-slate-50 border rounded-lg text-xs"></div>
                                    <div>ຜູ້ອຳນວຍການ<input id="cfgS3" onchange="saveCfg()" class="w-full p-1.5 bg-slate-50 border rounded-lg text-xs"></div>
                                    <div class="col-span-2">ຂັ້ນພາສີລາຍໄດ້ (ຂອບເທິງກີບ,ອັດຕາ% ; 0 = ບໍ່ຈຳກັດ) — ກວດກັບກົດໝາຍປັດຈຸບັນ<textarea id="cfgTax" rows="6" onchange="saveCfg()" class="w-full p-1.5 bg-slate-50 border rounded-lg text-xs font-mono"></textarea></div>
                                </div>
                            </details>
                            <div class="grid grid-cols-3 gap-1.5">
                                <button onclick="payApprove()" id="payApproveBtn" class="bg-blue-900 text-white py-2 rounded-xl text-[10px] font-bold"></button>
                                <button onclick="payMarkPaid()" class="bg-emerald-600 text-white py-2 rounded-xl text-[10px] font-bold">ຢືນຢັນຈ່າຍທັງໝົດ</button>
                                <button onclick="printSheet()" class="bg-slate-800 text-white py-2 rounded-xl text-[10px] font-bold"><i class="fa-solid fa-print"></i> ໃບສະຫຼຸບ</button>
                            </div>
                        </div>
                        <div class="bg-white p-2.5 rounded-2xl border border-slate-100 space-y-1.5">
                            <div class="relative"><i class="fa-solid fa-magnifying-glass absolute left-3 top-2.5 text-slate-400 text-xs"></i><input id="paySearch" oninput="renderPayroll()" placeholder="ຄົ້ນຫາຊື່, ລະຫັດ, ຕຳແໜ່ງ, ພະແນກ, ເບີໂທ..." class="w-full pl-8 pr-3 py-2 bg-slate-50 border border-slate-200 rounded-xl text-xs"></div>
                            <div class="grid grid-cols-2 gap-1.5"><select id="payDept" onchange="renderPayroll()" class="p-2 bg-slate-50 border border-slate-200 rounded-xl text-[11px]"></select>
                            <select id="payFilt" onchange="renderPayroll()" class="p-2 bg-slate-50 border border-slate-200 rounded-xl text-[11px]"><option value="">ທຸກສະຖານະ</option><option value="paid">ຈ່າຍແລ້ວ</option><option value="unpaid">ຍັງບໍ່ຈ່າຍ</option><option value="nosal">ຍັງບໍ່ຕັ້ງເງິນເດືອນ</option></select></div>
                            <div id="payCount" class="text-[10px] text-slate-500"></div>
                        </div>
                        <details class="bg-white p-2.5 rounded-2xl border border-slate-100"><summary class="text-[11px] font-bold text-blue-900 cursor-pointer">📅 ບັນທຶກ/ລາຍງານພະນັກງານ ປະຈຳເດືອນ-ປີ</summary>
                            <div class="grid grid-cols-2 gap-1.5 mt-2">
                                <select id="repType" onchange="repToggle()" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs"><option value="month">ລາຍເດືອນ</option><option value="year">ລາຍປີ (12 ເດືອນ)</option><option value="range">ເລືອກຊ່ວງເດືອນ</option></select>
                                <div id="repBoxMonth"><input type="month" id="repMonth" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs"></div>
                                <div id="repBoxYear" class="hidden"><input type="number" id="repYear" placeholder="ປີ ເຊັ່ນ 2026" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs"></div>
                                <div id="repBoxRange" class="hidden col-span-2 grid grid-cols-2 gap-1.5"><input type="month" id="repFrom" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs"><input type="month" id="repTo" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs"></div>
                            </div>
                            <div class="mt-2 text-[10px] font-bold text-slate-700">ເລືອກພະນັກງານ <span id="repSelCount" class="text-blue-900"></span></div>
                            <input id="repEmpSearch" oninput="repEmpRender()" placeholder="🔍 ຄົ້ນຫາເພື່ອກັ່ນຕອງລາຍຊື່" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs mt-1">
                            <div class="flex gap-1.5 mt-1"><button onclick="repAll(true)" class="flex-1 bg-slate-100 py-1.5 rounded-lg text-[10px] font-bold">ເລືອກທັງໝົດທີ່ສະແດງ</button><button onclick="repAll(false)" class="flex-1 bg-slate-100 py-1.5 rounded-lg text-[10px] font-bold">ລ້າງການເລືອກ</button></div>
                            <div id="repEmpList" class="mt-1 max-h-40 overflow-y-auto border border-slate-100 rounded-xl divide-y"></div>
                            <label class="flex items-center gap-1.5 text-[10px] mt-1.5"><input type="checkbox" id="repDraft" checked> ລວມເດືອນທີ່ຍັງເປັນ "ຮ່າງ"</label>
                            <button onclick="printEmpReport()" class="w-full mt-1.5 bg-blue-900 text-white py-2 rounded-xl text-[11px] font-bold"><i class="fa-solid fa-print"></i> ພິມລາຍງານ</button>
                            <p class="text-[9px] text-slate-500 mt-1">ເລືອກ 1 ຄົນ = ບັນທຶກລາຍລະອຽດແຕ່ລະເດືອນ · ເລືອກຫຼາຍຄົນ = ຕາຕະລາງສະຫຼຸບຂອງຊ່ວງທີ່ເລືອກ.</p></details>
                        <div id="payList" class="space-y-2"></div>
                        <div id="paySum" class="bg-blue-950 text-white p-3 rounded-2xl text-[11px]"></div>
                    </div>
                    <!-- DOCS -->
                    <div id="finSub_docs" class="hidden space-y-2">
                        <div class="bg-white p-3 rounded-3xl border border-slate-100 space-y-2">
                            <input id="docTitle" placeholder="ຊື່ເອກະສານ *" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                            <div class="grid grid-cols-2 gap-2">
                                <select id="docCat" class="p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs"><option>ໃບເກັບເງິນ/ໃບເສັດ</option><option>ສັນຍາ</option><option>ໃບອະນຸຍາດ</option><option>ເອກະສານພາສີ</option><option>ອື່ນໆ</option></select>
                                <input type="date" id="docDate" class="p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                            </div>
                            <select id="docEmp" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs"></select>
                            <input id="docNote" placeholder="ໝາຍເຫດ" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                            <div class="flex gap-2">
                                <label class="flex-1 text-center bg-blue-900 text-white py-2 rounded-xl text-[11px] font-bold cursor-pointer"><i class="fa-solid fa-camera mr-1"></i>ສະແກນ/ຖ່າຍຮູບ<input type="file" accept="image/*" capture="environment" class="hidden" onchange="docPick(this)"></label>
                                <label class="flex-1 text-center bg-slate-100 text-slate-700 py-2 rounded-xl text-[11px] font-bold cursor-pointer"><i class="fa-solid fa-image mr-1"></i>ເລືອກໄຟລ໌<input type="file" accept="image/*" class="hidden" onchange="docPick(this)"></label>
                            </div>
                            <label ondragover="event.preventDefault()" ondrop="event.preventDefault();docFilesPick(event.dataTransfer.files)" class="block text-center border-2 border-dashed border-blue-300 bg-blue-50/50 rounded-xl py-3 text-[11px] font-bold text-blue-900 cursor-pointer"><i class="fa-solid fa-cloud-arrow-up mr-1"></i>ອັບໂຫຼດໄຟລ໌ (PDF, Word, Excel, ຮູບ…) — ແຕະ ຫຼື ລາກມາວາງ<input type="file" multiple class="hidden" onchange="docFilesPick(this.files);this.value=''"></label>
                            <div id="docFileChips" class="flex flex-wrap gap-1"></div>
                            <p class="text-[9px] text-slate-400">ສູງສຸດ 10 ໄຟລ໌ ໄຟລ໌ລະ 15 MB. ໄຟລ໌ເກັບໃນເຄື່ອງນີ້ (ບໍ່ຊິງຄ໌ຂ້າມເຄື່ອງ) — ໃຊ້ ⬇ ສຳຮອງ ເພື່ອເກັບສຳຮອງ.</p>
                            <img id="docPrev" class="hidden w-full max-h-40 object-contain rounded-xl border">
                            <button onclick="saveDoc()" class="w-full bg-emerald-600 text-white py-2 rounded-xl text-xs font-bold">ບັນທຶກເອກະສານ</button>
                        </div>
                        <input id="docSearch" oninput="renderDocs()" placeholder="🔍 ຄົ້ນຫາຊື່, ໝວດ, ພະນັກງານ, ໝາຍເຫດ..." class="w-full p-2 bg-white border border-slate-200 rounded-xl text-xs">
                        <div class="grid grid-cols-2 gap-1.5"><select id="docFilCat" onchange="renderDocs()" class="p-2 bg-white border border-slate-200 rounded-xl text-[11px]"></select><select id="docFilEmp" onchange="renderDocs()" class="p-2 bg-white border border-slate-200 rounded-xl text-[11px]"></select></div>
                        <div id="docCount" class="text-[10px] text-slate-500"></div>
                        <div id="docList" class="space-y-2"></div>
                    </div>
                    <!-- LEDGER -->
                    <div id="finSub_led" class="hidden space-y-2">
                        <div class="bg-white p-3 rounded-3xl border border-slate-100 space-y-2">
                            <div class="grid grid-cols-2 gap-2">
                                <select id="ledType" class="p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs"><option value="in">ລາຍຮັບ</option><option value="out">ລາຍຈ່າຍ</option></select>
                                <input type="date" id="ledDate" class="p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                            </div>
                            <input id="ledCat" placeholder="ໝວດ/ລາຍການ *" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                            <div class="grid grid-cols-2 gap-2">
                                <input type="number" id="ledAmt" placeholder="ຈຳນວນເງິນ (ກີບ) *" class="p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                                <input type="number" step="0.1" id="ledTax" placeholder="ພາສີ/ອາກອນ % (VAT 10)" class="p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                            </div>
                            <label class="flex items-center gap-1.5 text-[10px]"><input type="checkbox" id="ledIncl"> ຈຳນວນເງິນລວມພາສີແລ້ວ</label>
                            <input id="ledNote" placeholder="ໝາຍເຫດ" class="w-full p-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                            <button onclick="saveLed()" class="w-full bg-emerald-600 text-white py-2 rounded-xl text-xs font-bold">ບັນທຶກລາຍການ</button>
                        </div>
                        <div class="flex gap-2"><input type="month" id="ledMonth" onchange="renderLed()" class="flex-1 p-2 bg-white border border-slate-200 rounded-xl text-xs"><button onclick="exportLed()" class="bg-slate-800 text-white px-3 rounded-xl text-[10px] font-bold">CSV</button></div>
                        <div class="grid grid-cols-2 gap-1.5"><button onclick="printLedMonth()" class="bg-blue-900 text-white py-2 rounded-xl text-[10px] font-bold"><i class="fa-solid fa-print"></i> ພິມລາຍງານລາຍເດືອນ</button><button onclick="printLedYear()" class="bg-indigo-700 text-white py-2 rounded-xl text-[10px] font-bold"><i class="fa-solid fa-print"></i> ພິມລາຍງານລາຍປີ</button></div>
                        <div id="ledSum" class="grid grid-cols-2 gap-1.5 text-center text-[10px]"></div>
                        <div id="ledList" class="space-y-1.5"></div>
                    </div>
                </div>

                <div id="adminTabLeaves" class="hidden space-y-2">
                    <div class="text-xs font-bold text-slate-700 mb-1 px-1">ລາຍການໃບຂໍລາພັກທີ່ສົ່ງເຂົ້າມາ</div>
                    <div id="adminLeaveList" class="space-y-2 max-h-96 overflow-y-auto"></div>
                </div>
            </section>
        </main>

        <!-- Bottom Mobile Navigation Bar -->
        <nav id="bottomNav" class="hidden bg-white border-t border-slate-200 flex justify-around items-center p-2.5 absolute bottom-0 left-0 right-0 z-40">
            <button onclick="switchTab('checkin')" id="navBtnCheckin" class="flex flex-col items-center text-blue-900"><i class="fa-solid fa-qrcode text-base"></i><span class="text-[10px] mt-0.5 font-bold">ສະແກນເວລາ</span></button>
            <button onclick="switchTab('duty')" id="navBtnDuty" class="flex flex-col items-center text-slate-400 hover:text-blue-900"><i class="fa-solid fa-calendar-days text-base"></i><span class="text-[10px] mt-0.5 font-bold">ຕາຕະລາງເວນ</span></button>
            <button onclick="switchTab('leave')" id="navBtnLeave" class="flex flex-col items-center text-slate-400 hover:text-blue-900"><i class="fa-solid fa-file-pen text-base"></i><span class="text-[10px] mt-0.5 font-bold">ຂໍລາພັກ</span></button>
            <button onclick="toggleAdminView()" id="navBtnAdmin" class="flex flex-col items-center text-amber-600 hover:text-amber-700"><i class="fa-solid fa-shield-halved text-base"></i><span class="text-[10px] mt-0.5 font-bold">Admin</span></button>
        </nav>
    </div>

    <script>
        // =========================================================================
        // 1. DEFAULT ECI 35TH ANNIVERSARY EMBLEM SVG DATA URI
        // =========================================================================
        const DEFAULT_ECI_LOGO_SVG = "data:image/jpeg;base64,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";

        const DEFAULT_AD_BANNER_IMG = `data:image/svg+xml;utf8,` + encodeURIComponent(`
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 600 260">
            <defs>
                <linearGradient id="bg" x1="0" y1="0" x2="1" y2="1">
                    <stop offset="0%" stop-color="#1e3a8a"/>
                    <stop offset="60%" stop-color="#1d4ed8"/>
                    <stop offset="100%" stop-color="#0f172a"/>
                </linearGradient>
            </defs>
            <rect width="600" height="260" fill="url(#bg)"/>
            <circle cx="510" cy="60" r="110" fill="#facc15" opacity="0.16"/>
            <circle cx="90" cy="220" r="95" fill="#38bdf8" opacity="0.15"/>
            <path d="M460 35 L425 125 L465 125 L440 215 L515 105 L470 105 Z" fill="#facc15" opacity="0.85"/>
            <text x="36" y="75" fill="#facc15" font-family="Noto Sans Lao, sans-serif" font-weight="800" font-size="20">ລັດວິສາຫະກິດກໍ່ສ້າງ ແລະ ຕິດຕັ້ງໄຟຟ້າ (ECI)</text>
            <text x="36" y="112" fill="#ffffff" font-family="Noto Sans Lao, sans-serif" font-weight="800" font-size="26">ລະບົບລົງເວລາເຮັດວຽກ 4 ຊ່ວງ</text>
            <text x="36" y="146" fill="#cbd5e1" font-family="Noto Sans Lao, sans-serif" font-size="16">ເຂົ້າເຊົ້າ 08:00 • ພັກສວຍ 12:00 • ເຂົ້າສວຍ 12:30 • ອອກແລງ 17:00</text>
        </svg>`);

        // =========================================================================
        // 2. FIREBASE ONLINE DATABASE CONFIGURATION
        // =========================================================================
        const firebaseConfig = {
            apiKey: "YOUR_API_KEY",
            authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
            databaseURL: "https://YOUR_PROJECT_ID-default-rtdb.firebaseio.com",
            projectId: "YOUR_PROJECT_ID",
            storageBucket: "YOUR_PROJECT_ID.appspot.com",
            messagingSenderId: "YOUR_SENDER_ID",
            appId: "YOUR_APP_ID"
        };

        const isFirebaseConfigured = firebaseConfig.apiKey !== "YOUR_API_KEY";
        let db = null;
        if (isFirebaseConfigured && window.firebase) {
            firebase.initializeApp(firebaseConfig);
            db = firebase.database();
        }

        // =========================================================================
        // 3. STORAGE ENGINE & DEFAULT DATA
        // =========================================================================
        const STORAGE_KEYS = {
            ACCOUNTS: 'eci_v3_accounts',
            LOGS: 'eci_v3_logs',
            LEAVES: 'eci_v3_leaves',
            DUTIES: 'eci_v3_duties',
            ADMIN_PIN: 'eci_v3_admin_pin',
            SESSION: 'eci_v3_session',
            BRAND_LOGO: 'eci_v3_brand_logo',
            ADS: 'eci_v3_ads',
            SHIFT_TIMES: 'eci_v3_shift_times',
            CHECKPOINTS: 'eci_v3_checkpoints',
            FINANCE: 'eci_v3_finance'
        };

        const DEFAULT_ACCOUNTS = [
            {
                id: 'ECI-135',
                code: 'ECI-135',
                name: 'ທ່ານ ສິງຫາ ສີເມືອງວົງ',
                dept: 'ພະແນກເຈລະຈາທຸລະກິດ ແລະ ການຮ່ວມມື',
                position: 'ຫົວໜ້າພະແນກ',
                phone: '020 91581225',
                pw: '123456',
                status: 'active'
            }
        ];

        const DEFAULT_SHIFT_TIMES = {
            t1: '08:00', // ເຂົ້າວຽກເຊົ້າ
            t2: '12:00', // ພັກ/ອອກສວຍ
            t3: '12:30', // ເຂົ້າວຽກສວຍ
            t4: '17:00'  // ອອກວຽກແລງ
        };

        const DEFAULT_CHECKPOINTS = [
            {
                id: 'ECI-35-GATE-01',
                name: 'ECI-35-GATE-01 (ສຳນັກງານໃຫຍ່)',
                payload: 'ECI-CP|ECI-35-GATE-01|ສຳນັກງານໃຫຍ່'
            }
        ];

        const DEFAULT_ADS = [
            {
                id: 1,
                title: 'ຍິນດີຕ້ອນຮັບສູ່ລະບົບບໍລິຫານພະນັກງານ ECI',
                desc: 'ກະລຸນາສະແກນ QR ຈຸດກວດກາໃຫ້ຄົບ 4 ຊ່ວງເວລາ: ເຂົ້າເຊົ້າ 08:00, ອອກສວຍ 12:00, ເຂົ້າສວຍ 12:30 ແລະ ອອກແລງ 17:00.',
                badge: '📢 ປະກາດສຳຄັນ',
                mode: 'banner',
                image: DEFAULT_AD_BANNER_IMG,
                link: '',
                active: true
            }
        ];

        function loadLocal(key, fallback) {
            try {
                const raw = localStorage.getItem(key);
                return raw ? JSON.parse(raw) : JSON.parse(JSON.stringify(fallback));
            } catch (e) {
                return JSON.parse(JSON.stringify(fallback));
            }
        }

        function saveStore(key, data) {
            try {
                localStorage.setItem(key, JSON.stringify(data));
            } catch (e) {
                showToast('ພື້ນທີ່ຈັດເກັບເຕັມ ກະລຸນາໃຊ້ຮູບຂະໜາດນ້ອຍລົງ', true);
            }
            if (db && key !== STORAGE_KEYS.SESSION) {
                db.ref('eci_system/' + key).set(data).catch(err => console.error('Cloud Sync Error:', err));
            }
        }

        let accounts = loadLocal(STORAGE_KEYS.ACCOUNTS, DEFAULT_ACCOUNTS);
        let attendanceLogs = loadLocal(STORAGE_KEYS.LOGS, []);
        let leaveRequests = loadLocal(STORAGE_KEYS.LEAVES, []);
        let dutyList = loadLocal(STORAGE_KEYS.DUTIES, []);
        let adminPin = localStorage.getItem(STORAGE_KEYS.ADMIN_PIN) || '1234';
        let currentUser = loadLocal(STORAGE_KEYS.SESSION, null);
        let companyLogo = localStorage.getItem(STORAGE_KEYS.BRAND_LOGO) || DEFAULT_ECI_LOGO_SVG;
        let companyAds = loadLocal(STORAGE_KEYS.ADS, DEFAULT_ADS);
        let shiftTimes = loadLocal(STORAGE_KEYS.SHIFT_TIMES, DEFAULT_SHIFT_TIMES);
        let checkpoints = loadLocal(STORAGE_KEYS.CHECKPOINTS, DEFAULT_CHECKPOINTS);
        let fin = loadLocal(STORAGE_KEYS.FINANCE, { salaries: {}, payroll: {}, docs: [], ledger: [], cfg: { sso: 5.5, tax: '1300000,0\n8000000,5\n15000000,10\n25000000,15\n65000000,20\n0,25' } });
        let activePosterPoint = checkpoints[0];

        let currentFilter = 'all';
        let toastTimeout = null;
        let currentAdIdx = 0;
        let adCarouselTimer = null;
        const $id = x => document.getElementById(x);

        // =========================================================================
        // 4. COMPANY LOGO & PWA ICON ENGINE
        // =========================================================================
        function applyBrandLogo() {
            const logoSrc = companyLogo || DEFAULT_ECI_LOGO_SVG;
            document.querySelectorAll('img[data-company-logo]').forEach(img => {
                img.src = logoSrc;
            });
            if ($id('appFavicon')) $id('appFavicon').href = logoSrc;
            if ($id('appleTouchIcon')) $id('appleTouchIcon').href = logoSrc;
        }

        function compressImageFile(file, maxWidth = 420, quality = 0.78) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = (ev) => {
                    const img = new Image();
                    img.onload = () => {
                        const scale = Math.min(1, maxWidth / img.width);
                        const w = Math.round(img.width * scale);
                        const h = Math.round(img.height * scale);
                        const canvas = document.createElement('canvas');
                        canvas.width = w;
                        canvas.height = h;
                        canvas.getContext('2d').drawImage(img, 0, 0, w, h);
                        resolve(canvas.toDataURL('image/jpeg', quality));
                    };
                    img.onerror = reject;
                    img.src = ev.target.result;
                };
                reader.onerror = reject;
                reader.readAsDataURL(file);
            });
        }

        async function handleLogoFileUpload(e) {
            const file = e.target.files[0];
            if (!file) return;
            try {
                const dataUrl = await compressImageFile(file, 320, 0.85);
                companyLogo = dataUrl;
                localStorage.setItem(STORAGE_KEYS.BRAND_LOGO, companyLogo);
                if (db) db.ref('eci_system/' + STORAGE_KEYS.BRAND_LOGO).set(companyLogo);
                applyBrandLogo();
                showToast('ປ່ຽນໂລໂກ້ບໍລິສັດໃໝ່ສຳເລັດ!');
            } catch (err) {
                showToast('ບໍ່ສາມາດໂຫຼດຮູບພາບໄດ້', true);
            }
            e.target.value = '';
        }

        function handleLogoUrlSave() {
            const url = $id('logoUrlInput').value.trim();
            if (!url) return showToast('ກະລຸນາປ້ອນ URL ຮູບໂລໂກ້', true);
            companyLogo = url;
            localStorage.setItem(STORAGE_KEYS.BRAND_LOGO, companyLogo);
            if (db) db.ref('eci_system/' + STORAGE_KEYS.BRAND_LOGO).set(companyLogo);
            applyBrandLogo();
            $id('logoUrlInput').value = '';
            showToast('ອັບເດດລິ້ງໂລໂກ້ບໍລິສັດແລ້ວ!');
        }

        function resetCompanyLogo() {
            companyLogo = DEFAULT_ECI_LOGO_SVG;
            localStorage.removeItem(STORAGE_KEYS.BRAND_LOGO);
            if (db) db.ref('eci_system/' + STORAGE_KEYS.BRAND_LOGO).remove();
            applyBrandLogo();
            showToast('ຄືນຄ່າໂລໂກ້ ECI 35 ປີ ແລ້ວ');
        }

        // =========================================================================
        // 5. 4-PERIOD SHIFT SCHEDULE HELPERS
        // =========================================================================
        function applyShiftTimeLabels() {
            const t = shiftTimes || DEFAULT_SHIFT_TIMES;
            if ($id('lblSummary1')) $id('lblSummary1').innerText = `ເຊົ້າ (${t.t1})`;
            if ($id('lblSummary2')) $id('lblSummary2').innerText = `ພັກສວຍ (${t.t2})`;
            if ($id('lblSummary3')) $id('lblSummary3').innerText = `ເຂົ້າສວຍ (${t.t3})`;
            if ($id('lblSummary4')) $id('lblSummary4').innerText = `ແລງ (${t.t4})`;

            if ($id('btnTime1')) $id('btnTime1').innerText = `ເວລາ ${t.t1}`;
            if ($id('btnTime2')) $id('btnTime2').innerText = `ເວລາ ${t.t2}`;
            if ($id('btnTime3')) $id('btnTime3').innerText = `ເວລາ ${t.t3}`;
            if ($id('btnTime4')) $id('btnTime4').innerText = `ເວລາ ${t.t4}`;

            if ($id('cfgTime1')) $id('cfgTime1').value = t.t1;
            if ($id('cfgTime2')) $id('cfgTime2').value = t.t2;
            if ($id('cfgTime3')) $id('cfgTime3').value = t.t3;
            if ($id('cfgTime4')) $id('cfgTime4').value = t.t4;
        }

        function saveShiftTimes() {
            shiftTimes = {
                t1: $id('cfgTime1').value || '08:00',
                t2: $id('cfgTime2').value || '12:00',
                t3: $id('cfgTime3').value || '12:30',
                t4: $id('cfgTime4').value || '17:00'
            };
            saveStore(STORAGE_KEYS.SHIFT_TIMES, shiftTimes);
            applyShiftTimeLabels();
            refreshAllViews();
            showToast('ບັນທຶກເວລາສະແກນ 4 ຊ່ວງສຳເລັດ!');
        }

        function getScanTypeMeta(type) {
            const t = shiftTimes || DEFAULT_SHIFT_TIMES;
            const map = {
                IN_MORNING: { label: `🌅 ເຂົ້າເຊົ້າ (${t.t1})`, short: 'ເຂົ້າເຊົ້າ', cls: 'bg-emerald-100 text-emerald-800' },
                OUT_NOON:   { label: `☀️ ອອກສວຍ (${t.t2})`, short: 'ອອກສວຍ', cls: 'bg-amber-100 text-amber-800' },
                IN_NOON:    { label: `🌤️ ເຂົ້າສວຍ (${t.t3})`, short: 'ເຂົ້າສວຍ', cls: 'bg-sky-100 text-sky-800' },
                OUT_EVENING:{ label: `🌆 ອອກແລງ (${t.t4})`, short: 'ອອກແລງ', cls: 'bg-rose-100 text-rose-800' },
                IN:         { label: 'ເຂົ້າວຽກ', short: 'ເຂົ້າວຽກ', cls: 'bg-emerald-100 text-emerald-800' },
                OUT:        { label: 'ອອກວຽກ', short: 'ອອກວຽກ', cls: 'bg-rose-100 text-rose-800' }
            };
            return map[type] || map.IN_MORNING;
        }

        // =========================================================================
        // 6. ADVERTISEMENT SYSTEM (POST-LOGIN BANNER + POPUP + ADMIN CRUD)
        // =========================================================================
        function getActiveAds() {
            return (companyAds || []).filter(a => a.active);
        }

        function renderPostLoginAds(triggerPopup = false) {
            const activeAds = getActiveAds();
            const bannerBox = $id('postLoginAdBanner');
            if (!bannerBox) return;

            const bannerAds = activeAds.filter(a => a.mode === 'banner' || a.mode === 'both');
            if (!bannerAds.length) {
                bannerBox.classList.add('hidden');
            } else {
                bannerBox.classList.remove('hidden');
                if (currentAdIdx >= bannerAds.length) currentAdIdx = 0;
                const ad = bannerAds[currentAdIdx];
                $id('adBannerImg').src = ad.image || DEFAULT_AD_BANNER_IMG;
                $id('adBannerBadge').innerText = ad.badge || '📢 ໂຄສະນາ';
                $id('adBannerTitle').innerText = ad.title;
                $id('adBannerDesc').innerText = ad.desc;
                $id('adBannerDots').innerHTML = bannerAds.map((_, i) =>
                    `<button type="button" onclick="goAdSlide(${i})" class="h-1.5 rounded-full transition-all ${i === currentAdIdx ? 'w-5 bg-blue-900' : 'w-1.5 bg-slate-300'}"></button>`
                ).join('');

                clearInterval(adCarouselTimer);
                if (bannerAds.length > 1) {
                    adCarouselTimer = setInterval(() => {
                        currentAdIdx = (currentAdIdx + 1) % bannerAds.length;
                        renderPostLoginAds(false);
                    }, 5000);
                }
            }

            if (triggerPopup) {
                const popupAd = activeAds.find(a => a.mode === 'popup' || a.mode === 'both');
                if (popupAd) openAdModal(popupAd);
            }
        }

        function prevAdSlide() {
            const bannerAds = getActiveAds().filter(a => a.mode === 'banner' || a.mode === 'both');
            if (!bannerAds.length) return;
            currentAdIdx = (currentAdIdx - 1 + bannerAds.length) % bannerAds.length;
            renderPostLoginAds(false);
        }

        function nextAdSlide() {
            const bannerAds = getActiveAds().filter(a => a.mode === 'banner' || a.mode === 'both');
            if (!bannerAds.length) return;
            currentAdIdx = (currentAdIdx + 1) % bannerAds.length;
            renderPostLoginAds(false);
        }

        function goAdSlide(idx) {
            currentAdIdx = idx;
            renderPostLoginAds(false);
        }

        function openCurrentAdDetail() {
            const bannerAds = getActiveAds().filter(a => a.mode === 'banner' || a.mode === 'both');
            if (bannerAds[currentAdIdx]) openAdModal(bannerAds[currentAdIdx]);
        }

        function openAdModal(ad) {
            $id('adPopupImg').src = ad.image || DEFAULT_AD_BANNER_IMG;
            $id('adPopupBadge').innerText = ad.badge || '📢 ປະກາດສຳຄັນ';
            $id('adPopupTitle').innerText = ad.title;
            $id('adPopupDesc').innerText = ad.desc;
            const linkEl = $id('adPopupLink');
            if (ad.link) {
                linkEl.href = ad.link;
                linkEl.classList.remove('hidden');
            } else {
                linkEl.classList.add('hidden');
            }
            $id('adPopupModal').classList.remove('hidden');
        }

        function closeAdPopup() {
            $id('adPopupModal').classList.add('hidden');
        }

        async function handleAdImageUpload(e) {
            const file = e.target.files[0];
            if (!file) return;
            try {
                const dataUrl = await compressImageFile(file, 560, 0.78);
                $id('adImgUrlInput').value = dataUrl;
                showToast('ໂຫຼດຮູບໂຄສະນາສຳເລັດ');
            } catch (err) {
                showToast('ບໍ່ສາມາດໂຫຼດຮູບໄດ້', true);
            }
            e.target.value = '';
        }

        function handleSaveAd(e) {
            e.preventDefault();
            const editId = $id('editAdId').value;
            const title = $id('adTitleInput').value.trim();
            const desc = $id('adDescInput').value.trim();
            const badge = $id('adBadgeInput').value;
            const mode = $id('adModeInput').value;
            const image = $id('adImgUrlInput').value.trim() || DEFAULT_AD_BANNER_IMG;
            const link = $id('adLinkInput').value.trim();
            const active = $id('adActiveInput').value === 'true';

            if (editId) {
                const idx = companyAds.findIndex(a => String(a.id) === String(editId));
                if (idx !== -1) companyAds[idx] = { ...companyAds[idx], title, desc, badge, mode, image, link, active };
            } else {
                companyAds.unshift({ id: Date.now(), title, desc, badge, mode, image, link, active });
            }

            saveStore(STORAGE_KEYS.ADS, companyAds);
            resetAdForm();
            refreshAllViews();
            showToast('ບັນທຶກໂຄສະນາສຳເລັດ!');
        }

        function editAd(id) {
            const ad = companyAds.find(a => String(a.id) === String(id));
            if (!ad) return;
            $id('editAdId').value = ad.id;
            $id('adTitleInput').value = ad.title;
            $id('adDescInput').value = ad.desc;
            $id('adBadgeInput').value = ad.badge || '📢 ໂຄສະນາ';
            $id('adModeInput').value = ad.mode || 'both';
            $id('adImgUrlInput').value = ad.image || '';
            $id('adLinkInput').value = ad.link || '';
            $id('adActiveInput').value = ad.active ? 'true' : 'false';
            $id('adFormTitle').innerHTML = `<i class="fa-solid fa-pen-to-square text-amber-500 mr-1.5"></i> ແກ້ໄຂໂຄສະນາ`;
            $id('cancelEditAdBtn').classList.remove('hidden');
        }

        function resetAdForm() {
            $id('adForm').reset();
            $id('editAdId').value = '';
            $id('adFormTitle').innerHTML = `<i class="fa-solid fa-bullhorn text-amber-500 mr-1.5"></i> ເພີ່ມໂຄສະນາ / ປະກາດຫຼັງ Login`;
            $id('cancelEditAdBtn').classList.add('hidden');
        }

        function toggleAdActive(id) {
            const ad = companyAds.find(a => String(a.id) === String(id));
            if (!ad) return;
            ad.active = !ad.active;
            saveStore(STORAGE_KEYS.ADS, companyAds);
            refreshAllViews();
            showToast(ad.active ? 'ເປີດສະແດງໂຄສະນາແລ້ວ' : 'ປິດການສະແດງໂຄສະນາຊົ່ວຄາວ');
        }

        function deleteAd(id) {
            companyAds = companyAds.filter(a => String(a.id) !== String(id));
            saveStore(STORAGE_KEYS.ADS, companyAds);
            refreshAllViews();
            showToast('ລຶບໂຄສະນາແລ້ວ');
        }

        function renderAdminAds() {
            const el = $id('adminAdList');
            if (!el) return;
            $id('adminAdCount').innerText = `${companyAds.length} ລາຍການ`;
            el.innerHTML = companyAds.length
                ? companyAds.map(ad => `
                    <div class="p-2.5 bg-slate-50 rounded-2xl border border-slate-200 space-y-2 text-xs">
                        <div class="flex gap-2.5">
                            <img src="${ad.image || DEFAULT_AD_BANNER_IMG}" class="w-16 h-14 rounded-xl object-cover shrink-0 border border-slate-200 bg-blue-950">
                            <div class="flex-1 min-w-0">
                                <div class="flex items-center justify-between gap-1">
                                    <span class="px-2 py-0.5 rounded-full bg-amber-100 text-amber-900 text-[9px] font-bold">${ad.badge}</span>
                                    <span class="px-2 py-0.5 rounded-full text-[9px] font-bold ${ad.active ? 'bg-emerald-100 text-emerald-700' : 'bg-slate-200 text-slate-600'}">${ad.active ? '🟢 ກຳລັງສະແດງ' : '⚪ ປິດໄວ້'}</span>
                                </div>
                                <h5 class="font-bold text-slate-900 truncate mt-1">${ad.title}</h5>
                                <p class="text-[10px] text-slate-500 line-clamp-1">${ad.desc}</p>
                            </div>
                        </div>
                        <div class="flex items-center gap-1.5 pt-1 border-t border-slate-200/70 text-[10px] font-bold">
                            <button type="button" onclick="toggleAdActive(${ad.id})" class="px-2 py-1 rounded-lg ${ad.active ? 'bg-slate-200 text-slate-800' : 'bg-emerald-600 text-white'}">${ad.active ? 'ປິດສະແດງ' : 'ເປີດສະແດງ'}</button>
                            <button type="button" onclick='openAdModal(${JSON.stringify(ad).replace(/'/g, "'")})' class="px-2 py-1 rounded-lg bg-purple-100 text-purple-900">ເບິ່ງຕົວຢ່າງ</button>
                            <button type="button" onclick="editAd(${ad.id})" class="px-2 py-1 rounded-lg bg-blue-100 text-blue-900"><i class="fa-solid fa-pen"></i></button>
                            <button type="button" onclick="deleteAd(${ad.id})" class="px-2 py-1 rounded-lg bg-rose-50 text-rose-600 ml-auto"><i class="fa-solid fa-trash"></i></button>
                        </div>
                    </div>`).join('')
                : '<p class="text-slate-400 text-xs text-center py-4">ຍັງບໍ່ມີລາຍການໂຄສະນາ</p>';
        }

        // =========================================================================
        // 7. FIREBASE LISTENERS & APP INITIALIZATION
        // =========================================================================
        function initCloudListeners() {
            const statusDot = $id('cloudStatusDot');
            const syncBadge = $id('syncBadge');
            if (!db) {
                if (statusDot) statusDot.innerHTML = '<i class="fa-solid fa-hard-drive text-amber-400"></i> Local';
                if (syncBadge) syncBadge.innerText = '● ໂໝດອອບໄລນ໌ (ໃສ່ Firebase Config ເພື່ອອອນໄລນ໌)';
                return;
            }

            db.ref('eci_system').on('value', (snapshot) => {
                const val = snapshot.val();
                if (!val) {
                    saveStore(STORAGE_KEYS.ACCOUNTS, accounts);
                    saveStore(STORAGE_KEYS.LOGS, attendanceLogs);
                    saveStore(STORAGE_KEYS.LEAVES, leaveRequests);
                    saveStore(STORAGE_KEYS.DUTIES, dutyList);
                    saveStore(STORAGE_KEYS.ADS, companyAds);
                    saveStore(STORAGE_KEYS.SHIFT_TIMES, shiftTimes);
                    return;
                }
                if (val[STORAGE_KEYS.ACCOUNTS]) { accounts = val[STORAGE_KEYS.ACCOUNTS]; localStorage.setItem(STORAGE_KEYS.ACCOUNTS, JSON.stringify(accounts)); }
                if (val[STORAGE_KEYS.LOGS]) { attendanceLogs = val[STORAGE_KEYS.LOGS]; localStorage.setItem(STORAGE_KEYS.LOGS, JSON.stringify(attendanceLogs)); }
                if (val[STORAGE_KEYS.LEAVES]) { leaveRequests = val[STORAGE_KEYS.LEAVES]; localStorage.setItem(STORAGE_KEYS.LEAVES, JSON.stringify(leaveRequests)); }
                if (val[STORAGE_KEYS.DUTIES]) { dutyList = val[STORAGE_KEYS.DUTIES]; localStorage.setItem(STORAGE_KEYS.DUTIES, JSON.stringify(dutyList)); }
                if (val[STORAGE_KEYS.ADMIN_PIN]) { adminPin = val[STORAGE_KEYS.ADMIN_PIN]; localStorage.setItem(STORAGE_KEYS.ADMIN_PIN, adminPin); }
                if (val[STORAGE_KEYS.BRAND_LOGO]) { companyLogo = val[STORAGE_KEYS.BRAND_LOGO]; localStorage.setItem(STORAGE_KEYS.BRAND_LOGO, companyLogo); applyBrandLogo(); }
                if (val[STORAGE_KEYS.ADS]) { companyAds = val[STORAGE_KEYS.ADS]; localStorage.setItem(STORAGE_KEYS.ADS, JSON.stringify(companyAds)); }
                if (val[STORAGE_KEYS.SHIFT_TIMES]) { shiftTimes = val[STORAGE_KEYS.SHIFT_TIMES]; localStorage.setItem(STORAGE_KEYS.SHIFT_TIMES, JSON.stringify(shiftTimes)); applyShiftTimeLabels(); }
                if (val[STORAGE_KEYS.FINANCE] && (val[STORAGE_KEYS.FINANCE].updated || 0) >= (fin.updated || 0)) { fin = val[STORAGE_KEYS.FINANCE]; fin.salaries = fin.salaries||{}; fin.payroll = fin.payroll||{}; fin.docs = fin.docs||[]; fin.ledger = fin.ledger||[]; localStorage.setItem(STORAGE_KEYS.FINANCE, JSON.stringify(fin)); if (typeof renderFinance === 'function') renderFinance(); } else if (fin.updated && typeof finSave === 'function') finSave();
                if (val[STORAGE_KEYS.CHECKPOINTS]) { checkpoints = val[STORAGE_KEYS.CHECKPOINTS]; localStorage.setItem(STORAGE_KEYS.CHECKPOINTS, JSON.stringify(checkpoints)); }
                refreshAllViews();
            });
        }

        document.addEventListener('DOMContentLoaded', () => {
            populateDeptSelect();
            applyBrandLogo();
            applyShiftTimeLabels();
            initCloudListeners();
            runSplashSequence();
            buildAuthForms();
            initApp();
            updateClock();
            setInterval(updateClock, 1000);
            $id('offsiteChk').addEventListener('change', (e) => {
                $id('offsiteNoteBox').classList.toggle('hidden', !e.target.checked);
            });
        });

        function runSplashSequence() {
            const splash = $id('splashScreen');
            const statusText = $id('splashStatusText');
            const bar = $id('splashProgressBar');
            if (!splash) return;
            splash.classList.remove('hidden', 'opacity-0', 'pointer-events-none');
            if (bar) {
                bar.classList.remove('animate-loadBar');
                void bar.offsetWidth;
                bar.classList.add('animate-loadBar');
            }
            setTimeout(() => {
                if (statusText) statusText.innerHTML = '<i class="fa-solid fa-circle-check text-emerald-400"></i> ພ້ອມໃຊ້ງານແລ້ວ';
            }, 1100);
            setTimeout(() => {
                splash.classList.add('opacity-0', 'pointer-events-none');
                setTimeout(() => splash.classList.add('hidden'), 600);
            }, 1700);
        }
        function replaySplash() { runSplashSequence(); }

        function showToast(message, isError = false) {
            const banner = $id('toastBanner');
            $id('toastMsg').innerText = message;
            $id('toastIcon').className = isError ? 'fa-solid fa-circle-exclamation text-rose-400 text-base' : 'fa-solid fa-circle-check text-emerald-400 text-base';
            banner.classList.remove('hidden');
            clearTimeout(toastTimeout);
            toastTimeout = setTimeout(() => banner.classList.add('hidden'), 3200);
        }
        function hideToast() { $id('toastBanner').classList.add('hidden'); }

        function updateClock() {
            const clockEl = $id('currentTime');
            if (clockEl) clockEl.innerText = new Date().toLocaleTimeString('en-GB', { hour: '2-digit', minute: '2-digit' });
        }

        function initApp() {
            if (currentUser) {
                const fresh = accounts.find(a => a.code === currentUser.code);
                if (fresh && fresh.status !== 'inactive') {
                    currentUser = fresh;
                    showDashboard(false);
                } else {
                    currentUser = null;
                    saveStore(STORAGE_KEYS.SESSION, null);
                    showLogin();
                }
            } else {
                showLogin();
            }
            refreshAllViews();
        }

        function refreshAllViews() {
            populateDeptSelect();
            renderUserLogs();
            renderMyLeaves();
            renderDuties();
            renderAdminEmployees();
            renderAdminLogs();
            renderAdminLeaves();
            renderAdminAds();
            renderPostLoginAds(false);
            renderCheckpointList();
            updateAdminStats();
            populateDutyEmployeeSelect();
        }

        function buildAuthForms() {
            const fld = (id, label, icon, type, ph, extra = '') =>
                `<div><label class="block text-xs font-semibold text-slate-700 mb-1">${label} <span class="text-red-500">*</span></label><div class="relative"><i class="fa-solid ${icon} absolute left-3.5 top-3 text-slate-400 text-xs"></i><input type="${type}" id="${id}" required placeholder="${ph}" ${extra} class="w-full pl-9 pr-3 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-900 text-xs"></div></div>`;
            const btn = (icon, text) =>
                `<button type="submit" class="w-full bg-blue-900 hover:bg-blue-800 text-white font-bold py-3 rounded-xl transition text-xs shadow-lg shadow-blue-200"><i class="fa-solid ${icon} mr-2"></i>${text}</button>`;

            $id('loginForm').innerHTML =
                fld('loginCode', 'ລະຫັດພະນັກງານ', 'fa-hashtag', 'text', 'ເຊັ່ນ ECI-135', 'autocomplete="username"') +
                fld('loginPw', 'ລະຫັດຜ່ານ', 'fa-lock', 'password', 'ປ້ອນລະຫັດຜ່ານ (123456)', 'autocomplete="current-password"') +
                btn('fa-right-to-bracket', 'ເຂົ້າສູ່ລະບົບ') +
                '<p class="text-[11px] text-slate-500 text-center">ຍັງບໍ່ມີບັນຊີ? <button type="button" onclick="authTab(\'signup\')" class="font-bold text-blue-900 underline">ສະໝັກສະມາຊິກ</button></p>';

            $id('signupForm').innerHTML =
                fld('suName', 'ຊື່ ແລະ ນາມສະກຸນ', 'fa-user', 'text', 'ຊື່ ແລະ ນາມສະກຸນ') +
                fld('suCode', 'ລະຫັດພະນັກງານ', 'fa-hashtag', 'text', 'ເຊັ່ນ ECI-138') +
                `<div><label class="block text-xs font-semibold text-slate-700 mb-1">ພະແນກ <span class="text-red-500">*</span></label>
                    <select id="suDept" class="w-full p-2.5 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                        <option value="ພະແນກເຈລະຈາທຸລະກິດ ແລະ ການຮ່ວມມື">ພະແນກເຈລະຈາທຸລະກິດ ແລະ ການຮ່ວມມື</option>
                        <option value="ພະແນກຈັດຕັ້ງ-ບໍລິຫານ">ພະແນກຈັດຕັ້ງ-ບໍລິຫານ</option>
                        <option value="ພະແນກເຕັກນິກ-ກໍ່ສ້າງ">ພະແນກເຕັກນິກ-ກໍ່ສ້າງ</option>
                        <option value="ພະແນກຝ່າຍປະຕິບັດການ O&M">ພະແນກຝ່າຍປະຕິບັດການ O&M</option>
                        <option value="ພະແນກການເງິນ-ບັນຊີ">ພະແນກການເງິນ-ບັນຊີ</option>
                        <option value="ພະແນກການສະຖາບັນ">ພະແນກການສະຖາບັນ</option>
                        <option value="ພະແນກ ຈັດຕັ້ງບໍລິຫານ ຫຼື Admin">ພະແນກ ຈັດຕັ້ງບໍລິຫານ ຫຼື Admin</option>
                    </select>
                </div>` +
                fld('suPos', 'ຕຳແໜ່ງ', 'fa-briefcase', 'text', 'ເຊັ່ນ: ວິຊາການ') +
                fld('suPhone', 'ເບີໂທຕິດຕໍ່', 'fa-phone', 'tel', '020 XXXXXXXX') +
                fld('suPw', 'ຕັ້ງລະຫັດຜ່ານ', 'fa-lock', 'password', 'ລະຫັດຜ່ານ', 'minlength="4" autocomplete="new-password"') +
                btn('fa-circle-plus', 'ສ້າງບັນຊີ ແລະ ບັນທຶກຂໍ້ມູນ') +
                '<p class="text-[11px] text-slate-500 text-center">ມີບັນຊີແລ້ວ? <button type="button" onclick="authTab(\'login\')" class="font-bold text-blue-900 underline">ເຂົ້າສູ່ລະບົບ</button></p>';
            authTab('login');
        }

        function authTab(t) {
            const isLogin = t === 'login';
            $id('loginForm').classList.toggle('hidden', !isLogin);
            $id('signupForm').classList.toggle('hidden', isLogin);
            $id('authTitle').innerText = isLogin ? 'ເຂົ້າສູ່ລະບົບພະນັກງານ' : 'ສະໝັກສະມາຊິກພະນັກງານ';
            const on = 'bg-white text-blue-900 shadow', off = 'text-slate-500';
            $id('tabLogin').className = 'py-2.5 rounded-xl transition ' + (isLogin ? on : off);
            $id('tabSignup').className = 'py-2.5 rounded-xl transition ' + (isLogin ? off : on);
        }

        function handleSignup(e) {
            e.preventDefault();
            const v = id => $id(id).value.trim();
            const name = v('suName'), code = v('suCode').toUpperCase(), dept = v('suDept'), position = v('suPos'), phone = v('suPhone'), pw = $id('suPw').value;
            if (!name || !code || !phone) return showToast('ກະລຸນາປ້ອນຂໍ້ມູນໃຫ້ຄົບຖ້ວນ!', true);
            if (pw.length < 4) return showToast('ລະຫັດຜ່ານຕ້ອງມີຢ່າງໜ້ອຍ 4 ຕົວ', true);
            if (accounts.some(a => a.code.toUpperCase() === code)) return showToast('ລະຫັດພະນັກງານນີ້ມີໃນລະບົບແລ້ວ', true);
            accounts.unshift({ id: code, code, name, dept, position, phone, pw, status: 'active' });
            saveStore(STORAGE_KEYS.ACCOUNTS, accounts);
            refreshAllViews();
            e.target.reset();
            authTab('login');
            $id('loginCode').value = code;
            showToast('ສະໝັກສຳເລັດ! ຂໍ້ມູນຖືກສົ່ງຂຶ້ນລະບົບອອນໄລນ໌ແລ້ວ');
        }

        function handleLogin(e) {
            e.preventDefault();
            const code = $id('loginCode').value.trim().toUpperCase();
            const pw = $id('loginPw').value;
            const acc = accounts.find(a => a.code.toUpperCase() === code);
            if (!acc || acc.pw !== pw) return showToast('ລະຫັດພະນັກງານ ຫຼື ລະຫັດຜ່ານບໍ່ຖືກຕ້ອງ', true);
            if (acc.status === 'inactive') return showToast('ບັນຊີນີ້ຖືກລະງັບການໃຊ້ງານຊົ່ວຄາວ', true);
            currentUser = acc;
            saveStore(STORAGE_KEYS.SESSION, currentUser);
            $id('loginPw').value = '';
            showDashboard(true);
            showToast(`ຍິນດີຕ້ອນຮັບ ${acc.name}`);
        }

        function quickDemoLogin() {
            const acc = accounts.find(a => a.code === 'ECI-135') || accounts[0];
            if (!acc) return;
            currentUser = acc;
            saveStore(STORAGE_KEYS.SESSION, currentUser);
            showDashboard(true);
            showToast(`ເຂົ້າສູ່ລະບົບ: ${acc.name}`);
        }

        function logout() {
            stopCam();
            clearInterval(adCarouselTimer);
            currentUser = null;
            saveStore(STORAGE_KEYS.SESSION, null);
            showLogin();
            showToast('ອອກຈາກລະບົບແລ້ວ');
        }

        function showLogin() {
            $id('secLogin').classList.remove('hidden');
            $id('secDashboard').classList.add('hidden');
            $id('secAdmin').classList.add('hidden');
            $id('secAdminPin').classList.add('hidden');
            $id('bottomNav').classList.add('hidden');
            $id('headerSubtext').innerText = 'ລະບົບບໍລິຫານພະນັກງານ ແລະ ເວນຍາມ';
        }

        function showDashboard(isFreshLogin = false) {
            $id('secLogin').classList.add('hidden');
            $id('secDashboard').classList.remove('hidden');
            $id('secAdmin').classList.add('hidden');
            $id('secAdminPin').classList.add('hidden');
            $id('bottomNav').classList.remove('hidden');

            $id('profileName').innerText = currentUser.name;
            $id('profileCode').innerText = `ID: ${currentUser.code} ${currentUser.position ? '• ' + currentUser.position : ''}`;
            $id('profileDept').innerText = currentUser.dept || 'ລັດວິສາຫະກິດກໍ່ສ້າງ ແລະ ຕິດຕັ້ງໄຟຟ້າ';
            $id('profilePhone').innerHTML = `<i class="fa-solid fa-phone mr-1 text-amber-400"></i> ${currentUser.phone}`;
            $id('avatarChar').innerText = currentUser.name.replace(/^(ທ່ານ|ທ້າວ|ນາງ)\s*/i, '').charAt(0) || currentUser.name.charAt(0);
            $id('headerSubtext').innerText = `ພະນັກງານ: ${currentUser.code}`;

            renderUserLogs();
            renderMyLeaves();
            renderPostLoginAds(isFreshLogin);
            switchTab('checkin');
        }

        function openBadgeModal() { if (currentUser) showBadgeForEmployee(currentUser); }
        function showBadgeForEmployee(emp) {
            $id('badgeAvatar').innerText = emp.name.replace(/^(ທ່ານ|ທ້າວ|ນາງ)\s*/i, '').charAt(0) || emp.name.charAt(0);
            $id('badgeName').innerText = emp.name;
            $id('badgeCode').innerText = emp.code;
            $id('badgeDept').innerText = emp.dept || '-';
            $id('badgePos').innerText = emp.position || '-';
            $id('badgePhone').innerText = emp.phone || '-';
            const qrBox = $id('personalQrBox');
            qrBox.innerHTML = '';
            if (window.QRCode) new QRCode(qrBox, { text: `EMP|${emp.code}`, width: 168, height: 168, correctLevel: QRCode.CorrectLevel.M });
            $id('badgeModal').classList.remove('hidden');
        }
        function closeBadgeModal() { $id('badgeModal').classList.add('hidden'); }

        function switchTab(tab) {
            ['subTabCheckin', 'subTabDuty', 'subTabLeave'].forEach(id => $id(id).classList.add('hidden'));
            ['tabBtnCheckin', 'tabBtnDuty', 'tabBtnLeave'].forEach(id => $id(id).className = 'py-2.5 rounded-xl transition text-slate-600 hover:text-slate-900 flex items-center justify-center gap-1');
            ['navBtnCheckin', 'navBtnDuty', 'navBtnLeave'].forEach(id => $id(id).className = 'flex flex-col items-center text-slate-400 hover:text-blue-900');

            if (tab === 'checkin') {
                $id('subTabCheckin').classList.remove('hidden');
                $id('tabBtnCheckin').className = 'py-2.5 rounded-xl transition shadow bg-white text-blue-900 flex items-center justify-center gap-1 font-bold';
                $id('navBtnCheckin').className = 'flex flex-col items-center text-blue-900 font-bold';
            } else if (tab === 'duty') {
                $id('subTabDuty').classList.remove('hidden');
                $id('tabBtnDuty').className = 'py-2.5 rounded-xl transition shadow bg-white text-blue-900 flex items-center justify-center gap-1 font-bold';
                $id('navBtnDuty').className = 'flex flex-col items-center text-blue-900 font-bold';
            } else if (tab === 'leave') {
                $id('subTabLeave').classList.remove('hidden');
                $id('tabBtnLeave').className = 'py-2.5 rounded-xl transition shadow bg-white text-blue-900 flex items-center justify-center gap-1 font-bold';
                $id('navBtnLeave').className = 'flex flex-col items-center text-blue-900 font-bold';
            }
        }

        // =========================================================================
        // 8. CAMERA, QR SCANNER & GPS ATTENDANCE (4 SHIFTS)
        // =========================================================================
        let camStream = null, camRaf = 0;
        const curType = () => (document.querySelector('input[name="checkType"]:checked') || {}).value || 'IN_MORNING';

        const cpQr = cp => 'ECI-CP|' + cp.id;
        /* ---- QR decoding: native BarcodeDetector first, jsQR as fallback; downscaled + reusable canvas ---- */
        const _qc = document.createElement('canvas'), _qx = _qc.getContext('2d', { willReadFrequently: true });
        let _bd = null; try { if ('BarcodeDetector' in window) _bd = new BarcodeDetector({ formats: ['qr_code'] }); } catch (e) {}
        async function decodeQr(src, w, h, maxDim = 720) {
            if (_bd) { try { const r = await _bd.detect(src); if (r && r.length) return r[0].rawValue; } catch (e) {} }
            if (!window.jsQR || !w || !h) return null;
            const sc = Math.min(1, maxDim / Math.max(w, h)); _qc.width = Math.round(w * sc); _qc.height = Math.round(h * sc);
            _qx.drawImage(src, 0, 0, _qc.width, _qc.height);
            const d = _qx.getImageData(0, 0, _qc.width, _qc.height), r = jsQR(d.data, d.width, d.height, { inversionAttempts: 'attemptBoth' });
            return r ? r.data : null;
        }
        function camErrMsg(e) {
            if (!window.isSecureContext || !navigator.mediaDevices) return 'ກ້ອງໃຊ້ບໍ່ໄດ້ ເພາະເປີດບໍ່ຜ່ານ HTTPS — ໃຫ້ໃຊ້ "ສະແກນຈາກຮູບ" ຫຼື "ໃສ່ລະຫັດຈຸດ" ດ້ານລຸ່ມແທນ';
            const n = e && e.name;
            if (n === 'NotAllowedError' || n === 'SecurityError') return 'ບໍ່ໄດ້ອະນຸຍາດໃຫ້ໃຊ້ກ້ອງ — ເປີດສິດກ້ອງໃຫ້ເວັບນີ້ໃນການຕັ້ງຄ່າບຣາວເຊີ ແລ້ວລອງໃໝ່';
            if (n === 'NotFoundError' || n === 'OverconstrainedError') return 'ບໍ່ພົບກ້ອງໃນອຸປະກອນນີ້ — ໃຊ້ "ສະແກນຈາກຮູບ" ແທນ';
            if (n === 'NotReadableError' || n === 'AbortError') return 'ກ້ອງຖືກແອັບອື່ນໃຊ້ຢູ່ — ປິດແອັບນັ້ນແລ້ວລອງໃໝ່';
            return 'ເປີດກ້ອງບໍ່ໄດ້ — ລອງ "ສະແກນຈາກຮູບ" ຫຼື "ໃສ່ລະຫັດຈຸດ" ແທນ';
        }
        function scanFromImage(inp) {
            const f = inp.files[0]; inp.value = ''; if (!f) return;
            if ($id('offsiteChk').checked) return showToast('ວຽກນອກສະຖານທີ່ໃຊ້ປຸ່ມ "ເປີດກ້ອງ ຖ່າຍຮູບ"', true);
            const url = URL.createObjectURL(f), im = new Image();
            im.onerror = () => { URL.revokeObjectURL(url); showToast('ເປີດຮູບບໍ່ໄດ້', true); };
            im.onload = async () => {
                const data = await decodeQr(im, im.naturalWidth, im.naturalHeight, 1200); URL.revokeObjectURL(url);
                if (!data) return showToast('ບໍ່ພົບ QR ໃນຮູບ — ຖ່າຍໃໝ່ໃຫ້ QR ຢູ່ກາງຮູບ, ຊັດ ແລະ ມີແສງພໍ', true);
                const c = document.createElement('canvas'), w = 320; c.width = w; c.height = Math.round(im.naturalHeight * w / im.naturalWidth);
                c.getContext('2d').drawImage(im, 0, 0, c.width, c.height);
                handleScannedCheckpointQr(data, c.toDataURL('image/jpeg', 0.45), ' · ສະແກນຈາກຮູບ');
            };
            im.src = url;
        }
        function manualCp() { const v = $id('manualCp').value.trim(); if (!v) return showToast('ກະລຸນາໃສ່ລະຫັດຈຸດ ເຊັ່ນ ECI-35-GATE-01', true); handleScannedCheckpointQr(v, null, ' · ປ້ອນລະຫັດເອງ'); $id('manualCp').value = ''; }

        function getGps() {
            return new Promise(res => {
                if (!navigator.geolocation) return res(null);
                navigator.geolocation.getCurrentPosition(
                    p => res({ lat: p.coords.latitude, lng: p.coords.longitude, acc: Math.round(p.coords.accuracy) }),
                    () => res(null),
                    { enableHighAccuracy: true, timeout: 8000 }
                );
            });
        }

        function grabPhoto() {
            const v = $id('camVid');
            if (!v || !v.videoWidth) return null;
            const w = 320, h = Math.round(v.videoHeight * w / v.videoWidth), c = document.createElement('canvas');
            c.width = w; c.height = h;
            c.getContext('2d').drawImage(v, 0, 0, w, h);
            return c.toDataURL('image/jpeg', 0.45);
        }

        function stopCam() {
            cancelAnimationFrame(camRaf);
            if (camStream) { camStream.getTracks().forEach(t => t.stop()); camStream = null; }
            $id('camBox').classList.add('hidden');
            $id('qrIdle').classList.remove('hidden');
            $id('camBtnTxt').innerText = $id('offsiteChk').checked ? 'ເປີດກ້ອງ ຖ່າຍຮູບ' : 'ສະແກນ QR + ຖ່າຍຮູບ';
        }

        async function startCam() {
            const off = $id('offsiteChk').checked;
            if (camStream) {
                if (off) {
                    const ph = grabPhoto();
                    stopCam();
                    const note = $id('offsiteNote').value.trim();
                    addLog(curType(), note ? `ວຽກນອກສະຖານທີ່: ${note}` : 'ວຽກນອກສະຖານທີ່', ph, true);
                }
                return;
            }
            try {
                if (!navigator.mediaDevices || !window.isSecureContext) throw { name: 'Insecure' };
                camStream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: { ideal: 'environment' }, width: { ideal: 1280 }, height: { ideal: 720 } }, audio: false });
            } catch (e) {
                return showToast(camErrMsg(e), true);
            }
            if (!off && !window.jsQR && !_bd) { stopCam(); return showToast('ໂຕອ່ານ QR ໂຫຼດບໍ່ໄດ້ (ກວດອິນເຕີເນັດ) — ໃຊ້ "ໃສ່ລະຫັດຈຸດ" ແທນ', true); }
            const v = $id('camVid');
            v.muted = true; v.setAttribute('playsinline', ''); v.srcObject = camStream;
            try { await v.play(); } catch (e) {}
            $id('camBox').classList.remove('hidden');
            $id('qrIdle').classList.add('hidden');
            $id('camFrame').classList.toggle('hidden', off);
            $id('camBtnTxt').innerText = off ? 'ຖ່າຍຮູບ ແລະ ບັນທຶກ' : 'ກຳລັງສະແກນ QR…';

            if (!off) {
                const mine = camStream; let busy = false, last = 0;
                setTimeout(() => { if (camStream === mine) showToast('ຍັງອ່ານບໍ່ໄດ້? ລອງຂະຍັບໃກ້/ຫ່າງ 20–30 ຊມ., ເພີ່ມແສງ ຫຼື ໃຊ້ "ສະແກນຈາກຮູບ"', true); }, 12000);
                (function loop(ts) {
                    if (camStream !== mine) return;
                    if (!busy && ts - last > 120 && v.readyState >= 2 && v.videoWidth) {
                        busy = true; last = ts;
                        decodeQr(v, v.videoWidth, v.videoHeight).then(data => {
                            busy = false;
                            if (data && camStream === mine) { const ph = grabPhoto(); stopCam(); handleScannedCheckpointQr(data, ph); }
                        }).catch(() => { busy = false; });
                    }
                    camRaf = requestAnimationFrame(loop);
                })(0);
            }
        }

        function matchAuthorizedCheckpoint(rawText) {
            const cleaned = String(rawText || '').trim();
            if (!cleaned) return null;
            return checkpoints.find(cp =>
                cp.payload === cleaned ||
                cp.id.toUpperCase() === cleaned.toUpperCase() ||
                cp.name.toUpperCase() === cleaned.toUpperCase() ||
                (cleaned.startsWith('ECI-CP|') && cleaned.split('|')[1]?.toUpperCase() === cp.id.toUpperCase())
            ) || null;
        }

        function handleScannedCheckpointQr(rawQrText, photoData, note = '') {
            const matchedCp = matchAuthorizedCheckpoint(rawQrText);
            if (!matchedCp) {
                showScanResultModal(false, null, rawQrText);
                return;
            }
            addLog(curType(), `${matchedCp.id} (${matchedCp.name})${note}`, photoData, false, null, true);
        }

        function showScanResultModal(isSuccess, entry = null, invalidRaw = '') {
            const modal = $id('scanResultModal');
            const header = $id('scanResultHeader');
            const icon = $id('scanResultIcon');
            const title = $id('scanResultTitle');
            const sub = $id('scanResultSub');
            const successBox = $id('scanSuccessDetails');
            const errBox = $id('scanErrorDetails');
            const retryBtn = $id('scanRetryBtn');

            if (isSuccess && entry) {
                header.className = 'bg-gradient-to-r from-emerald-600 to-teal-700 text-white p-5 text-center relative';
                icon.className = 'w-16 h-16 mx-auto mb-2 rounded-full bg-white text-emerald-600 flex items-center justify-center text-3xl shadow-lg animate-bounce';
                icon.innerHTML = '<i class="fa-solid fa-circle-check"></i>';
                title.innerText = 'ສະແກນ QR ຈຸດກວດສຳເລັດ!';
                sub.innerText = 'ຢືນຢັນປ້າຍ QR ຂອງ Admin ຖືກຕ້ອງ • ບັນທຶກເວລາແລ້ວ';

                const meta = getScanTypeMeta(entry.type);
                $id('resEmpName').innerText = `${entry.empName} (${entry.empCode})`;
                $id('resShiftBadge').className = `px-2 py-0.5 rounded-full text-[10px] font-bold ${meta.cls}`;
                $id('resShiftBadge').innerText = meta.label;
                $id('resTime').innerText = `${entry.date} ເວລາ ${entry.time}`;
                $id('resCheckpoint').innerText = entry.location;
                $id('resGps').innerText = `${Number(entry.lat).toFixed(5)}, ${Number(entry.lng).toFixed(5)}`;

                if (entry.photo) {
                    $id('resPhotoImg').src = entry.photo;
                    $id('resPhotoWrap').classList.remove('hidden');
                } else {
                    $id('resPhotoWrap').classList.add('hidden');
                }

                successBox.classList.remove('hidden');
                errBox.classList.add('hidden');
                retryBtn.classList.add('hidden');
            } else {
                header.className = 'bg-gradient-to-r from-rose-600 to-red-700 text-white p-5 text-center relative';
                icon.className = 'w-16 h-16 mx-auto mb-2 rounded-full bg-white text-rose-600 flex items-center justify-center text-3xl shadow-lg';
                icon.innerHTML = '<i class="fa-solid fa-triangle-exclamation"></i>';
                title.innerText = 'QR Code ບໍ່ຖືກຕ້ອງ!';
                sub.innerText = 'ບໍ່ສາມາດບັນທຶກເວລາໄດ້ ເນື່ອງຈາກບໍ່ແມ່ນ QR ຈຸດກວດ Admin';
                $id('scanErrorRaw').innerText = `ຂໍ້ມູນທີ່ສະແກນພົບ: ${invalidRaw || '(ບໍ່ຮູ້ຈັກ)'}`;

                successBox.classList.add('hidden');
                errBox.classList.remove('hidden');
                retryBtn.classList.remove('hidden');
            }
            modal.classList.remove('hidden');
        }

        function closeScanResultModal(restartCamera = false) {
            $id('scanResultModal').classList.add('hidden');
            if (restartCamera) startCam();
        }

        async function addLog(type, location, photo, offsite, overrideUser = null, openModal = false) {
            const targetUser = overrideUser || currentUser;
            if (!targetUser) return;
            const meta = getScanTypeMeta(type);
            window._logBusy = window._logBusy || {};
            if (window._logBusy[targetUser.code]) return false;
            if (!offsite) {
                const today = new Date().toLocaleDateString('en-GB');
                const dup = attendanceLogs.find(l => l.empCode === targetUser.code && l.type === type && l.date === today && !l.offsite);
                if (dup) { showToast(`${targetUser.name} ໄດ້ບັນທຶກ ${meta.short} ແລ້ວວັນນີ້ ເວລາ ${dup.time} — ບໍ່ບັນທຶກຊ້ຳ`, true); return false; }
                const recent = attendanceLogs.find(l => l.empCode === targetUser.code && Date.now() - l.id < 60000);
                if (recent) { showToast('ສະແກນຖີ່ເກີນໄປ — ກະລຸນາລໍຖ້າ 1 ນາທີ', true); return false; }
            }
            window._logBusy[targetUser.code] = true;
            showToast(`ກຳລັງບັນທຶກ ${meta.short} ແລະ ພິກັດ GPS…`);
            const g = await getGps();
            const now = new Date();
            const entry = {
                id: Date.now(),
                empName: targetUser.name,
                empCode: targetUser.code,
                empPhone: targetUser.phone,
                type,
                location,
                offsite: !!offsite,
                photo: photo || null,
                lat: g ? g.lat : 17.9757,
                lng: g ? g.lng : 102.6331,
                acc: g ? g.acc : null,
                date: now.toLocaleDateString('en-GB'),
                time: now.toLocaleTimeString('en-GB')
            };

            attendanceLogs.unshift(entry); window._logBusy[targetUser.code] = false;
            saveStore(STORAGE_KEYS.LOGS, attendanceLogs);
            if (!overrideUser) setGpsUi(g);
            showToast(`ບັນທຶກ ${meta.short} (${targetUser.name}) ສຳເລັດ!`);
            refreshAllViews();
            if (openModal) showScanResultModal(true, entry);
        }
        function simulateValidCheckpointScan() {
            const off = $id('offsiteChk').checked;
            if (off) {
                const note = $id('offsiteNote').value.trim();
                const loc = note ? `ວຽກນອກສະຖານທີ່: ${note}` : 'ວຽກນອກສະຖານທີ່ (GPS)';
                addLog(curType(), loc, null, true, null, true);
                return;
            }
            const cp = checkpoints[0] || DEFAULT_CHECKPOINTS[0];
            handleScannedCheckpointQr(cp.payload, null);
        }

        const mapsUrl = g => `https://www.google.com/maps?q=${g.lat},${g.lng}`;
        function setGpsUi(g) {
            $id('gpsTxt').innerHTML = g
                ? `GPS: <a href="${mapsUrl(g)}" target="_blank" class="text-blue-700 underline font-semibold">${g.lat.toFixed(5)}, ${g.lng.toFixed(5)}</a> (±${g.acc} ມ.)`
                : 'GPS: ບັນທຶກພິກັດສຳນັກງານ ECI';
        }

        async function sendLocation(mode) {
            if (!currentUser) return;
            const g = await getGps();
            if (!g) return showToast('ອ່ານຕຳແໜ່ງບໍ່ໄດ້ ກະລຸນາເປີດ GPS', true);
            setGpsUi(g);
            const text = `📍 ${currentUser.name} (${currentUser.code})\nທີ່ຢູ່ປັດຈຸບັນ: ${mapsUrl(g)}`;
            if (mode === 'wa') { window.open('https://wa.me/?text=' + encodeURIComponent(text), '_blank'); return; }
            if (mode === 'share' && navigator.share) { try { await navigator.share({ title: 'ທີ່ຢູ່ Google Map', text }); return; } catch (e) {} }
            await navigator.clipboard.writeText(text);
            showToast('ສຳເນົາລິ້ງ Google Map ແລ້ວ');
        }

        const logExtra = l => `${l.offsite ? '<span class="px-1.5 py-0.5 rounded-full text-[10px] font-bold bg-purple-100 text-purple-800 mr-1">🚗 ນອກສະຖານທີ່</span>' : ''}${l.lat != null ? `<a href="https://www.google.com/maps?q=${l.lat},${l.lng}" target="_blank" class="text-[10px] text-blue-700 underline"><i class="fa-solid fa-location-dot"></i> ${Number(l.lat).toFixed(4)}, ${Number(l.lng).toFixed(4)}</a>` : ''}`;
        const logPhoto = l => l.photo ? `<img src="${l.photo}" class="w-14 h-14 rounded-lg object-cover flex-none border border-slate-200">` : '';
        const badge = l => {
            const m = getScanTypeMeta(l.type);
            return `<span class="px-2 py-0.5 rounded text-[10px] font-bold ${m.cls}">${m.label}</span>`;
        };
        const kindBadge = d => d.kind === 'offsite' ? '<span class="ml-1 px-1.5 py-0.5 rounded-full text-[10px] font-bold bg-purple-100 text-purple-800">🚗 ນອກສະຖານທີ່</span>' : '';

        function renderUserLogs() {
            if (!currentUser) return;
            const todayStr = new Date().toLocaleDateString('en-GB');
            const my = attendanceLogs.filter(l => l.empCode === currentUser.code);
            const myToday = my.filter(l => l.date === todayStr);
            const reversedToday = [...myToday].reverse();

            const s1 = reversedToday.find(l => l.type === 'IN_MORNING' || l.type === 'IN');
            const s2 = reversedToday.find(l => l.type === 'OUT_NOON');
            const s3 = reversedToday.find(l => l.type === 'IN_NOON');
            const s4 = myToday.find(l => l.type === 'OUT_EVENING' || l.type === 'OUT');

            $id('todaySlot1').innerText = s1 ? s1.time.slice(0, 5) : '--:--';
            $id('todaySlot2').innerText = s2 ? s2.time.slice(0, 5) : '--:--';
            $id('todaySlot3').innerText = s3 ? s3.time.slice(0, 5) : '--:--';
            $id('todaySlot4').innerText = s4 ? s4.time.slice(0, 5) : '--:--';

            const countDone = [s1, s2, s3, s4].filter(Boolean).length;
            const targetMorning = (shiftTimes?.t1 || '08:00') + ':59';
            if (s1) {
                const isLate = s1.time > targetMorning;
                $id('todayStatusBadge').innerText = `${isLate ? '⚠️ ມາຊ້າ' : '✅ ທັນເວລາ'} (${countDone}/4 ຊ່ວງ)`;
            } else {
                $id('todayStatusBadge').innerText = `ຍັງບໍ່ໄດ້ສະແກນ (0/4)`;
            }

            $id('myLogCount').innerText = `${my.length} ລາຍການ`;
            $id('userLogHistory').innerHTML = my.length
                ? my.slice(0, 12).map(l => `<div class="flex items-center gap-2 p-2.5 bg-slate-50 rounded-xl border border-slate-100">${logPhoto(l)}<div class="flex-1 min-w-0"><div class="flex items-center justify-between gap-2">${badge(l)}<span class="text-slate-700 font-semibold text-[11px]">${l.date} • ${l.time}</span></div><p class="text-[11px] text-slate-600 truncate mt-0.5">${l.location}</p><div class="mt-0.5">${logExtra(l)}</div></div></div>`).join('')
                : '<p class="text-slate-400 text-[11px] text-center py-3">ຍັງບໍ່ມີປະຫວັດການເຂົ້າ-ອອກວຽກ</p>';
        }

        // =========================================================================
        // 9. LEAVE & DUTY MANAGEMENT
        // =========================================================================
        function handleLeaveSubmit(e) {
            e.preventDefault();
            leaveRequests.unshift({
                id: Date.now(),
                empName: currentUser.name,
                empCode: currentUser.code,
                empPhone: currentUser.phone,
                type: $id('leaveType').value,
                start: $id('leaveStart').value,
                end: $id('leaveEnd').value,
                reason: $id('leaveReason').value.trim(),
                status: 'ລໍຖ້າອະນຸມັດ',
                timestamp: new Date().toLocaleString('en-GB')
            });
            saveStore(STORAGE_KEYS.LEAVES, leaveRequests);
            $id('leaveReason').value = '';
            showToast('ສົ່ງໃບຂໍລາພັກໃຫ້ຝ່າຍຈັດຕັ້ງ (Admin) ສຳເລັດ!');
            refreshAllViews();
        }

        function renderMyLeaves() {
            const el = $id('myLeaveList');
            if (!el || !currentUser) return;
            const my = leaveRequests.filter(l => l.empCode === currentUser.code);
            el.innerHTML = my.length
                ? my.map(l => `<div class="flex justify-between items-center p-2.5 bg-slate-50 rounded-xl border border-slate-100 text-[11px]"><div><strong class="text-slate-800">${l.type}</strong><span class="block text-slate-500 text-[10px]">${l.start} → ${l.end}</span></div><span class="px-2 py-0.5 rounded-full text-[10px] font-bold ${l.status.includes('ອະນຸມັດແລ້ວ') ? 'bg-emerald-100 text-emerald-800' : l.status.includes('ປະຕິເສດ') ? 'bg-rose-100 text-rose-800' : 'bg-amber-100 text-amber-800'}">${l.status}</span></div>`).join('')
                : '<p class="text-slate-400 text-[11px] text-center py-2">ຍັງບໍ່ມີໃບລາ</p>';
        }

        function filterMyShift(type) {
            currentFilter = type;
            const map = { all: 'filterAll', 'ເຊົ້າ': 'filterMorning', 'ບ່າຍ': 'filterAfternoon', 'ເດິກ': 'filterNight' };
            Object.entries(map).forEach(([k, id]) => {
                if ($id(id)) $id(id).className = k === type ? 'py-1.5 bg-blue-900 text-white rounded-xl text-[10px] font-bold shadow' : 'py-1.5 bg-slate-100 text-slate-700 rounded-xl text-[10px] font-medium';
            });
            renderDuties();
        }

        function renderDuties() {
            const filtered = currentFilter === 'all' ? dutyList : dutyList.filter(d => d.shift.includes(currentFilter));
            $id('myDutyList').innerHTML = filtered.length
                ? filtered.map(d => `<div class="bg-slate-50 p-3.5 rounded-2xl border border-slate-200/80 space-y-1.5"><div class="flex justify-between items-start gap-2"><span class="font-bold text-slate-800 text-xs"><i class="fa-solid fa-user-tie text-blue-600 mr-1"></i> ${d.empName}</span><span class="px-2 py-0.5 rounded-full text-[10px] font-bold bg-amber-100 text-amber-800 shrink-0">${d.shift}</span></div><div class="text-[11px] text-slate-600"><i class="fa-regular fa-calendar mr-1.5 text-slate-400"></i> ວັນທີ: <strong>${d.date}</strong></div><div class="text-[11px] text-slate-600"><i class="fa-solid fa-location-dot mr-1.5 text-rose-500"></i> ຈຸດ: ${d.location} ${kindBadge(d)}</div></div>`).join('')
                : '<div class="bg-slate-50 p-6 rounded-2xl text-center text-slate-400 text-xs">ຍັງບໍ່ມີຕາຕະລາງເວນຍາມ</div>';

            $id('adminDutyList').innerHTML = dutyList.length
                ? dutyList.map(d => `<div class="p-2.5 bg-white rounded-xl border border-slate-200 flex justify-between items-center text-xs"><div><span class="font-bold text-slate-800">${d.empName}</span><p class="text-[10px] text-slate-500">${d.date} | ${d.shift}</p><p class="text-[10px] text-blue-900 font-medium">${d.location} ${kindBadge(d)}</p></div><button onclick="deleteDuty(${d.id})" class="text-rose-500 p-1.5"><i class="fa-solid fa-trash-can text-xs"></i></button></div>`).join('')
                : '<p class="text-slate-400 text-center py-3 text-xs">ຍັງບໍ່ມີຂໍ້ມູນເວນຍາມ</p>';
        }

        function populateDutyEmployeeSelect() {
            const sel = $id('dutyEmpSelect');
            if (!sel) return;
            sel.innerHTML = '<option value="">-- ເລືອກຈາກລາຍຊື່ພະນັກງານ --</option>' + accounts.map(a => `<option value="${a.name}">${a.code} - ${a.name}</option>`).join('');
        }
        function syncDutyEmpInput() { if ($id('dutyEmpSelect').value) $id('dutyEmpName').value = $id('dutyEmpSelect').value; }

        function handleAssignDuty(e) {
            e.preventDefault();
            dutyList.unshift({
                id: Date.now(),
                empName: $id('dutyEmpName').value.trim(),
                date: $id('dutyDate').value,
                shift: $id('dutyShift').value,
                location: $id('dutyLocation').value.trim(),
                kind: $id('dutyKind').value
            });
            saveStore(STORAGE_KEYS.DUTIES, dutyList);
            $id('dutyEmpName').value = '';
            $id('dutyLocation').value = '';
            showToast('ມອບໝາຍເວນຍາມສຳເລັດແລ້ວ!');
            refreshAllViews();
        }

        function deleteDuty(id) {
            dutyList = dutyList.filter(d => d.id !== id);
            saveStore(STORAGE_KEYS.DUTIES, dutyList);
            refreshAllViews();
            showToast('ລຶບຕາຕະລາງເວນແລ້ວ');
        }

        // =========================================================================
        // 10. ADMIN DASHBOARD, EMPLOYEES, LOGS & QR GATE
        // =========================================================================
        function updateAdminStats() {
            const todayStr = new Date().toLocaleDateString('en-GB');
            $id('statTotalEmp').innerText = accounts.length;
            $id('statTodayIn').innerText = attendanceLogs.filter(l => l.date === todayStr).length;
            $id('statActiveAds').innerText = getActiveAds().length;
            const pend = leaveRequests.filter(l => l.status.includes('ລໍຖ້າ')).length;
            $id('statPendingLeaves').innerText = pend;
            const lb = $id('leaveBadge'); if (lb) { lb.textContent = pend; lb.classList.toggle('hidden', !pend); }
        }

        function handleSaveEmployee(e) {
            e.preventDefault();
            const origCode = $id('editEmpOriginalCode').value.trim();
            const code = $id('empCodeInput').value.trim().toUpperCase();
            const name = $id('empNameInput').value.trim();
            const dept = $id('empDeptInput').value;
            const position = $id('empPosInput').value.trim();
            const phone = $id('empPhoneInput').value.trim();
            const pw = $id('empPwInput').value.trim() || '123456';
            const status = $id('empStatusInput').value;

            if (origCode) {
                const idx = accounts.findIndex(a => a.code === origCode);
                if (idx !== -1) accounts[idx] = { ...accounts[idx], id: code, code, name, dept, position, phone, pw, status };
            } else {
                if (accounts.some(a => a.code.toUpperCase() === code)) return showToast('ລະຫັດພະນັກງານນີ້ມີໃນລະບົບແລ້ວ!', true);
                accounts.unshift({ id: code, code, name, dept, position, phone, pw, status });
            }
            saveStore(STORAGE_KEYS.ACCOUNTS, accounts);
            resetEmpForm();
            refreshAllViews();
            showToast(`ບັນທຶກຂໍ້ມູນ ${name} ສຳເລັດ!`);
        }

        function editEmployee(code) {
            const emp = accounts.find(a => a.code === code);
            if (!emp) return;
            $id('editEmpOriginalCode').value = emp.code;
            $id('empCodeInput').value = emp.code;
            $id('empNameInput').value = emp.name;
            $id('empDeptInput').value = emp.dept || 'ພະແນກຈັດຕັ້ງ-ບໍລິຫານ';
            $id('empPosInput').value = emp.position || '';
            $id('empPhoneInput').value = emp.phone;
            $id('empPwInput').value = emp.pw || '123456';
            $id('empStatusInput').value = emp.status || 'active';
            $id('empFormTitle').innerHTML = `<i class="fa-solid fa-pen-to-square text-amber-500 mr-1.5"></i> ແກ້ໄຂຂໍ້ມູນ: ${emp.code}`;
            $id('cancelEditEmpBtn').classList.remove('hidden');
        }

        function resetEmpForm() {
            $id('empForm').reset();
            $id('editEmpOriginalCode').value = '';
            $id('empFormTitle').innerHTML = '<i class="fa-solid fa-user-plus text-blue-600 mr-1.5"></i> ເພີ່ມ / ບັນທຶກຂໍ້ມູນພະນັກງານໃໝ່';
            $id('cancelEditEmpBtn').classList.add('hidden');
        }

        function deleteEmployee(code) {
            accounts = accounts.filter(a => a.code !== code);
            saveStore(STORAGE_KEYS.ACCOUNTS, accounts);
            refreshAllViews();
            showToast(`ລຶບຂໍ້ມູນພະນັກງານ ${code} ແລ້ວ`);
        }

        function quickAdminCheckin(code, type) {
            const emp = accounts.find(a => a.code === code);
            if (emp) addLog(type, `ECI-35-GATE-01 (ບັນທຶກໂດຍ Admin)`, null, false, emp);
        }

        function renderAdminEmployees() {
            const el = $id('adminEmpList');
            if (!el) return;
            const q = ($id('empSearchInput')?.value || '').trim().toLowerCase();
            const list = q ? accounts.filter(a => a.name.toLowerCase().includes(q) || a.code.toLowerCase().includes(q) || (a.dept || '').toLowerCase().includes(q) || (a.position || '').toLowerCase().includes(q) || String(a.phone || '').replace(/\s/g, '').includes(q.replace(/\s/g, ''))) : accounts;
            el.innerHTML = list.length
                ? list.map(a => `<div class="p-3 bg-slate-50 rounded-2xl border border-slate-200/80 space-y-2 text-xs"><div class="flex justify-between items-start gap-2"><div><div class="flex items-center gap-1.5"><span class="px-2 py-0.5 rounded-md bg-blue-900 text-amber-300 font-bold text-[10px]">${a.code}</span><span class="font-bold text-slate-900">${a.name}</span></div><p class="text-[10px] text-slate-600 mt-1">${a.dept || '-'} • ${a.position || '-'}</p><p class="text-[10px] text-slate-500">${a.phone} | ຜ່ານ: <code class="bg-white px-1 rounded border">${a.pw}</code></p></div><span class="px-2 py-0.5 rounded-full text-[9px] font-bold ${a.status === 'inactive' ? 'bg-rose-100 text-rose-700' : 'bg-emerald-100 text-emerald-700'}">${a.status === 'inactive' ? 'ພັກວຽກ' : 'ປະຈຳການ'}</span></div><div class="flex flex-wrap gap-1 pt-1.5 border-t border-slate-200/70 text-[9px] font-bold"><button onclick="quickAdminCheckin('${a.code}','IN_MORNING')" class="px-1.5 py-1 rounded-lg bg-emerald-600 text-white">ເຂົ້າເຊົ້າ</button><button onclick="quickAdminCheckin('${a.code}','OUT_NOON')" class="px-1.5 py-1 rounded-lg bg-amber-500 text-blue-950">ອອກສວຍ</button><button onclick="quickAdminCheckin('${a.code}','IN_NOON')" class="px-1.5 py-1 rounded-lg bg-sky-600 text-white">ເຂົ້າສວຍ</button><button onclick="quickAdminCheckin('${a.code}','OUT_EVENING')" class="px-1.5 py-1 rounded-lg bg-rose-600 text-white">ອອກແລງ</button><button onclick='showBadgeForEmployee(${JSON.stringify(a).replace(/'/g, "'")})' class="px-1.5 py-1 rounded-lg bg-slate-800 text-amber-300">QR</button><button onclick="editEmployee('${a.code}')" class="px-1.5 py-1 rounded-lg bg-blue-100 text-blue-900"><i class="fa-solid fa-pen"></i></button><button onclick="deleteEmployee('${a.code}')" class="px-1.5 py-1 rounded-lg bg-rose-50 text-rose-600 ml-auto"><i class="fa-solid fa-trash"></i></button></div></div>`).join('')
                : '<p class="text-slate-400 text-xs text-center py-4">ບໍ່ພົບຂໍ້ມູນພະນັກງານ</p>';
        }

        function renderAdminLogs() {
            const el = $id('adminLogList');
            if (!el) return;
            const q = ($id('logSearchInput')?.value || '').trim().toLowerCase();
            const list = q ? attendanceLogs.filter(l => l.empName.toLowerCase().includes(q) || l.empCode.toLowerCase().includes(q) || l.date.includes(q)) : attendanceLogs;
            el.innerHTML = list.length
                ? list.map(l => `<div class="p-2.5 bg-white rounded-xl border border-slate-200 shadow-sm text-xs"><div class="flex justify-between items-start gap-2"><div class="flex gap-2">${logPhoto(l)}<div><span class="font-bold text-slate-800">${l.empName}</span> <span class="text-slate-500 text-[11px]">(${l.empCode})</span><p class="text-[11px] text-slate-500">${l.empPhone}</p></div></div>${badge(l)}</div><div class="mt-2 pt-1 border-t border-slate-100 text-[11px] text-slate-600"><div class="flex justify-between gap-2"><span class="font-semibold">${l.date} • ${l.time}</span><span class="text-blue-900 font-medium truncate">${l.location}</span></div><div class="mt-1">${logExtra(l)}</div></div></div>`).join('')
                : '<p class="text-slate-400 text-[11px] text-center py-4">ຍັງບໍ່ມີຂໍ້ມູນການເຂົ້າ-ອອກວຽກ</p>';
        }

        function renderAdminLeaves() {
            const container = $id('adminLeaveList');
            if (!container) return;
            container.innerHTML = leaveRequests.length
                ? leaveRequests.map(lr => `<div class="p-3 bg-white rounded-xl border border-slate-200 shadow-sm text-xs space-y-1.5"><div class="flex justify-between items-start"><div><span class="font-bold text-slate-900">${lr.empName}</span> <span class="text-slate-500 text-[11px]">(${lr.empCode})</span></div><span class="bg-amber-100 text-amber-800 px-2 py-0.5 rounded text-[10px] font-bold">${lr.type}</span></div><p class="text-slate-600 text-[11px]">ວັນທີ: <strong>${lr.start}</strong> ເຖິງ <strong>${lr.end}</strong></p><p class="text-slate-700 bg-slate-50 p-2 rounded-lg text-[11px]">💬 "${lr.reason}"</p><div class="flex items-center justify-between pt-1"><span class="px-2 py-0.5 rounded-full text-[10px] font-bold ${lr.status.includes('ອະນຸມັດແລ້ວ') ? 'bg-emerald-100 text-emerald-800' : lr.status.includes('ປະຕິເສດ') ? 'bg-rose-100 text-rose-800' : 'bg-slate-100 text-slate-600'}">${lr.status}</span><div class="space-x-1"><button onclick="setLeave(${lr.id},'✅ ອະນຸມັດແລ້ວ')" class="px-2.5 py-1 rounded-lg bg-emerald-600 text-white text-[10px] font-bold">ອະນຸມັດ</button><button onclick="setLeave(${lr.id},'❌ ປະຕິເສດ')" class="px-2.5 py-1 rounded-lg bg-rose-600 text-white text-[10px] font-bold">ປະຕິເສດ</button></div></div></div>`).join('')
                : '<p class="text-slate-400 text-[11px] text-center py-4">ຍັງບໍ່ມີໃບຂໍລາພັກ</p>';
        }

        function setLeave(id, st) {
            const l = leaveRequests.find(x => x.id === id);
            if (!l) return;
            l.status = st;
            saveStore(STORAGE_KEYS.LEAVES, leaveRequests);
            refreshAllViews();
            showToast('ອັບເດດສະຖານະໃບລາແລ້ວ');
        }

        function clearLogs() {
            attendanceLogs = [];
            saveStore(STORAGE_KEYS.LOGS, attendanceLogs);
            refreshAllViews();
            showToast('ລ້າງປະຫວັດເຂົ້າ-ອອກແລ້ວ');
        }

        let adminCamStream = null, adminCamRaf = 0;
        function stopAdminCam() {
            cancelAnimationFrame(adminCamRaf);
            if (adminCamStream) { adminCamStream.getTracks().forEach(t => t.stop()); adminCamStream = null; }
            $id('adminCamBox').classList.add('hidden');
            $id('adminScanBtn').innerHTML = '<i class="fa-solid fa-qrcode mr-1.5"></i> ເປີດກ້ອງສະແກນບັດພະນັກງານ';
        }

        async function toggleAdminCam() {
            if (adminCamStream) { stopAdminCam(); return; }
            try { if (!navigator.mediaDevices || !window.isSecureContext) throw { name: 'Insecure' }; adminCamStream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: { ideal: 'environment' }, width: { ideal: 1280 }, height: { ideal: 720 } }, audio: false }); }
            catch (e) { return showToast(camErrMsg(e), true); }
            const v = $id('adminCamVid');
            v.muted = true; v.setAttribute('playsinline', ''); v.srcObject = adminCamStream;
            try { await v.play(); } catch (e) {}
            $id('adminCamBox').classList.remove('hidden');
            $id('adminScanBtn').innerHTML = '<i class="fa-solid fa-stop mr-1.5"></i> ປິດກ້ອງສະແກນ';

            const mine = adminCamStream; let busy = false, last = 0, lastBad = '';
            (function loop(ts) {
                if (adminCamStream !== mine) return;
                if (!busy && ts - last > 120 && v.readyState >= 2 && v.videoWidth) {
                    busy = true; last = ts;
                    decodeQr(v, v.videoWidth, v.videoHeight).then(data => {
                        busy = false; if (!data || adminCamStream !== mine) return;
                        const raw = data.trim(), empCode = raw.startsWith('EMP|') ? raw.split('|')[1].trim().toUpperCase() : raw.toUpperCase();
                        const emp = accounts.find(a => a.code.toUpperCase() === empCode);
                        if (emp) {
                            stopAdminCam();
                            const type = (document.querySelector('input[name="adminGateType"]:checked') || {}).value || 'IN_MORNING';
                            addLog(type, `ECI-35-GATE-01 (ສະແກນບັດ QR)`, null, false, emp);
                        } else if (raw !== lastBad) { lastBad = raw; showToast('QR ນີ້ບໍ່ແມ່ນບັດພະນັກງານໃນລະບົບ', true); }
                    }).catch(() => { busy = false; });
                }
                adminCamRaf = requestAnimationFrame(loop);
            })(0);
        }

        function drawPointQr() {
            renderCheckpointList();
        }

        function addCheckpointPoint() {
            const rawName = $id('newCheckpointName').value.trim();
            if (!rawName) return showToast('ກະລຸນາປ້ອນຊື່ຈຸດສະແກນກ່ອນ', true);
            const num = String(checkpoints.length + 1).padStart(2, '0');
            const id = `ECI-35-GATE-${num}`;
            const cp = {
                id,
                name: rawName,
                payload: `ECI-CP|${id}`
            };
            checkpoints.push(cp);
            saveStore(STORAGE_KEYS.CHECKPOINTS, checkpoints);
            $id('newCheckpointName').value = '';
            renderCheckpointList();
            showToast(`ເພີ່ມຈຸດສະແກນ "${rawName}" ສຳເລັດ!`);
        }

        function deleteCheckpointPoint(id) {
            if (checkpoints.length <= 1) return showToast('ຕ້ອງມີຈຸດສະແກນຢ່າງໜ້ອຍ 1 ຈຸດໃນລະບົບ', true);
            checkpoints = checkpoints.filter(c => c.id !== id);
            saveStore(STORAGE_KEYS.CHECKPOINTS, checkpoints);
            renderCheckpointList();
            showToast('ລຶບຈຸດສະແກນແລ້ວ');
        }

        function renderCheckpointList() {
            const box = $id('checkpointListBox');
            if (!box) return;
            box.innerHTML = checkpoints.map(cp => `
                <div class="p-3 bg-slate-50 rounded-2xl border border-slate-200 flex items-center gap-3">
                    <div id="cp-qr-${cp.id}" class="p-1.5 bg-white rounded-xl border border-slate-200 shrink-0"></div>
                    <div class="flex-1 min-w-0">
                        <span class="px-2 py-0.5 rounded bg-blue-900 text-amber-300 text-[9px] font-bold">${cp.id}</span>
                        <h4 class="text-xs font-extrabold text-slate-900 truncate mt-1">${cp.name}</h4>
                        <p class="text-[9px] font-mono text-slate-500 truncate">${cp.payload}</p>
                        <div class="flex items-center gap-1.5 mt-2">
                            <button type="button" onclick="openPrintPoster('${cp.id}')" class="px-2.5 py-1 rounded-lg bg-amber-400 hover:bg-amber-500 text-blue-950 text-[10px] font-extrabold shadow-sm">
                                <i class="fa-solid fa-print mr-1"></i> ປິ້ນປ້າຍຕິດຝາ
                            </button>
                            <button type="button" onclick="deleteCheckpointPoint('${cp.id}')" class="px-2 py-1 rounded-lg bg-rose-50 text-rose-600 text-[10px] font-bold">
                                <i class="fa-solid fa-trash"></i>
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');

            if (window.QRCode) {
                checkpoints.forEach(cp => {
                    const target = $id(`cp-qr-${cp.id}`);
                    if (target) {
                        target.innerHTML = '';
                        new QRCode(target, { text: cpQr(cp), width: 120, height: 120, correctLevel: QRCode.CorrectLevel.M });
                    }
                });
            }
        }

        function openPrintPoster(cpId) {
            const cp = checkpoints.find(c => c.id === cpId) || checkpoints[0];
            if (!cp) return;
            activePosterPoint = cp;
            const t = shiftTimes || DEFAULT_SHIFT_TIMES;
            $id('posterPointTitle').innerText = `${cp.name} (${cp.id})`;
            $id('posterPayloadCode').innerText = cp.payload;
            $id('posterShiftGrid').innerHTML = `
                <div>🌅 ເຂົ້າເຊົ້າ: ${t.t1}</div>
                <div>☀️ ອອກສວຍ: ${t.t2}</div>
                <div>🌤️ ເຂົ້າສວຍ: ${t.t3}</div>
                <div>🌆 ອອກແລງ: ${t.t4}</div>
            `;
            const qrWrap = $id('posterQrCanvas');
            qrWrap.innerHTML = '';
            if (window.QRCode) new QRCode(qrWrap, { text: cpQr(cp), width: 220, height: 220, correctLevel: QRCode.CorrectLevel.M });
            applyBrandLogo();
            $id('printPosterModal').classList.remove('hidden');
        }

        function closePrintPosterModal() {
            $id('printPosterModal').classList.add('hidden');
        }

        function triggerNativePrintPoster() {
            const cp = activePosterPoint || checkpoints[0];
            const qrImg = $id('posterQrCanvas').querySelector('img') || $id('posterQrCanvas').querySelector('canvas');
            const qrSrc = qrImg ? (qrImg.src || qrImg.toDataURL('image/png')) : '';
            const t = shiftTimes || DEFAULT_SHIFT_TIMES;
            const win = window.open('', '_blank', 'width=600,height=820');
            if (!win) return showToast('ກະລຸນາອະນຸຍາດ Pop-up ເພື່ອປິ້ນປ້າຍ QR', true);
            win.document.write(`
                <!DOCTYPE html>
                <html lang="lo">
                <head>
                    <meta charset="UTF-8">
                    <title>ປ້າຍຈຸດສະແກນ QR - ${cp.name}</title>
                    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+Lao:wght@400;700;800&display=swap" rel="stylesheet">
                    <style>
                        body { font-family: 'Noto Sans Lao', sans-serif; display: flex; justify-content: center; align-items: center; min-height: 100vh; margin: 0; background: #fff; }
                        .poster { width: 420px; border: 6px solid #1e3a8a; border-radius: 28px; padding: 28px; text-align: center; }
                        .logo { width: 84px; height: 84px; border-radius: 50%; border: 3px solid #f59e0b; object-fit: cover; }
                        h1 { font-size: 18px; color: #1e3a8a; margin: 10px 0 2px; }
                        .sub { font-size: 11px; color: #d97706; font-weight: 800; margin-bottom: 14px; }
                        .gate { background: #1e3a8a; color: #fff; padding: 10px 14px; border-radius: 14px; margin-bottom: 18px; }
                        .gate small { color: #fcd34d; display: block; font-size: 12px; }
                        .gate strong { font-size: 18px; }
                        .qr-box { border: 3px dashed #1e3a8a; border-radius: 20px; padding: 16px; display: inline-block; margin-bottom: 14px; }
                        .qr-box img { width: 240px; height: 240px; display: block; }
                        .times { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; background: #fffbeb; border: 1px solid #fde68a; padding: 12px; border-radius: 14px; font-size: 13px; font-weight: 700; color: #1e293b; }
                        .foot { font-size: 12px; color: #64748b; margin-top: 14px; }
                
        </style>
                </head>
                <body>
                    <div class="poster">
                        <img class="logo" src="${companyLogo || DEFAULT_ECI_LOGO_SVG}" alt="ECI">
                        <h1>ລັດວິສາຫະກິດກໍ່ສ້າງ ແລະ ຕິດຕັ້ງໄຟຟ້າ</h1>
                        <div class="sub">ELECTRICAL CONSTRUCTION & INSTALLATION (ECI)</div>
                        <div class="gate">
                            <small>ຈຸດສະແກນບັນທຶກເວລາເຮັດວຽກ</small>
                            <strong>${cp.name} (${cp.id})</strong>
                        </div>
                        <div class="qr-box"><img src="${qrSrc}" alt="QR Checkpoint"></div>
                        <div class="times">
                            <div>🌅 ເຂົ້າເຊົ້າ: ${t.t1}</div>
                            <div>☀️ ອອກສວຍ: ${t.t2}</div>
                            <div>🌤️ ເຂົ້າສວຍ: ${t.t3}</div>
                            <div>🌆 ອອກແລງ: ${t.t4}</div>
                        </div>
                        <div class="foot">ສະແກນ QR Code ນີ້ຜ່ານລະບົບພະນັກງານ ECI ທຸກຄັ້ງທີ່ເຂົ້າ-ອອກວຽກ</div>
                    </div>
                    <script>window.onload = () => { setTimeout(() => window.print(), 400); };<\/script>
                </body>
                </html>
            `);
            win.document.close();
        }

        function downloadPosterPng() {
            const cp = activePosterPoint || checkpoints[0];
            const qrImg = $id('posterQrCanvas').querySelector('img') || $id('posterQrCanvas').querySelector('canvas');
            if (!qrImg) return;
            const src = qrImg.src || qrImg.toDataURL('image/png');
            const a = document.createElement('a');
            a.href = src;
            a.download = `QR_Checkpoint_${cp.id}.png`;
            a.click();
            showToast('ດາວໂຫຼດຮູບ QR ຈຸດສະແກນສຳເລັດ!');
        }

        function downloadFile(filename, content, mime) {
            const blob = new Blob([content], { type: mime });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url; a.download = filename; document.body.appendChild(a); a.click();
            setTimeout(() => { a.remove(); URL.revokeObjectURL(url); }, 1500);
        }

        function exportEmployeesCSV() {
            const header = '\uFEFFລະຫັດພະນັກງານ,ຊື່ ແລະ ນາມສະກຸນ,ພະແນກ,ຕຳແໜ່ງ,ເບີໂທ,ສະຖານະ\n';
            const rows = accounts.map(a => [a.code, a.name, a.dept || '', a.position || '', a.phone, a.status === 'inactive' ? 'ພັກວຽກ' : 'ປະຈຳການ'].map(v => `"${String(v).replace(/"/g, '""')}"`).join(',')).join('\n');
            downloadFile(`ECI_Employees_${new Date().toISOString().slice(0,10)}.csv`, header + rows, 'text/csv;charset=utf-8;');
        }

        function exportLogsCSV() {
            const header = '\uFEFFວັນທີ,ເວລາ,ລະຫັດພະນັກງານ,ຊື່ພະນັກງານ,ຊ່ວງເວລາສະແກນ,ສະຖານທີ່,Latitude,Longitude\n';
            const rows = attendanceLogs.map(l => [l.date, l.time, l.empCode, l.empName, getScanTypeMeta(l.type).short, l.location, l.lat || '', l.lng || ''].map(v => `"${String(v).replace(/"/g, '""')}"`).join(',')).join('\n');
            downloadFile(`ECI_Attendance_${new Date().toISOString().slice(0,10)}.csv`, header + rows, 'text/csv;charset=utf-8;');
        }

        function exportFullBackup() { exportBackup('full'); }

        // ---------------- BACKUP / RESTORE (computer + phone) ----------------
        const BACKUP_SCOPES = {
            full:  { label: 'ທັງລະບົບ', keys: ['ACCOUNTS', 'LOGS', 'LEAVES', 'DUTIES', 'ADS', 'SHIFT_TIMES', 'CHECKPOINTS', 'BRAND_LOGO'] },
            org:   { label: 'ລະບົບການຈັດຕັ້ງ', keys: ['ACCOUNTS', 'DUTIES', 'LEAVES', 'LOGS'] },
            admin: { label: 'ການຕັ້ງຄ່າ Admin', keys: ['ADS', 'SHIFT_TIMES', 'CHECKPOINTS', 'BRAND_LOGO'] }
        };
        const BACKUP_LABELS = { ACCOUNTS: 'ພະນັກງານ', LOGS: 'ປະຫວັດເຂົ້າ-ອອກ', LEAVES: 'ໃບລາພັກ', DUTIES: 'ເວນຍາມ', ADS: 'ໂຄສະນາ', SHIFT_TIMES: 'ເວລາ 4 ຊ່ວງ', CHECKPOINTS: 'ຈຸດສະແກນ', BRAND_LOGO: 'ໂລໂກ້' };
        const LAST_BACKUP_KEY = 'eci_v3_last_backup';

        function getBackupValue(k) {
            const m = { ACCOUNTS: accounts, LOGS: attendanceLogs, LEAVES: leaveRequests, DUTIES: dutyList, ADS: companyAds, SHIFT_TIMES: shiftTimes, CHECKPOINTS: checkpoints, BRAND_LOGO: companyLogo === DEFAULT_ECI_LOGO_SVG ? null : companyLogo };
            return m[k];
        }

        function buildBackupPayload(scope) {
            const data = {}, counts = {};
            BACKUP_SCOPES[scope].keys.forEach(k => {
                const v = getBackupValue(k);
                if (v === null || v === undefined) return;
                data[k] = v;
                counts[k] = Array.isArray(v) ? v.length : 1;
            });
            return { app: 'ECI-Staff-System', version: 3, scope, createdAt: new Date().toISOString(), counts, data };
        }

        function updateBackupInfo() {
            const el = $id('backupLastInfo');
            if (!el) return;
            let info = null;
            try { info = JSON.parse(localStorage.getItem(LAST_BACKUP_KEY) || 'null'); } catch (e) {}
            if (!info) { el.className = 'text-[10px] text-amber-600 font-semibold'; el.textContent = '⚠️ ຍັງບໍ່ເຄີຍສຳຮອງຂໍ້ມູນໃນເຄື່ອງນີ້'; return; }
            const days = Math.floor((Date.now() - new Date(info.at).getTime()) / 86400000);
            const when = new Date(info.at).toLocaleString('lo-LA');
            el.className = 'text-[10px] font-semibold ' + (days >= 7 ? 'text-amber-600' : 'text-emerald-600');
            el.textContent = (days >= 7 ? '⚠️ ' : '✅ ') + 'ສຳຮອງຄັ້ງຫຼ້າສຸດ: ' + when + ' (' + (BACKUP_SCOPES[info.scope] || {}).label + ')' + (days >= 7 ? ' — ຄວນສຳຮອງໃໝ່' : '');
        }

        async function exportBackup(scope) {
            const payload = buildBackupPayload(scope);
            const text = JSON.stringify(payload, null, 2);
            const name = `ECI_Backup_${scope}_${new Date().toISOString().slice(0, 10)}.json`;
            const done = () => { try { localStorage.setItem(LAST_BACKUP_KEY, JSON.stringify({ at: new Date().toISOString(), scope })); } catch (e) {} updateBackupInfo(); };
            const isTouch = window.matchMedia && window.matchMedia('(pointer: coarse)').matches;
            if (isTouch && navigator.canShare && window.File) {
                try {
                    const file = new File([text], name, { type: 'text/plain' });
                    if (navigator.canShare({ files: [file] })) {
                        await navigator.share({ files: [file], title: name });
                        done(); showToast('ແບ່ງປັນ/ບັນທຶກໄຟລ໌ Backup ສຳເລັດ'); return;
                    }
                } catch (err) { if (err && err.name === 'AbortError') return; }
            }
            downloadFile(name, text, 'application/json');
            done(); showToast('ດາວໂຫຼດໄຟລ໌ Backup ແລ້ວ: ' + name);
        }

        let pendingRestore = null;
        function validateBackup(j) {
            if (j && !j.app && (j.accounts || j.attendanceLogs || j.dutyList)) { // old format from v3 export
                j = { app: 'ECI-Staff-System', scope: 'full', data: { ACCOUNTS: j.accounts, LOGS: j.attendanceLogs, DUTIES: j.dutyList, LEAVES: j.leaveRequests, ADS: j.companyAds, SHIFT_TIMES: j.shiftTimes } };
            }
            if (!j || j.app !== 'ECI-Staff-System' || typeof j.data !== 'object' || !j.data) throw new Error('ບໍ່ແມ່ນໄຟລ໌ Backup ຂອງລະບົບ ECI');
            const d = {};
            ['ACCOUNTS', 'LOGS', 'LEAVES', 'DUTIES', 'ADS', 'CHECKPOINTS'].forEach(k => {
                if (j.data[k] === undefined || j.data[k] === null) return;
                if (!Array.isArray(j.data[k])) throw new Error('ຂໍ້ມູນໝວດ ' + BACKUP_LABELS[k] + ' ຜິດຮູບແບບ');
                d[k] = j.data[k];
            });
            if (d.ACCOUNTS && d.ACCOUNTS.some(a => !a || typeof a.code !== 'string' || !a.code)) throw new Error('ຂໍ້ມູນພະນັກງານບໍ່ຄົບ (ບໍ່ມີລະຫັດ)');
            if (j.data.SHIFT_TIMES && typeof j.data.SHIFT_TIMES === 'object') d.SHIFT_TIMES = j.data.SHIFT_TIMES;
            if (typeof j.data.BRAND_LOGO === 'string' && j.data.BRAND_LOGO) d.BRAND_LOGO = j.data.BRAND_LOGO;
            if (!Object.keys(d).length) throw new Error('ບໍ່ພົບຂໍ້ມູນໃນໄຟລ໌');
            return { meta: j, data: d };
        }

        async function handleRestoreFile(e) {
            const f = e.target.files[0];
            e.target.value = '';
            if (!f) return;
            try {
                pendingRestore = validateBackup(JSON.parse(await f.text()));
                const lines = Object.entries(pendingRestore.data).map(([k, v]) => `<div>• ${BACKUP_LABELS[k]}: <b>${Array.isArray(v) ? v.length + ' ລາຍການ' : 'ມີ'}</b></div>`).join('');
                const when = pendingRestore.meta.createdAt ? new Date(pendingRestore.meta.createdAt).toLocaleString('lo-LA') : 'ບໍ່ລະບຸ';
                $id('restoreSummary').innerHTML = `<div class="font-bold text-slate-800">ໄຟລ໌: ${(BACKUP_SCOPES[pendingRestore.meta.scope] || { label: 'ບໍ່ລະບຸ' }).label} — ສ້າງເມື່ອ ${when}</div>` + lines;
                $id('restorePreview').classList.remove('hidden');
            } catch (err) {
                pendingRestore = null;
                $id('restorePreview').classList.add('hidden');
                showToast('ອ່ານໄຟລ໌ບໍ່ໄດ້: ' + err.message, true);
            }
        }

        function cancelRestore() { pendingRestore = null; $id('restorePreview').classList.add('hidden'); }

        function applyRestore() {
            if (!pendingRestore) return;
            const mode = (document.querySelector('input[name="restoreMode"]:checked') || {}).value || 'merge';
            if (!confirm(mode === 'replace' ? 'ແທນທີ່ຂໍ້ມູນເດີມຕາມໝວດໃນໄຟລ໌? (ຈະດາວໂຫຼດ Backup ປັດຈຸບັນໃຫ້ກ່ອນ)' : 'ລວມຂໍ້ມູນຈາກໄຟລ໌ເຂົ້າລະບົບ?')) return;
            if (mode === 'replace') {
                const safety = buildBackupPayload('full');
                downloadFile(`ECI_Safety_Before_Restore_${Date.now()}.json`, JSON.stringify(safety, null, 2), 'application/json');
            }
            const d = pendingRestore.data;
            const gen = x => (x && x.id !== undefined && x.id !== null) ? String(x.id) : JSON.stringify(x);
            const merge = (cur, inc, keyFn) => { const m = new Map(cur.map(x => [keyFn(x), x])); inc.forEach(x => m.set(keyFn(x), x)); return Array.from(m.values()); };
            if (d.ACCOUNTS) { accounts = mode === 'replace' ? d.ACCOUNTS : merge(accounts, d.ACCOUNTS, a => a.code); saveStore(STORAGE_KEYS.ACCOUNTS, accounts); }
            if (d.LOGS) { attendanceLogs = mode === 'replace' ? d.LOGS : merge(attendanceLogs, d.LOGS, l => [l.date, l.time, l.empCode, l.type].join('|')); saveStore(STORAGE_KEYS.LOGS, attendanceLogs); }
            if (d.LEAVES) { leaveRequests = mode === 'replace' ? d.LEAVES : merge(leaveRequests, d.LEAVES, gen); saveStore(STORAGE_KEYS.LEAVES, leaveRequests); }
            if (d.DUTIES) { dutyList = mode === 'replace' ? d.DUTIES : merge(dutyList, d.DUTIES, gen); saveStore(STORAGE_KEYS.DUTIES, dutyList); }
            if (d.ADS) { companyAds = mode === 'replace' ? d.ADS : merge(companyAds, d.ADS, gen); saveStore(STORAGE_KEYS.ADS, companyAds); }
            if (d.CHECKPOINTS) { checkpoints = mode === 'replace' ? d.CHECKPOINTS : merge(checkpoints, d.CHECKPOINTS, gen); saveStore(STORAGE_KEYS.CHECKPOINTS, checkpoints); activePosterPoint = checkpoints[0]; }
            if (d.SHIFT_TIMES) { shiftTimes = d.SHIFT_TIMES; saveStore(STORAGE_KEYS.SHIFT_TIMES, shiftTimes); applyShiftTimeLabels(); }
            if (d.BRAND_LOGO) { companyLogo = d.BRAND_LOGO; localStorage.setItem(STORAGE_KEYS.BRAND_LOGO, companyLogo); if (db) db.ref('eci_system/' + STORAGE_KEYS.BRAND_LOGO).set(companyLogo); applyBrandLogo(); }
            cancelRestore();
            refreshAllViews();
            showToast('ກູ້ຄືນຂໍ້ມູນສຳເລັດ!');
        }

        // Department list (also fills the employee form select, which was empty)
        const DEPARTMENTS = ['ພະແນກເຈລະຈາທຸລະກິດ ແລະ ການຮ່ວມມື', 'ພະແນກຈັດຕັ້ງ-ບໍລິຫານ', 'ພະແນກເຕັກນິກ-ກໍ່ສ້າງ', 'ພະແນກຝ່າຍປະຕິບັດການ O&M', 'ພະແນກການເງິນ-ບັນຊີ', 'ພະແນກການສະຖາບັນ', 'ພະແນກ ຈັດຕັ້ງບໍລິຫານ ຫຼື Admin'];
        function populateDeptSelect() {
            const sel = $id('empDeptInput');
            if (!sel) return;
            const cur = sel.value;
            const list = DEPARTMENTS.slice();
            accounts.forEach(a => { if (a.dept && !list.includes(a.dept)) list.push(a.dept); });
            sel.innerHTML = list.map(dp => `<option value="${dp.replace(/"/g, '&quot;')}">${dp}</option>`).join('');
            if (cur && list.includes(cur)) sel.value = cur;
        }

        function resetDemoData() {
            accounts = JSON.parse(JSON.stringify(DEFAULT_ACCOUNTS));
            attendanceLogs = []; dutyList = []; leaveRequests = [];
            companyAds = JSON.parse(JSON.stringify(DEFAULT_ADS));
            shiftTimes = JSON.parse(JSON.stringify(DEFAULT_SHIFT_TIMES));
            saveStore(STORAGE_KEYS.ACCOUNTS, accounts);
            saveStore(STORAGE_KEYS.LOGS, attendanceLogs);
            saveStore(STORAGE_KEYS.DUTIES, dutyList);
            saveStore(STORAGE_KEYS.LEAVES, leaveRequests);
            saveStore(STORAGE_KEYS.ADS, companyAds);
            saveStore(STORAGE_KEYS.SHIFT_TIMES, shiftTimes);
            applyShiftTimeLabels();
            refreshAllViews();
            showToast('ຄືນຄ່າຂໍ້ມູນເລີ່ມຕົ້ນສຳເລັດ!');
        }

        let adminAuthed = false, pinBuf = '';
        function drawPin() {
            $id('pinDots').innerHTML = [0, 1, 2, 3].map(i => `<span class="w-3.5 h-3.5 rounded-full border-2 border-blue-900 ${i < pinBuf.length ? 'bg-blue-900' : 'bg-transparent'}"></span>`).join('');
        }
        $id('pinPad').innerHTML = ['1','2','3','4','5','6','7','8','9','','0','del'].map(k =>
            k === '' ? '<span></span>' : `<button type="button" onclick="pinKey('${k}')" class="h-12 rounded-2xl bg-slate-100 hover:bg-slate-200 text-base font-bold text-slate-800">${k === 'del' ? '<i class="fa-solid fa-delete-left"></i>' : k}</button>`
        ).join('');

        function pinKey(k) {
            if (k === 'del') pinBuf = pinBuf.slice(0, -1);
            else if (pinBuf.length < 4) pinBuf += k;
            drawPin();
            if (pinBuf.length < 4) return;
            const entered = pinBuf; pinBuf = ''; drawPin();
            if (entered === adminPin) {
                adminAuthed = true;
                $id('secAdminPin').classList.add('hidden');
                $id('secAdmin').classList.remove('hidden');
                refreshAllViews(); drawPointQr(); { let lt = 'employees'; try { lt = localStorage.getItem('eci_admin_tab') || lt; } catch (e) {} switchAdminTab($id('adminTab' + lt.charAt(0).toUpperCase() + lt.slice(1)) ? lt : 'employees'); }
            } else {
                showToast('Admin PIN ບໍ່ຖືກຕ້ອງ', true);
            }
        }

        function changeAdminPin() {
            const val = $id('newAdminPin').value.trim();
            if (!/^\d{4}$/.test(val)) return showToast('ກະລຸນາປ້ອນຕົວເລກ 4 ຫຼັກ', true);
            adminPin =val;
            saveStore(STORAGE_KEYS.ADMIN_PIN, adminPin);
            $id('newAdminPin').value = '';
            showToast('ປ່ຽນ Admin PIN ໃໝ່ສຳເລັດ!');
        }

        function toggleAdminView() {
            stopCam(); stopAdminCam();
            const A = $id('secAdmin'), P = $id('secAdminPin'), D = $id('secDashboard'), Lg = $id('secLogin');
            if (!A.classList.contains('hidden') || !P.classList.contains('hidden')) {
                A.classList.add('hidden'); P.classList.add('hidden'); adminAuthed = false;
                (currentUser ? D : Lg).classList.remove('hidden');
                return;
            }
            D.classList.add('hidden'); Lg.classList.add('hidden');
            if (adminAuthed) { A.classList.remove('hidden'); refreshAllViews(); }
            else { pinBuf = ''; drawPin(); P.classList.remove('hidden'); }
        }


        /* ===================== FINANCE MODULE ===================== */
        const fmt = n => Math.round(n || 0).toLocaleString('en-US');
        const fesc = t => String(t == null ? '' : t).replace(/[&<>"']/g, c => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' }[c]));
        /* ---- Durable storage: localStorage + IndexedDB (+ Firebase when online) ---- */
        function idb() { return window._idb || (window._idb = new Promise((res, rej) => { const r = indexedDB.open('eci_fin', 1); r.onupgradeneeded = () => r.result.createObjectStore('kv'); r.onsuccess = () => res(r.result); r.onerror = () => rej(r.error); })); }
        const idbTx = (mode, fn) => idb().then(d => new Promise((res, rej) => { const t = d.transaction('kv', mode), q = fn(t.objectStore('kv')); t.oncomplete = () => res(q && q.result); t.onerror = () => rej(t.error); }));
        const idbPut = (k, v) => idbTx('readwrite', s => s.put(v, k)), idbGet = k => idbTx('readonly', s => s.get(k)), idbDel = k => idbTx('readwrite', s => s.delete(k));
        function setSaveStat(ok) { const e = $id('saveStat'); if (!e) return; e.className = 'flex-1 text-[10px] ' + (ok ? 'text-emerald-600' : 'text-rose-600'); e.textContent = ok ? `💾 ບັນທຶກອັດຕະໂນມັດແລ້ວ ${new Date().toLocaleTimeString('lo-LA')} · ເຄື່ອງ ✓ · ${db ? 'Cloud ✓' : 'ອອບໄລນ໌ (ບໍ່ມີ Cloud)'}` : '⚠️ ບັນທຶກລົງເຄື່ອງບໍ່ສຳເລັດ — ກະລຸນາກົດ ⬇ ສຳຮອງ'; }
        const finSave = () => { fin.updated = Date.now(); saveStore(STORAGE_KEYS.FINANCE, fin); idbPut('fin', fin).then(() => setSaveStat(true)).catch(() => setSaveStat(false)); };
        async function initFinStore() {
            try {
                if (navigator.storage && navigator.storage.persist) navigator.storage.persist();
                const b = await idbGet('fin');
                if (b && (b.updated || 0) > (fin.updated || 0)) { fin = b; try { localStorage.setItem(STORAGE_KEYS.FINANCE, JSON.stringify(fin)); } catch (e) {} }
                fin.salaries = fin.salaries || {}; fin.pay2 = fin.pay2 || {}; fin.docs = fin.docs || []; fin.ledger = fin.ledger || [];
                let mig = false; for (const d of fin.docs) if (d.img) { await idbPut('img:' + d.id, d.img); delete d.img; d.hasImg = true; mig = true; }
                if (mig || !b) finSave(); else setSaveStat(true);
            } catch (e) { setSaveStat(false); }
            renderFinance();
        }
        const DRAFT_IDS = ['docTitle', 'docNote', 'docCat', 'docEmp', 'ledType', 'ledCat', 'ledAmt', 'ledTax', 'ledNote', 'ledIncl'];
        const draftLoad = () => { try { return JSON.parse(localStorage.getItem('eci_fin_draft') || '{}'); } catch (e) { return {}; } };
        const draftPut = d => { try { localStorage.setItem('eci_fin_draft', JSON.stringify(d)); } catch (e) {} };
        function draftSave(e) { if (!DRAFT_IDS.includes(e.target.id)) return; const d = draftLoad(); d[e.target.id] = e.target.type === 'checkbox' ? e.target.checked : e.target.value; draftPut(d); }
        function draftRestore() { const d = draftLoad(); DRAFT_IDS.forEach(i => { const e = $id(i); if (e && i in d) { if (e.type === 'checkbox') e.checked = d[i]; else e.value = d[i]; } }); }
        function clearDraft(ids) { const d = draftLoad(); ids.forEach(i => delete d[i]); draftPut(d); }
        document.addEventListener('input', draftSave); document.addEventListener('change', draftSave);
        const flushEdit = () => { if (document.hidden !== false && document.activeElement && document.activeElement.blur) document.activeElement.blur(); };
        document.addEventListener('visibilitychange', () => { if (document.hidden) flushEdit(); }); window.addEventListener('pagehide', flushEdit);
        async function backupFin() {
            const imgs = {}; for (const d of fin.docs) if (d.hasImg) imgs[d.id] = await idbGet('img:' + d.id);
            const files = {}; for (const d of fin.docs) for (let i = 0; i < (d.files || []).length; i++) { const b = await idbGet('file:' + d.id + ':' + i); if (b) files['file:' + d.id + ':' + i] = await blobUrl(b); }
            const a = document.createElement('a'); a.href = URL.createObjectURL(new Blob([JSON.stringify({ eciFinance: 1, fin, imgs, files })], { type: 'application/json' }));
            a.download = 'ECI_finance_backup_' + new Date().toISOString().slice(0, 10) + '.json'; a.click(); showToast('ດາວໂຫຼດໄຟລ໌ສຳຮອງແລ້ວ');
        }
        function restoreFin(inp) {
            const f = inp.files[0]; if (!f) return; const rd = new FileReader();
            rd.onload = async e => { try { const o = JSON.parse(e.target.result); if (!o.eciFinance || !o.fin) throw 0;
                if (!confirm('ກູ້ຄືນຂໍ້ມູນການເງິນຈາກໄຟລ໌? ຂໍ້ມູນປັດຈຸບັນຈະຖືກແທນທີ່.')) return;
                fin = o.fin; for (const [id, u] of Object.entries(o.imgs || {})) await idbPut('img:' + id, u); for (const [k, u] of Object.entries(o.files || {})) await idbPut(k, await (await fetch(u)).blob()); finSave(); initFinance(); showToast('ກູ້ຄືນຂໍ້ມູນສຳເລັດ');
            } catch (err) { showToast('ໄຟລ໌ສຳຮອງບໍ່ຖືກຕ້ອງ', true); } inp.value = ''; };
            rd.readAsText(f);
        }
        function viewImg(id) { idbGet('img:' + id).then(u => u && fetch(u).then(r => r.blob()).then(b => window.open(URL.createObjectURL(b)))); }
        function loadDocImgs() { document.querySelectorAll('#docList img[data-img]').forEach(im => idbGet('img:' + im.dataset.img).then(u => { if (u) im.src = u; })); }
        initFinStore();
        let docImg = '';
        function initFinance() {
            const t = new Date(), ym = t.toISOString().slice(0, 7);
            ['payMonth', 'ledMonth'].forEach(i => { if (!$id(i).value) $id(i).value = ym; });
            ['docDate', 'ledDate'].forEach(i => { if (!$id(i).value) $id(i).value = t.toISOString().slice(0, 10); });
            loadCfg();
            const eo = accounts.map(a => `<option value="${fesc(a.code)}">${fesc(a.code)} - ${fesc(a.name)}</option>`).join('');
            $id('docEmp').innerHTML = '<option value="">— ບໍ່ຜູກກັບພະນັກງານ —</option>' + eo;
            $id('docFilEmp').innerHTML = '<option value="">ທຸກພະນັກງານ</option>' + eo;
            $id('docFilCat').innerHTML = '<option value="">ທຸກໝວດ</option>' + [...$id('docCat').options].map(o => `<option>${fesc(o.value)}</option>`).join('');
            if (!$id('repYear').value) $id('repYear').value = t.getFullYear();
            ['repMonth', 'repFrom', 'repTo'].forEach(i => { if (!$id(i).value) $id(i).value = ym; });
            repToggle(); repEmpRender();
            draftRestore();
            finSub(window._finSub || 'pay');
        }
        function finSub(k) {
            window._finSub = k;
            ['pay', 'docs', 'led'].forEach(x => {
                $id('finSub_' + x).classList.toggle('hidden', x !== k);
                $id('finBtn_' + x).className = 'py-1.5 rounded-xl ' + (x === k ? 'bg-blue-900 text-white' : 'text-slate-600');
            });
            renderFinance();
        }
        function renderFinance() { if (!$id('adminTabFinance') || $id('adminTabFinance').classList.contains('hidden')) return; ({ pay: renderPayroll, docs: renderDocs, led: renderLed })[window._finSub || 'pay'](); }
        const CFG0 = { sso: 5.5, ssoCap: 4500000, days: 26, hours: 8, ot: 1.5, co: 'ລັດວິສາຫະກິດ (ECI)', s1: '', s2: '', s3: '' };
        const CFGM = { cfgCo: 'co', cfgSso: 'sso', cfgCap: 'ssoCap', cfgDays: 'days', cfgHours: 'hours', cfgOt: 'ot', cfgS1: 's1', cfgS2: 's2', cfgS3: 's3', cfgTax: 'tax' };
        function loadCfg() { fin.cfg = { ...CFG0, ...fin.cfg }; fin.pay2 = fin.pay2 || {}; Object.entries(CFGM).forEach(([i, k]) => { const e = $id(i); if (e) e.value = fin.cfg[k]; }); }
        function saveCfg() { Object.entries(CFGM).forEach(([i, k]) => { const e = $id(i), n = ['co', 's1', 's2', 's3', 'tax'].includes(k); fin.cfg[k] = n ? e.value : (parseFloat(e.value) || 0); }); finSave(); renderPayroll(); }
        function calcTax(taxable) {
            let prev = 0, tax = 0;
            for (const line of fin.cfg.tax.split('\n')) {
                const [lim, r] = line.split(',').map(Number); if (isNaN(r)) continue;
                const top = lim > 0 ? lim : Infinity;
                if (taxable > prev) tax += (Math.min(taxable, top) - prev) * r / 100;
                prev = top; if (taxable <= top) break;
            }
            return tax;
        }
        const pm = () => $id('payMonth').value;
        const payOpen = new Set(); function payToggle(d) { d.open ? payOpen.add(d.dataset.c) : payOpen.delete(d.dataset.c); }
        const pRec = m => (fin.pay2[m] = fin.pay2[m] || { status: 'draft', rows: {} });
        function payRow(code, m) {
            const c = fin.cfg, sv = fin.salaries[code] || {}, r = ((fin.pay2[m] || { rows: {} }).rows[code]) || {};
            const n = v => +v || 0, base = n(sv.base);
            const daily = base / (c.days || 26), hourly = daily / (c.hours || 8);
            const allow = n(sv.pos) + n(sv.trans) + n(sv.meal) + n(sv.oth);
            const otPay = n(r.otH) * hourly * c.ot, absDed = n(r.abs) * daily;
            const gross = base + allow + otPay + n(r.bonus);
            const sso = Math.min(base + n(sv.pos), c.ssoCap) * c.sso / 100;
            const taxable = Math.max(0, gross - absDed - sso), tax = calcTax(taxable);
            const net = gross - absDed - sso - tax - n(r.adv) - n(r.ded);
            return { sv, r, base, allow, otPay, absDed, gross, sso, taxable, tax, net, adv: n(r.adv), ded: n(r.ded), bonus: n(r.bonus), paid: !!r.paid };
        }
        function renderPayroll() {
            loadCfg(); const m = pm(); if (!m) return;
            const rec = pRec(m), lock = rec.status !== 'draft';
            const st = { draft: ['ຮ່າງ (ແກ້ໄຂໄດ້)', 'bg-amber-100 text-amber-700'], approved: ['ອະນຸມັດແລ້ວ 🔒', 'bg-blue-100 text-blue-800'], paid: ['ຈ່າຍແລ້ວ ✅', 'bg-emerald-100 text-emerald-700'] }[rec.status];
            $id('payStatus').textContent = st[0]; $id('payStatus').className = 'text-[10px] font-bold px-3 py-2 rounded-xl ' + st[1];
            $id('payApproveBtn').textContent = rec.status === 'draft' ? 'ອະນຸມັດງວດ' : 'ປົດລັອກ';
            const dsel = $id('payDept'), dcur = dsel.value, depts = [...new Set(accounts.map(a => a.dept).filter(Boolean))];
            dsel.innerHTML = '<option value="">ທຸກພະແນກ</option>' + depts.map(d => `<option value="${fesc(d)}">${fesc(d)}</option>`).join(''); dsel.value = depts.includes(dcur) ? dcur : '';
            const q = ($id('paySearch').value || '').trim().toLowerCase(), fd = dsel.value, ff = $id('payFilt').value; let shown = 0;
            const match = (a, p) => (!q || [a.name, a.code, a.position, a.dept, a.phone].some(x => String(x || '').toLowerCase().includes(q))) && (!fd || a.dept === fd) && (!ff || (ff === 'paid' ? p.paid : ff === 'unpaid' ? !p.paid : !p.base));
            let T = { gross: 0, sso: 0, tax: 0, net: 0, ded: 0 };
            const dis = lock ? 'disabled' : '';
            const f = (code, kind, k, l, v, st2) => `<div><label class="text-[9px] text-slate-500">${l}</label><input type="number" ${st2 ? '' : dis} value="${v || ''}" onchange="payEdit('${fesc(code)}','${kind}','${k}',this.value)" class="w-full p-1.5 bg-slate-50 border border-slate-200 rounded-lg text-[11px] disabled:opacity-60"></div>`;
            $id('payList').innerHTML = accounts.map((a, i) => {
                const p = payRow(a.code, m); T.gross += p.gross; T.sso += p.sso; T.tax += p.tax; T.net += p.net; T.ded += p.absDed + p.adv + p.ded;
                if (!match(a, p)) return ''; shown++;
                return `<details data-c="${fesc(a.code)}" ${payOpen.has(a.code) ? 'open' : ''} ontoggle="payToggle(this)" class="bg-white rounded-2xl border border-slate-100"><summary class="p-3 cursor-pointer flex justify-between items-center gap-2"><div class="min-w-0"><b class="text-xs text-blue-950 block truncate">${i + 1}. ${fesc(a.name)}</b><span class="text-[9px] text-slate-500">${fesc(a.code)} · ${fesc(a.position)}</span></div><div class="text-right"><b class="text-xs text-emerald-700">${fmt(p.net)}</b><span class="block text-[9px] ${p.paid ? 'text-emerald-600' : 'text-amber-600'}">${p.paid ? 'ຈ່າຍແລ້ວ' : 'ຍັງບໍ່ຈ່າຍ'}</span></div></summary>
                <div class="px-3 pb-3 space-y-2"><div class="text-[10px] font-bold text-blue-900">ໂຄງສ້າງເງິນເດືອນປະຈຳ</div>
                <div class="grid grid-cols-3 gap-1.5">${f(a.code, 's', 'base', 'ເງິນເດືອນຖານ', p.sv.base, 1)}${f(a.code, 's', 'pos', 'ເງິນປະຈຳຕຳແໜ່ງ', p.sv.pos, 1)}${f(a.code, 's', 'trans', 'ຄ່າເດີນທາງ', p.sv.trans, 1)}${f(a.code, 's', 'meal', 'ຄ່າອາຫານ', p.sv.meal, 1)}${f(a.code, 's', 'oth', 'ເງິນອຸດໜູນອື່ນ', p.sv.oth, 1)}</div>
                <div class="text-[10px] font-bold text-blue-900">ລາຍການປະຈຳງວດ</div>
                <div class="grid grid-cols-3 gap-1.5">${f(a.code, 'm', 'otH', 'ຊົ່ວໂມງ OT', p.r.otH)}${f(a.code, 'm', 'bonus', 'ໂບນັດ', p.r.bonus)}${f(a.code, 'm', 'abs', 'ມື້ຂາດ/ລາບໍ່ມີເງິນ', p.r.abs)}${f(a.code, 'm', 'adv', 'ເບີກລ່ວງໜ້າ', p.r.adv)}${f(a.code, 'm', 'ded', 'ຫັກອື່ນໆ', p.r.ded)}</div>
                <div class="text-[10px] text-slate-600 grid grid-cols-2 gap-x-3 border-t pt-1.5"><span>ເງິນໄດ້ລວມ: <b>${fmt(p.gross)}</b></span><span>ຫັກຂາດ/ລາ: <b class="text-rose-600">-${fmt(p.absDed)}</b></span><span>ປະກັນສັງຄົມ: <b class="text-rose-600">-${fmt(p.sso)}</b></span><span>ພາສີລາຍໄດ້: <b class="text-rose-600">-${fmt(p.tax)}</b></span><span class="col-span-2">ສຸດທິ: <b class="text-emerald-700">${fmt(p.net)} ກີບ</b></span></div>
                <button onclick="paySlip('${fesc(a.code)}')" class="w-full bg-slate-800 text-white py-1.5 rounded-lg text-[10px] font-bold"><i class="fa-solid fa-print"></i> ພິມໃບເງິນເດືອນ</button></div></details>`;
            }).join('');
            $id('payCount').textContent = `ພົບ ${shown} / ${accounts.length} ຄົນ (ຍອດລວມດ້ານລຸ່ມຄິດທັງໝົດ)`;
            if (!shown) $id('payList').innerHTML = '<p class="text-center text-xs text-slate-400 py-4">ບໍ່ພົບພະນັກງານທີ່ຄົ້ນຫາ</p>';
            $id('paySum').innerHTML = `ເງິນໄດ້ລວມ: <b>${fmt(T.gross)}</b> · ລາຍການຫັກ (ຂາດ/ເບີກ/ອື່ນ): <b>${fmt(T.ded)}</b><br>ປະກັນສັງຄົມ: <b>${fmt(T.sso)}</b> · ພາສີລາຍໄດ້: <b>${fmt(T.tax)}</b><br>ເງິນສຸດທິຕ້ອງຈ່າຍ: <b class="text-amber-300">${fmt(T.net)}</b> ກີບ`;
        }
        function payEdit(code, kind, k, v) {
            const m = pm(), val = parseFloat(v) || 0;
            if (kind === 's') { fin.salaries[code] = { ...(fin.salaries[code] || {}), [k]: val }; }
            else { const rec = pRec(m); if (rec.status !== 'draft') return; rec.rows[code] = { ...(rec.rows[code] || {}), [k]: val }; }
            finSave(); renderPayroll();
        }
        function payApprove() {
            const rec = pRec(pm());
            if (rec.status === 'draft') { if (!confirm('ອະນຸມັດງວດນີ້? ຫຼັງອະນຸມັດຈະແກ້ໄຂລາຍການປະຈຳງວດບໍ່ໄດ້.')) return; rec.status = 'approved'; rec.approvedAt = new Date().toISOString(); }
            else { if (!confirm('ປົດລັອກງວດນີ້ກັບໄປເປັນຮ່າງ?')) return; rec.status = 'draft'; Object.values(rec.rows).forEach(r => r.paid = false); }
            finSave(); renderPayroll();
        }
        function payMarkPaid() {
            const rec = pRec(pm()); if (rec.status === 'draft') return showToast('ກະລຸນາອະນຸມັດງວດກ່ອນຈ່າຍ', true);
            if (!confirm('ຢືນຢັນວ່າຈ່າຍເງິນເດືອນທຸກຄົນແລ້ວ?')) return;
            accounts.forEach(a => { rec.rows[a.code] = { ...(rec.rows[a.code] || {}), paid: true }; }); rec.status = 'paid'; rec.paidAt = new Date().toISOString(); finSave(); renderPayroll();
        }
        const PCSS = "<link href='https://fonts.googleapis.com/css2?family=Noto+Sans+Lao&display=swap' rel='stylesheet'><style>body{font-family:'Noto Sans Lao',sans-serif;font-size:12px;margin:18px}table{width:100%;border-collapse:collapse}td,th{border:1px solid #444;padding:4px 6px}th{background:#e8edf7}.r{text-align:right}.c{text-align:center}.sg{display:flex;justify-content:space-around;margin-top:40px;text-align:center}.sg div{width:30%}.sg i{display:block;height:60px}@page{size:A4 landscape;margin:10mm}</style>";
        function phead(c) {
            const logo = companyLogo || DEFAULT_ECI_LOGO_SVG;
            return `<div style="display:flex;align-items:center;justify-content:center;gap:12px;border-bottom:2px solid #1e3a8a;padding-bottom:6px;margin-bottom:6px"><img src="${logo}" style="height:58px;max-width:100px;object-fit:contain"><h3 style="margin:0;font-size:18px;color:#1e3a8a">${fesc(c.co || 'ລັດວິສາຫະກິດ (ECI)')}</h3></div>`;
        }
        function openPrint(body) { const w = window.open('', '_blank'); if (!w) return showToast('ບຣາວເຊີບລັອກປັອບອັບ', true); w.document.write('<html><head><meta charset="utf-8">' + PCSS + '</head><body>' + body + '<script>onload=()=>setTimeout(print,500)<\/script></body></html>'); w.document.close(); }
        function paySlip(code) {
            const m = pm(), p = payRow(code, m), a = accounts.find(x => x.code === code), c = fin.cfg;
            const row = (l, v, neg) => `<tr><td>${l}</td><td class="r">${neg && v ? '-' : ''}${fmt(v)}</td></tr>`;
            openPrint(`<div style="max-width:460px;margin:auto"><style>@page{size:A5 portrait}</style>${phead(c)}<h4 class="c" style="margin:4px 0">ໃບເງິນເດືອນ / PAYSLIP</h4><p class="c">ເລກທີ: PS-${m.replace('-', '')}-${fesc(code)} · ງວດ ${m}</p><p>ຊື່: <b>${fesc(a.name)}</b> (${fesc(a.code)})<br>ພະແນກ: ${fesc(a.dept)} · ຕຳແໜ່ງ: ${fesc(a.position)}</p>
            <table><tr><th colspan="2">ລາຍຮັບ</th></tr>${row('ເງິນເດືອນຖານ', p.base)}${row('ເງິນປະຈຳຕຳແໜ່ງ', +p.sv.pos || 0)}${row('ຄ່າເດີນທາງ', +p.sv.trans || 0)}${row('ຄ່າອາຫານ', +p.sv.meal || 0)}${row('ເງິນອຸດໜູນອື່ນ', +p.sv.oth || 0)}${row('ຄ່າລ່ວງເວລາ (OT)', p.otPay)}${row('ໂບນັດ', p.bonus)}<tr><th>ລວມລາຍຮັບ</th><th class="r">${fmt(p.gross)}</th></tr>
            <tr><th colspan="2">ລາຍການຫັກ</th></tr>${row('ຂາດວຽກ/ລາບໍ່ມີເງິນ', p.absDed, 1)}${row('ປະກັນສັງຄົມ ' + c.sso + '%', p.sso, 1)}${row('ພາສີລາຍໄດ້ (ຖານ ' + fmt(p.taxable) + ')', p.tax, 1)}${row('ເບີກລ່ວງໜ້າ', p.adv, 1)}${row('ຫັກອື່ນໆ', p.ded, 1)}<tr><th>ເງິນສຸດທິທີ່ໄດ້ຮັບ (ກີບ)</th><th class="r">${fmt(p.net)}</th></tr></table>
            <div class="sg"><div><i></i>ຜູ້ຈັດທຳ<br>${fesc(c.s1)}</div><div><i></i>ຜູ້ຮັບເງິນ<br>${fesc(a.name)}</div></div></div>`);
        }
        function printSheet() {
            const m = pm(), c = fin.cfg; let T = [0, 0, 0, 0, 0, 0, 0];
            const rows = accounts.map((a, i) => { const p = payRow(a.code, m), v = [p.base + p.allow, p.otPay + p.bonus, p.gross, p.absDed + p.adv + p.ded, p.sso, p.tax, p.net]; v.forEach((x, j) => T[j] += x);
                return `<tr><td class="c">${i + 1}</td><td>${fesc(a.name)}<br><small>${fesc(a.code)}</small></td><td>${fesc(a.position)}</td>${v.map(x => `<td class="r">${fmt(x)}</td>`).join('')}<td></td></tr>`; }).join('');
            openPrint(`${phead(c)}<h4 class="c" style="margin:4px 0">ໃບສະຫຼຸບການຈ່າຍເງິນເດືອນ ງວດ ${m}</h4><p class="c">ສະຖານະ: ${{ draft: 'ຮ່າງ', approved: 'ອະນຸມັດແລ້ວ', paid: 'ຈ່າຍແລ້ວ' }[pRec(m).status]} · ພິມວັນທີ ${new Date().toLocaleDateString('lo-LA')}</p>
            <table><tr><th>ລ/ດ</th><th>ຊື່ ແລະ ນາມສະກຸນ</th><th>ຕຳແໜ່ງ</th><th>ເງິນເດືອນ+ເງິນອຸດໜູນ</th><th>OT+ໂບນັດ</th><th>ລວມລາຍຮັບ</th><th>ຫັກຂາດ/ເບີກ/ອື່ນ</th><th>ປະກັນສັງຄົມ</th><th>ພາສີ</th><th>ສຸດທິ</th><th>ລາຍເຊັນຜູ້ຮັບ</th></tr>${rows}<tr><th colspan="3">ລວມທັງໝົດ</th>${T.map(x => `<th class="r">${fmt(x)}</th>`).join('')}<th></th></tr></table>
            <div class="sg"><div><i></i>ຜູ້ຈັດທຳ<br>${fesc(c.s1)}</div><div><i></i>ຫົວໜ້າບັນຊີ<br>${fesc(c.s2)}</div><div><i></i>ຜູ້ອຳນວຍການ<br>${fesc(c.s3)}</div></div>`);
        }
        /* ===== Reports: yearly employee record & ledger prints ===== */
        const MN = ['ມັງກອນ', 'ກຸມພາ', 'ມີນາ', 'ເມສາ', 'ພຶດສະພາ', 'ມິຖຸນາ', 'ກໍລະກົດ', 'ສິງຫາ', 'ກັນຍາ', 'ຕຸລາ', 'ພະຈິກ', 'ທັນວາ'];
        const vals = p => [p.base + p.allow, p.otPay + p.bonus, p.gross, p.absDed + p.adv + p.ded, p.sso, p.tax, p.net];
        const HD = ['ເງິນເດືອນ+ເງິນອຸດໜູນ', 'OT+ໂບນັດ', 'ລວມລາຍຮັບ', 'ຫັກຂາດ/ເບີກ/ອື່ນ', 'ປະກັນສັງຄົມ', 'ພາສີລາຍໄດ້', 'ສຸດທິ'];
        const sgn = (c, third) => `<div class="sg"><div><i></i>ຜູ້ຈັດທຳ<br>${fesc(c.s1)}</div><div><i></i>ຫົວໜ້າບັນຊີ<br>${fesc(c.s2)}</div><div><i></i>${third}</div></div>`;
        const stLbl = (m, code) => { const rec = fin.pay2[m]; return rec.rows[code] && rec.rows[code].paid ? 'ຈ່າຍແລ້ວ' : ({ draft: 'ຮ່າງ', approved: 'ອະນຸມັດແລ້ວ', paid: 'ຈ່າຍແລ້ວ' }[rec.status]); };
        const repSel = new Set();
        function repToggle() { const t = $id('repType').value; $id('repBoxMonth').classList.toggle('hidden', t !== 'month'); $id('repBoxYear').classList.toggle('hidden', t !== 'year'); $id('repBoxRange').classList.toggle('hidden', t !== 'range'); }
        function repFiltered() { const q = ($id('repEmpSearch').value || '').trim().toLowerCase(); return accounts.filter(a => !q || [a.name, a.code, a.position, a.dept].some(x => String(x || '').toLowerCase().includes(q))); }
        function repEmpRender() {
            $id('repEmpList').innerHTML = repFiltered().map(a => `<label class="flex items-center gap-2 px-2 py-1.5 text-[11px] cursor-pointer"><input type="checkbox" ${repSel.has(a.code) ? 'checked' : ''} onchange="repPick('${fesc(a.code)}',this.checked)"><span class="flex-1 truncate">${fesc(a.name)}</span><span class="text-[9px] text-slate-400">${fesc(a.code)}</span></label>`).join('') || '<p class="text-center text-[10px] text-slate-400 py-2">ບໍ່ພົບ</p>';
            $id('repSelCount').textContent = `(ເລືອກແລ້ວ ${repSel.size} / ${accounts.length} ຄົນ)`;
        }
        function repPick(c, on) { on ? repSel.add(c) : repSel.delete(c); $id('repSelCount').textContent = `(ເລືອກແລ້ວ ${repSel.size} / ${accounts.length} ຄົນ)`; }
        function repAll(on) { (on ? repFiltered() : accounts).forEach(a => on ? repSel.add(a.code) : repSel.delete(a.code)); repEmpRender(); }
        function repPeriod() {
            const t = $id('repType').value, ms = x => String(x).padStart(2, '0');
            if (t === 'month') { const m = $id('repMonth').value; if (!m) return null; return { months: [m], title: 'ປະຈຳເດືອນ ' + MN[+m.slice(5) - 1] + ' ' + m.slice(0, 4) }; }
            if (t === 'year') { const y = $id('repYear').value; if (!/^\d{4}$/.test(y)) return null; return { months: MN.map((_, i) => y + '-' + ms(i + 1)), title: 'ປະຈຳປີ ' + y }; }
            const f = $id('repFrom').value, e = $id('repTo').value; if (!f || !e || f > e) return null;
            const out = []; let y = +f.slice(0, 4), mo = +f.slice(5);
            while ((y + '-' + ms(mo)) <= e && out.length < 60) { out.push(y + '-' + ms(mo)); if (++mo > 12) { mo = 1; y++; } }
            return { months: out, title: `ຊ່ວງ ${f} ຫາ ${e}` };
        }
        function printEmpReport() {
            loadCfg(); const per = repPeriod(); if (!per) return showToast('ກະລຸນາເລືອກເດືອນ/ປີ/ຊ່ວງໃຫ້ຖືກຕ້ອງ', true);
            const list = accounts.filter(a => repSel.has(a.code)); if (!list.length) return showToast('ກະລຸນາເລືອກພະນັກງານຢ່າງໜ້ອຍ 1 ຄົນ', true);
            const c = fin.cfg, T = Array(7).fill(0), incl = $id('repDraft').checked;
            const ok = m => fin.pay2[m] && (incl || fin.pay2[m].status !== 'draft'), ms = per.months.filter(ok);
            if (list.length === 1) {
                const a = list[0];
                const rows = per.months.map(m => { const lb = MN[+m.slice(5) - 1] + ' ' + m.slice(0, 4);
                    if (!ok(m)) return `<tr><td>${lb}</td><td class="c" colspan="8" style="color:#999">— ບໍ່ມີການບັນທຶກ —</td></tr>`;
                    const p = payRow(a.code, m), v = vals(p); v.forEach((x, j) => T[j] += x);
                    return `<tr><td>${lb}</td>${v.map(x => `<td class="r">${fmt(x)}</td>`).join('')}<td class="c">${stLbl(m, a.code)}</td></tr>`; }).join('');
                openPrint(`${phead(c)}<h4 class="c" style="margin:4px 0">ບັນທຶກເງິນເດືອນພະນັກງານ ${per.title}</h4><p>ຊື່: <b>${fesc(a.name)}</b> (${fesc(a.code)}) · ພະແນກ: ${fesc(a.dept)} · ຕຳແໜ່ງ: ${fesc(a.position)} · ເບີໂທ: ${fesc(a.phone)}</p>
                <table><tr><th>ເດືອນ</th>${HD.map(h => `<th>${h}</th>`).join('')}<th>ສະຖານະ</th></tr>${rows}<tr><th>ລວມ (${ms.length} ເດືອນ)</th>${T.map(x => `<th class="r">${fmt(x)}</th>`).join('')}<th></th></tr></table>${sgn(c, 'ພະນັກງານຮັບຮູ້<br>' + fesc(a.name))}`);
            } else {
                const rows = list.map((a, i) => { const t = Array(7).fill(0); ms.forEach(m => vals(payRow(a.code, m)).forEach((x, j) => t[j] += x)); t.forEach((x, j) => T[j] += x);
                    return `<tr><td class="c">${i + 1}</td><td>${fesc(a.name)}<br><small>${fesc(a.code)}</small></td><td>${fesc(a.position)}</td><td class="c">${ms.length}</td>${t.map(x => `<td class="r">${fmt(x)}</td>`).join('')}</tr>`; }).join('');
                openPrint(`${phead(c)}<h4 class="c" style="margin:4px 0">ສະຫຼຸບເງິນເດືອນພະນັກງານ ${per.title}</h4><p class="c">ພະນັກງານ ${list.length} ຄົນ · ເດືອນທີ່ມີຂໍ້ມູນ ${ms.length} ເດືອນ</p>
                <table><tr><th>ລ/ດ</th><th>ຊື່ ແລະ ນາມສະກຸນ</th><th>ຕຳແໜ່ງ</th><th>ຈຳນວນເດືອນ</th>${HD.map(h => `<th>${h}</th>`).join('')}</tr>${rows}<tr><th colspan="4">ລວມທັງໝົດ</th>${T.map(x => `<th class="r">${fmt(x)}</th>`).join('')}</tr></table>${sgn(c, 'ຜູ້ອຳນວຍການ<br>' + fesc(c.s3))}`);
            }
        }
        const lsum = rows => { const r = { i: 0, o: 0, ti: 0, to: 0 }; rows.forEach(l => l.type === 'in' ? (r.i += l.base, r.ti += l.tax) : (r.o += l.base, r.to += l.tax)); return r; };
        function printLedMonth() {
            loadCfg(); const m = $id('ledMonth').value, c = fin.cfg; if (!m) return showToast('ກະລຸນາເລືອກເດືອນ', true);
            const rows = fin.ledger.filter(l => (l.date || '').startsWith(m)).sort((a, b) => (a.date || '').localeCompare(b.date || '')), r = lsum(rows);
            openPrint(`${phead(c)}<h4 class="c" style="margin:4px 0">ລາຍງານລາຍຮັບ-ລາຍຈ່າຍ ປະຈຳເດືອນ ${fesc(m)}</h4>
            <table><tr><th>ລ/ດ</th><th>ວັນທີ</th><th>ປະເພດ</th><th>ໝວດ/ລາຍການ</th><th>ຍອດກ່ອນພາສີ</th><th>ພາສີ %</th><th>ພາສີ</th><th>ລວມ</th><th>ໝາຍເຫດ</th></tr>${rows.map((l, i) => `<tr><td class="c">${i + 1}</td><td>${fesc(l.date)}</td><td>${l.type === 'in' ? 'ລາຍຮັບ' : 'ລາຍຈ່າຍ'}</td><td>${fesc(l.cat)}</td><td class="r">${fmt(l.base)}</td><td class="c">${l.rate}</td><td class="r">${fmt(l.tax)}</td><td class="r">${fmt(l.base + l.tax)}</td><td>${fesc(l.note)}</td></tr>`).join('') || '<tr><td colspan="9" class="c">ບໍ່ມີລາຍການ</td></tr>'}</table>
            <table style="width:60%;margin-top:10px"><tr><th>ລາຍຮັບລວມ</th><td class="r">${fmt(r.i)}</td><th>ລາຍຈ່າຍລວມ</th><td class="r">${fmt(r.o)}</td></tr><tr><th>ກຳໄລ/ຂາດທຶນ</th><td class="r">${fmt(r.i - r.o)}</td><th>ພາສີຕ້ອງນຳສົ່ງ (ຮັບ-ຈ່າຍ)</th><td class="r">${fmt(r.ti - r.to)}</td></tr></table>${sgn(c, 'ຜູ້ອຳນວຍການ<br>' + fesc(c.s3))}`);
        }
        function printLedYear() {
            loadCfg(); const y = ($id('ledMonth').value || '').slice(0, 4), c = fin.cfg; if (!y) return showToast('ກະລຸນາເລືອກເດືອນ/ປີ', true);
            const T = { i: 0, o: 0, ti: 0, to: 0 };
            const rows = MN.map((mn, k) => { const r = lsum(fin.ledger.filter(l => (l.date || '').startsWith(y + '-' + String(k + 1).padStart(2, '0')))); Object.keys(T).forEach(x => T[x] += r[x]);
                return `<tr><td>${mn}</td><td class="r">${fmt(r.i)}</td><td class="r">${fmt(r.o)}</td><td class="r">${fmt(r.i - r.o)}</td><td class="r">${fmt(r.ti)}</td><td class="r">${fmt(r.to)}</td><td class="r">${fmt(r.ti - r.to)}</td></tr>`; }).join('');
            openPrint(`${phead(c)}<h4 class="c" style="margin:4px 0">ລາຍງານລາຍຮັບ-ລາຍຈ່າຍ ປະຈຳປີ ${y}</h4>
            <table><tr><th>ເດືອນ</th><th>ລາຍຮັບ</th><th>ລາຍຈ່າຍ</th><th>ກຳໄລສຸດທິ</th><th>ພາສີຈາກລາຍຮັບ</th><th>ພາສີຈາກລາຍຈ່າຍ</th><th>ພາສີຕ້ອງນຳສົ່ງ</th></tr>${rows}<tr><th>ລວມທັງປີ</th><th class="r">${fmt(T.i)}</th><th class="r">${fmt(T.o)}</th><th class="r">${fmt(T.i - T.o)}</th><th class="r">${fmt(T.ti)}</th><th class="r">${fmt(T.to)}</th><th class="r">${fmt(T.ti - T.to)}</th></tr></table>${sgn(c, 'ຜູ້ອຳນວຍການ<br>' + fesc(c.s3))}`);
        }
        /* Documents */
        function docPick(inp) {
            const f = inp.files[0]; if (!f) return;
            const rd = new FileReader();
            rd.onload = e => { const im = new Image(); im.onload = () => {
                const sc = Math.min(1, 1000 / Math.max(im.width, im.height)), c = document.createElement('canvas');
                c.width = im.width * sc; c.height = im.height * sc; c.getContext('2d').drawImage(im, 0, 0, c.width, c.height);
                docImg = c.toDataURL('image/jpeg', 0.6); $id('docPrev').src = docImg; $id('docPrev').classList.remove('hidden');
            }; im.src = e.target.result; };
            rd.readAsDataURL(f); inp.value = '';
        }
        let docFiles = [];
        const fIcon = n => { n = (n || '').toLowerCase(); return /\.pdf$/.test(n) ? '📕' : /\.docx?$/.test(n) ? '📘' : /\.(xlsx?|csv)$/.test(n) ? '📗' : /\.pptx?$/.test(n) ? '📙' : /\.(png|jpe?g|gif|webp|heic)$/.test(n) ? '🖼️' : /\.(zip|rar|7z)$/.test(n) ? '📦' : '📄'; };
        const fSize = b => b >= 1048576 ? (b / 1048576).toFixed(1) + ' MB' : Math.max(1, Math.round(b / 1024)) + ' KB';
        function docFilesPick(list) {
            for (const f of list) {
                if (docFiles.length >= 10) { showToast('ອັບໂຫຼດໄດ້ສູງສຸດ 10 ໄຟລ໌ຕໍ່ເອກະສານ', true); break; }
                if (f.size > 15 * 1048576) { showToast(`${f.name} ໃຫຍ່ເກີນ 15 MB`, true); continue; }
                docFiles.push(f);
            }
            docChips();
        }
        function docChips() {
            $id('docFileChips').innerHTML = docFiles.map((f, i) => `<span class="inline-flex items-center gap-1 bg-slate-100 rounded-lg px-2 py-1 text-[10px] max-w-full"><span class="truncate">${fIcon(f.name)} ${fesc(f.name)} · ${fSize(f.size)}</span><button type="button" onclick="docFiles.splice(${i},1);docChips()" class="text-rose-500 font-bold">✕</button></span>`).join('');
        }
        async function openDocFile(id, i) {
            const d = fin.docs.find(x => x.id === id), m = d && d.files && d.files[i]; if (!m) return;
            const b = await idbGet('file:' + id + ':' + i).catch(() => null);
            if (!b) return showToast('ບໍ່ພົບໄຟລ໌ໃນເຄື່ອງນີ້ (ໄຟລ໌ບໍ່ຊິງຄ໌ຂ້າມເຄື່ອງ) — ກູ້ຄືນຈາກໄຟລ໌ສຳຮອງ', true);
            const url = URL.createObjectURL(b);
            if (/pdf|image/.test(m.t || '')) window.open(url, '_blank'); else { const a = document.createElement('a'); a.href = url; a.download = m.n; a.click(); }
            setTimeout(() => URL.revokeObjectURL(url), 60000);
        }
        const blobUrl = b => new Promise(r => { const f = new FileReader(); f.onload = () => r(f.result); f.readAsDataURL(b); });
        function saveDoc() {
            const t = $id('docTitle').value.trim(); if (!t) return showToast('ກະລຸນາໃສ່ຊື່ເອກະສານ', true);
            const did = Date.now(); docFiles.forEach((f, i) => idbPut('file:' + did + ':' + i, f).catch(() => showToast('ບັນທຶກໄຟລ໌ ' + f.name + ' ບໍ່ສຳເລັດ', true))); if (docImg) idbPut('img:' + did, docImg).catch(() => showToast('ບັນທຶກຮູບບໍ່ສຳເລັດ', true)); fin.docs.unshift({ id: did, title: t, cat: $id('docCat').value, date: $id('docDate').value, note: $id('docNote').value, emp: $id('docEmp').value, hasImg: !!docImg, files: docFiles.map(f => ({ n: f.name, s: f.size, t: f.type })) });
            finSave(); docImg = ''; $id('docPrev').classList.add('hidden'); $id('docTitle').value = ''; $id('docNote').value = ''; renderDocs(); docFiles = []; docChips(); clearDraft(['docTitle', 'docNote']); showToast('ບັນທຶກເອກະສານແລ້ວ');
        }
        function renderDocs() {
            const q = ($id('docSearch').value || '').toLowerCase();
            $id('docList').innerHTML = fin.docs.filter(d => { const e = accounts.find(a => a.code === d.emp); return ((d.title + d.cat + d.note + d.date + (d.files || []).map(f => f.n).join(' ') + (e ? e.name + e.code : '')).toLowerCase().includes(q)) && (!$id('docFilCat').value || d.cat === $id('docFilCat').value) && (!$id('docFilEmp').value || d.emp === $id('docFilEmp').value); }).map(d => { const e = accounts.find(a => a.code === d.emp); d._e = e ? e.name : ''; return d; }).map(d => `<div class="bg-white p-2.5 rounded-2xl border border-slate-100 flex gap-2 items-center">
                ${d.hasImg ? `<img data-img="${d.id}" onclick="viewImg(${d.id})" class="w-14 h-14 object-cover rounded-xl border cursor-pointer bg-slate-100">` : '<div class="w-14 h-14 bg-slate-100 rounded-xl flex items-center justify-center"><i class="fa-solid fa-file text-slate-400"></i></div>'}
                <div class="flex-1 min-w-0"><b class="text-xs block truncate">${fesc(d.title)}</b><span class="text-[10px] text-slate-500">${fesc(d.cat)} · ${fesc(d.date)}${d._e ? ' · 👤 ' + fesc(d._e) : ''}</span><div class="text-[10px] text-slate-400 truncate">${fesc(d.note)}</div>${(d.files || []).length ? `<div class="flex flex-wrap gap-1 mt-1">${d.files.map((f, i) => `<button type="button" onclick="openDocFile(${d.id},${i})" class="bg-blue-50 text-blue-900 rounded-lg px-1.5 py-0.5 text-[10px] font-semibold max-w-full truncate">${fIcon(f.n)} ${fesc(f.n)} · ${fSize(f.s)}</button>`).join('')}</div>` : ''}</div>
                <button onclick="delFin('docs',${d.id})" class="text-rose-500 text-xs px-2"><i class="fa-solid fa-trash"></i></button></div>`).join('') || '<p class="text-center text-xs text-slate-400">ບໍ່ພົບເອກະສານ</p>';
            $id('docCount').textContent = `ພົບ ${$id('docList').children.length && fin.docs.length ? $id('docList').querySelectorAll('button').length : 0} / ${fin.docs.length} ລາຍການ`;
            loadDocImgs();
        }
        function delFin(k, id) { if (!confirm('ລຶບລາຍການນີ້?')) return; if (k === 'docs') { const dd = fin.docs.find(x => x.id === id); idbDel('img:' + id); ((dd && dd.files) || []).forEach((_, i) => idbDel('file:' + id + ':' + i)); } fin[k] = fin[k].filter(x => x.id !== id); finSave(); renderFinance(); }
        /* Ledger */
        function saveLed() {
            const amt = parseFloat($id('ledAmt').value), cat = $id('ledCat').value.trim();
            if (!cat || !(amt > 0)) return showToast('ກະລຸນາໃສ່ໝວດ ແລະ ຈຳນວນເງິນ', true);
            const rate = parseFloat($id('ledTax').value) || 0, incl = $id('ledIncl').checked;
            const base = incl ? amt / (1 + rate / 100) : amt, tax = base * rate / 100;
            fin.ledger.unshift({ id: Date.now(), type: $id('ledType').value, date: $id('ledDate').value, cat, base, tax, rate, note: $id('ledNote').value });
            finSave(); ['ledCat', 'ledAmt', 'ledNote'].forEach(i => $id(i).value = ''); renderLed(); clearDraft(['ledCat', 'ledAmt', 'ledNote']); showToast('ບັນທຶກລາຍການແລ້ວ');
        }
        function ledMonthRows() { const m = $id('ledMonth').value; return fin.ledger.filter(l => !m || (l.date || '').startsWith(m)); }
        function renderLed() {
            const rows = ledMonthRows(); let inn = 0, out = 0, tin = 0, tout = 0;
            rows.forEach(l => l.type === 'in' ? (inn += l.base, tin += l.tax) : (out += l.base, tout += l.tax));
            const box = (l, v, c) => `<div class="bg-white p-2 rounded-xl border"><span class="text-slate-500 block">${l}</span><b class="${c}">${fmt(v)}</b></div>`;
            $id('ledSum').innerHTML = box('ລາຍຮັບ', inn, 'text-emerald-600') + box('ລາຍຈ່າຍ', out, 'text-rose-600') + box('ກຳໄລສຸດທິ', inn - out, 'text-blue-900') + box('ພາສີຕ້ອງນຳສົ່ງ (ຮັບ-ຈ່າຍ)', tin - tout, 'text-amber-600');
            $id('ledList').innerHTML = rows.map(l => `<div class="bg-white p-2 rounded-xl border flex items-center gap-2 text-[11px]"><span class="${l.type === 'in' ? 'text-emerald-600' : 'text-rose-600'} font-bold">${l.type === 'in' ? '＋' : '－'}</span>
                <div class="flex-1 min-w-0"><b class="block truncate">${fesc(l.cat)}</b><span class="text-[9px] text-slate-500">${fesc(l.date)} · ພາສີ ${l.rate}% = ${fmt(l.tax)} ${fesc(l.note)}</span></div>
                <b>${fmt(l.base + l.tax)}</b><button onclick="delFin('ledger',${l.id})" class="text-rose-500 px-1"><i class="fa-solid fa-trash"></i></button></div>`).join('') || '<p class="text-center text-xs text-slate-400">ຍັງບໍ່ມີລາຍການ</p>';
        }
        function exportLed() {
            const csv = '\ufeffວັນທີ,ປະເພດ,ໝວດ,ຍອດກ່ອນພາສີ,ພາສີ%,ພາສີ,ລວມ,ໝາຍເຫດ\n' + ledMonthRows().map(l => [l.date, l.type === 'in' ? 'ລາຍຮັບ' : 'ລາຍຈ່າຍ', l.cat, Math.round(l.base), l.rate, Math.round(l.tax), Math.round(l.base + l.tax), l.note].map(v => '"' + String(v).replace(/"/g, '""') + '"').join(',')).join('\n');
            const a = document.createElement('a'); a.href = URL.createObjectURL(new Blob([csv], { type: 'text/csv' })); a.download = 'ECI_ledger_' + $id('ledMonth').value + '.csv'; a.click();
        }

        function switchAdminTab(tab) {
            stopAdminCam();
            const tabs = {
                employees: ['adminTabEmployees', 'adminTabEmployeesBtn'],
                logs: ['adminTabLogs', 'adminTabLogsBtn'],
                scanner: ['adminTabScanner', 'adminTabScannerBtn'],
                ads: ['adminTabAds', 'adminTabAdsBtn'],
                duties: ['adminTabDuties', 'adminTabDutiesBtn'],
                leaves: ['adminTabLeaves', 'adminTabLeavesBtn'],
                finance: ['adminTabFinance', 'adminTabFinanceBtn'],
                backup: ['adminTabBackup', 'adminTabBackupBtn']
            };
            Object.entries(tabs).forEach(([k, [panelId, btnId]]) => {
                const active = k === tab;
                $id(panelId).classList.toggle('hidden', !active);
                $id(btnId).className = active
                    ? 'py-2.5 rounded-xl font-bold bg-blue-900 text-white flex flex-col items-center gap-1 shadow relative'
                    : 'py-2.5 rounded-xl font-semibold text-slate-600 bg-slate-50 flex flex-col items-center gap-1 relative';
            });
            if (tab === 'scanner') { drawPointQr(); applyShiftTimeLabels(); }
            if (tab === 'ads') renderAdminAds();
            try { localStorage.setItem('eci_admin_tab', tab); } catch (e) {}
            if (tab === 'finance') initFinance();
            if (tab === 'backup') updateBackupInfo();
        }
        drawPin();
    </script>
</body>
</html>
