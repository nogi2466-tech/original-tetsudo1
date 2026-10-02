<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>紫句守鉄道（しのもり鉄道）総合運行管理システム</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Firebase SDK (Compat) -->
    <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>
    <style>
        body { font-family: system-ui, -apple-system, sans-serif; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen flex flex-col">

    <!-- ヘッダー -->
    <header class="bg-indigo-950 border-b border-indigo-800 p-4 shadow-lg flex flex-wrap justify-between items-center gap-4 relative">
        <div class="flex items-center space-x-3">
            <span class="text-3xl">🚄</span>
            <div>
                <h1 class="text-xl font-bold tracking-wider text-indigo-200">紫句守鉄道 <span class="text-xs font-normal text-indigo-400">しのもり鉄道 - Shinomori Railway</span></h1>
                <p class="text-xs text-slate-400">総合運行管理・経営シミュレーター</p>
            </div>
        </div>

        <div class="flex items-center gap-3">
            <!-- 現在日時表示 -->
            <div id="live-datetime" class="bg-slate-900 px-3 py-1.5 rounded-lg border border-slate-700 text-xs font-mono text-indigo-300">
                2026/10/03(土) 00:00:00
            </div>

            <!-- クラウド同期ステータス -->
            <div class="hidden sm:flex items-center space-x-2 bg-slate-900 px-3 py-1.5 rounded-lg border border-slate-700 text-xs">
                <span id="sync-status-dot" class="w-2.5 h-2.5 rounded-full bg-amber-500 animate-pulse"></span>
                <span id="sync-status-text">クラウド同期: 接続待機中...</span>
            </div>

            <!-- ハンバーガーメニューボタン（三本線） -->
            <button onclick="toggleMenu()" class="bg-indigo-900 hover:bg-indigo-800 border border-indigo-700 p-2 rounded-lg text-white md:hidden transition flex items-center justify-center w-10 h-10 shadow">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path>
                </svg>
            </button>
        </div>
    </header>

    <!-- ナビゲーションタブ（デスクトップ常時表示 ＆ スマホはハンバーガーで開閉） -->
    <nav id="nav-menu" class="hidden md:flex bg-slate-900/95 border-b border-slate-800 px-4 py-2 overflow-x-auto space-x-1 sticky top-0 z-50 backdrop-blur shadow-md">
        <button onclick="switchTab('about')" class="tab-btn px-4 py-2 rounded-lg text-sm font-medium transition bg-indigo-600 text-white shadow" data-tab="about">会社について</button>
        <button onclick="switchTab('timetable')" class="tab-btn px-4 py-2 rounded-lg text-sm font-medium transition text-slate-400 hover:text-white hover:bg-slate-800" data-tab="timetable">時刻表</button>
        <button onclick="switchTab('operation')" class="tab-btn px-4 py-2 rounded-lg text-sm font-medium transition text-slate-400 hover:text-white hover:bg-slate-800" data-tab="operation">走行位置</button>
        <button onclick="switchTab('traininfo')" class="tab-btn px-4 py-2 rounded-lg text-sm font-medium transition text-slate-400 hover:text-white hover:bg-slate-800" data-tab="traininfo">列車情報</button>
        <button onclick="switchTab('addtrain')" class="tab-btn px-4 py-2 rounded-lg text-sm font-medium transition text-slate-400 hover:text-white hover:bg-slate-800" data-tab="addtrain">列車追加</button>
        <button onclick="switchTab('consist')" class="tab-btn px-4 py-2 rounded-lg text-sm font-medium transition text-slate-400 hover:text-white hover:bg-slate-800" data-tab="consist">編成表</button>
        <button onclick="switchTab('dia')" class="tab-btn px-4 py-2 rounded-lg text-sm font-medium transition text-slate-400 hover:text-white hover:bg-slate-800" data-tab="dia">ダイヤ表</button>
        <button onclick="switchTab('settings')" class="tab-btn px-4 py-2 rounded-lg text-sm font-medium transition text-slate-400 hover:text-white hover:bg-slate-800" data-tab="settings">設定</button>
    </nav>

    <!-- メインコンテンツ -->
    <main class="flex-1 p-4 md:p-6 max-w-7xl mx-auto w-full">

        <!-- 1. 会社について -->
        <div id="tab-about" class="tab-content active space-y-6">
            <div class="bg-gradient-to-r from-indigo-900 to-slate-800 p-6 rounded-2xl border border-indigo-700/50 shadow-xl space-y-3">
                <h2 class="text-2xl font-bold text-indigo-100">紫句守鉄道株式会社 <span class="text-sm font-normal text-indigo-300">Shinomori Railway Co., Ltd.</span></h2>
                <p class="text-slate-300 text-sm leading-relaxed">
                    紫句守鉄道は、首都圏と豊かな自然に恵まれた紫句守・星句高原エリアを結ぶ主要幹線を運行する鉄道会社です。「安全・信頼・快適」を経営の基本方針に掲げ、地域社会の発展と観光需要の活性化に貢献しています。
                </p>
            </div>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="bg-slate-800 p-5 rounded-xl border border-slate-700 space-y-3">
                    <h3 class="text-indigo-400 font-bold border-b border-slate-700 pb-2">企業概要</h3>
                    <ul class="text-sm text-slate-300 space-y-2">
                        <li><span class="text-slate-400 inline-block w-28">社名</span> 紫句守鉄道株式会社</li>
                        <li><span class="text-slate-400 inline-block w-28">設立</span> 1965年4月1日</li>
                        <li><span class="text-slate-400 inline-block w-28">本社所在地</span> 陽光県紫句守市中央一丁目1番地</li>
                        <li><span class="text-slate-400 inline-block w-28">代表取締役社長</span> 紫野 太郎</li>
                        <li><span class="text-slate-400 inline-block w-28">事業内容</span> 鉄道事業、観光開発事業、不動産事業</li>
                    </ul>
                </div>
                
                <div class="bg-slate-800 p-5 rounded-xl border border-slate-700 space-y-3">
                    <h3 class="text-indigo-400 font-bold border-b border-slate-700 pb-2">路線データ</h3>
                    <ul class="text-sm text-slate-300 space-y-2">
                        <li><span class="text-slate-400 inline-block w-28">営業路線数</span> 4路線（全60駅）</li>
                        <li><span class="text-slate-400 inline-block w-28">総営業キロ</span> 142.5 km</li>
                        <li><span class="text-slate-400 inline-block w-28">最高速度</span> 130 km/h（特急S100系）</li>
                        <li><span class="text-slate-400 inline-block w-28">保安方式</span> ATS-P / デジタル列車制御装置</li>
                    </ul>
                </div>
            </div>

            <div class="bg-slate-800 p-5 rounded-xl border border-slate-700 space-y-3">
                <h3 class="text-indigo-400 font-bold">沿革</h3>
                <div class="grid grid-cols-1 md:grid-cols-4 gap-4 text-sm text-slate-300">
                    <div class="bg-slate-900 p-3 rounded border border-slate-700">
                        <span class="text-indigo-400 font-bold block mb-1">1965年</span>
                        紫句守鉄道設立。紫雲本線（中央〜紫雲野間）開業。
                    </div>
                    <div class="bg-slate-900 p-3 rounded border border-slate-700">
                        <span class="text-indigo-400 font-bold block mb-1">1982年</span>
                        全線電化完了。句守支線および星句高原線が開業。
                    </div>
                    <div class="bg-slate-900 p-3 rounded border border-slate-700">
                        <span class="text-indigo-400 font-bold block mb-1">2005年</span>
                        新型特急S100系導入により、都心〜高原間の所要時間を大幅短縮。
                    </div>
                    <div class="bg-slate-900 p-3 rounded border border-slate-700">
                        <span class="text-indigo-400 font-bold block mb-1">2026年</span>
                        次世代クラウド運行管理システムを全線に導入し、リアルタイム運行監視を実現。
                    </div>
                </div>
            </div>
        </div>

        <!-- 2. 時刻表 -->
        <div id="tab-timetable" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">インタラクティブ時刻表</h2>
            <div class="bg-slate-800 p-4 rounded-xl border border-slate-700 grid grid-cols-1 sm:grid-cols-4 gap-4 items-end">
                <div>
                    <label class="block text-xs text-slate-400 mb-1">駅選択</label>
                    <select id="tt-station" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-sm text-white">
                        <!-- 動的生成 -->
                    </select>
                </div>
                <div>
                    <label class="block text-xs text-slate-400 mb-1">方向</label>
                    <select id="tt-dir" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-sm text-white">
                        <option value="down">下り (起点 → 終点方面)</option>
                        <option value="up">上り (終点 → 起点方面)</option>
                    </select>
                </div>
                <div>
                    <label class="block text-xs text-slate-400 mb-1">運行日</label>
                    <select id="tt-day" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-sm text-white">
                        <option value="weekday">平日ダイヤ</option>
                        <option value="holiday">土休日ダイヤ</option>
                    </select>
                </div>
                <button onclick="renderTimetable()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-4 py-2 rounded text-sm font-medium transition shadow">時刻表を表示</button>
            </div>
            <div id="timetable-container" class="bg-slate-800 rounded-xl border border-slate-700 p-4 overflow-x-auto text-sm">
                <p class="text-slate-400">条件を選択して「時刻表を表示」を押してください。</p>
            </div>
        </div>

        <!-- 3. 走行位置（路線ごと切り替え＆全駅縦型マップ） -->
        <div id="tab-operation" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">リアルタイム運行状況マップ（エレサイト風）</h2>
            
            <!-- 路線切り替えタブボタン -->
            <div class="flex flex-wrap gap-2 bg-slate-800 p-3 rounded-xl border border-slate-700">
                <button onclick="switchLine('main')" class="line-btn px-4 py-2 rounded-lg text-xs font-bold transition bg-indigo-600 text-white shadow" data-line="main">紫雲本線 (1〜30)</button>
                <button onclick="switchLine('branch')" class="line-btn px-4 py-2 rounded-lg text-xs font-bold transition bg-slate-900 text-slate-300 hover:bg-slate-700 border border-slate-700" data-line="branch">句守支線 (41〜55)</button>
                <button onclick="switchLine('sight')" class="line-btn px-4 py-2 rounded-lg text-xs font-bold transition bg-slate-900 text-slate-300 hover:bg-slate-700 border border-slate-700" data-line="sight">紫霞観光線 (56〜60)</button>
                <button onclick="switchLine('plateau')" class="line-btn px-4 py-2 rounded-lg text-xs font-bold transition bg-slate-900 text-slate-300 hover:bg-slate-700 border border-slate-700" data-line="plateau">星句高原線 (31〜40)</button>
            </div>

            <!-- 縦型路線ルートマップ -->
            <div class="bg-slate-800 p-6 rounded-xl border border-slate-700 relative overflow-y-auto max-h-[650px] shadow-xl">
                <div id="line-title-banner" class="text-xs text-indigo-300 mb-6 text-center font-bold">紫雲本線 運行モニター（全30駅）</div>
                
                <div class="relative max-w-lg mx-auto py-6">
                    <!-- 中央の縦線 -->
                    <div class="absolute left-1/2 transform -translate-x-1/2 top-0 bottom-0 w-2 bg-gradient-to-b from-indigo-500 via-sky-500 to-indigo-600 rounded-full"></div>

                    <!-- 全駅リストコンテナ -->
                    <div id="route-map-stations" class="space-y-8 relative z-10">
                        <!-- JSで動的生成 -->
                    </div>
                </div>
            </div>
        </div>

        <!-- 4. 列車情報 -->
        <div id="tab-traininfo" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">車両形式図鑑</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4" id="train-info-grid">
                <!-- 動的生成 -->
            </div>
        </div>

        <!-- 5. 列車追加 -->
        <div id="tab-addtrain" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">新規列車運用追加</h2>
            <div class="bg-slate-800 p-6 rounded-xl border border-slate-700 max-w-xl space-y-4">
                <div>
                    <label class="block text-xs text-slate-400 mb-1">列車番号 / 運用名</label>
                    <input type="text" id="add-train-name" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white" value="特急 101M">
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">種別</label>
                        <select id="add-train-type" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                            <option>特急</option>
                            <option>通勤急行</option>
                            <option>急行</option>
                            <option>通勤快速</option>
                            <option>快速</option>
                            <option>準急</option>
                            <option>普通</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">車両形式</label>
                        <select id="add-train-series" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                            <option>S100系 (特急型)</option>
                            <option>S1系</option>
                            <option>S2系</option>
                            <option>S3系</option>
                            <option>S4系</option>
                            <option>S5系</option>
                            <option>S900系 (事業用)</option>
                        </select>
                    </div>
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">配属路線</label>
                        <select id="add-train-line" onchange="updateAddStationDropdown()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                            <option value="main">紫雲本線</option>
                            <option value="branch">句守支線</option>
                            <option value="sight">紫霞観光線</option>
                            <option value="plateau">星句高原線</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">現在駅</label>
                        <select id="add-train-station-id" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                            <!-- 動的生成 -->
                        </select>
                    </div>
                </div>
                <button onclick="addNewTrain()" class="w-full bg-indigo-600 hover:bg-indigo-500 text-white font-medium py-2 rounded-lg transition shadow">運行リストに追加 (クラウド自動同期)</button>
            </div>
        </div>

        <!-- 6. 編成表 -->
        <div id="tab-consist" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">編成表（連結構成）</h2>
            <div id="consist-list" class="space-y-3">
                <!-- 動的生成 -->
            </div>
        </div>

        <!-- 7. ダイヤ表 -->
        <div id="tab-dia" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">ダイヤグラム（スジ引き）</h2>
            <div class="bg-slate-800 p-4 rounded-xl border border-slate-700 overflow-x-auto">
                <canvas id="diag-canvas" width="1000" height="600" class="bg-slate-900 rounded border border-slate-800"></canvas>
            </div>
        </div>

        <!-- 8. 設定 -->
        <div id="tab-settings" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">システム設定</h2>
            <div class="bg-slate-800 p-6 rounded-xl border border-slate-700 max-w-xl space-y-4">
                <p class="text-xs text-slate-300">
                    Firebase Realtime Database によるマルチデバイス間のリアルタイム自動同期が有効になっています。
                </p>
                <div>
                    <label class="block text-xs text-slate-400 mb-1">接続中 Firebase Database URL</label>
                    <input type="text" id="fb-url" readonly class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-slate-400 cursor-not-allowed">
                </div>
            </div>
        </div>

    </main>

    <script>
        // 全路線の全駅データマスター
        const linesData = {
            main: {
                name: "紫雲本線",
                stations: Array.from({length: 30}, (_, i) => ({
                    id: String(i+1).padStart(2, '0'),
                    name: i === 0 ? "紫句守中央" : i === 2 ? "紫雲野" : i === 12 ? "紫霞野" : i === 29 ? "紫句守展示場" : `紫雲本線-${i+1}号駅`,
                    facilities: i === 0 ? ["特急停車", "始発"] : i === 29 ? ["終点"] : ["普通停車"]
                }))
            },
            branch: {
                name: "句守支線",
                stations: Array.from({length: 15}, (_, i) => {
                    const stNum = 41 + i;
                    return {
                        id: String(stNum),
                        name: i === 0 ? "句守温泉" : i === 14 ? "奥句守" : `句守支線-${stNum}`,
                        facilities: ["支線運用"]
                    };
                })
            },
            sight: {
                name: "紫霞観光線",
                stations: Array.from({length: 5}, (_, i) => {
                    const stNum = 56 + i;
                    return {
                        id: String(stNum),
                        name: i === 0 ? "紫霞湖畔" : i === 4 ? "展望台" : `観光線-${stNum}`,
                        facilities: ["観光特急"]
                    };
                })
            },
            plateau: {
                name: "星句高原線",
                stations: Array.from({length: 10}, (_, i) => {
                    const stNum = 31 + i;
                    return {
                        id: String(stNum),
                        name: i === 0 ? "星句口" : i === 9 ? "高原リゾート" : `高原線-${stNum}`,
                        facilities: ["山岳対応"]
                    };
                })
            }
        };

        let currentActiveLine = 'main';

        // スマホ用ハンバーガーメニュー開閉
        function toggleMenu() {
            const menu = document.getElementById('nav-menu');
            menu.classList.toggle('hidden');
            menu.classList.toggle('flex');
            menu.classList.toggle('flex-col');
            menu.classList.toggle('absolute');
            menu.classList.toggle('top-16');
            menu.classList.toggle('left-0');
            menu.classList.toggle('right-0');
            menu.classList.toggle('bg-slate-950');
            menu.classList.toggle('p-4');
            menu.classList.toggle('shadow-2xl');
        }

        // タブ切り替え
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('bg-indigo-600', 'text-white', 'shadow');
                btn.classList.add('text-slate-400', 'hover:text-white', 'hover:bg-slate-800');
            });
            document.getElementById('tab-' + tabId).classList.add('active');
            const targetBtn = document.querySelector(`[data-tab="${tabId}"]`);
            if (targetBtn) {
                targetBtn.classList.add('bg-indigo-600', 'text-white', 'shadow');
                targetBtn.classList.remove('text-slate-400', 'hover:text-white', 'hover:bg-slate-800');
            }
            // スマホメニューが開いていれば閉じる
            const menu = document.getElementById('nav-menu');
            if(menu.classList.contains('absolute')) {
                toggleMenu();
            }

            if (tabId === 'dia') drawDiagram();
            if (tabId === 'operation') renderRouteMap();
            if (tabId === 'timetable') initTimetableDropdowns();
        }

        // 路線切り替え（走行位置ページ内）
        function switchLine(lineKey) {
            currentActiveLine = lineKey;
            document.querySelectorAll('.line-btn').forEach(btn => {
                if(btn.dataset.line === lineKey) {
                    btn.className = "line-btn px-4 py-2 rounded-lg text-xs font-bold transition bg-indigo-600 text-white shadow";
                } else {
                    btn.className = "line-btn px-4 py-2 rounded-lg text-xs font-bold transition bg-slate-900 text-slate-300 hover:bg-slate-700 border border-slate-700";
                }
            });
            renderRouteMap();
        }

        // 基本データ
        let appData = {
            trains: [
                { id: 1, name: "特急 101M", type: "特急", series: "S100系", line: "main", stationId: "01", direction: "down" },
                { id: 2, name: "普通 402C", type: "普通", series: "S3系", line: "main", stationId: "13", direction: "up" },
                { id: 3, name: "快速 205M", type: "快速", series: "S2系", line: "main", stationId: "30", direction: "down" }
            ]
        };

        let dbRef = null;
        const DEFAULT_FB_URL = "https://original-tetsudo-430ac-default-rtdb.firebaseio.com";

        // Firebase自動接続
        function initFirebase() {
            try {
                if(firebase.apps.length === 0) {
                    firebase.initializeApp({ databaseURL: DEFAULT_FB_URL });
                }
                dbRef = firebase.database().ref('shinomori_railway');
                dbRef.on('value', (snapshot) => {
                    const val = snapshot.val();
                    if(val) {
                        appData = val;
                        updateUI();
                    } else {
                        dbRef.set(appData);
                    }
                });
                document.getElementById('sync-status-dot').className = "w-2.5 h-2.5 rounded-full bg-emerald-500 animate-pulse";
                document.getElementById('sync-status-text').innerText = "クラウド同期: 接続中";
                document.getElementById('fb-url').value = DEFAULT_FB_URL;
            } catch(e) {
                document.getElementById('sync-status-dot').className = "w-2.5 h-2.5 rounded-full bg-red-500";
                document.getElementById('sync-status-text').innerText = "クラウド同期エラー";
            }
        }

        function pushData() {
            if(dbRef) {
                dbRef.set(appData);
            }
        }

        function updateUI() {
            renderRouteMap();
            renderTrainInfo();
            renderConsist();
            initTimetableDropdowns();
        }

        // 走行位置（エレサイト風縦型マップ描画）
        function renderRouteMap() {
            const container = document.getElementById('route-map-stations');
            const titleBanner = document.getElementById('line-title-banner');
            if(!container) return;

            const lineInfo = linesData[currentActiveLine];
            titleBanner.innerText = `${lineInfo.name} 運行モニター（全${lineInfo.stations.length}駅）`;

            container.innerHTML = lineInfo.stations.map((st) => {
                // この駅にいる下り列車
                const downTrains = appData.trains.filter(t => t.line === currentActiveLine && t.stationId === st.id && t.direction === 'down');
                // この駅にいる上り列車
                const upTrains = appData.trains.filter(t => t.line === currentActiveLine && t.stationId === st.id && t.direction === 'up');

                return `
                    <div class="relative flex items-center justify-between">
                        <!-- 左側：上り列車エリア -->
                        <div class="w-5/12 pr-4 text-right space-y-1">
                            ${upTrains.map(t => `
                                <div class="inline-block bg-slate-900 border border-sky-500/60 rounded px-2 py-1 text-xs shadow-lg animate-pulse">
                                    <span class="font-bold text-sky-300">${t.name}</span>
                                    <span class="text-[10px] text-slate-400 block">${t.series}</span>
                                </div>
                            `).join('')}
                        </div>

                        <!-- 中央：駅ノード -->
                        <div class="absolute left-1/2 transform -translate-x-1/2 flex items-center justify-center">
                            <div class="w-6 h-6 rounded-full bg-slate-900 border-4 border-indigo-500 shadow flex items-center justify-center z-20">
                                <div class="w-2 h-2 rounded-full bg-white"></div>
                            </div>
                        </div>

                        <!-- 右側：駅名 ＆ 下り列車エリア -->
                        <div class="w-5/12 pl-6 space-y-2">
                            <div class="bg-slate-900/90 border border-slate-700 px-3 py-2 rounded-lg shadow">
                                <span class="font-bold text-indigo-200 text-sm block">${st.id}. ${st.name}</span>
                                <div class="flex gap-1 mt-0.5">
                                    ${st.facilities.map(f => `<span class="text-[10px] bg-indigo-950 text-indigo-300 px-1.5 py-0.5 rounded border border-indigo-800">${f}</span>`).join('')}
                                </div>
                            </div>
                            <div class="space-y-1">
                                ${downTrains.map(t => `
                                    <div class="inline-block bg-slate-900 border border-rose-500/60 rounded px-2 py-1 text-xs shadow-lg animate-pulse">
                                        <span class="font-bold text-rose-300">${t.name}</span>
                                        <span class="text-[10px] text-slate-400 block">${t.series}</span>
                                    </div>
                                `).join('')}
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
        }

        // 列車追加時の駅ドロップダウン連動
        function updateAddStationDropdown() {
            const lineKey = document.getElementById('add-train-line').value;
            const stSelect = document.getElementById('add-train-station-id');
            const stations = linesData[lineKey].stations;
            stSelect.innerHTML = stations.map(st => `<option value="${st.id}">${st.id}. ${st.name}</option>`).join('');
        }

        // 新規列車追加
        function addNewTrain() {
            const name = document.getElementById('add-train-name').value;
            const type = document.getElementById('add-train-type').value;
            const series = document.getElementById('add-train-series').value;
            const line = document.getElementById('add-train-line').value;
            const stationId = document.getElementById('add-train-station-id').value;
            
            appData.trains.push({ 
                id: Date.now(), 
                name, 
                type, 
                series, 
                line,
                stationId, 
                direction: Math.random() > 0.5 ? 'down' : 'up' 
            });
            pushData();
            updateUI();
            alert("新規列車を追加し、運行マップに同期しました！");
            switchTab('operation');
        }

        // 時刻表用の駅ドロップダウン初期化
        function initTimetableDropdowns() {
            const stSelect = document.getElementById('tt-station');
            if(!stSelect) return;
            let allStations = [];
            Object.values(linesData).forEach(l => {
                l.stations.forEach(st => {
                    allStations.push(`<option value="${st.id}">${l.name} - ${st.id}. ${st.name}</option>`);
                });
            });
            stSelect.innerHTML = allStations.join('');
        }

        // 時刻表生成
        function renderTimetable() {
            const stId = document.getElementById('tt-station').value;
            const dir = document.getElementById('tt-dir').value;
            const dayType = document.getElementById('tt-day').value;
            const container = document.getElementById('timetable-container');

            let foundStName = "選択駅";
            Object.values(linesData).forEach(l => {
                const found = l.stations.find(s => s.id === stId);
                if(found) foundStName = found.name;
            });

            const dirText = dir === 'down' ? '下り（起点 → 終点方面）' : '上り（終点 → 起点方面）';
            const dayText = dayType === 'weekday' ? '平日ダイヤ' : '土休日ダイヤ';

            container.innerHTML = `
                <div class="mb-3 text-xs text-indigo-300 font-bold">
                    【${foundStName} (${stId})】 ${dirText} ／ ${dayText} 発車時刻表
                </div>
                <table class="w-full text-left border-collapse">
                    <thead>
                        <tr class="border-b border-slate-700 text-indigo-200 text-xs">
                            <th class="p-2">時</th>
                            <th class="p-2">分 (特急 / 急行 / 快速 / 普通)</th>
                        </tr>
                    </thead>
                    <tbody class="text-slate-300 text-sm">
                        <tr class="border-b border-slate-800"><td class="p-2 font-mono font-bold text-indigo-400">06</td><td class="p-2">15<span class="text-xs text-slate-500">(普)</span> 30<span class="text-xs text-slate-500">(快)</span> 50<span class="text-xs text-slate-500">(特)</span></td></tr>
                        <tr class="border-b border-slate-800"><td class="p-2 font-mono font-bold text-indigo-400">07</td><td class="p-2">05<span class="text-xs text-slate-500">(特)</span> 18<span class="text-xs text-slate-500">(急)</span> 32<span class="text-xs text-slate-500">(普)</span> 45<span class="text-xs text-slate-500">(快)</span></td></tr>
                        <tr class="border-b border-slate-800"><td class="p-2 font-mono font-bold text-indigo-400">08</td><td class="p-2">00<span class="text-xs text-slate-500">(特)</span> 12<span class="text-xs text-slate-500">(普)</span> 28<span class="text-xs text-slate-500">(急)</span> 44<span class="text-xs text-slate-500">(快)</span></td></tr>
                        <tr class="border-b border-slate-800"><td class="p-2 font-mono font-bold text-indigo-400">09</td><td class="p-2">10<span class="text-xs text-slate-500">(特)</span> 30<span class="text-xs text-slate-500">(普)</span> 55<span class="text-xs text-slate-500">(普)</span></td></tr>
                    </tbody>
                </table>
            `;
        }

        // 車両情報図鑑
        function renderTrainInfo() {
            const grid = document.getElementById('train-info-grid');
            if(!grid) return;
            const seriesList = [
                { name: "S100系 (特急型)", desc: "最高速度130km/h。紫句守鉄道のフラグシップ特急。4〜10両編成。" },
                { name: "S1系", desc: "主力通勤・近郊型車両。8〜10両編成で本線運用を中心に活躍。" },
                { name: "S2系", desc: "快速・急行用運用に対応する汎用型車両。8〜10両編成。" },
                { name: "S3系", desc: "4・6・8・10両と柔軟な編成が組める中距離向け車両。" },
                { name: "S4系", desc: "高出力モーター搭載の山岳・勾配線区対応車両。" },
                { name: "S5系", desc: "10両固定編成の大容量通勤型車両。" },
                { name: "S900系", desc: "検測・救援等の事業用4両編成車両。" }
            ];
            grid.innerHTML = seriesList.map(s => `
                <div class="bg-slate-800 p-5 rounded-xl border border-slate-700 space-y-2">
                    <h3 class="font-bold text-indigo-300 text-lg">${s.name}</h3>
                    <p class="text-sm text-slate-300">${s.desc}</p>
                </div>
            `).join('');
        }

        // 編成表
        function renderConsist() {
            const list = document.getElementById('consist-list');
            if(!list) return;
            list.innerHTML = [
                { name: "S100系 特急 (10両)", cars: ["クハ100", "モハ101", "サロ100", "モハ102", "サハ103", "モハ101", "サハ104", "モハ102", "モハ101", "クハ101"] },
                { name: "S1系 普通/快速 (8両)", cars: ["クハ1", "モハ2", "モハ3", "サハ4", "モハ2", "モハ3", "サハ4", "クハ1"] },
                { name: "S3系 4+4両編成", cars: ["クハ", "モハ", "モハ", "クハ", " ＋ ", "クハ", "モハ", "モハ", "クハ"] }
            ].map(c => `
                <div class="bg-slate-800 p-4 rounded-xl border border-slate-700 space-y-2">
                    <h3 class="font-bold text-indigo-300">${c.name}</h3>
                    <div class="flex flex-wrap gap-1 text-xs font-mono">
                        ${c.cars.map(car => `<span class="bg-slate-900 border border-slate-700 px-2 py-1 rounded text-slate-200">${car}</span>`).join('')}
                    </div>
                </div>
            `).join('');
        }

        // ダイヤグラム描画
        function drawDiagram() {
            const canvas = document.getElementById('diag-canvas');
            if(!canvas) return;
            const ctx = canvas.getContext('2d');
            ctx.fillStyle = '#0f172a';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            ctx.strokeStyle = '#334155';
            ctx.lineWidth = 1;
            for(let i=0; i<canvas.width; i+=50) {
                ctx.beginPath(); ctx.moveTo(i, 0); ctx.lineTo(i, canvas.height); ctx.stroke();
            }
            for(let i=0; i<canvas.height; i+=40) {
                ctx.beginPath(); ctx.moveTo(0, i); ctx.lineTo(canvas.width, i); ctx.stroke();
            }

            ctx.strokeStyle = '#f43f5e';
            ctx.lineWidth = 2;
            ctx.beginPath();
            ctx.moveTo(50, 50); ctx.lineTo(950, 550);
            ctx.stroke();

            ctx.strokeStyle = '#38bdf8';
            ctx.beginPath();
            ctx.moveTo(50, 500); ctx.lineTo(950, 100);
            ctx.stroke();
        }

        // リアルタイム時計更新
        function updateLiveDateTime() {
            const now = new Date();
            const year = now.getFullYear();
            const month = String(now.getMonth() + 1).padStart(2, '0');
            const day = String(now.getDate()).padStart(2, '0');
            const days = ['日', '月', '火', '水', '木', '金', '土'];
            const dayOfWeek = days[now.getDay()];
            const hours = String(now.getHours()).padStart(2, '0');
            const minutes = String(now.getMinutes()).padStart(2, '0');
            const seconds = String(now.getSeconds()).padStart(2, '0');

            const text = `${year}/${month}/${day}(${dayOfWeek}) ${hours}:${minutes}:${seconds}`;
            const el = document.getElementById('live-datetime');
            if(el) el.innerText = text;
        }

        // 初期化実行
        window.onload = function() {
            initFirebase();
            updateAddStationDropdown();
            initTimetableDropdowns();
            setInterval(updateLiveDateTime, 1000);
            updateLiveDateTime();
        };
    </script>
</body>
</html>
