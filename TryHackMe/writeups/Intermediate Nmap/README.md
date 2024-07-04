# Intermediate Nmap
IP=10.10.133.187

## Rekonesans
Przeprowadzamy wstępny skan za pomocą narzędzia nmap:

```
sudo nmap -p- 10.10.133.187
```

Otrzymujemy następujące wyniki:

Port 31337 jest portem bardzo wysokim, a co za tym idzie - ciekawym, dlatego to od niego zaczniemy.

## Skanowanie

### Port 31337

Rozpoczynamy od sprawdzenia portu 31337:

![Port](img/Port.JPG)

```
ubuntu:Dafdas!!/str0ng
```

### Port 22

Znajdujemy notatkę z nazwą użytkownika i hasłem. Dane te wykorzystujemy do logowania za pomocą SSH:

```
ssh ubuntu@10.10.133.187
```

![SSH](img/SSH.JPG)

## Eksploitacja

W katalogu domowym użytkownika user znajdujemy flagę:

![Flaga](img/Flaga.JPG)

```
flag{251f309497a18888dde5222761ea88e4}
```

Do zobaczenia na kolejnych CTF-ach!
