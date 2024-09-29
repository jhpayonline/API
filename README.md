1) Зарегистрируйтесь на сайте https://jhpay.online

2) Войдите в личный кабинет https://jhpay.online

3) Перейдите в "Мои мерчанты" https://jhpay.online/shops/index

4) Создайте мерчант "New shop"

5) Заполните поля, где
Имя - Название проекта

Url site - Адрес Вашего сайта (Например: https://example.com)

Url result - На этот адрес будет отправлен FEEDBACK после оплаты (Например: https://example.com/callback.php)

Url success - Пользователь будет отправлен по этой ссылке после "Успешной оплаты" (Например: https://example.com/success.php)

Url fail - Пользователь будет отправлен по этой ссылке в случае "Ошибки оплаты" (Например: https://example.com/fail.php)

6) Создайте магазин. После перейдите в его настройки. Там увидите API токен. Его копируем, он понадобится нам дальше! 



"*" - Обязательный параметры

-------------------СОЗДАНИЕ ПЛАТЕЖА-----------------------

https://pay.jhpay.online/api/pay/order/create (POST запрос)

orderNumber* (обязательное) (строка)	Уникальный номер платежа в вашей системе.
amount* (обязательное) (число)		Сумма платежа.
description* (обязательное) (строка)	Описание платежа.
currency* (обязательное) (число)		643 - Российский рубль.
email* (обязательное) (строка)		Электронная почта.
phone* (обязательное) (строка)		Номер телефона.

Пример кода:

<?php

$token = 'ВАШ ТОКЕН';
$orderNumber = ‘Ваш ID - ххх’;
$amount = сумма;
$description = ‘description’;
$currency = 643;
$email = ‘example@example.com’;
$phone = ‘+799999999’;’

$data = [
	‘orderNumber’	=> $orderNumber,
	'amount' 	=> $amount,
‘description’	=> $description,
‘currency’ 	=> $currency,;
‘email’		=> $email,
‘phone’ 	=> $phone,
];

$headers = [ 
"Content-Type: application/json",
 "API-TOKEN: $token",
];

$ch = curl_init('https://pay.jhpay.online/api/pay/order/create');
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($data));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
curl_setopt($ch, CURLOPT_HEADER, $headers);
$result = json_decode(curl_exec($ch));
curl_close($ch);

header('Location: ' . $result->formUrl); // Редирект на страницу оплаты

?>


Результатом вернет orderId и formUrl в формате JSON.
Где orderId - номер операции в нашей системе.
Где formUrl - ссылка на оплату.

В случае ошибки вернет error и message.
Где error - код ошибки.
Где message - сообщение ошибки.


-------------------FEEDBACK ПЛАТЕЖА-----------------------


При успешном проведении платежа, на ссылку, указанную в настройках (URL оповещения) будет отправлен POST-запрос со следующими JSON полями:

id					Номер операции в нашей системе.
order_id			Уникальный номер платежа в вашей системе.
amount				Сумма платежа.
data				Объект, передаваемый на сервер вместе с уведомлением об успешном платеже.
createdDateTime		Время создания платежа.
status				Статус платежа - PAID если платёж прошёл успешно, WAIT если платёж ещё не прошел.	



PHP КОД ПРОВЕРКИ ПОДПИСИ: 

<?php

$token = 'ВАШ ТОКЕН';

$sign = $_SERVER['HTTP_SIGNATURE'];
$sign2 = hash_hmac('sha256', $_POST['id'] . '|' . $_POST['createdDateTime'] . '|' . $_POST['amount'], $token);

if($sign == $sign2) {
	// Код в случае успешной оплаты
} else {
	echo 'ERROR SIGN';
}

?>





![image](https://github.com/user-attachments/assets/4605515e-7706-45c6-867e-d736f2642444)
