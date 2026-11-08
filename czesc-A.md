# Część A — diagnoza

Część A.1: dla każdej obserwacji fakty, hipotezy, brakujące informacje i trzy pytania przed decyzją.

Aplikacja mobilna jest we Flutterze: jeden kod daje build Androida i build iOS. Poprawka klienta idzie do zespołu Flutter. Backend zostaje osobno tam, gdzie decyzja zapada na serwerze.

Rozkład urządzeń poniżej to dane mockowe na potrzeby tej decyzji. W firmie wziąłbym je z Google Analytics albo innego narzędzia: ile z 60 000 kont jest na danej wersji systemu i jaki mix zakłada kampania 15 000 instalacji. Dopóki tego nie ma, przyjmuję rozkład rynku w Polsce: Android około 61%, iOS około 39%, Android 13 około 10% urządzeń z Androidem.

## A.1.1 Powiadomienie „Jestem na miejscu” trafia do niewłaściwego opiekuna

### Fakty

- Przy dwóch opiekunach w rodzinie powiadomienie „Jestem na miejscu” czasem trafia do niewłaściwego opiekuna.
- Odtworzone 2 razy na 7 prób.
- Odtworzenia były na Androidzie 13.
- Zespół zobaczył to w testach eksploracyjnych przed zamrożeniem 7.12.

### Hipotezy

- Test nie szedł matrycą telefon × konto × wersja systemu. Szedł jednym telefonem z Androidem 13: siedem prób, dwa trafienia. Oba trafienia mogą być z tego samego egzemplarza. „Tylko Android 13” jest wtedy nazwą tego telefonu, a nie wynikiem porównania z innymi wersjami.
- To samo konto na dwóch telefonach z tym samym buildem powinno dostać tego samego odbiorcę. Jeśli telefon A dostaje powiadomienie właściwie, a telefon B nie, przyczyna siedzi przy urządzeniu (system, model, token, uprawnienie, build na tym aparacie). Jeśli oba telefony mylą odbiorcę tak samo, przyczyna siedzi przy koncie albo przy backendzie.
- Routing bierze domyślnego opiekuna, gdy zdarzenie nie ma jednoznacznego odbiorcy. Ta hipoteza jest słabsza, jeśli objaw naprawdę ogranicza się do Androida 13.
- Różnica jest po stronie klienta Androida 13: token FCM, kanał powiadomień albo uprawnienie do powiadomień.
- Błąd już jest na produkcji albo leży w backlogu. Odkrycie w testach eksploracyjnych 7.12 nie mówi, że to regresja tego wydania.
- Wpływ, na mockowym rozkładzie: Android 13 to około 3 700 kont w istniejącej bazie, czyli około 6,2% z 60 000 (60 000 × 61% × 10%). W kampanii to około 900 nowych instalacji, czyli około 6% z 15 000 (15 000 × 61% × 10%). Procent jest podobny, bo na obie grupy nałożyłem ten sam mix rynku. Różnica jest w liczbie osób: około 3 700 kont już w bazie i około 900 z kampanii. Błąd widać przy dwóch opiekunach i w 2 próbach na 7, więc dotknięta część jest mniejsza od tych dwóch liczb. Przy tej skali nie trzymam całego 7.12, jeśli poprawka jest mała i wejdzie w poniedziałkowy build. Liczby służą do tej decyzji do czasu podmiany na rozkład z analytics.

### Brakujące informacje

- Czy błąd jest już na produkcji, czy jest ticket w backlogu, czy to pierwsze zgłoszenie na kandydata 7.12.
- Ile było telefonów, kont i modeli. Czy oba trafienia są z jednego urządzenia. Nie wiadomo też, czy to był fizyczny telefon, urządzenie w BrowserStacku, czy emulator.
- Czy sprawdzano drugi telefon z Androidem 13 na tym samym koncie i czy sprawdzano Androida 14+ oraz build sprzed 7.12.
- Kim był niewłaściwy odbiorca: drugi opiekun z tej samej rodziny dodany przez pierwszego(głównego).
- Czy w modelu danych jest główny opiekun i opiekun pomocniczy oraz który miał dostać to zdarzenie.
- Rzeczywisty udział Androida 13 w 60 000 kontach i w planie kampanii.

### Pytania

