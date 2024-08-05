# Crack the hash

## Level 1

### Hash 1

Łamiemy hash za pomocą strony [CrackStation](https://crackstation.net/):

![1](img/1.JPG)

```
48bb6e862e54f2a795ffc4e541caed4d  |  easy
```

### Hash 2

Łamiemy hash za pomocą strony [CrackStation](https://crackstation.net/):

![2](img/2.JPG)

```
CBFDAC6008F9CAB4083784CBD1874F76618D2A97  |  password123
```

### Hash 3

Łamiemy hash za pomocą strony [CrackStation](https://crackstation.net/):

![3](img/3.JPG)

```
1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032  |  letmein
```

### Hash 4

Strona CrackStation nie rozpoznała tego hasha. W związku z tym, że ten typ hasha łamie się bardzo długo za pomocą popularnej listy haseł "rockyou", tworzymy listę wszystkich możliwych haseł 4-literowych. Następnie łamiemy hash za pomocą narzędzia hashcat:

```
import itertools

characters = 'abcdefghijklmnopqrstuvwxyz'

passwords = [''.join(p) for p in itertools.product(characters, repeat=4)]

for password in passwords:
    print(password)

with open('passwords.txt', 'w') as f:
    for password in passwords:
        f.write(password + '\n')
```

Tworzymy skrypt, nadajemy mu uprawnienia do wykonywania oraz odpalamy go:

![Python](img/Python.JPG)

Wszystkie hasła zostały zapisane do pliku "passwords.txt". Odpalamy program hashcat i łamiemy hash:

```
hashcat -m 3200 -a 0 hash.txt /home/kali/passwords.txt
```

![4](img/4.JPG)

Hash ten można też złamać za pomocą innej [strony](https://hashes.com/en/decrypt/hash):

![Hashes](img/Hashes.JPG)

```
$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom    |    bleh
```

### Hash 5

Łamiemy hash za pomocą strony [CrackStation](https://crackstation.net/):

![5](img/5.JPG)

```
279412f945939ba78ce0758d3fd83daa    |    Eternity22
```

## Level 2

### Hash 1

Łamiemy hash za pomocą strony [CrackStation](https://crackstation.net/):

![21](img/21.JPG)

```
F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85    |    paule
```

### Hash 2

Łamiemy hash za pomocą strony [CrackStation](https://crackstation.net/):

![22](img/22.JPG)

```
1DFECA0C002AE40B8619ECF94819CC1B    |    n63umy8lkf4i
```

### Hash 3

Strona CrackStation nie rozpoznała tego hasha. Na stronie [hashcat'a](https://hashcat.net/wiki/doku.php?id=example_hashes) widzimy, że typ hasha to najprawdopodobniej SHA512 i mode powinien być ustawiony na 1800. Odpalamy hashcat'a:

```
hashcat -m 1800 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```

![Hash3](img/Hash3.JPG)

Hash ten można też złamać za pomocą [strony](https://hashes.com/en/decrypt/hash):

![23](img/23.JPG)

```
$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.    |    waka99
```

### Hash 4

Strona CrackStation nie rozpoznała tego hasha. Na stronie [hashcat'a](https://hashcat.net/wiki/doku.php?id=example_hashes) widzimy, że typ hasha to najprawdopodobniej HMAC-SHA1 i mode powinien być ustawiony na 160. Odpalamy hashcat'a:

```
hashcat -m 160 -d 1 hash.txt /usr/share/wordlists/rockyou.txt
```

Zawartość hash.txt to:

```
e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme
```

![Hash4](img/Hash4.JPG)

Hash ten można też złamać za pomocą [strony](https://hashes.com/en/decrypt/hash):

![24](img/24.JPG)

```
e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme    |    481616481616
```

Do zobaczenia na kolejnych CTF-ach!
