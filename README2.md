<?php

// Ваш токен от BotFather
$token = "6380873916:AAG3MukymrW5OEMMk1-F08HDzUga7pRkNOI";
// URL Telegram API
$apiUrl = "https://api.telegram.org/bot$token/";

// Получаем данные от webhook
$content = file_get_contents("php://input");
$update = json_decode($content, TRUE);

// Получаем ID чата и текст сообщения
$chatId = $update["message"]["chat"]["id"];
$message = $update["message"]["text"];

// Функция отправки сообщения
function sendMessage($chatId, $message, $apiUrl, $parseMode, $replyMarkup) {
    $sendMessageUrl = $apiUrl . "sendMessage?chat_id=" . $chatId . "&text=" . urlencode($message) . "&parse_mode=" . $parseMode . "&reply_markup=" . $replyMarkup;
    file_get_contents($sendMessageUrl);
}



// Простая логика ответа
switch($message) {
    case "/start":
        $reply = "Hi! If you need some help click here /help";
        $replyMarkup = json_encode(['remove_keyboard' => true]); // Убираем клавиатуру
        break;
    case "/help":
        $reply = "Go to /step1";
        $replyMarkup = json_encode(['remove_keyboard' => true]); // Убираем клавиатуру
        break;
    case "/step1":
        $reply = "Go to /step2";
        // Добавляем кнопку для /step1
        $inlineButton = [
    'inline_keyboard' => [
        [['text' => 'You win 1000000$', 'callback_data' => '/step2']]
    ]
];
$replyMarkup = json_encode($inlineButton);
        break;
    case "/step2":
        $reply = "Game Over";
        // Добавляем кнопку для /step2
        $replyMarkup = json_encode([
            'inline_keyboard' => [
                [['text' => 'You loose 1000000$', 'url' => 'https://www.google.com']]
            ]
        ]);
        break;
    case "/step3":
        $reply = "<b>12345</b>67890 <a href=\"https://www.google.at/\">Google</a>";
        $replyMarkup = json_encode(['remove_keyboard' => true]); // Убираем клавиатуру
        break;    
    default:
        $reply = "Sorry I know /step1 /step2 only. If you need some help click here /help";
        $replyMarkup = json_encode(['remove_keyboard' => true]); // Убираем клавиатуру
}

// Отправляем ответ
sendMessage($chatId, $reply, $apiUrl, "HTML", $replyMarkup);
