<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>大分オノマトペ旅マップ & 質感ギャラリー</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    
    <style>
        /* 全体の基本スタイル */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        body {
            font-family: 'Helvetica Neue', Arial, 'Hiragino Kaku Gothic ProN', Meiryo, sans-serif;
            background-color: #f7f5f0; /* 温かみのある背景色 */
            color: #333333;
            display: flex;
            justify-content: center;
            padding: 20px;
        }
        
        /* スマホ風の縦長コンテナ */
        .app-container {
            width: 100%;
            max-width: 480px;
            background: #ffffff;
            border-radius: 16px;
            box-shadow: 0 4px 16px rgba(0,0,0,0.08);
            overflow: hidden;
            display: flex;
            flex-direction: column;
            height: 90vh;
            position: relative;
            border: 1px solid #e0dcd3;
        }

        /* ヘッダーセクション（大分県カラー：エンジ色） */
        .header {
            padding: 16px;
            border-bottom: 3px solid #aa1e2d;
            position: relative;
            background: #ffffff;
            z-index: 1001;
        }
        .app-title {
            font-size: 1.15rem;
            color: #aa1e2d;
            text-align: center;
            margin-bottom: 12px;
            font-weight: bold;
            letter-spacing: 0.05em;
        }
        .search-bar-container {
            display: flex;
            align-items: center;
            background: #f4f0e6;
            border-radius: 24px;
            padding: 8px 16px;
            border: 1px solid #ded9cb;
        }
        .menu-button {
            background: none;
            border: none;
            font-size: 18px;
            color: #aa1e2d;
            cursor: pointer;
            margin-right: 12px;
            display: flex;
            align-items: center;
        }
        .search-input {
            flex: 1;
            border: none;
            background: none;
            font-size: 14px;
            outline: none;
            color: #333;
        }
        .search-input::placeholder {
            color: #8c8475;
        }
        
        /* プロフィールアイコン（「画像.txt」の画像背景スタイルを適用） */
        .profile-icon {
            width: 32px;
            height: 32px;
            background-image: url('image_1.png'); /* 大分県の形の画像 */
            background-size: contain;
            background-repeat: no-repeat;
            background-position: center;
        }

        /* 絞り込みドロップダウンメニュー */
        .filter-menu {
            position: absolute;
            top: 105px;
            left: 16px;
            background: white;
            border: 1px solid #ded9cb;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
            width: 160px;
            display: none;
            flex-direction: column;
            z-index: 1002;
        }
        .filter-menu.active {
            display: flex;
        }
        .filter-item {
            padding: 12px 16px;
            background: none;
            border: none;
            text-align: left;
            font-size: 14px;
            cursor: pointer;
            color: #4a443c;
            border-bottom: 1px solid #f4f0e6;
        }
        .filter-item:last-child {
            border-bottom: none;
        }
        .filter-item:hover {
            background: #fdfbf7;
            color: #aa1e2d;
        }

        /* メインコンテンツエリア（地図とギャラリーをスクロール） */
        .main-content {
            flex: 1;
            overflow-y: auto;
            background: #fdfbf7;
        }

        /* メインの地図 */
        #mainMap {
            height: 220px;
            width: 100%;
            border-bottom: 1px solid #ded9cb;
            z-index: 1;
        }

        /* 画像ギャラリー（グリッド配置） */
        .gallery-container {
            padding: 8px;
        }
        .gallery-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 4px;
        }
        .gallery-item {
            aspect-ratio: 1 / 1;
            background: #e6e1d6;
            cursor: pointer;
            overflow: hidden;
            position: relative;
            transition: opacity 0.2s;
            border-radius: 4px;
        }
        .gallery-item:hover {
            opacity: 0.8;
        }
        .gallery-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        /* 投稿ボタン（浮かぶプラスボタン） */
        .add-post-btn {
            position: absolute;
            bottom: 60px;
            right: 20px;
            width: 56px;
            height: 56px;
            background-color: #aa1e2d;
            color: #fff;
            border-radius: 50%;
            border: none;
            font-size: 28px;
            cursor: pointer;
            box-shadow: 0 4px 10px rgba(170,30,45,0.3);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
        }
        .add-post-btn:hover {
            background-color: #851622;
        }

        /* モーダル共通スタイル */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 2000;
        }
        .modal-overlay.active {
            display: flex;
        }
        .modal-content {
            background: white;
            padding: 20px;
            border-radius: 16px;
            width: 90%;
            max-width: 420px;
            position: relative;
            border-top: 6px solid #aa1e2d;
        }
        .modal-close {
            position: absolute;
            top: 12px;
            right: 16px;
            font-size: 24px;
            background: none;
            border: none;
            cursor: pointer;
            color: #888;
            z-index: 10;
        }

        /* 手書き・記録ノート風のデザイン */
        .detail-layout {
            border: 2px solid #555555;
            border-radius: 12px;
            padding: 12px;
            margin-top: 15px;
            background-color: #fff;
        }
        .detail-flex {
            display: flex;
            gap: 10px;
            margin-bottom: 12px;
        }
        .detail-box {
            flex: 1;
            border: 1px solid #aaaaaa;
            border-radius: 6px;
            height: 140px;
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
            background: #fafafa;
        }
        .detail-box img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        #modalMap {
            width: 100%;
            height: 100%;
        }
        .detail-tag-zone {
            border: 1px solid #aaaaaa;
            border-radius: 6px;
            padding: 10px;
            background-color: #fdfbf7;
            font-size: 1.1rem;
            font-weight: bold;
            color: #aa1e2d;
        }

        /* 削除ボタンのスタイル */
        .delete-btn-container {
            margin-top: 12px;
            text-align: right;
        }
        .delete-post-btn {
            background: none;
            border: 1px solid #aa1e2d;
            color: #aa1e2d;
            padding: 6px 12px;
            font-size: 12px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            transition: background 0.2s, color 0.2s;
        }
        .delete-post-btn:hover {
            background: #aa1e2d;
            color: #ffffff;
        }

        /* 投稿フォームのスタイル */
        .form-group {
            margin-bottom: 12px;
            text-align: left;
        }
        .form-group label {
            display: block;
            font-size: 13px;
            font-weight: bold;
            margin-bottom: 4px;
            color: #4a443c;
        }
        .form-control {
            width: 100%;
            padding: 8px;
            border: 1px solid #ded9cb;
            border-radius: 6px;
            font-size: 14px;
            background-color: #fdfbf7;
        }
        .submit-btn {
            width: 100%;
            padding: 10px;
            background-color: #aa1e2d;
            color: white;
            border: none;
            border-radius: 6px;
            font-size: 15px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 10px;
        }
        .submit-btn:hover {
            background-color: #851622;
        }

        /* フッター */
        .footer {
            height: 45px;
            border-top: 1px solid #ded9cb;
            background: #ffffff;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #8c8475;
            font-size: 11px;
        }
    </style>
