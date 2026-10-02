<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>クラウド対応 オリジナル鉄道 総合運行管理システム (OCC)</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome for Railway Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter & JetBrains Mono for High-Tech OCC Feel -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&family=JetBrains+Mono:wght@400;700&display=swap" rel="stylesheet">

    <!-- Firebase SDK Modular Scripts -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, onSnapshot, setDoc, updateDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Provided User Configuration embedded directly
        const firebaseConfig = {
            apiKey: "AIzaSyAIIwx9sg3QmgvgBN0I25DihN2atuWThWw",
            authDomain: "original-tetsudo.firebaseapp.com",
            databaseURL: "https://original-tetsudo-default-rtdb.firebaseio.com",
            projectId: "original-tetsudo",
            storageBucket: "original-tetsudo.firebasestorage.app",
            messagingSenderId: "922746472293",
            appId: "1:922746472293:web:4f1585e81205c739da438c",
            measurementId: "G-MNZ2W9CNZK"
        };

        // App Initialization
        window.firebaseApp = initializeApp(firebaseConfig);
        window.db = getFirestore(window.firebaseApp);
        window.auth = getAuth(window.firebaseApp);

        // Anonymous auth for Firestore security access
        signInAnonymously(window.auth).then(() => {
            console.log("Firebase Authenticated Anonymously");
            window.initCloudSync();
        }).catch(err => {
            console.warn("Auth failed or working offline:", err);
            window.initCloudSync();
        });
    </script>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        railway: {
                            dark: '#0f172a',
                            panel: '#1e293b',
                            border: '#334155',
                            accent: '#06b6d4',
                            warning: '#f59e0b',
                            danger: '#ef4444',
                            success: '#10b981'
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        mono: ['JetBrains Mono', 'monospace']
                    }
                }
            }
        };
    </script>

    <style>
        /* Custom OCC High-Tech Styling */
        body {
            background-color: #0b0f19;
            color: #f8fafc;
            font-family: 'Inter', sans-serif;
            overflow-x: hidden;
        }

        .digital-clock {
            font-family: 'JetBrains Mono', monospace;
            text-shadow: 0 0 10px rgba(6, 182, 212, 0.5);
        }

        /* Pulse Animations for Dispatch Lights */
        @keyframes emergency-glow {
            0%, 100% { background-color: rgba(239, 68, 68, 0.2); border-color: #ef4444; }
            50% { background-color: rgba(239, 68, 68, 0.8); border-color: #fca5a5; box-shadow: 0 0 20px #ef4444; }
        }

        .emergency-active {
            animation: emergency-glow 1s infinite;
        }

        /* Scrollbar styles */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #0f172a;
        }
        ::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #06b6d4;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between selection:bg-cyan-500 selection:text-white">

    <!-- Top OCC Navigation / Control Header -->
    <header class="bg-slate-900/90 border-b border-slate-800 backdrop-blur-md sticky top-0 z-50 px-4 py-2.5 flex flex-wrap items-center justify-between gap-4">
        <div class="flex items-center gap-3">
            <div class="bg-cyan-500/10 p-2 rounded-lg border border-cyan-500/30 flex items-center justify-center">
                <i class="fa-solid font-bold fa-train-subway text-cyan-400 text-xl"></i>
            </div>
            <div>
                <div class="flex items-center gap-2">
                    <h1 id="companyNameDisplay" class="font-black text-lg text-white tracking-wide">未来都市高速鉄道</h1>
                    <span class="text-xs bg-slate-800 text-cyan-400 px-2 py-0.5 rounded-full border border-slate-700" id="lineNameDisplay">中央本線</span>
                </div>
                <p class="text-xs text-slate-400">総合運行管理システム (OCC Dispatch Control Center)</p>
            </div>
        </div>

        <!-- Sync Indicator & Digital Clock -->
        <div class="flex items-center gap-4">
            <!-- Sync Status Badge -->
            <div id="syncBadge" class="flex items-center gap-2 px-3 py-1 rounded-full text-xs bg-amber-500/10 border border-amber-500/30 text-amber-400 transition-all">
                <span class="relative flex h-2 w-2">
                  <span id="syncPing" class="animate-ping absolute inline-flex h-full w-full rounded-full bg-amber-400 opacity-75"></span>
                  <span id="syncDot" class="relative inline-flex rounded-full h-2 w-2 bg-amber-500"></span>
                </span>
                <span id="syncText">クラウド接続中...</span>
            </div>

            <!-- Digital Clock -->
            <div class="bg-slate-950 px-3 py-1 rounded border border-slate-800 text-right">
                <div class="text-xs text-slate-500 uppercase tracking-widest">OCC TIME</div>
                <div id="occClock" class="digital-clock font-bold text-cyan-400 text-base leading-none">12:00:00</div>
            </div>
        </div>
    </header>

    <!-- Main Content Shell -->
    <div class="flex-1 flex flex-col p-4 max-w-[1700px] w-full mx-auto space-y-4">

        <!-- Emergency Alert Banner (Hidden by Default) -->
        <div id="emergencyBanner" class="hidden emergency-active border rounded-lg p-3 flex items-center justify-between text-red-200">
            <div class="flex items-center gap-3">
                <i class="fa-solid fa-triangle-exclamation text-2xl text-red-400 animate-bounce"></i>
                <div>
                    <div class="font-bold text-sm tracking-wider uppercase text-red-100">防護発砲指令・全線運転見合わせ中</div>
                    <div class="text-xs text-red-300">緊急指令により全箇所の列車進行が自動停止しています。指示に従い安全を確認してください。</div>
                </div>
            </div>
            <button onclick="toggleEmergencyStop(false)" class="bg-red-600 hover:bg-red-500 text-white font-bold px-4 py-1.5 rounded text-xs transition shadow-lg">
                防護解除・抑止解除
            </button>
        </div>

        <!-- Tab Navigation Bar -->
        <div class="flex flex-wrap items-center justify-between gap-2 border-b border-slate-800 pb-2">
            <div class="flex flex-wrap gap-1 bg-slate-900 p-1 rounded-lg border border-slate-800">
                <button onclick="switchTab('map')" id="tab-map" class="tab-btn px-4 py-1.5 rounded-md text-xs font-semibold flex items-center gap-2 bg-cyan-600 text-white transition">
                    <i class="fa-solid fa-map-location-dot"></i> リアルタイム路線図
                </button>
                <button onclick="switchTab('dispatch')" id="tab-dispatch" class="tab-btn px-4 py-1.5 rounded-md text-xs font-semibold flex items-center gap-2 text-slate-400 hover:text-white transition">
                    <i class="fa-solid fa-tower-broadcast"></i> 運行指令卓
                </button>
                <button onclick="switchTab('diagram')" id="tab-diagram" class="tab-btn px-4 py-1.5 rounded-md text-xs font-semibold flex items-center gap-2 text-slate-400 hover:text-white transition">
                    <i class="fa-solid fa-chart-line"></i> ダイヤグラム (スジ引き)
                </button>
                <button onclick="switchTab('timetable')" id="tab-timetable" class="tab-btn px-4 py-1.5 rounded-md text-xs font-semibold flex items-center gap-2 text-slate-400 hover:text-white transition">
                    <i class="fa-solid fa-table-list"></i> 駅時刻表管理
                </button>
                <button onclick="switchTab('depot')" id="tab-depot" class="tab-btn px-4 py-1.5 rounded-md text-xs font-semibold flex items-center gap-2 text-slate-400 hover:text-white transition">
                    <i class="fa-solid fa-warehouse"></i> 車両基地・検査管理
                </button>
                <button onclick="switchTab('settings')" id="tab-settings" class="tab-btn px-4 py-1.5 rounded-md text-xs font-semibold flex items-center gap-2 text-slate-400 hover:text-white transition">
                    <i class="fa-solid fa-sliders"></i> 路線・カスタマイズ設定
                </button>
            </div>

            <div class="flex items-center gap-2">
                <button onclick="addNewTrainModal()" class="bg-cyan-600 hover:bg-cyan-500 text-white text-xs font-bold px-3 py-1.5 rounded flex items-center gap-1.5 transition">
                    <i class="fa-solid fa-plus"></i> 列車新設 (臨時増発)
                </button>
                <button onclick="toggleEmergencyStop(true)" class="bg-red-600 hover:bg-red-700 text-white text-xs font-bold px-3 py-1.5 rounded flex items-center gap-1.5 transition shadow-lg shadow-red-900/30">
                    <i class="fa-solid fa-hand"></i> 防護発砲 (全線非常停止)
                </button>
            </div>
        </div>

        <!-- TAB CONTENT 1: LIVE RAILWAY MAP -->
        <div id="view-map" class="tab-view space-y-4">
            <div class="bg-slate-900 rounded-xl border border-slate-800 p-4 relative overflow-hidden shadow-2xl">
                <div class="flex items-center justify-between mb-2">
                    <div class="flex items-center gap-2">
                        <span class="w-2.5 h-2.5 rounded-full bg-cyan-400 animate-ping"></span>
                        <h2 class="text-sm font-bold text-slate-200">全線 リアルタイム在線モニター</h2>
                    </div>
                    <div class="text-xs text-slate-400 flex items-center gap-4">
                        <span class="flex items-center gap-1"><span class="w-3 h-1.5 bg-cyan-500 rounded-full inline-block"></span> 普通</span>
                        <span class="flex items-center gap-1"><span class="w-3 h-1.5 bg-emerald-500 rounded-full inline-block"></span> 快速</span>
                        <span class="flex items-center gap-1"><span class="w-3 h-1.5 bg-amber-500 rounded-full inline-block"></span> 急行</span>
                        <span class="flex items-center gap-1"><span class="w-3 h-1.5 bg-red-500 rounded-full inline-block"></span> 特急</span>
                    </div>
                </div>

                <!-- Interactive SVG Railway Track Container -->
                <div id="mapContainer" class="w-full h-[380px] bg-slate-950 rounded-lg border border-slate-800 relative overflow-x-auto overflow-y-hidden p-2">
                    <svg id="railwaySvg" class="w-full h-full min-w-[1000px]"></svg>
                </div>
            </div>

            <!-- Live Active Fleet Summary Cards -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-3" id="trainStatusGrid">
                <!-- Dynamically populated via JS -->
            </div>
        </div>

        <!-- TAB CONTENT 2: DISPATCH CONTROL -->
        <div id="view-dispatch" class="tab-view hidden space-y-4">
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-4">
                <!-- Dispatch Actions -->
                <div class="bg-slate-900 border border-slate-800 rounded-xl p-4 space-y-4">
                    <h3 class="text-sm font-bold text-cyan-400 flex items-center gap-2 border-b border-slate-800 pb-2">
                        <i class="fa-solid fa-bullhorn"></i> 運行指令・徐行発令
                    </h3>

                    <!-- Global Operations -->
                    <div class="space-y-2">
                        <label class="text-xs text-slate-400 font-semibold">一括指令</label>
                        <div class="grid grid-cols-2 gap-2">
                            <button onclick="setGlobalDelay(5)" class="bg-amber-600/20 border border-amber-500/40 hover:bg-amber-600/40 text-amber-300 text-xs py-2 px-2 rounded font-semibold transition">
                                全線+5分遅延発令
                            </button>
                            <button onclick="clearAllDelays()" class="bg-emerald-600/20 border border-emerald-500/40 hover:bg-emerald-600/40 text-emerald-300 text-xs py-2 px-2 rounded font-semibold transition">
                                全線定時復帰処理
                            </button>
                        </div>
                    </div>

                    <!-- Speed Restriction -->
                    <div class="space-y-2">
                        <label class="text-xs text-slate-400 font-semibold">区間最高速度規制 (徐行)</label>
                        <select id="speedLimitSelect" class="w-full bg-slate-950 border border-slate-700 rounded px-3 py-2 text-xs text-slate-200">
                            <option value="100">通常運転 (制限なし - 100km/h)</option>
                            <option value="60">警戒・徐行 (60km/h制限)</option>
                            <option value="40">強風・悪天候徐行 (40km/h制限)</option>
                            <option value="25">徐行運転 (25km/h制限)</option>
                        </select>
                        <button onclick="applySpeedRestriction()" class="w-full bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 text-xs font-semibold py-2 rounded transition">
                            速度規制適用
                        </button>
                    </div>

                    <!-- Dispatch Broadcast Log -->
                    <div class="space-y-2">
                        <label class="text-xs text-slate-400 font-semibold">指令告知放送・理由記録</label>
                        <textarea id="dispatchNoticeInput" rows="3" class="w-full bg-slate-950 border border-slate-700 rounded p-2 text-xs text-slate-200" placeholder="例: 強風のため全線で徐行運転を行っています。"></textarea>
                        <button onclick="postDispatchNotice()" class="w-full bg-cyan-600 hover:bg-cyan-500 text-white text-xs font-bold py-2 rounded transition">
                            全指令卓へ告知送信
                        </button>
                    </div>
                </div>

                <!-- Train Individual Dispatch Table -->
                <div class="lg:col-span-2 bg-slate-900 border border-slate-800 rounded-xl p-4 flex flex-col">
                    <h3 class="text-sm font-bold text-slate-200 mb-3 flex items-center justify-between border-b border-slate-800 pb-2">
                        <span><i class="fa-solid fa-list-check text-cyan-400 mr-2"></i>個別列車 運行制御一覧</span>
                        <span class="text-xs text-slate-500 font-normal">リアルタイム操作</span>
                    </h3>
                    <div class="overflow-x-auto flex-1">
                        <table class="w-full text-left text-xs text-slate-300">
                            <thead class="bg-slate-950 text-slate-400 uppercase text-[10px]">
                                <tr>
                                    <th class="p-2">列車番号</th>
                                    <th class="p-2">種別 / 行先</th>
                                    <th class="p-2">現在位置</th>
                                    <th class="p-2">遅延</th>
                                    <th class="p-2">状態</th>
                                    <th class="p-2 text-right">指令操作</th>
                                </tr>
                            </thead>
                            <tbody id="dispatchTableBody" class="divide-y divide-slate-800">
                                <!-- Populated dynamically -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>

        <!-- TAB CONTENT 3: DIAGRAM (SUJI) -->
        <div id="view-diagram" class="tab-view hidden space-y-4">
            <div class="bg-slate-900 border border-slate-800 rounded-xl p-4">
                <div class="flex items-center justify-between mb-3">
                    <h3 class="text-sm font-bold text-slate-200 flex items-center gap-2">
                        <i class="fa-solid fa-chart-line text-cyan-400"></i> ダイヤグラム (運行予定スジ引き画面)
                    </h3>
                    <div class="text-xs text-slate-400">
                        縦軸: 各駅位置 / 横軸: 時間 (リアルタイム位置追跡)
                    </div>
                </div>
                <div class="w-full h-[450px] bg-slate-950 rounded-lg border border-slate-800 relative p-2 overflow-auto">
                    <canvas id="diagramCanvas" class="w-full h-full min-w-[800px]"></canvas>
                </div>
            </div>
        </div>

        <!-- TAB CONTENT 4: TIMETABLES -->
        <div id="view-timetable" class="tab-view hidden space-y-4">
            <div class="bg-slate-900 border border-slate-800 rounded-xl p-4">
                <div class="flex items-center justify-between mb-4">
                    <div class="flex items-center gap-3">
                        <i class="fa-solid fa-clock text-cyan-400 text-lg"></i>
                        <h3 class="text-sm font-bold text-slate-200">駅別 発車時刻表データ</h3>
                    </div>
                    <select id="timetableStationSelect" onchange="renderTimetable()" class="bg-slate-950 border border-slate-700 text-xs text-cyan-400 rounded px-3 py-1.5 font-bold">
                        <!-- Dynamic stations options -->
                    </select>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <!-- Down direction timetable -->
                    <div class="bg-slate-950 p-3 rounded-lg border border-slate-800">
                        <h4 class="text-xs font-bold text-cyan-400 mb-2 border-b border-slate-800 pb-1 flex justify-between">
                            <span>下り (下り方面 行き)</span>
                            <span class="text-slate-500">発車便</span>
                        </h4>
                        <div id="timetableDown" class="space-y-1 text-xs font-mono max-h-[300px] overflow-y-auto"></div>
                    </div>

                    <!-- Up direction timetable -->
                    <div class="bg-slate-950 p-3 rounded-lg border border-slate-800">
                        <h4 class="text-xs font-bold text-amber-400 mb-2 border-b border-slate-800 pb-1 flex justify-between">
                            <span>上り (起点方面 行き)</span>
                            <span class="text-slate-500">発車便</span>
                        </h4>
                        <div id="timetableUp" class="space-y-1 text-xs font-mono max-h-[300px] overflow-y-auto"></div>
                    </div>
                </div>
            </div>
        </div>

        <!-- TAB CONTENT 5: DEPOT & FLEET -->
        <div id="view-depot" class="tab-view hidden space-y-4">
            <div class="bg-slate-900 border border-slate-800 rounded-xl p-4">
                <h3 class="text-sm font-bold text-slate-200 mb-4 flex items-center gap-2 border-b border-slate-800 pb-2">
                    <i class="fa-solid fa-warehouse text-cyan-400"></i> 車両基地・配属編成一覧 / 全般・交番検査管理
                </h3>
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4" id="fleetGrid">
                    <!-- Fleet Cards Dynamically Injected -->
                </div>
            </div>
        </div>

        <!-- TAB CONTENT 6: SETTINGS -->
        <div id="view-settings" class="tab-view hidden space-y-4">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <!-- Custom Company Config -->
                <div class="bg-slate-900 border border-slate-800 rounded-xl p-4 space-y-4">
                    <h3 class="text-sm font-bold text-cyan-400 border-b border-slate-800 pb-2">
                        <i class="fa-solid fa-pen-to-square"></i> 鉄道会社・路線カスタマイズ
                    </h3>
                    <div class="space-y-3 text-xs">
                        <div>
                            <label class="text-slate-400 block mb-1">鉄道会社名</label>
                            <input id="settingCompanyName" type="text" class="w-full bg-slate-950 border border-slate-700 rounded p-2 text-slate-200" value="未来都市高速鉄道">
                        </div>
                        <div>
                            <label class="text-slate-400 block mb-1">路線名</label>
                            <input id="settingLineName" type="text" class="w-full bg-slate-950 border border-slate-700 rounded p-2 text-slate-200" value="中央本線">
                        </div>
                        <div>
                            <label class="text-slate-400 block mb-1">ラインカラー (HEX)</label>
                            <div class="flex gap-2">
                                <input id="settingLineColor" type="color" class="h-8 w-12 bg-slate-950 border border-slate-700 rounded cursor-pointer" value="#06b6d4">
                                <input id="settingLineColorText" type="text" class="flex-1 bg-slate-950 border border-slate-700 rounded p-2 text-slate-200 font-mono" value="#06b6d4">
                            </div>
                        </div>
                        <button onclick="saveLineSettings()" class="w-full bg-cyan-600 hover:bg-cyan-500 text-white font-bold py-2 rounded transition">
                            設定変更を適用・保存
                        </button>
                    </div>
                </div>

                <!-- Station Management -->
                <div class="bg-slate-900 border border-slate-800 rounded-xl p-4 space-y-4">
                    <h3 class="text-sm font-bold text-cyan-400 border-b border-slate-800 pb-2">
                        <i class="fa-solid fa-route"></i> 設置駅管理
                    </h3>
                    <div id="stationListEditor" class="space-y-2 max-h-[250px] overflow-y-auto pr-1">
                        <!-- Station list editor UI -->
                    </div>
                    <button onclick="saveStationList()" class="w-full bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 font-bold text-xs py-2 rounded transition">
                        駅一覧の変更を更新
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Add Train Modal -->
    <div id="addTrainModal" class="hidden fixed inset-0 bg-black/70 backdrop-blur-sm z-50 flex items-center justify-center p-4">
        <div class="bg-slate-900 border border-slate-800 rounded-xl max-w-md w-full p-5 space-y-4 text-xs">
            <h3 class="text-sm font-bold text-white flex items-center gap-2 border-b border-slate-800 pb-2">
                <i class="fa-solid fa-train text-cyan-400"></i> 新設・臨時列車設定
            </h3>
            <div class="space-y-3">
                <div>
                    <label class="text-slate-400 block mb-1">列車番号 (例: 1021M)</label>
                    <input id="modalTrainCode" type="text" class="w-full bg-slate-950 border border-slate-700 rounded p-2 text-slate-200 font-mono" value="1050M">
                </div>
                <div>
                    <label class="text-slate-400 block mb-1">列車種別</label>
                    <select id="modalTrainType" class="w-full bg-slate-950 border border-slate-700 rounded p-2 text-slate-200">
                        <option value="Local">普通 (Local)</option>
                        <option value="Rapid">快速 (Rapid)</option>
                        <option value="Express">急行 (Express)</option>
                        <option value="Limited">特急 (Limited Express)</option>
                    </select>
                </div>
                <div>
                    <label class="text-slate-400 block mb-1">進行方向</label>
                    <select id="modalTrainDir" class="w-full bg-slate-950 border border-slate-700 rounded p-2 text-slate-200">
                        <option value="down">下り (起点 → 終点)</option>
                        <option value="up">上り (終点 → 起点)</option>
                    </select>
                </div>
                <div>
                    <label class="text-slate-400 block mb-1">運用車両形式</label>
                    <input id="modalTrainModel" type="text" class="w-full bg-slate-950 border border-slate-700 rounded p-2 text-slate-200" value="E235系 10両編成">
                </div>
            </div>
            <div class="flex items-center justify-end gap-2 border-t border-slate-800 pt-3">
                <button onclick="closeModal('addTrainModal')" class="px-4 py-2 bg-slate-800 text-slate-300 rounded font-semibold">キャンセル</button>
                <button onclick="confirmAddTrain()" class="px-4 py-2 bg-cyan-600 hover:bg-cyan-500 text-white rounded font-bold">新設決定</button>
            </div>
        </div>
    </div>

    <!-- Footer -->
    <footer class="bg-slate-950 border-t border-slate-900 py-3 px-4 text-center text-xs text-slate-600">
        クラウド対応 オリジナル鉄道 総合運行管理システム &copy; 2026 - Real-time Cloud Firestore Synchronized System
    </footer>

    <!-- Application Logic Javascript -->
    <script>
        /* State & Default Data Architecture */
        let state = {
            companyName: "未来都市高速鉄道",
            lineName: "中央本線",
            lineColor: "#06b6d4",
            emergencyStop: false,
            speedRestriction: 100,
            dispatchNotice: "",
            stations: [
                { id: "st1", name: "中央ターミナル", pos: 50 },
                { id: "st2", name: "新都心公園", pos: 200 },
                { id: "st3", name: "学園都市前", pos: 380 },
                { id: "st4", name: "西ハイランド", pos: 550 },
                { id: "st5", name: "未来国際空港", pos: 750 }
            ],
            trains: [
                { id: "tr1", code: "1011M", type: "Local", dir: "down", pos: 80, delay: 0, model: "E233系 8両", status: "RUNNING" },
                { id: "tr2", code: "2014M", type: "Rapid", dir: "up", pos: 600, delay: 0, model: "E235系 10両", status: "RUNNING" },
                { id: "tr3", code: "3001X", type: "Limited", dir: "down", pos: 300, delay: 2, model: "E353系 12両", status: "RUNNING" }
            ],
            fleet: [
                { id: "fl1", name: "E233系 T-01編成", length: "8両", inspectDays: 45, dist: "124,500 km", status: "運用中" },
                { id: "fl2", name: "E235系 F-12編成", length: "10両", inspectDays: 120, dist: "89,120 km", status: "運用中" },
                { id: "fl3", name: "E353系 S-105編成", length: "12両", inspectDays: 12, dist: "310,200 km", status: "要検査" }
            ]
        };

        let isCloudConnected = false;
        let unsubscribeFirestore = null;

        /* Clock Updater */
        setInterval(() => {
            const now = new Date();
            document.getElementById('occClock').innerText = now.toTimeString().split(' ')[0];
        }, 1000);

        /* Cloud Sync Setup */
        window.initCloudSync = function() {
            if (!window.db) {
                updateSyncStatus(false, "オフライン (ローカル動作)");
                return;
            }

            try {
                const docRef = doc(window.db, "artifacts", "original-tetsudo", "public", "data", "railwayState", "current");
                
                // Realtime Listener
                unsubscribeFirestore = onSnapshot(docRef, (docSnap) => {
                    if (docSnap.exists()) {
                        const data = docSnap.data();
                        state = { ...state, ...data };
                        updateSyncStatus(true, "クラウドリアルタイム同期中");
                    } else {
                        // First time save initial data
                        saveStateToCloud();
                        updateSyncStatus(true, "クラウド初期化完了");
                    }
                    renderAllViews();
                }, (error) => {
                    console.warn("Firestore access warning:", error);
                    updateSyncStatus(false, "ローカル動作中");
                });
            } catch(e) {
                console.error(e);
                updateSyncStatus(false, "ローカル動作中");
            }
        };

        function updateSyncStatus(connected, text) {
            isCloudConnected = connected;
            const badge = document.getElementById('syncBadge');
            const dot = document.getElementById('syncDot');
            const ping = document.getElementById('syncPing');
            const textEl = document.getElementById('syncText');

            textEl.innerText = text;
            if (connected) {
                badge.className = "flex items-center gap-2 px-3 py-1 rounded-full text-xs bg-emerald-500/10 border border-emerald-500/30 text-emerald-400";
                dot.className = "relative inline-flex rounded-full h-2 w-2 bg-emerald-500";
                ping.className = "animate-ping absolute inline-flex h-full w-full rounded-full bg-emerald-400 opacity-75";
            } else {
                badge.className = "flex items-center gap-2 px-3 py-1 rounded-full text-xs bg-amber-500/10 border border-amber-500/30 text-amber-400";
                dot.className = "relative inline-flex rounded-full h-2 w-2 bg-amber-500";
                ping.className = "animate-ping absolute inline-flex h-full w-full rounded-full bg-amber-400 opacity-75";
            }
        }

        async function saveStateToCloud() {
            if (!window.db) return;
            try {
                const docRef = doc(window.db, "artifacts", "original-tetsudo", "public", "data", "railwayState", "current");
                await setDoc(docRef, state, { merge: true });
            } catch (e) {
                console.warn("Could not save to cloud:", e);
            }
        }

        /* Simulation Movement Loop */
        setInterval(() => {
            if (state.emergencyStop) return; // Freeze train movement on emergency

            let changed = false;
            const maxPos = state.stations[state.stations.length - 1].pos;
            const minPos = state.stations[0].pos;

            state.trains.forEach(t => {
                if (t.status === "STOPPED_MANUAL") return;

                const speed = (state.speedRestriction / 100) * 1.5;
                if (t.dir === "down") {
                    t.pos += speed;
                    if (t.pos >= maxPos) { t.pos = minPos; }
                } else {
                    t.pos -= speed;
                    if (t.pos <= minPos) { t.pos = maxPos; }
                }
            });

            renderTrackMap();
            renderDiagram();
        }, 300);

        /* Tab Switcher */
        function switchTab(tabId) {
            document.querySelectorAll('.tab-view').forEach(el => el.classList.add('hidden'));
            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('bg-cyan-600', 'text-white');
                btn.classList.add('text-slate-400');
            });

            document.getElementById(`view-${tabId}`).classList.remove('hidden');
            const activeBtn = document.getElementById(`tab-${tabId}`);
            activeBtn.classList.add('bg-cyan-600', 'text-white');
            activeBtn.classList.remove('text-slate-400');

            if (tabId === 'diagram') renderDiagram();
            if (tabId === 'timetable') renderTimetable();
        }

        /* SVG Interactive Track Map Renderer */
        function renderTrackMap() {
            const svg = document.getElementById('railwaySvg');
            if (!svg) return;
            svg.innerHTML = '';

            const stations = state.stations;
            if (!stations || stations.length === 0) return;

            const startX = 60;
            const endX = 740;
            const trackYDown = 120;
            const trackYUp = 220;

            // Scale positioning
            const minP = stations[0].pos;
            const maxP = stations[stations.length - 1].pos;
            const getX = (pos) => startX + ((pos - minP) / (maxP - minP)) * (endX - startX);

            // Draw Track Lines (Double Track)
            const trackDown = document.createElementNS("http://www.w3.org/2000/svg", "line");
            trackDown.setAttribute("x1", startX); trackDown.setAttribute("y1", trackYDown);
            trackDown.setAttribute("x2", endX); trackDown.setAttribute("y2", trackYDown);
            trackDown.setAttribute("stroke", "#334155"); trackDown.setAttribute("stroke-width", "6");
            svg.appendChild(trackDown);

            const trackUp = document.createElementNS("http://www.w3.org/2000/svg", "line");
            trackUp.setAttribute("x1", startX); trackUp.setAttribute("y1", trackYUp);
            trackUp.setAttribute("x2", endX); trackUp.setAttribute("y2", trackYUp);
            trackUp.setAttribute("stroke", "#334155"); trackUp.setAttribute("stroke-width", "6");
            svg.appendChild(trackUp);

            // Draw Station Points
            stations.forEach((st, idx) => {
                const sx = getX(st.pos);

                // Station Line Connector
                const conn = document.createElementNS("http://www.w3.org/2000/svg", "line");
                conn.setAttribute("x1", sx); conn.setAttribute("y1", trackYDown - 20);
                conn.setAttribute("x2", sx); conn.setAttribute("y2", trackYUp + 20);
                conn.setAttribute("stroke", "#1e293b"); conn.setAttribute("stroke-width", "2");
                conn.setAttribute("stroke-dasharray", "4");
                svg.appendChild(conn);

                // Station Dots
                [trackYDown, trackYUp].forEach(ty => {
                    const circle = document.createElementNS("http://www.w3.org/2000/svg", "circle");
                    circle.setAttribute("cx", sx); circle.setAttribute("cy", ty);
                    circle.setAttribute("r", "7");
                    circle.setAttribute("fill", "#0b0f19");
                    circle.setAttribute("stroke", state.lineColor);
                    circle.setAttribute("stroke-width", "3");
                    svg.appendChild(circle);
                });

                // Station Name Label
                const text = document.createElementNS("http://www.w3.org/2000/svg", "text");
                text.setAttribute("x", sx); text.setAttribute("y", trackYDown - 30);
                text.setAttribute("fill", "#94a3b8");
                text.setAttribute("font-size", "11");
                text.setAttribute("font-weight", "bold");
                text.setAttribute("text-anchor", "middle");
                text.textContent = st.name;
                svg.appendChild(text);
            });

            // Draw Trains
            state.trains.forEach(t => {
                const tx = getX(t.pos);
                const ty = t.dir === "down" ? trackYDown : trackYUp;

                // Color based on type
                let color = "#06b6d4"; // Local
                if (t.type === "Rapid") color = "#10b981";
                if (t.type === "Express") color = "#f59e0b";
                if (t.type === "Limited") color = "#ef4444";

                // Group
                const g = document.createElementNS("http://www.w3.org/2000/svg", "g");

                // Train Box
                const rect = document.createElementNS("http://www.w3.org/2000/svg", "rect");
                rect.setAttribute("x", tx - 22); rect.setAttribute("y", ty - 10);
                rect.setAttribute("width", "44"); rect.setAttribute("height", "20");
                rect.setAttribute("rx", "4");
                rect.setAttribute("fill", color);
                rect.setAttribute("stroke", "#ffffff");
                rect.setAttribute("stroke-width", "1.5");
                g.appendChild(rect);

                // Train Code Text
                const txt = document.createElementNS("http://www.w3.org/2000/svg", "text");
                txt.setAttribute("x", tx); txt.setAttribute("y", ty + 3);
                txt.setAttribute("fill", "#ffffff");
                txt.setAttribute("font-size", "9");
                txt.setAttribute("font-weight", "bold");
                txt.setAttribute("text-anchor", "middle");
                txt.textContent = t.code;
                g.appendChild(txt);

                svg.appendChild(g);
            });
        }

        /* Render Train Status Grid */
        function renderTrainStatusGrid() {
            const container = document.getElementById('trainStatusGrid');
            if (!container) return;
            container.innerHTML = '';

            state.trains.forEach(t => {
                const card = document.createElement('div');
                card.className = "bg-slate-900 border border-slate-800 rounded-lg p-3 text-xs space-y-1.5";
                
                let badgeColor = "bg-cyan-500/20 text-cyan-400 border-cyan-500/30";
                if (t.type === "Rapid") badgeColor = "bg-emerald-500/20 text-emerald-400 border-emerald-500/30";
                if (t.type === "Express") badgeColor = "bg-amber-500/20 text-amber-400 border-amber-500/30";
                if (t.type === "Limited") badgeColor = "bg-red-500/20 text-red-400 border-red-500/30";

                card.innerHTML = `
                    <div class="flex items-center justify-between">
                        <span class="font-bold font-mono text-white">${t.code}</span>
                        <span class="px-2 py-0.5 rounded border ${badgeColor} font-semibold">${t.type}</span>
                    </div>
                    <div class="text-slate-400 flex justify-between">
                        <span>運用形式:</span>
                        <span class="text-slate-200">${t.model}</span>
                    </div>
                    <div class="text-slate-400 flex justify-between">
                        <span>方向 / 遅延:</span>
                        <span class="${t.delay > 0 ? 'text-amber-400 font-bold' : 'text-slate-200'}">
                            ${t.dir === 'down' ? '下り' : '上り'} / ${t.delay > 0 ? '+' + t.delay + '分' : '定時'}
                        </span>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        /* Render Dispatch Table */
        function renderDispatchTable() {
            const tbody = document.getElementById('dispatchTableBody');
            if (!tbody) return;
            tbody.innerHTML = '';

            state.trains.forEach(t => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-800/50 transition";
                tr.innerHTML = `
                    <td class="p-2 font-mono font-bold text-cyan-400">${t.code}</td>
                    <td class="p-2">${t.type}</td>
                    <td class="p-2 font-mono">${Math.round(t.pos)}km地点</td>
                    <td class="p-2">${t.delay > 0 ? `<span class="text-amber-400 font-bold">+${t.delay}分</span>` : '<span class="text-emerald-400">定時</span>'}</td>
                    <td class="p-2">${t.status === 'STOPPED_MANUAL' ? '<span class="text-red-400 font-bold">抑止中</span>' : '<span class="text-emerald-400">進行中</span>'}</td>
                    <td class="p-2 text-right space-x-1">
                        <button onclick="toggleTrainHold('${t.id}')" class="px-2 py-1 bg-slate-800 hover:bg-slate-700 text-slate-200 rounded text-[10px] font-semibold">
                            ${t.status === 'STOPPED_MANUAL' ? '抑止解除' : '手動抑止'}
                        </button>
                        <button onclick="addTrainDelay('${t.id}', 3)" class="px-2 py-1 bg-amber-600/30 hover:bg-amber-600/50 text-amber-300 rounded text-[10px]">
                            +3分遅延
                        </button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        /* Graphical Train Diagram Canvas Engine */
        function renderDiagram() {
            const canvas = document.getElementById('diagramCanvas');
            if (!canvas) return;
            const ctx = canvas.getContext('2d');

            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight;

            const w = canvas.width;
            const h = canvas.height;

            // Background
            ctx.fillStyle = '#020617';
            ctx.fillRect(0, 0, w, h);

            // Draw Station Horizontal Lines
            const marginY = 40;
            const stations = state.stations;
            if (stations.length === 0) return;

            const minP = stations[0].pos;
            const maxP = stations[stations.length - 1].pos;

            stations.forEach(st => {
                const sy = marginY + ((st.pos - minP) / (maxP - minP)) * (h - marginY * 2);
                
                ctx.strokeStyle = '#1e293b';
                ctx.lineWidth = 1;
                ctx.beginPath();
                ctx.moveTo(80, sy);
                ctx.lineTo(w - 20, sy);
                ctx.stroke();

                ctx.fillStyle = '#94a3b8';
                ctx.font = '10px Inter';
                ctx.textAlign = 'right';
                ctx.fillText(st.name, 70, sy + 3);
            });

            // Draw Live Train Positions on Plot
            state.trains.forEach(t => {
                const sy = marginY + ((t.pos - minP) / (maxP - minP)) * (h - marginY * 2);
                
                ctx.fillStyle = t.type === 'Limited' ? '#ef4444' : '#06b6d4';
                ctx.beginPath();
                ctx.arc(w / 2, sy, 5, 0, Math.PI * 2);
                ctx.fill();

                ctx.fillStyle = '#ffffff';
                ctx.font = 'bold 9px JetBrains Mono';
                ctx.textAlign = 'left';
                ctx.fillText(`${t.code} (${t.dir})`, w / 2 + 8, sy + 3);
            });
        }

        /* Render Station Timetables */
        function renderTimetable() {
            const select = document.getElementById('timetableStationSelect');
            if (!select) return;

            // Populate selector if empty
            if (select.children.length === 0) {
                select.innerHTML = state.stations.map(s => `<option value="${s.id}">${s.name}</option>`).join('');
            }

            const downBox = document.getElementById('timetableDown');
            const upBox = document.getElementById('timetableUp');

            downBox.innerHTML = `
                <div class="flex justify-between py-1 border-b border-slate-900"><span class="text-cyan-400 font-bold">07:12</span> <span>1011M 普通 (未来国際空港 行)</span></div>
                <div class="flex justify-between py-1 border-b border-slate-900"><span class="text-emerald-400 font-bold">07:25</span> <span>2015M 快速 (未来国際空港 行)</span></div>
                <div class="flex justify-between py-1 border-b border-slate-900"><span class="text-red-400 font-bold">07:40</span> <span>3001X 特急 (未来国際空港 行)</span></div>
            `;

            upBox.innerHTML = `
                <div class="flex justify-between py-1 border-b border-slate-900"><span class="text-cyan-400 font-bold">07:05</span> <span>1012M 普通 (中央ターミナル 行)</span></div>
                <div class="flex justify-between py-1 border-b border-slate-900"><span class="text-amber-400 font-bold">07:18</span> <span>2014M 急行 (中央ターミナル 行)</span></div>
            `;
        }

        /* Render Depot & Fleet */
        function renderDepot() {
            const grid = document.getElementById('fleetGrid');
            if (!grid) return;
            grid.innerHTML = '';

            state.fleet.forEach(f => {
                const card = document.createElement('div');
                card.className = "bg-slate-950 border border-slate-800 rounded-lg p-3 text-xs space-y-2";
                card.innerHTML = `
                    <div class="flex justify-between items-center border-b border-slate-900 pb-2">
                        <span class="font-bold text-white text-sm">${f.name}</span>
                        <span class="px-2 py-0.5 rounded ${f.status === '運用中' ? 'bg-emerald-500/20 text-emerald-400' : 'bg-red-500/20 text-red-400'} font-semibold">${f.status}</span>
                    </div>
                    <div class="flex justify-between text-slate-400"><span>両数編成:</span> <span class="text-slate-200">${f.length}</span></div>
                    <div class="flex justify-between text-slate-400"><span>次回検査まで:</span> <span class="text-slate-200">${f.inspectDays}日</span></div>
                    <div class="flex justify-between text-slate-400"><span>累計走行距離:</span> <span class="text-slate-200 font-mono">${f.dist}</span></div>
                `;
                grid.appendChild(card);
            });
        }

        /* Render Settings View */
        function renderSettings() {
            document.getElementById('settingCompanyName').value = state.companyName;
            document.getElementById('settingLineName').value = state.lineName;
            document.getElementById('settingLineColor').value = state.lineColor;
            document.getElementById('settingLineColorText').value = state.lineColor;

            const editor = document.getElementById('stationListEditor');
            editor.innerHTML = state.stations.map((st, i) => `
                <div class="flex items-center gap-2">
                    <span class="text-slate-500 font-mono w-4">${i+1}</span>
                    <input type="text" value="${st.name}" class="st-name-input flex-1 bg-slate-950 border border-slate-700 rounded p-1.5 text-xs text-slate-200">
                    <input type="number" value="${st.pos}" class="st-pos-input w-20 bg-slate-950 border border-slate-700 rounded p-1.5 text-xs text-slate-200 font-mono" placeholder="km">
                </div>
            `).join('');
        }

        /* Actions & Handlers */
        function toggleEmergencyStop(active) {
            state.emergencyStop = active;
            document.getElementById('emergencyBanner').classList.toggle('hidden', !active);
            saveStateToCloud();
            renderAllViews();
        }

        function setGlobalDelay(mins) {
            state.trains.forEach(t => t.delay += mins);
            saveStateToCloud();
            renderAllViews();
        }

        function clearAllDelays() {
            state.trains.forEach(t => t.delay = 0);
            saveStateToCloud();
            renderAllViews();
        }

        function applySpeedRestriction() {
            const val = parseInt(document.getElementById('speedLimitSelect').value);
            state.speedRestriction = val;
            saveStateToCloud();
            alert(`全線最高速度制限を ${val}km/h に設定しました。`);
        }

        function postDispatchNotice() {
            const text = document.getElementById('dispatchNoticeInput').value;
            state.dispatchNotice = text;
            saveStateToCloud();
            alert("全卓へ指示を放送送信しました。");
        }

        function toggleTrainHold(trainId) {
            const t = state.trains.find(x => x.id === trainId);
            if (t) {
                t.status = t.status === 'STOPPED_MANUAL' ? 'RUNNING' : 'STOPPED_MANUAL';
                saveStateToCloud();
                renderAllViews();
            }
        }

        function addTrainDelay(trainId, mins) {
            const t = state.trains.find(x => x.id === trainId);
            if (t) {
                t.delay += mins;
                saveStateToCloud();
                renderAllViews();
            }
        }

        function saveLineSettings() {
            state.companyName = document.getElementById('settingCompanyName').value;
            state.lineName = document.getElementById('settingLineName').value;
            state.lineColor = document.getElementById('settingLineColor').value;
            saveStateToCloud();
            renderAllViews();
            alert("路線設定を反映・クラウド同期しました。");
        }

        function saveStationList() {
            const names = document.querySelectorAll('.st-name-input');
            const poses = document.querySelectorAll('.st-pos-input');
            
            const newStations = [];
            names.forEach((el, idx) => {
                newStations.push({
                    id: `st${idx+1}`,
                    name: el.value,
                    pos: parseFloat(poses[idx].value) || (idx * 100)
                });
            });

            state.stations = newStations;
            saveStateToCloud();
            renderAllViews();
            alert("駅情報を更新しました。");
        }

        function addNewTrainModal() {
            document.getElementById('addTrainModal').classList.remove('hidden');
        }

        function closeModal(id) {
            document.getElementById(id).classList.add('hidden');
        }

        function confirmAddTrain() {
            const code = document.getElementById('modalTrainCode').value;
            const type = document.getElementById('modalTrainType').value;
            const dir = document.getElementById('modalTrainDir').value;
            const model = document.getElementById('modalTrainModel').value;

            state.trains.push({
                id: `tr_${Date.now()}`,
                code: code,
                type: type,
                dir: dir,
                pos: dir === 'down' ? state.stations[0].pos : state.stations[state.stations.length - 1].pos,
                delay: 0,
                model: model,
                status: 'RUNNING'
            });

            saveStateToCloud();
            closeModal('addTrainModal');
            renderAllViews();
        }

        /* Render All Master View Coordinator */
        function renderAllViews() {
            document.getElementById('companyNameDisplay').innerText = state.companyName;
            document.getElementById('lineNameDisplay').innerText = state.lineName;

            renderTrackMap();
            renderTrainStatusGrid();
            renderDispatchTable();
            renderDepot();
            renderSettings();
        }

        /* Initial Execution on Load */
        window.addEventListener('DOMContentLoaded', () => {
            renderAllViews();
        });
    </script>
</body>
</html>
