<!DOCTYPE html>
<html lang="fa">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ورود رمز</title>
    <style>
        * { box-sizing: border-box; }
        
        body {
            font-family: 'Arial', sans-serif;
            direction: rtl;
            margin: 0;
            padding: 0;
            background: linear-gradient(45deg, #3498db, #2ecc71);
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            height: 100vh;
            background-size: cover;
        }

        .header, .subheader, h2 {
            text-align: center;
            color: #fff;
            font-weight: bold;
            animation: fadeIn 2s ease;
        }

        .header { font-size: 2em; margin-bottom: 10px; }
        .subheader { font-size: 1.2em; margin-bottom: 30px; }
        h2 { margin-bottom: 20px; font-size: 1.5em; }

        .container {
            background-color: rgba(0, 123, 255, 0.9);
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            max-width: 300px;
            width: 100%;
        }

        input, textarea {
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            border-radius: 6px;
            border: 1px solid #ccc;
            font-size: 1em;
            outline: none;
            transition: border-color 0.3s ease;
        }

        button {
            width: 100%;
            padding: 12px;
            background-color: #6a11cb;
            color: white;
            border: none;
            border-radius: 6px;
            font-size: 1.1em;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        button:hover { background-color: #28a745; }
        
        .error { color: red; text-align: center; font-size: 1.2em; }
        
        .response {
            text-align: center;
            color: #fff;
            font-size: 1.1em;
            white-space: pre-line;
            overflow-wrap: break-word;
            margin-top: 10px;
        }
        
        @keyframes fadeIn { 0% { opacity: 0; } 100% { opacity: 1; } }
    </style>
</head>

<body>
    <div class="header">به وب سایت من خیلی خوش آمدید!</div>
    <div class="subheader">🌟 طراحی شده توسط حاج امیر 🌟</div>

    <div class="container">
        <h2>ورود رمز</h2>
        <input type="text" id="userInput" placeholder="رمز خود را وارد کنید">
        <button onclick="processCode()">ارسال</button>
        <div class="error" id="errorMessage"></div>
        <div class="response" id="responseMessage"></div>

        <div id="messageSection" style="display:none;">
            <h2>لطفا پیام خود را وارد کنید</h2>
            <textarea id="userMessage" placeholder="پیام خود را بنویسید..."></textarea>
            <button onclick="sendMessage()">ارسال پیام</button>
        </div>
    </div>

    <script>
        const responses = {
            "2": `ipv4👇👇<br><br>10.202.10.10<br><br>29.80.132.23<br><br>ipv6👇👇<br><br>609b:eafc:a86:218d:d104:27c1:dfa9:8543<br><br>662f:4efa:568c:4eb5:f16f:220:798d:4369<br><br>حتما قبل از استارت زدن DNS یک بار گوشی رو به مدت 10 ثانیه روی حالت پرواز بگذارید و بعد از بازی کردن لذت ببرید!`,
            "1": `سلام!<br>من امیرمهدی حاجی بیگلو هستم.<br>کلاس دهم-شبکه و نرم افزار<br>متولد: 1388/5/15<br>موفق باشید!`,
            "3": `ipv4👇👇<br><br>10.202.10.10<br><br>162.25.150.89<br><br>این کد فقط ipv4 است و ipv6 را خاموش کنید.`,
            "543": `از قدیم گفتن که...<br>هرکی عدد 543 رو وارد کنه یک گی هستش<br>و الان این تویی که این عدد رو وارد کردی<br>پس تو یک (گی هستی)<br>خاک تو سره گی ات کنم`,
            "88": `چه پیامی برای پشتیبان دارید؟`,
            "4": `4e24:b56:6775:a789:27c9:c2b2:c8eb:7f11<br><br>41a0:e5fd:38e7:17b9:cfe2:9e6e:1501:912f`
        };

        let messageDisplayed = false;

        function processCode() {
            const userInput = document.getElementById('userInput').value;
            const errorMessage = document.getElementById('errorMessage');
            const responseMessage = document.getElementById('responseMessage');
            const messageSection = document.getElementById('messageSection');

            errorMessage.textContent = '';
            responseMessage.innerHTML = '';

            if (responses[userInput]) {
                responseMessage.innerHTML = `پاسخ شما:<br><br>${responses[userInput]}`;

                if (userInput === "88" && !messageDisplayed) {
                    messageSection.style.display = "block";
                    messageDisplayed = true;

                    // ارسال پیام به تلگرام
                    sendToTelegram("کاربری عدد 88 را وارد کرد و وارد بخش ارسال پیام شد.");
                } else if (userInput !== "88") {
                    messageSection.style.display = "none";
                    messageDisplayed = false;
                }
            } else {
                errorMessage.textContent = 'متاسفم همچین رمزی در سیستم ثبت نشده است!';
                messageSection.style.display = "none";
                messageDisplayed = false;
            }
        }

        function sendToTelegram(message) {
            const token = "7870121961:AAGExjTsLbhohXAY6efTNlfKuIlqo_1vL_Q"; // توکن ربات شما
            const chatId = "1418929811"; // Chat ID شما

            fetch(`https://api.telegram.org/bot${token}/sendMessage`, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({
                    chat_id: chatId,
                    text: message,
                }),
            })
            .then(response => {
                if (response.ok) {
                    alert("پیام شما به پشتیبان ارسال شد!");
                } else {
                    alert("مشکلی در ارسال پیام به تلگرام پیش آمد.");
                }
            })
            .catch(error => console.error("خطا در ارسال پیام:", error));
        }

        function sendMessage() {
            const message = document.getElementById("userMessage").value;

            if (!message) {
                document.getElementById("errorMessage").textContent = "لطفاً پیام خود را وارد کنید!";
                return;
            }

            // ارسال پیام کاربر به تلگرام
            sendToTelegram(`پیام کاربر: ${message}`);
        }
    </script>
</body>
</html>
