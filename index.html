<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>紫句守鉄道（しのもり鉄道）総合運行管理システム ＆ 走行位置</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Firebase SDK (Compat) -->
    <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>
    <style>
        body { font-family: system-ui, -apple-system, sans-serif; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }

        /* 走行位置用スタイル（全駅縦方向配置・中央線） */
        .track-container-vertical {
            position: relative;
            min-height: 1400px;
            background: #1e293b;
            border-radius: 8px;
            padding: 40px 20px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            overflow: hidden;
            border: 1px solid #334155;
            display: flex;
            justify-content: center;
        }
        .vertical-rail {
            position: absolute;
            top: 40px;
            bottom: 40px;
            left: 50%;
            width: 4px;
            background: #64748b;
            transform: translateX(-50%);
        }
        .v-station-node {
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            align-items: center;
            width: 100%;
            pointer-events: none;
        }
        .v-station-dot {
            position: absolute;
            left: 50%;
            width: 12px;
            height: 12px;
            background: #94a3b8;
            border-radius: 50%;
            transform: translateX(-50%);
            z-index: 2;
        }
        .v-station-label {
            position: absolute;
            left: calc(50% + 20px);
            font-size: 0.8rem;
            color: #cbd5e1;
            white-space: nowrap;
        }
        
        /* 列車配置スタイル（左右分離 ＆ 改良された見やすいアイコン） */
        .v-train {
            position: absolute;
            transform: translateY(-50%);
            cursor: pointer;
            z-index: 10;
            transition: top 0.6s ease-in-out;
            display: flex;
            align-items: center;
            gap: 6px;
        }
        .v-train.up-train {
            right: calc(50% + 25px); /* 中央線の左側（上り） */
            flex-direction: row-reverse;
            text-align: right;
        }
        .v-train.down-train {
            left: calc(50% + 25px); /* 中央線の右側（下り） */
            flex-direction: row;
            text-align: left;
        }

        /* 列車アイコンのブラッシュアップデザイン */
        .train-icon-badge {
            background: linear-gradient(135deg, #3b82f6, #1d4ed8);
            color: white;
            padding: 4px 8px;
            border-radius: 6px;
            font-size: 11px;
            font-weight: bold;
            box-shadow: 0 3px 6px rgba(0,0,0,0.3);
            border: 1px solid #60a5fa;
            white-space: nowrap;
            display: flex;
            align-items: center;
            gap: 4px;
        }
        .train-icon-badge.type-特急 { background: linear-gradient(135deg, #ef4444, #991b1b); border-color: #fca5a5; }
        .train-icon-badge.type-急行 { background: linear-gradient(135deg, #f97316, #c2410c); border-color: #fdba74; }
        .train-icon-badge.type-快速 { background: linear-gradient(135deg, #eab308, #a16207); border-color: #fde047; color: #1e293b; }
        .train-icon-badge.type-準急 { background: linear-gradient(135deg, #10b981, #047857); border-color: #6ee7b7; }

        .train-card-v {
            background: rgba(15, 23, 42, 0.9);
            color: white;
            border-radius: 6px;
            padding: 4px 8px;
            font-size: 10px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.4);
            border: 1px solid #475569;
            backdrop-filter: blur(4px);
            min-width: 100px;
        }
        .train-num-top {
            font-size: 9px;
            font-weight: bold;
            color: #93c5fd;
            white-space: nowrap;
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen flex flex-col relative">

    <!-- ヘッダー -->
    <header class="bg-indigo-950 border-b border-indigo-800 p-4 shadow-lg flex justify-between items-center gap-4 relative z-50">
        <div class="flex items-center space-x-3">
            <span class="text-3xl">🚄</span>
            <div>
                <h1 class="text-xl font-bold tracking-wider text-indigo-200">紫句守鉄道 <span class="text-xs font-normal text-indigo-400">しのもり鉄道</span></h1>
                <p class="text-xs text-slate-400">総合運行管理システム</p>
            </div>
        </div>

        <div class="flex items-center gap-3">
            <div id="live-datetime" class="hidden sm:block bg-slate-900 px-3 py-1.5 rounded-lg border border-slate-700 text-xs font-mono text-indigo-300">
                2026/10/04(日) 00:00:00
            </div>
            <div id="event-banner-badge" class="hidden bg-rose-900 text-rose-200 px-2.5 py-1 rounded-lg text-xs font-bold border border-rose-700 animate-pulse">
                🎉 イベント日ダイヤ
            </div>
            <div class="hidden md:flex items-center space-x-2 bg-slate-900 px-3 py-1.5 rounded-lg border border-slate-700 text-xs">
                <span id="sync-status-dot" class="w-2.5 h-2.5 rounded-full bg-amber-500 animate-pulse"></span>
                <span id="sync-status-text">同期待機中</span>
            </div>
            <button onclick="toggleMenu()" class="bg-indigo-900 hover:bg-indigo-800 border border-indigo-700 p-2 rounded-lg text-white md:hidden transition flex items-center justify-center w-10 h-10 shadow">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path>
                </svg>
            </button>
        </div>
    </header>

    <!-- ナビゲーションタブ -->
    <nav id="nav-menu" class="hidden md:flex bg-slate-900/95 border-b border-slate-800 px-4 py-2 overflow-x-auto space-x-1 sticky top-0 z-40 backdrop-blur shadow-md">
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
    <main class="flex-1 p-4 md:p-6 max-w-7xl mx-auto w-full relative z-10">

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
                    <h3 class="text-indigo-400 font-bold border-b border-slate-700 pb-2">路線データ</h3>
                    <ul class="text-sm text-slate-300 space-y-2">
                        <li><span class="text-slate-400 inline-block w-28">紫雲本線</span> 1～30</li>
                        <li><span class="text-slate-400 inline-block w-28">句守支線</span> 41～55</li>
                        <li><span class="text-slate-400 inline-block w-28">紫霞観光線</span> 56～60</li>
                        <li><span class="text-slate-400 inline-block w-28">星句高原線</span> 31～40</li>
                        <li><span class="text-slate-400 inline-block w-28">直通路線</span> 09～1</li>
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
                <p class="text-slate-400">条件を選択して「時刻表を表示」してください。</p>
            </div>
        </div>

        <!-- 3. 走行位置 -->
        <div id="tab-operation" class="tab-content space-y-4">
            <div class="flex flex-wrap items-center justify-between gap-2">
                <h2 class="text-xl font-bold text-indigo-200">列車走行位置（全駅表示・上下分離リアルタイム）</h2>
                <div class="flex items-center gap-3">
                    <button onclick="toggleAutoMove()" id="auto-move-btn" class="bg-emerald-600 hover:bg-emerald-500 text-white px-3 py-1.5 rounded text-xs font-bold transition shadow">▶ 自動運行シミュレーション開始</button>
                </div>
            </div>
            
            <div class="controls bg-slate-800 p-4 rounded-xl border border-slate-700 flex flex-wrap items-center justify-between gap-4">
                <div class="flex items-center space-x-4">
                    <label class="flex items-center space-x-2 text-sm text-slate-200 cursor-pointer">
                        <input type="checkbox" id="toggleView" onchange="renderOperationTrack()" class="rounded bg-slate-900 border-slate-700 text-indigo-600" checked>
                        <span>詳細表示（情報カード付き）</span>
                    </label>
                </div>
                <div class="text-xs text-indigo-300">
                    📍 中央線を境に <strong>左側：上り列車</strong> ／ <strong>右側：下り列車</strong> （クリックで詳細ポップアップ表示）
                </div>
            </div>

            <div id="track-container-parent" class="track-container-vertical">
                <div class="vertical-rail"></div>
                <!-- 動的に全駅と列車が描画されます -->
            </div>

            <div class="status-panel bg-slate-800 p-4 rounded-xl border border-slate-700 flex justify-between items-center">
                <p class="text-sm text-slate-300"><strong>運行状況モニタリング:</strong> <span id="statusText" class="text-indigo-300 font-bold">全線正常運行中（登録列車を自動配置）</span></p>
                <button onclick="renderOperationTrack()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-3 py-1.5 rounded text-xs transition">位置を更新</button>
            </div>
        </div>

        <!-- 4. 列車情報 -->
        <div id="tab-traininfo" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">列車情報一覧</h2>
            <div class="bg-slate-800 rounded-xl border border-slate-700 overflow-hidden shadow-xl">
                <div class="overflow-x-auto">
                    <table class="w-full text-left text-xs">
                        <thead class="bg-slate-900 text-indigo-200 border-b border-slate-700">
                            <tr>
                                <th class="p-3">列車番号</th>
                                <th class="p-3">運用番号</th>
                                <th class="p-3">種別・区間ルール</th>
                                <th class="p-3">行き先</th>
                                <th class="p-3">両数・編成</th>
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
                <div class="border border-slate-700 p-4 rounded-lg bg-slate-900 space-y-3">
                    <div class="flex justify-between items-center">
                        <span class="text-xs text-indigo-300 font-bold">列車番号・運用番号の設定モード</span>
                        <select id="num-mode" onchange="toggleNumMode()" class="bg-slate-800 border border-slate-700 rounded px-2 py-1 text-xs text-white">
                            <option value="common">共通設定（前後共通）</option>
                            <option value="split">個別に分ける（前部・後部別）</option>
                        </select>
                    </div>

                    <div id="box-num-common" class="grid grid-cols-1 md:grid-cols-2 gap-4 pt-2">
                        <div>
                            <label class="block text-xs text-slate-400 mb-1">列車番号</label>
                            <input type="text" id="add-train-num" class="w-full bg-slate-800 border border-slate-700 rounded px-3 py-2 text-sm text-white" value="101M">
                        </div>
                        <div>
                            <label class="block text-xs text-slate-400 mb-1">運用番号</label>
                            <input type="text" id="add-op-num" class="w-full bg-slate-800 border border-slate-700 rounded px-3 py-2 text-sm text-white" value="A01">
                        </div>
                    </div>

                    <div id="box-num-split" class="grid grid-cols-1 md:grid-cols-2 gap-4 pt-2 hidden">
                        <div class="space-y-2 bg-slate-800/60 p-3 rounded border border-slate-700">
                            <span class="text-[11px] text-indigo-300 font-bold block">【前方列車（本務列車）】番号</span>
                            <div class="grid grid-cols-2 gap-2">
                                <input type="text" id="front-train-num" placeholder="列車番号" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white" value="101M">
                                <input type="text" id="front-op-num" placeholder="運用番号" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white" value="A01">
                            </div>
                        </div>
                        <div class="space-y-2 bg-slate-800/60 p-3 rounded border border-slate-700">
                            <span class="text-[11px] text-sky-300 font-bold block">【後方列車（増結・分割後）】番号</span>
                            <div class="grid grid-cols-2 gap-2">
                                <input type="text" id="rear-train-num" placeholder="列車番号" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white" value="103M">
                                <input type="text" id="rear-op-num" placeholder="運用番号" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white" value="A02">
                            </div>
                        </div>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">基本種別</label>
                        <select id="add-train-type" onchange="updateStationSchedulePreview()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
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
                        <label class="block text-xs text-slate-400 mb-1">運行日設定</label>
                        <select id="add-run-day" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                            <option value="weekday">平日</option>
                            <option value="holiday">土休日</option>
                            <option value="event">イベント日</option>
                            <option value="newyear">年末年始</option>
                        </select>
                    </div>
                </div>

                <div class="border border-slate-700 p-4 rounded-lg bg-slate-900 space-y-3">
                    <div class="flex justify-between items-center">
                        <span class="text-xs text-indigo-300 font-bold">編成構成（両数ルール準拠）</span>
                        <select id="consist-mode" onchange="toggleConsistMode()" class="bg-slate-800 border border-slate-700 rounded px-2 py-1 text-xs text-white">
                            <option value="single">単行編成</option>
                            <option value="double" selected>併結編成（前部 ＋ 後部）</option>
                        </select>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4 pt-2">
                        <div class="space-y-2 bg-slate-800/60 p-3 rounded border border-slate-700">
                            <span class="text-[11px] text-indigo-300 font-bold block">【前部編成】</span>
                            <div class="grid grid-cols-2 gap-2">
                                <select id="consist-series-1" onchange="updateConsistNumbers(1)" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white">
                                    <option value="S1">S1系</option><option value="S2">S2系</option><option value="S3">S3系</option><option value="S4">S4系</option><option value="S5">S5系</option><option value="S100" selected>S100系</option><option value="S900">S900系</option>
                                </select>
                                <select id="consist-number-1" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white"></select>
                            </div>
                        </div>
                        <div id="consist-2-container" class="space-y-2 bg-slate-800/60 p-3 rounded border border-slate-700">
                            <span class="text-[11px] text-sky-300 font-bold block">【後部編成】</span>
                            <div class="grid grid-cols-2 gap-2">
                                <select id="consist-series-2" onchange="updateConsistNumbers(2)" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white">
                                    <option value="S1">S1系</option><option value="S2">S2系</option><option value="S3">S3系</option><option value="S4" selected>S4系</option><option value="S5">S5系</option><option value="S100">S100系</option>
                                </select>
                                <select id="consist-number-2" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white"></select>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="border border-indigo-900/60 bg-indigo-950/20 p-4 rounded-lg space-y-4">
                    <span class="text-xs text-indigo-300 font-bold block">⚙️ 途中駅での種別変更・分割・連結作業設定（チェック式）</span>
                    
                    <div class="space-y-2 bg-slate-900 p-3 rounded border border-slate-700">
                        <label class="flex items-center space-x-2 text-xs text-indigo-200 cursor-pointer font-bold">
                            <input type="checkbox" id="chk-typechange" onchange="toggleActionCheckboxes('type')" class="rounded bg-slate-800 border-slate-600 text-indigo-600 focus:ring-0">
                            <span>途中駅から種別を変更する</span>
                        </label>
                        <div id="box-typechange" class="grid grid-cols-1 md:grid-cols-2 gap-3 pt-2 hidden">
                            <div>
                                <label class="block text-[11px] text-slate-400 mb-1">変更駅</label>
                                <select id="tc-station" onchange="updateStationSchedulePreview()" class="w-full bg-slate-800 border border-slate-700 rounded px-2 py-1 text-xs text-white"></select>
                            </div>
                            <div>
                                <label class="block text-[11px] text-slate-400 mb-1">変更後の種別</label>
                                <select id="tc-newtype" onchange="updateStationSchedulePreview()" class="w-full bg-slate-800 border border-slate-700 rounded px-2 py-1 text-xs text-white">
                                    <option value="普通">普通</option>
                                    <option value="準急">準急</option>
                                    <option value="快速">快速</option>
                                    <option value="急行">急行</option>
                                    <option value="特急">特急</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <div class="space-y-2 bg-slate-900 p-3 rounded border border-slate-700">
                        <label class="flex items-center space-x-2 text-xs text-indigo-200 cursor-pointer font-bold">
                            <input type="checkbox" id="chk-coupling" onchange="toggleActionCheckboxes('coupling')" class="rounded bg-slate-800 border-slate-600 text-indigo-600 focus:ring-0">
                            <span>途中駅で編成の切り離し（分割）または連結を行う</span>
                        </label>
                        <div id="box-coupling" class="grid grid-cols-1 md:grid-cols-3 gap-3 pt-2 hidden">
                            <div>
                                <label class="block text-[11px] text-slate-400 mb-1">作業種別選択</label>
                                <select id="cp-action" onchange="updateStationSchedulePreview()" class="w-full bg-slate-800 border border-slate-700 rounded px-2 py-1 text-xs text-white">
                                    <option value="uncouple">後部編成を切り離し（分割）</option>
                                    <option value="couple">後部編成を連結</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-[11px] text-slate-400 mb-1">作業対象駅</label>
                                <select id="cp-station" onchange="updateStationSchedulePreview()" class="w-full bg-slate-800 border border-slate-700 rounded px-2 py-1 text-xs text-white"></select>
                            </div>
                            <div>
                                <label class="block text-[11px] text-slate-400 mb-1">対象編成 / 番号</label>
                                <input type="text" id="cp-detail" value="S4-01" class="w-full bg-slate-800 border border-slate-700 rounded px-2 py-1 text-xs text-white">
                            </div>
                        </div>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="space-y-2">
                        <label class="block text-xs text-indigo-300 font-bold">始点駅設定</label>
                        <div class="space-y-1">
                            <span id="label-start-1" class="text-[11px] text-slate-400 block">始点駅</span>
                            <select id="add-start-station" onchange="updateStationSchedulePreview()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white"></select>
                        </div>
                        <div id="box-start-rear" class="space-y-1 hidden pt-1">
                            <span class="text-[11px] text-sky-300 block">【後方列車】連結前の始点駅</span>
                            <select id="add-start-station-rear" onchange="updateStationSchedulePreview()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white"></select>
                        </div>
                    </div>

                    <div class="space-y-2">
                        <label class="block text-xs text-indigo-300 font-bold">終点駅（行き先）設定</label>
                        <div class="space-y-1">
                            <span id="label-end-1" class="text-[11px] text-slate-400 block">終点駅（行き先）</span>
                            <select id="add-end-station" onchange="updateStationSchedulePreview()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white"></select>
                        </div>
                        <div id="box-end-rear" class="space-y-1 hidden pt-1">
                            <span class="text-[11px] text-sky-300 block">【後方列車（切り離し後）】終点駅（行き先）</span>
                            <select id="add-end-station-rear" onchange="updateStationSchedulePreview()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white"></select>
                        </div>
                    </div>
                </div>

                <div class="space-y-2">
                    <div class="flex justify-between items-center">
                        <span id="preview-title-label" class="text-xs text-indigo-300 font-bold block">停車駅スケジュール・発着時間</span>
                        <div id="preview-tabs" class="flex gap-1 hidden">
                            <button onclick="switchPreviewSubTab('front')" id="btn-prev-front" class="px-2.5 py-1 rounded text-xs font-bold bg-indigo-600 text-white transition">前方列車 (本務)</button>
                            <button onclick="switchPreviewSubTab('rear')" id="btn-prev-rear" class="px-2.5 py-1 rounded text-xs font-bold bg-slate-800 text-slate-400 transition">後方列車</button>
                        </div>
                    </div>
                    <div id="station-schedule-preview" class="bg-slate-900 border border-slate-700 rounded-lg p-3 max-h-64 overflow-y-auto text-xs space-y-2">
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
            <h2 class="text-xl font-bold text-indigo-200">ダイヤ表（紙の時刻表風マトリクス）</h2>
            <div class="bg-slate-800 p-4 rounded-xl border border-slate-700 overflow-x-auto shadow-xl">
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

    <!-- 列車詳細ポップアップモーダル -->
    <div id="train-modal" class="fixed inset-0 bg-slate-950/80 z-50 flex items-center justify-center hidden backdrop-blur-sm">
        <div class="bg-slate-800 border border-indigo-700 p-6 rounded-2xl w-full max-w-md space-y-4 shadow-2xl relative">
            <div class="flex justify-between items-center border-b border-slate-700 pb-3">
                <div class="flex items-center space-x-2">
                    <span class="text-2xl">🚄</span>
                    <div>
                        <h3 id="modal-train-num" class="text-lg font-bold text-indigo-200">101M</h3>
                        <p id="modal-op-num" class="text-xs text-slate-400 font-mono">運用: A01</p>
                    </div>
                </div>
                <button onclick="closeTrainModal()" class="text-slate-400 hover:text-white text-xl font-bold bg-slate-900 w-8 h-8 rounded-full flex items-center justify-center border border-slate-700">×</button>
            </div>
            <div id="modal-content" class="space-y-3 text-sm text-slate-300">
                <!-- 動的挿入 -->
            </div>
            <div class="pt-2 flex justify-end">
                <button onclick="closeTrainModal()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-4 py-2 rounded-lg text-xs font-medium transition shadow">閉じる</button>
            </div>
        </div>
    </div>

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

        const officialStopsMaster = {
            "特急": [
                ["1", "3", "13", "16", "17", "23", "28", "30"],
                ["1", "3", "13", "16", "17", "23", "28", "35", "40"],
                ["1", "3", "13", "43", "44", "50", "52", "55"],
                ["1", "3", "13", "16", "17", "23", "28", "56", "60"]
            ],
            "通勤急行": [
                ["1", "3", "6", "13", "16", "17", "20", "24", "28", "30"],
                ["1", "3", "6", "13", "43", "48", "50", "52", "55"],
                ["1", "3", "6", "13", "16", "17", "20", "24", "28", "56", "60"]
            ],
            "急行": [
                ["09", "06", "04", "02", "1", "3", "6", "9", "13", "16", "17", "20", "23", "28", "30"],
                ["09", "06", "04", "02", "1", "2", "3", "6", "9", "13", "43", "44", "48", "50", "52", "55"],
                ["09", "06", "04", "02", "1", "3", "6", "9", "13", "16", "17", "20", "23", "28", "56", "60"],
                ["1", "3", "6", "9", "13", "16", "17", "20", "23", "28", "30"],
                ["1", "3", "6", "9", "13", "43", "44", "48", "50", "52", "55"],
                ["1", "3", "6", "9", "13", "16", "17", "20", "23", "28", "56", "60"]
            ],
            "通勤快速": [
                ["1", "3", "5", "7", "9", "13", "16", "17", "20", "23", "24", "28", "30"],
                ["1", "3", "5", "7", "9", "13", "42", "44", "45", "46", "48", "50", "52", "53", "55"],
                ["1", "3", "5", "7", "9", "13", "16", "17", "20", "23", "24", "28", "56", "57", "58", "60"]
            ],
            "快速": [
                ["1", "3", "5", "6", "7", "9", "13", "16", "17", "20", "23", "24", "28", "30"],
                ["1", "3", "5", "6", "7", "9", "13", "41", "42", "43", "44", "45", "46", "47", "48", "49", "50", "51", "52", "53", "54", "55"],
                ["1", "3", "5", "6", "7", "9", "13", "16", "17", "20", "23", "24", "28", "56", "57", "58", "60"]
            ],
            "準急": [
                ["1", "3", "5", "6", "7", "9", "11", "13", "16", "17", "19", "20", "23", "24", "25", "26", "28", "29", "30"],
                ["1", "3", "5", "6", "7", "9", "11", "13", "41", "42", "43", "44", "45", "46", "47", "48", "49", "50", "51", "52", "53", "54", "55"],
                ["1", "3", "5", "6", "7", "9", "11", "13", "16", "17", "19", "20", "23", "24", "25", "26", "28", "56", "57", "58", "59", "60"]
            ],
            "普通": [
                ["09", "08", "07", "06", "05", "04", "03", "02", "1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", "18", "19", "20", "21", "22", "23", "24", "25", "26", "27", "28", "29", "30"],
                ["09", "08", "07", "06", "05", "04", "03", "02", "1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", "18", "19", "20", "21", "22", "23", "24", "25", "26", "27", "28", "29", "30", "31", "32", "33", "34", "35", "36", "37", "38", "39", "40"],
                ["09", "08", "07", "06", "05", "04", "03", "02", "1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "41", "42", "43", "44", "45", "46", "47", "48", "49", "50", "51", "52", "53", "54", "55"],
                ["09", "08", "07", "06", "05", "04", "03", "02", "1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", "18", "19", "20", "21", "22", "23", "24", "25", "26", "27", "28", "55", "56", "57", "58", "59", "60"],
                ["1", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", "18", "19", "20", "21", "22", "23", "24", "25", "26", "27", "28", "29", "30"],
                ["1", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", "18", "19", "20", "21", "22", "23", "24", "25", "26", "27", "28", "30", "31", "32", "33", "34", "35", "36", "37", "38", "39", "40"],
                ["1", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "41", "42", "43", "44", "45", "46", "47", "48", "49", "50", "51", "52", "53", "54", "55"],
                ["1", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", "18", "19", "20", "21", "22", "23", "24", "25", "26", "27", "28", "56", "57", "58", "59", "60"]
            ]
        };

        const fullConsistsData = [
            { series: "S1系", items: Array.from({length: 5}, (_,i) => `S1-${String(i+1).padStart(2,'0')} (10両)`).concat(Array.from({length: 5}, (_,i) => `S1-${String(i+6).padStart(2,'0')} (8両)`)) },
            { series: "S2系", items: Array.from({length: 15}, (_,i) => `S2-${String(i+1).padStart(2,'0')} (10両)`).concat(Array.from({length: 10}, (_,i) => `S2-${String(i+16).padStart(2,'0')} (8両)`)) },
            { series: "S3系", items: Array.from({length: 10}, (_,i) => `S3-${String(i+1).padStart(2,'0')} (10両)`).concat(Array.from({length: 5}, (_,i) => `S3-${String(i+11).padStart(2,'0')} (8両)`)).concat(Array.from({length: 5}, (_,i) => `S3-${String(i+16).padStart(2,'0')} (6両)`)).concat(Array.from({length: 5}, (_,i) => `S3-${String(i+21).padStart(2,'0')} (4両)`)) },
            { series: "S4系", items: Array.from({length: 10}, (_,i) => `S4-${String(i+1).padStart(2,'0')} (10両)`).concat(Array.from({length: 10}, (_,i) => `S4-${String(i+11).padStart(2,'0')} (8両)`)).concat(Array.from({length: 6}, (_,i) => `S4-${String(i+20).padStart(2,'0')} (6両)`)) },
            { series: "S5系", items: Array.from({length: 4}, (_,i) => `S5-${String(i+1).padStart(2,'0')} (10両)`) },
            { series: "S100系", items: Array.from({length: 10}, (_,i) => `S100-${String(i+1).padStart(2,'0')} (10両)`).concat(Array.from({length: 5}, (_,i) => `S100-${String(i+11).padStart(2,'0')} (6両)`)).concat(Array.from({length: 5}, (_,i) => `S100-${String(i+16).padStart(2,'0')} (4両)`)) },
            { series: "S900系", items: ["S900-01 (事業用4両)", "S900-02 (事業用4両)"] }
        ];

        let previewActiveSubTab = 'front';
        let autoMoveTimer = null;
        let isAutoMoving = false;

        function toggleMenu() {
            const menu = document.getElementById('nav-menu');
            if (menu.classList.contains('hidden')) {
                menu.classList.remove('hidden');
                menu.classList.add('flex', 'flex-col', 'absolute', 'top-16', 'right-2', 'bg-slate-950/95', 'border', 'border-indigo-800', 'p-3', 'rounded-xl', 'shadow-2xl', 'z-50', 'space-y-2');
                menu.classList.remove('space-x-1', 'px-4', 'py-2', 'sticky', 'top-0', 'backdrop-blur');
            } else {
                menu.classList.add('hidden');
                menu.classList.remove('flex', 'flex-col', 'absolute', 'top-16', 'right-2', 'bg-slate-950/95', 'border', 'border-indigo-800', 'p-3', 'rounded-xl', 'shadow-2xl', 'z-50', 'space-y-2');
                menu.classList.add('space-x-1', 'px-4', 'py-2', 'sticky', 'top-0', 'backdrop-blur');
            }
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
            if(window.innerWidth < 768 && !menu.classList.contains('hidden')) {
                toggleMenu();
            }

            if (tabId === 'dia') renderMatrixTimetable();
            if (tabId === 'timetable') initTimetableDropdowns();
            if (tabId === 'traininfo') renderTrainInfoTable();
            if (tabId === 'consist') renderConsistMatrix();
            if (tabId === 'settings') renderEventDatesList();
            if (tabId === 'operation') renderOperationTrack();
            if (tabId === 'addtrain') {
                updateAddStationDropdowns();
                updateStationSchedulePreview();
            }
        }

        let appData = {
            trains: [
                { id: 1, trainNum: "101M", opNum: "A01", type: "特急", runDay: "weekday", startSt: "1", endSt: "30", consistNum: "S100-01", status: "走行中", currentIdx: 5 },
                { id: 2, trainNum: "104M", opNum: "A02", type: "快速", runDay: "weekday", startSt: "30", endSt: "1", consistNum: "S2-01", status: "走行中", currentIdx: 25 },
                { id: 3, trainNum: "205M", opNum: "B03", type: "普通", runDay: "weekday", startSt: "09", endSt: "40", consistNum: "S3-05", status: "停車中", currentIdx: 12 }
            ],
            eventDates: ["2026-10-15"]
        };
        let dbRef = null;
        const DEFAULT_FB_URL = "https://original-tetsudo-430ac-default-rtdb.firebaseio.com";

        function initFirebase() {
            try {
                if(firebase.apps.length === 0) {
                    firebase.initializeApp({ databaseURL: DEFAULT_FB_URL });
                }
                dbRef = firebase.database().ref('shinomori_railway_master');
                dbRef.on('value', (snapshot) => {
                    const val = snapshot.val();
                    if(val) {
                        appData = val;
                        if(!appData.trains || appData.trains.length === 0) {
                            appData.trains = [
                                { id: 1, trainNum: "101M", opNum: "A01", type: "特急", runDay: "weekday", startSt: "1", endSt: "30", consistNum: "S100-01", status: "走行中", currentIdx: 5 },
                                { id: 2, trainNum: "104M", opNum: "A02", type: "快速", runDay: "weekday", startSt: "30", endSt: "1", consistNum: "S2-01", status: "走行中", currentIdx: 25 }
                            ];
                        }
                        if(!appData.eventDates) appData.eventDates = ["2026-10-15"];
                        updateUI();
                    } else {
                        dbRef.set(appData);
                    }
                });
                document.getElementById('sync-status-dot').className = "w-2.5 h-2.5 rounded-full bg-emerald-500 animate-pulse";
                document.getElementById('sync-status-text').innerText = "同期中";
                document.getElementById('fb-url').value = DEFAULT_FB_URL;
            } catch(e) {
                document.getElementById('sync-status-dot').className = "w-2.5 h-2.5 rounded-full bg-red-500";
                document.getElementById('sync-status-text').innerText = "同期エラー（オフライン動作）";
            }
        }

        function pushData() { if(dbRef) dbRef.set(appData); }
        function updateUI() {
            renderTrainInfoTable();
            renderConsistMatrix();
            initTimetableDropdowns();
            renderEventDatesList();
            checkEventDayStatus();
            if(document.getElementById('tab-operation').classList.contains('active')) {
                renderOperationTrack();
            }
        }

        function updateAddStationDropdowns() {
            const options = allStationsMaster.map(st => `<option value="${st.id}">${st.id}. ${st.name}</option>`).join('');
            document.getElementById('add-start-station').innerHTML = options;
            document.getElementById('add-start-station-rear').innerHTML = options;
            document.getElementById('add-end-station').innerHTML = options;
            document.getElementById('add-end-station-rear').innerHTML = options;
            document.getElementById('tc-station').innerHTML = options;
            document.getElementById('cp-station').innerHTML = options;
            document.getElementById('add-end-station').value = "30";
            document.getElementById('add-end-station-rear').value = "20";
            updateStationSchedulePreview();
        }

        function toggleNumMode() {
            const mode = document.getElementById('num-mode').value;
            const commonBox = document.getElementById('box-num-common');
            const splitBox = document.getElementById('box-num-split');
            if(mode === 'split') {
                commonBox.classList.add('hidden');
                splitBox.classList.remove('hidden');
            } else {
                commonBox.classList.remove('hidden');
                splitBox.classList.add('hidden');
            }
        }

        function toggleConsistMode() {
            const mode = document.getElementById('consist-mode').value;
            const c2Container = document.getElementById('consist-2-container');
            if(mode === 'double') {
                c2Container.classList.remove('hidden');
            } else {
                c2Container.classList.add('hidden');
            }
        }

        function updateConsistNumbers(num) {
            const series = document.getElementById(`consist-series-${num}`).value;
            const sel = document.getElementById(`consist-number-${num}`);
            let found = fullConsistsData.find(c => c.series.startsWith(series));
            if(found) {
                sel.innerHTML = found.items.map(it => `<option>${it}</option>`).join('');
            }
        }

        function toggleActionCheckboxes(changed) {
            const chkType = document.getElementById('chk-typechange');
            const chkCp = document.getElementById('chk-coupling');

            if(changed === 'type' && chkType.checked) {
                chkCp.checked = false;
                document.getElementById('box-coupling').classList.add('hidden');
            } else if(changed === 'coupling' && chkCp.checked) {
                chkType.checked = false;
                document.getElementById('box-typechange').classList.add('hidden');
            }

            document.getElementById('box-typechange').classList.toggle('hidden', !chkType.checked);
            document.getElementById('box-coupling').classList.toggle('hidden', !chkCp.checked);

            const cpAction = document.getElementById('cp-action').value;
            const boxStartRear = document.getElementById('box-start-rear');
            const boxEndRear = document.getElementById('box-end-rear');
            const labelStart1 = document.getElementById('label-start-1');
            const labelEnd1 = document.getElementById('label-end-1');

            if(chkCp && chkCp.checked) {
                if(cpAction === 'couple') {
                    boxStartRear.classList.remove('hidden');
                    boxEndRear.classList.add('hidden');
                    labelStart1.innerText = "【前方列車】始点駅";
                } else if(cpAction === 'uncouple') {
                    boxStartRear.classList.add('hidden');
                    boxEndRear.classList.remove('hidden');
                    labelEnd1.innerText = "【前方列車】終点駅（行き先）";
                }
            } else {
                boxStartRear.classList.add('hidden');
                boxEndRear.classList.add('hidden');
                labelStart1.innerText = "始点駅";
                labelEnd1.innerText = "終点駅（行き先）";
            }

            updateStationSchedulePreview();
        }

        function switchPreviewSubTab(subTab) {
            previewActiveSubTab = subTab;
            const btnFront = document.getElementById('btn-prev-front');
            const btnRear = document.getElementById('btn-prev-rear');
            if(subTab === 'front') {
                btnFront.className = "px-2.5 py-1 rounded text-xs font-bold bg-indigo-600 text-white transition";
                btnRear.className = "px-2.5 py-1 rounded text-xs font-bold bg-slate-800 text-slate-400 transition";
            } else {
                btnFront.className = "px-2.5 py-1 rounded text-xs font-bold bg-slate-800 text-slate-400 transition";
                btnRear.className = "px-2.5 py-1 rounded text-xs font-bold bg-indigo-600 text-white transition";
            }
            updateStationSchedulePreview();
        }

        function updateStationSchedulePreview() {
            const trainType = document.getElementById('add-train-type').value;
            const patternIdx = parseInt(document.getElementById('add-pattern-index') ? document.getElementById('add-pattern-index').value : 0) || 0;
            
            const chkType = document.getElementById('chk-typechange').checked;
            const tcStation = document.getElementById('tc-station').value;
            const tcNewType = document.getElementById('tc-newtype').value;

            const chkCp = document.getElementById('chk-coupling').checked;
            const cpAction = document.getElementById('cp-action').value;
            const cpStation = document.getElementById('cp-station').value;

            const previewTabs = document.getElementById('preview-tabs');
            const previewTitle = document.getElementById('preview-title-label');
            const preview = document.getElementById('station-schedule-preview');
            if(!preview) return;

            let startId = document.getElementById('add-start-station').value;
            let endId = document.getElementById('add-end-station').value;

            if(chkCp) {
                previewTabs.classList.remove('hidden');
                if(cpAction === 'uncouple') {
                    if(previewActiveSubTab === 'rear') {
                        startId = cpStation;
                        endId = document.getElementById('add-end-station-rear').value;
                        previewTitle.innerText = "【後方列車 (切り離し後)】 発着スケジュール設定";
                    } else {
                        previewTitle.innerText = "【前方列車 (本務)】 発着スケジュール設定";
                    }
                } else if(cpAction === 'couple') {
                    if(previewActiveSubTab === 'rear') {
                        startId = document.getElementById('add-start-station-rear').value;
                        endId = cpStation;
                        previewTitle.innerText = "【後方列車 (連結前)】 発着スケジュール設定";
                    } else {
                        previewTitle.innerText = "【前方列車 (本務)】 発着スケジュール設定";
                    }
                }
            } else {
                previewTabs.classList.add('hidden');
                previewTitle.innerText = "停車駅スケジュール・発着時間";
            }

            let sIdx = allStationsMaster.findIndex(s => s.id === startId);
            let eIdx = allStationsMaster.findIndex(s => s.id === endId);
            
            if(sIdx === -1 || eIdx === -1 || sIdx > eIdx) {
                preview.innerHTML = `<p class="text-red-400">始点と終点の順序を確認してください。</p>`;
                return;
            }

            let currentType = trainType;
            let stopsLists = officialStopsMaster[currentType] || officialStopsMaster["普通"];
            let currentStopIds = stopsLists[patternIdx % stopsLists.length] || stopsLists[0];

            let html = `<table class="w-full text-left border-collapse">
                <thead>
                    <tr class="text-indigo-300 border-b border-slate-800 text-[11px]">
                        <th class="p-2">駅名</th>
                        <th class="p-2">判定 (正式停車/通過)</th>
                        <th class="p-2">到着時刻</th>
                        <th class="p-2">発車時刻</th>
                    </tr>
                </thead>
                <tbody class="divide-y divide-slate-800">`;
            
            for(let i = sIdx; i <= eIdx; i++) {
                const st = allStationsMaster[i];

                if(chkType && st.id === tcStation) {
                    currentType = tcNewType;
                    stopsLists = officialStopsMaster[currentType] || officialStopsMaster["普通"];
                    currentStopIds = stopsLists[patternIdx % stopsLists.length] || stopsLists[0];
                }

                const isExplicitStop = currentStopIds.includes(st.id);
                const isStartOrEnd = (i === sIdx || i === eIdx);
                const stopping = isExplicitStop || isStartOrEnd;

                let baseHour = 8;
                let baseMin = 10 + (i - sIdx) * 3;
                if(baseMin >= 60) {
                    baseHour += Math.floor(baseMin / 60);
                    baseMin = baseMin % 60;
                }
                const timeStr = `${String(baseHour).padStart(2,'0')}:${String(baseMin).padStart(2,'0')}`;

                html += `
                    <tr>
                        <td class="p-2 font-bold text-slate-200">${st.id}. ${st.name}</td>
                        <td class="p-2"><span class="px-1.5 py-0.5 rounded text-[10px] ${stopping ? 'bg-indigo-950 text-indigo-300 border border-indigo-800' : 'bg-slate-800 text-slate-500'}">${stopping ? currentType + ' (停車)' : '通過'}</span></td>
                        <td class="p-2"><input type="text" value="${i === sIdx ? '-' : (stopping ? timeStr : '通過')}" class="arr-time bg-slate-950 border border-slate-700 rounded px-2 py-1 w-20 text-xs font-mono text-center text-white focus:border-indigo-500 outline-none"></td>
                        <td class="p-2"><input type="text" value="${i === eIdx ? '-' : (stopping ? timeStr : '通過')}" class="dep-time bg-slate-950 border border-slate-700 rounded px-2 py-1 w-20 text-xs font-mono text-center text-white focus:border-indigo-500 outline-none"></td>
                    </tr>
                `;
            }
            html += `</tbody></table>`;
            preview.innerHTML = html;
        }

        function addNewTrain() {
            const numMode = document.getElementById('num-mode').value;
            let trainNum = document.getElementById('add-train-num').value;
            let opNum = document.getElementById('add-op-num').value;

            if(numMode === 'split') {
                trainNum = `${document.getElementById('front-train-num').value} / ${document.getElementById('rear-train-num').value}`;
                opNum = `${document.getElementById('front-op-num').value} / ${document.getElementById('rear-op-num').value}`;
            }

            const type = document.getElementById('add-train-type').value;
            const runDay = document.getElementById('add-run-day').value;
            const startSt = document.getElementById('add-start-station').value;
            const endSt = document.getElementById('add-end-station').value;
            
            const consistMode = document.getElementById('consist-mode').value;
            const c1Full = document.getElementById('consist-number-1').value;
            let consistStr = c1Full ? c1Full.split(' ')[0] : 'S100-01';

            if(consistMode === 'double') {
                const c2Full = document.getElementById('consist-number-2').value;
                const c2 = c2Full ? c2Full.split(' ')[0] : 'S4-01';
                consistStr = `${consistStr} ＋ ${c2} (併結)`;
            }

            const chkType = document.getElementById('chk-typechange').checked;
            const tcStation = document.getElementById('tc-station').value;
            const tcNewType = document.getElementById('tc-newtype').value;

            const chkCp = document.getElementById('chk-coupling').checked;
            const cpAction = document.getElementById('cp-action').value;
            const cpStation = document.getElementById('cp-station').value;
            const cpDetail = document.getElementById('cp-detail').value;

            let opAction = 'none';
            let actionStation = '';
            let actionDetail = '';

            if(chkType) {
                opAction = 'typechange';
                actionStation = tcStation;
                actionDetail = tcNewType;
            } else if(chkCp) {
                opAction = cpAction;
                actionStation = cpStation;
                actionDetail = cpDetail;
            }

            const statuses = ["走行中", "停車中", "運行準備中"];
            const randomStatus = statuses[Math.floor(Math.random() * statuses.length)];
            const startIdx = allStationsMaster.findIndex(s => s.id === startSt);

            if(!appData.trains) appData.trains = [];
            appData.trains.push({
                id: Date.now(),
                trainNum,
                opNum,
                type,
                runDay,
                line: 'main',
                startSt,
                endSt,
                consistNum: consistStr,
                opAction,
                actionStation,
                actionDetail,
                status: randomStatus,
                currentIdx: startIdx !== -1 ? startIdx : 0
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
                tbody.innerHTML = `<tr><td colspan="6" class="p-4 text-center text-slate-500">追加された列車はありません。「列車追加」タブから登録してください。</td></tr>`;
                return;
            }

            tbody.innerHTML = trains.map(t => {
                let badgeColor = "bg-slate-700 text-slate-300";
                if(t.status === "走行中") badgeColor = "bg-emerald-950 text-emerald-300 border border-emerald-700";
                if(t.status === "停車中") badgeColor = "bg-sky-950 text-sky-300 border border-sky-700";
                if(t.status === "運行準備中") badgeColor = "bg-amber-950 text-amber-300 border border-amber-700";

                const endName = allStationsMaster.find(s => s.id === t.endSt)?.name || t.endSt;
                let typeDisplay = t.type;
                if(t.opAction === 'typechange') {
                    const stName = allStationsMaster.find(s => s.id === t.actionStation)?.name || '';
                    typeDisplay = `${t.type} → ${stName}から${t.actionDetail}に変更`;
                } else if(t.opAction === 'uncouple') {
                    const stName = allStationsMaster.find(s => s.id === t.actionStation)?.name || '';
                    typeDisplay = `${t.type} (${stName}で分割)`;
                } else if(t.opAction === 'couple') {
                    const stName = allStationsMaster.find(s => s.id === t.actionStation)?.name || '';
                    typeDisplay = `${t.type} (${stName}で連結)`;
                }

                return `
                    <tr class="hover:bg-slate-750 transition">
                        <td class="p-3 font-bold text-indigo-300">${t.trainNum}</td>
                        <td class="p-3 font-mono">${t.opNum}</td>
                        <td class="p-3">${typeDisplay}</td>
                        <td class="p-3 font-bold text-slate-200">${endName} 行</td>
                        <td class="p-3 font-mono text-indigo-400">${t.consistNum || '-'}</td>
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
                            const code = item.split(' ')[0];
                            const assigned = (appData.trains || []).filter(t => (t.consistNum && t.consistNum.includes(code)));
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
                            <th class="py-2 px-3 border-r border-slate-700 text-left sticky left-0 bg-slate-900 z-10 w-36 whitespace-nowrap">駅名</th>
            `;
            trains.forEach(t => {
                html += `<th class="py-2 px-2 border-r border-slate-700 min-w-[70px]"><div class="font-bold text-indigo-300 text-xs">${t.trainNum}</div><div class="text-[9px] text-slate-400 bg-slate-800 px-0.5 rounded mt-0.5">${t.type}</div></th>`;
            });
            html += `</tr></thead><tbody class="divide-y divide-slate-800 text-slate-300">`;

            allStationsMaster.forEach(st => {
                html += `<tr class="hover:bg-slate-750"><td class="py-1.5 px-3 border-r border-slate-700 text-left font-medium sticky left-0 bg-slate-800 z-10 whitespace-nowrap text-xs w-36 overflow-hidden text-ellipsis">${st.id}. ${st.name}</td>`;
                trains.forEach(t => {
                    const stopsLists = officialStopsMaster[t.type] || officialStopsMaster["普通"];
                    const stopIds = stopsLists[0];
                    const isStop = stopIds.includes(st.id) || st.id === t.startSt || st.id === t.endSt;
                    const timeCell = isStop ? `08:${String(parseInt(st.id || '1')*2).padStart(2,'0')}` : '｜';
                    html += `<td class="py-1.5 px-2 border-r border-slate-800 font-mono text-slate-300 text-[11px]">${timeCell}</td>`;
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

        /* --- 走行位置スクリプト（アイコンデザイン改良版） --- */
        function renderOperationTrack() {
            const containerParent = document.getElementById('track-container-parent');
            if(!containerParent) return;

            const isDetail = document.getElementById('toggleView').checked;
            const totalStations = allStationsMaster.length;
            const containerHeight = Math.max(1400, totalStations * 36 + 100);
            containerParent.style.height = `${containerHeight}px`;

            let html = `<div class="vertical-rail" style="height: ${containerHeight - 80}px;"></div>`;

            const spacing = (containerHeight - 80) / (totalStations - 1);
            allStationsMaster.forEach((st, idx) => {
                const topPos = 40 + (idx * spacing);
                html += `
                    <div class="v-station-node" style="top: ${topPos}px;">
                        <div class="v-station-dot"></div>
                        <div class="v-station-label">${st.id}. ${st.name}</div>
                    </div>
                `;
            });

            const trains = appData.trains || [];
            trains.forEach((t, i) => {
                const isUp = (i % 2 === 0);
                if(t.currentIdx === undefined) t.currentIdx = (i * 5) % totalStations;
                const topPos = 40 + (t.currentIdx * spacing);
                const destName = allStationsMaster.find(s => s.id === t.endSt)?.name || t.endSt;
                const currentStationName = allStationsMaster[t.currentIdx]?.name || '走行中';
                const typeClass = `type-${t.type}`;

                if(isDetail) {
                    html += `
                        <div class="v-train ${isUp ? 'up-train' : 'down-train'}" style="top: ${topPos}px;" onclick='openTrainModal(${JSON.stringify(t)})'>
                            <div class="train-icon-badge ${typeClass}">
                                <span>${isUp ? '▲' : '▼'}</span>
                                <span>${t.type}</span>
                            </div>
                            <div class="train-card-v">
                                <div class="train-num-top">${t.trainNum} (${t.opNum || 'A01'})</div>
                                <div class="font-bold text-slate-100">${destName}行</div>
                                <div class="text-[9px] text-sky-300">📍 ${currentStationName}</div>
                            </div>
                        </div>
                    `;
                } else {
                    html += `
                        <div class="v-train ${isUp ? 'up-train' : 'down-train'}" style="top: ${topPos}px;" onclick='openTrainModal(${JSON.stringify(t)})'>
                            <div class="train-icon-badge ${typeClass}" title="${t.trainNum}: ${t.type} (${currentStationName})">
                                <span>${isUp ? '▲' : '▼'}</span>
                                <span>${t.trainNum}</span>
                            </div>
                        </div>
                    `;
                }
            });

            containerParent.innerHTML = html;
        }

        function openTrainModal(train) {
            const modal = document.getElementById('train-modal');
            const numEl = document.getElementById('modal-train-num');
            const opEl = document.getElementById('modal-op-num');
            const contentEl = document.getElementById('modal-content');

            numEl.innerText = `列車番号: ${train.trainNum}`;
            opEl.innerText = `運用番号: ${train.opNum || 'A01'}`;

            const destName = allStationsMaster.find(s => s.id === train.endSt)?.name || train.endSt;
            const curStName = allStationsMaster[train.currentIdx || 0]?.name || '不明';

            contentEl.innerHTML = `
                <div class="bg-slate-900 p-3 rounded-lg border border-slate-700 space-y-2 text-xs">
                    <div class="flex justify-between"><span class="text-slate-400">列車種別:</span> <span class="font-bold text-indigo-300">${train.type}</span></div>
                    <div class="flex justify-between"><span class="text-slate-400">行先:</span> <span class="font-bold text-slate-200">${destName} 行</span></div>
                    <div class="flex justify-between"><span class="text-slate-400">現在位置:</span> <span class="font-bold text-emerald-300">📍 ${curStName}付近</span></div>
                    <div class="flex justify-between"><span class="text-slate-400">編成・両数:</span> <span class="font-mono text-indigo-400">${train.consistNum || 'S100-01'}</span></div>
                    <div class="flex justify-between"><span class="text-slate-400">運行状態:</span> <span class="font-bold text-sky-300">${train.status}</span></div>
                </div>
            `;
            modal.classList.remove('hidden');
        }

        function closeTrainModal() {
            document.getElementById('train-modal').classList.add('hidden');
        }

        function toggleAutoMove() {
            const btn = document.getElementById('auto-move-btn');
            if(isAutoMoving) {
                clearInterval(autoMoveTimer);
                isAutoMoving = false;
                btn.className = "bg-emerald-600 hover:bg-emerald-500 text-white px-3 py-1.5 rounded text-xs font-bold transition shadow";
                btn.innerText = "▶ 自動運行シミュレーション開始";
            } else {
                isAutoMoving = true;
                btn.className = "bg-rose-600 hover:bg-rose-500 text-white px-3 py-1.5 rounded text-xs font-bold transition shadow animate-pulse";
                btn.innerText = "⏹ 自動運行停止";
                autoMoveTimer = setInterval(() => {
                    if(appData.trains) {
                        appData.trains.forEach(t => {
                            if(t.currentIdx === undefined) t.currentIdx = 0;
                            t.currentIdx = (t.currentIdx + 1) % allStationsMaster.length;
                        });
                        if(document.getElementById('tab-operation').classList.contains('active')) {
                            renderOperationTrack();
                        }
                    }
                }, 2500);
            }
        }

        window.onload = function() {
            initFirebase();
            updateAddStationDropdowns();
            updateConsistNumbers(1);
            updateConsistNumbers(2);
            initTimetableDropdowns();
            setInterval(updateLiveDateTime, 1000);
            updateLiveDateTime();
            renderOperationTrack();
        };
    </script>
</body>
</html>
