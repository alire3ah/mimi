# mimi
<?php
define('API_KEY','توکن');
//----######------
function makereq($method,$datas=[]){
    $url = "https://api.telegram.org/bot".API_KEY."/".$method;
    $ch = curl_init();
    curl_setopt($ch,CURLOPT_URL,$url);
    curl_setopt($ch,CURLOPT_RETURNTRANSFER,true);
    curl_setopt($ch,CURLOPT_POSTFIELDS,http_build_query($datas));
    $res = curl_exec($ch);
    if(curl_error($ch)){
        var_dump(curl_error($ch));
    }else{
        return json_decode($res);
    }
}
//##############=--API_RE
//----######------
//---------
$update = json_decode(file_get_contents('php://input'));
var_dump($update);
//=========
$text = $update->message->text;
$message_id = $update->message->message_id;
$from_id = $update->message->from->id;
$username = $update->message->from->username;
mkdir("data/$from_id");
$data = $update->callback_query->data;
$data_id = $update->callback_query->id;
$admin = 1234567;
$tch = "@ViewMembersGramCh";
$mi = $update->callback_query->message->message_id;
$mt = $update->callback_query->message->chat->id;
$from_chat_msg_id = $update->message->forward_from_message_id;
$from_chat_username = $update->message->forward_from_chat->username;
$code = rand(0,9999);
$reply = $update->message->reply_to_message;
$type = $update->message->chat->type;
$ch = file_get_contents("data/$from_id/channeluser.txt");
$post = file_get_contents("data/$from_id/post.txt");
$chat_id = $update->message->chat->id;
$typing = file_get_contents("data/$from_id/typing.txt");
$message_id = $update->message->message_id;
$first_name = $update->message->from->first_name;
$last_name = $update->message->from->last_name;
mkdir("data/username.txt/$username");
$cs = file_get_contents("data/code.txt");
$c = file_get_contents("data/$from_id/c.txt");
$danestin = file_get_contents("http://api.hektor-tm.ir/danestani/");
$fal = file_get_contents("http://api.hektor-tm.ir/sangin/");
$coin = file_get_contents("data/$from_id/coin.txt");
$step= file_get_contents("data/$from_id/step.txt");
$footbal = json_decode(file_get_contents("http://mrphp.000webhostapp.com/iranleague/"));
//-------
function SendMessage($ChatId, $TextMsg)
{
 makereq('sendMessage',[
'chat_id'=>$ChatId,
'text'=>$TextMsg,
'parse_mode'=>"MarkDown"
]);
}
function SendSticker($ChatId, $sticker_ID)
{
 makereq('sendSticker',[
'chat_id'=>$ChatId,
'sticker'=>$sticker_ID
]);
}
function Forward($KojaShe,$AzKoja,$KodomMSG)
{
makereq('ForwardMessage',[
'chat_id'=>$KojaShe,
'from_chat_id'=>$AzKoja,
'message_id'=>$KodomMSG
]);
}
function save($filename,$TXTdata)
	{
	$myfile = fopen($filename, "w") or die("Unable to open file!");
	fwrite($myfile, "$TXTdata");
	fclose($myfile);
	}
