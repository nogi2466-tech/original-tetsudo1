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

        /* --- Geminiサイドメニュー風 スマホ用オーバーレイメニュー --- */
        #nav-menu {
            transition: transform 0.3s ease-in-out, opacity 0.3s ease-in-out;
        }
        @media (max-width: 767px) {
            #nav-menu {
                display: flex;
                flex-direction: column;
                position: fixed;
                top: 0;
                left: 0;
                bottom: 0;
                width: 280px;
                background: #0f172a;
                padding: 20px 16px;
                border-right: 1px solid #334155;
                box-shadow: 10px 0 25px rgba(0,0,0,0.6);
                z-index: 100;
                gap: 8px;
                transform: translateX(-100%);
                opacity: 0;
                pointer-events: none;
                overflow-y: auto;
            }
            #nav-menu.mobile-open {
                transform: translateX(0);
                opacity: 1;
                pointer-events: auto;
            }
            #nav-menu .tab-btn {
                width: 100% !important;
                text-align: left !important;
                padding: 12px 16px !important;
                border-radius: 9999px !important;
                font-size: 14px !important;
                font-weight: 500 !important;
                white-space: nowrap !important;
                display: flex !important;
                align-items: center !important;
                gap: 12px;
                box-sizing: border-box;
            }
        }

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

    <div id="menu-backdrop" onclick="toggleMenu()" class="fixed inset-0 bg-slate-950/60 z-40 hidden md:hidden transition-opacity"></div>

    <header class="bg-indigo-950 border-b border-indigo-800 p-4 shadow-lg flex justify-between items-center gap-4 relative z-50">
        <div class="flex items-center space-x-3">
            <span class="text-3xl">🚄</span>
            <div>
                <h1 class="text-xl font-bold tracking-wider text-indigo-200">紫句守鉄道 <span class="text-xs font-normal text-indigo-400">しのもり鉄道</span></h1>
                <p class="text-xs text-slate-400">総合運行管理システム</p>
            </div>
        </div>

        <div class="flex items-center gap-3 relative">
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

            <nav id="nav-menu" class="hidden md:flex bg-slate-900 md:bg-transparent border-b md:border-b-0 border-slate-800 px-4 md:px-0 py-2 md:py-0 overflow-x-auto space-x-0 md:space-x-1 items-center">
                <div class="flex md:hidden items-center justify-between pb-4 mb-2 border-b border-slate-800 w-full px-2">
                    <div class="flex items-center space-x-2">
                        <span class="text-xl">🚄</span>
                        <span class="font-bold text-indigo-200 text-sm">メニュー</span>
                    </div>
                    <button onclick="toggleMenu()" class="text-slate-400 hover:text-white p-1">✕</button>
                </div>

                <button onclick="switchTab('about')" class="tab-btn px-3 py-2 rounded-lg text-xs md:text-sm font-medium transition bg-indigo-600 text-white shadow text-left md:text-center" data-tab="about">
                    <span class="md:hidden">🏢</span> 会社について
                </button>
                <button onclick="switchTab('timetable')" class="tab-btn px-3 py-2 rounded-lg text-xs md:text-sm font-medium transition text-slate-400 hover:text-white hover:bg-slate-800 text-left md:text-center" data-tab="timetable">
                    <span class="md:hidden">🕒</span> 時刻表
                </button>
                <button onclick="switchTab('operation')" class="tab-btn px-3 py-2 rounded-lg text-xs md:text-sm font-medium transition text-slate-400 hover:text-white hover:bg-slate-800 text-left md:text-center" data-tab="operation">
                    <span class="md:hidden">📍</span> 走行位置
                </button>
                <button onclick="switchTab('traininfo')" class="tab-btn px-3 py-2 rounded-lg text-xs md:text-sm font-medium transition text-slate-400 hover:text-white hover:bg-slate-800 text-left md:text-center" data-tab="traininfo">
                    <span class="md:hidden">🚆</span> 列車情報
                </button>
                <button onclick="switchTab('addtrain')" class="tab-btn px-3 py-2 rounded-lg text-xs md:text-sm font-medium transition text-slate-400 hover:text-white hover:bg-slate-800 text-left md:text-center" data-tab="addtrain">
                    <span class="md:hidden">➕</span> 列車追加
                </button>
                <button onclick="switchTab('consist')" class="tab-btn px-3 py-2 rounded-lg text-xs md:text-sm font-medium transition text-slate-400 hover:text-white hover:bg-slate-800 text-left md:text-center" data-tab="consist">
                    <span class="md:hidden">📋</span> 編成表
                </button>
                <button onclick="switchTab('dia')" class="tab-btn px-3 py-2 rounded-lg text-xs md:text-sm font-medium transition text-slate-400 hover:text-white hover:bg-slate-800 text-left md:text-center" data-tab="dia">
                    <span class="md:hidden">📊</span> ダイヤ表
                </button>
                <button onclick="switchTab('settings')" class="tab-btn px-3 py-2 rounded-lg text-xs md:text-sm font-medium transition text-slate-400 hover:text-white hover:bg-slate-800 text-left md:text-center" data-tab="settings">
                    <span class="md:hidden">⚙️</span> 設定
                </button>
            </nav>
        </div>
    </header>

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
                        <li><span class="text-slate-400 inline-block w-28">紫雲本線</span> 駅ID: 1～30</li>
                        <li><span class="text-slate-400 inline-block w-28">星句高原線</span> 駅ID: 31～40</li>
                        <li><span class="text-slate-400 inline-block w-28">句守支線</span> 駅ID: 41～55</li>
                        <li><span class="text-slate-400 inline-block w-28">紫霞観光線</span> 駅ID: 56～60</li>
                        <li><span class="text-slate-400 inline-block w-28">直通路線</span> 駅ID: 09～1</li>
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

        <!-- 4. 列車情報 (操作モード追加) -->
        <div id="tab-traininfo" class="tab-content space-y-4">
            <div class="flex flex-wrap items-center justify-between gap-4">
                <h2 class="text-xl font-bold text-indigo-200">列車情報一覧（操作モード対応）</h2>
                <div class="flex flex-wrap items-center gap-3">
                    <div class="flex items-center gap-2">
                        <label class="text-xs text-indigo-300 font-bold">操作モード:</label>
                        <select id="train-op-mode" class="bg-indigo-950 border border-indigo-700 rounded px-3 py-1.5 text-xs text-indigo-200 font-bold">
                            <option value="normal">🔍 通常（詳細表示）</option>
                            <option value="edit">✏️ 編集（フォームへ反映）</option>
                            <option value="delete">🗑️ 削除（個別削除）</option>
                            <option value="copy">📋 複製（コピー作成）</option>
                        </select>
                    </div>
                    <div class="flex items-center gap-2">
                        <label class="text-xs text-slate-400">表示順:</label>
                        <select id="train-view-mode" onchange="renderTrainInfoTable()" class="bg-slate-800 border border-slate-700 rounded px-3 py-1.5 text-xs text-white font-medium">
                            <option value="trainnum">列車番号順一覧</option>
                            <option value="opnum">運用番号順一覧（時間順運用流れ）</option>
                        </select>
                    </div>
                </div>
            </div>

            <div class="bg-slate-800 rounded-xl border border-slate-700 overflow-hidden shadow-xl">
                <div id="train-info-container" class="overflow-x-auto"></div>
            </div>
        </div>

        <!-- 5. 列車追加 -->
        <div id="tab-addtrain" class="tab-content space-y-4">
            <div class="flex justify-between items-center">
                <h2 id="add-train-title" class="text-xl font-bold text-indigo-200">新規列車運用追加・詳細設定</h2>
                <button type="button" onclick="cancelEditMode()" id="cancel-edit-btn" class="hidden bg-slate-700 hover:bg-slate-600 text-white px-3 py-1 rounded text-xs transition">編集をキャンセルして新規に戻す</button>
            </div>
            <div class="bg-slate-800 p-6 rounded-xl border border-slate-700 max-w-3xl space-y-5">
                
                <div>
                    <label class="block text-xs text-slate-400 mb-1">列車番号</label>
                    <input type="text" id="add-train-num" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white" value="101M">
                </div>

                <div>
                    <label class="block text-xs text-slate-400 mb-1">運用番号</label>
                    <input type="text" id="add-op-num" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white" value="73K">
                </div>

                <div>
                    <label class="block text-xs text-slate-400 mb-1">種別</label>
                    <select id="add-train-type" onchange="renderStationScheduleInputs()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
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
                    <label class="block text-xs text-slate-400 mb-1">運行日</label>
                    <select id="add-run-day" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                        <option value="weekday">平日</option>
                        <option value="holiday">土休日</option>
                        <option value="event">イベント日</option>
                        <option value="newyear">年末年始</option>
                    </select>
                </div>

                <div>
                    <label class="block text-xs text-slate-400 mb-1">上下方向</label>
                    <select id="add-train-dir" onchange="renderStationScheduleInputs()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white">
                        <option value="down">下り (起点 → 終点)</option>
                        <option value="up">上り (終点 → 起点)</option>
                    </select>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="space-y-2">
                        <label class="block text-xs text-indigo-300 font-bold">始点駅</label>
                        <select id="add-start-station" onchange="renderStationScheduleInputs()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white"></select>
                    </div>
                    <div class="space-y-2">
                        <label class="block text-xs text-indigo-300 font-bold">終点駅（行き先）</label>
                        <select id="add-end-station" onchange="renderStationScheduleInputs()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white"></select>
                    </div>
                </div>

                <div class="border border-slate-700 p-4 rounded-lg bg-slate-900 space-y-3">
                    <div class="flex justify-between items-center">
                        <span class="text-xs text-indigo-300 font-bold">形式・編成番号設定</span>
                        <label class="flex items-center space-x-2 text-xs text-amber-300 cursor-pointer">
                            <input type="checkbox" id="check-coupling" onchange="toggleCouplingBox()" class="rounded bg-slate-800 border-slate-700 text-indigo-600">
                            <span>🔗 連結列車チェック（2編成連結）</span>
                        </label>
                    </div>

                    <div id="box-single-consist" class="grid grid-cols-2 gap-2 pt-2">
                        <select id="consist-series-1" onchange="updateConsistNumbers(1)" class="bg-slate-800 border border-slate-700 rounded px-2 py-1.5 text-xs text-white">
                            <option value="S1">S1系</option><option value="S2">S2系</option><option value="S3">S3系</option><option value="S4">S4系</option><option value="S5">S5系</option><option value="S100" selected>S100系</option><option value="S900">S900系</option>
                        </select>
                        <select id="consist-number-1" class="bg-slate-800 border border-slate-700 rounded px-2 py-1.5 text-xs text-white"></select>
                    </div>

                    <div id="box-double-consist" class="grid grid-cols-1 md:grid-cols-2 gap-4 pt-2 hidden">
                        <div class="space-y-1 bg-slate-800/80 p-3 rounded border border-slate-700">
                            <span class="text-[11px] text-indigo-300 font-bold block">【前部編成】</span>
                            <div class="grid grid-cols-2 gap-2">
                                <select id="coupling-series-1" onchange="updateCouplingConsistNumbers(1)" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white">
                                    <option value="S1">S1系</option><option value="S2">S2系</option><option value="S3">S3系</option><option value="S4">S4系</option><option value="S5">S5系</option><option value="S100" selected>S100系</option>
                                </select>
                                <select id="coupling-number-1" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white"></select>
                            </div>
                        </div>
                        <div class="space-y-1 bg-slate-800/80 p-3 rounded border border-slate-700">
                            <span class="text-[11px] text-sky-300 font-bold block">【後部編成】</span>
                            <div class="grid grid-cols-2 gap-2">
                                <select id="coupling-series-2" onchange="updateCouplingConsistNumbers(2)" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white">
                                    <option value="S1">S1系</option><option value="S2">S2系</option><option value="S3">S3系</option><option value="S4" selected>S4系</option><option value="S5">S5系</option><option value="S100">S100系</option>
                                </select>
                                <select id="coupling-number-2" class="bg-slate-900 border border-slate-700 rounded px-2 py-1 text-xs text-white"></select>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="border border-indigo-700/60 p-4 rounded-lg bg-indigo-950/40 space-y-3">
                    <label class="flex items-center space-x-2 text-xs text-indigo-200 font-bold cursor-pointer">
                        <input type="checkbox" id="check-decoupling" onchange="toggleWorkCheck()" class="rounded bg-slate-900 border-slate-700 text-indigo-600">
                        <span>✂ 切り離し・連結作業チェック</span>
                    </label>

                    <div id="box-work-details" class="space-y-3 pt-2 hidden">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-3 text-xs">
                            <div>
                                <label class="block text-[11px] text-slate-400 mb-1">作業内容</label>
                                <select id="work-type" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-white">
                                    <option value="decoupling">切り離し作業</option>
                                    <option value="coupling">連結作業</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-[11px] text-slate-400 mb-1">作業駅</label>
                                <select id="work-station" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-white"></select>
                            </div>
                        </div>

                        <div class="grid grid-cols-1 md:grid-cols-2 gap-3 text-xs">
                            <div>
                                <label class="block text-[11px] text-indigo-300 mb-1">前部車両の行き先</label>
                                <select id="work-front-dest" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-white"></select>
                            </div>
                            <div>
                                <label class="block text-[11px] text-sky-300 mb-1">後部車両の行き先</label>
                                <select id="work-rear-dest" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-white"></select>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="border border-sky-700/60 p-4 rounded-lg bg-sky-950/40 space-y-3">
                    <label class="flex items-center space-x-2 text-xs text-sky-200 font-bold cursor-pointer">
                        <input type="checkbox" id="check-changetype" onchange="toggleTypeChangeBox()" class="rounded bg-slate-900 border-slate-700 text-sky-600">
                        <span>🔄 種別変更チェック</span>
                    </label>

                    <div id="box-type-change" class="grid grid-cols-1 md:grid-cols-3 gap-3 pt-2 hidden text-xs">
                        <div>
                            <label class="block text-[11px] text-slate-400 mb-1">変更駅</label>
                            <select id="change-station" onchange="renderStationScheduleInputs()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-white"></select>
                        </div>
                        <div>
                            <label class="block text-[11px] text-slate-400 mb-1">変更後の種別</label>
                            <select id="add-changed-type" onchange="renderStationScheduleInputs()" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-white">
                                <option value="普通">普通</option>
                                <option value="準急">準急</option>
                                <option value="快速">快速</option>
                                <option value="急行">急行</option>
                                <option value="特急">特急</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-[11px] text-slate-400 mb-1">変更後の列車番号</label>
                            <input type="text" id="add-changed-trainnum" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-white" value="101M">
                        </div>
                    </div>
                </div>

                <div class="border border-slate-700 p-4 rounded-lg bg-slate-900 space-y-3">
                    <div class="flex flex-wrap justify-between items-center gap-2">
                        <div class="flex items-center gap-2">
                            <span class="text-xs text-indigo-300 font-bold">🚉 停車駅および到着・発車時間設定</span>
                            <div id="vehicle-tabs" class="hidden flex bg-slate-800 p-0.5 rounded border border-slate-700 text-xs">
                                <button type="button" id="tab-v-front" onclick="switchVehicleTab('front')" class="px-2.5 py-1 rounded bg-indigo-600 text-white font-bold transition">前部車両スケジュール</button>
                                <button type="button" id="tab-v-rear" onclick="switchVehicleTab('rear')" class="px-2.5 py-1 rounded text-slate-400 hover:text-white transition">後部車両スケジュール</button>
                            </div>
                        </div>
                        <div class="flex items-center gap-2">
                            <input type="time" id="add-dep-time" class="bg-slate-800 border border-slate-700 rounded px-2 py-1 text-xs text-white" value="08:00">
                            <button type="button" onclick="generateStationTimes()" class="bg-slate-800 hover:bg-slate-700 text-indigo-200 px-3 py-1 rounded text-xs border border-slate-600 transition">簡易自動生成</button>
                        </div>
                    </div>

                    <div id="station-schedule-list-front" class="space-y-2 max-h-56 overflow-y-auto pr-2 pt-2 border-t border-slate-800"></div>
                    <div id="station-schedule-list-rear" class="space-y-2 max-h-56 overflow-y-auto pr-2 pt-2 border-t border-slate-800 hidden"></div>
                </div>

                <button onclick="addNewTrain()" id="submit-train-btn" class="w-full bg-indigo-600 hover:bg-indigo-500 text-white font-medium py-2.5 rounded-lg transition shadow text-sm">列車を登録して一覧・ダイヤグラムに反映</button>
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
            
            <div id="modal-content" class="space-y-3 text-sm text-slate-300"></div>

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
            "特急": ["1", "3", "13", "16", "17", "23", "28", "30"],
            "通勤急行": ["1", "3", "6", "9", "13", "16", "17", "20", "23", "28", "30"],
            "急行": ["1", "3", "6", "9", "13", "16", "17", "20", "23", "28", "30"],
            "通勤快速": ["1", "3", "5", "6", "7", "9", "13", "16", "17", "20", "23", "24", "28", "30"],
            "快速": ["1", "3", "5", "6", "7", "9", "13", "16", "17", "20", "23", "24", "28", "30"],
            "準急": ["1", "3", "5", "6", "7", "9", "11", "13", "16", "17", "19", "20", "23", "24", "25", "26", "28", "29", "30"],
            "普通": allStationsMaster.map(s => s.id)
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
            const backdrop = document.getElementById('menu-backdrop');
            menu.classList.toggle('mobile-open');
            backdrop.classList.toggle('hidden');
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
            if(menu.classList.contains('mobile-open')) toggleMenu();

            if (tabId === 'dia') renderMatrixTimetable();
            if (tabId === 'timetable') initTimetableDropdowns();
            if (tabId === 'traininfo') renderTrainInfoTable();
            if (tabId === 'consist') renderConsistMatrix();
            if (tabId === 'settings') renderEventDatesList();
            if (tabId === 'operation') renderOperationTrack();
            if (tabId === 'addtrain') {
                updateAddStationDropdowns();
                if(!editingTrainId) renderStationScheduleInputs();
            }
        }

        let appData = {
            trains: [
                { id: 1, trainNum: "101M", opNum: "73K", type: "特急", runDay: "weekday", dir: "down", startSt: "1", endSt: "30", depTime: "06:00", arrTime: "23:59", consistNum: "S100-01 (10両)", status: "走行中", schedule: {} },
                { id: 2, trainNum: "104M", opNum: "54K", type: "快速", runDay: "weekday", dir: "up", startSt: "30", endSt: "1", depTime: "06:00", arrTime: "23:59", consistNum: "S2-01 (10両)", status: "走行中", schedule: {} }
            ],
            eventDates: ["2026-10-15"]
        };
        let dbRef = null;
        let editingTrainId = null;
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
            document.getElementById('work-station').innerHTML = options;
            document.getElementById('work-front-dest').innerHTML = options;
            document.getElementById('work-rear-dest').innerHTML = options;
            document.getElementById('change-station').innerHTML = options;
            updateConsistNumbers(1);
            updateCouplingConsistNumbers(1);
            updateCouplingConsistNumbers(2);
        }

        function updateConsistNumbers(num) {
            const series = document.getElementById(`consist-series-${num}`).value;
            const sel = document.getElementById(`consist-number-${num}`);
            let found = fullConsistsData.find(c => c.series.startsWith(series));
            if(found) {
                sel.innerHTML = found.items.map(it => `<option>${it}</option>`).join('');
            }
        }

        function updateCouplingConsistNumbers(num) {
            const series = document.getElementById(`coupling-series-${num}`).value;
            const sel = document.getElementById(`coupling-number-${num}`);
            let found = fullConsistsData.find(c => c.series.startsWith(series));
            if(found) {
                sel.innerHTML = found.items.map(it => `<option>${it}</option>`).join('');
            }
        }

        function toggleCouplingBox() {
            const isCoupling = document.getElementById('check-coupling').checked;
            document.getElementById('box-single-consist').classList.toggle('hidden', isCoupling);
            document.getElementById('box-double-consist').classList.toggle('hidden', !isCoupling);
        }

        function toggleWorkCheck() {
            const isWork = document.getElementById('check-decoupling').checked;
            document.getElementById('box-work-details').classList.toggle('hidden', !isWork);
            document.getElementById('vehicle-tabs').classList.toggle('hidden', !isWork);
            if(!isWork) switchVehicleTab('front');
        }

        function toggleTypeChangeBox() {
            const isChange = document.getElementById('check-changetype').checked;
            document.getElementById('box-type-change').classList.toggle('hidden', !isChange);
            renderStationScheduleInputs();
        }

        let activeVehicleTab = 'front';
        function switchVehicleTab(tab) {
            activeVehicleTab = tab;
            document.getElementById('tab-v-front').className = tab === 'front' ? 'px-2.5 py-1 rounded bg-indigo-600 text-white font-bold transition' : 'px-2.5 py-1 rounded text-slate-400 hover:text-white transition';
            document.getElementById('tab-v-rear').className = tab === 'rear' ? 'px-2.5 py-1 rounded bg-indigo-600 text-white font-bold transition' : 'px-2.5 py-1 rounded text-slate-400 hover:text-white transition';
            document.getElementById('station-schedule-list-front').classList.toggle('hidden', tab !== 'front');
            document.getElementById('station-schedule-list-rear').classList.toggle('hidden', tab !== 'rear');
        }

        function renderStationScheduleInputs(existingSchedule = null, existingScheduleRear = null) {
            const startId = document.getElementById('add-start-station').value;
            const endId = document.getElementById('add-end-station').value;
            const dir = document.getElementById('add-train-dir').value;
            const isChangeType = document.getElementById('check-changetype').checked;
            const changeStId = document.getElementById('change-station').value;
            const baseType = document.getElementById('add-train-type').value;
            const changedType = document.getElementById('add-changed-type').value;

            let sIdx = allStationsMaster.findIndex(s => s.id === startId);
            let eIdx = allStationsMaster.findIndex(s => s.id === endId);
            if(sIdx === -1) sIdx = 0;
            if(eIdx === -1) eIdx = allStationsMaster.length - 1;

            let currentSts = allStationsMaster.slice(Math.min(sIdx, eIdx), Math.max(sIdx, eIdx) + 1);
            if(dir === 'up') currentSts.reverse();

            const frontContainer = document.getElementById('station-schedule-list-front');
            const rearContainer = document.getElementById('station-schedule-list-rear');
            if(!frontContainer || !rearContainer) return;

            let htmlFront = '';
            let htmlRear = '';

            currentSts.forEach((st, idx) => {
                let activeType = baseType;
                if(isChangeType) {
                    let changeIndex = currentSts.findIndex(s => s.id === changeStId);
                    if(changeIndex !== -1 && idx >= changeIndex) {
                        activeType = changedType;
                    }
                }

                let stopsList = officialStopsMaster[activeType] || [];
                let isStop = stopsList.includes(st.id);
                let badge = isStop ? `<span class="text-[10px] text-emerald-300 bg-emerald-950 px-1.5 py-0.5 rounded border border-emerald-800">停車</span>` : `<span class="text-[10px] text-slate-500 bg-slate-800 px-1.5 py-0.5 rounded">通過</span>`;

                let arrVal = "08:00";
                let depVal = "08:01";
                if(existingSchedule && existingSchedule[st.id]) {
                    arrVal = existingSchedule[st.id].arr || "08:00";
                    depVal = existingSchedule[st.id].dep || "08:01";
                }

                let arrValR = "08:00";
                let depValR = "08:01";
                if(existingScheduleRear && existingScheduleRear[st.id]) {
                    arrValR = existingScheduleRear[st.id].arr || "08:00";
                    depValR = existingScheduleRear[st.id].dep || "08:01";
                }

                htmlFront += `
                    <div class="flex items-center justify-between bg-slate-800 p-2 rounded border border-slate-700 text-xs gap-2 st-sched-row" data-stid="${st.id}">
                        <div class="flex items-center gap-2 w-36 truncate">
                            <span class="font-bold text-indigo-200">${st.id}. ${st.name}</span>
                            ${badge}
                        </div>
                        <div class="flex items-center gap-1">
                            <span class="text-slate-400 text-[10px]">着</span>
                            <input type="time" class="st-arr bg-slate-900 border border-slate-700 rounded px-2 py-1 text-white text-xs font-mono" value="${arrVal}">
                        </div>
                        <div class="flex items-center gap-1">
                            <span class="text-slate-400 text-[10px]">発</span>
                            <input type="time" class="st-dep bg-slate-900 border border-slate-700 rounded px-2 py-1 text-white text-xs font-mono" value="${depVal}">
                        </div>
                    </div>
                `;

                htmlRear += `
                    <div class="flex items-center justify-between bg-slate-800 p-2 rounded border border-slate-700 text-xs gap-2 st-sched-row-rear" data-stid="${st.id}">
                        <div class="flex items-center gap-2 w-36 truncate">
                            <span class="font-bold text-indigo-200">${st.id}. ${st.name}</span>
                            ${badge}
                        </div>
                        <div class="flex items-center gap-1">
                            <span class="text-slate-400 text-[10px]">着</span>
                            <input type="time" class="st-arr-rear bg-slate-900 border border-slate-700 rounded px-2 py-1 text-white text-xs font-mono" value="${arrValR}">
                        </div>
                        <div class="flex items-center gap-1">
                            <span class="text-slate-400 text-[10px]">発</span>
                            <input type="time" class="st-dep-rear bg-slate-900 border border-slate-700 rounded px-2 py-1 text-white text-xs font-mono" value="${depValR}">
                        </div>
                    </div>
                `;
            });

            frontContainer.innerHTML = htmlFront;
            rearContainer.innerHTML = htmlRear;
        }

        function generateStationTimes() {
            const baseDep = document.getElementById('add-dep-time').value || "08:00";
            let [h, m] = baseDep.split(':').map(Number);

            ['station-schedule-list-front', 'station-schedule-list-rear'].forEach(containerId => {
                const container = document.getElementById(containerId);
                if(!container) return;
                const rows = container.querySelectorAll('.st-sched-row, .st-sched-row-rear');
                rows.forEach((row, idx) => {
                    let totalMin = h * 60 + m + (idx * 3);
                    let th = String(Math.floor(totalMin / 60) % 24).padStart(2, '0');
                    let tm = String(totalMin % 60).padStart(2, '0');
                    let timeStr = `${th}:${tm}`;

                    const arrInput = row.querySelector('.st-arr, .st-arr-rear');
                    const depInput = row.querySelector('.st-dep, .st-dep-rear');
                    if(arrInput) arrInput.value = timeStr;
                    if(depInput) depInput.value = timeStr;
                });
            });
        }

        function addNewTrain() {
            const trainNum = document.getElementById('add-train-num').value;
            const opNum = document.getElementById('add-op-num').value;
            const type = document.getElementById('add-train-type').value;
            const runDay = document.getElementById('add-run-day').value;
            const dir = document.getElementById('add-train-dir').value;
            const startSt = document.getElementById('add-start-station').value;
            const endSt = document.getElementById('add-end-station').value;
            
            const isCoupling = document.getElementById('check-coupling').checked;
            let consistStr = '';
            if(isCoupling) {
                consistStr = `${document.getElementById('coupling-number-1').value} ＋ ${document.getElementById('coupling-number-2').value}`;
            } else {
                consistStr = document.getElementById('consist-number-1').value;
            }

            const isWork = document.getElementById('check-decoupling').checked;
            const workType = isWork ? document.getElementById('work-type').value : null;
            const workStation = isWork ? document.getElementById('work-station').value : null;
            const workFrontDest = isWork ? document.getElementById('work-front-dest').value : null;
            const workRearDest = isWork ? document.getElementById('work-rear-dest').value : null;

            const isChangeType = document.getElementById('check-changetype').checked;
            const changeStation = isChangeType ? document.getElementById('change-station').value : null;
            const changedType = isChangeType ? document.getElementById('add-changed-type').value : null;
            const changedTrainNum = isChangeType ? document.getElementById('add-changed-trainnum').value : null;

            let scheduleFront = {};
            document.querySelectorAll('#station-schedule-list-front .st-sched-row').forEach(row => {
                const stId = row.getAttribute('data-stid');
                scheduleFront[stId] = { arr: row.querySelector('.st-arr').value, dep: row.querySelector('.st-dep').value };
            });

            let scheduleRear = {};
            if(isWork) {
                document.querySelectorAll('#station-schedule-list-rear .st-sched-row-rear').forEach(row => {
                    const stId = row.getAttribute('data-stid');
                    scheduleRear[stId] = { arr: row.querySelector('.st-arr-rear').value, dep: row.querySelector('.st-dep-rear').value };
                });
            }

            const firstRow = document.querySelector('#station-schedule-list-front .st-sched-row');
            const lastRows = document.querySelectorAll('#station-schedule-list-front .st-sched-row');
            const depTime = firstRow ? firstRow.querySelector('.st-dep').value : "08:00";
            const arrTime = lastRows.length > 0 ? lastRows[lastRows.length - 1].querySelector('.st-arr').value : "09:30";

            if(!appData.trains) appData.trains = [];

            const trainObj = {
                id: editingTrainId ? editingTrainId : Date.now(),
                trainNum, opNum, type, runDay, dir, startSt, endSt,
                depTime, arrTime, consistNum: consistStr,
                status: "走行中",
                isCoupling, isWork, workType, workStation, workFrontDest, workRearDest,
                isChangeType, changeStation, changedType, changedTrainNum,
                schedule: scheduleFront,
                scheduleRear: isWork ? scheduleRear : null
            };

            if(editingTrainId) {
                const idx = appData.trains.findIndex(t => t.id === editingTrainId);
                if(idx !== -1) appData.trains[idx] = trainObj;
                editingTrainId = null;
                document.getElementById('add-train-title').innerText = "新規列車運用追加・詳細設定";
                document.getElementById('cancel-edit-btn').classList.add('hidden');
                document.getElementById('submit-train-btn').innerText = "列車を登録して一覧・ダイヤグラムに反映";
            } else {
                appData.trains.push(trainObj);
            }

            pushData();
            updateUI();
            alert("列車情報を保存しました！");
            switchTab('traininfo');
        }

        function cancelEditMode() {
            editingTrainId = null;
            document.getElementById('add-train-title').innerText = "新規列車運用追加・詳細設定";
            document.getElementById('cancel-edit-btn').classList.add('hidden');
            document.getElementById('submit-train-btn').innerText = "列車を登録して一覧・ダイヤグラムに反映";
            renderStationScheduleInputs();
            alert("編集モードを解除しました。");
        }

        /* --- 列車情報クリック時の操作モード処理 --- */
        function handleTrainClick(trainId) {
            const mode = document.getElementById('train-op-mode').value;
            const train = (appData.trains || []).find(t => t.id === trainId);
            if(!train) return;

            if(mode === 'normal') {
                openTrainModal(train);
            } else if(mode === 'edit') {
                editingTrainId = train.id;
                document.getElementById('add-train-num').value = train.trainNum || '';
                document.getElementById('add-op-num').value = train.opNum || '';
                document.getElementById('add-train-type').value = train.type || '普通';
                document.getElementById('add-run-day').value = train.runDay || 'weekday';
                document.getElementById('add-train-dir').value = train.dir || 'down';
                document.getElementById('add-start-station').value = train.startSt || '1';
                document.getElementById('add-end-station').value = train.endSt || '30';

                if(train.isCoupling) {
                    document.getElementById('check-coupling').checked = true;
                    toggleCouplingBox();
                } else {
                    document.getElementById('check-coupling').checked = false;
                    toggleCouplingBox();
                }

                if(train.isWork) {
                    document.getElementById('check-decoupling').checked = true;
                    toggleWorkCheck();
                    document.getElementById('work-type').value = train.workType || 'decoupling';
                    document.getElementById('work-station').value = train.workStation || '1';
                    document.getElementById('work-front-dest').value = train.workFrontDest || '30';
                    document.getElementById('work-rear-dest').value = train.workRearDest || '30';
                } else {
                    document.getElementById('check-decoupling').checked = false;
                    toggleWorkCheck();
                }

                if(train.isChangeType) {
                    document.getElementById('check-changetype').checked = true;
                    toggleTypeChangeBox();
                    document.getElementById('change-station').value = train.changeStation || '1';
                    document.getElementById('add-changed-type').value = train.changedType || '普通';
                    document.getElementById('add-changed-trainnum').value = train.changedTrainNum || '';
                } else {
                    document.getElementById('check-changetype').checked = false;
                    toggleTypeChangeBox();
                }

                renderStationScheduleInputs(train.schedule, train.scheduleRear);

                document.getElementById('add-train-title').innerText = `列車編集モード (ID: ${train.trainNum})`;
                document.getElementById('cancel-edit-btn').classList.remove('hidden');
                document.getElementById('submit-train-btn').innerText = "変更を保存して反映";
                switchTab('addtrain');
            } else if(mode === 'delete') {
                if(confirm(`列車番号 [${train.trainNum}] （運用: ${train.opNum}）を削除しますか？`)) {
                    appData.trains = appData.trains.filter(t => t.id !== trainId);
                    pushData();
                    updateUI();
                }
            } else if(mode === 'copy') {
                const newTrain = JSON.parse(JSON.stringify(train));
                newTrain.id = Date.now();
                newTrain.trainNum = train.trainNum + '_copy';
                appData.trains.push(newTrain);
                pushData();
                updateUI();
                alert(`列車 [${train.trainNum}] を複製しました。`);
            }
        }

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
                                <th class="p-3">列車番号 (タップ操作)</th>
                                <th class="p-3">運用番号</th>
                                <th class="p-3">種別・特殊設定</th>
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

                    let tags = `<span class="px-1.5 py-0.5 rounded bg-slate-700 text-slate-200">${t.type}</span>`;
                    if(t.isCoupling) tags += ` <span class="text-[10px] text-amber-300 bg-amber-950 px-1 rounded border border-amber-800">連結</span>`;
                    if(t.isWork) tags += ` <span class="text-[10px] text-rose-300 bg-rose-950 px-1 rounded border border-rose-800">${t.workType === 'decoupling' ? '切離作業' : '連結作業'}</span>`;
                    if(t.isChangeType) tags += ` <span class="text-[10px] text-sky-300 bg-sky-950 px-1 rounded border border-sky-800">種別変(${t.changedType})</span>`;

                    html += `
                        <tr class="hover:bg-indigo-950/30 transition cursor-pointer" onclick="handleTrainClick(${t.id})">
                            <td class="p-3 font-bold text-indigo-300 underline decoration-indigo-500/50 hover:text-indigo-200">${t.trainNum} 🖱️</td>
                            <td class="p-3 font-mono">${t.opNum}</td>
                            <td class="p-3">${tags}</td>
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
                                <th class="p-3">運用順・時間順 走行列車リスト（タップで操作）</th>
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
                                <div class="text-[10px] text-slate-400 mt-1 text-center font-normal">担当: ${opTrains.length}本</div>
                            </td>
                            <td class="p-3"><div class="space-y-2">
                    `;

                    opTrains.forEach((t, idx) => {
                        const endName = allStationsMaster.find(s => s.id === t.endSt)?.name || t.endSt;
                        const startName = allStationsMaster.find(s => s.id === t.startSt)?.name || t.startSt;
                        const typeClass = `type-${t.type}`;

                        let tags = `<span class="px-2 py-0.5 rounded text-[10px] font-bold ${typeClass}">${t.type}</span>`;
                        if(t.isWork) tags += ` <span class="text-[9px] text-rose-300 bg-rose-950 px-1 rounded border border-rose-800">${t.workType}</span>`;

                        html += `
                            <div class="flex flex-wrap items-center justify-between bg-slate-900/70 p-2 rounded border border-slate-700/80 gap-2 cursor-pointer hover:border-indigo-500 transition" onclick="handleTrainClick(${t.id})">
                                <div class="flex items-center space-x-3">
                                    <span class="text-xs font-mono text-slate-400">#${idx + 1}</span>
                                    <span class="font-bold text-indigo-300 font-mono text-sm underline">${t.trainNum}</span>
                                    ${tags}
                                    <span class="font-mono text-indigo-300 text-[11px]">${t.depTime || '08:00'}発 〜 ${t.arrTime || '09:30'}着</span>
                                </div>
                                <div class="text-slate-200 font-medium">${startName}発 → <span class="text-indigo-200 font-bold">${endName}行</span></div>
                                <div class="text-[10px] text-indigo-400 font-mono bg-indigo-950 px-2 py-0.5 rounded border border-indigo-900">編成: ${t.consistNum || '-'}</div>
                            </div>
                        `;
                    });

                    html += `</div></td></tr>`;
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
                    let timeVal = '-';
                    if(t.schedule && t.schedule[st.id]) {
                        timeVal = t.schedule[st.id].dep || t.schedule[st.id].arr || '-';
                    }
                    html += `<td class="py-1.5 px-2 border-r border-slate-800 font-mono text-slate-300 text-[11px]">${timeVal}</td>`;
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

            if(document.getElementById('tab-operation').classList.contains('active')) {
                renderOperationTrack();
            }
        }

        function timeToMinutes(timeStr) {
            if (!timeStr || !timeStr.includes(':')) return 0;
            const parts = timeStr.split(':');
            return parseInt(parts[0], 10) * 60 + parseInt(parts[1], 10);
        }

        function renderOperationTrack() {
            const containerParent = document.getElementById('track-container-parent');
            if(!containerParent) return;

            const selectedLine = document.getElementById('op-line-select').value;
            const viewMode = document.getElementById('op-view-mode').value;

            let targetStations = allStationsMaster;
            if(selectedLine === 'main') {
                targetStations = allStationsMaster.filter(st => { const idNum = parseInt(st.id); return !isNaN(idNum) && idNum >= 1 && idNum <= 30; });
            } else if(selectedLine === 'hoshiku') {
                targetStations = allStationsMaster.filter(st => { const idNum = parseInt(st.id); return !isNaN(idNum) && idNum >= 31 && idNum <= 40; });
            } else if(selectedLine === 'shikan') {
                targetStations = allStationsMaster.filter(st => { const idNum = parseInt(st.id); return !isNaN(idNum) && idNum >= 41 && idNum <= 55; });
            } else if(selectedLine === 'shikasumitour') {
                targetStations = allStationsMaster.filter(st => { const idNum = parseInt(st.id); return !isNaN(idNum) && idNum >= 56 && idNum <= 60; });
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
            const currentMinutes = now.getHours() * 60 + now.getMinutes() + now.getSeconds() / 60;

            trains.forEach((t) => {
                if (!t.depTime || !t.arrTime) return;
                const depMin = timeToMinutes(t.depTime);
                const arrMin = timeToMinutes(t.arrTime);
                if (currentMinutes < depMin || currentMinutes > arrMin) return;

                const totalDuration = Math.max(1, arrMin - depMin);
                const elapsed = currentMinutes - depMin;
                const progressRate = Math.max(0, Math.min(1, elapsed / totalDuration));

                let sIdx = allStationsMaster.findIndex(s => s.id === t.startSt);
                let eIdx = allStationsMaster.findIndex(s => s.id === t.endSt);
                if(sIdx === -1) sIdx = 0;
                if(eIdx === -1) eIdx = allStationsMaster.length - 1;

                const stationCountSpan = Math.abs(eIdx - sIdx);
                const calculatedStationPosIdx = sIdx + (eIdx > sIdx ? progressRate * stationCountSpan : -progressRate * stationCountSpan);
                t._realtimeStationIdx = calculatedStationPosIdx;

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
                const decimalPart = t._realtimeStationIdx - floorIdx;
                const topPos = 40 + ((t.filteredIdx + decimalPart) * spacing);

                const isDownTrain = t.dir === 'down';
                const trainClass = isDownTrain ? 'down-train' : 'up-train';
                const destName = allStationsMaster.find(s => s.id === t.endSt)?.name || t.endSt;
                const typeClass = `type-${t.type}`;
                const opNum = t.opNum || '73K';
                const consistShort = t.consistNum ? t.consistNum.split(' ')[0] : 'S100-01';

                if(viewMode === 'card') {
                    html += `
                        <div class="v-train ${trainClass}" style="top: ${topPos}px;" onclick='openTrainModal(${JSON.stringify(t)})'>
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
                        <div class="v-train ${trainClass}" style="top: ${topPos}px;" onclick='openTrainModal(${JSON.stringify(t)})'>
                            <div class="train-icon-badge" title="${t.trainNum}: ${t.type} (${currentStObj.name})">
                                <div class="train-op-num">${opNum}</div>
                                <span class="train-type-tag ${typeClass}">${t.trainNum}</span>
                            </div>
                        </div>
                    `;
                } else {
                    html += `
                        <div class="v-train ${trainClass}" style="top: ${topPos}px;" onclick='openTrainModal(${JSON.stringify(t)})'>
                            <span class="bg-slate-900 text-indigo-200 border border-indigo-700 px-2 py-1 rounded text-[10px] font-bold font-mono">
                                ${t.trainNum} (${t.type}) - ${currentStObj.name}付近
                            </span>
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
            opEl.innerText = `運用番号: ${train.opNum || '73K'}`;

            const destName = allStationsMaster.find(s => s.id === train.endSt)?.name || train.endSt;
            const curStName = allStationsMaster[Math.floor(train._realtimeStationIdx || 0)]?.name || '不明';
            const startStName = allStationsMaster.find(s => s.id === train.startSt)?.name || train.startSt;

            let specialInfo = '';
            if(train.isCoupling) specialInfo += `<span class="text-amber-300 font-bold">・2編成連結列車 (${train.consistNum})</span><br>`;
            if(train.isWork) specialInfo += `<span class="text-rose-300 font-bold">・作業: ${train.workType === 'decoupling' ? '切り離し' : '連結'} (${allStationsMaster.find(s=>s.id===train.workStation)?.name || train.workStation}駅)</span><br>`;
            if(train.isChangeType) specialInfo += `<span class="text-sky-300 font-bold">・${allStationsMaster.find(s=>s.id===train.changeStation)?.name || train.changeStation}駅から「${train.changedType}」へ種別変更</span><br>`;

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

            if(train.schedule) {
                for(let stId in train.schedule) {
                    const stName = allStationsMaster.find(s => s.id === stId)?.name || stId;
                    stopsHtml += `
                        <tr class="border-b border-slate-800/50 hover:bg-slate-800">
                            <td class="p-1 font-bold text-slate-200">${stName}</td>
                            <td class="p-1 font-mono text-indigo-300">${train.schedule[stId].arr || '-'}</td>
                            <td class="p-1 font-mono text-indigo-300">${train.schedule[stId].dep || '-'}</td>
                        </tr>
                    `;
                }
            }
            stopsHtml += `</table></div></div>`;

            contentEl.innerHTML = `
                <div class="bg-slate-900 p-3 rounded-lg border border-slate-700 space-y-2 text-xs">
                    <div class="flex justify-between"><span class="text-slate-400">列車番号:</span> <span class="font-bold text-indigo-300">${train.trainNum}</span></div>
                    <div class="flex justify-between"><span class="text-slate-400">運用番号:</span> <span class="font-mono text-sky-300">${train.opNum || '73K'}</span></div>
                    <div class="flex justify-between"><span class="text-slate-400">編成番号:</span> <span class="font-mono text-indigo-400">${train.consistNum || '-'}</span></div>
                    <div class="flex justify-between"><span class="text-slate-400">種別:</span> <span class="font-bold text-emerald-300">${train.type}</span></div>
                    <div class="flex justify-between"><span class="text-slate-400">区間:</span> <span class="text-slate-200">${startStName}発 〜 ${destName}行</span></div>
                    ${specialInfo ? `<div class="pt-1 border-t border-slate-800">${specialInfo}</div>` : ''}
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
            renderStationScheduleInputs();
            initTimetableDropdowns();
            setInterval(updateLiveDateTime, 1000);
            updateLiveDateTime();
            renderOperationTrack();
        };
    </script>
</body>
</html>
```[cite: 1]
