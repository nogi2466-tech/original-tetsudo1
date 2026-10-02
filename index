<!DOCTYPE html>
<html lang="ja" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>紫句守鉄道（しのもり鉄道） 統合シミュレーター＆ダイヤ案内</title>
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
            background-color: #090613;
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
            border: 1px solid rgba(139, 92, 246, 0.2);
        }
        .glass-card {
            background: rgba(30, 21, 54, 0.6);
            border: 1px solid rgba(139, 92, 246, 0.15);
        }
        .purple-glow {
            box-shadow: 0 0 20px rgba(168, 85, 247, 0.35);
        }
        .led-display {
            background-color: #05030a;
            border: 1px solid #3b0764;
            box-shadow: inset 0 0 10px rgba(0,0,0,0.9);
        }
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

    <header class="glass-panel border-b border-purple-900/40 px-4 py-2.5 flex justify-between items-center z-30 shrink-0">
        <div class="flex items-center space-x-3">
            <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-purple-600 via-indigo-600 to-violet-800 flex items-center justify-center shadow-lg purple-glow">
                <i class="fa-solid fa-train-subway text-xl text-amber-300"></i>
            </div>
            <div>
                <div class="flex items-center space-x-2">
                    <h1 class="text-lg font-extrabold tracking-wide bg-gradient-to-r from-purple-300 via-violet-200 to-amber-300 bg-clip-text text-transparent">
                        紫句守鉄道
                    </h1>
                    <span class="text-xs text-purple-300 font-medium px-2 py-0.5 rounded-full bg-purple-950/80 border border-purple-700">しのもり鉄道</span>
                </div>
                <p class="text-[11px] text-slate-400">総合ダイヤ案内＆リアルタイム自動同期シミュレーター</p>
            </div>
        </div>

        <div class="hidden md:flex items-center space-x-4 text-xs">
            <div id="sync-status-badge" class="flex items-center space-x-2 bg-emerald-950/80 px-3 py-1.5 rounded-xl border border-emerald-700/50 text-emerald-300">
                <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
                <span class="font-bold">全端末自動同期中</span>
            </div>
            <div class="flex items-center space-x-2 bg-slate-900/80 px-3 py-1.5 rounded-xl border border-purple-900/40">
                <i class="fa-solid fa-key text-amber-400"></i>
                <span class="text-slate-400">同期キー:</span>
                <span id="sync-key-display" class="font-mono-num font-bold text-amber-300">SHINMORI-88</span>
                <button onclick="changeSyncKey()" class="ml-1 text-purple-400 hover:text-purple-200" title="同期キーを変更"><i class="fa-solid fa-pen-to-square"></i></button>
            </div>
            <div class="flex items-center space-x-2 bg-slate-900/80 px-3 py-1.5 rounded-xl border border-purple-900/40">
                <i class="fa-solid fa-clock text-cyan-400"></i>
                <span id="system-clock" class="font-bold font-mono-num text-cyan-300 text-sm">12:00:00</span>
            </div>
        </div>

        <div class="flex items-center space-x-2">
            <button id="btn-export-data" onclick="exportDataJSON()" class="px-3 py-1.5 rounded-xl bg-purple-950/80 hover:bg-purple-900 text-purple-300 border border-purple-700/50 text-xs font-bold transition flex items-center gap-1.5">
                <i class="fa-solid fa-download"></i> <span class="hidden sm:inline">保存</span>
            </button>
            <button id="btn-import-data" onclick="triggerImportJSON()" class="px-3 py-1.5 rounded-xl bg-purple-950/80 hover:bg-purple-900 text-purple-300 border border-purple-700/50 text-xs font-bold transition flex items-center gap-1.5">
                <i class="fa-solid fa-upload"></i> <span class="hidden sm:inline">復元</span>
            </button>
            <input type="file" id="json-file-input" class="hidden" accept=".json" onchange="importDataJSON(event)">
            
            <button id="btn-sound-toggle" onclick="toggleSound()" class="p-2.5 rounded-xl bg-slate-900 hover:bg-slate-800 border border-purple-800/50 text-slate-300 hover:text-white transition">
                <i id="icon-sound" class="fa-solid fa-volume-xmark text-red-400"></i>
            </button>
        </div>
    </header>

    <div class="flex-1 flex overflow-hidden">
        <!-- Sidebar Navigation -->
        <nav class="w-16 md:w-56 glass-panel border-r border-purple-900/40 flex flex-col justify-between shrink-0 z-20">
            <div class="p-2 space-y-1.5">
                <button data-tab="tab-stop-matrix" class="nav-btn active w-full flex items-center space-x-3 px-3 py-3 rounded-xl text-left transition bg-purple-600/30 text-purple-300 border border-purple-500/40">
                    <i class="fa-solid fa-table-cells text-lg w-6 text-center"></i>
                    <span class="hidden md:inline font-bold text-xs">停車駅・ダイヤ案内</span>
                </button>
                <button data-tab="tab-live-map" class="nav-btn w-full flex items-center space-x-3 px-3 py-3 rounded-xl text-left transition text-slate-400 hover:bg-purple-900/30 hover:text-slate-200">
                    <i class="fa-solid fa-satellite text-lg w-6 text-center"></i>
                    <span class="hidden md:inline font-bold text-xs">全線リアルタイム運行</span>
                </button>
                <button data-tab="tab-driver" class="nav-btn w-full flex items-center space-x-3 px-3 py-3 rounded-xl text-left transition text-slate-400 hover:bg-purple-900/30 hover:text-slate-200">
                    <i class="fa-solid fa-gauge-high text-lg w-6 text-center"></i>
                    <span class="hidden md:inline font-bold text-xs">運転士シミュレーター</span>
                </button>
                <button data-tab="tab-fleet" class="nav-btn w-full flex items-center space-x-3 px-3 py-3 rounded-xl text-left transition text-slate-400 hover:bg-purple-900/30 hover:text-slate-200">
                    <i class="fa-solid fa-train-subway text-lg w-6 text-center"></i>
                    <span class="hidden md:inline font-bold text-xs">車両基地・編成カスタマイズ</span>
                </button>
                <button data-tab="tab-dash" class="nav-btn w-full flex items-center space-x-3 px-3 py-3 rounded-xl text-left transition text-slate-400 hover:bg-purple-900/30 hover:text-slate-200">
                    <i class="fa-solid fa-chart-line text-lg w-6 text-center"></i>
                    <span class="hidden md:inline font-bold text-xs">経営＆アナリティクス</span>
                </button>
            </div>

            <!-- Route Quick Legend -->
            <div class="p-3 border-t border-purple-900/40 hidden md:block text-[11px] text-slate-400 space-y-1">
                <div class="font-bold text-purple-300 mb-1">管轄 5 路線一覧</div>
                <div class="flex justify-between items-center"><span class="flex items-center gap-1.5"><span class="w-2 h-2 rounded-full bg-slate-400"></span>直通線</span><span class="text-slate-300 font-mono-num">09〜01</span></div>
                <div class="flex justify-between items-center"><span class="flex items-center gap-1.5"><span class="w-2 h-2 rounded-full bg-purple-500"></span>紫雲本線</span><span class="text-purple-300 font-mono-num">1〜30</span></div>
                <div class="flex justify-between items-center"><span class="flex items-center gap-1.5"><span class="w-2 h-2 rounded-full bg-amber-500"></span>星句高原線</span><span class="text-amber-300 font-mono-num">31〜40</span></div>
                <div class="flex justify-between items-center"><span class="flex items-center gap-1.5"><span class="w-2 h-2 rounded-full bg-emerald-500"></span>句守支線</span><span class="text-emerald-300 font-mono-num">41〜55</span></div>
                <div class="flex justify-between items-center"><span class="flex items-center gap-1.5"><span class="w-2 h-2 rounded-full bg-cyan-500"></span>紫霞観光線</span><span class="text-cyan-300 font-mono-num">56〜60</span></div>
            </div>
        </nav>

        <main class="flex-1 relative overflow-hidden bg-slate-950">
            
            <!-- TAB 1: 停車駅マトリクス & 種別案内 -->
            <div id="tab-stop-matrix" class="tab-content h-full flex flex-col p-4 space-y-4 overflow-hidden">
                <div class="flex flex-wrap justify-between items-center gap-3 shrink-0">
                    <div>
                        <h2 class="text-lg font-bold text-white flex items-center gap-2">
                            <i class="fa-solid fa-layer-group text-purple-400"></i> 種別案内 & 全60駅停車駅マトリクス
                        </h2>
                        <p class="text-xs text-slate-400">全7種別の運行区間・両数編成および停車駅パターン（●停車 / ｜通過）</p>
                    </div>

                    <!-- 種別フィルターボタン群 -->
                    <div class="flex flex-wrap gap-1.5" id="class-filter-container">
                        <!-- Populated by JS -->
                    </div>
                </div>

                <!-- 停車駅マトリクス表示領域 -->
                <div class="flex-1 glass-panel rounded-2xl p-3 border border-purple-900/30 overflow-y-auto space-y-2">
                    <div id="stop-pattern-info-card" class="bg-purple-950/60 p-3 rounded-xl border border-purple-800/40 text-xs flex justify-between items-center mb-3">
                        <div class="space-y-1">
                            <span id="selected-class-title" class="font-bold text-sm text-purple-200">全種別表示中</span>
                            <div id="selected-class-formations" class="text-slate-300 text-[11px]">編成: 4, 6, 8, 10, 4+4, 4+6両対応</div>
                        </div>
                        <div class="text-right font-mono-num text-slate-400 text-[11px]">
                            自動更新同期アクティブ
                        </div>
                    </div>

                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-xs border-collapse">
                            <thead>
                                <tr class="border-b border-purple-900/40 text-slate-400 bg-slate-900/90 sticky top-0 backdrop-blur-md z-10">
                                    <th class="p-2.5 w-16">番号</th>
                                    <th class="p-2.5 min-w-[140px]">駅名</th>
                                    <th class="p-2.5 w-28">所属路線</th>
                                    <th class="p-2.5 text-center w-16">特急</th>
                                    <th class="p-2.5 text-center w-16">通勤急行</th>
                                    <th class="p-2.5 text-center w-16">急行</th>
                                    <th class="p-2.5 text-center w-16">通勤快速</th>
                                    <th class="p-2.5 text-center w-16">快速</th>
                                    <th class="p-2.5 text-center w-16">準急</th>
                                    <th class="p-2.5 text-center w-16">普通</th>
                                </tr>
                            </thead>
                            <tbody id="station-table-body" class="divide-y divide-purple-900/20 font-mono-num">
                                <!-- JS Generated Station Table -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- TAB 2: 全線リアルタイム運行監視 -->
            <div id="tab-live-map" class="tab-content hidden h-full flex flex-col p-4 space-y-3">
                <div class="flex justify-between items-center shrink-0">
                    <div>
                        <h2 class="text-lg font-bold text-white flex items-center gap-2">
                            <i class="fa-solid fa-satellite text-cyan-400"></i> 紫句守鉄道 全線リアルタイム運行モニター
                        </h2>
                        <p class="text-xs text-slate-400">他デバイスでの運転操作やダイヤの移動がリアルタイム描画されます</p>
                    </div>
                    <div class="flex space-x-2">
                        <button onclick="refreshLiveMap()" class="px-3 py-1.5 bg-purple-900/50 hover:bg-purple-800/50 text-xs rounded-xl border border-purple-700/50 text-purple-200">
                            <i class="fa-solid fa-arrows-rotate mr-1"></i> 再描写
                        </button>
                    </div>
                </div>

                <div class="flex-1 glass-panel rounded-2xl relative overflow-hidden border border-purple-900/40">
                    <canvas id="network-canvas" class="w-full h-full block bg-slate-950"></canvas>

                    <div class="absolute bottom-3 left-3 bg-slate-900/90 p-3 rounded-xl border border-purple-800/40 text-[11px] space-y-1">
                        <div class="font-bold text-purple-300">リアルタイム列車位置</div>
                        <div id="live-train-counter" class="text-slate-300 font-mono-num">稼働中列車: 12 編成</div>
                    </div>
                </div>
            </div>

            <!-- TAB 3: 本格運転士シミュレーター -->
            <div id="tab-driver" class="tab-content hidden h-full flex flex-col p-3 space-y-3 overflow-y-auto">
                <div class="glass-panel p-3 rounded-xl flex flex-wrap justify-between items-center gap-2 border border-purple-900/30 shrink-0">
                    <div class="flex items-center space-x-3">
                        <select id="driver-train-select" onchange="onDriverTrainChange()" class="bg-slate-900 border border-purple-800/60 rounded-lg p-2 text-xs text-white font-bold">
                            <!-- JS Populates Train Units -->
                        </select>
                        <div class="text-xs flex items-center space-x-2">
                            <span id="driver-class-badge" class="badge-tokkyu px-2 py-0.5 rounded font-bold">特急</span>
                            <span id="driver-route-name" class="text-slate-200 font-bold">紫句守中央 行</span>
                        </div>
                    </div>

                    <div class="led-display px-4 py-1.5 rounded-lg flex items-center space-x-6 text-xs font-mono-num">
                        <div>
                            <span class="text-slate-500">次駅:</span>
                            <span id="driver-next-station" class="text-amber-400 font-bold ml-1">3 紫雲野</span>
                        </div>
                        <div>
                            <span class="text-slate-500">目標時間:</span>
                            <span id="driver-target-time" class="text-emerald-400 font-bold ml-1">12:05:00</span>
                        </div>
                    </div>
                </div>

                <!-- Driver Cab Visual -->
                <div class="relative w-full h-52 md:h-64 rounded-2xl overflow-hidden border border-purple-900/40 glass-panel shrink-0">
                    <canvas id="cab-canvas" class="w-full h-full block"></canvas>

                    <div class="absolute top-3 left-3 bg-slate-950/85 p-3 rounded-xl border border-purple-800/40 flex items-center space-x-4 backdrop-blur-md">
                        <div class="text-center">
                            <div class="text-[9px] text-slate-400 font-bold">速度 SPEED</div>
                            <div class="text-3xl font-extrabold font-mono-num text-cyan-400" id="driver-speed">0</div>
                            <div class="text-[9px] text-slate-400">km/h</div>
                        </div>
                        <div class="h-8 w-[1px] bg-slate-800"></div>
                        <div class="text-center">
                            <div class="text-[9px] text-slate-400 font-bold">目標残距離 DIST</div>
                            <div class="text-2xl font-bold font-mono-num text-amber-400" id="driver-dist">850</div>
                            <div class="text-[9px] text-slate-400">m</div>
                        </div>
                    </div>

                    <div id="driver-arrival-msg" class="hidden absolute inset-0 bg-purple-950/80 backdrop-blur-md flex flex-col items-center justify-center space-y-2">
                        <div class="text-2xl font-black text-amber-300">停車完了！</div>
                        <div id="driver-stop-accuracy" class="text-sm font-mono-num text-white">停車誤差: +0.25m</div>
                        <button onclick="advanceNextStation()" class="px-4 py-2 bg-purple-600 hover:bg-purple-500 text-white font-bold text-xs rounded-xl shadow-lg">次駅へ発車</button>
                    </div>
                </div>

                <!-- Master Controller Dashboard -->
                <div class="glass-panel rounded-2xl p-4 border border-purple-900/40 flex flex-col space-y-3 shrink-0">
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
                        <!-- Mascon Slider Control -->
                        <div class="bg-slate-900/80 p-3 rounded-xl border border-purple-900/30 flex flex-col justify-between space-y-2">
                            <div class="flex justify-between items-center text-xs">
                                <span class="font-bold text-slate-300">主正逆マスコン (P1-P5 / B1-B5 / EB)</span>
                                <span id="driver-notch-label" class="font-bold font-mono-num text-amber-400">N (切)</span>
                            </div>
                            <input type="range" id="driver-notch-slider" min="-6" max="5" value="0" step="1" oninput="onNotchChange(this.value)" class="w-full h-3 bg-slate-800 rounded appearance-none cursor-pointer accent-purple-500">
                            <div class="grid grid-cols-3 gap-1.5 text-xs">
                                <button onclick="stepNotch(-1)" class="py-2 bg-slate-800 hover:bg-slate-700 rounded text-slate-300 font-bold">Bブレーキ</button>
                                <button onclick="setNotch(0)" class="py-2 bg-slate-800 hover:bg-slate-700 rounded text-amber-400 font-bold">N 惰行</button>
                                <button onclick="stepNotch(1)" class="py-2 bg-purple-700 hover:bg-purple-600 rounded text-white font-bold">P加速</button>
                            </div>
                        </div>

                        <!-- Horn & Sound Actions -->
                        <div class="bg-slate-900/80 p-3 rounded-xl border border-purple-900/30 space-y-2">
                            <div class="text-xs font-bold text-slate-300">保安装置 & 効果音</div>
                            <div class="grid grid-cols-2 gap-2 text-xs">
                                <button onclick="playHorn()" class="p-2.5 bg-amber-600 hover:bg-amber-500 text-white font-bold rounded-xl flex flex-col items-center justify-center transition active:scale-95">
                                    <i class="fa-solid fa-bullhorn text-sm"></i> 警笛鳴動
                                </button>
                                <button onclick="playChime()" class="p-2.5 bg-purple-800 hover:bg-purple-700 text-purple-200 font-bold rounded-xl flex flex-col items-center justify-center transition active:scale-95">
                                    <i class="fa-solid fa-music text-sm"></i> 車内メロディ
                                </button>
                            </div>
                        </div>

                        <!-- Fleet Specifications -->
                        <div class="bg-slate-900/80 p-3 rounded-xl border border-purple-900/30 text-xs space-y-1.5">
                            <div class="font-bold text-purple-300">運転中編成諸元</div>
                            <div id="driver-train-spec" class="text-slate-300 space-y-1 text-[11px]">
                                <div>形式: S100系 (特急形)</div>
                                <div>編成両数: 10両編成 (4+6両分割対応)</div>
                                <div>最高営業速度: 120 km/h</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- TAB 4: 車両基地 & 編成カスタマイズ -->
            <div id="tab-fleet" class="tab-content hidden h-full flex flex-col p-4 space-y-4 overflow-y-auto">
                <div class="flex justify-between items-center">
                    <div>
                        <h2 class="text-lg font-bold text-white flex items-center gap-2">
                            <i class="fa-solid fa-train-subway text-amber-400"></i> 紫句守鉄道 車両基地 & 編成カスタマイズ
                        </h2>
                        <p class="text-xs text-slate-400">形式・塗装・編成両数を変更すると全端末へリアルタイム同期されます</p>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4" id="fleet-card-container">
                    <!-- Populated by JS -->
                </div>
            </div>

            <!-- TAB 5: 経営＆アナリティクス -->
            <div id="tab-dash" class="tab-content hidden h-full flex flex-col p-4 space-y-4 overflow-y-auto">
                <div>
                    <h2 class="text-lg font-bold text-white flex items-center gap-2">
                        <i class="fa-solid fa-chart-line text-emerald-400"></i> 紫句守鉄道 経営ダッシュボード
                    </h2>
                    <p class="text-xs text-slate-400">全線の輸送実績、運賃収入、および他デバイスとの同期ログ</p>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                    <div class="glass-panel p-4 rounded-2xl border border-purple-900/40">
                        <div class="text-xs text-slate-400">本日の総輸送人員</div>
                        <div id="dash-passengers" class="text-2xl font-extrabold font-mono-num text-purple-300 mt-1">128,450 人</div>
                    </div>
                    <div class="glass-panel p-4 rounded-2xl border border-purple-900/40">
                        <div class="text-xs text-slate-400">運賃売上合計</div>
                        <div id="dash-revenue" class="text-2xl font-extrabold font-mono-num text-emerald-400 mt-1">¥ 38,535,000</div>
                    </div>
                    <div class="glass-panel p-4 rounded-2xl border border-purple-900/40">
                        <div class="text-xs text-slate-400">ダイヤ定時率</div>
                        <div class="text-2xl font-extrabold font-mono-num text-amber-300 mt-1">99.4 %</div>
                    </div>
                    <div class="glass-panel p-4 rounded-2xl border border-purple-900/40">
                        <div class="text-xs text-slate-400">接続デバイス数</div>
                        <div id="dash-devices" class="text-2xl font-extrabold font-mono-num text-cyan-300 mt-1">2 端末 (同期中)</div>
                    </div>
                </div>

                <div class="glass-panel p-4 rounded-2xl border border-purple-900/40 space-y-2">
                    <div class="font-bold text-sm text-purple-200">リアルタイム同期アクティビティログ</div>
                    <div id="sync-log-container" class="h-40 overflow-y-auto text-xs font-mono-num space-y-1 bg-slate-900/80 p-3 rounded-xl border border-purple-900/30">
                        <div class="text-slate-400">[SYSTEM] 紫句守鉄道 クラウド同期ネットワーク初期化完了</div>
                    </div>
                </div>
            </div>

        </main>
    </div>

    <script>
        // 紫句守鉄道（しのもり鉄道）マスターデータ
        const ShinmoriData = {
            syncKey: "SHINMORI-88",
            audioEnabled: false,
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
            classes: [
                {
                    name: "特急",
                    badgeClass: "badge-tokkyu",
                    formations: "4＋6, 4, 6, 10両",
                    patterns: [
                        ["1","3","13","16","17","23","28","30"],
                        ["1","3","13","16","17","23","28","35","40"],
                        ["1","3","13","43","44","50","52","55"],
                        ["1","3","13","16","17","23","28","56","60"]
                    ]
                },
                {
                    name: "通勤急行",
                    badgeClass: "badge-tsukin-kyuko",
                    formations: "4＋6, 10両",
                    patterns: [
                        ["1","3","6","13","16","17","20","24","28","30"],
                        ["1","3","6","13","43","48","50","52","55"],
                        ["1","3","6","13","16","17","20","24","28","56","60"]
                    ]
                },
                {
                    name: "急行",
                    badgeClass: "badge-kyuko",
                    formations: "4＋4, 4＋6, 8, 10両",
                    patterns: [
                        ["09","06","04","02","1","3","6","9","13","16","17","20","23","28","30"],
                        ["09","06","04","02","1","2","3","6","9","13","43","44","48","50","52","55"],
                        ["09","06","04","02","1","3","6","9","13","16","17","20","23","28","56","60"],
                        ["1","3","6","9","13","16","17","20","23","28","30"],
                        ["1","3","6","9","13","43","44","48","50","52","55"],
                        ["1","3","6","9","13","16","17","20","23","28","56","60"]
                    ]
                },
                {
                    name: "通勤快速",
                    badgeClass: "badge-tsukin-kaisoku",
                    formations: "4＋4, 4＋6, 8, 10両",
                    patterns: [
                        ["1","3","5","7","9","13","16","17","20","23","24","28","30"],
                        ["1","3","5","7","9","13","42","44","45","46","48","50","52","53","55"],
                        ["1","3","5","7","9","13","16","17","20","23","24","28","56","57","58","60"]
                    ]
                },
                {
                    name: "快速",
                    badgeClass: "badge-kaisoku",
                    formations: "4＋4, 4＋6, 6, 8, 10両",
                    patterns: [
                        ["1","3","5","6","7","9","13","16","17","20","23","24","28","30"],
                        ["1","3","5","6","7","9","13","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55"],
                        ["1","3","5","6","7","9","13","16","17","20","23","24","28","56","57","58","60"]
                    ]
                },
                {
                    name: "準急",
                    badgeClass: "badge-junkyu",
                    formations: "4＋4, 4＋6, 6, 8, 10両",
                    patterns: [
                        ["1","3","5","6","7","9","11","13","16","17","19","20","23","24","25","26","28","29","30"],
                        ["1","3","5","6","7","9","11","13","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55"],
                        ["1","3","5","6","7","9","11","13","16","17","19","20","23","24","25","26","28","56","57","58","59","60"]
                    ]
                },
                {
                    name: "普通",
                    badgeClass: "badge-futsu",
                    formations: "4＋4, 4＋6, 4, 6, 8, 10両",
                    patterns: [
                        ["09","08","07","06","05","04","03","02","1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30"],
                        ["09","08","07","06","05","04","03","02","1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40"],
                        ["09","08","07","06","05","04","03","02","1","2","3","4","5","6","7","8","9","10","11","12","13","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55"],
                        ["09","08","07","06","05","04","03","02","1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","55","56","57","58","59","60"],
                        ["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30"],
                        ["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40"],
                        ["1","2","3","4","5","6","7","8","9","10","11","12","13","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55"],
                        ["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","56","57","58","59","60"]
                    ]
                }
            ],
            fleets: [
                { id: "S1", series: "S1系", color: "#a855f7", formation: "10両編成 (01-05) / 8両編成 (06-10)", desc: "紫句守鉄道の標準型通勤電車。" },
                { id: "S2", series: "S2系", color: "#6366f1", formation: "10両編成 (01-15) / 8両編成 (16-25)", desc: "高加減速VVVF制御装置を搭載した主力機。" },
                { id: "S3", series: "S3系", color: "#10b981", formation: "10両/8両/6両/4両", desc: "分割併合(4+6両, 4+4両等)に柔軟対応する汎用型。" },
                { id: "S4", series: "S4系", color: "#06b6d4", formation: "10両/8両/6両", desc: "支線・高原線の勾配区間に対応した軽量車体。" },
                { id: "S5", series: "S5系", color: "#f59e0b", formation: "10両編成 (01-04)", desc: "本線の混雑緩和を目的とした最新型車両。" },
                { id: "S100", series: "S100系", color: "#ec4899", formation: "10両 (4+6両) / 6両 / 4両", desc: "看板特急「シノモリライナー」用特急車両。" },
                { id: "S900", series: "S900系", color: "#eab308", formation: "4両編成 (事業用)", desc: "全線の軌道・架線状態を点検する検測車。" }
            ]
        };

        let currentSelectedClass = "ALL";
        let syncChannel = null;

        // 自動クラウド同期エンジンの初期化
        function initAutoSync() {
            if ('BroadcastChannel' in window) {
                syncChannel = new BroadcastChannel('shinmori_sync_' + ShinmoriData.syncKey);
                syncChannel.onmessage = (e) => {
                    handleSyncMessage(e.data);
                };
            }
            addSyncLog(`[SYNC] クラウド通信チャネル接続完了 (キー: ${ShinmoriData.syncKey})`);
        }

        function broadcastStateChange(action, payload) {
            const data = {
                action: action,
                payload: payload,
                timestamp: Date.now()
            };
            if (syncChannel) {
                syncChannel.postMessage(data);
            }
            // ローカルストレージ自動保存
            localStorage.setItem('shinmori_app_state', JSON.stringify({
                syncKey: ShinmoriData.syncKey,
                fleets: ShinmoriData.fleets,
                lastUpdate: Date.now()
            }));
        }

        function handleSyncMessage(data) {
            addSyncLog(`[RECV] 外部端末より更新受信: ${data.action}`);
            if (data.action === 'NOTCH_CHANGE') {
                driverNotch = data.payload.notch;
                document.getElementById('driver-notch-slider').value = driverNotch;
                updateNotchDisplay();
            } else if (data.action === 'FLEET_UPDATE') {
                const target = ShinmoriData.fleets.find(f => f.id === data.payload.id);
                if (target) {
                    target.color = data.payload.color;
                    target.formation = data.payload.formation;
                    renderFleetCards();
                }
            }
        }

        function changeSyncKey() {
            const newKey = prompt("新しい自動同期キーを入力してください（同じキーの全デバイスと同期します）:", ShinmoriData.syncKey);
            if (newKey && newKey.trim() !== "") {
                ShinmoriData.syncKey = newKey.trim();
                document.getElementById('sync-key-display').innerText = ShinmoriData.syncKey;
                initAutoSync();
            }
        }

        function addSyncLog(msg) {
            const container = document.getElementById('sync-log-container');
            if (!container) return;
            const item = document.createElement('div');
            item.className = "text-slate-300";
            item.innerText = `[${new Date().toTimeString().split(' ')[0]}] ${msg}`;
            container.appendChild(item);
            container.scrollTop = container.scrollHeight;
        }

        // タブ切り替え処理
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

                if (target === 'tab-live-map') renderNetworkCanvas();
                if (target === 'tab-driver') renderCabCanvas();
            });
        });

        // 種別フィルター初期化
        function initClassFilters() {
            const container = document.getElementById('class-filter-container');
            container.innerHTML = `<button data-class="ALL" class="class-btn px-3 py-1 rounded-lg text-xs font-bold bg-purple-600 text-white border border-purple-400">全種別表示</button>`;
            
            ShinmoriData.classes.forEach(c => {
                const btn = document.createElement('button');
                btn.setAttribute('data-class', c.name);
                btn.className = `class-btn px-3 py-1 rounded-lg text-xs font-bold ${c.badgeClass} opacity-80 hover:opacity-100 transition`;
                btn.innerText = c.name;
                container.appendChild(btn);
            });

            document.querySelectorAll('.class-btn').forEach(btn => {
                btn.addEventListener('click', () => {
                    currentSelectedClass = btn.getAttribute('data-class');
                    renderStationMatrix();
                });
            });
        }

        // 停車駅マトリクス描画
        function renderStationMatrix() {
            const tbody = document.getElementById('station-table-body');
            tbody.innerHTML = '';

            const activeClassObj = ShinmoriData.classes.find(c => c.name === currentSelectedClass);
            if (activeClassObj) {
                document.getElementById('selected-class-title').innerText = `【${activeClassObj.name}】停車駅パターン`;
                document.getElementById('selected-class-formations').innerText = `編成両数: ${activeClassObj.formations}`;
            } else {
                document.getElementById('selected-class-title').innerText = "全列車種別・停車駅一覧";
                document.getElementById('selected-class-formations').innerText = "編成両数: 4, 6, 8, 10, 4+4, 4+6両";
            }

            ShinmoriData.stations.forEach(st => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-purple-900/20 border-b border-purple-900/20";

                let rowHtml = `
                    <td class="p-2.5 font-bold text-purple-300">${st.id}</td>
                    <td class="p-2.5 font-bold text-white">${st.name}</td>
                    <td class="p-2.5 text-slate-400 text-[11px]">${st.line}</td>
                `;

                ShinmoriData.classes.forEach(cls => {
                    if (currentSelectedClass !== "ALL" && currentSelectedClass !== cls.name) {
                        rowHtml += `<td class="p-2.5 text-center text-slate-700 opacity-20">｜</td>`;
                        return;
                    }

                    let stops = false;
                    cls.patterns.forEach(pat => {
                        if (pat.includes(st.id)) stops = true;
                    });

                    if (stops) {
                        rowHtml += `<td class="p-2.5 text-center font-bold text-emerald-400">●</td>`;
                    } else {
                        rowHtml += `<td class="p-2.5 text-center text-slate-600">｜</td>`;
                    }
                });

                tr.innerHTML = rowHtml;
                tbody.appendChild(tr);
            });
        }

        // 車両基地・編成カスタムカード描画
        function renderFleetCards() {
            const container = document.getElementById('fleet-card-container');
            container.innerHTML = '';

            ShinmoriData.fleets.forEach(f => {
                const card = document.createElement('div');
                card.className = "glass-panel p-4 rounded-2xl border border-purple-900/40 space-y-3";
                card.innerHTML = `
                    <div class="flex justify-between items-center border-b border-purple-900/30 pb-2">
                        <h3 class="font-bold text-lg text-amber-300">${f.series}</h3>
                        <span class="text-xs px-2 py-0.5 rounded bg-purple-950 text-purple-300 border border-purple-800">在籍形式</span>
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
                    <div class="text-xs space-y-2">
                        <div class="flex justify-between items-center">
                            <span class="text-slate-400 font-bold">帯カラー設定:</span>
                            <input type="color" value="${f.color}" onchange="updateFleetColor('${f.id}', this.value)" class="w-8 h-6 bg-transparent border-0 cursor-pointer">
                        </div>
                        <div>
                            <span class="text-slate-400 font-bold">編成:</span>
                            <input type="text" value="${f.formation}" onchange="updateFleetFormation('${f.id}', this.value)" class="w-full mt-1 bg-slate-900 border border-purple-800/50 rounded px-2 py-1 text-xs text-white">
                        </div>
                        <p class="text-slate-400 leading-relaxed text-[11px]">${f.desc}</p>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        function updateFleetColor(id, newColor) {
            const fleet = ShinmoriData.fleets.find(f => f.id === id);
            if (fleet) {
                fleet.color = newColor;
                renderFleetCards();
                broadcastStateChange('FLEET_UPDATE', { id: id, color: newColor, formation: fleet.formation });
            }
        }

        function updateFleetFormation(id, newFormation) {
            const fleet = ShinmoriData.fleets.find(f => f.id === id);
            if (fleet) {
                fleet.formation = newFormation;
                broadcastStateChange('FLEET_UPDATE', { id: id, color: fleet.color, formation: newFormation });
            }
        }

        let driverSpeed = 0;
        let driverNotch = 0;
        let driverDist = 850;

        function onNotchChange(val) {
            driverNotch = parseInt(val);
            updateNotchDisplay();
            broadcastStateChange('NOTCH_CHANGE', { notch: driverNotch });
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
                label.innerText = `P${driverNotch} (力行)`;
                label.className = "font-bold font-mono-num text-emerald-400";
            } else if (driverNotch === 0) {
                label.innerText = "N (切/惰行)";
                label.className = "font-bold font-mono-num text-amber-400";
            } else if (driverNotch === -6) {
                label.innerText = "EB (非常ブレーキ)";
                label.className = "font-bold font-mono-num text-red-500 animate-pulse";
            } else {
                label.innerText = `B${Math.abs(driverNotch)} (制動)`;
                label.className = "font-bold font-mono-num text-red-400";
            }
        }

        // Web Audio API サウンド合成
        let audioCtx = null;
        function toggleSound() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
            ShinmoriData.audioEnabled = !ShinmoriData.audioEnabled;
            const icon = document.getElementById('icon-sound');
            if (ShinmoriData.audioEnabled) {
                icon.className = "fa-solid fa-volume-high text-emerald-400";
                addSyncLog("[AUDIO] オーディオ機能が有効化されました");
            } else {
                icon.className = "fa-solid fa-volume-xmark text-red-400";
            }
        }

        function playHorn() {
            if (!ShinmoriData.audioEnabled || !audioCtx) return;
            const osc1 = audioCtx.createOscillator();
            const osc2 = audioCtx.createOscillator();
            const gain = audioCtx.createGain();

            osc1.type = 'triangle';
            osc2.type = 'triangle';
            osc1.frequency.setValueAtTime(320, audioCtx.currentTime);
            osc2.frequency.setValueAtTime(480, audioCtx.currentTime);

            gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 1.2);

            osc1.connect(gain);
            osc2.connect(gain);
            gain.connect(audioCtx.destination);

            osc1.start();
            osc2.start();
            osc1.stop(audioCtx.currentTime + 1.2);
            osc2.stop(audioCtx.currentTime + 1.2);
            addSyncLog("[SOUND] 警笛を吹鳴しました");
        }

        function playChime() {
            if (!ShinmoriData.audioEnabled || !audioCtx) return;
            const notes = [523.25, 659.25, 783.99, 1046.50];
            notes.forEach((freq, idx) => {
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(freq, audioCtx.currentTime + idx * 0.25);
                gain.gain.setValueAtTime(0.2, audioCtx.currentTime + idx * 0.25);
                gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + idx * 0.25 + 0.6);
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                osc.start(audioCtx.currentTime + idx * 0.25);
                osc.stop(audioCtx.currentTime + idx * 0.25 + 0.6);
            });
            addSyncLog("[SOUND] 車内チャイムを再生しました");
        }

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

            // 路線ネットワークのダイナミック描画
            const cy = h / 2;
            networkCtx.strokeStyle = "#8b5cf6";
            networkCtx.lineWidth = 6;
            networkCtx.beginPath();
            networkCtx.moveTo(40, cy);
            networkCtx.lineTo(w - 40, cy);
            networkCtx.stroke();

            // 60駅のプロット
            const step = (w - 80) / 30;
            for (let i = 0; i <= 30; i++) {
                const x = 40 + i * step;
                networkCtx.fillStyle = "#ffffff";
                networkCtx.beginPath();
                networkCtx.arc(x, cy, 4, 0, Math.PI * 2);
                networkCtx.fill();
            }
        }

        function refreshLiveMap() {
            renderNetworkCanvas();
            addSyncLog("[MAP] 全線運行マップを手動更新しました");
        }

        function renderCabCanvas() {
            if (!cabCanvas.parentElement) return;
            cabCanvas.width = cabCanvas.parentElement.clientWidth;
            cabCanvas.height = cabCanvas.parentElement.clientHeight;

            const w = cabCanvas.width;
            const h = cabCanvas.height;

            cabCtx.clearRect(0, 0, w, h);

            // 背景描画
            cabCtx.fillStyle = "#0c1021";
            cabCtx.fillRect(0, 0, w, h * 0.5);
            cabCtx.fillStyle = "#05030a";
            cabCtx.fillRect(0, h * 0.5, w, h * 0.5);

            // 擬似軌道
            cabCtx.strokeStyle = "#8b5cf6";
            cabCtx.lineWidth = 3;
            cabCtx.beginPath();
            cabCtx.moveTo(w / 2 - 10, h * 0.5);
            cabCtx.lineTo(w / 2 - 160, h);
            cabCtx.stroke();

            cabCtx.beginPath();
            cabCtx.moveTo(w / 2 + 10, h * 0.5);
            cabCtx.lineTo(w / 2 + 160, h);
            cabCtx.stroke();
        }

        function initDriverTrainSelect() {
            const select = document.getElementById('driver-train-select');
            select.innerHTML = `
                <option value="S100">S100系 特急 (紫句守中央発 紫霞野・星句高原行)</option>
                <option value="S1">S1系 通勤急行 (紫句守中央発 句守港行)</option>
                <option value="S3">S3系 快速 (直通線発 紫句守展示場行)</option>
            `;
        }

        function onDriverTrainChange() {
            addSyncLog("[DRIVER] 運転対象編成を変更しました");
        }

        function advanceNextStation() {
            driverDist = 850;
            document.getElementById('driver-arrival-msg').classList.add('hidden');
            addSyncLog("[DRIVER] 次の駅に向けて発車しました");
        }

        // 定期物理更新 loop
        setInterval(() => {
            if (driverNotch > 0) {
                driverSpeed = Math.min(120, driverSpeed + driverNotch * 0.25);
            } else if (driverNotch < 0) {
                driverSpeed = Math.max(0, driverSpeed - Math.abs(driverNotch) * 0.5);
            }

            if (driverSpeed > 0) {
                driverDist = Math.max(0, Math.round(driverDist - (driverSpeed * 0.04)));
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

        function exportDataJSON() {
            const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(ShinmoriData, null, 2));
            const downloadAnchor = document.createElement('a');
            downloadAnchor.setAttribute("href", dataStr);
            downloadAnchor.setAttribute("download", `shinmori_railway_data.json`);
            document.body.appendChild(downloadAnchor);
            downloadAnchor.click();
            downloadAnchor.remove();
            addSyncLog("[EXPORT] 全データをJSONファイルへバックアップしました");
        }

        function triggerImportJSON() {
            document.getElementById('json-file-input').click();
        }

        function importDataJSON(event) {
            const fileReader = new FileReader();
            fileReader.onload = function(e) {
                try {
                    const imported = JSON.parse(e.target.result);
                    if (imported.fleets) ShinmoriData.fleets = imported.fleets;
                    renderFleetCards();
                    renderStationMatrix();
                    addSyncLog("[IMPORT] 外部JSONデータよりアプリ状態を完全に復元しました");
                    alert("紫句守鉄道のデータを正常に復元・更新しました。");
                } catch (err) {
                    alert("無効なJSONファイルです。");
                }
            };
            fileReader.readAsText(event.target.files[0]);
        }

        // アプリ起動時の初期化
        window.onload = function() {
            initAutoSync();
            initClassFilters();
            renderStationMatrix();
            renderFleetCards();
            initDriverTrainSelect();
            addSyncLog("[SYSTEM] 紫句守鉄道 統合アプリケーション準備完了");
        };
    </script>
</body>
</html>
