# php-tg-bot

<?php

// Ваш токен от BotFather
$token = "API";
// URL Telegram API
$apiUrl = "https://api.telegram.org/bot$token/";

// Получаем данные от webhook
$content = file_get_contents("php://input");
$update = json_decode($content, TRUE);

// Получаем ID чата и текст сообщения
$chatId = $update["message"]["chat"]["id"];
$message = $update["message"]["text"];

// Функция отправки сообщения
function sendMessage($chatId, $message, $apiUrl) {
    $sendMessageUrl = $apiUrl . "sendMessage?chat_id=" . $chatId . "&text=" . urlencode($message);
    file_get_contents($sendMessageUrl);
}

// Простая логика ответа
switch($message) {
    case "/start":
        $reply = "If you need some help go to /help";
        break;
    case "/help":
        $reply = "Commands: /step1 /step2";
        break;
        case "/step1":
            $reply = "Yes! Step 1";
            break;
            case "/step2":
                $reply = "Yes! Step 2";
                break;
    default:
        $reply = "If you need some help go to /help";
}

// Отправляем ответ
sendMessage($chatId, $reply, $apiUrl);

// web hook
https://api.telegram.org/botAPI/setwebhook?url=https://url/tg-bot/index.php
