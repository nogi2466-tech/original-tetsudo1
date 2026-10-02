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
    <header class="bg-indigo-950 border-b border-indigo-800 p-4 shadow-lg flex flex-wrap justify-between items-center gap-4">
        <div class="flex items-center space-x-3">
            <span class="text-3xl">🚄</span>
            <div>
                <h1 class="text-xl font-bold tracking-wider text-indigo-200">紫句守鉄道 <span class="text-xs font-normal text-indigo-400">しのもり鉄道 - Shinomori Railway</span></h1>
                <p class="text-xs text-slate-400">総合運行管理・経営シミュレーター</p>
            </div>
        </div>
        <div class="flex items-center space-x-2 bg-slate-900 px-3 py-1.5 rounded-lg border border-slate-700 text-xs">
            <span id="sync-status-dot" class="w-2.5 h-2.5 rounded-full bg-amber-500 animate-pulse"></span>
            <span id="sync-status-text">クラウド同期: 未設定 (設定タブで接続)</span>
        </div>
    </header>

    <!-- ナビゲーションタブ -->
    <nav class="bg-slate-900/90 border-b border-slate-800 px-4 py-2 overflow-x-auto flex space-x-1 sticky top-0 z-50 backdrop-blur">
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
            <div class="bg-gradient-to-r from-indigo-900 to-slate-800 p-6 rounded-2xl border border-indigo-700/50 shadow-xl">
                <h2 class="text-2xl font-bold text-indigo-100 mb-2">紫句守鉄道株式会社</h2>
                <p class="text-slate-300 text-sm leading-relaxed">
                    当社は紫雲本線をはじめとする5路線（全60駅）を有し、首都圏と観光地・高原リゾートを結ぶ快適な輸送サービスを提供しています。最新のデジタル運行管理システムと高品質なSシリーズ車両により、安全・快適・スピーディーな鉄道ネットワークを実現しています。
                </p>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                <div class="bg-slate-800 p-5 rounded-xl border border-slate-700">
                    <h3 class="text-indigo-400 font-bold mb-1">沿線路線網</h3>
                    <ul class="text-sm text-slate-300 space-y-1">
                        <li>• 紫雲本線 (1〜30)</li>
                        <li>• 句守支線 (41〜55)</li>
                        <li>• 紫霞観光線 (56〜60)</li>
                        <li>• 星句高原線 (31〜40)</li>
                        <li>• 他の会社直通路線 (09〜1)</li>
                    </ul>
                </div>
                <div class="bg-slate-800 p-5 rounded-xl border border-slate-700">
                    <h3 class="text-indigo-400 font-bold mb-1">主要運行種別</h3>
                    <p class="text-sm text-slate-300 leading-relaxed">特急、通勤急行、急行、通勤快速、快速、準急、普通 の7種別を網羅。各路線の特性に合わせたダイヤグラムで運行しています。</p>
                </div>
                <div class="bg-slate-800 p-5 rounded-xl border border-slate-700">
                    <h3 class="text-indigo-400 font-bold mb-1">クラウド自動同期</h3>
                    <p class="text-sm text-slate-300 leading-relaxed">Firebase連携により、複数の端末（スマートフォンやPC）から同時にデータを共有・編集可能です。</p>
                </div>
            </div>
        </div>

        <!-- 2. 時刻表 -->
        <div id="tab-timetable" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">インタラクティブ時刻表</h2>
            <div class="bg-slate-800 p-4 rounded-xl border border-slate-700 flex flex-wrap gap-4 items-center">
                <div>
                    <label class="block text-xs text-slate-400 mb-1">路線選択</label>
                    <select id="tt-line" class="bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-sm text-white">
                        <option value="main">紫雲本線 (1〜30)</option>
                        <option value="branch">句守支線 (41〜55)</option>
                        <option value="sight">紫霞観光線 (56〜60)</option>
                        <option value="plateau">星句高原線 (31〜40)</option>
                    </select>
                </div>
                <div>
                    <label class="block text-xs text-slate-400 mb-1">方向</label>
                    <select id="tt-dir" class="bg-slate-900 border border-slate-700 rounded px-3 py-1.5 text-sm text-white">
                        <option value="down">下り (紫句守中央方面 → 各地)</option>
                        <option value="up">上り (各地 → 紫句守中央方面)</option>
                    </select>
                </div>
                <button onclick="renderTimetable()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-4 py-1.5 rounded text-sm font-medium transition self-end">時刻表表示</button>
            </div>
            <div id="timetable-container" class="bg-slate-800 rounded-xl border border-slate-700 p-4 overflow-x-auto text-sm">
                <p class="text-slate-400">「時刻表表示」を押すと駅ごとの発車時刻一覧が表示されます。</p>
            </div>
        </div>

        <!-- 3. 走行位置＆運転シミュレーター -->
        <div id="tab-operation" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">リアルタイム全線運行マップ & 運転士体験</h2>
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-4">
                <div class="lg:col-span-2 bg-slate-800 p-4 rounded-xl border border-slate-700 space-y-4">
                    <h3 class="font-bold text-sm text-indigo-300">運行状況モニター</h3>
                    <div class="bg-slate-900 h-64 rounded-lg border border-slate-800 p-4 relative overflow-y-auto">
                        <div id="train-list-status" class="space-y-2 text-sm">
                            <!-- 列車運行状況 -->
                        </div>
                    </div>
                </div>
                <div class="bg-slate-800 p-4 rounded-xl border border-slate-700 space-y-4">
                    <h3 class="font-bold text-sm text-indigo-300">簡易マスコン（運転席）</h3>
                    <div class="bg-slate-900 p-4 rounded-lg border border-slate-800 text-center space-y-3">
                        <div class="text-2xl font-mono text-emerald-400 font-bold" id="cab-speed">0 km/h</div>
                        <div class="text-xs text-slate-400" id="cab-status">停止中 - ドア閉</div>
                        <div class="flex justify-center gap-2">
                            <button onclick="cabBrake()" class="bg-red-600 hover:bg-red-500 px-3 py-1.5 rounded text-xs font-bold text-white">ブレーキ</button>
                            <button onclick="cabNeutral()" class="bg-slate-700 hover:bg-slate-600 px-3 py-1.5 rounded text-xs font-bold text-white">N</button>
                            <button onclick="cabAccel()" class="bg-indigo-600 hover:bg-indigo-500 px-3 py-1.5 rounded text-xs font-bold text-white">力行</button>
                            <button onclick="playHorn()" class="bg-amber-600 hover:bg-amber-500 px-3 py-1.5 rounded text-xs font-bold text-white">警笛</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- 4. 列車情報 -->
        <div id="tab-traininfo" class="tab-content space-y-4">
            <h2 class="text-xl font-bold text-indigo-200">車両形式図鑑</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4" id="train-info-grid">
                <!-- JavaScriptで動的生成 -->
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
                <button onclick="addNewTrain()" class="w-full bg-indigo-600 hover:bg-indigo-500 text-white font-medium py-2 rounded-lg transition shadow">運行リストに追加 (クラウド保存)</button>
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
            <h2 class="text-xl font-bold text-indigo-200">システム設定 & クラウド同期</h2>
            <div class="bg-slate-800 p-6 rounded-xl border border-slate-700 max-w-xl space-y-4">
                <p class="text-xs text-slate-300">
                    Firebase Realtime Database のURLを入力すると、複数のデバイス間でのリアルタイム自動同期が有効になります。
                </p>
                <div>
                    <label class="block text-xs text-slate-400 mb-1">Firebase Database URL</label>
                    <input type="text" id="fb-url" class="w-full bg-slate-900 border border-slate-700 rounded px-3 py-2 text-sm text-white" value="https://original-tetsudo-430ac-default-rtdb.firebaseio.com">
                </div>
                <button onclick="saveFirebaseConfig()" class="w-full bg-emerald-600 hover:bg-emerald-500 text-white font-medium py-2 rounded-lg transition shadow">クラウド接続を保存・開始</button>
                <hr class="border-slate-700 my-2">
                <div class="flex gap-4">
                    <button onclick="exportJSON()" class="flex-1 bg-indigo-700 hover:bg-indigo-600 text-white py-2 rounded text-sm">JSONファイル出力</button>
                    <button onclick="document.getElementById('import-file').click()" class="flex-1 bg-slate-700 hover:bg-slate-600 text-white py-2 rounded text-sm">JSON読込</button>
                    <input type="file" id="import-file" class="hidden" onchange="importJSON(event)">
                </div>
            </div>
        </div>

    </main>

    <script>
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
            if (tabId === 'dia') drawDiagram();
        }

        // 基本データ
        let appData = {
            trains: [
                { id: 1, name: "特急 101M", type: "特急", series: "S100系", status: "運行中 (紫句守中央 → 30.紫句守展示場)" },
                { id: 2, name: "普通 402C", type: "普通", series: "S3系", status: "停車中 (09.水鳥湿原)" },
                { id: 3, name: "快速 205M", type: "快速", series: "S2系", status: "運行中 (01.紫句守中央 → 55.詩句の丘)" }
            ]
        };

        let dbRef = null;

        // Firebase接続開始
        function saveFirebaseConfig() {
            const url = document.getElementById('fb-url').value.trim();
            if(!url) { alert("Database URLを入力してください"); return; }
            
            try {
                if(firebase.apps.length === 0) {
                    firebase.initializeApp({ databaseURL: url });
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
                document.getElementById('sync-status-dot').className = "w-2.5 h-2.5 rounded-full bg-emerald-500";
                document.getElementById('sync-status-text').innerText = "クラウド同期: 接続中 (リアルタイム)";
                localStorage.setItem('shinomori_fb_url', url);
                alert("クラウド同期接続に成功しました！");
            } catch(e) {
                alert("接続エラー: " + e.message);
            }
        }

        function pushData() {
            if(dbRef) {
                dbRef.set(appData);
            } else {
                localStorage.setItem('shinomori_local', JSON.stringify(appData));
            }
        }

        function updateUI() {
            const container = document.getElementById('train-list-status');
            if(container) {
                container.innerHTML = appData.trains.map(t => `
                    <div class="bg-slate-800 p-3 rounded border border-slate-700 flex justify-between items-center">
                        <div>
                            <span class="font-bold text-indigo-300">${t.name}</span>
                            <span class="text-xs bg-indigo-900 text-indigo-200 px-2 py-0.5 rounded ml-2">${t.type}</span>
                            <span class="text-xs text-slate-400 ml-2">(${t.series})</span>
                        </div>
                        <div class="text-xs text-emerald-400 font-mono">${t.status}</div>
                    </div>
                `).join('');
            }
            renderTrainInfo();
            renderConsist();
        }

        // 列車追加
        function addNewTrain() {
            const name = document.getElementById('add-train-name').value;
            const type = document.getElementById('add-train-type').value;
            const series = document.getElementById('add-train-series').value;
            appData.trains.push({ id: Date.now(), name, type, series, status: "車庫待機中" });
            pushData();
            updateUI();
            alert("新規列車を追加し、クラウドへ保存しました！");
            switchTab('operation');
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

        // 時刻表生成
        function renderTimetable() {
            const container = document.getElementById('timetable-container');
            container.innerHTML = `
                <table class="w-full text-left border-collapse">
                    <thead>
                        <tr class="border-b border-slate-700 text-indigo-300">
                            <th class="p-2">駅名</th>
                            <th class="p-2">特急</th>
                            <th class="p-2">急行</th>
                            <th class="p-2">快速</th>
                            <th class="p-2">普通</th>
                        </tr>
                    </thead>
                    <tbody class="text-slate-300">
                        <tr class="border-b border-slate-800"><td class="p-2 font-bold">01. 紫句守中央</td><td class="p-2">08:00</td><td class="p-2">08:05</td><td class="p-2">08:08</td><td class="p-2">08:10</td></tr>
                        <tr class="border-b border-slate-800"><td class="p-2 font-bold">03. 紫雲野</td><td class="p-2">08:05</td><td class="p-2">08:12</td><td class="p-2">08:16</td><td class="p-2">08:21</td></tr>
                        <tr class="border-b border-slate-800"><td class="p-2 font-bold">13. 紫霞野</td><td class="p-2">08:15</td><td class="p-2">08:26</td><td class="p-2">08:31</td><td class="p-2">08:42</td></tr>
                        <tr class="border-b border-slate-800"><td class="p-2 font-bold">30. 紫句守展示場</td><td class="p-2">08:35</td><td class="p-2">-</td><td class="p-2">-</td><td class="p-2">09:20</td></tr>
                    </tbody>
                </table>
            `;
        }

        // 運転シミュレーター制御
        let speed = 0;
        function cabAccel() { if(speed < 130) speed += 10; updateCab(); }
        function cabBrake() { if(speed > 0) speed -= 15; if(speed < 0) speed = 0; updateCab(); }
        function cabNeutral() { updateCab(); }
        function playHorn() { alert("ﾟ3ﾟ♪ ﾌﾟーーーッ！（警笛吹鳴）"); }
        function updateCab() {
            document.getElementById('cab-speed').innerText = speed + " km/h";
            document.getElementById('cab-status').innerText = speed > 0 ? "走行中..." : "停車中";
        }

        // JSON入出力
        function exportJSON() {
            const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(appData, null, 2));
            const dlAnchor = document.createElement('a');
            dlAnchor.setAttribute("href", dataStr);
            dlAnchor.setAttribute("download", "shinomori_railway_data.json");
            document.body.appendChild(dlAnchor);
            dlAnchor.click();
            dlAnchor.remove();
        }
        function importJSON(event) {
            const reader = new FileReader();
            reader.onload = function(e) {
                try {
                    appData = JSON.parse(e.target.result);
                    pushData();
                    updateUI();
                    alert("JSONデータを正常に読み込みました！");
                } catch(err) {
                    alert("JSONの読み込みに失敗しました。");
                }
            };
            reader.readAsText(event.target.files[0]);
        }

        // 初期化
        window.onload = function() {
            const savedUrl = localStorage.getItem('shinomori_fb_url') || "https://original-tetsudo-430ac-default-rtdb.firebaseio.com";
            document.getElementById('fb-url').value = savedUrl;
            saveFirebaseConfig();
        };
    </script>
</body>
</html>
