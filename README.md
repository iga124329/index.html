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
            margin: 0; background: var(--bg-color); color: #333; line-height: 1.6;
        }
        header {
            background: var(--secondary-color); color: white; padding: 20px;
            text-align: center; box-shadow: 0 2px 10px rgba(0,0,0,0.2);
        }
        .container { max-width: 600px; margin: auto; padding: 20px; }
        .page { display: none; animation: fadeIn 0.3s ease; }
        .active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .card {
            background: white; border-radius: 12px; padding: 20px; margin-bottom: 15px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05); cursor: pointer; border: 1px solid #ddd;
        }
        .back-btn {
            background: #95a5a6; color: white; border: none; padding: 10px 20px;
            border-radius: 8px; margin-bottom: 20px; font-size: 16px; cursor: pointer;
        }
        .sop-section { background: white; border-radius: 15px; padding: 25px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
        .ingredient-item { background: #fff8f0; border-left: 4px solid var(--primary-color); padding: 10px; margin-bottom: 8px; list-style: none; }
        
        /* 關鍵修正：步驟圖文並列 */
        .step-container { margin-bottom: 25px; border-bottom: 2px solid #eee; padding-bottom: 15px; }
        .step-text { display: flex; align-items: flex-start; gap: 10px; margin-bottom: 10px; font-weight: bold; }
        .step-num {
            background: var(--secondary-color); color: white; border-radius: 50%;
            width: 25px; height: 25px; display: flex; align-items: center; justify-content: center; flex-shrink: 0;
        }
        .step-img { width: 100%; border-radius: 10px; overflow: hidden; background: #eee; }
        .step-img img { width: 100%; display: block; }
        
        .version { font-size: 12px; color: #999; margin-top: 10px; text-align: right; }
    </style>
</head>
<body>

<header><h1>餐廳 SOP 管理系統</h1></header>

<div class="container">
    <div id="page1" class="page active">
        <h2>📂 選擇料理系列</h2>
        <div class="card" onclick="showPage2('義大利麵系列')"><h3>🍝 義大利麵系列</h3></div>
        <div class="card" onclick="showPage2('湯品鍋物系列')"><h3>🍲 湯品鍋物系列</h3></div>
        <div class="card" onclick="showPage2('炸物系列')"><h3>🍗 炸物系列</h3></div>
        <div class="card" onclick="showPage2('單點系列')"><h3>🥗 單點系列</h3></div>
    </div>

    <div id="page2" class="page">
        <button class="back-btn" onclick="showPage1()">← 返回系列</button>
        <h2 id="category-name"></h2>
        <div id="item-list"></div>
    </div>

    <div id="page3" class="page">
        <button class="back-btn" onclick="showPage2(currentCategory)">← 返回列表</button>
        <div class="sop-section">
            <h2 id="dish-name" style="color:var(--primary-color);"></h2>
            <h3>🛒 食材清單（Mise en Place）</h3>
            <div id="ingredients-container"></div>
            <br>
            <h3>👨‍🍳 製作流程（SOP）</h3>
            <div id="steps-display-area"></div>
            <div class="version">最後更新：2026-02</div>
        </div>
    </div>
</div>

<script>
const recipeData = {
    "義大利麵系列": [
        {
            name: "泡菜辣肉醬義大利麵",
            ingredients: ["義大利麵 180g（預煮）", "特製辣肉醬 100g", "韓式泡菜 40g", "起司粉 / 蔥花"],
            // 這裡還原圖文並茂的流程
            steps: [
                { desc: "加熱底醬與泡菜均勻翻炒", img: "pasta_step1.jpg" },
                { desc: "加入麵條與少許煮麵水煨煮", img: "pasta_step2.jpg" },
                { desc: "收汁至掛麵，淋上香油", img: "pasta_step3.jpg" },
                { desc: "裝盤並撒上起司粉完成", img: "泡菜肉醬義大利麵.jpg" }
            ]
        },
        {
            name: "韓式烤肉義大利麵",
            ingredients: ["義大利麵 180g（預煮）", "板腱牛 100g（逆紋切）", "烤肉醬 5g", "雞高湯 75ml", "蒜碎 / 辣椒 / 芝麻葉"],
            steps: [
                { desc: "煎烤牛肉，表面上色後靜置保汁", img: "beef_step1.jpg" },
                { desc: "爆香蒜碎與辣椒（避免焦化）", img: "beef_step2.jpg" },
                { desc: "加入義大利麵與雞高湯煨煮", img: "beef_step3.jpg" },
                { desc: "切牛肉擺盤，完成出餐", img: "韓式烤肉義大利麵.jpg" }
            ]
        }
    ],
    "湯品鍋物系列": [
        {
            name: "辣炒年糕",
            ingredients: ["年糕 156g", "魚板 10g", "年糕醬 50g", "雞高湯 150ml"],
            steps: [
                { desc: "炒香蒜碎與洋蔥", img: "tteok_step1.jpg" },
                { desc: "加入高湯、年糕、醬汁煨煮至濃稠", img: "辣炒年糕.jpg" }
            ]
        }
    ],
    "炸物系列": [
        {
            name: "韓式炸雞",
            ingredients: ["雞肉", "炸粉", "醬汁"],
            steps: [{ desc: "裹粉油炸至金黃酥脆", img: "炸雞.jpg" }]
        }
    ],
    "單點系列": [
        {
            name: "韓式小菜",
            ingredients: ["各式小菜"],
            steps: [{ desc: "裝盤出餐", img: "小菜.jpg" }]
        }
    ]
};

let currentCategory = "";

function showPage1() { 
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById("page1").classList.add("active");
}

function showPage2(category) {
    currentCategory = category;
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById("page2").classList.add("active");
    document.getElementById("category-name").innerText = category;
    const list = document.getElementById("item-list");
    list.innerHTML = "";
    recipeData[category].forEach(item => {
        const div = document.createElement("div");
        div.className = "card";
        div.innerHTML = `<h3>${item.name}</h3>`;
        div.onclick = () => showPage3(item);
        list.appendChild(div);
    });
}

function showPage3(item) {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById("page3").classList.add("active");
    document.getElementById("dish-name").innerText = item.name;
    document.getElementById("ingredients-container").innerHTML = 
        item.ingredients.map(i => `<li class="ingredient-item">${i}</li>`).join("");
    
    // 渲染圖文並列的製作流程
    const stepsArea = document.getElementById("steps-display-area");
    stepsArea.innerHTML = item.steps.map((s, index) => `
        <div class="step-container">
            <div class="step-text">
                <div class="step-num">${index + 1}</div>
                <div>${s.desc}</div>
            </div>
            <div class="step-img">
                <img src="${s.img}" alt="步驟圖" onerror="this.src='https://placehold.co/600x400?text=圖片尚未上傳'">
            </div>
        </div>
    `).join("");
}

function hideAllPages() { document.querySelectorAll(".page").forEach(p => p.classList.remove("active")); }
</script>
</body>
</html>
