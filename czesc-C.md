# Część C — plan 30 dni

Sytuacja z 7.12 nie ma się powtórzyć w tej formie: migracja rozliczeń i nowa ścieżka kampanii w jednym buildzie, bez flagi, ze freeze na jeden dzień i z decyzją rano w dniu startu.

## 1. Duże wydanie jest osobnym rodzajem wydania

Tygodniowy rytm zostaje. Obok niego jest drugi rodzaj: wydanie, w którym jest nowa ścieżka dla użytkownika albo migracja danych, albo oba. Takie wydanie nie jedzie trybem zwykłego tygodnia.

Reguła:

- W jednym takim wydaniu jest jeden temat. Migracja 60 000 kont i nowa kampania z onboardingu nie wchodzą w ten sam build.
- Nowa ścieżka ma flagę, zanim build zostanie zamrożony. Bez flagi nie ma freeze.
- Termin liczy się wstecz od dnia wejścia na produkcję. W tym liczeniu jest pełna regresja (3 dni) i dwie fazy na poprawki. Freeze nie jest dzień przed startem.
- Zanim freeze się zacznie, wiadomo, kto co robi: jakość, produkt, osoba od migracji, osoby od buildu. Czas na poprawki jest w planie, nie „jak się zmieści po freeze”.

### Właściciel

Ja, w roli QA Lead. Trzymam regułę: co jest dużym wydaniem, czego brakuje, żeby wolno było zamrozić build, i czy odliczanie od dnia startu się zgadza.

Daty kampanii i daty migracji nie ustawiam. Ustawia je osoba, która decyduje o wydaniu (product manager albo osoba od release). Bez jej terminu reguła nie ma od czego liczyć wstecz.

### Zależności

- Osoba decydująca o wydaniu przyjmuje, że duży start ma jeden temat i datę policzoną wstecz.
- Osoba od migracji mówi, czy przełączenie rozliczeń w ogóle wymaga nowego klienta. Jeśli nie wymaga, migracja nie wchodzi do buildu aplikacji.
- Znany jest czas pełnej regresji: 3 dni. Dwie fazy poprawek dokłada zespół buildu, nie jakość z pamięci.

### Kryteria odbioru

- Jest krótki opis dwóch rodzajów wydania i jednej listy warunków freeze dla dużego.
- Najbliższy duży start, albo próba na historii 7.12, jest rozpisany wstecz: dzień produkcji, dwie fazy poprawek, trzy dni regresji, dzień freeze.
- W tym rozpisaniu nie ma naraz migracji kont i nowej ścieżki kampanii, a nowa ścieżka ma flagę przed freeze.

### Z czego rezygnuję w 30 dniach

Nie stawiam narzędzia do planowania wydań i nie zmieniam zwykłego tygodniowego rytmu. Nie biorę na siebie kalendarza marketingu. Jest jedna reguła i jedno rozpisanie wstecz.

## 2. Raport na code freeze, z udziałem AI

Przy code freeze agent zbiera to, co już jest, i oddaje jeden raport dla wszystkich. Raport mówi, co się zmieniło, co zostało wykonane w testach i czego nie wiadomo. Nie jest wiadomością na czacie i nie jest decyzją GO.

Agent czyta trzy źródła:

- diff buildu, który wchodzi w freeze,
- zgłoszenia podpięte do tej wersji,
- ostatni przebieg automatów (Maestro, Newman, suite jednostkowy).

Gdy w zgłoszeniu brakuje liczby prób, urządzenia, buildu albo informacji, czy to regresja, raport wpisuje „nie wiadomo” i osobę, która zgłoszenie dodała. Ta osoba jest w raporcie po to, żeby brak uzupełnić. Raport sprawdza komplet względem bramki jakości. Nie domyka bramki sam.

### Właściciel

QA Lead trzyma, co raport musi zawierać i że powstaje przy freeze, zanim ktokolwiek pyta o decyzję. Agent raport przygotowuje. Osoba ze zgłoszenia uzupełnia brak, który raport jej wypisze.

### Zależności

- Dostęp do diffa, listy zgłoszeń wersji i ostatniego przebiegu CI. W 30 dniach wystarczy odczyt, który da się powtórzyć. MCP tylko tam, gdzie dostęp już jest. Reszta może być eksportem.
- Jedno miejsce, w którym raport ląduje i skąd widać, kogo dotyczy brak. Nie prywatna wiadomość.

### Kryteria odbioru

- Jedno freeze, albo suche przejście na materiałach 7.12, kończy się raportem.
- Przy każdej luce raport ma „nie wiadomo” i osobę, nie zgadywaną powagę.
- Raport da się przeczytać bez dopytywania na czacie, co właściwie weszło w build i co było puszczone.

### Z czego rezygnuję w 30 dniach

Nie podłączam Grafany, Sentry, Zephyra i całego Jiry naraz. Nie każę agentowi decydować o wydaniu. Trzy źródła i jeden raport. Kolejne źródła są po tych 30 dniach, jeśli te trzy już się spinają.

## Wskaźniki

Dwa wskaźniki. Liczą się tylko duże wydania, takie jak w punkcie 1. Zwykły tygodniowy rytm ich nie dotyczy.

To, że reguła i raport powstały, że ludzie oddali do nich uwagi i że suche przejście na 7.12 jest dostępne dla wszystkich, jest odbiorem punktów 1 i 2. Nie jest osobnym wskaźnikiem.

### 1. Zero powtórzeń tej samej sytuacji

**Definicja.** Liczba dużych wydań, w których zaszło choć jedno z dwóch:

