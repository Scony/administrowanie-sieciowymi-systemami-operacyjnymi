# Zajecia 7

## 1. Konfiguracja wstepna

Do niniejszych zajec potrzebne nam beda dwie maszyny wirtualne spiete w siec oraz z ustawiona statyczna adresacja sieci (jak na zajeciach 3, punkt 4):

```
[A](192.168.1.5/24)<---netx--->(192.168.1.7/24)[B]
```

Dodatkowo, maszynie `A` nalezy nadac hostname `debian-a`, a maszynie `B` - `debian-b`:

```
hostname [NOWA_NAZWA_HOSTA]
exec bash
```

Jezeli maszyny sie ping-uja to mozna przejsc dalej.

## 2. Podstawowa praca z SSH

Najczestszymi przypadkami uzycia protokolu SSH jest logowanie sie na zdalne maszyny oraz wykorzystywanie szyfrowanego polaczenia do transferu danych.

W kontekscie systemow GNU/Linux najczestszymi poleceniami wykorzystywanymi do pracy z szyfrowanmi polaczeniami sa: `ssh`, `ssh-keygen`, `scp`, `sshfs`, `sftp` oraz `rsync`.

### Zadania

1. Na maszynie `A` wlaczyc podglad logow `sshd`: `tail -f /var/log/auth.log` i zostawic
2. Z maszyny `A`:
   1. zalogowac sie po protokole SSH na uzytkownika `student` na maszynie `B`
   2. wydac kilka polecen
   3. wrocic na maszyne `A` (`CTRL+d` lub `exit`)
3. Z maszyny `A` wydac zdalne polecenie (np `hostname`) na maszynie `B`, ale bez wchodzenia na maszyne `B`
4. Na maszynie `A` napisac prosty skrypt BASHowy wywolujacy kilka prostych polecen i uruchomic go zdalnie (poprzez SSH) na maszynie `B`
5. Sprawdzic czy z maszyny `A` mozna zalogowac sie po protokole SSH na uzytkownika `root` na maszynie `B`
6. Na maszynie `A` skonfigurowac klienta SSH tak, aby zamiast adresu IP maszyny `B` mozna bylo podawac prosty alias

## 3. Konfiguracja SSH do pracy z kluczami - zamiast uzywania hasla

Z uwagi na wygode oraz bezpieczenstwo, nie zaleca sie logowania na zdalne maszyny po SSH za pomoca hasla.
Zamiast tego, preferowanym jest korzystanie z pary kluczy publiczny-prywatny czyli tzw. kryptografii klucza publicznego.

### Zadania

1. Na maszynie `A`, z konta uzytkownika `student`:
   1. wygenerowac pare kluczy SSH
   2. odszukac plik zawierajacy klucz publiczny i sprawdzic jego zawartosc
   3. wgrac klucz publiczny na maszyne `B`, tak aby zadzialala autoryzacja klucza publicznego
   4. zalogowac na maszyne `B` bez uzycia hasla
2. Na maszynie `B` odszukac plik do ktorego zostal dodany klucz publiczny uzytkownika `student` z maszyny `A` i zweryfikowac jego zawartosc
3. Na maszynie `B` usunac plik znaleziony w powyzszym podpunkcie i sprawdzic czy nadal mozna zalogowac sie bez hasla na konto uzytkownika `student` z maszyny `A`

## 4. Rekonfiguracja serwera SSH

Jednym z podstawowych zadan kazdego administratora jest odpowiednie skonfigurowanie daemona SSH w celu precyzyjnej kontroli nad zdalnymi logowaniami do systemu.

### Zadania

1. Kolejno:
   1. na maszynie `B` zmienic konfiguracje `sshd` w taki sposob, aby mozliwe bylo zdalne logowanie na uzytkownika `root` z podaniem hasla
   2. zweryfikowac wprowadzona zmiane poprzez zalogowanie sie na uzytkownika `root`, na maszynie `B` z maszyny `A`
2. Kolejno:
   1. na maszynie `B` zmienic konfiguracje `sshd` w taki sposob, aby poziom logowania ustawiony byl na DEBUG
   2. na maszynie `B` wlaczyc podglad na zywo pliku z logami: `tail -f /var/log/auth.log`
   3. z maszyny `A` zalogowac sie na maszyne `B`
   4. zweryfikowac czy na maszynie `B` pojawily sie logi typu `debug1`

## 5. Eksploracja mozliwosci SSH

Jak zostalo wspomniane wczesniej, protokol SSH ma wiele zastosowan oraz wspolpracuje z nim cala gamma roznych polecen. Wiele z nich jest przydatne do codziennej pracy w srodowisku sieciowym tudziez w klastrze.

### Zadania

1. Na maszynie `A`, z konta uzytkownika `student` wygenerowac duzy plik tekstowy (`dd if=/dev/zero of=plik.txt bs=1M count=500`) i skopiowac go (polecenie `scp`) na maszyne `B` do katalogu `/home/student/`
2. Kolejno:
   1. Na maszynie `B` uruchomic netcata nasluchujacego na porcie `1338`
   2. Na maszynie `A` uruchomic tunel SSH z lokalnego portu `1337`, na zdalny port `1338` na maszynie `B`
   3. Na maszynie `A`, na drugim wirtualnym terminalu (`ALT + ->`) podlaczyc sie netcatem do lokalnego (`127.0.0.1`) portu `1337` i przetestowac mozliwosc komunkacji
   4. Na maszynie `A`, na poprzednium wirtualnym terminalu (`ALT + <-`) zamknac tunel SSH
   5. Sprawdzic czy polaczenie miedzy netcatami na maszynach `A` i `B` zostalo podtrzymane
3. Na maszynie `A`:
   1. zainstalowac pakiet `sshfs`
   2. stworzyc pusty katalog
   3. zamontowac katalog `/home/student` z maszyny `B` w stworzonym katalogu
   4. zweryfikowac zawartosc zamontowanego katalogu (`ls -al [NAZWA_KATALOGU]`)
   5. odmontowac katalog
   6. zweryfikowac zawartosc zamontowanego katalogu raz jeszcze (`ls -al [NAZWA_KATALOGU]`)
4. Na maszynie `A`, z konta uzytkownia `student`:
   1. Obejrzec output klienta ssh z nawiazywania polaczenia oraz wywolywania zdalnego polecenia: `ssh -vvv student@192.168.1.7 'hostname' |& less`
   2. Otworzyc polaczenie ssh w tle (`-f`), w trybie `ControlMaster` oraz z ustawiona opcja `ControlPath` do maszyny `B` - aby sprawdzic, zeby polaczenie nie zamykalo sie od razu lecz po upywie kwantu czasu, mozna np wywolac za jego pomoca polecenie `sleep 1000`
   3. Obejrzec i porownac output klienta ssh z nawiazywania polaczenia oraz wywolywania zdalnego polecenia gdy uzywane jest aktywne polaczenie typu `ControlMaster`: `ssh -vvv -o ControlPath=[SCIEZKA] student@192.168.1.7 'hostname' |& less`
