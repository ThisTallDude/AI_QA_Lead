# ZADANIE REKRUTACYJNE

## AI QA Lead

Forma: zadanie decyzyjno-techniczne · Szacowany nakład: około 3 godzin

## Cel zadania i Twoja rola

Obejmujesz rolę AI QA Lead w firmie rozwijającej aplikację mobilną połączoną z backendem, usługami operatorów komórkowych i urządzeniami IoT (zegarki, lokalizatory). Odpowiadasz za jakość w całym cyklu wytwarzania, nie tylko za testy przed wydaniem. Zespół już korzysta z asystentów AI w pracy; Twoim zadaniem jest rozwijanie tych praktyk, nie ich wprowadzanie.

Zadanie ma trzy części: decyzja o wydaniu pod presją czasu, minimalny plan testów z jednym działającym elementem AI oraz plan usprawnień na 30 dni z pomiarem efektu. Nie oczekujemy audytu ani wdrożenia frameworka testowego. Liczy się trafność decyzji, uzasadnienie i to, co faktycznie zadziała.

Wszystkie dane w zadaniu są fikcyjne.

## Kontekst organizacyjny

| Obszar | Stan obecny |
| --- | --- |
| Zespół QA | QA Lead, 3 osoby testujące manualnie, 2 inżynierów automatyzacji; sprinty tygodniowe |
| Produkt | Aplikacja Android/iOS dla rodzin: logowanie, lokalizacja, powiadomienia, strefy, parowanie urządzeń IoT, zarządzanie członkami rodziny |
| Wydania | Tygodniowy rytm wydań, nocne buildy, wdrożenia etapowe (5% → 25% → 100%), flagi funkcji dla części modułów |
| Testy | Automaty UI (Maestro) i API (Postman/Newman), ok. 40% regresji pokryte; pełna regresja manualna: 3 dni pracy zespołu |
| Narzędzia | Jira + Zephyr, GitLab CI, BrowserStack, 12 urządzeń fizycznych, Grafana + Sentry |
| AI w zespole | Asystenci kodu (Cursor, Claude Code) przy pisaniu automatów; dostęp do LLM przez firmowy endpoint API |
| Ograniczenia | Zakaz przesyłania danych klientów do zewnętrznych usług; dostępy według zasady minimalnych uprawnień; części scenariuszy z operatorami nie da się powtarzalnie sprawdzić na preprodzie |
| Przepływ pracy | QA często poznaje szczegóły zakresu dopiero przy zamrożeniu wersji; zakres zmian w sprincie jest zmienny |

## Scenariusz: wydanie pod presją

W piątek zamrożono wersję 7.12. Zawiera nowy onboarding z przebudowanym parowaniem zegarka dziecka oraz nowy model zgód (dwa etapy: zgoda opiekuna, potwierdzenie na urządzeniu). W poniedziałek rano marketing uruchamia kampanię płatną, która ma przynieść ok. 15 000 nowych instalacji w tygodniu.

Przed zamrożeniem, w testach eksploracyjnych, zespół zgłosił cztery obserwacje:

1. Powiadomienie „Jestem na miejscu” przy dwóch opiekunach w rodzinie trafia czasem do niewłaściwego opiekuna. Odtworzone 2 razy na 7 prób, tylko na Androidzie 13.
2. Link zapraszający drugiego opiekuna nie otwiera aplikacji na iOS 18, gdy aplikacja nie była wcześniej zainstalowana. Odtworzone za każdym razem na 2 urządzeniach.
3. Po drugim etapie zgody aplikacja na części urządzeń wraca do pierwszego ekranu onboardingu. Zgłoszone przez jedną osobę, nieodtworzone.
4. Gdy w jednej rodzinie część osób ma starą wersję aplikacji, a część nową, lista urządzeń pokazuje duplikaty. Odtworzone, przyczyna nieznana.

Równolegle zespół backendu kończy migrację ok. 60 000 kont ze starego systemu rozliczeń; przełączenie zaplanowano na wtorek. Nowy onboarding nie został objęty flagą funkcji, a z powodu migracji wersja 7.12 ma trafić od razu do 100% użytkowników. Pełna regresja wymaga 3 dni, a do poniedziałku dostępny jest jeden dzień pracy zespołu QA. Osoba kierująca produktem oczekuje decyzji o wydaniu w poniedziałek o 9:00.

