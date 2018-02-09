# kannel ubuntu 16.04

#install kannel 

sudo apt-get update

sudo apt-get install kannel

# редактируем конфиг / update config

sudo gedit /etc/kannel/kannel.conf

# пример конфига / config example

group = core

admin-port = 13000 #порт для проверки состояний подключений

smsbox-port = 13001 #порт для подклчения смс-боксов

admin-password = 1

log-file = "/var/log/kannel/kannel.log"

log-level = 0

dlr-storage = internal

store-file = "/var/log/kannel/kannel.store"

group = smsbox

bearerbox-host = "127.0.0.1"

sendsms-port = 13003

group = sendsms-user

username = foo

password = bar

concatenation = true

max-messages = 20

#тут настройки оператора смс-рассылок / config from sms operator

group = smsc

smsc = smpp

smsc-id = gw-bulkness

smsc-admin-id = gw-bulkness

allowed-smsc-id = gw-bulkness

host = "smpp05.bulkness.com"

port = 8887

transceiver-mode = 1

smsc-username = "username"

smsc-password = "password" #!!!длина пароля не должна превышать 8 символов!!! password length must be no more than 8 chars

source-addr-autodetect = 1

dest-addr-ton = 1

dest-addr-npi = 1

system-type = ""

enquire-link-interval = 30

log-file = "/var/log/kannel/smsc-bulkness.log" #логи смс-рассылок смотрим здесь

log-level = 0

# добавляем автозагрузку смс-бокса / add sms-box autoload

sudo gedit /etc/default/kannel и расскоментируем строку START_SMSBOX=1 / uncomment string START_SMSBOX=1

# перезапускаем канел / restart kannel

/etc/init.d/kannel stop

/etc/init.d/kannel start

вариант /etc/init.d/kannel restart у меня работал криво 

если не запускается, останавливаем канел и стартуем боксы вручную

sudo /usr/sbin/bearerbox /etc/kannel/kannel.conf

sudo /usr/sbin/smsbox /etc/kannel/kannel.conf


# проверка статуса / check status

http://127.0.0.1:13000/status

# пример отправки сообщения из консоли курлом / send sms example

curl  "http://127.0.0.1:13003/cgi-bin/sendsms?user=foo&pass=bar&text=Hello&to=380505554411&from=Test"