</head>
<body>

<div class="app-container">
    <header class="header">
        <div class="app-title">♨️ 大分オノマトペ旅マップ & ギャラリー</div>
        <div class="search-bar-container">
            <button class="menu-button" id="menuBtn" title="絞り込みメニュー">☰</button>
            <input type="text" class="search-input" id="searchInput" placeholder="オノマトペ・場所で検索...">
            <div class="profile-icon"></div>
        </div>
        
        <div class="filter-menu" id="filterMenu">
            <button class="filter-item" data-tag="すべて">すべて</button>
            <button class="filter-item" data-tag="モチモチ">モチモチ</button>
            <button class="filter-item" data-tag="フワフワ">フワフワ</button>
            <button class="filter-item" data-tag="ザクザク">ザクザク</button>
            <button class="filter-item" data-tag="シュワシュワ">シュワシュワ</button>
            <button class="filter-item" data-tag="しんしん">しんしん</button>
            <button class="filter-item" data-tag="とろとろ">とろとろ</button>
            <button class="filter-item" data-tag="ぷくぷく">ぷくぷく</button>
            <button class="filter-item" data-tag="もくもく">もくもく</button>
        </div>
    </header>

    <div class="main-content">
        <div id="mainMap"></div>

        <main class="gallery-container">
            <div class="gallery-grid" id="galleryGrid"></div>
        </main>
    </div>

    <button class="add-post-btn" id="addPostBtn" title="画像を追加する">＋</button>

    <footer class="footer">
        © 大分県オノマトペ観光マップ（統合モックアップ）
    </footer>
</div>

