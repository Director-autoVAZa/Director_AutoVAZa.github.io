<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<script src="https://telegram.org/js/telegram-web-app.js"></script>

<title>VPN TG</title>

<style>
    body {
        margin: 0;
        font-family: Arial;
        background: #0b1220;
        color: white;
        text-align: center;
    }

    header {
        padding: 20px;
        font-size: 20px;
        background: #111827;
        font-weight: bold;
    }

    .card {
        margin: 15px;
        padding: 20px;
        background: #1f2937;
        border-radius: 15px;
    }

    .status {
        font-size: 18px;
        margin: 10px 0;
    }

    .btn {
        padding: 12px 25px;
        border: none;
        border-radius: 25px;
        cursor: pointer;
        font-size: 16px;
        margin-top: 10px;
        background: #22c55e;
        color: white;
    }

    .btn.off {
        background: #ef4444;
    }

    .server {
        margin: 8px 0;
        padding: 10px;
        background: #111827;
        border-radius: 10px;
        cursor: pointer;
        transition: 0.2s;
    }

    .server:hover {
        background: #374151;
    }

    .active {
        border: 2px solid #22c55e;
    }

    .log {
        font-size: 14px;
        opacity: 0.7;
        margin-top: 10px;
    }
</style>
</head>

<body>

<header>🔐 VPN Директора</header>

<div class="card">
    <div class="status" id="status">VPN отключен 🔴</div>

    <button class="btn" id="btn" onclick="toggleVPN()">
        Подключить
    </button>

    <div class="log" id="log">Выберите сервер</div>
</div>

<div class="card">
    <h3>🌍 Серверы</h3>

    <div class="server active" onclick="selectServer(this,'Netherlands')">
        🇳🇱 Нидерланды
    </div>

    <div class="server" onclick="selectServer(this,'Germany')">
        🇩🇪 Германия
    </div>

    <div class="server" onclick="selectServer(this,'USA')">
        🇺🇸 США
    </div>
</div>

<script>
let tg = window.Telegram.WebApp;
tg.expand();

let connected = false;
let server = "Netherlands";

function selectServer(el, name) {
    document.querySelectorAll(".server").forEach(s => s.classList.remove("active"));
    el.classList.add("active");
    server = name;

    document.getElementById("log").innerText = "Выбран сервер: " + server;
}

function toggleVPN() {
    let status = document.getElementById("status");
    let btn = document.getElementById("btn");

    connected = !connected;

    if (connected) {
        status.innerText = "Подключение... ⏳";

        setTimeout(() => {
            status.innerText = "VPN подключен 🟢 (" + server + ")";
        }, 1200);

        btn.innerText = "Отключить";
        btn.classList.add("off");
    } else {
        status.innerText = "VPN отключен 🔴";
        btn.innerText = "Подключить";
        btn.classList.remove("off");
    }

    tg.sendData(JSON.stringify({
        action: connected ? "connect" : "disconnect",
        server: server
    }));
}
</script>

</body>
</html>