//------------
if($text == '/start')
{
	save("data/$from_id/step.txt","none");
	mkdir("data/$from_id");
    makereq('sendMessage',[
        'chat_id'=>$chat_id,
        'text'=>"سلام به ربات ممبرگیر پارسیان خوش آمدید",
	'parse_mode'=>'MarkDown',
        'reply_markup'=>json_encode([
            'keyboard'=>[
                [
                    ['text'=>"🌐حساب کاربری🌐"],['text'=>"💰موجودی سکه💰"]
                ],
                [
                   ['text'=>"📤انتقال موجودی📤"],['text'=>"🙄مشاهده کانال🙄"]
               ],
             [
               ['text'=>"🚫گزارش تخلف🛇"],['text'=>"📣ثبت کانال📣"]
             ],
               [
                 ['text'=>"📌سکه رایگان📍"]
               ]
            ],'resize_keyboard'=>true,
        ])
    ]);
}
else if($text == "🌐حساب کاربری🌐")
if($coin == "0"){
		makereq('sendMessage',[
        'chat_id'=>$chat_id,
        'text'=>"اطلاعات حساب کاربری شما 👇

💰موجودی سکه شما : *0*

👤نام اصلی شما : *$first_name*

💡ایدی شما : @$username
",
'parse_mode'=>"markdown",
]);
}
else
{
	makereq('sendMessage',[
        'chat_id'=>$chat_id,
        'text'=>"اطلاعات حساب کاربری شما 👇

💰موجودی سکه شما : *$coin*

👤نام اصلی شما : *$first_name*

💡ایدی شما : @$username
",
'parse_mode'=>"markdown",
]);
}
else if($text == "📌سکه رایگان📍")
{
	SendMessage($chat_id,"30 ثانیه صبر کن تا سکه رو بگیری ..");
	SendMessage($chat_id,"یادت باشه همیشه میتونی بگیری !!");
	SendMessage($chat_id,"منتظر باش .....");
	sleep(30);
	save("data/$from_id/coin.txt",($coin + 3));
	SendMessage($chat_id,"دریافت کردی 😄🙄😎");
}
else if($text == "💰موجودی سکه💰")
{
	makereq('sendMessage',[
        'chat_id'=>$chat_id,
        'text'=>"تعداد سکه شما : *$coin*",
'parse_mode'=>"markdown",
]);
}
else if($text == "🙄مشاهده کانال🙄")
{
	makereq('sendMessage',[
        'chat_id'=>$chat_id,
        'text'=>"برای مشاهده کانال و دریافت سکه و ثبت بازدید
در کانال زیر عضو شوید 🔥",
'parse_mode'=>"markdown",
'reply_markup'=>json_encode([
            'inline_keyboard'=>[
                [
                    ['text'=>"مشاهده کانال لیست کانال",'url'=>"t.me/ViewMembersGramCh"]
                ],
             ],
        ])
]);
}
else if($text == "📣ثبت کانال📣")
if($coin >= 20)
{
	save("data/$from_id/step.txt","sabt");
	makereq('sendMessage',[
        'chat_id'=>$chat_id,
        'text'=>"بنر خود را ارسال کنید :

هزینه ارسال هر بنر در کانال 10 سکه !!!

فقط ارسال بنر متنی مجاز است !!!",
'reply_markup'=>json_encode([
            'inline_keyboard'=>[
                [
                    ['text'=>"مشاهده کانال لیست کانال",'url'=>"t.me/ViewMembersGramCh"]
                ],
             ],
        ])
]);
}
else
{
	makereq('sendMessage',[
        'chat_id'=>$chat_id,
        'text'=>"سکه شما کافی نیست حداقل 20 سکه نیاز دارید !!!",
'reply_markup'=>json_encode([
            'inline_keyboard'=>[
                [
                    ['text'=>"مشاهده کانال لیست کانال",'url'=>"t.me/ViewMembersGramCh"]
                ],
             ],
        ])
]);
}
 elseif ($text =="/sendAll"  && $chat_id == $admin | $booleans[0]=="false") {
	{
          sendmessage($chat_id,"`لطفا متن خود را ارسال کنید`");
	}
      $boolean = file_get_contents('booleans.txt');
		  $booleans= explode("\n",$boolean);
	  	$addd = file_get_contents('banlist.txt');
	  	$addd = "true";
    	file_put_contents('booleans.txt',$addd);
    	
    }
      elseif($chat_id == $admin && $booleans[0] == "true") {
    $texttoall = $textmessage;
		$ttxtt = file_get_contents('member.txt');
		$membersidd= explode("\n",$ttxtt);
		for($y=0;$y<count($membersidd);$y++){
			sendmessage($membersidd[$y],"$texttoall");
			
		}
		$memcout = count($membersidd)-1;
	 	{
	 	Sendmessage($chat_id,"پیغام شما به $memcout مخاطب ارسال شد.");
	 	}
         $addd = "false";
    	file_put_contents('booleans.txt',$addd);
    	}
    	
    		
    		
    		elseif($text == '/amar' && $chat_id == $admin)
	{
		$txtt = file_get_contents('member.txt');
		$membersidd= explode("\n",$txtt);
		$mmemcount = count($membersidd) -1;
{
sendmessage($chat_id,"لیست افراد ربات : $mmemcount");
}
}
else if($step == "sabt")
{
	save("data/$from_id/coin.txt",($coin - 10));
	save("data/$from_id/step.txt","none");
	makereq('sendMessage',[
        'chat_id'=>"@ViewMembersGramCh",
        'text'=>"$text",
]);
	SendMessage($chat_id,"ثبت شد !!");
}
	$txxt = file_get_contents('member.txt');
$pmembersid= explode("\n",$txxt);
	if (!in_array($chat_id,$pmembersid)) {
		$aaddd = file_get_contents('member.txt');
		$aaddd .= $chat_id."
";
    	file_put_contents('member.txt',$aaddd);
} 


	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
