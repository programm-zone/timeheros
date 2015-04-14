# timeheros
Время Героев

﻿<?
define('PROTECTOR', 1);

$textl='Время Героев - новая онлайн игра';
@include('files/db.php');
@include('files/auth.php');
@include('files/func.php');
@include('files/core.php');
if ($user_id==0){
/////////////////////ГОСТИ/////
include('files/head.php');
@include('files/guest.php');
///
$refer=htmlspecialchars(stripslashes($_SERVER['HTTP_REFERER']));
///////////сохранение рефов, временно отключено /*
$brauser=htmlspecialchars(stripslashes($_SERVER['HTTP_USER_AGENT']));
$braus = strtok($brauser, '/');
$ip=htmlspecialchars(stripslashes($_SERVER['REMOTE_ADDR']));
$refl = mysql_num_rows(mysql_query("SELECT * FROM referer WHERE ip = '$ip' and brauser = '$braus'"));
if($refl<=0){
$den = date("d", time());
$h_ua = str_replace('windows ce', '', strtolower($brauser));
if (
    !$h_ua ||
    strpos($h_ua, 'windows') !== false    ||
    strpos($h_ua, 'linux') !== false    ||
    strpos($h_ua, 'unix') !== false
)
{$agent = "pk";}else{$agent = "mobile";}
if($braus=="Opera"){$braus="Opera $agent";}
if($braus=="Mozilla"){$braus="Mozilla $agent";}
if(!empty($refer)){
mysql_query("INSERT INTO
        `referer` SET `ip` = '$ip', `brauser` = '$braus', `day` = '$den',`ref` = '$refer' ") or die (mysql_error());}
}
/////////// */
$timeout = time() - 180;
$online = mysql_num_rows(mysql_query("SELECT * FROM online WHERE laikas > '$timeout'"));
if($online==0){$online=1;}
$guest = mysql_num_rows(mysql_query("SELECT * FROM guest WHERE laikas > '$timeout'"));
$online=$online+$guest;
$reg = mysql_num_rows(mysql_query("SELECT * FROM users"));
$asd = mysql_query("SELECT time FROM news ORDER BY id DESC LIMIT 1");
$news = mysql_fetch_array($asd);
$ref=htmlspecialchars(stripslashes(addslashes($_GET['ref'])));
switch($_GET[mod]){default:
echo'<div class="heads"><b>«Время Героев»</b></div>';
echo'<div class="headmenu"><center><img src="pic/logod.gif" alt="logo"/>';
echo'<div class="dot">';
echo'<center><font color="#6FCD72"> «Время Героев»</font> - это новая онлайн игра.
В нее можно играть с мобильного телефона или компьютера!</center></div>';
echo"С нами <font color = '#6FCD72'>$reg</font> игроков<br>";
$req=mysql_query("SELECT * FROM domination WHERE id = '1'");
$dom = mysql_fetch_assoc($req);
$my=$dom['white'];
$enemy=$dom['black'];
$all=$my+$enemy;
if($my<$enemy){$lid="Тёмные";}else{$lid="Светлые";}
if($my/$all<$enemy/$all){$domin=round($enemy/$all * 100);}else{$domin=round($my/$all * 100);}
if($domin<='15'){
echo'Лидируют '.$lid.': <img src="pic/main/towerscontrol1.png" alt="d">'.$domin.'%';}elseif($domin<='30'){
echo'Лидируют '.$lid.': <img src="pic/main/towerscontrol2.png" alt="d">'.$domin.'%';}elseif($domin<='45'){
echo'Лидируют '.$lid.': <img src="pic/main/towerscontrol3.png" alt="d">'.$domin.'%';}elseif($domin<='55'){
echo'Баланс сил: <img src="pic/main/towerscontrol4.png" alt="d">'.$domin.'%';}elseif($domin<='70'){
echo'Лидируют '.$lid.': <img src="pic/main/towerscontrol5.png" alt="d">'.$domin.'%';}elseif($domin<='85'){
echo'Лидируют '.$lid.': <img src="pic/main/towerscontrol6.png" alt="d">'.$domin.'%';}elseif($domin<='100'){
echo'Лидируют '.$lid.': <img src="pic/main/towerscontrol7.png" alt="d">'.$domin.'%';}

echo'<br>Светлых: ';
$a = mysql_num_rows(mysql_query("SELECT * FROM users WHERE storona = 'white'"));
$b = $reg-$a;
echo" <b>$a</b><br/>";
echo"Темных: <b>$b</b>";
echo"<br/>Онлайн: <b>$online</b></div></div>";
echo'</div><b>Новости: </b>';
include ('pnews.php');
echo'<div class="f_bg">';
if (isset($_GET['error']))
{ echo"</div><div class=\"log\">Пароль или логин введены неправильно.
Логин надо вводить с учетом регистра (если при регистрации вы ввели ник PlayeR - то ник player не авторизуется).
</br>Если забыли пароль,вы можете <a href=\"index.php?pass\">Восстановить</a> </div><div class=\"menu\">"; }
if(isset($_GET['exit'])){
echo'<font color=#FF6633>Вы успешно покинули игру! Враги не дремлют, а друзья ждут вас в игре! Возвращайтесь поскорей</font><br/><br/>';
}
if(isset($_GET['cookie'])){echo"<div class =\"info\"><big>В вашем браузере отключены Куки (cookie). Чтобы авторизоваться, необходимо их включить!</big></div>";}

if(isset($_GET['pass'])){
if(empty($_POST[email]) and empty($_POST[log])){
echo "<div class='log'>Внимание! вам будет создан новый пароль.</br>
<form action='index.php?pass' method='POST'>";
echo"<b>Логин:</b><br/>
<input class=\"input\" type=\"text\" value=\"\" size=\"25\" name=\"log\"/><br/>";
echo "<b>E-mail или Ключевое слово:</b><br/>
<input class=\"input\" type=\"text\" value=\"\" size=\"25\" name=\"email\"/><br/>";
echo '<input class="button" type="submit" value="Восстановить" /></form></div>';}else{
$lost = htmlspecialchars(stripslashes(addslashes($_POST['log'])));
$email = htmlspecialchars(stripslashes(addslashes($_POST['email'])));
$npass = rand(1000,10000);
$mnpass = md5($npass);
$rep = mysql_query("SELECT * FROM `users` WHERE `usr` = '$lost' and `email`='$email'");
$avtol=mysql_num_rows($rep);
if ($avtol==1){mysql_query("UPDATE `users` SET
`pass` =  '$mnpass' WHERE `usr` = '$lost' ");
        $title = "Восстановление пароля Время Героев";
        $mess =  "Пароль успешно восстановлен.<br> Ваш логин: $lost<br> Ваш новый пароль: $npass<br> 
        Для входа на сайт используйте ссылку:<br>
        <a href=\"http://timeheros.7gin.ru/enter.php?login=$lost&pas=$npass\">/enter.php?login=$lost&pas=$npass</a>";
        // $to - кому отправляем 
        $to = "$email"; 
        // $from - от кого 
        $from='recowery@timeheros.7gin.ru'; 
        // функция, которая отправляет наше письмо. 
        mail($to, $title, $mess, 'From:'.$from. "\r\n" . "MIME-Version: 1.0\r\nContent-type: text/html; charset=utf-8"); 
echo"<div class =\"log\">Новый пароль успешно отправлен на ваш email.</div>";}
else{echo"<div class =\"log\">Неверный E-mail адрес или ключевое слово <b>$lost $email</b></div>";}}
}

if(isset($_GET['vxod'])){
echo'<form method="post" action="enter.php">';
echo "<b>Логин:</b><br/>";
echo "<input name=\"log\" type=\"text\" maxlength=\"20\" title=\"nick\" emptyok=\"true\"/><br/>";
echo "<b>Пароль:</b><br/>";
echo "<input name=\"pas\" type=\"password\" maxlength=\"20\" title=\"password\" emptyok=\"true\"/><br/>";
echo'<input name="mem" type="checkbox" value="1" /> Запомнить меня<br />';
echo '<input class="button" type="submit" value="Войти"/></form>';}else{
echo"<div class='menuList'>";
echo "<li><a href=\"index.php?vxod&refer=$refer\"><img src='pic/game/onl.png' alt='*'/> Войти</a></li>";
echo "<li><a href=\"reg.php?ref=$ref&refer=$refer\"><img src='pic/game/reg.png' alt='*'/> Регистрация</a></li></span>";}
if(isset($_GET['vxod'])){echo"<div class='menuList'>";}
//echo "<li><a href=\"partner.php?ref=$ref&refer=$refer\"><img src='pic/main/5.png' alt='*'/> Партнёрская программа</a></li>";
echo "<li><a href=\"index.php?mod=about&ref=$ref&refer=$refer\"><img src='pic/main/6.png' alt='*'/> Об игре</a></li>";
echo "<li><a href=\"stat.php?ref=$ref&refer=$refer\"><img src='pic/main/194.gif' alt='*'/> Статистика</a></li>";
break;

case 'reklama':
echo"<div class='menu'>Если вы администратор какого нибудь сайта, вы можете продать нам рекламу со своего сайта, или купить рекламу у нас!</br>Все предложения будут рассмотрены,</br>
Обратитесь к администрации на ник Admin</br>
<a href='index.php?ref=$ref&refer=$refer'>Назад</a></div>";
break;

case 'site':
echo'<div class="menu">';
echo'У вас есть возможность установить на своём сайте нашу игру!<br/>
На все страницах игры будет стоять ссылка "Главная" ведущая на ваш сайт</br> И у ваших пользователей будет возможность играть в онлайн игру<br/>
Сделайте ссылку вида:<br/>
<b>?site=ваш_сайт</b><br/>
Где ваш_сайт адрес вашего сайта без http://<br/>
Скопируйте ссылку:<br/>
<input value="?site=ваш_сайт" name="ssilka" /><br/></div><div class="f_bg">';
echo "<a href='index.php?ref=$ref&refer=$refer'>Назад</a></div>";
break;

case 'about':
echo'<div class="menu">';
echo "Время Героев - это игра в которой вы окунётесь в средневековый мир полный опасностей и приклёчений,<br/>
создавайте своего героя и крушите толпы монстров на пути к победе!<br/>
Станьте лидером клана и приведите его к славе! Либо станьте великим торговцем!<br/>
В этой игре вы можете завести питомца, который будет верным помощником в бою,<br/>
также Вы найдёте здесь онлайн бои и квесты.
<br/> Устраивайте засады на других игроков и победивший заберет все деньги,<br/>
сражайтесь на арене, используйте ауры,свитки, магию,приёмы.<br/>
Захватите со своим кланом город и получайте за это процент от покупаемых вещей в городе !<br/>
<b>Удачи Герой!</b></div><div class='f_bg'>
<a href='index.php?ref=$ref&refer=$refer'>Назад</a></div>";
break;
}@include('files/end.php');
}else{

if($_GET[mod]=='exit'){
$header=TRUE;}
include('files/head.php');
@include('files/zag.php');

switch($_GET[mod]){
default:
//////////
$taim = 180;
$date = time();
$timeout = $date - $taim;
$online = mysql_num_rows(mysql_query("SELECT * FROM online WHERE laikas > '$timeout'"));
$guest = mysql_num_rows(mysql_query("SELECT * FROM guest WHERE laikas > '$timeout'"));
$online=$online+$guest;
////////////////
$q = mysql_query("SELECT COUNT(*) FROM `msg_r` WHERE `user_to` = '$log' AND `read` = '1';");
$new_mail = mysql_result($q, 0);
$w = mysql_query("SELECT COUNT(*) FROM `msg_r` WHERE `user_to` = '$log';");
$old_mail = mysql_result($w, 0);
//////////////
$asd = mysql_query("SELECT time FROM news ORDER BY id DESC LIMIT 1");
$news = mysql_fetch_array($asd);
$req = mysql_query("SELECT * FROM `invite` WHERE `usr` = '$log'");
$inv=mysql_num_rows($req);
///////////////////
$chat= mysql_num_rows(mysql_query("SELECT * FROM komentarai"));
$ft=mysql_result(mysql_query("SELECT count(*) FROM `forum_topics`"), 0); // кол-о тем в форуме
$fm=mysql_result(mysql_query("SELECT count(*) FROM `forum_msg`"), 0); // кол-о сообщений в форуме
$nfm= mysql_num_rows(mysql_query("SELECT * FROM `forum_msg` WHERE `time` > '$date'-'86000'"));
//////////////


//Модификация главной страницы
if(isset($_GET[cookie])){
$brauser=htmlspecialchars(stripslashes($_SERVER['HTTP_USER_AGENT']));
$ip=htmlspecialchars(stripslashes($_SERVER['REMOTE_ADDR']));
echo"<div class=\"a\">Авторизация прошла Успешно <img src=\"pic/yes.png\" alt=\"*\"/></div>Ваш браузер:  <font color='gray'>$brauser</font> <br>
Ваш IP: <font color='gray'>$ip</font> <br>
Чтобы каждый раз не вводить логин и пароль, добавьте в закладки ссылку на автологин<br>
/enter.php?login=$log&pas=$_SESSION[pass]<br>Скопировать:<br>";
echo'<input class="input" type="text" value="http://timeheros.7gin.ru/enter.php?login='.$log.'&pas='.$_SESSION['pass'].'" name="ssilka"/><br/></div><div class="f_bg">';
echo "<div class=\"a\"><a href='index.php?'>На главную</a></div></div><div class=\"f_bg\">";
}else{
$onlines = mysql_num_rows(mysql_query("SELECT * FROM mesto WHERE usr = '$log'"));
if($onlines ==0 ){mysql_query("INSERT INTO `mesto` SET `usr` = '$log',`place` = 'gorod', `city` = '0' ");}
if($udata[prava]>=4){
echo'<div class="menuList"><li><a href="adm_panel.php?"><img src="pic/main/status/madm.png"/> Админ-панель</a></li></div></div><div class="menu">';}
if($udata[prava]==3){echo'<div class="menuList"><li><a href="mod_panel.php?"><img src="pic/main/status/mmod3.png"/> Модер-панель</a></li></div></div><div class="menu">';}
if($udata[prava]==2){
echo'<img src="pic/main/status/mmod.png"/><a href="mod_panel.php?">Модер-панель</a></div><div class="menu">';}
echo'<center><img src="pic/game/city.jpg" alt="city"/> </center>';
echo"<b><center>Приключения</b></center>$div";
echo'<div class="menuList">';
echo'<li><a href="dost.php?"> <img src="pic/game/dost.png" alt="*"/> Достижения</a></li>';
echo'<li><a href="bitva.php?"> <img src="pic/main/zver.png" alt="*"/> Охота</a></li>'; 
echo'<li><a href="boss.php?"> <img src="pic/game/boss.png" alt="*"/> Боссы</a></li> ';
echo'<li><a href="duel.php?"> <img src="pic/main/duel.png" alt="*"/> Дуэли</a></li> ';
echo'<li><a href="towers.php?mod=outtower"> <img src="pic/main/def.png" alt="*"/> Башни'; domin(); echo'</a>';
echo'</li><li><a href="arena.php?"> <img src="pic/main/red_warrior.png" alt="*"/> Арена</a></li>';
echo'<li><a href="olimp.php?"><img src="pic/game/olimp.png" alt="*"/> Турнир</a></li>';
echo'<li><a href="index.php?mod=city"> <img src="pic/main/city.gif" alt="*"/> Посёлок</a></li>';
echo'<li><a href="okrestnosti.php?"> <img src="pic/game/karta.png" alt="*"/> Окрестности Города</a></li>'; 
if($udata[lvl]<=5){echo'<li><a href="istok.php?"> <img src="pic/main/eye.png" alt="*"/> Источник Жизни</a></li>';}

/// хеппи енд приключений
echo"</div>$div<center> <b>Торговля</b> </center>$div";
echo"<div class=\"menuList\">";
echo'<li><a href="index.php?mod=magaz"><img src="pic/main/guilds.png" alt="*"/> Магазин</a></li>';
echo'<li><a href="auction.php?"><img src="pic/main/bank.png" alt="*"/> Аукцион</a></li>';
echo'<li><a href="almaz.php?"><img src="pic/almaz.png" alt="*"/> Лавка Драгоценностей</a></li>';
echo'<li><a href="pay.php?"><img src="pic/main/auction.png" alt="*"/> <b>Покупка Алмазов <font color="#71cc71"> </font></b></a></li>';
/// хеппи енд торговли
echo'</div></div><div class="menu"><center><b>Общение</b><br/></center></div><div class="menu">';
echo"<div class=\"menuList\">";
echo'<li><a href="posnews.php"><img src="pic/main/truba.gif" alt="*"/> <b>Новости</b> [<font color="99CC99">'.$news[time].'</font>]</a></li>';
echo"<li><a href=\"forum\"><img src='pic/main/main.gif' alt='*'/> Форум (<font color=\"99CC99\">$ft/$fm</font>+<font color='#f1d99a'>$nfm</font>)</a></li> " ;
echo "<li><a href=\"chat.php?\"><img src='pic/main/pero.gif' alt='*'/> Общий чат (<font color=\"99CC99\">$chat</font>)</a></li>" ;
echo "<li><a href=\"online.php?\"><img src='pic/main/online.gif' alt='*'/> Игроки онлайн (<font color=\"99CC99\">$online</font>)</a></li> " ;
echo'<li><a href="rating.php?"><img src="pic/2.png" alt="*"/> Зал Славы</a></li>
</div>';

$set = mysql_fetch_array(mysql_query("SELECT * FROM `set` WHERE `usr` = '$log'"));
if(empty($set[name])){echo"<font color='grey'></br>Примечание: У вас не заполнена анкета</br>
<a href=\"anketa.php?go=zap\">Заполнить</a></font>";}
echo'</div></div>';}
break;

case 'domin':
$req=mysql_query("SELECT * FROM domination WHERE id = '1'");
$dom = mysql_fetch_assoc($req);
echo'Доминация';echo domin();echo'<br/>';
echo'<img src="pic/main/white.png" alt="*"/>Светлые: '.$dom['white'].' очков<br/>';
echo'<img src="pic/main/black.png" alt="*"/>Тёмные: '.$dom['black'].' очков<br/>';
echo"<a href=\"index.php?\">Назад</a>";
break;

case 'city':
/////kontrol
$req = mysql_query("SELECT * FROM `zamok` WHERE `city` = '$udata[city]' LIMIT 1");
$avto=mysql_num_rows($req);
if($avto==1){
$city = mysql_fetch_array($req);
if($city[clan]!='not'){
echo'<center><div>Контроль: ';
$req = mysql_query("SELECT * FROM `clan` WHERE `lider`='$city[clan]'");
////////////////////////////
$clan = mysql_fetch_array($req);
if($clan[name] == NULL) { echo'<b>нет лидера</b>'; } 
else{
echo"<a href=\"clan.php?id=$clan[id]\"><font color ='grey'><b>$clan[name]</b></font></a>";}
echo'</div></center>';
}
}
/////
echo'<center><img src="pic/game/cp.jpg" alt="city"/> </center>';
echo"<div class='a'><b>Посёлок</b></div><div class='menuList'>";
echo'<li><a href="start.php?"> <img src="pic/main/beer.png" alt="*"/> Таверна "Уют"</a></li>';
echo'<li><a href="asshome.php?"> <img src="pic/main/sword.png" alt="*"/> Гильдия Наёмников</a></li>';
echo'<li><a href="taverna.php?"> <img src="pic/main/beer.png" alt="*"/> Таверна "Пьяный Герой"</a></li>';
echo'<li><a href="akademmag.php?"><img src="pic/main/enchant.png" alt="*"/> Академия Магов</a></li>';
echo'<li><a href="kuznec.php?"> <img src="pic/main/kuznec.png" alt="*"/> Кузница</a></li>';
echo'<li><a href="pitomec.php?"> <img src="pic/main/pit.gif" alt="*"/> Питомник</a></li>';
echo'<li><a href="rydnik.php?"> <img src="pic/main/rud.png" alt="*"/> Рудник</a></li>';
echo'<li><a href="home.php?"> <img src="pic/main/flags.png" alt="*"/> Кланы</a></li>'; 
echo'<li><a href="fortuna.php?"><img src="pic/game/fart.png" alt="*"/> Колесо Фортуны</a></li>';
echo'<li><a href="olimp.php?"><img src="pic/game/olimp.png" alt="*"/> Турнир</a></li>';
//echo'<li><a href="zamok.php?"><img src="pic/main/def.png" alt="*"/> Замок</a></li>';
echo"</div>";
break;

case 'magaz':
echo"<div class='a'><b>Магазин</b></div><div class='menuList'>";
echo'<li><a href="weapon.php?"><img src="pic/main/guilds.png" alt="*"/> Магазин снаряжения</a></li>';
echo"<li><a href=\"trenirovka.php?\"><img src=\"pic/game/umen.png\" alt=\"*\"/> Тренировка умений</a></li>";
echo'<li><a href="alhimiya.php?"><img src="pic/main/alh.gif" alt="*"/> Лавка алхимии</a></li>';
echo'<li><a href="shopaura.php?"><img src="pic/main/aura.png" alt="*"/> Магазин Аур</a></li>';
echo'<li><a href="lambard.php?"><img src="pic/main/lombard.png" alt="*"/> Ломбард</a></li>';
echo"</div>";
break;

case 'prim':
if (isset($_GET['close'])){mysql_query("UPDATE `set` SET `prim` = 'off' WHERE `usr` = '$log'");
echo'Объвление скрыто</br><a href="javascript:history.go(-2)">Назад</a>';
}else{
$pri = mysql_query("SELECT * FROM `zametka` WHERE `id` = '1' and `status`='on'");
$prim = mysql_num_rows($pri);
$prime = mysql_fetch_array($pri);
if($prim > 0 and $set[prim]==on){
echo'<div class="log">';
echo"<font size='2'>Объявление:</br>$prime[name]</br>Опубликовал: $prime[log]</font><a href=\"index.php?mod=prim&close\"><font color='#7e7d76'> скрыть...</font></a>";
echo'</div> <a href="javascript:history.go(-1)">Назад</a>';}else{echo'Нет объявлений</br><a href="javascript:history.go(-1)">Назад</a>';}}
break;

case 'ref':
$req = mysql_query("SELECT * FROM `users` WHERE `ref` = '$udata[id]'");
$avto=mysql_num_rows($req);
echo'У вас есть возможность пригласить своих друзей и получить за это вознаграждение!<br/>
Когда ваш друг достигает 3 уровня вы получаете 350 монет и 700 опыта! И это за одного игрока!<br/>';
echo"<b><font color='#f1d99a'>Ваша ссылка для приглашений: <a href='http://herogame.pp.ua/reg.php?ref=$udata[id]'>http://herogame.pp.ua/reg.php?ref=$udata[id]</a></b></font><br/>
Вы можете разместить ссылку где угодно, на форуме, чате, на своем сайте,<br/>
Но нельзя размещать там, где запрещена реклама ссылок.<br/>
Так же запрещено давать ложную информацию об игре, чтобы привлеч друзей.<br/>
Но все зарегистрированные по этой ссылке пользователи, станут вашими друзьями<br/>
</div><div class='menu'>
Ваших друзей ($avto)</div><div class='menu'>";
$req = mysql_query("SELECT * FROM `users` WHERE `ref` = '$udata[id]'");
$avto=mysql_num_rows($req);
if($avto>=1){
While($usdata = mysql_fetch_array($req)){
echo"<a href='search.php?nick=$usdata[usr]&go=go'>$usdata[usr]</a> [<a href=\"index.php?mod=delref&amp;usr=$usdata[usr]\">x</a>]<br/>";}}else{
echo'<b>Нет друзей :( </b><br/>';}
echo"</div><div class='menu'><a href='index.php'>Назад</a></div>";
break;

case 'delref':
if(empty($_GET[usr])){
echo'Не выбран игрок!<br/>';
include('files/down.php');exit;}
$req = mysql_query("SELECT * FROM `users` WHERE `usr` = '$_GET[usr]' LIMIT 1");
$avto = mysql_num_rows($req);
if($avto == 1){$claner = mysql_fetch_array($req);
mysql_query("UPDATE `users` SET `ref` = 'not' WHERE `usr` = '$_GET[usr]'");
echo"$_GET[usr] исключён из друзей!<br/>";}else{
echo'Нет такого игрока в вашем клане!';
@include('files/down.php');exit;}
echo"<a href=\"index.php?mod=ref\">Назад</a>";
break;

case 'exit':
setcookie('log', '');
setcookie('pas', '');
session_destroy();
header ('Location: index.php?exit');exit;

break;
}
if($user_id==1 and isset($_GET[cookie])){}else{
@include('files/down.php');}
}
?>
