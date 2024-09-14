# AntiZapret-VPN (версия без контейнера)

Скрипт для автоматического развертывания AntiZapret VPN + обычный VPN

Через AntiZapret VPN работают только:
- Заблокированные сайты из единого реестра РФ, список автоматически обновляется раз в 6 часов
- Сайты к которым ограничивается доступ без судебного решения (например youtube.com)
- Сайты ограничивающие доступ из России (например intel.com, chatgpt.com)

Список сайтов для AntiZapret VPN предзаполнен (include-hosts-dist.txt)\
Доступно ручное добавление своих сайтов (include-hosts-custom.txt)

Все остальные сайты работают через вашего провайдера с максимальной доступной вам скоростью

**Внимание!** Для правильной работы AntiZapret VPN нужно [отключить DNS в браузере](https://www.google.ru/search?q=отключить+DNS+в+браузере)

Через обычный VPN работают все сайты, доступные с вашего сервера

Ваш сервер должен находится за перелелами России, в противном случае разблокировка сайтов не гарантируется

AntiZapret VPN (antizapret-\*.ovpn) и обычный VPN (vpn-\*.ovpn) работают через [OpenVPN Connect](https://openvpn.net/client)\
Поддерживается подключение по UDP или TCP, или только по UDP (\*-udp.ovpn) или только по TCP (\*-tcp.ovpn)\
Используются порты 50080 и 50443, и резервные порты 80 и 443 для обхода блокировок по портам

OpenVPN позволяет нескольким клиентам использовать один и тот же \*.ovpn для подключения к серверу

При подключении используется AdGuard DNS для блокировки рекламы, отслеживающих модулей и фишинга

За основу взяты [эти исходники](https://bitbucket.org/anticensority/antizapret-vpn-container/src/master) разработанные ValdikSS

Протестировано на Ubuntu 22.04/24.04 и Debian 11*/12 - Процессор: 1 core Память: 1 Gb Хранилище: 10 Gb\
\* Debian 11 требует установки обновления dnslib
***
### Установка:
1. Устанавливать на чистую Ubuntu 22.04/24.04 или Debian 11*/12 (рекомендуется Ubuntu 24.04)
2. В терминале под root выполнить
```sh
apt-get update && apt-get install -y git && git clone https://github.com/GubernievS/AntiZapret-VPN.git antizapret-vpn && chmod +x antizapret-vpn/setup.sh && antizapret-vpn/setup.sh
```
3. Дождаться перезагрузки сервера и скопировать файлы *.ovpn с сервера из папки /root

Опционально можно:
1. Установить обновления Knot Resolver, dnslib, OpenVPN и DCO
2. Поставить патч для обхода блокировки протокола OpenVPN
3. Включить DCO
4. Добавить клиентов
***
Установить обновления Knot Resolver, dnslib, OpenVPN и DCO (если был установлен ранее)
```sh
./upgrade.sh
```
***
Поставить патч для обхода блокировки протокола OpenVPN (работает только для UDP соединений)
```sh
./patch-openvpn.sh
```
***
Если у вас Ubuntu 24.04 или Debian 12, или вы установили обновление OpenVPN, то можете включить модуль [DCO](https://community.openvpn.net/openvpn/wiki/DataChannelOffload), он заметно снижает нагрузку на CPU сервера и клиента - это экономит аккумулятор мобильных устройств и увеличивает скорость передачи данных через VPN\
Включить DCO
```sh
./enable-openvpn-dco.sh
```
Выключить DCO
```sh
./disable-openvpn-dco.sh
```
***
Добавить нового клиента
```sh
./add-client.sh [имя_клиента]
```
Удалить клиента
```sh
./delete-client.sh [имя_клиента]
```
После добавления нового клиента скопируйте новые файлы \*.ovpn с сервера из папки /root
***
Добавить свои сайты в список антизапрета (include-hosts-custom.txt)
```sh
nano /root/antizapret/config/include-hosts-custom.txt
```
Обновить список антизапрета
```sh
/root/antizapret/doall.sh
```
***
Обсуждение скрипта на [ntc.party](https://ntc.party/t/9270)
***
Инструкция по настройке на роутерах [Keenetic](./Keenetic.md) и [TP-Link](./TP-Link.md)
***
Хостинги в Европе для VPN принимающие рубли: [vdsina.com](https://www.vdsina.com/?partner=9br77jaat2) с бонусом 10% и [aeza.net](https://aeza.net/?ref=529527) с бонусом 15% (если пополнение сделать в течении 24 часов с момента регистрации)
