# Easy Peasy

IP=10.10.158.179

## Rekonesans
Przeprowadzamy wstępny skan za pomocą narzędzia nmap:

```
sudo nmap -p- 10.10.158.179
```

```
Starting Nmap 7.92 ( https://nmap.org ) at 2024-08-09 05:41 EDT
Nmap scan report for 10.10.158.179
Host is up (0.049s latency).
Not shown: 65532 closed tcp ports (reset)
PORT      STATE SERVICE
80/tcp    open  http
6498/tcp  open  unknown
65524/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 52.41 seconds
```

Przeprowadzamy bardziej szczegółowy skan:

```
sudo nmap -p 80,6498,65524 -sV -sC 10.10.158.179
```

```
Starting Nmap 7.92 ( https://nmap.org ) at 2024-08-09 05:43 EDT
Nmap scan report for 10.10.158.179
Host is up (0.048s latency).

PORT      STATE SERVICE VERSION
80/tcp    open  http    nginx 1.16.1
| http-robots.txt: 1 disallowed entry 
|_/
|_http-title: Welcome to nginx!
|_http-server-header: nginx/1.16.1
6498/tcp  open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 30:4a:2b:22:ac:d9:56:09:f2:da:12:20:57:f4:6c:d4 (RSA)
|   256 bf:86:c9:c7:b7:ef:8c:8b:b9:94:ae:01:88:c0:85:4d (ECDSA)
|_  256 a1:72:ef:6c:81:29:13:ef:5a:6c:24:03:4c:fe:3d:0b (ED25519)
65524/tcp open  http    Apache httpd 2.4.43 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.43 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.37 seconds
```

Zostały wykryte 3 porty. Wersja nginx to 1.16.1, a na najwyższym porcie działa usługa Apache.

## Skanowanie

### Port 80

Rozpoczynamy od sprawdzenia portu 80:

![80](img/80.JPG)

Jest to domyślna strona nginx. Przechodzimy do enumeracji katalogów za pomocą narzędzia ffuf:

```
ffuf -w /usr/share/wordlists/dirb/big.txt -u http://10.10.158.179/FUZZ -c  -recursion
```

![Ffuf](img/Ffuf.JPG)

Program wykrył dwa katalogi. Przechodzimy do katalogu /hidden/whatever:

![What](img/What.JPG)

Znajdujemy tutaj fotografię. Sprawdzamy źródło strony:

![Base64](img/Base64.JPG)

Widzimy ukryty ciąg znaków zakodowany Base64, który dekodujemy przy pomocy [CyberChef'a](https://gchq.github.io/CyberChef/):

![Flag1](img/Flag1.JPG)

Odczytujemy pierwszą flagę:

```
flag{f1rs7_fl4g}
```

### Port 65524

Przechodzimy do sprawdzenia portu 65524. Jest to domyślna strona Apache'a:

![Apache](img/Apache.JPG)

Przechodzimy do enumeracji katalogów za pomocą narzędzia ffuf:

```
ffuf -w /usr/share/wordlists/dirb/big.txt -u http://10.10.158.179:65524/FUZZ -c -recursion
```

![Ffuf2](img/Ffuf2.JPG)

W pliku robots.txt w polu User-Agent znajdujemy ciekawy ciąg znaków przypominających hash:

![Robots](img/Robots.JPG)

```
a18672860d0510e5ab6699730763b250
```

Korzystamy z [dekodera online](https://md5hashing.net/), aby złamać hash'a w formacie MD5. Otrzymujemy drugą flagę:

![Flag2](img/Flag2.JPG)

```
flag{1m_s3c0nd_fl4g}
```

W źródle domyślnej strony Apache'a znajdujemy trzecią flagę oraz pewną ukrytą notkę:

![Flag3](img/Flag3.JPG)

```
flag{9fdafbd64c47471a8f54cd3fc64cd312}
```

![Hidden](img/Hidden.JPG)

```
ObsJmP173N2X6dOrAgEAL0Vu
```

Dekodujemy ciąg znaków przy pomocy [CyberChef'a](https://gchq.github.io/CyberChef/):

![Chef](img/Chef.JPG)

```
/n0th1ng3ls3m4tt3r
```

Przechodzimy do ukrytej lokalizacji, która zawiera zdjęcie:

![Photo](img/Photo.JPG)

