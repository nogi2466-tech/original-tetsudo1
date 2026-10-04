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

        /* --- えれサイト風 走行位置スタイル --- */
        .track-container-vertical {
            position: relative;
            min-height: 1400px;
            background: #f1f5f9;
            border-radius: 12px;
            padding: 40px 20px;
            box-shadow: inset 0 2px 4px rgba(0,0,0,0.05);
            overflow: hidden;
            border: 1px solid #cbd5e1;
            display: flex;
            justify-content: center;
        }
        .vertical-rail {
            position: absolute;
            top: 40px;
            bottom: 40px;
            left: 50%;
            width: 6px;
            background: #db2777;
            transform: translateX(-50%);
            border-radius: 3px;
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
        .v-station-bar {
            position: absolute;
            left: 10%;
            right: 10%;
            height: 32px;
            background: rgba(226, 232, 240, 0.7);
            border-radius: 4px;
            transform: translateY(-50%);
            z-index: 1;
        }
        .v-station-dot {
            position: absolute;
            left: 50%;
            width: 16px;
            height: 16px;
            background: #ffffff;
            border: 4px solid #db2777;
            border-radius: 50%;
            transform: translateX(-50%);
            z-index: 3;
            box-shadow: 0 1px 3px rgba(0,0,0,0.15);
        }
        .v-station-label {
            position: absolute;
            right: calc(50% + 30px);
            font-size: 0.85rem;
            font-weight: bold;
            color: #1e293b;
            white-space: nowrap;
            z-index: 3;
        }
        
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
        /* 線を挟み：左側が上り、右側が下り */
        .v-train.up-train {
            right: calc(50% + 25px);
            flex-direction: row-reverse;
            text-align: right;
        }
        .v-train.down-train {
            left: calc(50% + 25px);
            flex-direction: row;
            text-align: left;
        }

        .train-icon-badge {
            background: #334155;
            color: white;
            padding: 2px 6px;
            border-radius: 4px;
            font-size: 10px;
            font-weight: bold;
            box-shadow: 0 2px 4px rgba(0,0,0,0.2);
            border: 1px solid #475569;
            white-space: nowrap;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .train-op-num {
            background: #0f172a;
            color: #38bdf8;
            padding: 1px 4px;
            border-radius: 3px;
            font-size: 10px;
            font-family: monospace;
            margin-bottom: 2px;
            border: 1px solid #334155;
        }
        .train-type-tag {
            font-size: 9px;
            padding: 0 3px;
            border-radius: 2px;
        }
        .type-特急 { background: #ef4444; color: white; }
        .type-急行 { background: #f97316; color: white; }
        .type-快速 { background: #eab308; color: #1e293b; }
        .type-準急 { background: #10b981; color: white; }
        .type-普通 { background: #64748b; color: white; }

        .train-card-v {
            background: rgba(255, 255, 255, 0.95);
            color: #1e293b;
            border-radius: 6px;
            padding: 4px 8px;
            font-size: 10px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.15);
            border: 1px solid #cbd5e1;
            backdrop-filter: blur(4px);
            min-width: 90px;
        }
        .train-num-top {
            font-size: 9px;
            font-weight: bold;
            color: #2563eb;
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
                        <li><span class="text-slate-400 inline-block w-28">社名</span> 紫句守鉄道株式会社[cite: 1]</li>
                        <li><span class="text-slate-400 inline-block w-28">設立</span> 1965年4月1日[cite: 1]</li>
                        <li><span class="text-slate-400 inline-block w-28">本社所在地</span> 陽光県紫句守市中央一丁目1番地[cite: 1]</li>
                    </ul>
                </div>
                <div class="bg-slate-800 p-5 rounded-xl border border-slate-700 space-y-3">
                    <h3 class="text-indigo-400 font-bold border-b border-slate-700 pb-2">路線データ</h3>
                    <ul class="text-sm text-slate-300 space-y-2">
                        <li><span class="text-slate-400 inline-block w-28">紫雲本線</span> 駅ID: 1～30[cite: 1]</li>
                        <li><span class="text-slate-400 inline-block w-28">星句高原線</span> 駅ID: 31～40[cite: 1]</li>
                        <li><span class="text-slate-400 inline-block w-28">句守支線</span> 駅ID: 41～55[cite: 1]</li>
                        <li><span class="text-slate-400 inline-block w-28">紫霞観光線</span> 駅ID: 56～60[cite: 1]</li>
                        <li><span class="text-slate-400 inline-block w-28">直通路線</span> 駅ID: 09～1[cite: 1]</li>
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
                <h2 class="text-xl font-bold text-indigo-200">列車走行位置（時刻連動・リアルタイム自動運行中）</h2>
            </div>
            
            <div class="controls bg-slate-800 p-4 rounded-xl border border-slate-700 flex flex-wrap items-center justify-between gap-4">
                <div class="flex flex-wrap items-center gap-4">
                    <div>
                        <label class="block text-[11px] text-slate-400 mb-1">路線切り替え</label>
                        <select id="op-line-select" onchange="renderOperationTrack()" class="bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-xs text-white">
                            <option value="main">紫雲本線 (1～30)</option>
                            <option value="hoshiku">星句高原線 (31～40)</option>
                            <option value="shikan">句守支線 (41～55)</option>
                            <option value="shikasumitour">紫霞観光線 (56～60)</option>
                            <option value="direct">直通路線 (09～1)</option>
                            <option value="all">全線一括表示</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-[11px] text-slate-400 mb-1">表示形式</label>
                        <select id="op-view-mode" onchange="renderOperationTrack()" class="bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-xs text-white">
                            <option value="card">詳細情報カード付き表示</option>
                            <option value="icon">アイコン（ヘッドマーク）のみ</option>
                            <option value="simple">簡易テキスト表示</option>
                        </select>
                    </div>
                </div>
                <div class="text-xs text-indigo-300">
                    📍 中央線を挟み <strong>左側：上り列車</strong> ／ <strong>右側：下り列車</strong> （タップで詳細確認）
                </div>
            </div>

            <div id="track-container-parent" class="track-container-vertical">
                <div class="vertical-rail"></div>
            </div>

            <div class="status-panel bg-slate-800 p-4 rounded-xl border border-slate-700 flex justify-between items-center">
                <p class="text-sm text-slate-300"><strong>運行状況モニタリング:</strong> <span id="statusText" class="text-indigo-300 font-bold">全線正常運行中（現在時刻に基づき自動連動）</span></p>
                <button onclick="renderOperationTrack()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-3 py-1.5 rounded text-xs transition">今すぐ位置を再計算</button>
            </div>
        </div>

        <!-- 4. 列車情報 -->
        <div id="tab-traininfo" class="tab-content space-y-4">
            <div class="flex flex-wrap items-center justify-between gap-4">
                <h2 class="text-xl font-bold text-indigo-200">列車情報一覧（リアルタイム連動）</h2>
                <div class="flex items-center gap-2">
                    <label class="text-xs text-slate-400">表示モード:</label>
                    <select id="train-view-mode" onchange="renderTrainInfoTable()" class="bg-slate-800 border border-slate-700 rounded px-3 py-1.5 text-xs text-white font-medium">
                        <option value="trainnum">列車番号順一覧</option>
                        <option value="opnum">運用番号順一覧（時間順運用流れ）</option>
                    </select>
                </div>
            </div>

            <div class="bg-slate-800 rounded-xl border border-slate-700 overflow-hidden shadow-xl">
                <div id="train-info-container" class="overflow-x-auto">
                    <!-- 動的描画 -->
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
                            <input type="text" id="add-op-num" class="w-full bg-slate-800 border border-slate-700 rounded px-3 py-2 text-sm text-white" value="73K">
                        </div>
                    </div>

                    <div id="box-num-split" class="grid grid-cols-1 md:grid-cols-2 gap-4 pt-2 hidden">
                        <div class="space-y-2 bg-slate-800/60 p-3 rounded border border-slate-700">
                            <span class="text-[11px] text-indigo-300 font-bold block">【前方列車（本務列車）】番号</span>
                            <div class="grid grid-cols-2 gap-2">
                                <input type="text" id="front-train-num" placeholder="列車番号" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white" value="101M">
                                <input type="text" id="front-op-num" placeholder="運用番号" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white" value="73K">
                            </div>
                        </div>
                        <div class="space-y-2 bg-slate-800/60 p-3 rounded border border-slate-700">
                            <span class="text-[11px] text-sky-300 font-bold block">【後方列車（増結・分割後）】番号</span>
                            <div class="grid grid-cols-2 gap-2">
                                <input type="text" id="rear-train-num" placeholder="列車番号" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white" value="103M">
                                <input type="text" id="rear-op-num" placeholder="運用番号" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white" value="54K">
                            </div>
                        </div>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">基本種別</label>
                        <select id="add-train-type" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
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

                <!-- 発着時間入力フィールド -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs text-indigo-300 mb-1 font-bold">始発駅 発車時刻</label>
                        <input type="time" id="add-dep-time" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white" value="08:00">
                    </div>
                    <div>
                        <label class="block text-xs text-indigo-300 mb-1 font-bold">終着駅 到着時刻</label>
                        <input type="time" id="add-arr-time" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white" value="09:30">
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

                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="space-y-2">
                        <label class="block text-xs text-indigo-300 font-bold">始点駅設定</label>
                        <select id="add-start-station" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white"></select>
                    </div>
                    <div class="space-y-2">
                        <label class="block text-xs text-indigo-300 font-bold">終点駅（行き先）設定</label>
                        <select id="add-end-station" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white"></select>
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
        <div class="bg-slate-800 border border-indigo-700 p-6 rounded-2xl w-full max-w-lg space-y-4 shadow-2xl relative max-h-[90vh] overflow-y-auto">
            <div class="flex justify-between items-center border-b border-slate-700 pb-3">
                <div class="flex items-center space-x-2">
                    <span class="text-2xl">🚄</span>
                    <div>
                        <h3 id="modal-train-num" class="text-lg font-bold text-indigo-200">101M</h3>
                        <p id="modal-op-num" class="text-xs text-slate-400 font-mono">運用: 73K</p>
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
            "特急": [["1", "3", "13", "16", "17", "23", "28", "30"]],
            "急行": [["1", "3", "6", "9", "13", "16", "17", "20", "23", "28", "30"]],
            "快速": [["1", "3", "5", "6", "7", "9", "13", "16", "17", "20", "23", "24", "28", "30"]],
            "準急": [["1", "3", "5", "6", "7", "9", "11", "13", "16", "17", "19", "20", "23", "24", "25", "26", "28", "29", "30"]],
            "普通": [["09", "08", "07", "06", "05", "04", "03", "02", "1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", "18", "19", "20", "21", "22", "23", "24", "25", "26", "27", "28", "29", "30"]]
        };

        const fullConsistsData = [
            { series: "S1系", items: Array.from({length: 5}, (_,i) => `S1-${String(i+1).padStart(2,'0')} (10両)`).concat(Array.from({length: 5}, (_,i) => `S1-${String(i+6).padStart(2,'0')} (8両)`)) },
            { series: "S2系", items: Array.from({length: 15}, (_,i) => `S2-${String(i+1).padStart(2,'0')} (10両)`).concat(Array.from({length: 10}, (_,i) => `S2-${String(i+16).padStart(2,'0')} (8両)`)) },
            { series: "S3系", items: Array.from({length: 10}, (_,i) => `S3-${String(i+1).padStart(2,'0')} (10両)`).concat(Array.from({length: 5}, (_,i) => `S3-${String(i+11).padStart(2,'0')} (8両)`)) },
            { series: "S4系", items: Array.from({length: 10}, (_,i) => `S4-${String(i+1).padStart(2,'0')} (10両)`).concat(Array.from({length: 10}, (_,i) => `S4-${String(i+11).padStart(2,'0')} (8両)`)) },
            { series: "S5系", items: Array.from({length: 4}, (_,i) => `S5-${String(i+1).padStart(2,'0')} (10両)`) },
            { series: "S100系", items: Array.from({length: 10}, (_,i) => `S100-${String(i+1).padStart(2,'0')} (10両)`).concat(Array.from({length: 5}, (_,i) => `S100-${String(i+11).padStart(2,'0')} (6両)`)) },
            { series: "S900系", items: ["S900-01 (事業用4両)", "S900-02 (事業用4両)"] }
        ];

        function toggleMenu() {
            const menu = document.getElementById('nav-menu');
            menu.classList.toggle('hidden');
            menu.classList.toggle('flex');
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

            if (tabId === 'dia') renderMatrixTimetable();
            if (tabId === 'timetable') initTimetableDropdowns();
            if (tabId === 'traininfo') renderTrainInfoTable();
            if (tabId === 'consist') renderConsistMatrix();
            if (tabId === 'settings') renderEventDatesList();
            if (tabId === 'operation') renderOperationTrack();
            if (tabId === 'addtrain') updateAddStationDropdowns();
        }

        let appData = {
            trains: [
                { id: 1, trainNum: "101M", opNum: "73K", type: "特急", runDay: "weekday", startSt: "1", endSt: "30", depTime: "06:00", arrTime: "23:59", consistNum: "S100-01 (10両)", status: "走行中" },
                { id: 2, trainNum: "104M", opNum: "54K", type: "快速", runDay: "weekday", startSt: "30", endSt: "1", depTime: "06:00", arrTime: "23:59", consistNum: "S2-01 (10両)", status: "走行中" },
                { id: 3, trainNum: "205M", opNum: "73K", type: "普通", runDay: "weekday", startSt: "09", endSt: "40", depTime: "06:00", arrTime: "23:59", consistNum: "S3-05 (8両)", status: "停車中" }
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
                        if(!appData.trains) appData.trains = [];
                        if(!appData.eventDates) appData.eventDates = ["2026-10-15"];
                        updateUI();
                    } else {
                        dbRef.set(appData);
                    }
                });
                document.getElementById('sync-status-dot').className = "w-2.5 h-2.5 rounded-full bg-emerald-500 animate-pulse";
                document.getElementById('sync-status-text').innerText = "リアルタイム同期中";
                document.getElementById('fb-url').value = DEFAULT_FB_URL;
            } catch(e) {
                document.getElementById('sync-status-dot').className = "w-2.5 h-2.5 rounded-full bg-red-500";
                document.getElementById('sync-status-text').innerText = "同期エラー（ローカル動作）";
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
            document.getElementById('add-end-station').innerHTML = options;
            document.getElementById('add-end-station').value = "30";
        }

        function toggleNumMode() {
            const mode = document.getElementById('num-mode').value;
            document.getElementById('box-num-common').classList.toggle('hidden', mode === 'split');
            document.getElementById('box-num-split').classList.toggle('hidden', mode !== 'split');
        }

        function toggleConsistMode() {
            const mode = document.getElementById('consist-mode').value;
            document.getElementById('consist-2-container').classList.toggle('hidden', mode !== 'double');
        }

        function updateConsistNumbers(num) {
            const series = document.getElementById(`consist-series-${num}`).value;
            const sel = document.getElementById(`consist-number-${num}`);
            let found = fullConsistsData.find(c => c.series.startsWith(series));
            if(found) {
                sel.innerHTML = found.items.map(it => `<option>${it}</option>`).join('');
            }
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
            const depTime = document.getElementById('add-dep-time').value || "08:00";
            const arrTime = document.getElementById('add-arr-time').value || "09:30";
            
            const consistMode = document.getElementById('consist-mode').value;
            const c1Full = document.getElementById('consist-number-1').value;
            let consistStr = c1Full || 'S100-01 (10両)';

            if(consistMode === 'double') {
                const c2Full = document.getElementById('consist-number-2').value;
                consistStr = `${consistStr} ＋ ${c2Full || 'S4-01 (10両)'}`;
            }

            if(!appData.trains) appData.trains = [];
            appData.trains.push({
                id: Date.now(),
                trainNum,
                opNum,
                type,
                runDay,
                startSt,
                endSt,
                depTime,
                arrTime,
                consistNum: consistStr,
                status: "走行中"
            });

            pushData();
            updateUI();
            alert("列車を正常に追加しました！");
            switchTab('traininfo');
        }

        /* --- 列車情報テーブル描画 --- */
        function renderTrainInfoTable() {
            const container = document.getElementById('train-info-container');
            const viewMode = document.getElementById('train-view-mode')?.value || 'trainnum';
            if(!container) return;

            const trains = appData.trains || [];
            if(trains.length === 0) {
                container.innerHTML = `<div class="p-4 text-center text-slate-500 text-xs">追加された列車はありません。</div>`;
                return;
            }

            if(viewMode === 'trainnum') {
                let html = `
                    <table class="w-full text-left text-xs">
                        <thead class="bg-slate-900 text-indigo-200 border-b border-slate-700">
                            <tr>
                                <th class="p-3">列車番号</th>
                                <th class="p-3">運用番号</th>
                                <th class="p-3">種別・区間ルール</th>
                                <th class="p-3">行き先</th>
                                <th class="p-3">運行時間</th>
                                <th class="p-3">両数・編成番号</th>
                                <th class="p-3">状態</th>
                            </tr>
                        </thead>
                        <tbody class="divide-y divide-slate-700 text-slate-300">
                `;
                trains.forEach(t => {
                    let badgeColor = "bg-emerald-950 text-emerald-300 border border-emerald-700";
                    if(t.status === "停車中") badgeColor = "bg-sky-950 text-sky-300 border border-sky-700";
                    const endName = allStationsMaster.find(s => s.id === t.endSt)?.name || t.endSt;

                    html += `
                        <tr class="hover:bg-slate-750 transition">
                            <td class="p-3 font-bold text-indigo-300">${t.trainNum}</td>
                            <td class="p-3 font-mono">${t.opNum}</td>
                            <td class="p-3">${t.type}</td>
                            <td class="p-3 font-bold text-slate-200">${endName} 行</td>
                            <td class="p-3 font-mono text-indigo-300">${t.depTime || '08:00'}発 〜 ${t.arrTime || '09:30'}着</td>
                            <td class="p-3 font-mono text-indigo-400">${t.consistNum || '-'}</td>
                            <td class="p-3"><span class="px-2 py-0.5 rounded text-[10px] font-bold ${badgeColor}">${t.status}</span></td>
                        </tr>
                    `;
                });
                html += `</tbody></table>`;
                container.innerHTML = html;
            } else {
                let opMap = {};
                trains.forEach(t => {
                    const op = t.opNum || '未割当';
                    if(!opMap[op]) opMap[op] = [];
                    opMap[op].push(t);
                });

                let html = `
                    <table class="w-full text-left text-xs">
                        <thead class="bg-slate-900 text-indigo-200 border-b border-slate-700">
                            <tr>
                                <th class="p-3 w-32 border-r border-slate-700">運用番号</th>
                                <th class="p-3">運用順・時間順 走行列車リスト（列車番号 / 種別 / 行き先）</th>
                            </tr>
                        </thead>
                        <tbody class="divide-y divide-slate-700 text-slate-300">
                `;

                for(let op in opMap) {
                    let opTrains = opMap[op];
                    opTrains.sort((a,b) => (a.depTime || "").localeCompare(b.depTime || ""));

                    html += `
                        <tr class="hover:bg-slate-750 align-top">
                            <td class="p-3 font-mono font-bold text-sky-300 text-sm bg-slate-900/50 border-r border-slate-700">
                                <div class="bg-indigo-950 px-2 py-1 rounded border border-indigo-800 text-center">${op}</div>
                                <div class="text-[10px] text-slate-400 mt-1 text-center font-normal">担当列車: ${opTrains.length}本</div>
                            </td>
                            <td class="p-3">
                                <div class="space-y-2">
                    `;

                    opTrains.forEach((t, idx) => {
                        const endName = allStationsMaster.find(s => s.id === t.endSt)?.name || t.endSt;
                        const startName = allStationsMaster.find(s => s.id === t.startSt)?.name || t.startSt;
                        const typeClass = `type-${t.type}`;

                        html += `
                            <div class="flex flex-wrap items-center justify-between bg-slate-900/70 p-2 rounded border border-slate-700/80 gap-2">
                                <div class="flex items-center space-x-3">
                                    <span class="text-xs font-mono text-slate-400">#${idx + 1}</span>
                                    <span class="font-bold text-indigo-300 font-mono text-sm">${t.trainNum}</span>
                                    <span class="px-2 py-0.5 rounded text-[10px] font-bold ${typeClass}">${t.type}</span>
                                    <span class="font-mono text-indigo-300 text-[11px]">${t.depTime || '08:00'}発 〜 ${t.arrTime || '09:30'}着</span>
                                </div>
                                <div class="text-slate-200 font-medium">
                                    ${startName}発 → <span class="text-indigo-200 font-bold">${endName}行</span>
                                </div>
                                <div class="text-[10px] text-indigo-400 font-mono bg-indigo-950 px-2 py-0.5 rounded border border-indigo-900">
                                    編成: ${t.consistNum || '-'}
                                </div>
                            </div>
                        `;
                    });

                    html += `
                                </div>
                            </td>
                        </tr>
                    `;
                }

                html += `</tbody></table>`;
                container.innerHTML = html;
            }
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
                container.innerHTML = `<p class="text-slate-500 text-center py-8">列車が登録されていません。</p>`;
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
                    html += `<td class="py-1.5 px-2 border-r border-slate-800 font-mono text-slate-300 text-[11px]">${t.depTime || '08:10'}</td>`;
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
                        <tr class="border-b border-slate-800"><td class="p-2 font-mono font-bold text-indigo-400">07</td><td class="p-2">05(特急) 18(快速) 32(普通)</td></tr>
                        <tr class="border-b border-slate-800"><td class="p-2 font-mono font-bold text-indigo-400">08</td><td class="p-2">02(特急) 15(普通) 30(快速)</td></tr>
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

            // 走行位置タブが表示されている場合は、リアルタイムの時刻進行に合わせて位置を自動更新
            if(document.getElementById('tab-operation').classList.contains('active')) {
                renderOperationTrack();
            }
        }

        // 時刻文字列（"08:30"など）を分単位に変換するヘルパー
        function timeToMinutes(timeStr) {
            if (!timeStr || !timeStr.includes(':')) return 0;
            const parts = timeStr.split(':');
            return parseInt(parts[0], 10) * 60 + parseInt(parts[1], 10);
        }

        /* --- 走行位置スクリプト（完全自動時間連動版） --- */
        function renderOperationTrack() {
            const containerParent = document.getElementById('track-container-parent');
            if(!containerParent) return;

            const selectedLine = document.getElementById('op-line-select').value;
            const viewMode = document.getElementById('op-view-mode').value;

            let targetStations = allStationsMaster;
            if(selectedLine === 'main') {
                targetStations = allStationsMaster.filter(st => {
                    const idNum = parseInt(st.id);
                    return !isNaN(idNum) && idNum >= 1 && idNum <= 30;
                });
            } else if(selectedLine === 'hoshiku') {
                targetStations = allStationsMaster.filter(st => {
                    const idNum = parseInt(st.id);
                    return !isNaN(idNum) && idNum >= 31 && idNum <= 40;
                });
            } else if(selectedLine === 'shikan') {
                targetStations = allStationsMaster.filter(st => {
                    const idNum = parseInt(st.id);
                    return !isNaN(idNum) && idNum >= 41 && idNum <= 55;
                });
            } else if(selectedLine === 'shikasumitour') {
                targetStations = allStationsMaster.filter(st => {
                    const idNum = parseInt(st.id);
                    return !isNaN(idNum) && idNum >= 56 && idNum <= 60;
                });
            } else if(selectedLine === 'direct') {
                targetStations = allStationsMaster.filter(st => st.id.startsWith("0"));
            }

            const totalStations = targetStations.length;
            const containerHeight = Math.max(1200, totalStations * 40 + 100);
            containerParent.style.height = `${containerHeight}px`;

            let html = `<div class="vertical-rail" style="height: ${containerHeight - 80}px;"></div>`;

            const spacing = (containerHeight - 80) / Math.max(1, (totalStations - 1));
            targetStations.forEach((st, idx) => {
                const topPos = 40 + (idx * spacing);
                html += `
                    <div class="v-station-node" style="top: ${topPos}px;">
                        <div class="v-station-bar"></div>
                        <div class="v-station-dot"></div>
                        <div class="v-station-label">${st.id}. ${st.name}</div>
                    </div>
                `;
            });

            const trains = appData.trains || [];
            const now = new Date();
            const currentMinutes = now.getHours() * 60 + now.getMinutes() + now.getSeconds() / 60; // 秒単位までスムーズに計算

            trains.forEach((t, i) => {
                // 1. 発着時間による表示・非表示判定
                if (t.depTime && t.arrTime) {
                    const depMin = timeToMinutes(t.depTime);
                    const arrMin = timeToMinutes(t.arrTime);
                    if (currentMinutes < depMin || currentMinutes > arrMin) {
                        return; // 運行時間外のため非表示
                    }

                    // 2. 運行時間内であれば、経過割合に応じて駅間を自動で移動させる
                    const totalDuration = Math.max(1, arrMin - depMin);
                    const elapsed = currentMinutes - depMin;
                    const progressRate = Math.max(0, Math.min(1, elapsed / totalDuration));

                    let sIdx = allStationsMaster.findIndex(s => s.id === t.startSt);
                    let eIdx = allStationsMaster.findIndex(s => s.id === t.endSt);
                    if(sIdx === -1) sIdx = 0;
                    if(eIdx === -1) eIdx = allStationsMaster.length - 1;

                    // 路線上のインデックス進行位置を算出
                    const stationCountSpan = Math.abs(eIdx - sIdx);
                    const calculatedStationPosIdx = sIdx + (eIdx > sIdx ? progressRate * stationCountSpan : -progressRate * stationCountSpan);
                    t._realtimeStationIdx = calculatedStationPosIdx;
                } else {
                    t._realtimeStationIdx = 0;
                }

                // 現在位置の駅オブジェクトを取得
                const floorIdx = Math.floor(t._realtimeStationIdx);
                const currentStObj = allStationsMaster[floorIdx] || allStationsMaster[0];

                if(selectedLine !== 'all') {
                    const exists = targetStations.some(st => st.id === currentStObj.id);
                    if(!exists) return;
                    t.filteredIdx = targetStations.findIndex(st => st.id === currentStObj.id);
                } else {
                    t.filteredIdx = allStationsMaster.findIndex(st => st.id === currentStObj.id);
                }

                if(t.filteredIdx === -1) t.filteredIdx = 0;
                // 小数点以下の進捗を反映して駅間を滑らかに移動
                const decimalPart = t._realtimeStationIdx - floorIdx;
                const topPos = 40 + ((t.filteredIdx + decimalPart) * spacing);

                const isUp = (i % 2 === 0);
                const destName = allStationsMaster.find(s => s.id === t.endSt)?.name || t.endSt;
                const typeClass = `type-${t.type}`;
                const opNum = t.opNum || '73K';
                const consistShort = t.consistNum ? t.consistNum.split(' ')[0] : 'S100-01';

                if(viewMode === 'card') {
                    html += `
                        <div class="v-train ${isUp ? 'up-train' : 'down-train'}" style="top: ${topPos}px;" onclick='openTrainModal(${JSON.stringify(t)})'>
                            <div class="train-icon-badge">
                                <div class="train-op-num">${opNum}</div>
                                <span class="train-type-tag ${typeClass}">${t.type}</span>
                            </div>
                            <div class="train-card-v">
                                <div class="train-num-top">${t.trainNum} <span class="text-slate-500 font-normal">(${consistShort})</span></div>
                                <div class="font-bold text-slate-800">${destName}行</div>
                                <div class="text-[9px] text-pink-600 font-semibold">📍 ${currentStObj.name}付近</div>
                            </div>
                        </div>
                    `;
                } else if(viewMode === 'icon') {
                    html += `
                        <div class="v-train ${isUp ? 'up-train' : 'down-train'}" style="top: ${topPos}px;" onclick='openTrainModal(${JSON.stringify(t)})'>
                            <div class="train-icon-badge" title="${t.trainNum}: ${t.type} (${currentStObj.name})">
                                <div class="train-op-num">${opNum}</div>
                                <span class="train-type-tag ${typeClass}">${t.trainNum}</span>
                            </div>
                        </div>
                    `;
                } else {
                    html += `
                        <div class="v-train ${isUp ? 'up-train' : 'down-train'}" style="top: ${topPos}px;" onclick='openTrainModal(${JSON.stringify(t)})'>
                            <span class="bg-slate-900 text-indigo-200 border border-indigo-700 px-2 py-1 rounded text-[10px] font-bold font-mono">
                                ${t.trainNum} (${t.type}) - ${currentStObj.name}付近
                            </span>
                        </div>
                    `;
                }
            });

            containerParent.innerHTML = html;
        }

        /* --- 列車詳細モーダル --- */
        function openTrainModal(train) {
            const modal = document.getElementById('train-modal');
            const numEl = document.getElementById('modal-train-num');
            const opEl = document.getElementById('modal-op-num');
            const contentEl = document.getElementById('modal-content');

            numEl.innerText = `列車番号: ${train.trainNum}`;
            opEl.innerText = `運用番号: ${train.opNum || '73K'}`;

            const destName = allStationsMaster.find(s => s.id === train.endSt)?.name || train.endSt;
            const curStName = allStationsMaster[Math.floor(train._realtimeStationIdx || 0)]?.name || '不明';
            const startStName = allStationsMaster.find(s => s.id === train.startSt)?.name || train.startSt;

            const stopsLists = officialStopsMaster[train.type] || officialStopsMaster["普通"];
            const stopIds = stopsLists[0];

            let stopsHtml = `
                <div class="mt-3 border-t border-slate-700 pt-3">
                    <span class="text-xs text-indigo-300 font-bold block mb-2">🚉 停車駅および発着時間スケジュール</span>
                    <div class="bg-slate-900 rounded border border-slate-700 max-h-40 overflow-y-auto p-2 text-xs">
                        <table class="w-full text-left">
                            <tr class="text-slate-400 border-b border-slate-800">
                                <th class="p-1">駅名</th>
                                <th class="p-1">到着</th>
                                <th class="p-1">発車</th>
                            </tr>
            `;

            let sIdx = allStationsMaster.findIndex(s => s.id === train.startSt);
            let eIdx = allStationsMaster.findIndex(s => s.id === train.endSt);
            if(sIdx === -1) sIdx = 0;
            if(eIdx === -1) eIdx = allStationsMaster.length - 1;
            if(sIdx > eIdx) { let tmp = sIdx; sIdx = eIdx; eIdx = tmp; }

            for(let i = sIdx; i <= eIdx; i++) {
                const st = allStationsMaster[i];
                const isStop = stopIds.includes(st.id) || i === sIdx || i === eIdx;
                if(!isStop) continue;

                let arrTime = i === sIdx ? '-' : `08:${String(10 + i * 2).padStart(2,'0')}`;
                let depTime = i === eIdx ? '-' : `08:${String(12 + i * 2).padStart(2,'0')}`;

                stopsHtml += `
                    <tr class="border-b border-slate-800/50 hover:bg-slate-800">
                        <td class="p-1 font-bold text-slate-200">${st.id}. ${st.name}</td>
                        <td class="p-1 font-mono text-indigo-300">${arrTime}</td>
                        <td class="p-1 font-mono text-indigo-300">${depTime}</td>
                    </tr>
                `;
            }
            stopsHtml += `</table></div></div>`;

            contentEl.innerHTML = `
                <div class="bg-slate-900 p-3 rounded-lg border border-slate-700 space-y-2 text-xs">
                    <div class="flex justify-between"><span class="text-slate-400">列車番号:</span> <span class="font-bold text-indigo-300">${train.trainNum}</span></div>
                    <div class="flex justify-between"><span class="text-slate-400">運用番号:</span> <span class="font-mono text-sky-300">${train.opNum || '73K'}</span></div>
                    <div class="flex justify-between"><span class="text-slate-400">編成番号:</span> <span class="font-mono text-indigo-400">${train.consistNum || 'S100-01'}</span></div>
                    <div class="flex justify-between"><span class="text-slate-400">種別:</span> <span class="font-bold text-emerald-300">${train.type}</span></div>
                    <div class="flex justify-between"><span class="text-slate-400">区間:</span> <span class="text-slate-200">${startStName}発 〜 ${destName}行</span></div>
                    <div class="flex justify-between"><span class="text-slate-400">運行時間:</span> <span class="font-mono text-indigo-300">${train.depTime || '08:00'}発 〜 ${train.arrTime || '09:30'}着</span></div>
                    <div class="flex justify-between"><span class="text-slate-400">両数:</span> <span class="font-mono text-indigo-200">${train.consistNum ? train.consistNum.split(' ')[1] || '10両' : '10両'}</span></div>
                    <div class="flex justify-between"><span class="text-slate-400">現在の状態:</span> <span class="font-bold text-amber-300">${train.status} (📍 ${curStName}付近)</span></div>
                </div>
                ${stopsHtml}
            `;
            modal.classList.remove('hidden');
        }

        function closeTrainModal() {
            document.getElementById('train-modal').classList.add('hidden');
        }

        window.onload = function() {
            initFirebase();
            updateAddStationDropdowns();
            updateConsistNumbers(1);
            updateConsistNumbers(2);
            initTimetableDropdowns();
            setInterval(updateLiveDateTime, 1000); // 1秒ごとに時刻と走行位置を自動更新
            updateLiveDateTime();
            renderOperationTrack();
        };
    </script>
</body>
</html>
