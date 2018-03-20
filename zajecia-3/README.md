# Zajecia 3

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

## 3. Domyslna konfiguracja sieciowa maszyny

Domyslnie, nasza maszyna powinna miec ustawiony tylko jeden adapter sieciowy ktory jest podlaczony do `NAT`. Mozna to sprawdzic klikajac PPM na maszyne i wybierajac `Settings->Network`.

Po uruchomieniu maszyny komenda `ip link` (w skrocie `ip l`) powinna nam pokazac dwa interfejsy sieciowe:
 1. `lo` - loopback, wirtualny interfejs do celow w ogolnosci diagnostycznych
 2. inny, np `enp0s3` - interfejs zwiazany z powyzszym adapterem, mamy dzieki niemu polaczenie do internetu poprzez system hosta (ten na ktorym zainstalowany jest Virtualbox)
 
Z kolei komenda `ip address` (w skrocie `ip a`) powinna pokazac ewentualne adresy IP zwiazane z tymi interfejsami.
 
### Zadania

 - Sprawdzic czy mamy polaczenie z internetem za pomoca polecenia `ping 8.8.8.8` ktore wysyla pakiety do adresu IP `8.8.8.8` i sprawdza czy dostaje pakety w odpowiedzi (w pewnym sensie [z dokladnoscia do filtrowania pakietow ICMP] - czy mamy polaczenie z maszyna o zadanym adresie IP)

## 4. Statyczna konfiguracja adresacji

W dalszej czesci bedziemy odwzorowywac sytuacje w ktorej polaczylibysmy dwa komputery za pomoca osprzetu sieciowego.

Aby zasymulowac powyzsza sytuacje w srodowisku virtualbox, potrzebujemy dwoch swiezych maszyn (nazwijmy je `A` oraz `B`) bazujacych na obrazie `debian-headless.ova`. Kazdej z nich nalezy dodac nowy adapter sieciowy:
 1. kliknac PPM na maszyne
 2. wybierac `Settings->Network->Adapter 2`
 3. zaznaczyc `Enable Network Adapter`
 4. wybrac `Internal Network`
 5. podac nazwe `netx`

Po uruchomieniu, kazda z maszyn powinna miec jeden nowy interfejs bez ustawionego adresu IP (np `enp0s8`) - mozna to sprawdzic poleceniem `ip a`.

Naszym ostatecznym zadaniem bedzie polaczenie oraz skomunikowanie maszyn. Aby tego dokonac, nalezy maszynom ustawic adresy IP oraz *podniesc* interfejsy. Jesli adresacja jest poprawna, wszystko powinno byc ok.

### Zadania

 1. Kazdej maszynie ustawic statyczny, unikatowy adres IP (z tej samej podsieci) na powyzszym *nowym* interfejsie - za pomoca **odpowiedniego** wywolanego polecenia `ip address`
 2. Na kazdej maszynie *podniesc* interfejs na ktorym ustawilismy adres IP - za pomoca **odpowiedniego** wywolanego polecenia `ip link`
 3. Upewnic sie, ze mamy polaczenie z maszyny `A` do `B` i vice versa - za pomoca polecenia `ping [adres IP maszyny przeciwnej]`
 4. Na maszynie `B` postawic serwer TCP za pomoca polecenia `nc -l -p 12345`
 5. Z maszyny `A` nawiazac polaczenie TCP do serwera na maszynie `B` za pomoca polecenia `nc [adres IP maszyny B] 12345` i wyslac jakis tekst
 
**Po wykonaniu zadan, niezwlocznie zglosic i pokazac prowadzacemu.**

## 5. Dynamiczna konfiguracja adresacji

W kolejnej czesci bedziemy po raz kolejny odwzorowywac sytuacje w ktorej polaczylibysmy dwa komputery za pomoca osprzetu sieciowego.

**UWAGA!** Zalecam usunac stare maszyny z poprzednich zadan i stworzyc nowe w identyczny sposob. Dzieki temu bedziemy znowu mieli dwie czyste maszyny z dodatkowym, nieskonfigurowanym interfejsem sieciowym.

Tym razem, zamiast statycznie nadawac maszynom adresy IP, uruchomimy na jednej z nich serwer DHCP ktory bedzie adresy przydzielac dynamicznie.

Aby na maszynie `A` uruchomic serwer DHCP, nalezy najpierw nadac jej statycznie adres IP (z takiej podsieci z jakiej bedziemy udostepniac pule adresow IP) oraz podniesc interfejs. Dalej, nalezy zainstalowac, skonfigurowac i uruchomic usluge (implementacja wedle uznania).

Gdy na maszynie `A` uruchomiony bedzie dzialajacy poprawnie serwer DHCP, na maszynie `B` bedziemy mogli wydac polecenie `dhclient [nazwa interfejsu]` aby uzyskac na interfejsie `[nazwa interfejsu]` adres IP z puli oferowanej przez serwer z maszyny `A`.

### Zadania

 1. Na maszynie `A`, na interfejsie od adaptera z siecia `netx` skonfigurowac statyczny adres IP i podniesc ten interfejs.
 2. Na maszynie `A` zainstalowac dowolna implementacje serwera DHCP (np `isc-dhcp-server`)
 3. Na maszynie `A` skonfigurowac serwer DHCP tak aby przydzielal adresy IP z jakiejs puli - serwer powinien dzialac tylko dla interfejsu od adaptera z siecia `netx`. W przypadku `isc-dhcp-server` konieczna bedzie modyfikacja plikow `/etc/dhcp/dhcpd.conf` oraz `/etc/default/isc-dhcp-server`. Do edycji plikow polecam `vi`, `nano`  lub w idealnym przypadku `emacs` (ktory jednak trzeba doinstalowac). Uruchamianie serwera, zatrzymywanie, podglad statusu itp. realizujemy za pomoca odpowiednich wywolan polecenia `systemctl`. Przydatne logi znajdziemy w plkach `/var/log/syslog` oraz `/var/log/messages`.
 4. Przy jednoczesnym uruchomieniu na maszynie `A` polecenia `tail -f /var/log/syslog`, na maszynie `B` zarequestowac o adres IP poleceniem `dhclient [nazwa interfejsu]` a nastepnie go zwolnic poleceniem `dhclient -r [nazwa interfejsu]`.
