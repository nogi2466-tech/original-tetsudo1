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

        <!-- 5. 列車追加 (停車駅・時間設定プレビュー付き) -->
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
            {id: "1"
