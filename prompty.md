# Prompty i nota o pracy z AI

Pracowałem mówieniem. Najpierw szeroko: co widzę, co mi nie pasuje, jaki ma być efekt. Potem zawężenie do jednego miejsca w pliku. Nie zaczynałem od gotowego promptu systemowego. Punktem odniesienia przez całe zadanie był [CONTEXT.md](CONTEXT.md).

Tekst składał agent. Ja mówiłem albo pisałem treść i wskazywałem, co w którym miejscu zmienić. Nowa wersja powstawała od razu, po kolei: A, potem B, potem C.

## Prompty w kolejności użycia

Część szła głosem, część pisana. Niżej jest sens polecenia, nie stenogram.

### Kontekst

Usunąć pliki od PDF. Zostawić w repozytorium tylko wklejony tekst briefu, w `CONTEXT.md`. Do tego pliku wracać przy każdej części.

### Część A

Rozpisać obserwacje jako A.1.1–A.1.4, a potem A.2. W każdej obserwacji: fakty, hipotezy, braki, pytania. Bez dodatkowych sekcji.

Fakty trzymać przy briefie. Macierz urządzeń i kont, udział rynku i skutek dla 60 000 oraz 15 000 to hipotezy. Przy udziale urządzeń dopisać, że w firmie wzięłoby się to z analityki, nie z publicznego składu rynku.

Pytania: trzy na obserwację, po jednym do QA, do dewelopera i do produktu. Adresat to „deweloper”. Flutter zostaje tylko jako informacja, że Android i iOS są z jednego kodu.

Przy obserwacji 3 faktem jest „zgłoszone przez jedną osobę, nieodtworzone”. Trzecie pytanie nie jest prośbą o decyzję. Jest informacją dla dewelopera: niepotwierdzone, po potwierdzeniu przez QA byłoby krytyczne, poprawki na razie nie ma.

Przy obserwacji 4 nie pisać, że sklep sam aktualizuje telefony. „Odtworzone” nie znaczy, że sprawdził to ktoś drugi.

A.2 zostawić tak, jak jest, po uwadze, że zamrożone 7.12 to nie jest hotfix 7.12.1.

### Część B1

Pięć scenariuszy na jeden dzień. Dwa pierwsze na czystym urządzeniu i koncie, Android i iOS, na urządzeniu, blisko produkcji: ścieżka kampanii oraz link zaproszenia po poprawce. Trzeci: istniejący suite jednostkowy, krótki sprawdzian migracji rozliczeń na około 60 000 kont. Czwarty: smoke UI (Maestro) onboardingu i parowania od zera. Piąty: API (Newman), parowanie zegarka, drugi zapis nie dubluje urządzenia.

Poza tym nic. Bez edge case’ów spoza kampanii i migracji. Bez starych urządzeń. Słowo „suite” w rodzaju męskim.

### Część B2

Najpierw powstał skrypt, który sam układał pięć przypadków. Odrzuciłem go: nie było w nim AI.

Polecenie, które zostało:

- usunąć runner, raporty i ranking,
- jeden plik skilla,
- drugi plik: 40 wymyślonych przypadków,
- dwa pliki zmian,
- ja wybieram skill, wskazuję plik i dostaję pięć przypadków na czacie,
- skill nie wymyśla przypadków i nie punktuje wagami,
- katalog jest zawsze `b2/katalog-40.json`,
- bez pliku zmian skill ma się zatrzymać.

Potem dwa uruchomienia, już jako użycie, nie jako opis:

- skill `wybor-dnia-testow` na `b2/mock-7-12.json`,
- ten sam skill na `b2/mock-powiadomienia.json`.

Na końcu README w `b2`: po polsku, z linkami do plików B2 i do reszty zadania.

### Część C

Dwa usprawnienia na 30 dni, co najmniej jedno z AI. Pierwsze: duże wydanie jest osobnym rodzajem wydania. Drugie: raport na code freeze. Ocenić, zmienić, jeśli coś nie trzyma się briefu, i napisać.

Potem tylko forma: przenieść to do `czesc-C.md` w katalogu głównym, z nagłówkiem jak część A i część B.

Wskaźniki: zero takich samych przypadków (za mało czasu na regresję, niewiadoma na czacie) oraz czas pracy wobec planu. Po 30 i po 90 dniach to samo. Do tego retrospektywa, która nie jest wskaźnikiem. Ocenić i wpisać.

## Prompt systemowy i konfiguracja agenta

Na części A, B1 i C nie było osobnego promptu systemowego. Była rozmowa i `CONTEXT.md`.

Jedyna zapisana konfiguracja agenta jest w [`.cursor/skills/wybor-dnia-testow/SKILL.md`](.cursor/skills/wybor-dnia-testow/SKILL.md). Włącza się tylko, gdy zostanie wybrana (`disable-model-invocation: true`).

W nagłówku:

- `name`: `wybor-dnia-testow`
- `description`: wybrać pięć przypadków z katalogu 40 na jeden dzień testów; użyć, gdy ktoś wskaże plik zmian buildu

W treści, po kolei: stanowisko (jakość jednego dnia, nie decyzja o wydaniu), kontekst aplikacji i zespołu, wejście (plik zmian plus katalog), priorytet pięciu miejsc, wyjście na czat. Katalogu nie wolno rozszerzać. Przypadki spoza pliku zmian nie wchodzą do dnia.

## Co powstało z udziałem AI

Wszystko, co jest w plikach odpowiedzi, powstało w tej rozmowie, po kolei.

- `CONTEXT.md` — brief przepisany do pliku, bez PDF.
- `czesc-A.md` — diagnoza i decyzja.
- `czesc-B.md` — pięć scenariuszy.
- `b2/katalog-40.json`, `b2/mock-7-12.json`, `b2/mock-powiadomienia.json`, skill i `b2/README.md`.
- `czesc-C.md` — dwa usprawnienia i wskaźniki.
- Ten plik.

Agent proponował brzmienie i układał plik. Ja oddawałem uwagę do konkretnego akapitu. Następna wersja szła od razu. Skrypt z B2, który układał przypadki bez modelu, został usunięty po mojej uwadze, że nie ma w nim AI.

## Co zweryfikowałem i co zmieniłem

Szło to w rozmowie, część po części, nie jako jeden odbiór na końcu. Sam pisałem i mówiłem treść. Tam, gdzie zdania układał agent, mówiłem, który fragment przestawić.

Z mojej strony, nie z briefu, weszły między innymi: trzy pytania na obserwację, hipoteza o macierzy i o udziale urządzeń, „suite” w rodzaju męskim, „deweloper” zamiast roli Flutter, pięć scenariuszy B1, kształt B2 jako skill oraz oba usprawnienia i oba wskaźniki w części C.

Zostawiłem A.2 w wersji z hotfixem 7.12.1, już po uwadze, że poniedziałkowa decyzja dotyczy zamrożonego 7.12.

Tam, gdzie agent zawęził moje sformułowanie, a ja tego nie cofnąłem, zostało tak:

- w C regułę dużego wydania trzyma QA Lead, a daty kampanii i migracji osoba, która decyduje o wydaniu,
- raport freeze czyta trzy źródła i nie decyduje o GO,
- zero powtórzeń ma punkt odniesienia w 7.12, nie w samym haśle „zero”,
- godziny są porównaniem planu z tym, co zeszło; 10 godzin spotkań jest pierwszym planem, nie pomiarem z 7.12,
- retrospektywa nie jest trzecim wskaźnikiem.
