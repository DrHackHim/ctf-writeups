# Cat Pictures II
IP=10.10.63.124

## Rekonesans
Przeprowadzamy wstępny skan za pomocą narzędzia nmap:

```
sudo nmap -p- 10.10.63.124
```

Otrzymujemy następujące wyniki:

```
Starting Nmap 7.92 ( https://nmap.org ) at 2024-08-11 04:54 EDT
Nmap scan report for 10.10.63.124
Host is up (0.048s latency).
Not shown: 65531 closed tcp ports (reset)
PORT      STATE SERVICE
25/tcp    open  smtp
80/tcp    open  http
55006/tcp open  unknown
55007/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 529.90 seconds
```

Przeprowadźmy bardziej szczegółowy skan:

```
sudo nmap -p 25,80,55006,55007 -sC -sV 10.10.63.124
```

Otrzymujemy następujące wyniki:

```
Starting Nmap 7.92 ( https://nmap.org ) at 2024-08-11 05:05 EDT
Nmap scan report for 10.10.63.124
Host is up (0.049s latency).

PORT      STATE SERVICE  VERSION
25/tcp    open  smtp     Postfix smtpd
|_smtp-commands: ubuntu, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN
| ssl-cert: Subject: commonName=ubuntu
| Not valid before: 2018-04-24T03:22:34
|_Not valid after:  2028-04-21T03:22:34
|_ssl-date: TLS randomness does not represent time
80/tcp    open  http     Apache httpd 2.4.7 ((Ubuntu))
|_http-title: GoldenEye Primary Admin Server
|_http-server-header: Apache/2.4.7 (Ubuntu)
55006/tcp open  ssl/pop3 Dovecot pop3d
|_pop3-capabilities: SASL(PLAIN) PIPELINING USER TOP UIDL AUTH-RESP-CODE RESP-CODES CAPA
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=localhost/organizationName=Dovecot mail server
| Not valid before: 2018-04-24T03:23:52
|_Not valid after:  2028-04-23T03:23:52
55007/tcp open  pop3     Dovecot pop3d
|_pop3-capabilities: AUTH-RESP-CODE TOP STLS SASL(PLAIN) PIPELINING CAPA USER RESP-CODES UIDL
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=localhost/organizationName=Dovecot mail server
| Not valid before: 2018-04-24T03:23:52
|_Not valid after:  2028-04-23T03:23:52

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.06 seconds
```

Zostały wykryte 4 porty.

## Skanowanie

### Port 80
Rozpoczynamy od sprawdzenia portu 80:

![80](img/80.JPG)

Jest to strona nawiązująca do tytułu tego CTF'a. Sprawdzamy źródło strony:

![Source](img/Source.JPG)

Przechodzimy do skryptu terminal.js: 

![Terminal](img/Terminal.JPG)

Z ukrytej notatki otrzymujemy zakodowane hasło użytkownika Boris oraz kolejnego użytkownika - Natalya. Przechodzimy na stronę [CyberChef](https://gchq.github.io/CyberChef/) i dekoduejmy hasło:

![Cyber](img/Cyber.JPG)

```
boris:InvincibleHack3r
```

Przechodzimy do katalogu /sev-home/ i logujemy się za pomocą znalezionych danych do logowania:

![Login](img/Login.JPG)

Po zalogowaniu widzimy tajemniczą stronę:

![Sev](img/Sev.JPG)

W źródle strony znajdujemy notatkę, która potwierdza fakt istnienia dwóch użytkowników: Natalya i Boris

![Sup](img/Sup.JPG)

Próby zalogowania się na konto Borisa na innych serwisach zakończyły się niepowodzeniem. W związku z tym przeprowadzamy atak za pomocą hydry, w celu znalezienia innego hasła:

```
hydra -l Boris -P /usr/share/set/src/fasttrack/wordlist.txt 10.10.63.124 -s 55007 pop3
```

![Hydra](img/Hydra.JPG)

```
Boris:secret1!
```

Wykorzystujemy znalezione hasło, aby zalogować się do serwisu telnet skonfigurowanego na porcie 55007:

```
telnet 10.10.63.124 55007
```

![Telnet](img/Telnet.JPG)

W serwisie znajdujemy wiadomości (emails). Dowiadujemy się z nich, że Natalya może złamać kody Borisa. Poznajemy też nową nazwę użytkownika - Xenia:

![RETR](img/RETR.JPG)

Kontynuujemy ataki za pomocą hydry, tym razem na użytkownika Natalya:

![Hydra2](img/Hydra2.JPG)

```
Natalya:bird
```

Wykorzystujemy znalezione hasło, aby zalogować się do serwisu telnet skonfigurowanego na porcie 55007:

![Telnet2](img/Telnet2.JPG)

Odczytujemy dostępne wiadomości, z których otrzymujemy dane do logowania użytkwonika Xenia oraz adres tajemniczej strony ('severnaya-station.com/gnocertdir/'):

![RETR2](img/RETR2.JPG)

```
xenia:RCP90rulez!
```

Dodajemy nowo poznaną stronę do pliku /etc/hosts:

![Su](img/Su.JPG)

Logujemy się do serwisu za pomocą znalezionych danych do logowania:

```
xenia:RCP90rulez!
```

![Moodle](img/Moodle.JPG)




