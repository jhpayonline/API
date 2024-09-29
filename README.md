@extends(activeTemplate().'layouts.master')
@section('title',"$page_title")
@section('content')

    <!--breadcrumb area-->
    <section class="breadcrumb-area fixed-head blue-bg">
        <div class="container">
            <div class="row ">
                <div class="col-xl-8 col-lg-8 col-md-12 col-sm-12 centered">
                    <div class="banner-title">
                        <h2>{{__($page_title)}}</h2>
                    </div>
                    <ul class="bread-crumb">
                        <li><a href="{{route('home')}}">@lang('Home')</a></li>
                        <li>{{__($page_title)}}</li>
                    </ul>
                </div>
            </div>
        </div>
    </section><!--/breadcrumb area-->
   {{-- <section class="section-padding">
        <div class="container">


            <div class="row">

                <div class="col-xl-12 col-lg-12 col-md-12 col-sm-12">
                      <h2>{{__($page_title)}} @lang("For individuals")</h2>
                    <h2 class="section-title">Initiate API Payment</h2>

                    <strong>Endpoint: </strong> <span style="color:#4931a8">{{route('express.initiate')}}</span> <br>
                    <strong>Method: </strong> <span style="color:#4931a8">GET</span>

                    <div class="row">
                        <div class="col-md-7 mt-5">


                            <p style="font-size: 20px;">Just request to that endpoint with all parameter listed below: </p>
                            <div class="table-responsive">
                                <table class="table table-striped">
                                    <thead class="thead-dark">
                                    <tr>
                                        <th scope="col">Parameter</th>
                                        <th scope="col">Details</th>
                                    </tr>
                                    </thead>
                                    <tbody>

                                    <tr class="">
                                        <td class="weight--medium">amount</td>
                                        <td>Your Amount , Must be rounded at 2 precision.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">currency</td>
                                        <td>Currency Code, Must be in Upper Case.<br> Supported Currency : @foreach($currency as $cur) <code> {{$cur->code}} </code> @endforeach
                                            <div class="pm-markdown docs-request-table__desc"></div>
                                        </td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">details</td>
                                        <td>Details of the Transaction. <br> <code>string</code> not more than 100 char.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">custom</td>
                                        <td>Custom Code/Transaction Number for Identify at your end. <br> <code>string</code> not more than 16 char.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">ipn_url</td>
                                        <td>URL of your IPN Listener</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">success_url</td>
                                        <td>Redirect url after successful Payment</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">cancel_url</td>
                                        <td>Redirect url after Cancel the Payment</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">public_key</td>
                                        <td>Your API Public Key</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">name</td>
                                        <td>Name of the Client. <br> <code>string</code> not more than 100 char.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">email</td>
                                        <td>Email of the Client. <br> <code>string</code> not more than 100 char.</td>
                                    </tr>

                                    </tbody>
                                </table>
                                <code>** All Parameters are required!</code>
                            </div>

                        </div>

                        <div class="col-md-5 mt-5 text-white" style="background: #303030;">
                            <h5 class="my-3">Example PHP Code to Request</h5>


                            <pre style="color: #f1c40f;">
<span style="color:#bbb;">// create array With Parameters</span>
$parameters = array(
    'amount' => '10.33',
    'currency' => 'RUB',
    'details' => 'Purchase Software',
    'custom' => 'ABCD1234',
    'ipn_url' => 'http://www.abc.com/ipn.php',
    'success_url' => 'http://www.abc.com/success.php',
    'cancel_url' => 'http://www.abc.com/cancelled.php',
    'public_key' => 'ABCDEFGH123456789',
    'name' => 'Mr. Salim Aliev',
    'email' => 'abc@abc.com'
);

<span style="color:#bbb;">// Generate The Url</span>
$endpoint = '{{route('express.initiate')}}';
$call = $endpoint . "?" . http_build_query($parameters);

<span style="color:#bbb;">// Send Request</span>
$ch = curl_init();
curl_setopt($ch, CURLOPT_AUTOREFERER, TRUE);
curl_setopt($ch, CURLOPT_HEADER, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_URL, $add);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, TRUE);
$response = curl_exec($ch);
curl_close($ch);

<span style="color:#bbb;">// $response contain the response from server</span>

                            </pre>


                            <h5 class="my-3">Response (Error)</h5>


                            <pre style="color: #eb4d4b;">
{
    "error": "error",
    "message": {
        "amount": [
            "The amount format is invalid."
        ],
        "details": [
            "The details field is required."
        ]
    }
}

                            </pre>



                            <h5 class="my-3">Response (Success)</h5>


                            <pre style="color: #2ecc71;">

{
    "error": "ok",
    "message": "Payment Initiated. Redirect to url",
    "url": "{{route('express.payment',['UNIQUE_CODE'])}}"
}

                            </pre>




                        </div>


                    </div>

                </div>
            </div>


            <div class="row mt-5">

                <div class="col-xl-12 col-lg-12 col-md-12 col-sm-12">
                    <h2 class="section-title">Get Payment</h2>

                    <p style="font-size: 20px;">After Successful response from last step, Redirect the User to the URL you get as response. User can transact over our system and we will notify on your IPN after Successful payment. Remember, we send response to IPN only once per successful Transaction. </p>

                </div>
            </div>


            <div class="row mt-5">

                <div class="col-xl-12 col-lg-12 col-md-12 col-sm-12">
                    <h2 class="section-title">IPN and Validate The Payment</h2>

                    <strong>Endpoint: </strong> <span style="color:#4931a8">Your IPN URL</span> <br>
                    <strong>Method: </strong> <span style="color:#4931a8">POST</span>

                    <div class="row">
                        <div class="col-md-7 mt-5">


                            <p style="font-size: 20px;">You will get below parameter on your IPN: </p>
                            <div class="table-responsive">
                                <table class="table table-striped">
                                    <thead class="thead-dark">
                                    <tr>
                                        <th scope="col">Parameter</th>
                                        <th scope="col">Details</th>
                                    </tr>
                                    </thead>
                                    <tbody>

                                    <tr class="">
                                        <td class="weight--medium">amount</td>
                                        <td>Your Requested Amount</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">currency</td>
                                        <td>Currency Code, as You passed on first step.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">custom</td>
                                        <td>Custom String for Identify the payment, as You passed on first step.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">trx_num</td>
                                        <td>Transaction Number on our platform.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">signature</td>
                                        <td>A Hash to Verify the Payment. See code on right side to get more on it.</td>
                                    </tr>


                                    </tbody>
                                </table>
                            </div>

                        </div>

                        <div class="col-md-5 mt-5 text-white" style="background: #303030;">
                            <h5 class="my-3">Example PHP Code to validate The Payment</h5>


                            <pre style="color: #f1c40f;">

$amount = $_POST['amount'];
$currency = $_POST['currency'];
$custom = $_POST['custom'];
$trx_num = $_POST['trx_num'];
$sentSign = $_POST['signature'];
<span style="color:#bbb;white-space: pre-wrap;">// with the 'custom' you can find your original amount and currency. just cross check yourself. if that check is pass, proceed to next step. </span>


<span style="color:#bbb;">// Generate your signature</span>
$string = $amount.$currency.$custom.$trx_num;
$secret = 'YOUR_SECRET_KEY';
$mySign = strtoupper(hash_hmac('sha256', $string , $secret));

if($sentSign == $mySign){
 <span style="color:#bbb;white-space: pre-wrap;">// if sentSign and your generated signature match, The Payment is verified. Never Share 'SECRET KEY' with anyone else.</span>
}

                            </pre>




                        </div>


                    </div>

                </div>
            </div>



        </div>
    </section> --}}
                 
                  <section class="section-padding">
        <div class="container">
              <table class="table table-striped"> 
                                    <tr>
                                        <th> ----------------</th>
                                        </tr> 
                                        </table> 
            <div class="row">

                <div class="col-xl-12 col-lg-12 col-md-12 col-sm-12">
                      <h2>{{__($page_title)}} @lang("For business")</h2>
                      <p class="mt-4">1) Зарегистрируйтесь на сайте <a href="https://jhpay.online">https://jhpay.online</a></p>

