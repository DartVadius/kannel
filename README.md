# kannel ubuntu 16.04

#install kannel 

sudo apt-get update

sudo apt-get install kannel

# редактируем конфиг / update config

sudo gedit /etc/kannel/kannel.conf

# пример конфига / config example


group = core  
admin-port = 13000  
smsbox-port = 13001  
admin-password = 1234  
log-file = "/var/log/kannel/kannel.log"  
log-level = 0  
dlr-storage = internal  
store-type = file  
store-location = "/var/log/kannel/kannel.store"  

group = smsbox  
bearerbox-host = "127.0.0.1"  
sendsms-port = 13003  

group = sendsms-user  
username = foo  
password = bar  
concatenation = true  
max-messages = 20  
default-smsc = gw-bulkness  

group = smsc  
smsc = smpp  
smsc-id = gw-bulkness  
smsc-admin-id = gw-bulkness  
allowed-smsc-id = gw-bulkness  
host = "smpp05.bulkness.com"  
port = 8887  
transceiver-mode = 1  
smsc-username = "login"  
smsc-password = "password"  
source-addr-autodetect = 1  
dest-addr-ton = 1  
dest-addr-npi = 1  
system-type = ""  
enquire-link-interval = 200  
log-file = "/var/log/kannel/smsc-bulkness.log"  
log-level = 0  


# конфигурация ядра (bearerbox)

group = core

admin-port = 13000 #порт для проверки состояний подключений и администрирования, рекомендовано > 1023 

smsbox-port = 13001 #порт для подклчения смс-боксов

admin-password = 1234 #админ пароль для администрирования через http интерфейс

log-file = "/var/log/kannel/kannel.log" #путь к лог файлу 

log-level = 0 #уровень логирования 0 'debug', 1 'info', 2 'warning, 3 'error' and 4 'panic' 

dlr-storage = internal #система хранения сообщений (delivery report, DLR), поддерживает internal, mysql, pgsql, sdb, mssql, oracle

store-type = file # система хранения сообщений перед обработкой, file - сообщения пишутся в один файл, spool - сообщения пишутся в отдельные файлы, она запись - один файл

store-location = "/var/log/kannel/kannel.store" # путь к логам, в зависимости от выбранного типа

# конфигурация smsbox для отправки сообщений по протоколу http

group = smsbox

bearerbox-host = "127.0.0.1"

sendsms-port = 13003 # порт для обработки запросов на отрпавку смс

group = sendsms-user

username = foo

password = bar

concatenation = true # сбор большого сообщения из частей, если false части сообщения отдаются как отдельные sms

max-messages = 20 # максимум сообщений, отрпавляемых при разбивке одного длинного сообщения на части

default-smsc = gw-bulkness # дефолтный провайдер, если в запросе не указан идентификатор SMSC

#тут настройки смс центра smsbox от оператора смс-рассылок / config from sms operator

# transceiver mode

group = smsc

smsc = smpp # тип смс центра

smsc-id = gw-bulkness

smsc-admin-id = gw-bulkness

allowed-smsc-id = gw-bulkness

host = "smpp05.bulkness.com"

port = 8887

transceiver-mode = 1 # включение/отключение трансресиверного типа подключения

smsc-username = "username"

smsc-password = "password" #!!!длина пароля не должна превышать 8 символов!!! password length must be no more than 8 chars

source-addr-autodetect = 1 # пытается проверить исходный адрес и соответственно установить настройки TON и NPI

dest-addr-ton = 1 # подключение ton адреса назначения для ссылки ton - номер без префикса??

dest-addr-npi = 1 # подключение npi адреса назначения для ссылки npi - National Provider Identifier

system-type = ""

enquire-link-interval = 30 # интервал проверки на активный сеанс 

log-file = "/var/log/kannel/smsc-bulkness.log" #логи смс-рассылок смотрим здесь

log-level = 0

# transmitter + reciver mode 

# transmitter

group = smsc

smsc = smpp

smsc-id = gw-bulkness

#smsc-admin-id = gw-bulkness

allowed-smsc-id = gw-bulkness

host = "smpp05.bulkness.com"

port = 8887

receive-port = 0

transceiver-mode = 0

smsc-username = "username"

smsc-password = "password"

source-addr-autodetect = 1

dest-addr-ton = 1

dest-addr-npi = 1

system-type = ""

enquire-link-interval = 30

log-file = "/var/log/kannel/smsc-bulkness.log"

log-level = 0

# reciver

group = smsc

smsc = smpp

smsc-id = gw-bulkness-r

#smsc-admin-id = gw-bulkness

allowed-smsc-id = gw-bulkness-r

host = "smpp05.bulkness.com"

port = 0

receive-port = 8887

transceiver-mode = 0

smsc-username = "username"

smsc-password = "password"

source-addr-autodetect = 1

dest-addr-ton = 1

dest-addr-npi = 1

system-type = ""

enquire-link-interval = 30

log-file = "/var/log/kannel/smsc-bulkness.log"

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

# смотрим логи рассылок

смотрим тут /var/log/kannel/smsc-bulkness.log

submit_sm, submit_sm_resp отправка сообщения поставщику услуг (запрос от клиента, ответ от сервера)

deliver_sm, deliver_sm_resp отчет об отправке сообщения от поставщика услуг (запрос от сервера, ответ от клиента)

request-ы - response-ы идентифицируются между собой по строке sequence_number
