<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>廚房標準作業程序 (SOP)</title>
    <style>
        :root {
            --primary-color: #d35400;
            --secondary-color: #2c3e50;
            --accent-color: #27ae60;
            --bg-color: #f4f7f6;
        }
        body {
            font-family: -apple-system, "Noto Sans TC", sans-serif;
            margin: 0;
            background: var(--bg-color);
            color: #333;
            line-height: 1.6;
        }
        header {
            background: var(--secondary-color);
            color: white;
            padding: 20px;
            text-align: center;
            box-shadow: 0 2px 10px rgba(0,0,0,0.2);
        }
        .container {
            max-width: 600px;
            margin: auto;
            padding: 20px;
        }
        .page { display: none; animation: fadeIn 0.3s ease; }
        .active { display: block; }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .card {
            background: white;
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 15px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
            cursor: pointer;
            transition: 0.2s;
            border: 1px solid #ddd;
        }
        .card:active { transform: scale(0.98); background: #f9f9f9; }
        .card h3 { margin-top: 0; color: var(--secondary-color); }
        .back-btn {
            background: #95a5a6;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 8px;
            margin-bottom: 20px;
            font-size: 16px;
            cursor: pointer;
        }
        .sop-section {
            background: white;
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        .ingredient-item {
            background: #fff8f0;
            border-left: 4px solid var(--primary-color);
            padding: 10px;
            margin-bottom: 8px;
            list-style: none;
        }
        .step-item {
            margin-bottom: 15px;
            padding-bottom: 15px;
            border-bottom: 1px dashed #eee;
            display: flex;
            gap: 10px;
        }
        .step-num {
            background: var(--secondary-color);
            color: white;
            border-radius: 50%;
            width: 25px;
            height: 25px;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-shrink: 0;
            font-size: 14px;
        }
        .photo-placeholder {
            background: #eee;
            border-radius: 10px;
            overflow: hidden;
            margin-top: 15px;
            min-height: 200px;
        }
        .photo-placeholder img {
            width: 100%;
            height: auto;
            display: block;
            object-fit: cover;
        }
        .version {
            font-size: 12px;
            color: #999;
            margin-top: 10px;
            text-align: right;
        }
    </style>
</head>
<body>

<header>
    <h1>餐廳 SOP 管理系統</h1>
</header>

<div class="container">
    <div id="page1" class="page active">
        <h2>📂 選擇料理系列</h2>
        <div class="card" onclick="showPage2('義大利麵系列')">
            <h3>🍝 義大利麵系列</h3>
            <p>泡菜肉醬、韓式烤肉、季節時蔬</p>
        </div>
        <div class="card" onclick="showPage2('湯品鍋物系列')">
            <h3>🍲 湯品鍋物系列</h3>
            <p>泡菜豆腐鍋、辣炒年糕、韓式雞湯</p>
        </div>
    </div>

    <div id="page2" class="page">
        <button class="back-btn" onclick="showPage1()">← 返回系列</button>
        <h2 id="category-name">系列名稱</h2>
        <div id="item-list"></div>
    </div>

    <div id="page3" class="page">
        <button class="back-btn" onclick="showPage2(currentCategory)">← 返回列表</button>
        <div class="sop-section">
            <h2 id="dish-name" style="color:var(--primary-color); margin-top:0;"></h2>
            <h3>🛒 食材清單</h3>
            <div id="ingredients-container"></div>
            <h3>👨‍🍳 製作流程</h3>
            <div id="steps-container"></div>
            <h3>📸 成品標準圖</h3>
            <div id="photo-container" class="photo-placeholder"></div>
            <div class="version">SOP v1.1｜最後更新：2026-02</div>
        </div>
    </div>
</div>

<script>
const recipeData = {
    "義大利麵系列": [
        {
            name: "泡菜辣肉醬義大利麵",
            image: "泡菜肉醬義大利麵.jpg",
            ingredients: ["義大利麵 180g", "特製辣肉醬 100g", "韓式泡菜 40g", "起司粉", "青蔥"],
            steps: ["加熱底醬與泡菜翻炒", "加入麵條與少許煮麵水煨煮", "收汁至掛麵，淋上香油", "裝盤撒上起司粉與蔥花"]
        },
        {
            name: "韓式烤肉義大利麵",
            image: "韓式烤肉義大利麵.jpg",
            ingredients: ["義大利麵 180g", "板腱牛 100g", "烤肉醬", "雞高湯 75ml", "辣椒絲", "芝麻葉"],
            steps: ["煎烤牛肉至表面上色靜置", "爆香蒜碎，加入麵條與高湯煨煮", "乳化收汁後將牛肉切片擺盤", "點綴辣椒絲與芝麻葉"]
        },
        {
            name: "季節時蔬義大利麵",
            image: "季節時蔬義大利麵.jpg",
            ingredients: ["義大利麵 180g", "季節時蔬", "蒜碎", "橄欖油"],
            steps: ["橄欖油爆香蒜碎", "放入季節時蔬翻炒至軟化", "加入麵條與高湯收汁", "維持清爽色澤，裝盤出餐"]
        }
    ],
    "湯品鍋物系列": [
        {
            name: "泡菜豆腐鍋",
            image: "泡菜豆腐鍋.jpg",
            ingredients: ["韓式泡菜", "嫩豆腐 1盒", "豬肉片", "雞蛋 1顆", "高湯"],
            steps: ["炒香肉片與泡菜", "加入高湯煮沸", "放入豆腐煨煮入味", "起鍋前加入雞蛋與蔥段"]
        },
        {
            name: "辣炒年糕",
            image: "辣炒年糕.jpg",
            ingredients: ["年糕 156g", "魚板", "辣炒年糕醬", "洋蔥", "起司"],
            steps: ["炒香洋蔥與魚板", "加入高湯與醬汁煮滾", "放入年糕煮至軟Q濃稠", "鋪上起司融化後出餐"]
        },
        {
            name: "韓式雞湯",
            image: "韓式雞湯.jpg",
            ingredients: ["雞肉塊", "高麗菜", "年糕", "蔥段", "特製雞高湯"],
            steps: ["將雞肉與高湯燉煮出味", "放入高麗菜與年糕煮熟", "確認湯頭清澈入味", "點綴蔥段完成"]
        }
    ]
};

let currentCategory = "";

function showPage1() {
    hideAllPages();
    document.getElementById("page1").classList.add("active");
}

function showPage2(category) {
    currentCategory = category;
    hideAllPages();
    document.getElementById("page2").classList.add("active");
    document.getElementById("category-name").innerText = category;
    const list = document.getElementById("item-list");
    list.innerHTML = "";
    recipeData[category].forEach(item => {
        const div = document.createElement("div");
        div.className = "card";
        div.innerHTML = `<h3>${item.name}</h3><p>點擊查看 SOP</p>`;
        div.onclick = () => showPage3(item);
        list.appendChild(div);
    });
}

function showPage3(item) {
    hideAllPages();
    document.getElementById("page3").classList.add("active");
    document.getElementById("dish-name").innerText = item.name;
    document.getElementById("ingredients-container").innerHTML =
        item.ingredients.map(i => `<li class="ingredient-item">${i}</li>`).join("");
    document.getElementById("steps-container").innerHTML =
        item.steps.map((s, i) => `<div class="step-item"><div class="step-num">${i+1}</div><div>${s}</div></div>`).join("");
    const photo = document.getElementById("photo-container");
    photo.innerHTML = item.image ? `<img src="${item.image}" alt="${item.name}">` : "尚未上傳照片";
}

function hideAllPages() {
    document.querySelectorAll(".page").forEach(p => p.classList.remove("active"));
}
</script>
</body>
</html>
