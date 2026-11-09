# B2. Wybór pięciu przypadków na dzień

Skill czyta listę zmian buildu i katalog 40 przypadków. Oddaje pięć, które mieszczą się w jednym dniu testów. Nie ma skryptu do uruchomienia. Krok robi agent w Cursorze.

## Jak użyć

1. Otwórz ten repozytorium w Cursorze.
2. W czacie wybierz skill [wybor-dnia-testow](../.cursor/skills/wybor-dnia-testow/SKILL.md).
3. Wskaż jeden plik zmian, na przykład: użyj skilla na [b2/mock-7-12.json](mock-7-12.json).
4. Odpowiedź w czacie to pięć przypadków z [katalogu](katalog-40.json): id, tytuł, dlaczego wchodzą przy tym buildzie, dane testowe, oczekiwany wynik, poziom, środowisko i czy trzeba fizycznego telefonu.

Skill sam nie startuje. Trzeba go wybrać i podać plik. Katalog jest zawsze ten sam. Zmienia się tylko plik zmian.

## Pliki tego folderu

- [katalog-40.json](katalog-40.json) to czterdzieści wymyślonych przypadków. Z niego skill wybiera piątkę.
- [mock-7-12.json](mock-7-12.json) to build 7.12: onboarding, parowanie zegarka, zgoda, link drugiego opiekuna, migracja rozliczeń.
- [mock-powiadomienia.json](mock-powiadomienia.json) to build 7.13: tylko powiadomienia, bez kampanii i bez migracji.

## Skill

- [SKILL.md](../.cursor/skills/wybor-dnia-testow/SKILL.md) to stanowisko, kontekst aplikacji, co skill dostaje, jak układa priorytet i co oddaje.

## Reszta zadania

- [CONTEXT.md](../CONTEXT.md) to treść zadania rekrutacyjnego.
- [czesc-A.md](../czesc-A.md) to diagnoza czterech obserwacji i decyzja o wydaniu.
- [czesc-B.md](../czesc-B.md) to plan testów na jeden dzień, część B1.
- [czesc-C.md](../czesc-C.md) to plan 30 dni: dwa usprawnienia, wskaźniki i odróżnienie wcześniejszego łapania błędów od gorszego kodu.
- [prompty.md](../prompty.md) to prompty, konfiguracja skilla i nota o pracy z AI.
