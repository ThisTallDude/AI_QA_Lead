# Część B — plan testów na jeden dzień

Testuję tylko to, co jest krytyczne: czy aplikacja wstaje, czy działa to, co niesie kampania, i czy migracja rozliczeń jest na właściwym poziomie. Jeden dzień. Trzy osoby na telefonach, dwie od automatyzacji. Automaty idą równolegle z matrycą na urządzeniach.

## Scenariusz 1. Wejście z kampanii

Czysty telefon, czyste konto. Założenie konta, instalacja aplikacji, sparowanie zegarka. To jest flow, który ludzie zrobią po reklamie.

Matryca: jeden telefon z Androidem i jeden z iOS. Ten sam przebieg na obu.

- Dane testowe: nowe konto na każdym telefonie, telefon bez aplikacji, zegarek nieprzypisany do innej rodziny.
- Oczekiwany wynik: konto powstaje, zegarek jest sparowany raz, onboarding kończy się za drugim etapem zgody i nie wraca na pierwszy ekran. Wynik jest taki sam na Androidzie i na iOS.
- Poziom: na urządzeniu.
- Środowisko: dwa fizyczne telefony, build kandydata, środowisko jak najbliższe produkcji. Konta testowe, bez danych klientów.

## Scenariusz 2. Link zapraszający, po poprawce

Po hotfixie linku. Nie tylko iOS. Ta sama matryca: telefon z Androidem i telefon z iOS. Na obu aplikacji nie ma, instalacja jest czysta.

- Dane testowe: konto z zaproszeniem drugiego opiekuna, dwa telefony bez tej aplikacji, link z tego zaproszenia.
- Oczekiwany wynik: na Androidzie i na iOS link prowadzi do instalacji, a po niej zaproszenie da się dokończyć.
- Poziom: na urządzeniu.
- Środowisko: te same dwa telefony i to samo środowisko co w scenariuszu 1, po wgraniu buildu z poprawką. Przed testem aplikacja jest odinstalowana.

## Scenariusz 3. Istniejący suite testów jednostkowych migracji

Puszcza go osoba od automatyzacji. Suite jest przygotowany wcześniej albo powstaje jako mały test tego dnia: czy rozliczenie konta po migracji jest spójne. Odpalenie idzie po kontach z migracji, około 60 000. Sam przebieg to minuty, nie 60 000 osobnych testów pisanych ręcznie.

- Dane testowe: działające parametry rozliczenia konta (stan przed migracją i stan po regule migracji). Bez danych klientów na zewnątrz.
- Oczekiwany wynik: konto ma jeden spójny stan rozliczenia. Suite nie wywala się na przejściu całego zestawu.
- Poziom: jednostkowy.
- Środowisko: GitLab CI, na danych migracji, nie na telefonie.

## Scenariusz 4. Smoke suite UI: onboarding i parowanie od zera

Niższy, przygotowany wcześniej suite automatyczny. Odpowiada na pytanie, czy aplikacja w ogóle przechodzi to, co robimy w kampanii: onboarding i sparowanie zegarka od czystego stanu. Jeden test albo krótka grupa, nie cała regresja.

- Dane testowe: czysty stan aplikacji, konto testowe, zegarek testowy. Scenariusze są już w packu automatów.
- Oczekiwany wynik: suite przechodzi. Onboarding dochodzi do końca, zegarek jest sparowany raz.
- Poziom: UI.
- Środowisko: GitLab CI, Maestro, na buildzie kandydata.

## Scenariusz 5. Parowanie zegarka po API

To samo parowanie, od strony API. Jest w kampanii i jest w aplikacji. Telefon ze scenariusza 1 pokazuje, czy człowiek da radę. Ten suite pokazuje, czy kontrakt parowania w ogóle zwraca jedno urządzenie.

- Dane testowe: konto testowe, identyfikator zegarka testowego, żądanie parowania.
- Oczekiwany wynik: parowanie kończy się jednym urządzeniem przy koncie. Drugie wywołanie nie dokłada duplikatu.
- Poziom: API.
- Środowisko: Newman, środowisko jak najbliższe produkcji, bez danych klientów.

## Czego świadomie nie testuję

Niczego poza rzeczami krytycznymi. Poza kampanią i migracją rozliczeń ten dzień nic innego nie bierze.

Najpierw to, co musi działać: aplikacja, flow z kampanii, migracja na właściwym poziomie. Dlatego matryca jest krótka, Android i iOS, a nie siatka starych urządzeń.

Nie testuję innych edge case'ów. Nie testuję pokrycia na starych urządzeniach.
