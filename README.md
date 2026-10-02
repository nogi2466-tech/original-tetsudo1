<!DOCTYPE html>
<html lang="ja" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>紫句守鉄道（しのもり鉄道） 総合統合システム</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        shinmori: {
                            50: '#f5f3ff',
                            100: '#ede9fe',
                            400: '#a78bfa',
                            500: '#8b5cf6',
                            600: '#7c3aed',
                            800: '#5b21b6',
                            900: '#4c1d95',
                            950: '#2e1065',
                        }
                    }
                }
            }
        }
    </script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=M+PLUS+Rounded+1c:wght@400;500;700;800&family=Share+Tech+Mono&display=swap" rel="stylesheet">
    
    <style>
        body {
            font-family: 'M PLUS Rounded 1c', sans-serif;
            background-color: #080511;
            color: #f1f5f9;
            user-select: none;
            overflow: hidden;
        }
        .font-mono-num {
            font-family: 'Share Tech Mono', monospace;
        }
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #0f0a1f;
        }
        ::-webkit-scrollbar-thumb {
            background: #3b2d54;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #583f80;
        }
        .glass-panel {
            background: rgba(18, 12, 33, 0.85);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(139, 92, 246, 0.25);
        }
        .purple-glow {
            box-shadow: 0 0 20px rgba(168, 85, 247, 0.35);
        }
        .led-display {
            background-color: #05030a;
            border: 1px solid #3b0764;
            box-shadow: inset 0 0 10px rgba(0,0,0,0.9);
        }
        /* 種別バッジカラー定義 */
        .badge-tokkyu { background-color: #dc2626; color: white; }
        .badge-tsukin-kyuko { background-color: #c026d3; color: white; }
        .badge-kyuko { background-color: #ea580c; color: white; }
        .badge-tsukin-kaisoku { background-color: #0284c7; color: white; }
        .badge-kaisoku { background-color: #16a34a; color: white; }
        .badge-junkyu { background-color: #0d9488; color: white; }
        .badge-futsu { background-color: #475569; color: white; }
    </style>
</head>
<body class="h-screen flex flex-col bg-slate-950 text-slate-100">

    <!-- HEADER NAVIGATION & STATUS -->
    <header class="glass-panel border-b border-purple-900/40 px-4 py-2.5 flex justify-between items-center z-30 shrink-0">
        <div class="flex items-center space-x-3">
            <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-purple-600 via-indigo-600 to-amber-500 flex items-center justify-center shadow-lg purple-glow">
                <i class="fa-solid fa-train-subway text-xl text-amber-300"></i>
            </div>
            <div>
                <div class="flex items-center space-x-2">
                    <h1 class="text-lg font-extrabold tracking-wide bg-gradient-to-r from-purple-300 via-violet-200 to-amber-300 bg-clip-text text-transparent">
                        紫句守鉄道
                    </h1>
                    <span class="text-xs text-purple-300 font-medium px-2 py-0.5 rounded-full bg-purple-950/80 border border-purple-700">しのもり鉄道</span>
                </div>
                <p class="text-[10px] text-slate-400">総合統合ダイヤ・運行＆自動同期管理システム</p>
            </div>
        </div>

        <div class="hidden md:flex items-center space-x-4 text-xs">
            <div id="sync-status-badge" class="flex items-center space-x-2 bg-emerald-950/80 px-3 py-1.5 rounded-xl border border-emerald-700/50 text-emerald-300">
                <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
                <span id="sync-status-text" class="font-bold">同期準備完了</span>
            </div>
            <div class="flex items-center space-x-2 bg-slate-900/80 px-3 py-1.5 rounded-xl border border-purple-900/40">
                <i class="fa-solid fa-key text-amber-400"></i>
                <span class="text-slate-400">ルームID:</span>
                <span id="sync-room-display" class="font-mono-num font-bold text-amber-300">SHINMORI-ONLINE</span>
            </div>
            <div class="flex items-center space-x-2 bg-slate-900/80 px-3 py-1.5 rounded-xl border border-purple-900/40">
                <i class="fa-solid fa-clock text-cyan-400"></i>
                <span id="system-clock" class="font-bold font-mono-num text-cyan-300 text-sm">12:00:00</span>
            </div>
        </div>

        <div class="flex items-center space-x-2">
            <button onclick="exportDataJSON()" class="px-3 py-1.5 rounded-xl bg-purple-950/80 hover:bg-purple-900 text-purple-300 border border-purple-700/50 text-xs font-bold transition flex items-center gap-1.5" title="JSON保存">
                <i class="fa-solid fa-download"></i> <span class="hidden sm:inline">保存</span>
            </button>
            <button onclick="triggerImportJSON()" class="px-3 py-1.5 rounded-xl bg-purple-950/80 hover:bg-purple-900 text-purple-300 border border-purple-700/50 text-xs font-bold transition flex items-center gap-1.5" title="JSON復元">
                <i class="fa-solid fa-upload"></i> <span class="hidden sm:inline">復元</span>
            </button>
            <input type="file" id="json-file-input" class="hidden" accept=".json" onchange="importDataJSON(event)">
            
            <button id="btn-sound-toggle" onclick="toggleAudioMaster()" class="p-2.5 rounded-xl bg-slate-900 hover:bg-slate-800 border border-purple-800/50 text-slate-300 hover:text-white transition">
                <i id="icon-sound" class="fa-solid fa-volume-xmark text-red-400"></i>
            </button>
        </div>
    </header>

    <!-- MAIN WRAPPER (SIDEBAR + CONTENT PANELS) -->
    <div class="flex-1 flex overflow-hidden">
        <!-- Sidebar Navigation (8 Tabs) -->
        <nav class="w-16 md:w-56 glass-panel border-r border-purple-900/40 flex flex-col justify-between shrink-0 z-20 overflow-y-auto">
            <div class="p-2 space-y-1">
                <button data-tab="tab-about" class="nav-btn active w-full flex items-center space-x-3 px-3 py-2.5 rounded-xl text-left transition bg-purple-600/30 text-purple-300 border border-purple-500/40">
                    <i class="fa-solid fa-building text-base w-6 text-center text-purple-400"></i>
                    <span class="hidden md:inline font-bold text-xs">1. 会社について</span>
                </button>
                <button data-tab="tab-timetable" class="nav-btn w-full flex items-center space-x-3 px-3 py-2.5 rounded-xl text-left transition text-slate-400 hover:bg-purple-900/30 hover:text-slate-200">
                    <i class="fa-solid fa-calendar-days text-base w-6 text-center text-amber-400"></i>
                    <span class="hidden md:inline font-bold text-xs">2. 時刻表検索</span>
                </button>
                <button data-tab="tab-live-sim" class="nav-btn w-full flex items-center space-x-3 px-3 py-2.5 rounded-xl text-left transition text-slate-400 hover:bg-purple-900/30 hover:text-slate-200">
                    <i class="fa-solid fa-satellite text-base w-6 text-center text-cyan-400"></i>
                    <span class="hidden md:inline font-bold text-xs">3. 走行位置＆運転シミュ</span>
                </button>
                <button data-tab="tab-fleet-info" class="nav-btn w-full flex items-center space-x-3 px-3 py-2.5 rounded-xl text-left transition text-slate-400 hover:bg-purple-900/30 hover:text-slate-200">
                    <i class="fa-solid fa-train-subway text-base w-6 text-center text-emerald-400"></i>
                    <span class="hidden md:inline font-bold text-xs">4. 車両形式情報</span>
                </button>
                <button data-tab="tab-add-train" class="nav-btn w-full flex items-center space-x-3 px-3 py-2.5 rounded-xl text-left transition text-slate-400 hover:bg-purple-900/30 hover:text-slate-200">
                    <i class="fa-solid fa-plus-circle text-base w-6 text-center text-pink-400"></i>
                    <span class="hidden md:inline font-bold text-xs">5. 列車追加</span>
                </button>
                <button data-tab="tab-formation" class="nav-btn w-full flex items-center space-x-3 px-3 py-2.5 rounded-xl text-left transition text-slate-400 hover:bg-purple-900/30 hover:text-slate-200">
                    <i class="fa-solid fa-cubes text-base w-6 text-center text-indigo-400"></i>
                    <span class="hidden md:inline font-bold text-xs">6. 号車別編成表</span>
                </button>
                <button data-tab="tab-diagram" class="nav-btn w-full flex items-center space-x-3 px-3 py-2.5 rounded-xl text-left transition text-slate-400 hover:bg-purple-900/30 hover:text-slate-200">
                    <i class="fa-solid fa-chart-line text-base w-6 text-center text-orange-400"></i>
                    <span class="hidden md:inline font-bold text-xs">7. ダイヤ表 (スジ描画)</span>
                </button>
                <button data-tab="tab-settings" class="nav-btn w-full flex items-center space-x-3 px-3 py-2.5 rounded-xl text-left transition text-slate-400 hover:bg-purple-900/30 hover:text-slate-200">
                    <i class="fa-solid fa-gear text-base w-6 text-center text-slate-400"></i>
                    <span class="hidden md:inline font-bold text-xs">8. 同期＆システム設定</span>
                </button>
            </div>

            <!-- Route Summary Legend -->
            <div class="p-3 border-t border-purple-900/40 hidden md:block text-[11px] text-slate-400 space-y-1">
                <div class="font-bold text-purple-300 mb-1">管轄 5 路線 (全60駅)</div>
                <div class="flex justify-between items-center"><span class="flex items-center gap-1.5"><span class="w-2 h-2 rounded-full bg-slate-400"></span>直通線</span><span class="text-slate-300 font-mono-num">09〜01</span></div>
                <div class="flex justify-between items-center"><span class="flex items-center gap-1.5"><span class="w-2 h-2 rounded-full bg-purple-500"></span>紫雲本線</span><span class="text-purple-300 font-mono-num">1〜30</span></div>
                <div class="flex justify-between items-center"><span class="flex items-center gap-1.5"><span class="w-2 h-2 rounded-full bg-amber-500"></span>星句高原線</span><span class="text-amber-300 font-mono-num">31〜40</span></div>
                <div class="flex justify-between items-center"><span class="flex items-center gap-1.5"><span class="w-2 h-2 rounded-full bg-emerald-500"></span>句守支線</span><span class="text-emerald-300 font-mono-num">41〜55</span></div>
                <div class="flex justify-between items-center"><span class="flex items-center gap-1.5"><span class="w-2 h-2 rounded-full bg-cyan-500"></span>紫霞観光線</span><span class="text-cyan-300 font-mono-num">56〜60</span></div>
            </div>
        </nav>

        <!-- MAIN DISPLAY PANELS -->
        <main class="flex-1 relative overflow-hidden bg-slate-950">

            <!-- TAB 1: 会社について -->
            <div id="tab-about" class="tab-content h-full p-4 overflow-y-auto space-y-4">
                <div class="glass-panel p-6 rounded-2xl border border-purple-900/40 relative overflow-hidden">
                    <div class="absolute right-0 top-0 opacity-10 text-9xl text-purple-500 p-4">
                        <i class="fa-solid fa-train"></i>
                    </div>
                    <span class="px-3 py-1 rounded-full bg-purple-900/80 text-purple-300 text-xs font-bold border border-purple-700">鉄道事業者情報</span>
                    <h2 class="text-2xl font-black text-white mt-2 mb-2">紫句守鉄道株式会社 <span class="text-sm font-normal text-purple-300">（しのもりてつどう）</span></h2>
                    <p class="text-xs text-slate-300 leading-relaxed max-w-3xl">
                        紫句守鉄道（通称: しのもり鉄道）は、都心ターミナルの紫句守中央駅を中心に、全5路線・総計60駅を展開する広域都市・観光鉄道ネットワークです。本線による速達輸送、自然豊かな星句高原線や紫霞観光線へのアクセス観光輸送、港湾・研究所を結ぶ句守支線など、多様なニーズに応えるスマートで快適な輸送サービスを提供しています。
                    </p>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                    <div class="glass-panel p-4 rounded-2xl border border-purple-900/30 space-y-2">
                        <div class="flex items-center space-x-2 text-purple-300 font-bold text-sm">
                            <i class="fa-solid fa-route"></i>
                            <span>紫雲本線（駅番号 1～30）</span>
                        </div>
                        <p class="text-xs text-slate-400">
                            紫句守中央から紫句守展示場までを結ぶ主要幹線。特急「シノモリライナー」をはじめ、多彩な種別が高頻度で運行されています。
                        </p>
                    </div>
                    <div class="glass-panel p-4 rounded-2xl border border-purple-900/30 space-y-2">
                        <div class="flex items-center space-x-2 text-amber-300 font-bold text-sm">
                            <i class="fa-solid fa-mountain"></i>
                            <span>星句高原線（駅番号 31～40）</span>
                        </div>
                        <p class="text-xs text-slate-400">
                            星句高原入口から星句高原までの山岳・リゾート路線。温泉街や牧場、湖畔への観光アクセス路線として親しまれています。
                        </p>
                    </div>
                    <div class="glass-panel p-4 rounded-2xl border border-purple-900/30 space-y-2">
                        <div class="flex items-center space-x-2 text-emerald-300 font-bold text-sm">
                            <i class="fa-solid fa-industry"></i>
                            <span>句守支線（駅番号 41～55）</span>
                        </div>
                        <p class="text-xs text-slate-400">
                            紫句守港や研究所、未来都市を結ぶ先端産業・ベイエリアアクセス線。朝夕の通勤特急・急行が多数活躍します。
                        </p>
                    </div>
                    <div class="glass-panel p-4 rounded-2xl border border-purple-900/30 space-y-2">
                        <div class="flex items-center space-x-2 text-cyan-300 font-bold text-sm">
                            <i class="fa-solid fa-camera font-bold"></i>
                            <span>紫霞観光線（駅番号 56～60）</span>
                        </div>
                        <p class="text-xs text-slate-400">
                            展望台や詩碑前など、歴史と絶景を巡る特別観光ライン。四季折々の車窓風景を楽しめるリゾート列車も走ります。
                        </p>
                    </div>
                    <div class="glass-panel p-4 rounded-2xl border border-purple-900/30 space-y-2">
                        <div class="flex items-center space-x-2 text-slate-300 font-bold text-sm">
                            <i class="fa-solid fa-link"></i>
                            <span>他社直通線（駅番号 09～1）</span>
                        </div>
                        <p class="text-xs text-slate-400">
                            水鳥湿原方面からの相互直通運転ルート。乗り換えなしでの都心アクセスを実現する利便性の高いラインです。
                        </p>
                    </div>
                    <div class="glass-panel p-4 rounded-2xl border border-purple-900/30 space-y-2 bg-gradient-to-br from-purple-900/20 to-slate-900">
                        <div class="flex items-center space-x-2 text-pink-300 font-bold text-sm">
                            <i class="fa-solid fa-microchip"></i>
                            <span>統合次世代運行システム</span>
                        </div>
                        <p class="text-xs text-slate-400">
                            全端末クラウド同期、リアルタイム自動スジ描画、Web Audio音響合成、スマート時刻表エンジンを搭載。
                        </p>
                    </div>
                </div>
            </div>

            <!-- TAB 2: 時刻表検索 -->
            <div id="tab-timetable" class="tab-content hidden h-full p-4 flex flex-col space-y-3 overflow-hidden">
                <div class="glass-panel p-3 rounded-2xl flex flex-wrap justify-between items-center gap-3 shrink-0 border border-purple-900/40">
                    <div class="flex items-center space-x-3">
                        <i class="fa-solid fa-clock text-amber-400 text-lg"></i>
                        <div>
                            <h2 class="text-sm font-bold text-white">全60駅 インタラクティブ時刻表検索</h2>
                            <p class="text-[11px] text-slate-400">検索する駅を選択し、発車ダイヤと種別・行先を確認できます</p>
                        </div>
                    </div>
                    <div class="flex items-center space-x-2">
                        <label class="text-xs text-purple-300 font-bold">対象駅:</label>
                        <select id="tt-station-select" onchange="renderTimetable()" class="bg-slate-900 border border-purple-800 rounded-xl px-3 py-1.5 text-xs text-white font-bold">
                            <!-- JS Populates Stations -->
                        </select>
                    </div>
                </div>

                <div class="flex-1 glass-panel rounded-2xl p-4 overflow-y-auto border border-purple-900/30">
                    <div class="flex justify-between items-center mb-3 pb-2 border-b border-purple-900/40">
                        <span id="tt-station-title" class="font-extrabold text-base text-purple-200">1 紫句守中央 発車時刻表</span>
                        <div class="flex space-x-2 text-xs">
                            <span class="badge-tokkyu px-2 py-0.5 rounded font-bold">特急</span>
                            <span class="badge-tsukin-kyuko px-2 py-0.5 rounded font-bold">通勤急行</span>
                            <span class="badge-kyuko px-2 py-0.5 rounded font-bold">急行</span>
                            <span class="badge-tsukin-kaisoku px-2 py-0.5 rounded font-bold">通勤快速</span>
                            <span class="badge-kaisoku px-2 py-0.5 rounded font-bold">快速</span>
                            <span class="badge-junkyu px-2 py-0.5 rounded font-bold">準急</span>
                            <span class="badge-futsu px-2 py-0.5 rounded font-bold">普通</span>
                        </div>
                    </div>

                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-xs border-collapse">
                            <thead>
                                <tr class="border-b border-purple-900/40 text-slate-400 bg-slate-900/90">
                                    <th class="p-2 w-16 text-center">時</th>
                                    <th class="p-2">発車分・種別・行先・編成・スジ番号</th>
                                </tr>
                            </thead>
                            <tbody id="timetable-rows" class="divide-y divide-purple-900/20 font-mono-num">
                                <!-- JS Populated -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- TAB 3: 走行位置＆運転シミュレーター -->
            <div id="tab-live-sim" class="tab-content hidden h-full p-3 flex flex-col space-y-3 overflow-y-auto">
                <!-- Map / Driver Selector Toggle -->
                <div class="glass-panel p-3 rounded-xl flex justify-between items-center shrink-0 border border-purple-900/30">
                    <div class="flex items-center space-x-3">
                        <select id="driver-train-select" onchange="onDriverTrainChange()" class="bg-slate-900 border border-purple-800 rounded-lg p-2 text-xs text-white font-bold">
                            <!-- JS Populates Train List -->
                        </select>
                        <span id="sim-train-badge" class="badge-tokkyu px-2 py-0.5 rounded text-xs font-bold">特急</span>
                        <span id="sim-route-desc" class="text-xs text-slate-300 font-bold">紫句守中央 行</span>
                    </div>

                    <div class="led-display px-3 py-1 rounded-lg flex items-center space-x-4 text-xs font-mono-num">
                        <div><span class="text-slate-500">次駅:</span><span id="sim-next-st" class="text-amber-400 font-bold ml-1">3 紫雲野</span></div>
                        <div><span class="text-slate-500">定刻:</span><span id="sim-target-time" class="text-emerald-400 font-bold ml-1">12:05:00</span></div>
                    </div>
                </div>

                <!-- Upper Visual Canvas Area -->
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-3 shrink-0">
                    <!-- Network Real-Time Visualizer -->
                    <div class="glass-panel h-52 rounded-2xl relative overflow-hidden border border-purple-900/40">
                        <canvas id="network-canvas" class="w-full h-full block bg-slate-950"></canvas>
                        <div class="absolute top-2 left-2 bg-slate-900/90 px-2.5 py-1 rounded-lg text-[10px] text-purple-300 border border-purple-800/40 font-bold">
                            全線リアルタイム位置モニター
                        </div>
                    </div>

                    <!-- Driving Simulator Cab Canvas -->
                    <div class="glass-panel h-52 rounded-2xl relative overflow-hidden border border-purple-900/40">
                        <canvas id="cab-canvas" class="w-full h-full block"></canvas>
                        <div class="absolute top-2 left-2 bg-slate-950/85 px-3 py-2 rounded-xl border border-purple-800/40 flex items-center space-x-3 backdrop-blur-md">
                            <div class="text-center">
                                <div class="text-[9px] text-slate-400 font-bold">速度 SPEED</div>
                                <div class="text-2xl font-extrabold font-mono-num text-cyan-400" id="driver-speed">0</div>
                                <div class="text-[8px] text-slate-400">km/h</div>
                            </div>
                            <div class="h-6 w-[1px] bg-slate-800"></div>
                            <div class="text-center">
                                <div class="text-[9px] text-slate-400 font-bold">残り距離 DIST</div>
                                <div class="text-xl font-bold font-mono-num text-amber-400" id="driver-dist">850</div>
                                <div class="text-[8px] text-slate-400">m</div>
                            </div>
                        </div>

                        <div id="driver-arrival-msg" class="hidden absolute inset-0 bg-purple-950/90 backdrop-blur-md flex flex-col items-center justify-center space-y-2">
                            <div class="text-xl font-black text-amber-300">駅 停車完了！</div>
                            <div id="driver-stop-accuracy" class="text-xs font-mono-num text-white">停車位置誤差: +0.22m</div>
                            <button onclick="advanceNextStation()" class="px-4 py-1.5 bg-purple-600 hover:bg-purple-500 text-white font-bold text-xs rounded-xl shadow-lg">戸じめ・次駅発車</button>
                        </div>
                    </div>
                </div>

                <!-- Mascon & Audio Control Dashboard -->
                <div class="glass-panel rounded-2xl p-4 border border-purple-900/40 flex flex-col space-y-3 shrink-0">
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
                        <!-- Mascon Notch -->
                        <div class="bg-slate-900/80 p-3 rounded-xl border border-purple-900/30 space-y-2">
                            <div class="flex justify-between items-center text-xs">
                                <span class="font-bold text-slate-300">主正逆マスコン (P1-P5 / B1-B5 / EB)</span>
                                <span id="driver-notch-label" class="font-bold font-mono-num text-amber-400">N (切)</span>
                            </div>
                            <input type="range" id="driver-notch-slider" min="-6" max="5" value="0" step="1" oninput="onNotchChange(this.value)" class="w-full h-3 bg-slate-800 rounded appearance-none cursor-pointer accent-purple-500">
                            <div class="grid grid-cols-3 gap-1 text-xs">
                                <button onclick="stepNotch(-1)" class="py-1.5 bg-slate-800 hover:bg-slate-700 rounded text-slate-300 font-bold">Bブレーキ</button>
                                <button onclick="setNotch(0)" class="py-1.5 bg-slate-800 hover:bg-slate-700 rounded text-amber-400 font-bold">N 惰行</button>
                                <button onclick="stepNotch(1)" class="py-1.5 bg-purple-700 hover:bg-purple-600 rounded text-white font-bold">P力行</button>
                            </div>
                        </div>

                        <!-- Web Audio Sound Actions -->
                        <div class="bg-slate-900/80 p-3 rounded-xl border border-purple-900/30 space-y-2">
                            <div class="text-xs font-bold text-slate-300">自作 Web Audio API サウンド操作</div>
                            <div class="grid grid-cols-3 gap-1.5 text-xs">
                                <button onclick="playAudioHorn()" class="py-2 bg-amber-600 hover:bg-amber-500 text-white font-bold rounded-xl transition active:scale-95 text-[11px]">
                                    <i class="fa-solid fa-bullhorn"></i> 警笛
                                </button>
                                <button onclick="playAudioChime()" class="py-2 bg-purple-800 hover:bg-purple-700 text-purple-200 font-bold rounded-xl transition active:scale-95 text-[11px]">
                                    <i class="fa-solid fa-music"></i> 発車メロディ
                                </button>
                                <button onclick="playAudioDoor()" class="py-2 bg-indigo-800 hover:bg-indigo-700 text-indigo-200 font-bold rounded-xl transition active:scale-95 text-[11px]">
                                    <i class="fa-solid fa-door-open"></i> ドア開閉
                                </button>
                            </div>
                        </div>

                        <!-- Fleet Specifications -->
                        <div class="bg-slate-900/80 p-3 rounded-xl border border-purple-900/30 text-xs space-y-1">
                            <div class="font-bold text-purple-300">選択中車両スペック</div>
                            <div id="sim-spec-info" class="text-slate-300 space-y-0.5 text-[11px]">
                                <div>形式: S100系 (看板特急)</div>
                                <div>最高営業速度: 120 km/h</div>
                                <div>制御方式: 高効率VVVFインバータ</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- TAB 4: 車両形式情報 -->
            <div id="tab-fleet-info" class="tab-content hidden h-full p-4 overflow-y-auto space-y-4">
                <div class="flex justify-between items-center">
                    <div>
                        <h2 class="text-lg font-bold text-white flex items-center gap-2">
                            <i class="fa-solid fa-train-subway text-emerald-400"></i> 紫句守鉄道 車両形式・グラフィック図鑑
                        </h2>
                        <p class="text-xs text-slate-400">各形式のSVGグラフィック、運用規定および編成情報</p>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4" id="fleet-cards-container">
                    <!-- JS Populated Fleet Cards -->
                </div>
            </div>

            <!-- TAB 5: 列車追加 -->
            <div id="tab-add-train" class="tab-content hidden h-full p-4 overflow-y-auto space-y-4">
                <div class="glass-panel p-6 rounded-2xl border border-purple-900/40 max-w-2xl mx-auto space-y-4">
                    <div class="flex items-center space-x-3 border-b border-purple-900/40 pb-3">
                        <i class="fa-solid fa-plus-circle text-2xl text-pink-400"></i>
                        <div>
                            <h2 class="text-base font-bold text-white">新規スジ・運用列車の追加登録</h2>
                            <p class="text-xs text-slate-400">作成された列車データは全端末の時刻表・ダイヤ表へリアルタイム即時反映されます</p>
                        </div>
                    </div>

                    <form id="add-train-form" onsubmit="handleTrainSubmit(event)" class="space-y-4 text-xs">
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div>
                                <label class="block font-bold text-purple-300 mb-1">列車番号 / ID</label>
                                <input type="text" id="input-train-id" required placeholder="例: 1005M" class="w-full bg-slate-900 border border-purple-800 rounded-xl px-3 py-2 text-white font-mono-num font-bold">
                            </div>

                            <div>
                                <label class="block font-bold text-purple-300 mb-1">列車種別</label>
                                <select id="input-train-class" required onchange="updateFormCarOptions()" class="w-full bg-slate-900 border border-purple-800 rounded-xl px-3 py-2 text-white font-bold">
                                    <option value="特急">特急</option>
                                    <option value="通勤急行">通勤急行</option>
                                    <option value="急行">急行</option>
                                    <option value="通勤快速">通勤快速</option>
                                    <option value="快速">快速</option>
                                    <option value="準急">準急</option>
                                    <option value="普通" selected>普通</option>
                                </select>
                            </div>

                            <div>
                                <label class="block font-bold text-purple-300 mb-1">編成形式</label>
                                <select id="input-train-series" required class="w-full bg-slate-900 border border-purple-800 rounded-xl px-3 py-2 text-white font-bold">
                                    <option value="S1系">S1系</option>
                                    <option value="S2系">S2系</option>
                                    <option value="S3系">S3系</option>
                                    <option value="S4系">S4系</option>
                                    <option value="S5系">S5系</option>
                                    <option value="S100系">S100系 (特急)</option>
                                    <option value="S900系">S900系 (事業用)</option>
                                </select>
                            </div>

                            <div>
                                <label class="block font-bold text-purple-300 mb-1">編成両数 (種別制限連動)</label>
                                <select id="input-train-cars" required class="w-full bg-slate-900 border border-purple-800 rounded-xl px-3 py-2 text-white font-bold">
                                    <!-- JS populated dynamically -->
                                </select>
                            </div>

                            <div>
                                <label class="block font-bold text-purple-300 mb-1">始発駅</label>
                                <select id="input-start-st" required class="w-full bg-slate-900 border border-purple-800 rounded-xl px-3 py-2 text-white font-bold">
                                    <!-- JS populated -->
                                </select>
                            </div>

                            <div>
                                <label class="block font-bold text-purple-300 mb-1">終着駅</label>
                                <select id="input-end-st" required class="w-full bg-slate-900 border border-purple-800 rounded-xl px-3 py-2 text-white font-bold">
                                    <!-- JS populated -->
                                </select>
                            </div>

                            <div class="sm:col-span-2">
                                <label class="block font-bold text-purple-300 mb-1">始発時刻 (HH:MM)</label>
                                <input type="time" id="input-dep-time" required value="08:00" class="w-full bg-slate-900 border border-purple-800 rounded-xl px-3 py-2 text-white font-mono-num font-bold">
                            </div>
                        </div>

                        <button type="submit" class="w-full py-3 bg-gradient-to-r from-purple-600 to-indigo-600 hover:from-purple-500 hover:to-indigo-500 text-white font-extrabold rounded-xl shadow-lg transition">
                            <i class="fa-solid fa-paper-plane mr-2"></i> ダイヤ・列車を登録・リアルタイム同期
                        </button>
                    </form>
                </div>
            </div>

            <!-- TAB 6: 号車別編成表 -->
            <div id="tab-formation" class="tab-content hidden h-full p-4 overflow-y-auto space-y-4">
                <div class="glass-panel p-4 rounded-2xl border border-purple-900/40 flex flex-wrap justify-between items-center gap-3">
                    <div>
                        <h2 class="text-lg font-bold text-white flex items-center gap-2">
                            <i class="fa-solid fa-cubes text-indigo-400"></i> 号車別編成構成図ビジュアル
                        </h2>
                        <p class="text-xs text-slate-400">クハ/モハ、パンタグラフ、車椅子・優先席位置の号車構成図</p>
                    </div>

                    <div class="flex items-center space-x-2">
                        <label class="text-xs text-purple-300 font-bold">編成選択:</label>
                        <select id="formation-select" onchange="renderCarFormations()" class="bg-slate-900 border border-purple-800 rounded-xl px-3 py-1.5 text-xs text-white font-bold">
                            <option value="S100-10">S100系 10両編成 (特急/4+6分割可)</option>
                            <option value="S1-10">S1系 10両編成 (主力通勤形)</option>
                            <option value="S3-8">S3系 8両編成 (4+4両併合)</option>
                            <option value="S900-4">S900系 4両編成 (事業用検測車)</option>
                        </select>
                    </div>
                </div>

                <div class="glass-panel p-4 rounded-2xl border border-purple-900/30 overflow-x-auto space-y-4">
                    <div id="formation-cars-flex" class="flex space-x-2 min-w-[750px] p-2">
                        <!-- JS Rendered Cars -->
                    </div>

                    <div class="grid grid-cols-2 sm:grid-cols-4 gap-2 text-[11px] text-slate-400 pt-3 border-t border-purple-900/30">
                        <div class="flex items-center gap-1.5"><i class="fa-solid fa-bolt text-amber-400"></i> パンタグラフ搭載車 (モハ)</div>
                        <div class="flex items-center gap-1.5"><i class="fa-solid fa-wheelchair text-cyan-400"></i> 車椅子・フリースペース対応</div>
                        <div class="flex items-center gap-1.5"><i class="fa-solid fa-[#ec4899] fa-wifi text-pink-400"></i> Wi-Fi / 車内コンセント完備</div>
                        <div class="flex items-center gap-1.5"><i class="fa-solid fa-crown text-amber-300"></i> 1号車 特別指定席（特急）</div>
                    </div>
                </div>
            </div>

            <!-- TAB 7: ダイヤ表 (スジ描画) -->
            <div id="tab-diagram" class="tab-content hidden h-full p-3 flex flex-col space-y-3">
                <div class="glass-panel p-3 rounded-2xl flex justify-between items-center shrink-0 border border-purple-900/40">
                    <div>
                        <h2 class="text-sm font-bold text-white flex items-center gap-2">
                            <i class="fa-solid fa-chart-line text-orange-400"></i> 全線列車ダイヤグラム（スジ可視化チャート）
                        </h2>
                        <p class="text-[11px] text-slate-400">縦軸：全60駅 / 横軸：時間軸 (05:00 ～ 24:00)</p>
                    </div>
                    <button onclick="renderDiagramCanvas()" class="px-3 py-1.5 bg-purple-900/50 hover:bg-purple-800 text-xs rounded-xl border border-purple-700/50 text-purple-200 font-bold">
                        <i class="fa-solid fa-arrows-rotate mr-1"></i> 再描写
                    </button>
                </div>

                <div class="flex-1 glass-panel rounded-2xl relative overflow-hidden border border-purple-900/30">
                    <canvas id="diagram-canvas" class="w-full h-full block bg-slate-950"></canvas>
                </div>
            </div>

            <!-- TAB 8: 同期＆システム設定 -->
            <div id="tab-settings" class="tab-content hidden h-full p-4 overflow-y-auto space-y-4">
                <div class="glass-panel p-6 rounded-2xl border border-purple-900/40 max-w-2xl mx-auto space-y-4">
                    <div class="flex items-center space-x-3 border-b border-purple-900/40 pb-3">
                        <i class="fa-solid fa-gear text-2xl text-slate-400"></i>
                        <div>
                            <h2 class="text-base font-bold text-white">システム・リアルタイム同期設定</h2>
                            <p class="text-xs text-slate-400">Firebase Realtime DatabaseおよびWeb Audioの設定</p>
                        </div>
                    </div>

                    <div class="space-y-4 text-xs">
                        <div class="bg-slate-900/80 p-4 rounded-xl border border-purple-900/30 space-y-3">
                            <div class="font-bold text-purple-300 text-sm">Firebase ルーム同期設定</div>
                            <p class="text-slate-400">同じ同期ルームIDを入力した端末同士（PC/スマホ）で列車・ダイヤデータがリアルタイム同期されます。</p>
                            <div class="flex gap-2">
                                <input type="text" id="setting-room-input" value="SHINMORI-ONLINE" class="flex-1 bg-slate-950 border border-purple-800 rounded-xl px-3 py-2 text-amber-300 font-mono-num font-bold">
                                <button onclick="updateSyncRoomKey()" class="px-4 py-2 bg-purple-600 hover:bg-purple-500 text-white font-bold rounded-xl shadow">
                                    ルーム変更
                                </button>
                            </div>
                        </div>

                        <div class="bg-slate-900/80 p-4 rounded-xl border border-purple-900/30 space-y-3">
                            <div class="font-bold text-purple-300 text-sm">完全自作 Web Audio マスター音量</div>
                            <div class="flex items-center space-x-4">
                                <input type="range" id="setting-audio-vol" min="0" max="1" step="0.05" value="0.5" oninput="updateAudioVolume(this.value)" class="flex-1 h-2 bg-slate-800 rounded appearance-none cursor-pointer accent-purple-500">
                                <span id="vol-display-label" class="font-mono-num font-bold text-cyan-300">50%</span>
                            </div>
                        </div>

                        <div class="bg-slate-900/80 p-4 rounded-xl border border-purple-900/30 space-y-2">
                            <div class="font-bold text-purple-300 text-sm">データバックアップ＆リセット</div>
                            <div class="flex gap-2">
                                <button onclick="exportDataJSON()" class="px-3 py-2 bg-slate-800 hover:bg-slate-700 text-purple-200 font-bold rounded-xl border border-purple-700/40">
                                    <i class="fa-solid fa-download mr-1"></i> JSON書き出し
                                </button>
                                <button onclick="resetDefaultData()" class="px-3 py-2 bg-red-900/40 hover:bg-red-800/60 text-red-200 font-bold rounded-xl border border-red-700/40">
                                    <i class="fa-solid fa-rotate-left mr-1"></i> 初期状態リセット
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

        </main>
    </div>

    <!-- Firebase SDK (v9 Moduled Loaded via ESM Script) -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getDatabase, ref, set, onValue } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

        // Firebase Config Setup (Falls back to local BroadcastChannel if offline or config un-provided)
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {
            apiKey: "demo-key",
            authDomain: "demo.firebaseapp.com",
            databaseURL: "https://demo-default-rtdb.firebaseio.com",
            projectId: "demo",
            storageBucket: "demo.appspot.com",
            messagingSenderId: "00000000000",
            appId: "1:00000000000:web:00000000000"
        };

        let fbApp = null;
        let fbDb = null;
        try {
            fbApp = initializeApp(firebaseConfig);
            fbDb = getDatabase(fbApp);
        } catch(e) {
            console.log("Firebase initialized in fallback mode.");
        }

        window.FB_SYNC = {
            db: fbDb,
            pushState: function(roomKey, data) {
                if (fbDb) {
                    try {
                        set(ref(fbDb, 'shinmori_rooms/' + roomKey), data);
                    } catch(e) {}
                }
            },
            subscribe: function(roomKey, callback) {
                if (fbDb) {
                    try {
                        onValue(ref(fbDb, 'shinmori_rooms/' + roomKey), (snapshot) => {
                            const val = snapshot.val();
                            if (val) callback(val);
                        });
                    } catch(e) {}
                }
            }
        };
    </script>

    <script>
        // 紫句守鉄道 マスターデータ構造
        const ShinmoriApp = {
            roomKey: "SHINMORI-ONLINE",
            audioEnabled: false,
            audioVolume: 0.5,
            
            // 全60駅の完全定義
            stations: [
                { id: "09", name: "水鳥湿原", line: "直通線" },
                { id: "08", name: "青蓮寺", line: "直通線" },
                { id: "07", name: "紫水", line: "直通線" },
                { id: "06", name: "瑠璃川", line: "直通線" },
                { id: "05", name: "翡翠野", line: "直通線" },
                { id: "04", name: "琥珀谷", line: "直通線" },
                { id: "03", name: "瑪瑙台", line: "直通線" },
                { id: "02", name: "天翔", line: "直通線" },
                { id: "1", name: "紫句守中央", line: "紫雲本線" },
                { id: "2", name: "霞詠ヶ丘", line: "紫雲本線" },
                { id: "3", name: "紫雲野", line: "紫雲本線" },
                { id: "4", name: "詩羽町", line: "紫雲本線" },
                { id: "5", name: "星句台", line: "紫雲本線" },
                { id: "6", name: "紫陽花前", line: "紫雲本線" },
                { id: "7", name: "句守書院", line: "紫雲本線" },
                { id: "8", name: "銀墨坂", line: "紫雲本線" },
                { id: "9", name: "紫峰高原", line: "紫雲本線" },
                { id: "10", name: "風詠の森", line: "紫雲本線" },
                { id: "11", name: "宵月町", line: "紫雲本線" },
                { id: "12", name: "句読通り", line: "紫雲本線" },
                { id: "13", name: "紫霞野", line: "紫雲本線" },
                { id: "14", name: "鏡句池", line: "紫雲本線" },
                { id: "15", name: "雨詠坂", line: "紫雲本線" },
                { id: "16", name: "紫都新町", line: "紫雲本線" },
                { id: "17", name: "句守空港", line: "紫雲本線" },
                { id: "18", name: "星詠港", line: "紫雲本線" },
                { id: "19", name: "紫光浜", line: "紫雲本線" },
                { id: "20", name: "潮句の杜", line: "紫雲本線" },
                { id: "21", name: "紫苑台", line: "紫雲本線" },
                { id: "22", name: "句守温泉", line: "紫雲本線" },
                { id: "23", name: "霧詠峠", line: "紫雲本線" },
                { id: "24", name: "鷲羽句守", line: "紫雲本線" },
                { id: "25", name: "紫野学園前", line: "紫雲本線" },
                { id: "26", name: "書詠通り", line: "紫雲本線" },
                { id: "27", name: "月句台", line: "紫雲本線" },
                { id: "28", name: "紫句守美術館", line: "紫雲本線" },
                { id: "29", name: "詩風町", line: "紫雲本線" },
                { id: "30", name: "紫句守展示場", line: "紫雲本線" },
                { id: "31", name: "星句高原入口", line: "星句高原線" },
                { id: "32", name: "星霧の丘", line: "星句高原線" },
                { id: "33", name: "天詠台", line: "星句高原線" },
                { id: "34", name: "星句牧場前", line: "星句高原線" },
                { id: "35", name: "星句温泉郷", line: "星句高原線" },
                { id: "36", name: "星句森林", line: "星句高原線" },
                { id: "37", name: "星句湖畔", line: "星句高原線" },
                { id: "38", name: "句守詩碑前", line: "星句高原線" },
                { id: "39", name: "星句高原村", line: "星句高原線" },
                { id: "40", name: "星句高原", line: "星句高原線" },
                { id: "41", name: "紫句守港", line: "句守支線" },
                { id: "42", name: "紫句守湾岸", line: "句守支線" },
                { id: "43", name: "句守湾岸", line: "句守支線" },
                { id: "44", name: "紫句守タワー", line: "句守支線" },
                { id: "45", name: "句守の杜", line: "句守支線" },
                { id: "46", name: "紫句守工業団地", line: "句守支線" },
                { id: "47", name: "紫句守工業", line: "句守支線" },
                { id: "48", name: "鉄輪詩町", line: "句守支線" },
                { id: "49", name: "紫句守市場", line: "句守支線" },
                { id: "50", name: "詩詠の里", line: "句守支線" },
                { id: "51", name: "紫句守農園", line: "句守支線" },
                { id: "52", name: "紫句守劇場前", line: "句守支線" },
                { id: "53", name: "句守未来都市", line: "句守支線" },
                { id: "54", name: "紫句守研究所", line: "句守支線" },
                { id: "55", name: "詩句の丘", line: "句守支線" },
                { id: "56", name: "紫句守展望台", line: "紫霞観光線" },
                { id: "57", name: "句守星見台", line: "紫霞観光線" },
                { id: "58", name: "紫句守森林公園", line: "紫霞観光線" },
                { id: "59", name: "紫句守詩碑前", line: "紫霞観光線" },
                { id: "60", name: "紫句守詩碑", line: "紫霞観光線" }
            ],

            // 7種別と停車駅パターン規定
            classes: {
                "特急": {
                    badge: "badge-tokkyu",
                    cars: [10, 6, 4], // 4+6対応
                    patterns: [
                        ["1","3","13","16","17","23","28","30"],
                        ["1","3","13","16","17","23","28","30","35","40"],
                        ["1","3","13","43","44","50","52","55"],
                        ["1","3","13","16","17","23","28","56","60"]
                    ]
                },
                "通勤急行": {
                    badge: "badge-tsukin-kyuko",
                    cars: [10], // 4+6対応
                    patterns: [
                        ["1","3","6","13","16","17","20","24","28","30"],
                        ["1","3","6","13","43","48","50","52","55"],
                        ["1","3","6","13","16","17","20","24","28","56","60"]
                    ]
                },
                "急行": {
                    badge: "badge-kyuko",
                    cars: [10, 8], // 4+4, 4+6
                    patterns: [
                        ["09","06","04","02","1","3","6","9","13","16","17","20","23","28","30"],
                        ["09","06","04","02","1","2","3","6","9","13","43","44","48","50","52","55"],
                        ["09","06","04","02","1","3","6","9","13","16","17","20","23","28","56","60"],
                        ["1","3","6","9","13","16","17","20","23","28","30"],
                        ["1","3","6","9","13","43","44","48","50","52","55"],
                        ["1","3","6","9","13","16","17","20","23","28","56","60"]
                    ]
                },
                "通勤快速": {
                    badge: "badge-tsukin-kaisoku",
                    cars: [10, 8], // 4+4, 4+6
                    patterns: [
                        ["1","3","5","7","9","13","16","17","20","23","24","28","30"],
                        ["1","3","5","7","9","13","42","44","45","46","48","50","52","53","55"],
                        ["1","3","5","7","9","13","16","17","20","23","24","28","56","57","58","60"]
                    ]
                },
                "快速": {
                    badge: "badge-kaisoku",
                    cars: [10, 8, 6], // 4+4, 4+6
                    patterns: [
                        ["1","3","5","6","7","9","13","16","17","20","23","24","28","30"],
                        ["1","3","5","6","7","9","13","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55"],
                        ["1","3","5","6","7","9","13","16","17","20","23","24","28","56","57","58","60"]
                    ]
                },
                "準急": {
                    badge: "badge-junkyu",
                    cars: [10, 8, 6], // 4+4, 4+6
                    patterns: [
                        ["1","3","5","6","7","9","11","13","16","17","19","20","23","24","25","26","28","29","30"],
                        ["1","3","5","6","7","9","11","13","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55"],
                        ["1","3","5","6","7","9","11","13","16","17","19","20","23","24","25","26","28","56","57","58","59","60"]
                    ]
                },
                "普通": {
                    badge: "badge-futsu",
                    cars: [10, 8, 6, 4], // 4+4, 4+6
                    patterns: [
                        ["09","08","07","06","05","04","03","02","1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30"],
                        ["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30"],
                        ["31","32","33","34","35","36","37","38","39","40"],
                        ["41","42","43","44","45","46","47","48","49","50","51","52","53","54","55"],
                        ["56","57","58","59","60"]
                    ]
                }
            },

            // 運用中列車リスト (リアルタイム同期対象)
            trains: [
                { id: "101M", class: "特急", series: "S100系", cars: 10, startSt: "1", endSt: "30", depTime: "08:00" },
                { id: "205M", class: "通勤急行", series: "S1系", cars: 10, startSt: "1", endSt: "55", depTime: "08:15" },
                { id: "309M", class: "急行", series: "S2系", cars: 10, startSt: "09", endSt: "30", depTime: "08:30" },
                { id: "401M", class: "快速", series: "S3系", cars: 8, startSt: "1", endSt: "40", depTime: "08:45" },
                { id: "503M", class: "普通", series: "S4系", cars: 6, startSt: "1", endSt: "30", depTime: "09:00" }
            ],

            // 車両形式一覧定義
            fleets: [
                { series: "S1系", color: "#a855f7", formations: "10両 (01-05), 8両 (06-10)", desc: "本線の混雑緩和を目的とした紫句守鉄道の主力標準型通勤電車。" },
                { series: "S2系", color: "#6366f1", formations: "10両 (01-15), 8両 (16-25)", desc: "高効率VVVFインバータ制御を搭載した高加減速仕様の都市型電車。" },
                { series: "S3系", color: "#10b981", formations: "10両/8両/6両/4両", desc: "4+4両、4+6両などの柔軟な分割併合に対応する多目的汎用車。" },
                { series: "S4系", color: "#06b6d4", formations: "10両/8両/6両", desc: "勾配区間の多い星句高原線・紫霞観光線にも対応する軽量アルミ車体。" },
                { series: "S5系", color: "#f59e0b", formations: "10両 (01-04)", desc: "ワイドドアと座席定員を最適化した混雑対策最新型車両。" },
                { series: "S100系", color: "#ec4899", formations: "10両(4+6)/6両/4両", desc: "フラッグシップ特急「シノモリライナー」用ハイグレード特急車。" },
                { series: "S900系", color: "#eab308", formations: "4両編成 (事業用)", desc: "全線の軌道・架線状態をミリ単位で総合検測する特別事業用車両。" }
            ]
        };

        // タブ切り替え制御
        document.querySelectorAll('.nav-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                const target = btn.getAttribute('data-tab');
                document.querySelectorAll('.nav-btn').forEach(b => {
                    b.classList.remove('active', 'bg-purple-600/30', 'text-purple-300', 'border', 'border-purple-500/40');
                    b.classList.add('text-slate-400');
                });
                btn.classList.add('active', 'bg-purple-600/30', 'text-purple-300', 'border', 'border-purple-500/40');
                btn.classList.remove('text-slate-400');

                document.querySelectorAll('.tab-content').forEach(tc => tc.classList.add('hidden'));
                document.getElementById(target).classList.remove('hidden');

                if (target === 'tab-live-sim') {
                    renderNetworkCanvas();
                    renderCabCanvas();
                } else if (target === 'tab-diagram') {
                    renderDiagramCanvas();
                }
            });
        });

        // 時刻表初期化 & 描画
        function initTimetableStations() {
            const select = document.getElementById('tt-station-select');
            select.innerHTML = '';
            ShinmoriApp.stations.forEach(st => {
                const opt = document.createElement('option');
                opt.value = st.id;
                opt.innerText = `${st.id} ${st.name} (${st.line})`;
                select.appendChild(opt);
            });
            renderTimetable();
        }

        function renderTimetable() {
            const stId = document.getElementById('tt-station-select').value || "1";
            const stationObj = ShinmoriApp.stations.find(s => s.id === stId);
            document.getElementById('tt-station-title').innerText = `${stationObj.id} ${stationObj.name} 発車時刻表`;

            const tbody = document.getElementById('timetable-rows');
            tbody.innerHTML = '';

            // 時刻表生成（5時～23時）
            for (let hour = 5; hour <= 23; hour++) {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-purple-900/20 border-b border-purple-900/20";

                const hourTd = `<td class="p-2 text-center font-bold text-amber-400 bg-slate-900/40">${hour.toString().padStart(2, '0')}</td>`;
                
                // 発車分のダミー・実データ算出
                let departuresHtml = `<div class="flex flex-wrap gap-2 items-center p-1">`;
                
                // 該当駅に停止する種別・スジから生成
                const activeTrains = ShinmoriApp.trains.filter(t => {
                    const clsInfo = ShinmoriApp.classes[t.class];
                    if (!clsInfo) return false;
                    return clsInfo.patterns.some(p => p.includes(stId));
                });

                if (activeTrains.length === 0) {
                    // 基本パターンフォールバック
                    const mins = [(hour * 7) % 60, (hour * 19 + 12) % 60, (hour * 31 + 25) % 60, (hour * 43 + 42) % 60].sort((a,b)=>a-b);
                    mins.forEach((m, idx) => {
                        const classKeys = Object.keys(ShinmoriApp.classes);
                        const cls = classKeys[(hour + idx) % classKeys.length];
                        const badge = ShinmoriApp.classes[cls].badge;
                        departuresHtml += `
                            <span class="inline-flex items-center space-x-1 px-2 py-1 rounded bg-slate-900 border border-purple-900/40">
                                <span class="font-extrabold text-white">${m.toString().padStart(2, '0')}</span>
                                <span class="${badge} text-[9px] px-1 rounded font-bold">${cls}</span>
                                <span class="text-slate-400 text-[10px]">紫句守行</span>
                            </span>
                        `;
                    });
                } else {
                    activeTrains.forEach((t, i) => {
                        const m = (parseInt(t.depTime.split(':')[1] || 0) + i * 15 + hour * 3) % 60;
                        const badge = ShinmoriApp.classes[t.class]?.badge || "badge-futsu";
                        const endStObj = ShinmoriApp.stations.find(s=>s.id === t.endSt);
                        departuresHtml += `
                            <span class="inline-flex items-center space-x-1.5 px-2.5 py-1 rounded bg-slate-900 border border-purple-800/60 shadow">
                                <span class="font-extrabold text-amber-300 text-xs">${m.toString().padStart(2, '0')}</span>
                                <span class="${badge} text-[9px] px-1.5 py-0.5 rounded font-bold">${t.class}</span>
                                <span class="text-white font-bold text-[11px]">${endStObj ? endStObj.name : "中央"}行</span>
                                <span class="text-slate-400 text-[9px]">(${t.cars}両/${t.id})</span>
                            </span>
                        `;
                    });
                }

                departuresHtml += `</div>`;
                tr.innerHTML = hourTd + `<td class="p-1">${departuresHtml}</td>`;
                tbody.appendChild(tr);
            }
        }

        let driverSpeed = 0;
        let driverNotch = 0;
        let driverDist = 850;

        function initDriverTrainSelect() {
            const select = document.getElementById('driver-train-select');
            select.innerHTML = '';
            ShinmoriApp.trains.forEach(t => {
                const opt = document.createElement('option');
                opt.value = t.id;
                const endSt = ShinmoriApp.stations.find(s => s.id === t.endSt)?.name || "";
                opt.innerText = `${t.id} ${t.class} (${t.series}/${t.cars}両) -> ${endSt}行`;
                select.appendChild(opt);
            });
            onDriverTrainChange();
        }

        function onDriverTrainChange() {
            const trainId = document.getElementById('driver-train-select').value;
            const train = ShinmoriApp.trains.find(t => t.id === trainId);
            if (train) {
                const badgeEl = document.getElementById('sim-train-badge');
                badgeEl.className = `${ShinmoriApp.classes[train.class]?.badge || 'badge-futsu'} px-2 py-0.5 rounded text-xs font-bold`;
                badgeEl.innerText = train.class;
                const endSt = ShinmoriApp.stations.find(s => s.id === train.endSt)?.name || "";
                document.getElementById('sim-route-desc').innerText = `${endSt} 行 (${train.cars}両編成)`;

                document.getElementById('sim-spec-info').innerHTML = `
                    <div>形式: ${train.series} (${train.cars}両編成)</div>
                    <div>最高営業速度: 120 km/h</div>
                    <div>制御方式: 高効率VVVFインバータ制御</div>
                `;
            }
        }

        function onNotchChange(val) {
            driverNotch = parseInt(val);
            updateNotchDisplay();
            broadcastSyncState('NOTCH', { notch: driverNotch });
        }

        function stepNotch(delta) {
            const slider = document.getElementById('driver-notch-slider');
            let nextVal = parseInt(slider.value) + delta;
            if (nextVal >= -6 && nextVal <= 5) {
                slider.value = nextVal;
                onNotchChange(nextVal);
            }
        }

        function setNotch(val) {
            const slider = document.getElementById('driver-notch-slider');
            slider.value = val;
            onNotchChange(val);
        }

        function updateNotchDisplay() {
            const label = document.getElementById('driver-notch-label');
            if (driverNotch > 0) {
                label.innerText = `P${driverNotch} (力行加速)`;
                label.className = "font-bold font-mono-num text-emerald-400";
            } else if (driverNotch === 0) {
                label.innerText = "N (切/惰行)";
                label.className = "font-bold font-mono-num text-amber-400";
            } else if (driverNotch === -6) {
                label.innerText = "EB (非常ブレーキ)";
                label.className = "font-bold font-mono-num text-red-500 animate-pulse";
            } else {
                label.innerText = `B${Math.abs(driverNotch)} (常用制動)`;
                label.className = "font-bold font-mono-num text-red-400";
            }
        }

        function advanceNextStation() {
            driverDist = 850;
            document.getElementById('driver-arrival-msg').classList.add('hidden');
            playAudioDoor();
        }

        // Canvas 描画エンジン
        const networkCanvas = document.getElementById('network-canvas');
        const networkCtx = networkCanvas.getContext('2d');
        const cabCanvas = document.getElementById('cab-canvas');
        const cabCtx = cabCanvas.getContext('2d');

        function renderNetworkCanvas() {
            if (!networkCanvas.parentElement) return;
            networkCanvas.width = networkCanvas.parentElement.clientWidth;
            networkCanvas.height = networkCanvas.parentElement.clientHeight;
            const w = networkCanvas.width;
            const h = networkCanvas.height;

            networkCtx.clearRect(0, 0, w, h);

            // 路線描画 (紫雲本線)
            networkCtx.strokeStyle = "#8b5cf6";
            networkCtx.lineWidth = 4;
            networkCtx.beginPath();
            networkCtx.moveTo(30, h * 0.5);
            networkCtx.lineTo(w - 30, h * 0.5);
            networkCtx.stroke();

            // 駅の点プロット
            const total = ShinmoriApp.stations.length;
            const step = (w - 60) / 30; // 本線30駅表示
            for (let i = 0; i < 30; i++) {
                const x = 30 + i * step;
                networkCtx.fillStyle = "#ffffff";
                networkCtx.beginPath();
                networkCtx.arc(x, h * 0.5, 3, 0, Math.PI * 2);
                networkCtx.fill();
            }

            // 動的列車位置プロット
            ShinmoriApp.trains.forEach((t, idx) => {
                const tx = 30 + ((Date.now() / 200 + idx * 80) % (w - 60));
                networkCtx.fillStyle = "#f59e0b";
                networkCtx.beginPath();
                networkCtx.arc(tx, h * 0.5, 6, 0, Math.PI * 2);
                networkCtx.fill();
                networkCtx.fillStyle = "#ffffff";
                networkCtx.font = "10px monospace";
                networkCtx.fillText(t.id, tx - 10, h * 0.5 - 10);
            });
        }

        function renderCabCanvas() {
            if (!cabCanvas.parentElement) return;
            cabCanvas.width = cabCanvas.parentElement.clientWidth;
            cabCanvas.height = cabCanvas.parentElement.clientHeight;
            const w = cabCanvas.width;
            const h = cabCanvas.height;

            cabCtx.clearRect(0, 0, w, h);

            // 空＆線路背景
            cabCtx.fillStyle = "#090d1f";
            cabCtx.fillRect(0, 0, w, h * 0.55);
            cabCtx.fillStyle = "#05030a";
            cabCtx.fillRect(0, h * 0.55, w, h * 0.45);

            // レールパースライン
            cabCtx.strokeStyle = "#8b5cf6";
            cabCtx.lineWidth = 3;
            cabCtx.beginPath();
            cabCtx.moveTo(w / 2 - 5, h * 0.55);
            cabCtx.lineTo(w / 2 - 140, h);
            cabCtx.stroke();

            cabCtx.beginPath();
            cabCtx.moveTo(w / 2 + 5, h * 0.55);
            cabCtx.lineTo(w / 2 + 140, h);
            cabCtx.stroke();
        }

        // ダイヤ表 (スジ描画) Canvas
        function renderDiagramCanvas() {
            const canvas = document.getElementById('diagram-canvas');
            if (!canvas || !canvas.parentElement) return;
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight;

            const ctx = canvas.getContext('2d');
            const w = canvas.width;
            const h = canvas.height;

            ctx.clearRect(0, 0, w, h);

            // グリッド描画 (時間軸: 横, 駅: 縦)
            ctx.strokeStyle = "rgba(139, 92, 246, 0.15)";
            ctx.lineWidth = 1;

            // 縦軸 (時間: 5時〜24時)
            const hourStep = (w - 80) / 19;
            for (let i = 0; i <= 19; i++) {
                const x = 60 + i * hourStep;
                ctx.beginPath();
                ctx.moveTo(x, 20);
                ctx.lineTo(x, h - 30);
                ctx.stroke();

                ctx.fillStyle = "#94a3b8";
                ctx.font = "10px Share Tech Mono";
                ctx.fillText(`${(i + 5).toString().padStart(2, '0')}:00`, x - 12, h - 10);
            }

            // 横軸 (主要駅)
            const stStep = (h - 50) / 10;
            for (let j = 0; j <= 10; j++) {
                const y = 20 + j * stStep;
                ctx.beginPath();
                ctx.moveTo(60, y);
                ctx.lineTo(w - 20, y);
                ctx.stroke();

                const st = ShinmoriApp.stations[j * 3] || ShinmoriApp.stations[0];
                ctx.fillStyle = "#cbd5e1";
                ctx.font = "10px sans-serif";
                ctx.fillText(st.name, 5, y + 3);
            }

            // 列車スジ描画
            ShinmoriApp.trains.forEach((t, idx) => {
                ctx.strokeStyle = idx % 2 === 0 ? "#a855f7" : "#06b6d4";
                ctx.lineWidth = 2;
                ctx.beginPath();

                const startHour = parseInt(t.depTime.split(':')[0]) || 8;
                const startX = 60 + (startHour - 5) * hourStep;
                const endX = startX + hourStep * 1.5;

                ctx.moveTo(startX, 20);
                ctx.lineTo(endX, h - 30);
                ctx.stroke();

                ctx.fillStyle = "#f59e0b";
                ctx.font = "10px Share Tech Mono";
                ctx.fillText(t.id, startX + 5, 35 + idx * 15);
            });
        }

        // 車両形式カード描画
        function renderFleetCatalog() {
            const container = document.getElementById('fleet-cards-container');
            container.innerHTML = '';

            ShinmoriApp.fleets.forEach(f => {
                const card = document.createElement('div');
                card.className = "glass-panel p-4 rounded-2xl border border-purple-900/40 space-y-3";
                card.innerHTML = `
                    <div class="flex justify-between items-center border-b border-purple-900/30 pb-2">
                        <h3 class="font-extrabold text-lg text-amber-300">${f.series}</h3>
                        <span class="text-xs px-2 py-0.5 rounded bg-purple-950 text-purple-300 border border-purple-800 font-bold">主力在籍車</span>
                    </div>
                    <div class="w-full h-24 bg-slate-950 rounded-xl flex items-center justify-center p-2 border border-purple-900/30">
                        <svg class="w-full h-20" viewBox="0 0 300 60">
                            <rect x="10" y="15" width="280" height="30" rx="4" fill="#130e26" stroke="${f.color}" stroke-width="2.5"/>
                            <rect x="10" y="32" width="280" height="5" fill="${f.color}"/>
                            <circle cx="50" cy="48" r="4" fill="#64748b"/>
                            <circle cx="70" cy="48" r="4" fill="#64748b"/>
                            <circle cx="230" cy="48" r="4" fill="#64748b"/>
                            <circle cx="250" cy="48" r="4" fill="#64748b"/>
                        </svg>
                    </div>
                    <div class="text-xs space-y-1">
                        <div class="text-slate-300 font-bold">編成仕様: ${f.formations}</div>
                        <p class="text-slate-400 leading-relaxed text-[11px]">${f.desc}</p>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        // 列車追加フォーム初期化
        function initAddTrainForm() {
            const startSel = document.getElementById('input-start-st');
            const endSel = document.getElementById('input-end-st');
            startSel.innerHTML = '';
            endSel.innerHTML = '';

            ShinmoriApp.stations.forEach(st => {
                startSel.innerHTML += `<option value="${st.id}">${st.id} ${st.name}</option>`;
                endSel.innerHTML += `<option value="${st.id}">${st.id} ${st.name}</option>`;
            });
            endSel.value = "30";

            updateFormCarOptions();
        }

        function updateFormCarOptions() {
            const cls = document.getElementById('input-train-class').value;
            const carSel = document.getElementById('input-train-cars');
            carSel.innerHTML = '';

            const allowedCars = ShinmoriApp.classes[cls]?.cars || [10, 8, 6, 4];
            allowedCars.forEach(c => {
                carSel.innerHTML += `<option value="${c}">${c}両編成</option>`;
            });
        }

        function handleTrainSubmit(e) {
            e.preventDefault();
            const newTrain = {
                id: document.getElementById('input-train-id').value.trim(),
                class: document.getElementById('input-train-class').value,
                series: document.getElementById('input-train-series').value,
                cars: parseInt(document.getElementById('input-train-cars').value),
                startSt: document.getElementById('input-start-st').value,
                endSt: document.getElementById('input-end-st').value,
                depTime: document.getElementById('input-dep-time').value
            };

            ShinmoriApp.trains.push(newTrain);
            renderTimetable();
            initDriverTrainSelect();
            renderDiagramCanvas();
            broadcastSyncState('ADD_TRAIN', newTrain);

            alert(`列車 [${newTrain.id} ${newTrain.class}] を正常にダイヤ登録しました。`);
            document.getElementById('add-train-form').reset();
            updateFormCarOptions();
        }

        // 号車別編成表 描画
        function renderCarFormations() {
            const val = document.getElementById('formation-select').value;
            const container = document.getElementById('formation-cars-flex');
            container.innerHTML = '';

            const totalCars = parseInt(val.split('-')[1]) || 10;
            const series = val.split('-')[0];

            for (let i = 1; i <= totalCars; i++) {
                const isKuha = (i === 1 || i === totalCars);
                const hasPanta = (!isKuha && i % 2 === 0);
                const isWheelchair = (i === 1 || i === totalCars || i === 4);

                const carBox = document.createElement('div');
                carBox.className = "flex-1 bg-slate-900 border border-purple-800/60 rounded-xl p-3 flex flex-col justify-between items-center text-center relative shadow-lg";
                carBox.innerHTML = `
                    <div class="text-[10px] text-purple-300 font-bold">${i}号車</div>
                    <div class="my-2">
                        <i class="fa-solid fa-train text-2xl ${isKuha ? 'text-amber-400' : 'text-purple-400'}"></i>
                    </div>
                    <div class="text-[9px] font-mono-num font-bold text-slate-300">
                        ${isKuha ? 'クハ' : 'モハ'}${series.replace('系','')}-${100 + i}
                    </div>
                    <div class="flex gap-1 mt-1 text-[10px]">
                        ${hasPanta ? '<i class="fa-solid fa-bolt text-amber-400" title="パンタグラフ"></i>' : ''}
                        ${isWheelchair ? '<i class="fa-solid fa-wheelchair text-cyan-400" title="車椅子スペース"></i>' : ''}
                    </div>
                `;
                container.appendChild(carBox);
            }
        }

        // 完全自作 Web Audio API 音響合成
        let audioCtx = null;

        function initAudio() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
        }

        function toggleAudioMaster() {
            initAudio();
            ShinmoriApp.audioEnabled = !ShinmoriApp.audioEnabled;
            const icon = document.getElementById('icon-sound');
            if (ShinmoriApp.audioEnabled) {
                icon.className = "fa-solid fa-volume-high text-emerald-400";
            } else {
                icon.className = "fa-solid fa-volume-xmark text-red-400";
            }
        }

        function updateAudioVolume(val) {
            ShinmoriApp.audioVolume = parseFloat(val);
            document.getElementById('vol-display-label').innerText = `${Math.round(val * 100)}%`;
        }

        function playAudioHorn() {
            if (!ShinmoriApp.audioEnabled || !audioCtx) return;
            const osc1 = audioCtx.createOscillator();
            const osc2 = audioCtx.createOscillator();
            const gain = audioCtx.createGain();

            osc1.type = 'triangle';
            osc2.type = 'triangle';
            osc1.frequency.setValueAtTime(320, audioCtx.currentTime);
            osc2.frequency.setValueAtTime(480, audioCtx.currentTime);

            gain.gain.setValueAtTime(ShinmoriApp.audioVolume * 0.4, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 1.2);

            osc1.connect(gain);
            osc2.connect(gain);
            gain.connect(audioCtx.destination);

            osc1.start();
            osc2.start();
            osc1.stop(audioCtx.currentTime + 1.2);
            osc2.stop(audioCtx.currentTime + 1.2);
        }

        function playAudioChime() {
            if (!ShinmoriApp.audioEnabled || !audioCtx) return;
            const notes = [523.25, 659.25, 783.99, 1046.50];
            notes.forEach((freq, idx) => {
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(freq, audioCtx.currentTime + idx * 0.22);
                gain.gain.setValueAtTime(ShinmoriApp.audioVolume * 0.3, audioCtx.currentTime + idx * 0.22);
                gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + idx * 0.22 + 0.5);
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                osc.start(audioCtx.currentTime + idx * 0.22);
                osc.stop(audioCtx.currentTime + idx * 0.22 + 0.5);
            });
        }

        function playAudioDoor() {
            if (!ShinmoriApp.audioEnabled || !audioCtx) return;
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.type = 'square';
            osc.frequency.setValueAtTime(880, audioCtx.currentTime);
            gain.gain.setValueAtTime(ShinmoriApp.audioVolume * 0.2, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 0.3);
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            osc.start();
            osc.stop(audioCtx.currentTime + 0.3);
        }

        function broadcastSyncState(action, payload) {
            const data = {
                action: action,
                payload: payload,
                trains: ShinmoriApp.trains,
                timestamp: Date.now()
            };

            // Firebase Room Sync
            if (window.FB_SYNC) {
                window.FB_SYNC.pushState(ShinmoriApp.roomKey, data);
            }

            // LocalStorage Auto Backup
            localStorage.setItem('shinmori_system_state', JSON.stringify({
                roomKey: ShinmoriApp.roomKey,
                trains: ShinmoriApp.trains
            }));
        }

        function initSyncRoom() {
            if (window.FB_SYNC) {
                window.FB_SYNC.subscribe(ShinmoriApp.roomKey, (data) => {
                    if (data && data.trains) {
                        ShinmoriApp.trains = data.trains;
                        renderTimetable();
                        initDriverTrainSelect();
                        renderDiagramCanvas();
                        document.getElementById('sync-status-text').innerText = "クラウド同期アクティブ";
                    }
                });
            }
        }

        function updateSyncRoomKey() {
            const newKey = document.getElementById('setting-room-input').value.trim();
            if (newKey) {
                ShinmoriApp.roomKey = newKey;
                document.getElementById('sync-room-display').innerText = newKey;
                initSyncRoom();
                alert(`同期ルームを [${newKey}] に更新しました。`);
            }
        }

        function exportDataJSON() {
            const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(ShinmoriApp, null, 2));
            const anchor = document.createElement('a');
            anchor.setAttribute("href", dataStr);
            anchor.setAttribute("download", `shinmori_railway_${ShinmoriApp.roomKey}.json`);
            document.body.appendChild(anchor);
            anchor.click();
            anchor.remove();
        }

        function triggerImportJSON() {
            document.getElementById('json-file-input').click();
        }

        function importDataJSON(e) {
            const reader = new FileReader();
            reader.onload = function(evt) {
                try {
                    const imported = JSON.parse(evt.target.result);
                    if (imported.trains) ShinmoriApp.trains = imported.trains;
                    renderTimetable();
                    initDriverTrainSelect();
                    renderDiagramCanvas();
                    alert("JSONデータを正常に復元・インポートしました。");
                } catch(err) {
                    alert("無効なJSONフォーマットです。");
                }
            };
            reader.readAsText(e.target.files[0]);
        }

        function resetDefaultData() {
            if (confirm("全データと追加されたダイヤを初期状態へリセットしますか？")) {
                localStorage.removeItem('shinmori_system_state');
                location.reload();
            }
        }

        // 定期物理・タイマー更新ループ
        setInterval(() => {
            if (driverNotch > 0) {
                driverSpeed = Math.min(120, driverSpeed + driverNotch * 0.2);
            } else if (driverNotch < 0) {
                driverSpeed = Math.max(0, driverSpeed - Math.abs(driverNotch) * 0.45);
            }

            if (driverSpeed > 0) {
                driverDist = Math.max(0, Math.round(driverDist - (driverSpeed * 0.035)));
            }

            if (driverDist === 0 && driverSpeed === 0) {
                document.getElementById('driver-arrival-msg').classList.remove('hidden');
            }

            document.getElementById('driver-speed').innerText = Math.round(driverSpeed);
            document.getElementById('driver-dist').innerText = driverDist;

            const now = new Date();
            document.getElementById('system-clock').innerText = now.toTimeString().split(' ')[0];

            renderCabCanvas();
        }, 100);

        // アプリ起動時初期化
        window.onload = function() {
            initTimetableStations();
            renderFleetCatalog();
            initAddTrainForm();
            initDriverTrainSelect();
            renderCarFormations();
            initSyncRoom();

            window.addEventListener('resize', () => {
                renderNetworkCanvas();
                renderCabCanvas();
                renderDiagramCanvas();
            });
        };
    </script>
</body>
</html>