1. Do QA, które robiło testy eksploracyjne: jak wyglądało pokrycie tego błędu? Ile było urządzeń (jedno, dwa, trzy), czy próby szły na tych samych kontach, czy 2 trafienia na 7 prób zeszły tak, jak jest w zgłoszeniu, i czy to regresja 7.12, czy coś zgłoszone wcześniej? Zapis takiego przebiegu powinien być do odczytu: urządzenia, build, konto, link w Zephyrze albo Jirze, który albo sam otwieram, albo dostaję od QA. Zakładam, że tego zapisu nie ma.
2. Do dewelopera: jak szybko jesteście w stanie to naprawić, ile potrzebujecie czasu i jakie są warunki wejścia poprawki na produkcję? Czy wchodzi w najbliższe tygodniowe wydanie? Jeśli nie wejdzie w build na poniedziałek 9:00, czy jesteście w stanie wypchnąć to jako hotfix?
3. Do product managera albo head of product: kampania startuje w poniedziałek rano. Czy marketing może ją przesunąć, i czy „Jestem na miejscu” przy dwóch opiekunach jest dla was funkcjonalnością core'ową na ścieżce tej kampanii, czy problem widziany na Androidzie 13 nie zaburza tego, z czym chcecie wyjść, także wtedy, gdy nacisk kampanii jest po stronie Androida?

## A.1.2 Link zapraszający drugiego opiekuna nie otwiera aplikacji na iOS 18

### Fakty

- Link zapraszający drugiego opiekuna nie otwiera aplikacji na iOS 18, gdy aplikacja nie była wcześniej zainstalowana.
- Odtworzone za każdym razem, na 2 urządzeniach.
- Zaproszenie drugiego opiekuna jest w nowym onboardingu i w nowym modelu zgód wersji 7.12.
- Nowy onboarding nie ma flagi funkcji.

### Hipotezy

- W notatce jest iOS 18, bo taki system był na telefonach z próby. Nie zakładam modelu ani tego, że oba urządzenia to na pewno iOS 18. Innych wersji iOS i próby na Androidzie bez aplikacji mogło nie być.
- Gdy aplikacja już jest zainstalowana, link działa. Zgłoszenie opisuje tylko brak wcześniejszej instalacji, więc odwrotności mogło nikt nie sprawdzić. Psuje się wtedy wejście linkiem, zanim ktoś cokolwiek zainstaluje, a nie każda nowa instalacja z reklamy.
- Link jest schematem aplikacji (`aplikacja://zaproszenie`). Bez zainstalowanego programu iOS nie ma czego otworzyć i nie prowadzi do App Store. Wadą jest brak zejścia do sklepu i brak dokończenia zaproszenia po instalacji.
- Link jest zwykłym adresem https (universal link). Aplikacja otwiera się tylko wtedy, gdy domena jest z nią skojarzona. Bez aplikacji otwiera się Safari i zostaje w przeglądarce, bo strona nie przekierowuje do App Store albo gubi kod zaproszenia po instalacji.
- Dwa telefony i powtarzanie się „za każdym razem” wskazują na rodzaj linku, nie na jeden zepsuty egzemplarz.
- To regresja albo luka nowego linku w 7.12, a nie stary ticket. Tego nie wiem bez backlogu i bez starego linku.
- Wpływ, na mockowym rozkładzie: iOS to około 39% rynku, czyli około 5 850 nowych instalacji z kampanii (15 000 × 39%) i około 23 400 kont w istniejącej bazie (60 000 × 39%). Udziału samego iOS 18 nie znam, więc 5 850 to górka całego iOS, nie wersji 18. Ta górka to nowe instalacje, nie liczba klikniętych zaproszeń. Nowy model zgód stoi na zaproszeniu drugiego opiekuna, flagi nie ma, a na dwóch urządzeniach link nie dowoził do aplikacji.

### Brakujące informacje

- Co znaczy „za każdym razem”: ile było prób, czy to ten sam link i czy oba urządzenia.
- Jakie to były urządzenia: model, dokładna wersja systemu, fizyczny telefon, BrowserStack czy symulator.
- W którym momencie ścieżka staje: kliknięcie, Safari, App Store, ekran po instalacji.
- Czy przy już zainstalowanej aplikacji link działa.
- Czy to znany problem na produkcji albo ticket w backlogu, czy pierwsze zgłoszenie na 7.12.
- Czy obok eksploracji jest automat Maestro na ten link.
- Czy po ręcznej instalacji ze sklepu zaproszenie da się dokończyć.
- Czy stary link zaproszenia, sprzed 7.12, zachowuje się tak samo.

### Pytania

