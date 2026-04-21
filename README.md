
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Telegram VPN</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<script src="https://telegram.org/js/telegram-web-app.js"></script>

<style>
    body {
        margin: 0;
        font-family: Arial;
        background: #0f172a;
        color: white;
        text-align: center;
    }

    header {
        padding: 20px;
        font-size: 22px;
        background: #111827;
    }

    .card {
        margin: 20px;
        padding: 20px;
        background: #1f2937;
        border-radius: 15px;
    }

    .btn {
        padding: 12px 25px;
        border: none;
        border-radius: 20px;
        background: #22c55e;
        color: white;
        font-size: 16px;
        cursor: pointer;
        margin-top: 10px;
    }

    .btn.off {
        background: #ef4444;
    }

    .server {
        margin: 10px 0;
        padding: 10px;
        background: #111827;
        border-radius: 10px;
        cursor: pointer;
    }

    .active {
        border: 2px solid #22c55e;
    }
</style>
</head>

<body>

<header>🔐 VPN Директора Автоваза</header>

<div class="card">
    <h3>Статус</h3>
    <div id="status">Отключен 🔴</div>

    <button class="btn" id="connectBtn" onclick="toggleVPN()">
        Подключить VPN
    </button>
</div>

<div class="card">
    <h3>Серверы</h3>

    <div class="server active" onclick="selectServer(this, 'Netherlands')">
        🇳🇱 Нидерланды
    </div>

    <div class="server" onclick="selectServer(this, 'Germany')">
        🇩🇪 Германия
    </div>

    <div class="server" onclick="selectServer(this, 'USA')">
        🇺🇸 США
    </div>
</div>

<script>
let tg = window.Telegram.WebApp;
tg.expand();

let connected = false;
let server = "Netherlands";

function toggleVPN() {
    connected = !connected;

    let status = document.getElementById("status");
    let btn = document.getElementById("connectBtn");

    if (connected) {
        status.innerHTML = "Подключен 🟢 (" + server + ")";
        btn.innerHTML = "Отключить VPN";
        btn.classList.add("off");
    } else {
        status.innerHTML = "Отключен 🔴";
        btn.innerHTML = "Подключить VPN";
        btn.classList.remove("off");
    }

    tg.sendData(JSON.stringify({
        action: connected ? "connect" : "disconnect",
        server: server
    }));
}

function selectServer(el, name) {
    document.querySelectorAll(".server").forEach(s => s.classList.remove("active"));
    el.classList.add("active");
    server = name;
}
</script>

</body>
</html>