<p>2) Войдите в личный кабинет <a href="https://jhpay.online">https://jhpay.online</a></p>

<p>3) Перейдите в "Мои мерчанты" <a href="https://jhpay.online/shops/index">https://jhpay.online/shops/index</a></p>

<p>4) Создайте мерчант "New shop"</p>

<p>5) Заполните поля, где:<br>
 – Имя - Название проекта<br>
 – Url site - Адрес Вашего сайта (Например: <a href="#">https://example.com</a>)<br>
 – Url result - На этот адрес будет отправлен FEEDBACK после оплаты (Например: <a href="#">https://example.com/callback.php</a>)<br>
 – Url success - Пользователь будет отправлен по этой ссылке после "Успешной оплаты" (Например: <a href="#">https://example.com/success.php</a>)<br>
 – Url fail - Пользователь будет отправлен по этой ссылке в случае "Ошибки оплаты" (Например: <a href="#">https://example.com/fail.php</a>)</p>

<p>6) Создайте магазин. После перейдите в его настройки. Там увидите API токен. Его копируем, он понадобится нам дальше!</p>

<p>"*" - Обязательные параметры</p>

                    <h2 class="section-title mb-3">СОЗДАНИЕ ПЛАТЕЖА</h2>

                    <strong><span style="color:#4931a8">https://pay.jhpay.online/api/pay/order/create</span> (POST запрос)</strong>
                    <!-- <strong>@lang("Endpoint"): </strong> <span style="color:#4931a8">https://pay.jhpay.online/api/pay/order/create</span> <br>
                    <strong>@lang("Method"): </strong> <span style="color:#4931a8">POST</span> -->

                    <div class="row">
                        <div class="col-md-7 mt-5">


                            <!-- <p style="font-size: 20px;">Just request to that endpoint with all parameter listed below: </p> -->
                            <div class="table-responsive">
                                <table class="table table-striped">
                                    <thead class="thead-dark">
                                    <tr>
                                        <th scope="col">Parameter</th>
                                        <th scope="col">Details</th>
                                    </tr>
                                    </thead>
                                    <tbody>

                                    <tr class="">
                                        <td class="weight--medium">orderNumber* (обязательное) (строка)</td>
                                        <td>Уникальный номер платежа в вашей системе.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">amount* (обязательное) (число)</td>
                                        <td>Сумма платежа.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">description* (обязательное) (строка)</td>
                                        <td>Описание платежа.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">currency* (обязательное) (число)</td>
                                        <td>643 - Российский рубль.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">email* (обязательное) (строка)</td>
                                        <td>Электронная почта.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">phone* (обязательное) (строка)</td>
                                        <td>Номер телефона.</td>
                                    </tr>
                                    <!-- <tr class="">
                                        <td class="weight--medium">cancel_url</td>
                                        <td>Redirect url after Cancel the Payment</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">public_key</td>
                                        <td>Your API Public Key</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">name</td>
                                        <td>Name of the Client. <br> <code>string</code> not more than 100 char.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">email</td>
                                        <td>Email of the Client. <br> <code>string</code> not more than 100 char.</td>
                                    </tr> -->

                                    </tbody>
                                </table>
                            </div>

                        </div>

                        <div class="col-md-5 mt-5 text-white" style="background: #303030;">
                            <h5 class="my-3">Пример кода:</h5>


                            <pre style="color: #f1c40f;">
&lt;?php

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

?&gt;
<!-- 
<span style="color:#bbb;">// create array With Parameters</span>
$parameters = array(
    'amount' => '10.33',
    'currency' => 'RUB',
    'details' => 'Purchase Software',
    'custom' => 'ABCD1234',
    'ipn_url' => 'http://www.abc.com/ipn.php',
    'success_url' => 'http://www.abc.com/success.php',
    'cancel_url' => 'http://www.abc.com/cancelled.php',
    'public_key' => 'ABCDEFGH123456789',
    'name' => 'Mr. Salim Aliev',
    'email' => 'abc@abc.com'
);

<span style="color:#bbb;">// Generate The Url</span>
$endpoint = '{{route('express.initiate')}}';
$call = $endpoint . "?" . http_build_query($parameters);

<span style="color:#bbb;">// Send Request</span>
$ch = curl_init();
curl_setopt($ch, CURLOPT_AUTOREFERER, TRUE);
curl_setopt($ch, CURLOPT_HEADER, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_URL, $add);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, TRUE);
$response = curl_exec($ch);
curl_close($ch);

<span style="color:#bbb;">// $response contain the response from server</span>

                            </pre>


                            <h5 class="my-3">Response (Error)</h5>


                            <pre style="color: #eb4d4b;">
{
    "error": "error",
    "message": {
        "amount": [
            "The amount format is invalid."
        ],
        "details": [
            "The details field is required."
        ]
    }
} -->

                            <!-- </pre>



                            <h5 class="my-3">Response (Success)</h5>


                            <pre style="color: #2ecc71;">

{
    "error": "ok",
    "message": "Payment Initiated. Redirect to url",
    "url": "{{route('express.payment',['UNIQUE_CODE'])}}"
} -->

                            </pre>




                        </div>


                    </div>

                </div>
            </div>


            <div class="row mt-5">

                <div class="col-xl-12 col-lg-12 col-md-12 col-sm-12">

                    <p style="font-size: 20px;">Результатом вернет orderId и formUrl в формате JSON.
Где orderId - номер операции в нашей системе.
Где formUrl - ссылка на оплату.
</p>
<p style="font-size: 20px;">
В случае ошибки вернет error и message.
Где error - код ошибки.
Где message - сообщение ошибки.
</p>

                </div>
            </div>


            <div class="row mt-5">

                <div class="col-xl-12 col-lg-12 col-md-12 col-sm-12">
                    <h2 class="section-title">FEEDBACK ПЛАТЕЖА</h2>

                    <!-- <strong>Endpoint: </strong> <span style="color:#4931a8">Your IPN URL</span> <br>
                    <strong>Method: </strong> <span style="color:#4931a8">POST</span> -->

                    <div class="row">
                        <div class="col-md-7 mt-5">


                            <p style="font-size: 20px;">При успешном проведении платежа, на ссылку, указанную в настройках (URL оповещения) будет отправлен POST-запрос со следующими JSON полями:</p>
                            <div class="table-responsive">
                                <table class="table table-striped">
                                    <thead class="thead-dark">
                                    <tr>
                                        <th scope="col">Parameter</th>
                                        <th scope="col">Details</th>
                                    </tr>
                                    </thead>
                                    <tbody>
                                    <tr class="">
                                        <td class="weight--medium">id</td>
                                        <td>Номер операции в нашей системе.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">order_id</td>
                                        <td>Уникальный номер платежа в вашей системе.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">amount</td>
                                        <td>Сумма платежа.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">data</td>
                                        <td>Объект, передаваемый на сервер вместе с уведомлением об успешном платеже.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">createdDateTime</td>
                                        <td>Время создания платежа.</td>
                                    </tr>
                                    <tr class="">
                                        <td class="weight--medium">status</td>
                                        <td>Статус платежа - PAID если платёж прошёл успешно, WAIT если платёж ещё не прошел.</td>
                                    </tr>


                                    </tbody>
                                </table>
                            </div>

                        </div>

                        <div class="col-md-5 mt-5 text-white" style="background: #303030;">
                            <h5 class="my-3">PHP КОД ПРОВЕРКИ ПОДПИСИ: 
                            </h5>


                            <pre style="color: #f1c40f;">
&lt;?php

$token = 'ВАШ ТОКЕН';

$sign = $_SERVER['HTTP_SIGNATURE'];
$sign2 = hash_hmac('sha256', $_POST['id'] . '|' . $_POST['createdDateTime'] . '|' . $_POST['amount'], $token);

if($sign == $sign2) {
	// Код в случае успешной оплаты
} else {
	echo 'ERROR SIGN';
}

?&gt;


<!-- $amount = $_POST['amount'];
$currency = $_POST['currency'];
$custom = $_POST['custom'];
$trx_num = $_POST['trx_num'];
$sentSign = $_POST['signature'];
<span style="color:#bbb;white-space: pre-wrap;">// with the 'custom' you can find your original amount and currency. just cross check yourself. if that check is pass, proceed to next step. </span>


<span style="color:#bbb;">// Generate your signature</span>
$string = $amount.$currency.$custom.$trx_num;
$secret = 'YOUR_SECRET_KEY';
$mySign = strtoupper(hash_hmac('sha256', $string , $secret));

if($sentSign == $mySign){
 <span style="color:#bbb;white-space: pre-wrap;">// if sentSign and your generated signature match, The Payment is verified. Never Share 'SECRET KEY' with anyone else.</span>
} -->

                            </pre>




                        </div>


                    </div>

                </div>
            </div>



        </div>
    </section>
@endsection

@section('style')
    <style>
        pre {
            line-height: 16px;
        }
    </style>
@endsection
