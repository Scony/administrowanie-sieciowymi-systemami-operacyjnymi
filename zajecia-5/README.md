# Zajecia 5

## 1. Tworzenie i uruchamianie maszyny wirtualniej

Zanim przejdziemy do uruchomienia maszyny wirtualnej, musimy najpierw pobrac przygotowany wczesniej snapshot gotowej maszyny (z zainstalowanym debianem):

```
wget http://pawel.lampe.staff.iiar.pwr.wroc.pl/debian-headless.ova
```

Aby stworzyc maszyne wirtualna nalezy:
 - uruchmic program `virtualbox`
 - z menu `File` wybrac opcje `Import appliance`
 - wyszukac i wybrac pobrany wczesniej `debian-headless.ova`

Po poprawnym imporcie maszyny, mozna ja uruchomic.

## 2. Logowanie do systemu

Poprawnie uruchomiony system powinien uruchomic sie az do ekranu logowania. Do dyspozycji sa dwa podstawowe konta:
 - zwykle - `student` z haslem `student`
 - uprzywilejowane - `root` z haslem `root`

Zalecam uzywanie konta `student` z ewentualnym przechodzeniem na konto `root` tylko kiedy potrzeba. Przejscie z uzytkownika `student` na `root` mozna zrealizowac poprzez wykonanie komendy `su` i podanie hasla uzytkownika `root`.

## 3. Konfiguracja maszyn

Na tych zajeciach bedziemy po raz kolejny odwzorowywac sytuacje identyczna do tej z zajec nr 3 (punkt 4) - taka w ktorej polaczylismy dwa komputery za pomoca osprzetu sieciowego.

Naszym celem bedzie zainstalowanie oraz skonfigurowanie serwera DNS na maszynie `A` oraz konfiguracja resolvera na maszynie `B` ktora bedzie nam sluzyc jako klient DNS.

### Zadania

1. Na obu maszynach **upewnic sie, ze mamy polaczenie z internetem `ping onet.pl` - jesli maszyna zostala stworzona w domu moze byc problem z DNSami**
2. Ustawic statycznie adresy IP na maszynach `A` i `B` tak aby maszyny sie wzajemnie ping-owaly
3. Z maszyny `B` przetestowac dzialanie polecen `host`, `dig` oraz `nslookup`:
  * sprawdzic nazwe domenowa dla `8.8.8.8`
  * sprawdzic adres IP dla `facebook.com`
  * sprawdzic adres IP dla `facebook.com` zwracany przez serwer DNS o adresie `8.8.8.8`
  * sprawdzic adres IP dla `facebook.com` zwracany przez serwer DNS o adresie `208.67.222.222`
4. Na maszynie `A` zainstalowac serwer DNS (`bind9`)
5. Na maszynie `A` skonfigurowac domene prosta i do pliku strefy wpisac po jednej nazwie domenowej dla maszyny `A` oraz `B` (do walidacji konfiguracji moga sie przydac polecenia `named-checkconf` oraz `named-checkzone`)
6. Na maszynie `B` skonfigurowac resolver tak aby uzywal serwera DNS z maszyny `A` (plik `/etc/resolv.conf`)
7. Na maszynie `B` przetestowac rozwiazywanie nazw zdefiniowanych na serwerze
8. Na maszynie `B` przetestowac ping-owanie maszyny `A` **po nazwie domenowej** (a nie po adresie IP)
9. Na maszynie `A` skonfigurowac translacje odwrotna (reverse DNS) dla nazw domenowych maszyn `A` oraz `B` zdefiniowanych w zadaniu `5`.
10. Na maszynie `B` przetestowac skonfigurowana wyzej translacje odwrotna