1. Do QA, które robiło testy eksploracyjne: jak wyglądała matryca tego linku? Ile było urządzeń, czy próby szły na innych telefonach i wersjach niż te dwa ze zgłoszenia, czy „za każdym razem” da się policzyć, w którym momencie ścieżka staje i czy to regresja 7.12, czy coś zgłoszone wcześniej? Zapis przebiegu powinien być do odczytu: urządzenia, build, link w Zephyrze albo Jirze. Zakładam, że tego zapisu nie ma. Czy jest i gdzie?
2. Do dewelopera: dla mnie to jest NO-GO na ścieżce zaproszenia. Jak szybko jesteście w stanie wprowadzić hotfix, żeby link na iOS dowoził do instalacji i do dokończenia zaproszenia, i żeby QA zdążyło go sprawdzić przed publikacją? Czy da się to oflagować albo wycofać bez cofania całego 7.12?
3. Do product managera albo head of product: na mockowym rozkładzie iOS to około 5 800 nowych instalacji z kampanii. Problem jest znaczący właśnie na tych nowych osobach, które wchodzą linkiem, zanim mają aplikację. Czy kampanię da się opóźnić? Co możecie zrobić po swojej stronie: wyjść w poniedziałek z informacją, że nowa wersja będzie w tym tygodniu, na przykład w środę albo w czwartek, zamiast puszczać w poniedziałek rano ścieżkę, która na iOS nie dowozi drugiego opiekuna?

## A.1.3 Po drugim etapie zgody aplikacja wraca na pierwszy ekran onboardingu

### Fakty

- Po drugim etapie zgody aplikacja na części urządzeń wraca do pierwszego ekranu onboardingu.
- Zgłoszone przez jedną osobę, nieodtworzone.
- Drugi etap zgody jest w nowym modelu zgód wersji 7.12.
- W zgłoszeniu nie ma liczby prób, wersji systemu, modelu telefonu ani zapisu, co zrobiono po powrocie na pierwszy ekran.

### Hipotezy

- Ta sama osoba zobaczyła powrót raz, spróbowała jeszcze raz na tym samym telefonie i tym samym koncie, bez czystego startu (reinstalacja, wyczyszczenie danych), i drugi raz już nie zeszło. „Nieodtworzone” znaczy wtedy „nie weszło powtórnie w tym samym przebiegu”, nie „sprawdzone i czyste”.
- Ta sama osoba przeszła matrycę: inny telefon, inne konto, czysty start, inna wersja systemu, i powrót nie wrócił. Tego z opisu nie widać. Nie ma wyniku w stylu 1 na 4, 1 na 7 albo 1 na 10.
- „Nieodtworzone” bez tego zapisu mieści w sobie dużo różnych rzeczy: jeden przypadek, zły krok, stan konta, konkretny telefon, wersja systemu, której nikt nie wpisał. Bez wejścia w to, jak test wyglądał, nie da się powiedzieć, co to jest za błąd.
- Dopóki QA tego nie potwierdzi, nie proszę o poprawkę. Deweloper dostaje tylko informację, że zgłoszenie jest. Po potwierdzeniu, z mojej strony, byłoby krytyczne, bo to krok nowego onboardingu. Decyzję podejmę dopiero na odpowiedziach QA.

### Brakujące informacje

- Ile razy osoba zgłaszająca to klikała i czy druga próba była od razu, na tym samym telefonie i koncie, bez resetu.
- Czy ktokolwiek sprawdził to poza tym jednym przebiegiem: drugi telefon, drugie konto, czysta instalacja, inna wersja Androida albo iOS.
- Jaki był telefon, system i build. Tego w zgłoszeniu nie ma.
- Co stało się po powrocie na pierwszy ekran: został tam, jak na początku onboardingu, czy dało się przejść dalej.
- Gdzie jest zapis tej próby: Zephyr, Jira, telefon, konto, build.

### Pytania

1. Do QA, które ma to zgłoszenie: jak to było sprawdzane? Jedna osoba, jedna próba, potem druga na tym samym telefonie i koncie bez resetu, czy ta sama osoba przeszła matrycę (inny telefon, inne konto, czysta instalacja, inna wersja systemu)? Ile było prób? Jaki telefon, system i build? Zapis przebiegu powinien być do odczytu. Zakładam, że go nie ma. Czy jest i gdzie?
2. Do QA: po powrocie na pierwszy ekran onboardingu dało się przejść dalej, czy ekran został tak, jak na starcie zgody?
3. Do dewelopera: daję znać, że takie zgłoszenie jest i że jest niepotwierdzone. Nie pytam, czy to wasza decyzja wydaniowa. Z mojej strony, jeśli QA potwierdzi je odpowiedziami na pytania powyżej, byłoby to krytyczne. Teraz potwierdzenia nie ma, więc poprawki nie zamawiam. Decyzję podejmę po odpowiedzi QA.

