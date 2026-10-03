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
            <div id="live-datetime" class="bg-slate-900 px-3 py-1.5 rounded-lg border border-slate-700 text-xs font-mono text-indigo-300">
                2026/10/03(土) 00:00:00
            </div>
            <div id="event-banner-badge" class="hidden bg-rose-900 text-rose-200 px-2.5 py-1 rounded-lg text-xs font-bold border border-rose-700 animate-pulse">
                🎉 イベント日ダイヤ運行中
            </div>
            <div class="hidden sm:flex items-center space-x-2 bg-slate-900 px-3 py-1.5 rounded-lg border border-slate-700 text-xs">
                <span id="sync-status-dot" class="w-2.5 h-2.5 rounded-full bg-amber-500 animate-pulse"></span>
                <span id="sync-status-text">クラウド同期: 接続待機中...</span>
            </div>
            <button onclick="toggleMenu()" class="bg-indigo-900 hover:bg-indigo-800 border border-indigo-700 p-2 rounded-lg text-white md:hidden transition flex items-center justify-center w-10 h-10 shadow">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path>
                </svg>
            </button>
        </div>
    </header>

    <!-- ナビゲーションタブ -->
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
                    紫句守鉄道は、首都圏と豊かな自然に恵まれた紫句守・星句高原・句守支線エリアを結ぶ主要幹線を運行する鉄道会社です。「安全・信頼・快適」を経営の基本方針に掲げ、地域社会の発展と観光需要の活性化に貢献しています。
                </p>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="bg-slate-800 p-5 rounded-xl border border-slate-700 space-y-3">
                    <h3 class="text-indigo-400 font-bold border-b border-slate-700 pb-2">企業概要</h3>
                    <ul class="text-sm text-slate-300 space-y-2">
                        <li><span class="text-slate-400 inline-block w-28">社名</span> 紫句守鉄道株式会社</li>
                        <li><span class="text-slate-400 inline-block w-28">設立</span> 1965年4月1日</li>
                        <li><span class="text-slate-400 inline-block w-28">本社所在地</span> 陽光県紫句守市中央一丁目1番地</li>
                    </ul>
                </div>
                <div class="bg-slate-800 p-5 rounded-xl border border-slate-700 space-y-3">
                    <h3 class="text-indigo-400 font-bold border-b border-slate-700 pb-2">路線・車両データ</h3>
                    <ul class="text-sm text-slate-300 space-y-2">
                        <li><span class="text-slate-400 inline-block w-28">運行路線</span> 紫雲本線(1〜30)、星句高原線(31〜40)、句守支線(41〜55)、紫霞観光線(56〜60)</li>
                        <li><span class="text-slate-400 inline-block w-28">保有形式</span> S1系, S2系, S3系, S4系, S5系, S100系, S900系</li>
                    </ul>
                </div>
            </div>
        </div>

        <!-- 2. 時刻表 -->
        <div id="tab-timetable" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">インタラクティブ時刻表</h2>
            <div class="bg-slate-800 p-4 rounded-xl border border-slate-700 grid grid-cols-1 sm:grid-cols-4 gap-4 items-end">
                <div>
                    <label class="block text-xs text-slate-400 mb-1">駅選択</label>
                    <select id="tt-station" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-sm text-white"></select>
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
                        <option value="event">イベント日ダイヤ</option>
                        <option value="newyear">年末年始ダイヤ</option>
                    </select>
                </div>
                <button onclick="renderTimetable()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-4 py-2 rounded text-sm font-medium transition shadow">時刻表を表示</button>
            </div>
            <div id="timetable-container" class="bg-slate-800 rounded-xl border border-slate-700 p-4 overflow-x-auto text-sm">
                <p class="text-slate-400">条件を選択して「時刻表を表示」を押してください。</p>
            </div>
        </div>

        <!-- 3. 走行位置 -->
        <div id="tab-operation" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">リアルタイム運行状況マップ</h2>
            <div class="flex flex-wrap gap-2 bg-slate-800 p-3 rounded-xl border border-slate-700">
                <button onclick="switchLine('main')" class="line-btn px-4 py-2 rounded-lg text-xs font-bold transition bg-indigo-600 text-white shadow" data-line="main">紫雲本線 (1〜30)</button>
                <button onclick="switchLine('plateau')" class="line-btn px-4 py-2 rounded-lg text-xs font-bold transition bg-slate-900 text-slate-300 hover:bg-slate-700 border border-slate-700" data-line="plateau">星句高原線 (31〜40)</button>
                <button onclick="switchLine('branch')" class="line-btn px-4 py-2 rounded-lg text-xs font-bold transition bg-slate-900 text-slate-300 hover:bg-slate-700 border border-slate-700" data-line="branch">句守支線 (41〜55)</button>
                <button onclick="switchLine('sight')" class="line-btn px-4 py-2 rounded-lg text-xs font-bold transition bg-slate-900 text-slate-300 hover:bg-slate-700 border border-slate-700" data-line="sight">紫霞観光線 (56〜60)</button>
            </div>

            <div class="bg-slate-800 p-6 rounded-xl border border-slate-700 relative overflow-y-auto max-h-[650px] shadow-xl">
                <div id="line-title-banner" class="text-xs text-indigo-300 mb-6 text-center font-bold">紫雲本線 運行モニター</div>
                <div class="relative max-w-lg mx-auto py-6">
                    <div class="absolute left-1/2 transform -translate-x-1/2 top-0 bottom-0 w-2 bg-gradient-to-b from-indigo-500 via-sky-500 to-indigo-600 rounded-full"></div>
                    <div id="route-map-stations" class="space-y-8 relative z-10"></div>
                </div>
            </div>
        </div>

        <!-- 4. 列車情報 -->
        <div id="tab-traininfo" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">列車情報一覧</h2>
            <div class="bg-slate-800 rounded-xl border border-slate-700 overflow-hidden">
                <div class="overflow-x-auto">
                    <table class="w-full text-left text-xs">
                        <thead class="bg-slate-900 text-indigo-200 border-b border-slate-700">
                            <tr>
                                <th class="p-3">列車番号</th>
                                <th class="p-3">運用</th>
                                <th class="p-3">種別</th>
                                <th class="p-3">運行日</th>
                                <th class="p-3">区間</th>
                                <th class="p-3">編成</th>
                                <th class="p-3">状態</th>
                            </tr>
                        </thead>
                        <tbody id="train-info-tbody" class="divide-y divide-slate-700 text-slate-300"></tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- 5. 列車追加 -->
        <div id="tab-addtrain" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">新規列車運用追加・詳細設定</h2>
            <div class="bg-slate-800 p-6 rounded-xl border border-slate-700 max-w-3xl space-y-5">
                
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">列車番号</label>
                        <input type="text" id="add-train-num" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white" value="101M">
                    </div>
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">運用番号</label>
                        <input type="text" id="add-op-num" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white" value="A01">
                    </div>
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">運行日設定</label>
                        <select id="add-run-day" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                            <option value="weekday">平日</option>
                            <option value="holiday">土休日</option>
                            <option value="event">イベント日</option>
                            <option value="newyear">年末年始</option>
                        </select>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">種別</label>
                        <select id="add-train-type" onchange="updateStationSchedulePreview()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                            <option>特急</option><option>通勤急行</option><option>急行</option><option>通勤快速</option><option>快速</option><option>準急</option><option>普通</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">路線名</label>
                        <select id="add-train-line" onchange="updateAddStationDropdowns()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                            <option value="main">紫雲本線 (1〜30)</option>
                            <option value="plateau">星句高原線 (31〜40)</option>
                            <option value="branch">句守支線 (41〜55)</option>
                            <option value="sight">紫霞観光線 (56〜60)</option>
                            <option value="direct">直通路線 (09〜1)</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">両数構成</label>
                        <select id="add-cars-count" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                            <option>10両</option><option>8両</option><option>6両</option><option>4両</option><option>4＋4両</option><option>4＋6両</option>
                        </select>
                    </div>
                </div>

                <!-- 列車編成選択 -->
                <div class="border border-slate-700 p-4 rounded-lg bg-slate-900 space-y-3">
                    <span class="text-xs text-indigo-300 font-bold block">列車編成（形式と編成番号）</span>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label class="block text-xs text-slate-400 mb-1">形式</label>
                            <select id="consist-series" onchange="updateConsistNumbers()" class="w-full bg-slate-800 border border-slate-700 rounded px-2 py-1.5 text-xs text-white">
                                <option value="S1">S1系 (01-05:10両 / 06-10:8両)</option>
                                <option value="S2">S2系 (01-15:10両 / 16-25:8両)</option>
                                <option value="S3">S3系 (10/8/6/4両)</option>
                                <option value="S4">S4系 (10/8/6両)</option>
                                <option value="S5">S5系 (01-04:10両)</option>
                                <option value="S100" selected>S100系 特急 (10/6/4両)</option>
                                <option value="S900">S900系 事業用 (4両)</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs text-slate-400 mb-1">編成番号</label>
                            <select id="consist-number-sel" class="w-full bg-slate-800 border border-slate-700 rounded px-2 py-1.5 text-xs text-white"></select>
                        </div>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">始点駅</label>
                        <select id="add-start-station" onchange="updateStationSchedulePreview()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white"></select>
                    </div>
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">終点駅</label>
                        <select id="add-end-station" onchange="updateStationSchedulePreview()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white"></select>
                    </div>
                </div>

                <!-- 停車駅・時間設定プレビューエリア -->
                <div class="space-y-2">
                    <span class="text-xs text-indigo-300 font-bold block">停車駅スケジュール・発着時間設定</span>
                    <div id="station-schedule-preview" class="bg-slate-900 border border-slate-700 rounded-lg p-3 max-h-56 overflow-y-auto text-xs space-y-2">
                        <p class="text-slate-400">始点・終点を選択すると停車駅と時間設定欄が展開されます。</p>
                    </div>
                </div>

                <button onclick="addNewTrain()" class="w-full bg-indigo-600 hover:bg-indigo-500 text-white font-medium py-2.5 rounded-lg transition shadow text-sm">列車を登録して一覧・ダイヤグラムに反映</button>
            </div>
        </div>

        <!-- 6. 編成表 -->
        <div id="tab-consist" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">全形式・編成運用管理表</h2>
            <div id="consist-matrix-container" class="space-y-4"></div>
        </div>

        <!-- 7. ダイヤ表 -->
        <div id="tab-dia" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">ダイヤグラム（紙の時刻表風マトリクス）</h2>
            <div class="bg-slate-800 p-4 rounded-xl border border-slate-700 overflow-x-auto">
                <div id="matrix-timetable-container" class="min-w-[800px]"></div>
            </div>
        </div>

        <!-- 8. 設定 -->
        <div id="tab-settings" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">システム設定・イベント日管理</h2>
            <div class="bg-slate-800 p-6 rounded-xl border border-slate-700 max-w-xl space-y-5">
                <p class="text-xs text-slate-300">Firebase Realtime Database によるマルチデバイス間の自動同期が有効です。</p>
                <div>
                    <label class="block text-xs text-slate-400 mb-1">接続中 Firebase Database URL</label>
                    <input type="text" id="fb-url" readonly class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-slate-400 cursor-not-allowed">
                </div>
                <hr class="border-slate-700">
                <div class="space-y-3">
                    <h3 class="text-sm font-bold text-indigo-300">イベント日カレンダー設定（複数日登録）</h3>
                    <div class="flex gap-2">
                        <input type="date" id="new-event-date" class="bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-sm text-white">
                        <button onclick="addEventDate()" class="bg-emerald-600 hover:bg-emerald-500 text-white px-4 py-1.5 rounded text-xs font-bold transition">イベント日を追加</button>
                    </div>
                    <div id="event-dates-list" class="flex flex-wrap gap-2 pt-2"></div>
                </div>
            </div>
        </div>

    </main>

    <script>
        const allStationsMaster = [
            {id: "09", name: "水鳥湿原"}, {id: "08", name: "青蓮寺"}, {id: "07", name: "紫水"}, {id: "06", name: "瑠璃川"}, {id: "05", name: "翡翠野"}, {id: "04", name: "琥珀谷"}, {id: "03", name: "瑪瑙台"}, {id: "02", name: "天翔"},
            {id: "1", name: "紫句守中央"}, {id: "2", name: "霞詠ヶ丘"}, {id: "3", name: "紫雲野"}, {id: "4", name: "詩羽町"}, {id: "5", name: "星句台"}, {id: "6", name: "紫陽花前"}, {id: "7", name: "句守書院"}, {id: "8", name: "銀墨坂"}, {id: "9", name: "紫峰高原"}, {id: "10", name: "風詠の森"},
            {id: "11", name: "宵月町"}, {id: "12", name: "句読通り"}, {id: "13", name: "紫霞野"}, {id: "14", name: "鏡句池"}, {id: "15", name: "雨詠坂"}, {id: "16", name: "紫都新町"}, {id: "17", name: "句守空港"}, {id: "18", name: "星詠港"}, {id: "19", name: "紫光浜"}, {id: "20", name: "潮句の杜"},
            {id: "21", name: "紫苑台"}, {id: "22", name: "句守温泉"}, {id: "23", name: "霧詠峠"}, {id: "24", name: "鷲羽句守"}, {id: "25", name: "紫野学園前"}, {id: "26", name: "書詠通り"}, {id: "27", name: "月句台"}, {id: "28", name: "紫句守美術館"}, {id: "29", name: "詩風町"}, {id: "30", name: "紫句守展示場"},
            {id: "31", name: "星句高原入口"}, {id: "32", name: "星霧の丘"}, {id: "33", name: "天詠台"}, {id: "34", name: "星句牧場前"}, {id: "35", name: "星句温泉郷"}, {id: "36", name: "星句森林"}, {id: "37", name: "星句湖畔"}, {id: "38", name: "句守詩碑前"}, {id: "39", name: "星句高原村"}, {id: "40", name: "星句高原"},
            {id: "41", name: "紫句守港"}, {id: "42", name: "紫句守湾岸"}, {id: "43", name: "句守湾岸"}, {id: "44", name: "紫句守タワー"}, {id: "45", name: "句守の杜"}, {id: "46", name: "紫句守工業団地"}, {id: "47", name: "紫句守工業"}, {id: "48", name: "鉄輪詩町"}, {id: "49", name: "紫句守市場"}, {id: "50", name: "詩詠の里"},
            {id: "51", name: "紫句守農園"}, {id: "52", name: "紫句守劇場前"}, {id: "53", name: "句守未来都市"}, {id: "54", name: "紫句守研究所"}, {id: "55", name: "詩句の丘"},
            {id: "56", name: "紫句守展望台"}, {id: "57", name: "句守星見台"}, {id: "58", name: "紫句守森林公園"}, {id: "59", name: "紫句守詩碑前"}, {id: "60", name: "紫句守詩碑"}
        ];

        const linesData = {
            main: { name: "紫雲本線", stations: allStationsMaster.filter(s => { const n = parseInt(s.id); return !isNaN(n) && n >= 1 && n <= 30; }) },
            plateau: { name: "星句高原線", stations: allStationsMaster.filter(s => { const n = parseInt(s.id); return !isNaN(n) && n >= 31 && n <= 40; }) },
            branch: { name: "句守支線", stations: allStationsMaster.filter(s => { const n = parseInt(s.id); return !isNaN(n) && n >= 41 && n <= 55; }) },
            sight: { name: "紫霞観光線", stations: allStationsMaster.filter(s => { const n = parseInt(s.id); return !isNaN(n) && n >= 56 && n <= 60; }) },
            direct: { name: "他の会社直通路線", stations: allStationsMaster.filter(s => s.id.startsWith("0") || parseInt(s.id) <= 9) }
        };

        const fullConsistsData = [
            { series: "S1系", items: ["S1-01 (10両)", "S1-02 (10両)", "S1-03 (10両)", "S1-04 (10両)", "S1-05 (10両)", "S1-06 (8両)", "S1-07 (8両)", "S1-08 (8両)", "S1-09 (8両)", "S1-10 (8両)"] },
            { series: "S2系", items: Array.from({length: 15}, (_,i) => `S2-${String(i+1).padStart(2,'0')} (10両)`).concat(Array.from({length: 10}, (_,i) => `S2-${String(i+16).padStart(2,'0')} (8両)`)) },
            { series: "S3系", items: Array.from({length: 10}, (_,i) => `S3-${String(i+1).padStart(2,'0')} (10両)`).concat(Array.from({length: 5}, (_,i) => `S3-${String(i+11).padStart(2,'0')} (8両)`)).concat(Array.from({length: 5}, (_,i) => `S3-${String(i+16).padStart(2,'0')} (6両)`)).concat(Array.from({length: 5}, (_,i) => `S3-${String(i+21).padStart(2,'0')} (4両)`)) },
            { series: "S4系", items: Array.from({length: 10}, (_,i) => `S4-${String(i+1).padStart(2,'0')} (10両)`).concat(Array.from({length: 10}, (_,i) => `S4-${String(i+11).padStart(2,'0')} (8両)`)).concat(Array.from({length: 5}, (_,i) => `S4-${String(i+21).padStart(2,'0')} (6両)`)) },
            { series: "S5系", items: ["S5-01 (10両)", "S5-02 (10両)", "S5-03 (10両)", "S5-04 (10両)"] },
            { series: "S100系", items: Array.from({length: 10}, (_,i) => `S100-${String(i+1).padStart(2,'0')} (特急10両)`).concat(Array.from({length: 5}, (_,i) => `S100-${String(i+11).padStart(2,'0')} (特急6両)`)).concat(Array.from({length: 5}, (_,i) => `S100-${String(i+16).padStart(2,'0')} (特急4両)`)) },
            { series: "S900系", items: ["S900-01 (事業用4両)", "S900-02 (事業用4両)"] }
        ];

        let currentActiveLine = 'main';

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
            const menu = document.getElementById('nav-menu');
            if(menu.classList.contains('absolute')) toggleMenu();

            if (tabId === 'dia') renderMatrixTimetable();
            if (tabId === 'operation') renderRouteMap();
            if (tabId === 'timetable') initTimetableDropdowns();
            if (tabId === 'traininfo') renderTrainInfoTable();
            if (tabId === 'consist') renderConsistMatrix();
            if (tabId === 'settings') renderEventDatesList();
            if (tabId === 'addtrain') updateStationSchedulePreview();
        }

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

        let appData = { trains: [], eventDates: ["2026-10-15"] };
        let dbRef = null;
        const DEFAULT_FB_URL = "https://original-tetsudo-430ac-default-rtdb.firebaseio.com";

        function initFirebase() {
            try {
                if(firebase.apps.length === 0) {
                    firebase.initializeApp({ databaseURL: DEFAULT_FB_URL });
                }
                dbRef = firebase.database().ref('shinomori_railway_v6');
                dbRef.on('value', (snapshot) => {
                    const val = snapshot.val();
                    if(val) {
                        appData = val;
                        if(!appData.trains) appData.trains = [];
                        if(!appData.eventDates) appData.eventDates = ["2026-10-15"];
                        updateUI();
                    } else {
                        dbRef.set(appData);
                    }
                    // URLクエリパラメータによる運行状況変更の処理
                    handleUrlQueryAction();
                });
                document.getElementById('sync-status-dot').className = "w-2.5 h-2.5 rounded-full bg-emerald-500 animate-pulse";
                document.getElementById('sync-status-text').innerText = "クラウド同期: 接続中";
                document.getElementById('fb-url').value = DEFAULT_FB_URL;
            } catch(e) {
                document.getElementById('sync-status-dot').className = "w-2.5 h-2.5 rounded-full bg-red-500";
                document.getElementById('sync-status-text').innerText = "クラウド同期エラー";
            }
        }

        // URLクエリパラメータを解析して運行状況を自動変更する機能
        function handleUrlQueryAction() {
            const params = new URLSearchParams(window.location.search);
            const targetTrainNum = params.get('train');
            const newStatus = params.get('status');

            if (targetTrainNum && newStatus && appData.trains) {
                let updated = false;
                appData.trains.forEach(t => {
                    if (t.trainNum === targetTrainNum && t.status !== newStatus) {
                        t.status = newStatus;
                        updated = true;
                    }
                });
                if (updated) {
                    pushData();
                    console.log(`URLクエリにより列車 ${targetTrainNum} のステータスを ${newStatus} に更新しました`);
                }
            }
        }

        function pushData() { if(dbRef) dbRef.set(appData); }
        function updateUI() {
            renderRouteMap();
            renderTrainInfoTable();
            renderConsistMatrix();
            initTimetableDropdowns();
            renderEventDatesList();
            checkEventDayStatus();
            updateStationSchedulePreview();
        }

        function updateAddStationDropdowns() {
            const lineKey = document.getElementById('add-train-line').value;
            const stations = linesData[lineKey].stations;
            const options = stations.map(st => `<option value="${st.id}">${st.id}. ${st.name}</option>`).join('');
            
            document.getElementById('add-start-station').innerHTML = options;
            document.getElementById('add-end-station').innerHTML = options;
            if(stations.length > 1) document.getElementById('add-end-station').selectedIndex = stations.length - 1;
            updateStationSchedulePreview();
        }

        function updateConsistNumbers() {
            const series = document.getElementById('consist-series').value;
            const sel = document.getElementById('consist-number-sel');
            let found = fullConsistsData.find(c => c.series.startsWith(series));
            if(found) {
                sel.innerHTML = found.items.map(it => `<option>${it}</option>`).join('');
            }
        }

        function updateStationSchedulePreview() {
            const lineKey = document.getElementById('add-train-line').value;
            const startId = document.getElementById('add-start-station').value;
            const endId = document.getElementById('add-end-station').value;
            const stations = linesData[lineKey].stations;
            
            const sIdx = stations.findIndex(s => s.id === startId);
            const eIdx = stations.findIndex(s => s.id === endId);
            const preview = document.getElementById('station-schedule-preview');
            if(!preview) return;
            
            if(sIdx === -1 || eIdx === -1 || sIdx > eIdx) {
                preview.innerHTML = `<p class="text-red-400">始点と終点の順序を確認してください。</p>`;
                return;
            }

            let html = `<table class="w-full text-left"><thead><tr class="text-indigo-300 border-b border-slate-800"><th class="p-1">駅名</th><th class="p-1">到着時刻</th><th class="p-1">発車時刻</th></tr></thead><tbody>`;
            for(let i = sIdx; i <= eIdx; i++) {
                const st = stations[i];
                const timeStr = `08:${String(10 + (i - sIdx)*3).padStart(2,'0')}`;
                html += `
                    <tr class="border-b border-slate-800">
                        <td class="p-1 font-bold">${st.id}. ${st.name}</td>
                        <td class="p-1 font-mono"><input type="text" value="${timeStr}" class="arr-time bg-slate-800 border border-slate-700 rounded px-1 w-16 text-xs text-white"></td>
                        <td class="p-1 font-mono"><input type="text" value="${timeStr}" class="dep-time bg-slate-800 border border-slate-700 rounded px-1 w-16 text-xs text-white"></td>
                    </tr>
                `;
            }
            html += `</tbody></table>`;
            preview.innerHTML = html;
        }

        function addNewTrain() {
            const trainNum = document.getElementById('add-train-num').value;
            const opNum = document.getElementById('add-op-num').value;
            const type = document.getElementById('add-train-type').value;
            const runDay = document.getElementById('add-run-day').value;
            const cars = document.getElementById('add-cars-count').value;
            const line = document.getElementById('add-train-line').value;
            const startSt = document.getElementById('add-start-station').value;
            const endSt = document.getElementById('add-end-station').value;
            const series = document.getElementById('consist-series').value;
            const consistNum = document.getElementById('consist-number-sel').value;
            
            const carsStr = cars + ` (${series} ${consistNum})`;
            const statuses = ["運行前", "運行準備中", "走行中", "停車中", "運行終了", "運行なし"];
            const randomStatus = statuses[Math.floor(Math.random() * statuses.length)];

            if(!appData.trains) appData.trains = [];
            appData.trains.push({
                id: Date.now(),
                trainNum,
                opNum,
                type,
                runDay,
                line,
                startSt,
                endSt,
                cars: carsStr,
                status: randomStatus,
                stationId: startSt
            });

            pushData();
            updateUI();
            alert("列車を正常に追加しました！");
            switchTab('traininfo');
        }

        function renderTrainInfoTable() {
            const tbody = document.getElementById('train-info-tbody');
            if(!tbody) return;

            const trains = appData.trains || [];
            if(trains.length === 0) {
                tbody.innerHTML = `<tr><td colspan="7" class="p-4 text-center text-slate-500">追加された列車はありません。「列車追加」タブから登録してください。</td></tr>`;
                return;
            }

            tbody.innerHTML = trains.map(t => {
                let badgeColor = "bg-slate-700 text-slate-300";
                if(t.status === "走行中") badgeColor = "bg-emerald-950 text-emerald-300 border border-emerald-700";
                if(t.status === "停車中") badgeColor = "bg-sky-950 text-sky-300 border border-sky-700";
                if(t.status === "運行準備中") badgeColor = "bg-amber-950 text-amber-300 border border-amber-700";
                if(t.status === "運行前") badgeColor = "bg-purple-950 text-purple-300 border border-purple-700";
                if(t.status === "運行終了") badgeColor = "bg-rose-950 text-rose-300 border border-rose-700";
                if(t.status === "運行なし") badgeColor = "bg-slate-800 text-slate-500";

                const startName = allStationsMaster.find(s => s.id === t.startSt)?.name || t.startSt;
                const endName = allStationsMaster.find(s => s.id === t.endSt)?.name || t.endSt;
                const dayLabel = t.runDay === 'weekday' ? '平日' : t.runDay === 'holiday' ? '土休日' : t.runDay === 'event' ? 'イベント日' : '年末年始';

                return `
                    <tr class="hover:bg-slate-750 transition">
                        <td class="p-3 font-bold text-indigo-300">${t.trainNum}</td>
                        <td class="p-3 font-mono">${t.opNum}</td>
                        <td class="p-3">${t.type}</td>
                        <td class="p-3"><span class="bg-slate-900 px-1.5 py-0.5 rounded border border-slate-700 text-[10px]">${dayLabel}</span></td>
                        <td class="p-3">${startName} → ${endName}</td>
                        <td class="p-3">${t.cars}</td>
                        <td class="p-3"><span class="px-2 py-0.5 rounded text-[10px] font-bold ${badgeColor}">${t.status}</span></td>
                    </tr>
                `;
            }).join('');
        }

        function renderConsistMatrix() {
            const container = document.getElementById('consist-matrix-container');
            if(!container) return;

            container.innerHTML = fullConsistsData.map(group => `
                <div class="bg-slate-800 p-4 rounded-xl border border-slate-700 space-y-3">
                    <h3 class="font-bold text-indigo-300 text-sm border-b border-slate-700 pb-1">${group.series} 運用割当表</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-2 text-xs">
                        ${group.items.map(item => {
                            const assigned = (appData.trains || []).filter(t => t.cars.includes(item.split(' ')[0]));
                            const opText = assigned.length > 0 ? assigned.map(a => `${a.opNum}(${a.trainNum})`).join(', ') : '予備・非稼働';
                            return `
                                <div class="bg-slate-900 p-2.5 rounded border border-slate-700 flex justify-between items-center">
                                    <span class="font-mono text-slate-200 font-bold">${item}</span>
                                    <span class="text-indigo-400 font-mono bg-indigo-950 px-2 py-0.5 rounded border border-indigo-900">${opText}</span>
                                </div>
                            `;
                        }).join('')}
                    </div>
                </div>
            `).join('');
        }

        function renderMatrixTimetable() {
            const container = document.getElementById('matrix-timetable-container');
            if(!container) return;

            const trains = appData.trains || [];
            if(trains.length === 0) {
                container.innerHTML = `<p class="text-slate-500 text-center py-8">列車が登録されていません。「列車追加」から登録してください。</p>`;
                return;
            }

            let html = `
                <table class="w-full border-collapse text-xs text-center">
                    <thead>
                        <tr class="bg-slate-900 text-indigo-200 border-b border-slate-700">
                            <th class="p-2 border-r border-slate-700 text-left sticky left-0 bg-slate-900 z-10">駅名</th>
            `;
            trains.forEach(t => {
                html += `<th class="p-2 border-r border-slate-700"><div class="font-bold text-indigo-300">${t.trainNum}</div><div class="text-[10px] text-slate-400">${t.type}</div></th>`;
            });
            html += `</tr></thead><tbody class="divide-y divide-slate-800 text-slate-300">`;

            allStationsMaster.forEach(st => {
                html += `<tr class="hover:bg-slate-750"><td class="p-2 border-r border-slate-700 text-left font-bold sticky left-0 bg-slate-800 z-10">${st.id}. ${st.name}</td>`;
                trains.forEach(t => {
                    const isMatch = (t.startSt === st.id || t.endSt === st.id || parseInt(st.id || '1')%3 === 0);
                    const timeCell = isMatch ? `08:${String(parseInt(st.id || '1')*2).padStart(2,'0')}` : '｜';
                    html += `<td class="p-2 border-r border-slate-800 font-mono">${timeCell}</td>`;
                });
                html += `</tr>`;
            });

            html += `</tbody></table>`;
            container.innerHTML = html;
        }

        function renderEventDatesList() {
            const listEl = document.getElementById('event-dates-list');
            if(!listEl) return;
            const dates = appData.eventDates || [];
            listEl.innerHTML = dates.map((d, idx) => `
                <span class="bg-indigo-950 border border-indigo-700 text-indigo-200 px-3 py-1 rounded-lg text-xs flex items-center gap-2">
                    📅 ${d}
                    <button onclick="removeEventDate(${idx})" class="text-rose-400 hover:text-rose-200 font-bold">×</button>
                </span>
            `).join('');
        }

        function addEventDate() {
            const val = document.getElementById('new-event-date').value;
            if(!val) { alert("日付を選択してください"); return; }
            if(!appData.eventDates) appData.eventDates = [];
            if(!appData.eventDates.includes(val)) {
                appData.eventDates.push(val);
                pushData();
                renderEventDatesList();
                checkEventDayStatus();
                alert("イベント日を追加しました！");
            }
        }

        function removeEventDate(idx) {
            appData.eventDates.splice(idx, 1);
            pushData();
            renderEventDatesList();
            checkEventDayStatus();
        }

        function checkEventDayStatus() {
            const now = new Date();
            const yyyy = now.getFullYear();
            const mm = String(now.getMonth() + 1).padStart(2, '0');
            const dd = String(now.getDate()).padStart(2, '0');
            const todayStr = `${yyyy}-${mm}-${dd}`;

            const badge = document.getElementById('event-banner-badge');
            if(badge && appData.eventDates && appData.eventDates.includes(todayStr)) {
                badge.classList.remove('hidden');
            } else if(badge) {
                badge.classList.add('hidden');
            }
        }

        function renderRouteMap() {
            const container = document.getElementById('route-map-stations');
            const titleBanner = document.getElementById('line-title-banner');
            if(!container) return;
            const lineInfo = linesData[currentActiveLine];
            titleBanner.innerText = `${lineInfo.name} 運行モニター（全${lineInfo.stations.length}駅）`;

            container.innerHTML = lineInfo.stations.map((st) => {
                const trains = (appData.trains || []).filter(t => t.line === currentActiveLine && t.stationId === st.id);
                return `
                    <div class="relative flex items-center justify-between">
                        <div class="w-5/12 pr-4 text-right space-y-1"></div>
                        <div class="absolute left-1/2 transform -translate-x-1/2 flex items-center justify-center">
                            <div class="w-6 h-6 rounded-full bg-slate-900 border-4 border-indigo-500 shadow flex items-center justify-center z-20">
                                <div class="w-2 h-2 rounded-full bg-white"></div>
                            </div>
                        </div>
                        <div class="w-5/12 pl-6 space-y-2">
                            <div class="bg-slate-900/90 border border-slate-700 px-3 py-2 rounded-lg shadow">
                                <span class="font-bold text-indigo-200 text-sm block">${st.id}. ${st.name}</span>
                            </div>
                            <div class="space-y-1">
                                ${trains.map(t => `
                                    <div class="inline-block bg-slate-900 border border-emerald-500/60 rounded px-2 py-1 text-xs shadow-lg animate-pulse">
                                        <span class="font-bold text-emerald-300">${t.trainNum} (${t.type})</span>
                                    </div>
                                `).join('')}
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
        }

        function initTimetableDropdowns() {
            const stSelect = document.getElementById('tt-station');
            if(!stSelect) return;
            stSelect.innerHTML = allStationsMaster.map(st => `<option value="${st.id}">${st.id}. ${st.name}</option>`).join('');
        }

        function renderTimetable() {
            const stId = document.getElementById('tt-station').value;
            const container = document.getElementById('timetable-container');
            const stObj = allStationsMaster.find(s => s.id === stId);
            container.innerHTML = `
                <div class="mb-3 text-xs text-indigo-300 font-bold">【${stObj ? stObj.name : stId}駅】 発車時刻表</div>
                <table class="w-full text-left border-collapse text-sm">
                    <thead><tr class="border-b border-slate-700 text-indigo-200 text-xs"><th class="p-2">時</th><th class="p-2">分・列車種別</th></tr></thead>
                    <tbody class="text-slate-300">
                        <tr class="border-b border-slate-800"><td class="p-2 font-mono font-bold text-indigo-400">07</td><td class="p-2">05(特急) 18(快速) 32(普通) 45(急行)</td></tr>
                        <tr class="border-b border-slate-800"><td class="p-2 font-mono font-bold text-indigo-400">08</td><td class="p-2">02(特急) 15(普通) 30(快速) 48(通勤急行)</td></tr>
                    </tbody>
                </table>
            `;
        }

        function updateLiveDateTime() {
            const now = new Date();
            const yyyy = now.getFullYear();
            const mm = String(now.getMonth() + 1).padStart(2, '0');
            const dd = String(now.getDate()).padStart(2, '0');
            const days = ['日', '月', '火', '水', '木', '金', '土'];
            const dayOfWeek = days[now.getDay()];
            const hours = String(now.getHours()).padStart(2, '0');
            const minutes = String(now.getMinutes()).padStart(2, '0');
            const seconds = String(now.getSeconds()).padStart(2, '0');
            const el = document.getElementById('live-datetime');
            if(el) el.innerText = `${yyyy}/${mm}/${dd}(${dayOfWeek}) ${hours}:${minutes}:${seconds}`;
            checkEventDayStatus();
        }

        window.onload = function() {
            initFirebase();
            updateAddStationDropdowns();
            updateConsistNumbers();
            initTimetableDropdowns();
            setInterval(updateLiveDateTime, 1000);
            updateLiveDateTime();
        };
    </script>
</body>
</html>
