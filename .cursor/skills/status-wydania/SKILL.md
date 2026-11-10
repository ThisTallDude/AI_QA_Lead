---
name: status-wydania
description: >-
  Podaje aktualny status wydania z mocka MCP: wykonane testy Zephyra, czas
  jaki został QA i deweloperom, oraz liczbę bugów. Use when the user invokes
  /status-wydania or asks for the current release status.
disable-model-invocation: true
---

# Aktualny status wydania

## Stanowisko

Podajesz status jednego wydania tak, jak wraca z narzędzi zespołu. Nie decydujesz o wydaniu. Nie dopisujesz tasków, testów ani bugów.

## Kontekst

Dane są wymyślone. Źródło to jeden plik, który udaje odpowiedź MCP: Zephyr dla cyklu regresji, taski ról i bugi. Plik jest zawsze ten sam: `b2/mock-mcp.json`.

## Wejście

Przeczytaj `b2/mock-mcp.json`. Jeśli pliku nie ma, zatrzymaj się i napisz, że mocka MCP nie ma. Nie zgaduj liczb.

Z pliku bierz tylko:

- `release`
- `zephyr`: `cycle`, `total`, `executed`, `passed`, `failed`, `waiting`
- `bugs`: `open`, `resolved`, `open_items`
- `tasks`: `id`, `role`, `title`, `status`, `estimate_hours`, `spent_hours`, `remaining_hours`

Czas, który został, to suma `remaining_hours` tasków o statusie innym niż `skończone`, osobno dla `qa`, `deweloper` i `devops`. Nie licz tego drugi raz z estymacji i z godzin już spędzonych.

## Wyjście

Odpowiedź w czacie, po polsku. Najpierw zdanie: aktualny status wydania i numer z pola `release`. Potem trzy bloki, liczbami z pliku:

1. Zephyr. Nazwa cyklu. Ile testów wykonano z ilu jest w cyklu. Ile jeszcze czeka na egzekucję. Z wykonanych: ile zaliczonych i ile niezaliczonych.
2. Czas na domknięcie otwartych tasków. Osobno QA, deweloperzy i devops, w godzinach z sumy `remaining_hours`.
3. Bugi. Ile jest jeszcze do rozwiązania i ile jest już rozwiązanych. Pod spodem lista `open_items`.

Na końcu otwarte taski, czyli status `w toku` albo `nieskończone`: id, rola, tytuł, status, estymacja, czas spędzony, czas który został. Tasków skończonych w tej liście nie ma.
