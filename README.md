<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>豐原中正公園線上探勘 —— 消失的地景檔案</title>
    <style>
        :root {
            --primary-color: #1a252c;
            --secondary-color: #f39c12;
            --accent-color: #3498db;
            --bg-color: #f4f7f6;
            --text-main: #2c3e50;
            --success-color: #27ae60;
            --card-bg: #ffffff;
        }

        body {
            font-family: '微軟正黑體', -apple-system, BlinkMacSystemFont, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            margin: 0; 
            padding: 0;
            display: flex; 
            flex-direction: column;
            min-height: 100vh;
            -webkit-tap-highlight-color: transparent;
        }

        header {
            background-color: var(--primary-color);
            color: white; 
            text-align: center; 
            padding: 1rem;
            position: sticky; 
            top: 0; 
            z-index: 100;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

        header h1 { margin: 0; font-size: 1.3rem; color: var(--secondary-color); }
        header p { margin: 5px 0 0 0; font-size: 0.9rem; color: #bdc3c7; }

        main { 
            flex: 1; 
            padding: 1rem; 
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            width: 100%; 
            box-sizing: border-box; 
        }

        .view-section { 
            width: 100%; 
            max-width: 600px; 
            display: none; 
            animation: fadeIn 0.3s ease-out; 
        }
        .view-section.active { display: block; }

        @keyframes fadeIn { 
            from { opacity: 0; transform: translateY(10px); } 
            to { opacity: 1; transform: translateY(0); } 
        }

        .card { 
            background: var(--card-bg); 
            border-radius: 12px; 
            padding: 1.5rem; 
            margin-bottom: 1.5rem; 
            border-top: 4px solid var(--secondary-color); 
            box-shadow: 0 4px 15px rgba(0,0,0,0.05); 
        }

        h2 { 
            color: var(--primary-color); 
            font-size: 1.2rem; 
            border-bottom: 2px dashed #ecf0f1; 
            padding-bottom: 10px; 
            margin-top: 0; 
        }
        .text-box { line-height: 1.7; font-size: 1.05rem; color: #444; margin-bottom: 1.5rem; }
        .clue-box { background: #e8f4f8; border-left: 4px solid var(--accent-color); padding: 12px; font-size: 0.95rem; color: #2980b9; margin-bottom: 1.5rem; border-radius: 0 8px 8px 0; }

        .btn {
            display: block; width: 100%; box-sizing: border-box; 
            background-color: var(--secondary-color); color: #1a252c; 
            border: none; padding: 16px; font-size: 1.1rem; font-weight: bold;
            border-radius: 10px; cursor: pointer; text-align: center; margin-bottom: 1rem;
            transition: transform 0.1s, filter 0.1s;
        }
        .btn:active { transform: scale(0.98); filter: brightness(0.9); }
        .btn.btn-outline { background-color: transparent; color: var(--primary-color); border: 2px solid var(--primary-color); }

        .input-box { 
            width: 100%; box-sizing: border-box; padding: 16px; 
            font-size: 16px;
            border: 2px solid #bdc3c7; border-radius: 10px; 
            margin-bottom: 1rem; outline: none; transition: border-color 0.2s;
        }
        .input-box:focus { border-color: var(--accent-color); }
        .input-box:disabled { background: #ecf0f1; color: var(--success-color); font-weight: bold; border-color: var(--success-color); }

        .list-group { display: flex; flex-direction: column; gap: 10px; }
        .list-item {
            background: white; border: 1px solid #ddd; border-left: 5px solid #ccc;
            padding: 16px; border-radius: 8px; display: flex; justify-content: space-between;
            align-items: center; font-weight: bold; cursor: pointer;
        }
        .list-item.unlocked { border-left-color: var(--success-color); background-color: #f4fbf7; color: var(--success-color); }
        .status-badge { font-size: 0.8rem; padding: 4px 8px; border-radius: 20px; background: #eee; color: #666; }
        .unlocked .status-badge { background: #d5f5e3; color: var(--success-color); }

        .image-gallery { display: flex; flex-direction: column; gap: 10px; margin-bottom: 1.5rem; }
        .image-gallery img { width: 100%; height: auto; border-radius: 8px; object-fit: cover; box-shadow: 0 2px 8px rgba(0,0,0,0.1); }
        .img-fallback { background: #ecf0f1; color: #7f8c8d; border: 2px dashed #bdc3c7; border-radius: 8px; width: 100%; padding: 20px; text-align: center; word-break: break-all; font-size: 0.9rem; box-sizing: border-box; }

        .modal-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.6); display: flex; justify-content: center; align-items: center; z-index: 1000; visibility: hidden; opacity: 0; transition: opacity 0.3s; }
        .modal-overlay.active { visibility: visible; opacity: 1; }
        .modal-content { background: white; width: 90%; max-width: 350px; border-radius: 15px; padding: 2rem 1.5rem; text-align: center; transform: translateY(20px); transition: transform 0.3s; }
        .modal-overlay.active .modal-content { transform: translateY(0); }
        .modal-title { font-size: 1.4rem; color: var(--primary-color); margin: 0 0 10px 0; }
        .modal-text { color: #555; margin-bottom: 20px; line-height: 1.5; }

        footer { background-color: var(--primary-color); color: #ecf0f1; text-align: center; padding: 1.5rem 1rem; font-size: 0.85rem; line-height: 1.6; margin-top: auto; }
        
        .accordion details { margin-bottom: 0.8rem; border: 1px solid #e0e0e0; border-radius: 8px; overflow: hidden; }
        .accordion summary { font-weight: bold; background-color: #f9f9f9; padding: 1rem; cursor: pointer; color: var(--primary-color); outline: none; }
        .accordion .content { padding: 1rem; font-size: 0.95rem; line-height: 1.6; color: #555; background: #fff; border-top: 1px solid #e0e0e0; }
    </style>
</head>
<body>
    <header>
        <h1>虛擬地景檔案室</h1>
        <p>線上探勘活動</p>
    </header>

    <main>
        <!-- 首頁視圖 -->
        <section id="home-view" class="view-section active">
            <div class="card">
                <h2>📜 線上探勘簡報</h2>
                <div class="text-box">各位地理探員請注意：由於時空數據異常，關於豐原中正公園的歷史與地景資料已散落於虛擬檔案室中。請透過殘存的文本與影像線索，還原這座公園的真實樣貌。</div>
                
                <h2>📁 歷史擴充情資：豐原產業</h2>
                <div class="accordion text-box">
                    <details>
                        <summary>🪵 1. 木藝產業：台灣木藝加工重鎮</summary>
                        <div class="content"><strong>林場帶動：</strong>日治時期大甲溪畔「八仙山林場」開發。1950年代「大雪山林業公司」成立，讓豐原成為全台製材重鎮。<br><br><strong>外銷神話：</strong>外銷沙拉碗、玩具等，在國際上賺取大量外匯。</div>
                    </details>
                    <details>
                        <summary>🎨 2. 漆藝產業：台灣漆器之鄉</summary>
                        <div class="content"><strong>木與漆的結合：</strong>木器加工產生大量木胎體。日本商人將漆器訂單交由豐原代工。<br><br><strong>極盛時期：</strong>1970年代前後，外銷日本漆器高達90%產自豐原。<br><br><small style="color:var(--accent-color);">（提示：這與檔案室中的部分任務息息相關！）</small></div>
                    </details>
                    <details>
                        <summary>🥮 3. 糕餅產業：葫蘆墩「餅窟」</summary>
                        <div class="content">盛產高品質「葫蘆墩米」且水質清澈。孕育了雪花齋、寶泉、犁記等百年老店。</div>
                    </details>
                </div>

                <h2>🎯 操作指南</h2>
                <ul class="text-box" style="padding-left: 20px;">
                    <li>點擊下方按鈕進入「數位探勘地圖」。</li>
                    <li>閱讀謎題文本，並觀察夾帶的數位影像紀錄。</li>
                    <li>在修復框輸入正確「關鍵字」解鎖檔案。</li>
                    <li>成功修復全部 13 筆資料即完成任務！</li>
                </ul>
                <button class="btn" onclick="switchView('map-view')">啟動虛擬檔案室系統</button>
            </div>
        </section>

        <section id="map-view" class="view-section">
            <h2 style="text-align: center; border: none; margin-bottom: 1.5rem;">🗺️ 數位探勘地圖</h2>
            <div class="list-group" id="map-container"></div>
            <button class="btn btn-outline" style="margin-top: 1.5rem;" onclick="switchView('home-view')">返回任務簡報</button>
        </section>

        <section id="level-view" class="view-section">
            <div class="card">
                <button class="btn btn-outline" style="padding: 10px; margin-bottom: 1.5rem;" onclick="switchView('map-view')">← 返回地圖</button>
                <h2 id="level-title">檔案標題</h2>
                <div class="text-box" id="level-text" style="font-style: italic; background: #fdfefe; padding: 15px; border: 1px solid #eee; border-radius: 8px;"></div>
                <div class="clue-box" id="level-clue"></div>
                
                <!-- 圖片展示區塊 -->
                <div id="level-image-gallery" class="image-gallery"></div>

                <div class="input-group">
                    <input type="text" id="answer-input" class="input-box" placeholder="請輸入修復關鍵字..." autocomplete="off">
                    <button class="btn" id="submit-btn" onclick="checkAnswer()">驗證修復</button>
                </div>
            </div>
        </section>
    </main>

    <div id="custom-modal" class="modal-overlay">
        <div class="modal-content">
            <h3 class="modal-title" id="modal-title">系統提示</h3>
            <p class="modal-text" id="modal-text">訊息內容</p>
            <button class="btn" onclick="closeModal()">確認</button>
        </div>
    </div>

    <footer>
        <div>👨‍🏫 設計開發：豐原高中 林心穎老師</div>
        <div style="margin-top: 5px; opacity: 0.8;">📚 參考資料：《豐原中正公園實境解謎》<br>臺灣原生樹木推廣及媒合平臺</div>
    </footer>

    <script>
        const puzzleData = [
            { 
                id: 1, title: "檔案 01：撕裂的大地", 
                text: "在一九九九年，秋風蕭瑟的夜晚，凌晨一點四十七分，轟隆一聲巨響，大地被撕裂，將平地拔成了崖。從此它大名如雷貫耳——車籠埔！", 
                clue: "【地理線索】請觀察現場影像地形的高低落差，這是一種內營力作用形成的地形名稱。提示：五個字。",
                answer: "斷層崖隆起／車籠埔斷層／斷層",
                images: [
                    "https://drive.google.com/file/d/1oGE499BwZTfE9wmJSdalqRnrRqadFflM/view?usp=drive_link",
                    "斷層隆起_3.png"
                ],
                isUnlocked: false 
            },
            { 
                id: 2, title: "檔案 02：會咬人的樹", 
                text: "日本人大老遠從南洋引進這種樹，就為了取其汁液，用以保護易腐朽的木頭器具。「癢！癢！好癢！」因其汁液帶毒，常令工人嚴重過敏。", 
                clue: "【生態線索】這種植物的汁液是天然的塗料，但會引起皮膚過敏。提示：兩個字。",
                answer: "漆樹", 
                images: ["室外小漆樹.jpg"],
                isUnlocked: false 
            },
            { 
                id: 3, title: "檔案 03：晨光手路", 
                text: "天還沒亮，就準備上工了。叫上張三、李四、王五，和趙六，大家一起摸黑出發。帶上刀與容器，今天可是至少要採五百棵樹呢？看大家熟練地在樹上刻畫V字的溝壑，插上貝殼，我可要多學學他們的「手路」。是說辛苦歸辛苦，誰叫這工作薪水那麼高呢？", 
                clue: "【人文線索】請觀察附圖中樹幹上的 V 字割痕，這是他們獨特的採集手路。",
                answer: "採漆工人像／採漆工人／漆樹", 
                images: ["漆樹取漆_2.jpg"],
                isUnlocked: false 
            },
            { 
                id: 4, title: "檔案 04：悲傷的故事", 
                text: "這是一個悲傷的故事：「大人大人！小姐她……她！她落水啦！」為了祈求往後的人平安，於是……。", 
                clue: "【歷史線索】這座石碑是為了鎮壓水患、祈求平安而設立的。提示：三個字。",
                answer: "水神碑", 
                images: [],
                isUnlocked: false 
            },
            { 
                id: 5, title: "檔案 05：水利工程", 
                text: "這是一連串的水利工程：儲水塔匯集旱溪、北坑、中坑、南坑等河水並將其儲存；匯入上池、澄清泥水，再經下池，過濾雜質，然後流入清水池。最後由配水池配往神岡與豐原。配水池的疏壓孔極具特色，而下池有一個有趣的歷史，就是曾改建為中正游泳池。", 
                clue: "【建設線索】請觀察影像中與水利工程相關的設施遺址。",
                answer: "自來水廠／儲水塔／澄泥池／過濾池／清水池／配水池", 
                images: ["澄泥池_3.jpg", "清水池_3.jpg", "配水池.png"],
                isUnlocked: false 
            },
            { 
                id: 6, title: "檔案 06：居住的痕跡", 
                text: "吉永氏住過，王阿脾也住過，現在可能沒人住那，但要是你想，你可以隨時去吃麻辣火鍋。", 
                clue: "【地景線索】這裡曾是政府要員的居所，現在飄著麻辣鍋的香味。",
                answer: "廠長宿舍／市長官邸／一鼎饌／麻辣鍋", 
                images: [],
                isUnlocked: false 
            },
            { 
                id: 7, title: "檔案 07：地下的神獸", 
                text: "傳說地下有牛，有著玄武岩的身體，每當他背癢癢，往往天翻地覆。", 
                clue: "【傳說線索】尋找公園內代表地震傳說的神獸雕像。",
                answer: "地牛翻身／地牛", 
                images: [],
                isUnlocked: false 
            },
            { 
                id: 8, title: "檔案 08：威權的凝視", 
                text: "呼風喚雨一時，令你我處處見其大名。黃埔軍校校長他當過，陸海空統帥更是日常。如今人已千古，仍求萬世不朽。要見他，去慈湖。", 
                clue: "【歷史線索】這是一座曾經代表政治威權的紅銅色巨像。",
                answer: "蔣中正雕像／蔣公銅像／蔣介石", 
                images: ["蔣公銅像.jpg"],
                isUnlocked: false 
            },
            { 
                id: 9, title: "檔案 09：紅橋點點", 
                text: "跨越旱溪，遠處見它殷紅如血，點點白鷺點綴其中。", 
                clue: "【地景線索】這是一座跨越溪流、名字中有鳥類的紅色橋樑。",
                answer: "來鷺橋", 
                images: [],
                isUnlocked: false 
            },
            { 
                id: 10, title: "檔案 10：金黃的火堆", 
                text: "過了清明，低海拔山林被染成一片金黃，原來是它盛開的花。本是為減少水分蒸散而退化的葉，卻成為你我仰望天空之時，在天空作畫的細碎線條。它的身體，甚至能化作溫暖的火堆，陪你我度過漫漫長夜，想著許久未見的他。", 
                clue: "【生態線索】觀察影像中的假葉與樹冠，它是早期燒製木炭的重要樹種。",
                answer: "相思樹", 
                images: ["相思葉片_2.jpg", "相思樹_2.jpg", "相思樹_2.png"],
                isUnlocked: false 
            },
            { 
                id: 11, title: "檔案 11：低海拔神木", 
                text: "被稱為低海拔的神木，先人取其樹幹，碎其身，熬出精華。其精華竟是重要的工業原料，可用於醫藥、製造賽璐珞，甚至製造火藥。於是清代日治之時，台灣淺山處處可見其身影，它也成為台灣出口商品之大宗。若想找到它，請尋找它裂紋密布的身體，香氣四溢的枝葉。", 
                clue: "【生態線索】觀察影像中樹幹明顯的縱裂紋，它是台灣早期的出口大宗。",
                answer: "樟樹", 
                images: ["樟樹-2_2.jpg"],
                isUnlocked: false 
            },
            { 
                id: 12, title: "檔案 12：最高點的秘寶", 
                text: "此處乃中正公園最高點，若你到此，必有大秘寶！少則身心舒暢，多則永保安康！在此遠眺后豐遠景、大甲溪流，盡收眼底。", 
                clue: "【地標線索】觀察影像，這是公園的最高點，擁有極佳的視野與祝福。",
                answer: "平安嶺", 
                images: ["平安嶺_2.jpg"],
                isUnlocked: false 
            },
            { 
                id: 13, title: "檔案 13：溫柔的守護", 
                text: "不見叢林，卻見獅蹤。雖有王者的威嚴，卻是最溫柔的守護。由熱心的仕紳社團「獅子會」聯名致贈，我的名字裡，刻著這座城市的「豐」饒榮耀，也隱含著那群森林之王的足跡。累了，就在我的懷抱裡聽聽風聲。", 
                clue: "【地標線索】這是一座由獅子會捐贈的涼亭。提示：三個字。",
                answer: "豐獅亭", 
                images: [],
                isUnlocked: false 
            }
        ];

        let currentActiveLevel = null;

        function switchView(viewId) {
            document.querySelectorAll('.view-section').forEach(el => el.classList.remove('active'));
            document.getElementById(viewId).classList.add('active');
            if(viewId === 'map-view') renderMap();
            window.scrollTo(0, 0);
        }

        function renderMap() {
            const container = document.getElementById('map-container');
            container.innerHTML = ''; 
            puzzleData.forEach(puzzle => {
                const node = document.createElement('div');
                node.className = `list-item ${puzzle.isUnlocked ? 'unlocked' : ''}`;
                node.innerHTML = `<span>${puzzle.title}</span><span class="status-badge">${puzzle.isUnlocked ? '✅ 已修復' : '🔒 鎖定中'}</span>`;
                node.onclick = () => openLevel(puzzle.id);
                container.appendChild(node);
            });
        }

        // 解析 Google Drive 與檔案路徑的核心函式
        function parseImageUrl(src) {
            if (!src) return '';
            
            // 處理 Google Drive 分享連結
            if (src.includes('drive.google.com')) {
                let fileId = '';
                const matchD = src.match(/\/d\/([a-zA-Z0-9_-]+)/);
                const matchId = src.match(/[?&]id=([a-zA-Z0-9_-]+)/);
                
                if (matchD && matchD[1]) {
                    fileId = matchD[1];
                } else if (matchId && matchId[1]) {
                    fileId = matchId[1];
                }
                
                if (fileId) {
                    // 改用穩定直連的 Google 縮圖 CDN
                    return `https://lh3.googleusercontent.com/d/${fileId}`;
                }
            }
            
            // 處理相對路徑（若包含中文檔名，編碼確保網址合法）
            return encodeURI(src);
        }

        function openLevel(id) {
            const puzzle = puzzleData.find(p => p.id === id);
            if (!puzzle) return;
            currentActiveLevel = puzzle;
            switchView('level-view');
            
            document.getElementById('level-title').innerText = puzzle.title;
            document.getElementById('level-text').innerText = puzzle.text;
            document.getElementById('level-clue').innerText = puzzle.clue;
            
            const gallery = document.getElementById('level-image-gallery');
            gallery.innerHTML = '';
            
            if (puzzle.images && puzzle.images.length > 0) {
                gallery.style.display = 'flex';
                puzzle.images.forEach(originalSrc => {
                    const img = document.createElement('img');
                    const resolvedSrc = parseImageUrl(originalSrc);
                    
                    img.src = resolvedSrc;
                    img.alt = puzzle.title;
                    
                    // 影像無法載入時的降級提示
                    img.onerror = function() {
                        this.onerror = null;
                        this.outerHTML = `<div class="img-fallback">📷 影像無法載入<br><small>原始檔案名稱：${originalSrc}</small></div>`;
                    };
                    gallery.appendChild(img);
                });
            } else {
                gallery.style.display = 'none';
            }
            
            const input = document.getElementById('answer-input');
            const submitBtn = document.getElementById('submit-btn');
            input.value = '';
            if (puzzle.isUnlocked) {
                input.value = puzzle.answer.split('／')[0];
                input.disabled = true; 
                submitBtn.style.display = 'none';
            } else {
                input.disabled = false; 
                submitBtn.style.display = 'block';
            }
        }

        function checkAnswer() {
            if (!currentActiveLevel || currentActiveLevel.isUnlocked) return;
            const inputVal = document.getElementById('answer-input').value.trim();
            const possibleAnswers = currentActiveLevel.answer.split('／');
            
            let isCorrect = possibleAnswers.some(ans => 
                inputVal && (inputVal.includes(ans) || (ans.includes(inputVal) && inputVal.length >= 2))
            );

            if (isCorrect) {
                currentActiveLevel.isUnlocked = true;
                showModal('🎉 檔案修復成功！', `關鍵字比對吻合，資料已登錄。`);
                document.getElementById('answer-input').disabled = true;
                document.getElementById('submit-btn').style.display = 'none';
            } else {
                showModal('❌ 修復失敗', '關鍵字比對不符，請重新閱讀文本或觀察影像進行推理。');
            }
        }

        const modal = document.getElementById('custom-modal');
        function showModal(title, text) {
            document.getElementById('modal-title').innerText = title;
            document.getElementById('modal-text').innerText = text;
            modal.classList.add('active');
        }

        function closeModal() {
            modal.classList.remove('active');
            
            const allUnlocked = puzzleData.every(p => p.isUnlocked);
            if (allUnlocked && currentActiveLevel && currentActiveLevel.isUnlocked) {
                setTimeout(() => {
                    showModal('🏆 最終榮耀', '您已成功修復所有地景檔案！系統最高點「平安嶺」已為您敞開，線上探勘任務圓滿完成！');
                    currentActiveLevel = null;
                }, 400);
            }
        }
    </script>
</body>
</html>
