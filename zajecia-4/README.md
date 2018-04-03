# Zajecia 4

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

## 3. Ustawianie zapory sieciowej

W tej czesci bedziemy odwzorowywac sytuacje identyczną do tej z zajec nr 3 (punkt 4) - taka w ktorej polaczylismy dwa komputery za pomoca osprzetu sieciowego.

Naszym ostatecznym zadaniem bedzie skomunikowanie maszyn oraz odpowiednie przefiltrowanie ruchu sieciowego. Aby tego dokonac, nalezy maszynom ustawic adresy IP oraz *podniesc* interfejsy. Jesli adresacja jest poprawna, wszystko powinno byc ok i maszyny powinny sie wzajemnie pingowac. Dalej, nalezalo bedzie odpowiednimi wywolaniami polecenia `iptables` wprowadzac/usuwac odpowiednie reguly.

### Zadania

 1. **Upewnic sie, ze mamy polaczenie z internetem `ping 8.8.8.8` - jesli maszyna zostala stworzona w domu moze byc problem z DNSami**
 2. Ustawic statycznie adresy IP na maszynach `A` i `B` tak aby maszyny sie wzajemnie ping-owaly oraz nawiazywaly polaczenia TCP/UDP za pomoca netcata `nc` - tak jak na zeszlych zajeciach
 3. Na maszynie `A` zablokowac ruch TCP na porcie `12345` i udowodnic to za pomoca netcat-a `nc`
 4. Na maszynie `A` zablokowac caly ruch UDP i przetestowac netcat-em
 5. Zainstalowac `lynx` i upewnic sie, ze mozemy wejsc na `onet.pl` oraz `interia.pl`, nastepnie zablokowac ruch TCP do `onet.pl` tak zeby `lynx` w jego przypadku nie dzialal, ale dzialal w przypadku `interia.pl`
 6. Zablokowac mozliwosc pingowania sie maszyn
 
**Po wykonaniu wszystkich zadan, niezwlocznie zglosic i pokazac prowadzacemu przed przejsciem dalej.**

## 5. Konfiguracja routing-u

W tej czesci bedziemy odwzorowywac sytuacje w ktorej polaczylibysmy trzy komputery w lancuchu (chain) poprzez dwie oddzielne sieci.

**UWAGA!** Zalecam usunac stare maszyny z poprzednich zadan i stworzyc nowe.

Aby uzyskac pozadana konfiguracje bedziemy potrzebowac trzech maszyn; `A`, `R` oraz `B` kazda z nastepujacymi adapterami sieciowymi:

 - `A` - `Internal Network` (`net1`)
 - `R` - `Internal Network` (`net1`) oraz `Internal Network` (`net2`) (kolejnosc adapterow ma znaczenie)
 - `B` - `Internal Network` (`net2`)
 
Sytuacje mozna zobrazowac w ten sposob:

```
[A]<---net1--->[R]<---net2--->[B]
```

Prosze nie zapomniec o *podlaczeniu kabla* w *zaawansowanych*.

Nastepnie:

 1. Na interfejsach `enp0s8` maszyn `A` oraz `R` nalezy ustawic statycznie adresy IP z jakiejs sieci (nazwijmy ja `X`)
 2. Na interfejsie `enp0s9` maszyny `R` nalezy ustawic statycznie adres IP z innej sieci (nazwijmy ja `Y`)
 3. Na interfejsie `enp0s8` maszyny `B` nalezy ustawic statycznie inny adres IP z sieci `Y`

### Zadania

 1. **Upewnic sie, ze mamy polaczenie z internetem `ping 8.8.8.8` - jesli maszyna zostala stworzona w domu moze byc problem z DNSami**
 2. Upewnic sie, ze maszyny `A` oraz `R` pinguja sie w zakresie sieci `X`
 3. Upewnic sie, ze maszyny `B` oraz `R` pinguja sie w zakresie sieci `Y`
 4. Upewnic sie, ze maszyny `A` oraz `B` **nie** pinguja sie
 5. Sprawic aby maszyna `R` zadzialala jak router - maszyny `A` i `B` powinny sie za jej posrednictwem pingowac (bedac w innych podsieciach). Dokonac tego mozna poprzez ustawienie odpowiedniego routingu na maszynach `A` i `B` oraz wlaczenie trybu forwardowania pakietow miedzy interfejsami na maszynie `R`
 6. Zainstalowac `tcpdump`
 7. Na maszynie `R` wlaczyc dump-owanie ruchu tcp (`tcpdump -i any -X | tee plik.txt`). Nastepnie przeslac bajty z maszyny `A` do `B` za pomoca netcat-a. Na koniec, na maszynie `R` zweryfikowac zawartosc pliku `plik.txt`
