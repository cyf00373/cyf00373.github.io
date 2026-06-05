# cyf00373.github.io
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>動態人員計數器（手機優化版）</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            margin: 15px;
            background-color: #f4f7f6;
            /* 防止手機上連續點擊時選取到文字 */
            user-select: none; 
            -webkit-user-select: none;
        }
        h2 {
            color: #333;
            margin-bottom: 5px;
            font-size: 22px;
        }
        .container {
            max-width: 500px;
            margin: 0 auto;
            background: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.08);
        }
        /* 功能按鈕樣式 */
        .action-btn {
            width: 100%; /* 在手機上按鈕撐滿，更好按 */
            padding: 12px;
            font-size: 16px;
            background-color: #008CBA;
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            margin-bottom: 15px;
            font-weight: bold;
        }
        /* 姓名按鈕與計數器的包裝區塊 */
        .person-row {
            display: flex;
            align-items: center;
            justify-content: space-between; /* 讓元件在手機畫面上均勻分配 */
            padding: 10px 0;
            border-bottom: 1px solid #eee;
        }
        /* 名字按鈕樣式 - 手機大按鈕 */
        .name-btn {
            flex: 1; /* 讓名字按鈕自動伸展，佔滿剩餘空間 */
            padding: 12px;
            font-size: 18px; /* 放大字體，方便閱讀 */
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            text-align: center;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            /* 解決行動裝置點擊延遲與防放大 */
            touch-action: manipulation; 
        }
        .name-btn:active {
            background-color: #3e8e41;
        }
        /* 計數器數字樣式 */
        .counter {
            font-size: 24px;
            font-weight: bold;
            color: #2c3e50;
            min-width: 60px;
            text-align: center;
        }
        /* 重置按鈕樣式 */
        .reset-btn {
            padding: 6px 10px;
            font-size: 12px;
            background-color: #e74c3c;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>人員點擊計數器</h2>
    <p style="color: #666; font-size: 14px; margin-top: 0;">點擊名字點數 +1</p>
    
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

        // 支援中英文逗號拆分
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

            // 獨立的重置按鈕 (防按錯)
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
