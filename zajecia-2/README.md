# Zajecia 2

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

## 3. Pierwsze kroki w systemie

Poniewaz system jest w wersji `headless` - nie ma w nim graficznego interfejsu uzytkownika (`GUI`). Interakcja z systemem odbywa sie w pelni poprzez wydawanie polecen.

Z uwagi na powyzsze, nie mozemy przewijac mysza tekstu ktory nie miesci sie na jednym ekranie. W takim przypadku musimy uzyc potoku `|` oraz polecenia `less`:

```
ls -l / | less
```

Jesli potrzebujemy wydac kilka czasochlonnych polecen na raz, mozemy uzyc innego niz pierwszy terminala. Przechodzenie pomiedzy terminalami realizuje sie za pomoca skrotu `logo_windows + strzalka (lewo/prawo)`.

## 4. Podstawowe informacje o systemie

Aby uzyskac garsc podstawowych informacji na temat nazwy hosta, wersji jadra, nazwy systemu itp. mozna skorzystac z polecenia:

```
uname -a
```

Aby dowiedziec sie o nim wiecej, mozemy przestudiowac strone manuala:

```
man uname
```

### Zadania

Sprawdzic jakim poleceniem mozna:
 - sprawdzic zajetosc systemow plikow
 - wyswietlic informacje na temat procesora (CPU)
 - sprawdzic jacy uzytkownicy sa zalogowani w systemie
 - wylistowac interfejsy sieciowe
 - wylistowac urzadzenia podpiete do magistrali PCI

Poszukac pliku ktory:
 - zawiera informacje o kontach uzytkownikow istniejacych w systemie
 - zawiera wydanie systemu (tzw. release)

## 5. Zarzadzanie oprogramowaniem

W przypadkach gdy potrzebujemy programu, a nie jest on zainstalowany w systemie lub gdy chcemy sie czegos dowiedziec o oprogramowaniu ktore posiadamy, mozemy (w przypadku debiana) skorzystac z polecen takich jak np:
 - `apt-cache` - pozwala na nieinwazyjne przegladanie informacji zawartych w metadanych systemu zarzadzania pakietami
 - `apt-get` - pozwala zarzadzac pakietami
 
Zanim zaczniemy wyszukiwac/instalowac nowe pakiety, musimy zsynchronizowac pliki indeksow pakietow ze zdalnymi zrodlami (aby to zrobic potrzebujemy uprawnien uzytkownika `root`):

```
su
# wpisujemy haslo
apt-get update # wymaga polaczenia z internetem
```

### Zadania

 - za pomoca `apt-cache` sprawdzic jaka wersja(wersje) edytora `emacs` dostepna jest do zainstalowania
 - zainstalowac pakiety: `lynx`, `nmap`, `apt-file`
 - zsynchronizowac `apt-file` komenda `apt-file update`
 - za pomoca polecenia `apt-file` dowiedziec sie jaki pakiet dostarcza plik `/bin/netstat` oraz zainstalowac ten pakiet (odpowiednie wywolanie `apt-get`)
 - sprawdzic co robi polecenie `lynx pawel.lampe.staff.iiar.pwr.edu.pl`
 - sprawdzic za pomoca manuala co robi polecenie `nmap`

## 6. Budowanie oprogramowania ze zrodel

Czasami zdarza sie, iz repozytoria nie zawieraja pakietu ktory nas interesuje. W takich przypadkach zazwyczaj jestesmy zdani za budowanie pakietu ze zrodel.

Aby zbudowac przykladowy program:
 1. musimy zainstalowac odpowiedni toolchain (w tym przypadku pakiet `build-essential`)
 2. musimy pobrac paczke ze zrodlami: `wget http://mama.indstate.edu/users/ice/tree/src/tree-1.7.0.tgz`
 3. musimy rozpakowac paczke: `tar xzf tree-1.7.0.tgz`
 4. musimy wejsc do katalogu: `cd tree-1.7.0`
 5. musimy zbudowac program: `make`
 6. i uruchomic: `./tree`
 7. mozemy rowniez przeczytac strone manuala: `man ./doc/tree.1`
 8. oraz zainstalowac program w systemie: `make install` (wymaga uprawnien uzytkownika `root`)

### Zadania

 - sprawdzic ile bajtow ma zbudowany program (`tree`)
 - wydac polecenie `strip tree` i sprawdzic jak zmienil sie rozmiar programu oraz czy nadal dziala
