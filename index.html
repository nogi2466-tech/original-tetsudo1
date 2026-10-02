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

            <!-- ハンバーガーメニューボタン -->
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

        <!-- 3. 走行位置（エレサイト風縦型マップ） -->
        <div id="tab-operation" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">リアルタイム運行状況マップ（エレサイト風）</h2>
            
            <div class="flex flex-wrap gap-2 bg-slate-800 p-3 rounded-xl border border-slate-700">
                <button onclick="switchLine('main')" class="line-btn px-4 py-2 rounded-lg text-xs font-bold transition bg-indigo-600 text-white shadow" data-line="main">紫雲本線 (1〜30)</button>
                <button onclick="switchLine('branch')" class="line-btn px-4 py-2 rounded-lg text-xs font-bold transition bg-slate-900 text-slate-300 hover:bg-slate-700 border border-slate-700" data-line="branch">句守支線 (41〜55)</button>
                <button onclick="switchLine('sight')" class="line-btn px-4 py-2 rounded-lg text-xs font-bold transition bg-slate-900 text-slate-300 hover:bg-slate-700 border border-slate-700" data-line="sight">紫霞観光線 (56〜60)</button>
                <button onclick="switchLine('plateau')" class="line-btn px-4 py-2 rounded-lg text-xs font-bold transition bg-slate-900 text-slate-300 hover:bg-slate-700 border border-slate-700" data-line="plateau">星句高原線 (31〜40)</button>
            </div>

            <div class="bg-slate-800 p-6 rounded-xl border border-slate-700 relative overflow-y-auto max-h-[650px] shadow-xl">
                <div id="line-title-banner" class="text-xs text-indigo-300 mb-6 text-center font-bold">紫雲本線 運行モニター（全30駅）</div>
                <div class="relative max-w-lg mx-auto py-6">
                    <div class="absolute left-1/2 transform -translate-x-1/2 top-0 bottom-0 w-2 bg-gradient-to-b from-indigo-500 via-sky-500 to-indigo-600 rounded-full"></div>
                    <div id="route-map-stations" class="space-y-8 relative z-10">
                        <!-- 動的生成 -->
                    </div>
                </div>
            </div>
        </div>

        <!-- 4. 列車情報一覧 -->
        <div id="tab-traininfo" class="tab-content space-y-4">
            <div class="flex justify-between items-center">
                <h2 class="text-xl font-bold text-indigo-200">列車情報一覧（リアルタイムステータス）</h2>
                <button onclick="switchTab('addtrain')" class="bg-indigo-600 hover:bg-indigo-500 text-white px-3 py-1.5 rounded text-xs font-medium transition shadow">+ 新規列車追加へ</button>
            </div>
            <div class="bg-slate-800 rounded-xl border border-slate-700 overflow-x-auto shadow-xl">
                <table class="w-full text-left border-collapse">
                    <thead>
                        <tr class="border-b border-slate-700 text-indigo-300 text-xs bg-slate-900/60">
                            <th class="p-3">列車番号</th>
                            <th class="p-3">運用番号</th>
                            <th class="p-3">種別</th>
                            <th class="p-3">行き先</th>
                            <th class="p-3">両数</th>
                            <th class="p-3">状態</th>
                            <th class="p-3">編成 / 詳細</th>
                        </tr>
                    </thead>
                    <tbody id="train-table-body" class="text-sm text-slate-200 divide-y divide-slate-700/50">
                        <!-- 動的生成 -->
                    </tbody>
                </table>
            </div>
        </div>

        <!-- 5. 列車追加（拡張機能搭載） -->
        <div id="tab-addtrain" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">新規列車運用・詳細設定追加</h2>
            <div class="bg-slate-800 p-6 rounded-xl border border-slate-700 max-w-3xl space-y-5 shadow-xl">
                
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
                        <label class="block text-xs text-slate-400 mb-1">種別</label>
                        <select id="add-train-type" onchange="onRouteOrTypeChanged()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                            <option>特急</option>
                            <option>通勤急行</option>
                            <option>急行</option>
                            <option>通勤快速</option>
                            <option>快速</option>
                            <option>準急</option>
                            <option>普通</option>
                        </select>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">配属路線</label>
                        <select id="add-train-line" onchange="updateStationDropdowns()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                            <option value="main">紫雲本線</option>
                            <option value="branch">句守支線</option>
                            <option value="sight">紫霞観光線</option>
                            <option value="plateau">星句高原線</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">始点駅</label>
                        <select id="add-start-station" onchange="onRouteOrTypeChanged()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                            <!-- 動的 -->
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">終点駅（行き先）</label>
                        <select id="add-end-station" onchange="onRouteOrTypeChanged()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                            <!-- 動的 -->
                        </select>
                    </div>
                </div>

                <!-- 連結編成設定 -->
                <div class="space-y-3 bg-slate-900/60 p-4 rounded-xl border border-slate-700">
                    <label class="flex items-center space-x-2 cursor-pointer">
                        <input type="checkbox" id="add-coupled-check" onchange="toggleCoupledSection()" class="rounded bg-slate-900 border-slate-700 text-indigo-600 focus:ring-indigo-500 w-4 h-4">
                        <span class="text-sm font-bold text-indigo-300">連結編成にする（前・後ろ2編成併結）</span>
                    </label>
                    <div id="coupled-section-container" class="hidden grid grid-cols-1 md:grid-cols-2 gap-4 pt-2">
                        <div class="bg-slate-900 p-3 rounded border border-slate-800 space-y-2">
                            <h4 class="text-xs font-bold text-indigo-400">前の編成情報</h4>
                            <div>
                                <label class="block text-[10px] text-slate-400 mb-1">車両形式</label>
                                <select id="add-front-series" class="w-full bg-slate-950 border border-slate-700 rounded px-2 py-1.5 text-xs text-white">
                                    <option>S100系 (特急型)</option>
                                    <option>S1系</option>
                                    <option>S2系</option>
                                    <option>S3系</option>
                                    <option>S4系</option>
                                    <option>S5系</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-[10px] text-slate-400 mb-1">両数</label>
                                <select id="add-front-cars" class="w-full bg-slate-950 border border-slate-700 rounded px-2 py-1.5 text-xs text-white">
                                    <option value="6">6両</option>
                                    <option value="4">4両</option>
                                    <option value="2">2両</option>
                                </select>
                            </div>
                        </div>
                        <div class="bg-slate-900 p-3 rounded border border-slate-800 space-y-2">
                            <h4 class="text-xs font-bold text-indigo-400">後ろの編成情報</h4>
                            <div>
                                <label class="block text-[10px] text-slate-400 mb-1">車両形式</label>
                                <select id="add-rear-series" class="w-full bg-slate-950 border border-slate-700 rounded px-2 py-1.5 text-xs text-white">
                                    <option>S1系</option>
                                    <option>S2系</option>
                                    <option>S3系</option>
                                    <option>S4系</option>
                                    <option>S5系</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-[10px] text-slate-400 mb-1">両数</label>
                                <select id="add-rear-cars" class="w-full bg-slate-950 border border-slate-700 rounded px-2 py-1.5 text-xs text-white">
                                    <option value="4">4両</option>
                                    <option value="6">6両</option>
                                    <option value="2">2両</option>
                                </select>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- 併結分割作業設定（詳細拡張） -->
                <div class="space-y-3 bg-slate-900/60 p-4 rounded-xl border border-slate-700">
                    <label class="flex items-center space-x-2 cursor-pointer">
                        <input type="checkbox" id="add-coupling-work-check" onchange="toggleCouplingWorkSection()" class="rounded bg-slate-900 border-slate-700 text-indigo-600 focus:ring-indigo-500 w-4 h-4">
                        <span class="text-sm font-bold text-indigo-300">併結分割作業を行う</span>
                    </label>
                    <div id="coupling-work-container" class="hidden space-y-3 pt-2">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs text-slate-400 mb-1">作業実施駅名</label>
                                <select id="add-coupling-station" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                                    <!-- 動的 -->
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs text-slate-400 mb-1">作業種類</label>
                                <select id="add-coupling-type" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                                    <option value="couple">併結（後ろに列車を連結）</option>
                                    <option value="split">分割（後ろの列車を切り離し）</option>
                                </select>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- 種別変更設定（詳細拡張） -->
                <div class="space-y-3 bg-slate-900/60 p-4 rounded-xl border border-slate-700">
                    <label class="flex items-center space-x-2 cursor-pointer">
                        <input type="checkbox" id="add-type-change-check" onchange="toggleTypeChangeSection()" class="rounded bg-slate-900 border-slate-700 text-indigo-600 focus:ring-indigo-500 w-4 h-4">
                        <span class="text-sm font-bold text-indigo-300">途中で種別・列車番号を変更する</span>
                    </label>
                    <div id="type-change-container" class="hidden grid grid-cols-1 md:grid-cols-3 gap-4 pt-2">
                        <div>
                            <label class="block text-xs text-slate-400 mb-1">変更駅名</label>
                            <select id="add-type-change-station" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                                <!-- 動的 -->
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs text-slate-400 mb-1">変更後の種別</label>
                            <select id="add-new-type" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                                <option>普通</option>
                                <option>準急</option>
                                <option>快速</option>
                                <option>急行</option>
                                <option>特急</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs text-slate-400 mb-1">変更後の列車番号</label>
                            <input type="text" id="add-new-train-num" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white" value="103C">
                        </div>
                    </div>
                </div>

                <!-- 停車駅・時刻設定連動セクション -->
                <div class="space-y-2 border-t border-slate-700 pt-4">
                    <h3 class="text-sm font-bold text-indigo-300">区間停車駅ごとの到着・発車時刻設定（種別自動切替反映）</h3>
                    <p class="text-xs text-slate-400">変更駅までは変更前の種別停車駅、変更駅からは変更後の種別停車駅が自動表示されます。</p>
                    <div id="stops-time-container" class="space-y-2 max-h-60 overflow-y-auto pr-2 bg-slate-900 p-3 rounded-lg border border-slate-700">
                        <!-- 動的生成 -->
                    </div>
                </div>

                <button onclick="addNewTrain()" class="w-full bg-indigo-600 hover:bg-indigo-500 text-white font-medium py-3 rounded-lg transition shadow-lg text-base">新規列車を登録する (クラウド自動同期)</button>
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
                    isMajor: (i === 0 || i === 2 || i === 12 || i === 29),
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
                        isMajor: (i === 0 || i === 14),
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
                        isMajor: (i === 0 || i === 4),
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
                        isMajor: (i === 0 || i === 9),
                        facilities: ["山岳対応"]
                    };
                })
            }
        };

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
            if(menu.classList.contains('absolute')) {
                toggleMenu();
            }

            if (tabId === 'dia') drawDiagram();
            if (tabId === 'operation') renderRouteMap();
            if (tabId === 'timetable') initTimetableDropdowns();
            if (tabId === 'traininfo') renderTrainTable();
            if (tabId === 'addtrain') updateStationDropdowns();
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

        let appData = {
            trains: [
                { 
                    id: 1, trainNum: "101M", opNum: "A01", type: "特急", series1: "S100系 (特急型)", cars: "10", 
                    line: "main", startSt: "01", endSt: "30", endName: "紫句守展示場", status: "走行中", 
                    isCoupled: false, frontSeries: "S100系 (特急型)", frontCars: "6", rearSeries: "", rearCars: "",
                    hasCouplingWork: false, couplingSt: "", couplingType: "",
                    hasTypeChange: false, changeSt: "", newType: "", newTrainNum: "",
                    stationId: "12", direction: "down" 
                }
            ]
        };

        let dbRef = null;
        const DEFAULT_FB_URL = "https://original-tetsudo-430ac-default-rtdb.firebaseio.com";

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
            renderTrainTable();
            renderConsist();
            initTimetableDropdowns();
        }

        function renderRouteMap() {
            const container = document.getElementById('route-map-stations');
            const titleBanner = document.getElementById('line-title-banner');
            if(!container) return;

            const lineInfo = linesData[currentActiveLine];
            titleBanner.innerText = `${lineInfo.name} 運行モニター（全${lineInfo.stations.length}駅）`;

            container.innerHTML = lineInfo.stations.map((st) => {
                const downTrains = appData.trains.filter(t => t.line === currentActiveLine && t.stationId === st.id && t.direction === 'down');
                const upTrains = appData.trains.filter(t => t.line === currentActiveLine && t.stationId === st.id && t.direction === 'up');

                return `
                    <div class="relative flex items-center justify-between">
                        <div class="w-5/12 pr-4 text-right space-y-1">
                            ${upTrains.map(t => `
                                <div class="inline-block bg-slate-900 border border-sky-500/60 rounded px-2 py-1 text-xs shadow-lg animate-pulse">
                                    <span class="font-bold text-sky-300">${t.trainNum} (${t.type})</span>
                                    <span class="text-[10px] text-slate-400 block">${t.frontSeries}</span>
                                </div>
                            `).join('')}
                        </div>

                        <div class="absolute left-1/2 transform -translate-x-1/2 flex items-center justify-center">
                            <div class="w-6 h-6 rounded-full bg-slate-900 border-4 border-indigo-500 shadow flex items-center justify-center z-20">
                                <div class="w-2 h-2 rounded-full bg-white"></div>
                            </div>
                        </div>

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
                                        <span class="font-bold text-rose-300">${t.trainNum} (${t.type})</span>
                                        <span class="text-[10px] text-slate-400 block">${t.frontSeries}</span>
                                    </div>
                                `).join('')}
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
        }

        function renderTrainTable() {
            const tbody = document.getElementById('train-table-body');
            if(!tbody) return;

            if(!appData.trains || appData.trains.length === 0) {
                tbody.innerHTML = `<tr><td colspan="7" class="p-4 text-center text-slate-400">登録されている列車はありません。「列車追加」から登録してください。</td></tr>`;
                return;
            }

            tbody.innerHTML = appData.trains.map(t => {
                const lineObj = linesData[t.line];
                const lineName = lineObj ? lineObj.name : "不明路線";
                const stObj = lineObj ? lineObj.stations.find(s => s.id === t.stationId) : null;
                const currentLocStr = stObj ? stObj.name : t.stationId;

                let consistStr = t.frontSeries + (t.isCoupled ? ` + ${t.rearSeries}` : '');

                return `
                    <tr class="border-b border-slate-800 hover:bg-slate-900/40 transition">
                        <td class="p-3 font-mono font-bold text-indigo-300">${t.trainNum}</td>
                        <td class="p-3 font-mono text-slate-300">${t.opNum}</td>
                        <td class="p-3"><span class="bg-indigo-900/60 text-indigo-200 px-2 py-0.5 rounded text-xs border border-indigo-700">${t.type}</span></td>
                        <td class="p-3">${t.endName || '未設定'}</td>
                        <td class="p-3">${t.cars || '8'}両</td>
                        <td class="p-3">
                            <span class="px-2.5 py-1 rounded-full text-xs border bg-emerald-950 text-emerald-300 border-emerald-700 animate-pulse">${t.status}</span>
                            <span class="text-[10px] text-slate-400 block mt-1">現在: ${currentLocStr}</span>
                        </td>
                        <td class="p-3 text-xs text-slate-400">
                            <div>編成: ${consistStr}</div>
                            <div>路線: ${lineName}</div>
                        </td>
                    </tr>
                `;
            }).join('');
        }

        function updateStationDropdowns() {
            const lineKey = document.getElementById('add-train-line').value;
            const startSel = document.getElementById('add-start-station');
            const endSel = document.getElementById('add-end-station');
            const couplingStSel = document.getElementById('add-coupling-station');
            const typeChangeStSel = document.getElementById('add-type-change-station');
            const stations = linesData[lineKey].stations;

            const optionsHtml = stations.map(st => `<option value="${st.id}">${st.id}. ${st.name}</option>`).join('');
            startSel.innerHTML = optionsHtml;
            endSel.innerHTML = optionsHtml;
            if(couplingStSel) couplingStSel.innerHTML = optionsHtml;
            if(typeChangeStSel) typeChangeStSel.innerHTML = optionsHtml;

            if(stations.length > 1) {
                endSel.selectedIndex = stations.length - 1;
            }
            onRouteOrTypeChanged();
        }

        function toggleCoupledSection() {
            const isChecked = document.getElementById('add-coupled-check').checked;
            const container = document.getElementById('coupled-section-container');
            if(isChecked) {
                container.classList.remove('hidden');
            } else {
                container.classList.add('hidden');
            }
        }

        function toggleCouplingWorkSection() {
            const isChecked = document.getElementById('add-coupling-work-check').checked;
            const container = document.getElementById('coupling-work-container');
            if(isChecked) {
                container.classList.remove('hidden');
            } else {
                container.classList.add('hidden');
            }
        }

        function toggleTypeChangeSection() {
            const isChecked = document.getElementById('add-type-change-check').checked;
            const container = document.getElementById('type-change-container');
            if(isChecked) {
                container.classList.remove('hidden');
            } else {
                container.classList.add('hidden');
            }
            onRouteOrTypeChanged();
        }

        function onRouteOrTypeChanged() {
            const lineKey = document.getElementById('add-train-line').value;
            const startId = document.getElementById('add-start-station').value;
            const endId = document.getElementById('add-end-station').value;
            const hasTypeChange = document.getElementById('add-type-change-check').checked;
            const changeStId = document.getElementById('add-type-change-station').value;
            const initialType = document.getElementById('add-train-type').value;
            const newType = document.getElementById('add-new-type').value;

            const stopsContainer = document.getElementById('stops-time-container');
            const stations = linesData[lineKey].stations;

            const startIndex = stations.findIndex(s => s.id === startId);
            const endIndex = stations.findIndex(s => s.id === endId);

            if(startIndex === -1 || endIndex === -1) {
                stopsContainer.innerHTML = `<p class="text-xs text-slate-400">有効な始点駅と終点駅を選択してください。</p>`;
                return;
            }

            const step = startIndex <= endIndex ? 1 : -1;
            let activeStops = [];
            for(let i = startIndex; ; i += step) {
                activeStops.push(stations[i]);
                if(i === endIndex) break;
            }

            const changeIndex = activeStops.findIndex(s => s.id === changeStId);

            stopsContainer.innerHTML = activeStops.map((st, idx) => {
                let currentTypeForSt = initialType;
                if(hasTypeChange && changeIndex !== -1) {
                    if(startIndex <= endIndex) {
                        if(idx >= changeIndex) currentTypeForSt = newType;
                    } else {
                        if(idx <= changeIndex) currentTypeForSt = newType;
                    }
                }

                const isStopRequired = (st.isMajor || currentTypeForSt === '普通' || idx === 0 || idx === activeStops.length - 1);
                if(!isStopRequired && currentTypeForSt === '特急') return '';

                const defaultArr = `08:${String(10 + idx * 4).padStart(2, '0')}`;
                const defaultDep = `08:${String(12 + idx * 4).padStart(2, '0')}`;

                return `
                    <div class="flex items-center justify-between bg-slate-800 p-2 rounded border border-slate-700 text-xs gap-2">
                        <div class="font-bold text-indigo-200 w-1/3">
                            ${st.id}. ${st.name} 
                            <span class="text-[10px] text-indigo-400 bg-indigo-950 px-1.5 py-0.5 rounded ml-1">${currentTypeForSt}</span>
                        </div>
                        <div class="flex items-center gap-1 w-2/3 justify-end">
                            <span class="text-slate-400">着</span>
                            <input type="time" value="${defaultArr}" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-white text-xs font-mono stop-arr" data-stid="${st.id}">
                            <span class="text-slate-400 ml-2">発</span>
                            <input type="time" value="${defaultDep}" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-white text-xs font-mono stop-dep" data-stid="${st.id}">
                        </div>
                    </div>
                `;
            }).join('');
        }

        function addNewTrain() {
            const trainNum = document.getElementById('add-train-num').value.trim();
            const opNum = document.getElementById('add-op-num').value.trim();
            const type = document.getElementById('add-train-type').value;
            const line = document.getElementById('add-train-line').value;
            const startSt = document.getElementById('add-start-station').value;
            const endSt = document.getElementById('add-end-station').value;
            
            const isCoupled = document.getElementById('add-coupled-check').checked;
            const frontSeries = isCoupled ? document.getElementById('add-front-series').value : document.getElementById('add-front-series').value;
            const frontCars = isCoupled ? document.getElementById('add-front-cars').value : "6";
            const rearSeries = isCoupled ? document.getElementById('add-rear-series').value : "";
            const rearCars = isCoupled ? document.getElementById('add-rear-cars').value : "";
            const totalCars = isCoupled ? (parseInt(frontCars) + parseInt(rearCars)) : parseInt(frontCars);

            const hasCouplingWork = document.getElementById('add-coupling-work-check').checked;
            const couplingSt = hasCouplingWork ? document.getElementById('add-coupling-station').value : "";
            const couplingType = hasCouplingWork ? document.getElementById('add-coupling-type').value : "";

            const hasTypeChange = document.getElementById('add-type-change-check').checked;
            const changeSt = hasTypeChange ? document.getElementById('add-type-change-station').value : "";
            const newType = hasTypeChange ? document.getElementById('add-new-type').value : "";
            const newTrainNum = hasTypeChange ? document.getElementById('add-new-train-num').value : "";

            if(!trainNum) {
                alert("列車番号を入力してください。");
                return;
            }

            const lineObj = linesData[line];
            const endStObj = lineObj.stations.find(s => s.id === endSt);
            const endName = endStObj ? endStObj.name : "終点";

            const newTrain = {
                id: Date.now(),
                trainNum,
                opNum,
                type,
                line,
                startSt,
                endSt,
                endName,
                status: "運行前",
                isCoupled,
                frontSeries,
                frontCars,
                rearSeries,
                rearCars,
                cars: String(totalCars),
                hasCouplingWork,
                couplingSt,
                couplingType,
                hasTypeChange,
                changeSt,
                newType,
                newTrainNum,
                stationId: startSt,
                direction: startSt <= endSt ? "down" : "up"
            };

            appData.trains.push(newTrain);
            pushData();
            updateUI();
            alert("新規列車を登録し、クラウド同期しました！");
            switchTab('traininfo');
        }

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

            container.innerHTML = `
                <div class="mb-3 text-xs text-indigo-300 font-bold">
                    【${foundStName} (${stId})】 発車時刻表
                </div>
                <table class="w-full text-left border-collapse">
                    <thead>
                        <tr class="border-b border-slate-700 text-indigo-200 text-xs">
                            <th class="p-2">時</th>
                            <th class="p-2">分 (特急 / 急行 / 快速 / 普通)</th>
                        </tr>
                    </thead>
                    <tbody class="text-slate-300 text-sm">
                        <tr class="border-b border-slate-800"><td class="p-2 font-mono font-bold text-indigo-400">07</td><td class="p-2">05<span class="text-xs text-slate-500">(特)</span> 18<span class="text-xs text-slate-500">(急)</span> 32<span class="text-xs text-slate-500">(普)</span> 45<span class="text-xs text-slate-500">(快)</span></td></tr>
                        <tr class="border-b border-slate-800"><td class="p-2 font-mono font-bold text-indigo-400">08</td><td class="p-2">00<span class="text-xs text-slate-500">(特)</span> 12<span class="text-xs text-slate-500">(普)</span> 28<span class="text-xs text-slate-500">(急)</span> 44<span class="text-xs text-slate-500">(快)</span></td></tr>
                    </tbody>
                </table>
            `;
        }

        function renderConsist() {
            const list = document.getElementById('consist-list');
            if(!list) return;
            list.innerHTML = appData.trains.map(t => `
                <div class="bg-slate-800 p-4 rounded-xl border border-slate-700 space-y-2">
                    <h3 class="font-bold text-indigo-300">${t.trainNum}列車 (${t.type}) - ${t.cars}両編成</h3>
                    <div class="text-xs text-slate-300">前の編成: ${t.frontSeries} (${t.frontCars}両) ${t.isCoupled ? `／ 後ろの編成: ${t.rearSeries} (${t.rearCars}両)` : ''}</div>
                    ${t.hasCouplingWork ? `<div class="text-xs text-amber-400">※ 実施駅 ${t.couplingSt} にて ${t.couplingType === 'couple' ? '併結作業あり' : '分割作業あり'}</div>` : ''}
                    ${t.hasTypeChange ? `<div class="text-xs text-sky-400">※ 変更駅 ${t.changeSt} にて ${t.newType} (${t.newTrainNum}) に種別変更</div>` : ''}
                </div>
            `).join('');
        }

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
        }

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

        window.onload = function() {
            initFirebase();
            updateStationDropdowns();
            initTimetableDropdowns();
            setInterval(updateLiveDateTime, 1000);
            updateLiveDateTime();
        };
    </script>
</body>
</html>