- na regresję zostało mniej niż 3 dni albo tych 3 dni nie ma w odliczaniu wstecz,
- ktoś dopytuje na czacie, co weszło w build albo co zostało wykonane, a raport freeze tego nie ma (także jako „nie wiadomo” z osobą).

Cel: zero.

**Źródło danych.** Rozpisanie wstecz tego wydania, raport freeze w miejscu, które widzą wszyscy, oraz rozmowa wokół freeze (czy takie pytanie w ogóle padło). Nie biorę całego statusu produktu.

**Punkt odniesienia.** Wydanie 7.12: jedno duże wydanie, regresja ściśnięta do jednego dnia przed decyzją, a niewiadome dochodziły na czacie, bez raportu. To jest jeden taki przypadek.

**Po 30 dniach.** Suche przejście na materiałach 7.12 pokazuje oba rodzaje przypadku na tym wydaniu. Jeśli w tych 30 dniach nie było nowego dużego wydania, nie zapisuję zera z produkcji. Zapisuję, że 7.12 jest tym jednym przypadkiem, od którego liczę dalej.

**Po 90 dniach.** Każde duże wydanie z tego okna ma zero obu rodzajów. Jeden taki przypadek znaczy, że wskaźnik nie zszedł do zera.

### 2. Godziny planu wobec godzin, które zeszły

**Definicja.** Różnica między godzinami wpisanymi w plan dużego wydania a godzinami, które faktycznie zeszły na spotkania przy tym planie i na raport freeze. Cel nie brzmi „ma być mniej”. Cel brzmi: plan, który został po korekcie, dalej się zgadza z tym, co schodzi.

**Źródło danych.** Liczba godzin wpisana w rozpisaniu oraz krótka notatka po suchym przejściu albo po wydaniu, ile zeszło. Bez nowego narzędzia do ewidencji czasu.

**Punkt odniesienia.** Dziś takiej liczby nie ma: przy 7.12 nikt jej nie zapisał. Pierwszy plan, od którego liczę, to 10 godzin na spotkania. Drugą liczbę, na pozostałą pracę przy planie i raporcie, wpisuje się w to samo rozpisanie przed suchym przejściem. Nie zgaduję jej z góry.

**Po 30 dniach.** Suche przejście daje pierwsze rzeczywiste godziny. Jeśli zeszło wyraźnie mniej niż 10, następny plan jest krótszy. Jeśli więcej, 10 było za mało i plan rośnie. W notatce jest jedna poprawiona para liczb.

**Po 90 dniach.** Najbliższe duże wydanie mieści się w poprawionym planie. Jak nie, para liczb jest poprawiana drugi raz, a wskaźnik za to okno jest niezaliczony.

### Po 30 i po 90 dniach: retrospektywa

To nie jest trzeci wskaźnik. To jedno spotkanie w dniu 30 i jedno w dniu 90 z ludźmi, których dotyczy reguła: jakość, deweloper, produkt, osoba od migracji, osoby od buildu oraz osoba, którą raport wypisał przy „nie wiadomo”.

Z spotkania wychodzi krótka notatka: co zostaje i co się zmienia w regule, w raporcie albo w godzinach planu. Trzy pytania:

- czy odliczanie wstecz zostawiło 3 dni regresji,
- czy raport odpowiadał, co weszło i co było puszczone, czy i tak poszło to na czat,
- czy wpisane godziny dalej się zgadzają z tym, co zeszło.

## 3. Więcej zgłoszeń: wcześniejsze QA albo gorszy kod

Samej liczby zgłoszeń nie rozdzielam. Rozdzielam ją po czterech sygnałach. Źródło jest w Jirze i w wyniku tego samego suite'u: środowisko wpisane w zgłoszeniu, ile razy zgłoszenie wróciło po poprawce, oraz czy padło sprawdzenie, które poprzedni build przechodził.

### Gdzie schodzi zgłoszenie

Wcześniejsze QA: zgłoszenie powstaje na środowisku, na którym jakość patrzy przed użytkownikiem. Test, preprod, dzień testów. Build jeszcze nie jest na produkcji.

Gorszy kod: to samo zachowanie schodzi na produkcji albo na buildzie, na którym te sprawdzenia już raz przeszły.

### Ile razy wraca po poprawce

Gorszy kod: błąd jest poprawiony, wraca do testu i znowu nie przechodzi. Liczy się powrót tego samego zgłoszenia. Im więcej takich powrotów, tym więcej razy trzeba retestować tę samą poprawkę, i to jest sygnał kodu.

Wcześniejsze QA daje nowe zgłoszenie z jednym cyklem. Nie serię powrotów tego samego numeru.

### Te same sprawdzenia

Automatyzacja i te same suite'y puszczane na tym samym zakresie. Więcej niezaliczonych niż przy poprzednim buildzie, na zadaniach, które wcześniej przechodziły, to kod.

Przypadki dopisane pod nowy feature do tego porównania nie wchodzą. One podnoszą liczbę, bo zakres jest szerszy, a nie dlatego, że stary zakres się posypał.

### Co jest w zgłoszeniu

Wcześniejsze QA: część znalezisk nie jest usterką aplikacji. Jest rozjazdem z opisem nowego feature'a. Dokumentacja, kryteria odbioru, brak w specyfikacji. Takie zgłoszenie powstaje, bo jakość weszła, zanim opis był domknięty.

Gorszy kod: zgłoszenie dotyczy zachowania, które suite już znał i które wcześniej przechodziło.