## A.1.4 Lista urządzeń pokazuje duplikaty przy mieszanych wersjach

### Fakty

- Gdy w jednej rodzinie część osób ma starą wersję aplikacji, a część nową, lista urządzeń pokazuje duplikaty.
- Odtworzone. Przyczyna nieznana.
- W zgłoszeniu nie ma systemu, liczby prób, liczby kont ani tego, czy druga próba szła na tych samych telefonach.

### Hipotezy

- „Odtworzone” znaczy, że inna osoba wzięła inne konta i inne telefony i duplikat wrócił. To jest założenie. Równie dobrze ktoś mógł kliknąć drugi raz na tej samej parze telefonów.
- Skoro dało się to powtórzyć poza jednym egzemplarzem, duplikat będzie widać na dużej części kont, w których w jednej rodzinie stoją obok siebie stara i nowa wersja.
- To nie jest krytyczne. Aplikacja działa. Da się dostać dwa powiadomienia albo źle wybrać urządzenie i skonfigurować je jeszcze raz. Dwa takie same wpisy jednego zegarka, z tymi samymi danymi, to dwa taby, nie zepsuta funkcja.
- 15 000 nowych instalacji z kampanii schodzi na nową wersję. Jeśli to nowe rodziny, a nie drugie konto do rodzica, który zostaje na starej aplikacji, ta kampania w ogóle tego błędu nie dotyka.
- Telefon sam nie przechodzi na 7.12 w chwili wrzucenia buildu do sklepu. Automatycznej aktualizacji nie ma, chyba że ktoś ją wymusi. Mieszane wersje mogą zostać u obecnych rodzin. Tego nie biorę z testu. To opis tego, jak sklep działa.

### Brakujące informacje

- Czy odtworzenie to druga osoba, inne konta i inne telefony, czy powtórka na tych samych urządzeniach.
- Android, iOS, czy oba. Jakie wersje systemów i które buildy: stary i 7.12.
- Jak duży jest zasięg: jedna rodzina czy więcej. Czy duplikat widać w odpowiedzi API, czy tylko na ekranie.
- Czy było to wcześniej, na produkcji albo w backlogu.
- Czy dwa wpisy mają te same dane, czy rozjeżdżają się.
- Czy 15 000 z kampanii to nowe rodziny na samej nowej wersji, czy dołączają do kont, które zostają na starej.

### Pytania

1. Do QA, które to odtwarzało: jak wyglądało odtworzenie? Te same telefony jeszcze raz, czy inna osoba, inne konta i inne urządzenia? Android, iOS, czy oba, i czy to regresja, czy coś zgłoszone wcześniej? Zapis przebiegu powinien być do odczytu: urządzenia, build, link w Zephyrze albo Jirze. Zakładam, że go nie ma. Czy jest i gdzie?
2. Do dewelopera: to nie jest krytyczne i może poczekać. Czy da się to naprawić w kolejnym wydaniu, i czy widzicie możliwość dołożenia poprawki do innego hotfixa, zamiast pchać ją osobno przed poniedziałkiem?
3. Do product managera albo head of product: 15 000 z kampanii to nowe rodziny, które instalują od razu nową wersję, czy część z nich dochodzi jako drugie konto do rodzica zostającego na starej aplikacji? Od tego zależy, czy kampania w ogóle ten duplikat zobaczy.

## A.2 Decyzja

**Warunkowe GO** na hotfix 7.12.1.

Publikacja zamrożonego 7.12 do 100% użytkowników w poniedziałek o 9:00 nie przechodzi. Jeśli warunek poniżej nie jest spełniony przed tą godziną, decyzja na 9:00 zostaje NO-GO. Kampania zostaje na ostatniej stabilnej wersji albo się przesuwa.

### Bloker

Obserwacja 2. Link zapraszający drugiego opiekuna nie dowozi na iOS, gdy aplikacji nie ma. To jest core nowego onboardingu i ścieżka, którą kampania dopiero otwiera. Na mockowym rozkładzie iOS to około 39% nowych instalacji, około 5 800 z 15 000. Nie cały rynek. Prawie 40% kampanii, i właśnie te osoby wchodzą linkiem, zanim cokolwiek zainstalują.

