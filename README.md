# cyf00373.github.io
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>動態人員計數器（Safari 最佳化版）</title>
    <style>
        * {
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent; /* 移除 Safari 點擊時的藍色高亮框 */
        }
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            margin: 0;
            padding: 15px;
            background-color: #f4f7f6;
            user-select: none; 
            -webkit-user-select: none; /* 防止 iOS 點擊時誤選取文字 */
            min-height: 100svh; /* 針對 Safari 網址列設計的動態高度 */
            display: flex;
            justify-content: center;
            align-items: flex-start;
        }
        .container {
            width: 100%;
            max-width: 480px; /* 完美符合手機單手操作寬度 */
            background: white;
            padding: 24px 16px;
            border-radius: 16px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
            margin-top: 10px;
        }
        h2 {
            color: #333;
            margin: 0 0 4px 0;
            font-size: 22px;
            text-align: center;
        }
        .subtitle {
            color: #666;
            font-size: 13px;
            margin: 0 0 16px 0;
            text-align: center;
        }
        /* 功能按鈕樣式 */
        .action-btn {
            -webkit-appearance: none; /* 移除 iOS 預設按鈕外觀 */
            width: 100%;
            padding: 14px;
            font-size: 16px;
            background-color: #008CBA;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            margin-bottom: 20px;
            font-weight: bold;
            display: block;
        }
        /* 姓名按鈕與計數器的包裝區塊 */
        .person-row {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 12px 0;
            border-bottom: 1px solid #edf2f7;
        }
        .person-row:last-child {
            border-bottom: none; /* 最後一行不要底線 */
        }
        /* 名字按鈕樣式 - 針對 Safari 指尖觸控優化 */
        .name-btn {
            -webkit-appearance: none; /* 移除 iOS 預設按鈕外觀 */
            flex: 1;
            padding: 14px 10px; /* 加大垂直點擊範圍 */
            font-size: 18px;
            font-weight: 600;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            text-align: center;
            white-space: nowrap; /* 防止名字斷行 */
            overflow: hidden;
            text-overflow: ellipsis;
            box-shadow: 0 2px 4px rgba(76, 175, 80, 0.2);
            touch-action: manipulation; /* 停用 Safari 的雙擊縮放延遲 */
        }
        .name-btn:active {
            background-color: #3e8e41;
            transform: scale(0.98); /* 點擊時的微小回饋感 */
        }
        /* 計數器數字樣式 */
        .counter {
            font-size: 26px; /* 放大數字，更清晰 */
            font-weight: 700;
            color: #2c3e50;
            width: 70px; /* 固定寬度，防止數字從個位數變十位數時排版跳動 */
            text-align: center;
            font-variant-numeric: tabular-nums; /* 讓每個數字寬度一致，對齊更好看 */
        }
        /* 重置按鈕樣式 */
        .reset-btn {
            -webkit-appearance: none;
            padding: 8px 12px;
            font-size: 13px;
            background-color: #fee2e2;
            color: #ef4444;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 500;
        }
        .reset-btn:active {
            background-color: #fca5a5;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>人員點擊計數器</h2>
    <div class="subtitle">點擊名字按鈕即可讓點數 +1</div>
    
    <button class="action-btn" id="import-btn">📱 點此輸入人員名單</button>
    
    <div id="button-list"></div>
</div>

<script>
    const importBtn = document.getElementById('import-btn');
    const buttonListContainer = document.getElementById('button-list');

    importBtn.addEventListener('click', () => {
        const input = prompt("請輸入人員名單，並用逗號(,)分隔：\n例如: 張小明,王大同,李美玲");
        
        if (input === null || input.trim() === "") return; 

        buttonListContainer.innerHTML = "";

        // 支援中英文逗號
        const memberList = input.split(/[,，]/).map(name => name.trim()).filter(name => name !== "");

        memberList.forEach(name => {
            const row = document.createElement('div');
            row.className = 'person-row';

            // 名字按鈕
            const button = document.createElement('button');
            button.className = 'name-btn';
            button.innerText = name;

            // 數字顯示
            const countSpan = document.createElement('span');
            countSpan.className = 'counter';
            countSpan.innerText = '0';

            // 重置按鈕
            const resetBtn = document.createElement('button');
            resetBtn.className = 'reset-btn';
            resetBtn.innerText = '重置';

            // 點擊名字 +1
            button.addEventListener('click', () => {
                let currentCount = parseInt(countSpan.innerText, 10);
                countSpan.innerText = currentCount + 1;
            });

            // 點擊重置歸零
            resetBtn.addEventListener('click', () => {
                if(confirm(`確定要將 ${name} 的計數歸零嗎？`)) {
                    countSpan.innerText = '0';
                }
            });

            row.appendChild(button);
            row.appendChild(countSpan);
            row.appendChild(resetBtn);
            buttonListContainer.appendChild(row);
        });
    });
</script>

</body>
</html>
