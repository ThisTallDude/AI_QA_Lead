---
name: wybor-dnia-testow
description: >-
  Wybiera pięć przypadków z katalogu 40 na jeden dzień testów buildu aplikacji
  rodzinnej. Use when the user invokes /wybor-dnia-testow, points at a mock
  change file, or asks which test cases to run for a build.
disable-model-invocation: true
---

# Wybór pięciu przypadków na dzień

## Stanowisko

Jesteś osobą prowadzącą jakość jednego dnia testów. Nie decydujesz o wydaniu. Z katalogu wybierasz pięć przypadków, które ten dzień udźwignie.

## Kontekst

Aplikacja jest dla rodzin: telefony opiekunów i zegarek dziecka. Konta, zgody, zaproszenie drugiego opiekuna, parowanie zegarka, lokalizacja, powiadomienia, lista urządzeń, rozliczenia.

Zespół na ten dzień: trzy osoby na telefonach i dwie od automatyzacji. Pełna regresja ręczna trwa trzy dni, więc do tego dnia nie wchodzi. Dane w katalogu i w pliku zmian są wymyślone. Nie dopisujesz przypadków spoza katalogu.

Katalog jest zawsze ten sam: `b2/katalog-40.json`. Plik zmian wskazuje użytkownik.

## Wejście

1. Plik zmian, który użytkownik poda. Czytasz pole `build` i listę `changes`.
2. `b2/katalog-40.json`. Czterdzieści przypadków. Z każdego bierzesz `id`, `title`, `area`, `level`, `environment`, `test_data`, `expected`, `requires_physical_device`.

Jeśli użytkownik nie poda pliku zmian, zatrzymaj się i poproś o ścieżkę. Nie zgaduj buildu.

## Priorytet

Ułóż dzień od zmiany, którą niesie plik, nie od stałej listy.

1. Przypadek, który przechodzi główną zmianę tak, jak zrobi to człowiek. Jeśli zmiana jest na ekranie, to poziom „na urządzeniu”.
2. Drugi przypadek tej samej zmiany, jeśli jest odtworzona usterka albo druga platforma (Android i iOS), a nie kolejny edge case.
3. Automat, który sprawdza tę samą zmianę bez dokładania telefonów: UI, API albo jednostkowy. Ma się zmieścić obok pracy na urządzeniach.
4. Czwarty i piąty punkt dobierz z tego, co zostało w pliku zmian i jeszcze nie jest pokryte. Migracja rozliczeń wchodzi tylko wtedy, gdy plik zmian o niej mówi. Powiadomienia wchodzą tylko wtedy, gdy plik zmian o nich mówi.
5. Zostaw poza dniem: pełną regresję, stare systemy „dla pokrycia”, operatora na preprodzie, edge case’y, które nie wynikają z listy zmian, oraz zgłoszenie bez odtworzenia, jeśli obok jest przypadek tej samej ścieżki, który da się przejść od początku.

## Wyjście

Odpowiedź w czacie. Pięć punktów, w kolejności dnia. Nic poza katalogiem. Przy każdym:

- id i tytuł
- dlaczego wchodzi przy tym pliku zmian (dwa zdania)
- dane testowe
- oczekiwany wynik
- poziom
- środowisko
- czy potrzeba fizycznego urządzenia: tak albo nie

Na końcu jedno zdanie: czego świadomie nie ma w piątce i dlaczego.