Poufne — materiały rekrutacyjne. Strona 1 / 2

## Część A — diagnoza i decyzja o wydaniu

1. Dla każdej z czterech obserwacji oddziel fakty, hipotezy i brakujące informacje. Wskaż trzy pytania, które zadasz przed decyzją, i do kogo je skierujesz.
2. Podejmij decyzję: GO, warunkowe GO albo NO-GO. Określ blokery, warunki zmiany decyzji, kto odpowiada za wyjątek od progu jakości oraz jak ograniczysz skutki awarii po wydaniu (etapowanie, flagi, rollback, monitoring) i jaki sygnał uruchamia reakcję.

## Część B — minimalny plan testów z działającym elementem AI

### B1. Plan testów

Wybierz maksymalnie 5 scenariuszy według ryzyka, mieszczących się w jednym dniu pracy zespołu. Dla każdego podaj: dane testowe, oczekiwany wynik, poziom testu (jednostkowy / API / UI / na urządzeniu) i wymagane środowisko.

Uzasadnij w kilku zdaniach, czego świadomie nie testujesz i dlaczego.

### B2. Działający element AI

Zbuduj i uruchom jeden element wykorzystujący AI, który realnie wspiera decyzje z części A lub plan z B1. Przez element rozumiemy mały, działający przepływ: dane wejściowe trafiają do modelu, model wykonuje jeden krok pracy QA, a wynik nadaje się do użycia bez dalszej obróbki. Przykład: na wejściu lista zmian w wersji 7.12 i katalog 40 przypadków testowych (dane mockowe), na wyjściu 5 przypadków do uruchomienia w jednym dniu, uszeregowanych według ryzyka, z uzasadnieniem i flagą „wymaga urządzenia fizycznego”. Przykładowe kierunki (wybierz własny):

- selekcja regresji: z listy zmian i katalogu przypadków AI proponuje uszeregowany podzbiór z uzasadnieniem ryzyka,
- generowanie scenariuszy testowych z opisu zmiany onboardingu,
- triage czterech obserwacji ze scenariusza według powagi i prawdopodobieństwa.

Wymagania:

- element musi działać: wejście, przetwarzanie i wynik, nie makieta; dane mockowe tworzysz samodzielnie, bez danych osobowych i bez danych firm, w których pracujesz lub pracowałeś/aś,
- określ, co AI robi samodzielnie, co tylko rekomenduje i kiedy oddaje decyzję człowiekowi,
- wskaż jedno miejsce, w którym AI się pomyliło lub zaproponowało coś, czego nie przyjąłeś/aś, i jak to zweryfikowałeś/aś.

Prosty, działający element jest wart więcej niż rozbudowana koncepcja.

## Część C — plan 30 dni i pomiar efektu

1. Wybierz dwa usprawnienia na pierwsze 30 dni, żeby sytuacja ze scenariusza nie powtórzyła się w tej formie; co najmniej jedno z udziałem AI. Dla każdego podaj właściciela według roli, zależności, kryteria odbioru i to, z czego rezygnujesz, żeby zmieścić się w 30 dniach.
2. Zaproponuj maksymalnie trzy wskaźniki, którymi ocenisz efekt po 30 i 90 dniach: definicja, źródło danych, punkt odniesienia.
3. Wyjaśnij, jak odróżnisz lepsze wykrywanie błędów (więcej zgłoszeń, bo QA działa wcześniej) od pogorszenia jakości (więcej zgłoszeń, bo kod jest gorszy). Podaj konkretne sygnały.

## Co należy oddać

| Element | Forma |
| --- | --- |
| Odpowiedzi na części A, B1 i C | Dowolna |
| Element AI z części B2 | Link do działającego workflow, repozytorium lub arkusza z dostępem do podglądu; dane mockowe w komplecie |
| Prompty i nota o pracy z AI | Prompty, prompty systemowe i konfiguracje agentów użyte w całym zadaniu, w kolejności użycia, plus kilka zdań: co powstało z udziałem AI, co zweryfikowałeś/aś samodzielnie i co zmieniłeś/aś |

Poufne — materiały rekrutacyjne. Strona 2 / 2
