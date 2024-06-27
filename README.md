# Домашнее задание к занятию "`Защита сети`" - `Тен Денис`

### Подготовка к выполнению заданий

1. Подготовка защищаемой системы:

- установите **Suricata**,
- установите **Fail2Ban**.

Установка **Surikata**

```sh
sudo apt install software-properties-common
sudo apt update
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt install suricata
```
Обновление базы сигнатур

```sh
sudo suricata-update
```
```
27/6/2024 -- 21:51:21 - <Info> -- Writing rules to /var/lib/suricata/rules/suricata.rules: total: 50657; enabled: 38545; added: 50657; removed 0; modified: 0
```

Проверка установки Surikata

```sh
root@ubuntu22-client:~# sudo systemctl status suricata
● suricata.service - LSB: Next Generation IDS/IPS
     Loaded: loaded (/etc/init.d/suricata; generated)
     Active: active (exited) since Thu 2024-06-27 21:51:10 UTC; 1min 52s ago
       Docs: man:systemd-sysv-generator(8)
    Process: 2192 ExecStart=/etc/init.d/suricata start (code=exited, status=0/SUCCESS)
        CPU: 193ms

Jun 27 21:51:10 ubuntu22-client systemd[1]: Starting LSB: Next Generation IDS/IPS...
Jun 27 21:51:10 ubuntu22-client suricata[2192]: Starting suricata in IDS (af-packet) mode... done.
Jun 27 21:51:10 ubuntu22-client systemd[1]: Started LSB: Next Generation IDS/IPS.
```

Настройка Suricata

```sh
sudo vim /etc/suricata/suricata.yaml
EXTERNAL_NET: “any”
af-packet:
interface: ens160
default-rule-path: /var/lib/suricata/rules
rule-files:
- suricata.rules
```

Установка **Fail2Ban**

```sh
sudo apt install fail2ban
```

Проверка установки **Fail2Ban**

```sh
root@ubuntu22-client:~# sudo systemctl status fail2ban

○ fail2ban.service - Fail2Ban Service
     Loaded: loaded (/lib/systemd/system/fail2ban.service; disabled; vendor preset: enabled)
     Active: inactive (dead)
       Docs: man:fail2ban(1)
```


2. Подготовка системы злоумышленника: установите **nmap** и **thc-hydra** либо скачайте и установите **Kali linux**.

Обе системы должны находится в одной подсети.

------

### Задание 1

Проведите разведку системы и определите, какие сетевые службы запущены на защищаемой системе:

**sudo nmap -sA < ip-адрес >**

**sudo nmap -sT < ip-адрес >**

**sudo nmap -sS < ip-адрес >**

**sudo nmap -sV < ip-адрес >**

По желанию можете поэкспериментировать с опциями: https://nmap.org/man/ru/man-briefoptions.html.


*В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.*

### Решение Задание 1


```sh
sudo nmap -sA 10.159.86.98
```
![image](https://github.com/killakazzak/13-03-sdb-hw/assets/32342205/5c09ccdf-49e1-4dea-b020-f61d25b2a35d)


```sh
sudo nmap -sT 10.159.86.98
```
![image](https://github.com/killakazzak/13-03-sdb-hw/assets/32342205/c3c44384-ed75-458b-8682-218d40b92ddb)


```sh
sudo nmap -sS 10.159.86.98
```
![image](https://github.com/killakazzak/13-03-sdb-hw/assets/32342205/a3b4a300-0151-41a9-888f-9386c525626d)


```sh
sudo nmap -sV 10.159.86.98
```
![image](https://github.com/killakazzak/13-03-sdb-hw/assets/32342205/6eaa40f6-3cd0-451e-a055-283e471980ba)

------

### Задание 2

Проведите атаку на подбор пароля для службы SSH:

**hydra -L users.txt -P pass.txt < ip-адрес > ssh**

1. Настройка **hydra**: 
 
 - создайте два файла: **users.txt** и **pass.txt**;
 - в каждой строчке первого файла должны быть имена пользователей, второго — пароли. В нашем случае это могут быть случайные строки, но ради эксперимента можете добавить имя и пароль существующего пользователя.

Дополнительная информация по **hydra**: https://kali.tools/?p=1847.

2. Включение защиты SSH для Fail2Ban:

-  открыть файл /etc/fail2ban/jail.conf,
-  найти секцию **ssh**,
-  установить **enabled**  в **true**.

Дополнительная информация по **Fail2Ban**:https://putty.org.ru/articles/fail2ban-ssh.html.

### Решение Задание 2