<div class="modal-overlay" id="detailModal">
    <div class="modal-content">
        <button class="modal-close" id="detailClose">&times;</button>
        <h3 style="text-align: center; font-size: 14px; color: #666; margin-bottom: 5px;">スポット詳細</h3>
        
        <div class="detail-layout">
            <div class="detail-flex">
                <div class="detail-box">
                    <img src="" alt="投稿画像" id="modalImg">
                </div>
                <div class="detail-box">
                    <div id="modalMap"></div>
                </div>
            </div>
            <div class="detail-tag-zone" id="modalTag"># オノマトペ</div>
            <p id="modalLoc" style="font-size: 13px; font-weight: bold; color: #4b5563; margin-top: 8px; padding: 0 4px;"></p>
            <p id="modalDesc" style="font-size: 13px; color: #555; margin-top: 6px; padding: 0 4px; line-height: 1.4;"></p>
            
            <div class="delete-btn-container">
                <button type="button" class="delete-post-btn" id="deletePostBtn">🗑️ この投稿を削除する</button>
            </div>
        </div>
    </div>
</div>

<div class="modal-overlay" id="formModal">
    <div class="modal-content">
        <button class="modal-close" id="formClose">&times;</button>
        <h3 style="margin-bottom: 15px; text-align: center; color: #aa1e2d;">新しい質感スポットを投稿</h3>
        
        <form id="postForm">
            <div class="form-group">
                <label>1. 写真を選択</label>
                <input type="file" id="formFile" class="form-control" accept="image/*" required>
            </div>
            <div class="form-group">
                <label>2. オノマトペ（自分で決める）</label>
                <input type="text" id="formTag" class="form-control" placeholder="例: ザクザク、モチモチ" required>
            </div>
            <div class="form-group">
                <label>3. 大まかな場所のエリアを選択</label>
                <select id="formArea" class="form-control" required>
                    <option value="中津市エリア">中津市エリア</option>
                    <option value="宇佐市エリア">宇佐市エリア</option>
                    <option value="臼杵市エリア">臼杵市エリア</option>
                    <option value="別府温泉エリア">別府温泉エリア</option>
                    <option value="由布岳・湯布院エリア">由布岳・湯布院エリア</option>
                    <option value="大分駅周辺エリア">大分駅周辺エリア</option>
                </select>
            </div>
            <div class="form-group">
                <label>4. 簡単な説明（メモ）</label>
                <input type="text" id="formDesc" class="form-control" placeholder="例: 地元の美味しい名物、素敵な景色">
            </div>
            <button type="submit" class="submit-btn">アプリに追加する</button>
        </form>
    </div>
</div>

