# Cześć

Dziękuję za czas poświęcony na rekrutację.

Poniżej jest moja odpowiedź. Brief, do którego się odnoszę, jest w [CONTEXT.md](CONTEXT.md).

## Część A

[czesc-A.md](czesc-A.md) to diagnoza czterech obserwacji i decyzja o wydaniu 7.12.

## Część B

[czesc-B.md](czesc-B.md) to plan testów na jeden dzień. Pięć scenariuszy, nic poza kampanią i migracją rozliczeń.

### B2

W [b2](b2/README.md) jest wybór pięciu przypadków na ten jeden dzień. Nie ma skryptu. Krok robi agent w Cursorze.

1. Otwórz to repozytorium w Cursorze.
2. W czacie wybierz skilla [wybor-testow-pod-zmiane](.cursor/skills/wybor-testow-pod-zmiane/SKILL.md).
3. Wskaż plik zmian. Do tego zadania są dwa:
   - [b2/mock-1.json](b2/mock-1.json) to build 7.12,
   - [b2/mock-2.json](b2/mock-2.json) to build tylko z powiadomieniami.
4. Katalog, z którego skill bierze przypadki, jest zawsze ten sam: [b2/test-case-scenarios.json](b2/test-case-scenarios.json).

Odpowiedź przychodzi na czacie: pięć przypadków, z uzasadnieniem, danymi i tym, czego w tym dniu świadomie nie ma.

Skill do tego zadania leży w [.cursor/skills/wybor-testow-pod-zmiane](.cursor/skills/wybor-testow-pod-zmiane/SKILL.md). Sam się nie włącza. Trzeba go wybrać i podać plik zmian.

### Status wydania

Drugi skill czyta wymyśloną odpowiedź MCP i mówi, na czym stoi wydanie. Też sam się nie włącza.

1. W czacie wybierz skilla [status-wydania](.cursor/skills/status-wydania/SKILL.md).
2. Zapytaj, jaki jest aktualny status.
3. Źródło jest w [b2/mock-mcp.json](b2/mock-mcp.json): cykl regresji w Zephyrze, taski QA, deweloperów i devops, estymacja wobec czasu spędzonego, bugi otwarte i rozwiązane.

Odpowiedź przychodzi na czacie: ile testów wykonano i ile czeka, ile godzin zostało rolom na domknięcie tasków oraz ile bugów zostało.

## Część C

[czesc-C.md](czesc-C.md) to plan na 30 dni: dwa usprawnienia, wskaźniki po 30 i 90 dniach, retrospektywa oraz jak odróżnić wcześniejsze łapanie błędów od gorszego kodu.

## Prompty

[prompty.md](prompty.md) to prompty w kolejności pracy, konfiguracja skilla i nota o tym, co powstało z AI, co sprawdziłem i co zmieniłem.

Dzięki,  
Matt
