# pay
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>资源获取 - 扫码支付</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', 'Microsoft YaHei', sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #1a1f35, #2a1f3c, #1a2335);
            color: #fff;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            overflow-x: hidden;
        }
        
        .container {
            background: rgba(10, 15, 35, 0.92);
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.6),
                        0 0 100px rgba(98, 0, 234, 0.15);
            width: 100%;
            max-width: 650px;
            padding: 40px;
            text-align: center;
            position: relative;
            overflow: hidden;
            border: 1px solid rgba(98, 0, 234, 0.3);
            z-index: 10;
        }
        
        .container::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(98,0,234,0.15) 0%, rgba(255,255,255,0) 70%);
            z-index: -1;
            animation: rotate 30s linear infinite;
        }
        
        .header {
            position: relative;
            z-index: 2;
            margin-bottom: 30px;
        }
        
        h1 {
            font-size: 2.8rem;
            margin-bottom: 15px;
            background: linear-gradient(90deg, #ff8a00, #e52e71, #ff8a00);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
            animation: gradientShift 4s infinite linear;
            background-size: 300% 100%;
        }
        
        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        
        .subtitle {
            font-size: 1.25rem;
            color: #c0c0ff;
            margin-bottom: 30px;
            line-height: 1.6;
            text-shadow: 0 1px 3px rgba(0,0,0,0.5);
        }
        
        .highlight {
            color: #ffcc00;
            font-weight: bold;
            text-shadow: 0 0 10px rgba(255, 204, 0, 0.5);
        }
        
        .qr-container {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            padding: 25px;
            margin: 30px 0;
            position: relative;
            z-index: 2;
            backdrop-filter: blur(5px);
            border: 1px solid rgba(98, 0, 234, 0.4);
            box-shadow: 0 0 20px rgba(98, 0, 234, 0.2);
        }
        
        .qr-content {
            width: 250px;
            height: 330px;
            margin: 0 auto;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        
        .qr-image {
            width: 250px;
            height: 250px;
            border-radius: 12px;
            overflow: hidden;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .qr-image img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        .payment-info {
            margin-top: 15px;
            text-align: center;
            width: 100%;
        }
        
        .payment-info p {
            margin: 3px 0;
            font-size: 1.05rem;
            color: #ffffff;
        }
        
        .payment-info .amount {
            font-size: 1.8rem;
            font-weight: bold;
            color: #ffcc00;
            margin: 8px 0;
            text-shadow: 0 0 10px rgba(255, 204, 0, 0.3);
        }
        
        .payment-info .note {
            font-size: 0.95rem;
            color: #a0c0ff;
            margin-top: 10px;
        }
        
        .payment-info .platform {
            font-size: 1.1rem;
            font-weight: bold;
            color: #00a0e9;
            margin-top: 12px;
            text-shadow: 0 0 10px rgba(0, 160, 233, 0.3);
        }
        
        .method {
            background: rgba(255, 255, 255, 0.08);
            border-radius: 10px;
            padding: 15px 25px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            transition: all 0.3s ease;
            max-width: 300px;
            margin: 0 auto;
            border: 1px solid rgba(98, 0, 234, 0.5);
        }
        
        .method.active {
            background: rgba(0, 123, 255, 0.2);
            border: 1px solid rgba(0, 123, 255, 0.5);
        }
        
        .method i {
            font-size: 2.2rem;
            color: #00a0e9;
        }
        
        .method div {
            text-align: left;
        }
        
        .method h3 {
            font-size: 1.4rem;
            margin-bottom: 5px;
        }
        
        .method p {
            color: #a0c0ff;
            font-size: 0.95rem;
        }
        
        .instructions {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 15px;
            padding: 20px;
            margin: 25px 0;
            text-align: left;
            position: relative;
            z-index: 2;
            border: 1px solid rgba(98, 0, 234, 0.2);
        }
        
        .instructions h3 {
            color: #ffcc00;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 1.3rem;
        }
        
        .instructions ol {
            padding-left: 25px;
        }
        
        .instructions li {
            margin-bottom: 12px;
            line-height: 1.5;
            color: #d0d0ff;
        }
        
        .countdown {
            font-size: 1.8rem;
            margin: 25px 0;
            color: #ffcc00;
            display: none;
            font-weight: bold;
            text-shadow: 0 0 10px rgba(255, 204, 0, 0.5);
        }
        
        .btn {
            background: linear-gradient(90deg, #ff8a00, #e52e71);
            color: white;
            border: none;
            padding: 16px 40px;
            font-size: 1.2rem;
            border-radius: 50px;
            cursor: pointer;
            margin: 20px 0;
            transition: all 0.3s ease;
            position: relative;
            z-index: 2;
            display: none;
            box-shadow: 0 5px 15px rgba(229, 46, 113, 0.4);
            font-weight: bold;
            letter-spacing: 1px;
        }
        
        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(229, 46, 113, 0.6);
            background: linear-gradient(90deg, #ff9a33, #ff5285);
        }
        
        .btn:active {
            transform: translateY(1px);
        }
        
        .password-container {
            margin: 30px 0;
            display: none;
            animation: fadeIn 0.8s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .password-box {
            background: rgba(0, 0, 0, 0.4);
            border-radius: 10px;
            padding: 25px;
            font-size: 2.2rem;
            letter-spacing: 8px;
            font-weight: bold;
            color: #ffcc00;
            border: 2px dashed rgba(255, 204, 0, 0.4);
            position: relative;
            overflow: hidden;
            text-shadow: 0 0 15px rgba(255, 204, 0, 0.7);
            font-family: 'Courier New', monospace;
        }
        
        .password-box::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
            animation: shine 3s infinite;
        }
        
        @keyframes shine {
            100% {
                left: 100%;
            }
        }
        
        .contact {
            margin-top: 25px;
            color: #a0c0ff;
            font-size: 0.9rem;
            background: rgba(0, 0, 0, 0.3);
            padding: 12px;
            border-radius: 8px;
            display: inline-block;
        }
        
        .contact a {
            color: #ffcc00;
            text-decoration: none;
            font-weight: bold;
        }
        
        .contact a:hover {
            text-decoration: underline;
        }
        
        .footer {
            margin-top: 30px;
            font-size: 0.85rem;
            color: #8899cc;
            padding-top: 15px;
            border-top: 1px solid rgba(98, 0, 234, 0.3);
        }
        
        .pulse {
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% {
                transform: scale(1);
                box-shadow: 0 0 0 0 rgba(0, 123, 255, 0.7);
            }
            70% {
                transform: scale(1.02);
                box-shadow: 0 0 0 15px rgba(0, 123, 255, 0);
            }
            100% {
                transform: scale(1);
                box-shadow: 0 0 0 0 rgba(0, 123, 255, 0);
            }
        }
        
        .floating {
            position: absolute;
            width: 150px;
            height: 150px;
            border-radius: 50%;
            background: radial-gradient(circle, rgba(98,0,234,0.3) 0%, transparent 70%);
            filter: blur(30px);
            z-index: 1;
        }
        
        .floating:nth-child(1) {
            top: 10%;
            left: 10%;
            animation: float 15s infinite linear;
        }
        
        .floating:nth-child(2) {
            bottom: 5%;
            right: 10%;
            animation: float 20s infinite linear reverse;
            width: 200px;
            height: 200px;
        }
        
        @keyframes float {
            0% { transform: translate(0, 0) rotate(0deg); }
            25% { transform: translate(50px, 50px) rotate(90deg); }
            50% { transform: translate(100px, 0) rotate(180deg); }
            75% { transform: translate(50px, -50px) rotate(270deg); }
            100% { transform: translate(0, 0) rotate(360deg); }
        }
        
        .deploy-options {
            background: rgba(0, 0, 0, 0.4);
            border-radius: 12px;
            padding: 20px;
            margin-top: 30px;
            text-align: left;
        }
        
        .deploy-options h3 {
            color: #00ccff;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .deploy-options ul {
            padding-left: 25px;
        }
        
        .deploy-options li {
            margin-bottom: 10px;
            line-height: 1.5;
            color: #d0d0ff;
        }
        
        .deploy-options a {
            color: #ffcc00;
            text-decoration: none;
        }
        
        .deploy-options a:hover {
            text-decoration: underline;
        }
        
        @media (max-width: 600px) {
            .container {
                padding: 25px 20px;
            }
            
            h1 {
                font-size: 2.2rem;
            }
            
            .subtitle {
                font-size: 1.05rem;
            }
            
            .qr-content {
                width: 220px;
                height: 300px;
            }
            
            .qr-image {
                width: 220px;
                height: 220px;
            }
            
            .btn {
                padding: 14px 30px;
                font-size: 1.1rem;
            }
            
            .password-box {
                font-size: 1.8rem;
                padding: 20px;
                letter-spacing: 6px;
            }
        }
    </style>
</head>
<body>
    <div class="floating"></div>
    <div class="floating"></div>
    
    <div class="container">
        <div class="header">
            <h1>资源获取平台</h1>
            <div class="subtitle">
                资源由专业团队精心制作，实属不易！<span class="highlight">付款后自动显示解压密码</span>，尽享优质完整资源
            </div>
            <div class="subtitle">
                这绝对是你有史以来花的最有价值的<span class="highlight">几毛钱！</span>
            </div>
        </div>
        
        <div class="qr-container pulse">
            <div class="qr-content">
                <div class="qr-image">
                    <!-- 支付宝收款码图片 -->
                    <img src="https://aliaobing.github.io/pay/Y0099.png" alt="支付宝收款码">
                </div>
                <div class="payment-info">
                    <p>了了 发起收款</p>
                    <p class="amount">¥0.99</p>
                    <p>感谢老铁！</p>
                    <p class="note">支持信用卡 信用购 | 花呗</p>
                    <p class="platform">支付宝收款单</p>
                </div>
            </div>
        </div>
        
        <div class="method active">
            <i class="fab fa-alipay"></i>
            <div>
                <h3>支付宝支付</h3>
                <p>推荐使用支付宝扫码支付</p>
            </div>
        </div>
        
        <div class="instructions">
            <h3><i class="fas fa-info-circle"></i>操作说明</h3>
            <ol>
                <li>使用支付宝扫描上方二维码完成支付</li>
                <li>支付成功后，页面将自动开始倒计时</li>
                <li>倒计时结束后，点击"获取密码"按钮</li>
                <li>系统将显示解压密码（请妥善保存）</li>
            </ol>
        </div>
        
        <div id="countdown" class="countdown"></div>
        
        <button id="getPasswordBtn" class="btn">获取解压密码</button>
        
        <div id="passwordContainer" class="password-container">
            <div class="password-box" id="passwordDisplay">********</div>
        </div>
        
        <div class="contact">
            支付遇到问题？请联系QQ: <a href="#">1173956412</a>
        </div>
        
        <div class="deploy-options">
            <h3><i class="fas fa-cloud-upload-alt"></i>免费托管平台推荐</h3>
            <ul>
                <li><a href="https://pages.github.com/" target="_blank">GitHub Pages</a> - 最推荐的免费托管服务</li>
                <li><a href="https://www.netlify.com/" target="_blank">Netlify</a> - 简单易用的静态网站托管</li>
                <li><a href="https://vercel.com/" target="_blank">Vercel</a> - 开发者友好的部署平台</li>
                <li><a href="https://pages.cloudflare.com/" target="_blank">Cloudflare Pages</a> - 高速全球CDN</li>
            </ul>
            <p style="margin-top: 15px; color: #a0c0ff;">只需将本页HTML文件上传到上述任一平台即可免费部署！</p>
        </div>
        
        <div class="footer">
            <p>温馨提示：本资源为虚拟产品，支付成功后不支持退款</p>
            <p>© 2025 资源获取平台 - 版权所有</p>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const countdownEl = document.getElementById('countdown');
            const getPasswordBtn = document.getElementById('getPasswordBtn');
            const passwordContainer = document.getElementById('passwordContainer');
            const passwordDisplay = document.getElementById('passwordDisplay');
            
            // 模拟支付成功
            setTimeout(function() {
                // 显示倒计时
                countdownEl.style.display = 'block';
                countdownEl.textContent = '5';
                
                // 开始倒计时
                let count = 5;
                const countdownInterval = setInterval(function() {
                    count--;
                    countdownEl.textContent = count;
                    
                    if (count === 0) {
                        clearInterval(countdownInterval);
                        countdownEl.textContent = '支付验证成功！';
                        countdownEl.style.color = '#2aac19';
                        
                        // 显示按钮
                        setTimeout(function() {
                            getPasswordBtn.style.display = 'inline-block';
                            getPasswordBtn.focus();
                        }, 1000);
                    }
                }, 1000);
            }, 3000);
            
            // 点击获取密码按钮
            getPasswordBtn.addEventListener('click', function() {
                // 设置固定密码
                const password = "xiexie520";
                passwordDisplay.textContent = password;
                passwordContainer.style.display = 'block';
                
                // 复制密码到剪贴板
                navigator.clipboard.writeText(password).then(() => {
                    passwordDisplay.textContent = password + ' (已复制)';
                    setTimeout(() => {
                        passwordDisplay.textContent = password;
                    }, 2000);
                });
                
                // 添加庆祝效果
                celebrate();
            });
            
            // 庆祝效果
            function celebrate() {
                const container = document.querySelector('.container');
                for(let i = 0; i < 20; i++) {
                    const confetti = document.createElement('div');
                    confetti.innerHTML = '🎉';
                    confetti.style.position = 'absolute';
                    confetti.style.fontSize = Math.random() * 20 + 10 + 'px';
                    confetti.style.left = Math.random() * 100 + '%';
                    confetti.style.top = '-30px';
                    confetti.style.opacity = '0';
                    confetti.style.zIndex = '100';
                    confetti.style.animation = `confettiFall ${Math.random() * 3 + 2}s linear forwards`;
                    container.appendChild(confetti);
                    
                    setTimeout(() => {
                        confetti.style.opacity = '1';
                    }, 100);
                    
                    setTimeout(() => {
                        confetti.remove();
                    }, 5000);
                }
                
                // 添加CSS动画
                const style = document.createElement('style');
                style.innerHTML = `
                    @keyframes confettiFall {
                        0% {
                            transform: translateY(0) rotate(0deg);
                            opacity: 1;
                        }
                        100% {
                            transform: translateY(100vh) rotate(720deg);
                            opacity: 0;
                        }
                    }
                `;
                document.head.appendChild(style);
            }
        });
    </script>
</body>
</html>