<script>
    // エリアごとのデフォルト座標
    const areaCoordinates = {
        "中津市エリア": { lat: 33.5969, lng: 131.1892 },
        "宇佐市エリア": { lat: 33.5309, lng: 131.3524 },
        "臼杵市エリア": { lat: 33.1152, lng: 131.8016 },
        "別府温泉エリア": { lat: 33.3148, lng: 131.4844 },
        "由布岳・湯布院エリア": { lat: 33.2842, lng: 131.3917 },
        "大分駅周辺エリア": { lat: 33.2341, lng: 131.6054 }
    };

    // 初期サンプルデータ
    const defaultData = [
        { id: 1, tag: 'ザクザク', loc: '中津市エリア', desc: '中津といえばお馴染みのからあげ。揚げたてで衣がザクザクジューシー！', lat: 33.5969, lng: 131.1892, img: 'https://images.unsplash.com/photo-1544025162-d76694265947?w=600' },
        { id: 2, tag: 'しんしん', loc: '宇佐市エリア', desc: '歴史ある宇佐神宮の境内。静けさの中に、厳かな空気がしんしんと満ちています。', lat: 33.5309, lng: 131.3524, img: 'https://images.unsplash.com/photo-1506744038136-46273834b3fb?w=600' },
        { id: 3, tag: 'とろとろ', loc: '臼杵市エリア', desc: '臼杵名物の郷土料理や、特産のかぼすを使ったソフトクリーム。口当たりがとろとろ。', lat: 33.1152, lng: 131.8016, img: 'https://picsum.photos/id/429/300/300' },
        { id: 4, tag: 'ぷくぷく', loc: '別府温泉エリア', desc: 'あちこちから湧き出る温泉の恵み。お湯がぷくぷくと湧いています。', lat: 33.3148, lng: 131.4844, img: 'https://images.unsplash.com/photo-1542314831-068cd1dbfeeb?w=600' },
        { id: 5, tag: 'もくもく', loc: '由布岳・湯布院エリア', desc: '朝霧や由布岳周辺に広がる雲、そして温泉の湯気がもくもくと立ち上る絶景。', lat: 33.2842, lng: 131.3917, img: 'https://images.unsplash.com/photo-1506744038136-46273834b3fb?w=600' },
        { id: 6, tag: 'モチモチ', loc: '由布岳・湯布院エリア', desc: 'コシと粘りのある美味しい手打ちうどん。麺がモチモチ！', lat: 33.2800, lng: 131.3950, img: 'https://picsum.photos/id/429/300/300' },
        { id: 7, tag: 'シュワシュワ', loc: '別府温泉エリア', desc: '炭酸成分を多く含んだ珍しい温泉。肌に気泡がシュワシュワとつきます。', lat: 33.3200, lng: 131.4800, img: 'https://picsum.photos/id/1043/300/300' },
        { id: 8, tag: 'フワフワ', loc: '大分駅周辺エリア', desc: '駅近くのカフェで見つけたシフォンケーキ。口に入れるとフワフワ。', lat: 33.2341, lng: 131.6054, img: 'https://picsum.photos/id/1025/300/300' }
    ];

    // ローカルストレージ対応
    let locationData = JSON.parse(localStorage.getItem('combined_onomato_posts')) || defaultData;
    let selectedActiveItem = null; // 現在詳細画面で開いているアイテムを追跡

    // DOM要素
    const menuBtn = document.getElementById('menuBtn');
    const filterMenu = document.getElementById('filterMenu');
    const galleryGrid = document.getElementById('galleryGrid');
    const searchInput = document.getElementById('searchInput');
    const addPostBtn = document.getElementById('addPostBtn');

    const detailModal = document.getElementById('detailModal');
    const detailClose = document.getElementById('detailClose');
    const formModal = document.getElementById('formModal');
    const formClose = document.getElementById('formClose');
    const postForm = document.getElementById('postForm');
    const deletePostBtn = document.getElementById('deletePostBtn');

    const modalImg = document.getElementById('modalImg');
    const modalTag = document.getElementById('modalTag');
    const modalLoc = document.getElementById('modalLoc');
    const modalDesc = document.getElementById('modalDesc');

    // メイン地図の初期化
    const mainMap = L.map('mainMap').setView([33.35, 131.50], 9);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '© OpenStreetMap'
    }).addTo(mainMap);

    // 詳細モーダル内のミニ地図の初期化
    const modalMap = L.map('modalMap', {
        zoomControl: false,
        attributionControl: false
    }).setView([33.35, 131.50], 11);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(modalMap);

    let currentCircles = []; 
    let detailCircle = null;

    // 画面・地図のビュー更新関数
    function updateAppView(items) {
        galleryGrid.innerHTML = '';
        currentCircles.forEach(circle => mainMap.removeLayer(circle));
        currentCircles = [];

        items.forEach(item => {
            // ギャラリーへ追加
            const div = document.createElement('div');
            div.className = 'gallery-item';
            div.innerHTML = `<img src="${item.img}" alt="${item.tag}">`;
            div.addEventListener('click', () => openDetailModal(item));
            galleryGrid.appendChild(div);

            // メイン地図上に半径1kmの曖昧な円を描画
            const circle = L.circle([item.lat, item.lng], {
                color: '#0095f6',
                fillColor: '#0095f6',
                fillOpacity: 0.25,
                radius: 1000 
            }).addTo(mainMap);
            
            circle.bindPopup(`<b>${item.loc}付近<br>#${item.tag} なスポット！</b>`);
            currentCircles.push(circle);
        });
    }

    // 詳細モーダルを開く
    function openDetailModal(item) {
        selectedActiveItem = item; // 削除対象として記憶
        modalImg.src = item.img;
        modalTag.textContent = `# ${item.tag}`;
        modalLoc.textContent = `📍 位置: ${item.loc}`;
        modalDesc.textContent = item.desc || "説明はありません。";
        
        detailModal.classList.add('active');
        // ミニマップの描画処理
        setTimeout(() => {
            modalMap.invalidateSize();
            modalMap.setView([item.lat, item.lng], 12);
            
            if (detailCircle) {
                modalMap.removeLayer(detailCircle);
            }
         
            detailCircle = L.circle([item.lat, item.lng], {
                color: '#aa1e2d',
                fillColor: '#aa1e2d',
                fillOpacity: 0.3,
                radius: 1000
            }).addTo(modalMap);
        }, 200);
    }

    // 削除ボタンのクリックイベント
    deletePostBtn.addEventListener('click', () => {
        if (!selectedActiveItem) return;

        const confirmDelete = confirm(`この「#${selectedActiveItem.tag}」の投稿を削除してもよろしいですか？`);
        if (confirmDelete) {
            // IDが一致しないデータだけで新しく配列を作り直す
            locationData = locationData.filter(item => item.id !== selectedActiveItem.id);
            
            // ローカルストレージを更新
            localStorage.setItem('combined_onomato_posts', JSON.stringify(locationData));
            
            // 画面の更新とモーダルを閉じる
            updateAppView(locationData);
            detailModal.classList.remove('active');
            selectedActiveItem = null;
        }
    });

    // モーダルを閉じる
    detailClose.addEventListener('click', () => {
        detailModal.classList.remove('active');
        selectedActiveItem = null;
    });
    formClose.addEventListener('click', () => formModal.classList.remove('active'));

    addPostBtn.addEventListener('click', () => formModal.classList.add('active'));

    // 新規投稿フォームの送信
    postForm.addEventListener('submit', (e) => {
        e.preventDefault();
        
        const fileInput = document.getElementById('formFile');
        const tagInput = document.getElementById('formTag').value.trim();
        const areaSelect = document.getElementById('formArea').value;
        const descInput = document.getElementById('formDesc').value.trim();

        const file = fileInput.files[0];
        if (file) {
            const reader = new FileReader();
            reader.onload = function(event) {
                const base64Image = event.target.result;
                const coords = areaCoordinates[areaSelect] || { lat: 33.35, lng: 131.50 };

                // ランダムな位置の微調整（同じエリア内で重なりすぎないようにする）
                const randomLat = coords.lat + (Math.random() - 0.5) * 0.01;
                const randomLng = coords.lng + (Math.random() - 0.5) * 0.01;

                const newPost = {
                    id: Date.now(), // ユニークなIDを設定
                    tag: tagInput,
                    loc: areaSelect,
                    desc: descInput,
                    lat: randomLat,
                    lng: randomLng,
                    img: base64Image
                };
                locationData.push(newPost);
                localStorage.setItem('combined_onomato_posts', JSON.stringify(locationData));

                updateAppView(locationData);
                postForm.reset();
                formModal.classList.remove('active');
            };
            reader.readAsDataURL(file);
        }
    });

    // 検索・絞り込みメニューの処理
    menuBtn.addEventListener('click', (e) => {
        e.stopPropagation();
        filterMenu.classList.toggle('active');
    });
    document.addEventListener('click', () => filterMenu.classList.remove('active'));

    filterMenu.addEventListener('click', (e) => {
        if (e.target.classList.contains('filter-item')) {
            const selectedTag = e.target.getAttribute('data-tag');
            searchInput.value = selectedTag === 'すべて' ? '' : selectedTag;
            
            const filtered = selectedTag === 'すべて' 
                ? locationData 
                : locationData.filter(item => item.tag === selectedTag);
            
            updateAppView(filtered);
        }
    });

    searchInput.addEventListener('input', () => {
        const keyword = searchInput.value.toLowerCase().trim();
        const filtered = locationData.filter(item => 
            item.tag.toLowerCase().includes(keyword) || 
            item.loc.toLowerCase().includes(keyword) ||
            item.desc.toLowerCase().includes(keyword)
        );
        updateAppView(filtered);
    });

    window.addEventListener('click', (e) => {
        if (e.target === detailModal) {
            detailModal.classList.remove('active');
            selectedActiveItem = null;
        }
        if (e.target === formModal) formModal.classList.remove('active');
    });

    // 起動時にデータを反映
    updateAppView(locationData);
</script>

</body>
</html>
