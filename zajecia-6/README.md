# Zajecia 6

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

## 3. Podstawowe informacje o uzytkownikach i grupach

Aby uzyskac informacje o uzytkownikach oraz grupach w systemie, mozna skorzystac z szeregu plikow tekstowych takich jak `/etc/passwd`, `/etc/group`, `/etc/shadow`, `/etc/gshadow` lub polecen np `id`, `finger` (domyslne nie zainstalowane).

### Zadania

1. Poszukac informacji tlumaczacych co oznaczaja kolejne kolumny w pliku `/etc/passwd`
2. Sprawdzic jacy uzytkownicy oraz grupy istnieja w systemie
3. Sprawdzic `UID`, `GID` oraz przynaleznosc do grup uzytkownikow `student` oraz `root`

## 4. Zarzadzanie uzytkownikami i grupami

Do zarzadzania uzytkownikami oraz grupami zaleca sie (zamiast recznej modyfikacji plikow konfiguracyjnych) uzycie polecen takich jak `useradd`, `userdel`, `usermod`, `groupadd`, `groupdel`, `groupmod`, `passwd`, `gpasswd`.

Przelaczanie uzytkownikow jest mozliwe przy uzyciu polecenia `su`. Z kolei rownoczesne logowania mozna realizowac za pomoca wirtualnych terminali: `Alt` + `->` - nastepny, `Alt` + `<-` - poprzedni.

### Zadania

1. Dodac nowego uzytkownika
2. Dodac nowa grupe
3. Dodac nowego uzytkownika tak aby nalezal do grupy utworzonej wczesniej
4. Dodac nowego uzytkownika z jednoczesnym stworzeniem katalogu domowego
5. Dodac nowego uzytkownika tak aby jego domyslna powolka byl `/bin/dash`
6. Zablokowac mozliwosc logowania uzytkownikowi `student`
7. Zmienic haslo jednemu z nowo dodanych uzytkownikow
8. Usunac jednego z nowo dodanych uzytkownikow
9. Dodac uzytkownika `student` do jednej z nowo dodanych grup

## 5. Zarzadzanie prawami dostepu

Dostep do plikow i katalogow jest w najprostszym wariancie kontrolowany poprzez proste prawa ktore mozna zmieniac poleceniami takimi jak `chmod`, `chown`, `chgrp`, `umask`. Istnieje bardziej ogolny mechanizm - ACL, jednak nie bedzie on przedmiotem naszych rozwazan.

Aby sprawdzic zestaw praw zwiazanych z danym plikiem/katalogiem, mozna skorzystac z polecenia `ls` z przelacznikiem `-l` np `ls -l /etc/passwd`.

### Zadania

1. Stworzyc plik oraz sprawic aby przynalezal do uzytkownika `root` oraz grupy `student`
2. Stworzyc plik tylko do odcztu (dla wszystkich uzytkownikow/grup)
3. Stworzyc katalog, a w nim plik. Nadac uzytkownikowi `student` pelen dostep do pliku ale odebrac calkowicie dostep do katalogu. Na koniec sprawdzic, czy uzytkownik `student` moze odczytac plik
4. Stworzyc katalog, a w nim plik. Nadac uzytkownikowi `student` pelen dostep do pliku oraz katalogu, ale w przypadku katalogu, dodatkowo usunac wszystkie wystapienia flagi `x` (executable). Na koniec sprawdzic:
 * czy uzytkownik `student` moze wejsc do katalogu
 * czy uzytkownik `student` moze wylistowac katalog
 * czy uzytkownik `student` moze przeczytac plik
5. Stworzyc plik i nadac mu prawa `-r---w---x` za pomoca polecenia `chmod` oraz numerycznej reprezentacji praw
6. Sprawdzic i wytlumaczyc dlaczego plik `/usr/bin/passwd` ma takie a nie inne prawa dostepu
