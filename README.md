# kannel ubuntu 16.04

install kannel 

sudo apt-get update

sudo apt-get install kannel

# редактируем конфиг

sudo gedit /etc/kannel/kannel.conf

#пример конфига

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

# тут настройки оператора смс-рассылок 

group = smsc

smsc = smpp

smsc-id = gw-bulkness

smsc-admin-id = gw-bulkness

allowed-smsc-id = gw-bulkness

host = "smpp05.bulkness.com"

port = 8887

transceiver-mode = 1

smsc-username = "username"

smsc-password = "password" #!!!длина пароля не должна превышать 8 символов!!!

source-addr-autodetect = 1

dest-addr-ton = 1

dest-addr-npi = 1

system-type = ""

enquire-link-interval = 30

log-file = "/var/log/kannel/smsc-bulkness.log"

log-level = 0