Obserwacja 1 i obserwacja 4 blokerem nie są. Jeśli mieszczą się w tym samym hotfixie, idą jako priorytet drugi i trzeci, za zaproszeniem. Obserwacja 3 zostaje poza poprawką, dopóki QA jej nie potwierdzi.

### Warunek, po którym wchodzimy

1. Poprawka linku jest w 7.12.1 i QA ją sprawdziło.
2. Jest matryca tej zmiany: brak aplikacji i aplikacja już zainstalowana, więcej niż jeden telefon, wynik zapisany w Zephyrze albo Jirze. Tę część testujemy najpierw.
3. Jeden dzień zespołu nie jest pełną regresją. Pełna regresja trwa 3 dni i jej nie było. Ten dzień idzie w hotfix. Automaty, które już są, odpalamy od razu: Maestro i Newman, około 40% regresji w GitLab CI. Nowego zestawu testów w ten dzień nie piszemy. Reszta dnia to testy ręczne zaproszenia, potem obserwacja 1 i 4, jeśli zostanie czas.
4. W 7.12.1 wraca flaga nowego onboardingu. Zamrożonego 7.12 już nie oflagujemy, tej flagi w nim nie ma. Klient nie jedzie na 100% tylko dlatego, że we wtorek jest migracja 60 000 kont. Rozliczenia, jeśli mają się przełączyć, przełączają się backendem. Jeśli architekt powie, że migracja technicznie wymaga 100% klientów 7.12, warunek pada i zostaje NO-GO.

### Kto odpowiada za wyjątek od progu jakości

Product manager albo head of product. Jakość jest zmierzona, rekomendację daje QA. Wyjątku QA nie podpisuje. Jeśli produkt puszcza kampanię bez poprawki linku albo bez flagi, w decyzji jest ta osoba, zakres wyjątku i warunek wycofania.

### Jeden dzień na kandydacie

Automaty i testy ręczne są na miejscu przy 7.12.1. Crash, zawieszenie albo wyraźny spadek wydajności zatrzymuje wydanie. Nie publikujemy. Wracamy do ustalonej wcześniej stabilnej wersji.

### Trzy plany na awarię

Wersja, do której wolno się cofnąć, jest nazwana zanim dzień się zacznie: ostatnia stabilna, najbliższa temu, co kampania może puścić. Wszyscy wiedzą, która to jest.

1. Najgorszy plan: rollback do tej wersji. Sklep nie zbiera binarki w ciągu godziny, więc rollback jest na awarię szerszą niż jedna ścieżka: crash na starcie, utrata logowania, lokalizacja, która idzie nie tam.
2. Plan normalny: hotfix 7.12.1 wchodzi etapami 5% → 25% → 100%, z flagą onboardingu, tak jak dotychczasowe wydania. Jeśli statystyki siadają, flaga schodzi i procent stoi. Jeśli to nie wystarcza, plan 1.
3. Plan na sam dzień testów: cokolwiek z automatu albo z testu ręcznego ingeruje w jakość w sposób z punktu wyżej, kandydat nie wychodzi. To jest ten sam rollback, tylko zanim build trafi do sklepu.

### Jaki sygnał uruchamia reakcję

Monitoring stoi w Grafanie i w Sentry. Gdzie deweloper zdąży, dokładamy zdarzenie po stronie backendu i aplikacji, żeby było widać, że dana rzecz się dzieje, a nie tylko że aplikacja się wywaliła. Sam wykres nic nie robi. Reakcję uruchamia przekroczenie progu. Liczby poniżej są propozycją na to wydanie.

| Sygnał | Gdzie | Próg | Kto reaguje | Akcja |
| --- | --- | --- | --- | --- |
| Zaproszenie drugiego opiekuna na iOS nie kończy się wejściem w zgodę | Grafana | spadek ukończenia o więcej niż 20 punktów procentowych względem tygodnia przed wydaniem, utrzymany godzinę | QA woła produkt | flaga onboardingu schodzi, etap stoi |
| Crash albo zawieszenie na starcie, utrata logowania | Sentry | crash-free spada o więcej niż 1 punkt procentowy na etapie 5%, albo crash na starcie | QA | rollback do ustalonej stabilnej wersji |
| „Jestem na miejscu” do złego opiekuna, duplikat urządzenia | Grafana, tylko jeśli zdarzenie jest dołożone | widać je na etapie 5% | deweloper | poprawka przy kolejnym hotfixie, bez rollbacku całego wydania |

Obserwacja 1 i 4 nie uruchamiają planu 1. Są w hotfixie tylko wtedy, gdy wchodzą razem z linkiem, na drugim i trzecim priorytecie.
