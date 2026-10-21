# Cenflix — porównywarka kosztu oglądania seriali
## Wymagania biznesowe — wersja user-centered / cost-of-watching

**Wersja:** 0.2  
**Status:** Draft  
**Główny cel:** pozwolić użytkownikowi szybko ustalić, gdzie i za ile najtaniej obejrzy wybrany serial, w wymaganej jakości i wersji językowej, przy jego własnym tempie oglądania  
**Główne źródła danych:** katalog seriali, dostępność seriali na platformach, cenniki i plany platform, preferencje użytkownika

> **Założenia robocze (do potwierdzenia, patrz rozdz. 27):** rynek docelowy w MVP to Polska, ceny w PLN brutto; produkt obejmuje seriale (filmy poza MVP); użytkownik może korzystać z serwisu bez konta; dane o cenach i dostępności są utrzymywane przez zespół Cenflix (ręcznie lub półautomatycznie).

---

## 1. Cel projektu

Celem projektu jest stworzenie strony internetowej, na której użytkownik wybiera serial, a system pokazuje mu:

- na jakich platformach serial jest dostępny;
- w jakich planach subskrypcji (lub w modelu zakupu / wypożyczenia);
- w jakiej jakości obrazu i dźwięku;
- w jakich wersjach językowych (dubbing, lektor, napisy);
- **ile łącznie zapłaci za obejrzenie całego serialu (lub wybranych sezonów)** przy swoim tempie oglądania.

Kluczowa różnica względem zwykłego porównania cenników: Cenflix nie porównuje tylko ceny miesięcznej abonamentu, lecz **całkowity koszt obejrzenia konkretnego serialu** wynikający z:

- długości serialu (liczba odcinków, czas trwania);
- planu oglądania użytkownika (binging, codziennie, raz w tygodniu, własny);
- okresu rozliczeniowego platformy (miesiąc, rok, inny);
- wymagań użytkownika (jakość, wersja językowa, reklamy, liczba ekranów).

**Domyślne zachowanie:** po wybraniu serialu system od razu pokazuje **najtańszy możliwy plan**, bez filtrów jakości i języka, przy profilu oglądania **binging**. Użytkownik może następnie zmieniać kategorie (jakość, wersja językowa, reklamy, profil oglądania), a wynik przelicza się na bieżąco.

**Kluczowym elementem produktu jest plan oglądania (harmonogram).** System układa harmonogram oglądania odcinków w czasie, nakłada go na cykle rozliczeniowe platformy i na tej podstawie wylicza **cenę finalną**.

Przykład:

> Serial ma 3 sezony po 10 odcinków po 50 minut. Użytkownik ogląda 1 odcinek dziennie. Potrzebuje ok. 30 dni, czyli jednego okresu rozliczeniowego. Przy oglądaniu 1 odcinka tygodniowo potrzebowałby ok. 7 miesięcy — wtedy korzystniejszy może być inny plan lub inna platforma.

---

## 2. Główna zasada produktu

Platforma jest **user-centered** i **cost-of-watching centered**.

Centralnym obiektem analizy jest **serial w kontekście preferencji i tempa oglądania konkretnego użytkownika**.

Oznacza to, że:

- domyślnym wynikiem jest najtańszy możliwy plan przy profilu binging i bez dodatkowych filtrów — użytkownik dostaje odpowiedź bez konfigurowania czegokolwiek;
- każda zmiana kategorii lub profilu oglądania przelicza harmonogram i cenę finalną;
- wynik porównania zależy od preferencji użytkownika, a nie wyłącznie od ceny katalogowej;
- domyślne sortowanie wyników odbywa się według całkowitego kosztu obejrzenia przy spełnieniu wymagań użytkownika;
- wymagania użytkownika (np. „tylko z dubbingiem”, „minimum 4K”) są filtrem twardym — oferta, która ich nie spełnia, nie może być prezentowana jako pasująca;
- użytkownik musi zawsze widzieć, jak wyliczono koszt (przejrzystość kalkulacji);
- ranking wyników nie może być zmieniany przez współpracę komercyjną z platformą; treści sponsorowane muszą być wyraźnie oznaczone;
- dane muszą posiadać informację o dacie ostatniej weryfikacji, bo ceny i katalogi platform często się zmieniają.

---

## 3. Główne zastosowania biznesowe

Platforma ma wspierać przede wszystkim:

1. wyszukanie serialu i sprawdzenie, gdzie jest dostępny;
2. porównanie platform i planów pod kątem ceny;
3. wyliczenie całkowitego kosztu obejrzenia serialu przy wybranym tempie oglądania;
4. filtrowanie ofert według jakości obrazu i dźwięku;
5. filtrowanie ofert według dostępnych wersji językowych (dubbing, lektor, napisy);
6. filtrowanie według dodatkowych cech planu (reklamy, liczba ekranów, pobieranie offline);
7. dobranie najkorzystniejszego planu subskrypcji do stylu oglądania;
8. pokazanie użytkownikowi, czy opłaca się subskrybować tylko na czas oglądania i potem zrezygnować;
9. porównanie subskrypcji z zakupem / wypożyczeniem odcinków lub sezonów (jeżeli dostępne);
10. przekierowanie użytkownika na stronę wybranej platformy;
11. budowanie zaufania poprzez aktualne i weryfikowalne dane;
12. analizowanie zachowań użytkowników (popularne seriale, wybierane preferencje) na potrzeby rozwoju produktu.

---

## 4. Główni użytkownicy

W systemie występują co najmniej następujące role:

- **Gość** — niezalogowany użytkownik korzystający z wyszukiwarki i kalkulatora;
- **Użytkownik zarejestrowany** — opcjonalnie; może zapisać preferencje, listę seriali do obejrzenia i alerty cenowe (poza MVP, do decyzji);
- **Redaktor treści** — utrzymuje dane o serialach, dostępności, cenach i planach;
- **Administrator** — zarządza platformą, użytkownikami wewnętrznymi i konfiguracją.

Dodatkowo w procesie występują:

- **Platformy streamingowe** — źródło danych o katalogu i cenach; potencjalni partnerzy afiliacyjni;
- **Zewnętrzni dostawcy danych** — opcjonalne źródło danych katalogowych (do decyzji).

---

## 5. Główne obiekty biznesowe

Platforma powinna operować co najmniej na następujących obiektach:

- serial,
- sezon,
- odcinek (z czasem trwania i datą premiery),
- platforma streamingowa,
- plan subskrypcji,
- cena planu (z okresem rozliczeniowym i historią zmian),
- oferta zakupu / wypożyczenia (odcinka lub sezonu),
- dostępność serialu na platformie (serial × platforma × region × okres dostępności),
- wersja językowa (dubbing, lektor, napisy — w danym języku),
- jakość (rozdzielczość, HDR, format dźwięku),
- cechy planu (reklamy, liczba jednoczesnych ekranów, pobieranie offline),
- profil oglądania (binging, daily, weekly, własny),
- preferencje użytkownika,
- wynik kalkulacji (koszt obejrzenia),
- przekierowanie do platformy (kliknięcie wyjściowe),
- źródło i data weryfikacji danych.

Centralnym obiektem analitycznym pozostaje **serial w kontekście preferencji i tempa oglądania użytkownika**.

---

# 6. Katalog seriali

### BR-001 — Wyszukiwanie serialu

Użytkownik powinien móc wyszukać serial po tytule (polskim i oryginalnym), z podpowiedziami w trakcie wpisywania.

### BR-002 — Karta serialu

Każdy serial powinien posiadać kartę zawierającą co najmniej:

- tytuł polski i oryginalny;
- plakat / grafikę (zgodnie z prawami do wykorzystania);
- krótki opis;
- gatunek;
- rok premiery;
- liczbę sezonów i odcinków;
- łączny czas trwania;
- status serialu (zakończony / w emisji / zawieszony);
- listę platform, na których jest dostępny.

### BR-003 — Struktura sezonów i odcinków

System powinien przechowywać strukturę serialu: sezony, odcinki, czas trwania odcinków.

Jest to niezbędne do wyliczenia czasu oglądania.

### BR-004 — Seriale w emisji

Dla seriali w emisji system powinien znać harmonogram publikacji odcinków (np. cały sezon naraz lub 1 odcinek tygodniowo), ponieważ wpływa to na minimalny czas oglądania i koszt.

### BR-005 — Przeglądanie katalogu

Użytkownik powinien móc przeglądać seriale według kategorii, np.: gatunek, platforma, nowości, popularne.

---

# 7. Platformy i plany subskrypcji

### BR-006 — Platforma

System powinien przechowywać listę platform streamingowych dostępnych na rynku docelowym.

### BR-007 — Plany

Każda platforma może posiadać wiele planów subskrypcji.

### BR-008 — Atrybuty planu

Plan powinien posiadać co najmniej:

- nazwę;
- cenę;
- walutę;
- okres rozliczeniowy (miesięczny, roczny, inny);
- maksymalną jakość obrazu (SD / HD / Full HD / 4K);
- obsługę HDR / Dolby Vision (jeżeli dotyczy);
- format dźwięku (np. stereo, 5.1, Dolby Atmos);
- informację o reklamach;
- liczbę jednoczesnych ekranów;
- możliwość pobierania offline;
- warunki rezygnacji (np. możliwość anulowania w dowolnym momencie).

### BR-009 — Pakiety i oferty łączone

System powinien umożliwiać zapisanie ofert łączonych (np. platforma w pakiecie z innym usługodawcą), jeżeli zostaną włączone do zakresu.

W MVP może to być wyłącznie informacja tekstowa, bez wliczania do kalkulacji (do decyzji).

### BR-010 — Promocje i okresy próbne

System powinien umożliwiać zapisanie promocji i okresów próbnych z datą obowiązywania.

Promocje nie mogą być domyślnie wliczane do kalkulacji jako cena standardowa; użytkownik powinien widzieć je osobno.

### BR-011 — Zakup i wypożyczenie

System powinien umożliwiać zapisanie ofert transakcyjnych (zakup / wypożyczenie odcinka lub sezonu), jeżeli serial jest tak dostępny.

### BR-012 — Historia cen

Zmiana ceny planu nie może nadpisywać historii; system powinien przechowywać historię cen z datami obowiązywania.

---

# 8. Dostępność serialu na platformie

### BR-013 — Dostępność

System powinien przechowywać, na których platformach i w jakich planach dostępny jest dany serial.

### BR-014 — Dostępność częściowa

Dostępność może dotyczyć tylko wybranych sezonów. System musi to rozróżniać i pokazywać użytkownikowi.

### BR-015 — Dostępność w planie

Serial może być dostępny tylko w części planów danej platformy (np. bez reklam / z reklamami, plan z dodatkiem). System musi to uwzględniać.

### BR-016 — Okres dostępności

Jeżeli znana jest data, od której lub do której serial jest dostępny na platformie, system powinien ją przechowywać i pokazywać (np. „znika z platformy za 14 dni”).

### BR-017 — Region

Dostępność i ceny są przypisane do regionu (kraju). W MVP obsługiwany jest jeden region (założenie: Polska).

---

# 9. Wersje językowe

### BR-018 — Rodzaje wersji językowych

System powinien rozróżniać:

- **dubbing** — w danym języku;
- **lektor** — w danym języku;
- **napisy** — w danym języku;
- **wersja oryginalna**.

### BR-019 — Wersja językowa na poziomie platformy

Wersje językowe mogą różnić się pomiędzy platformami dla tego samego serialu. System musi przechowywać je dla pary serial × platforma.

### BR-020 — Wersja językowa na poziomie sezonu

Jeżeli wersja językowa jest dostępna tylko dla części sezonów, system powinien to rozróżniać.

### BR-021 — Filtr językowy

Użytkownik powinien móc wskazać wymaganą wersję językową (np. „tylko z lektorem PL” lub „dubbing PL albo lektor PL”).

Oferty niespełniające wymagania nie mogą być pokazywane jako pasujące.

### BR-022 — Brak informacji ≠ brak wersji

System musi rozróżniać:

- wersja dostępna;
- wersja niedostępna;
- brak informacji.

Brak informacji nie może być prezentowany jako potwierdzona dostępność ani jako potwierdzony brak.

---

# 10. Jakość

### BR-023 — Jakość obrazu i dźwięku

System powinien przechowywać maksymalną jakość obrazu i dźwięku dostępną dla serialu na platformie.

### BR-024 — Efektywna jakość

Jakość, którą użytkownik faktycznie otrzyma, jest **niższą z dwóch wartości**:

- jakości, w jakiej serial jest udostępniony na platformie;
- maksymalnej jakości oferowanej przez wybrany plan.

Przykład:

> Serial jest dostępny w 4K, ale plan podstawowy oferuje maksymalnie Full HD — użytkownik otrzyma Full HD.

### BR-025 — Filtr jakości

Użytkownik powinien móc wskazać minimalną wymaganą jakość (np. „minimum 4K”, „HDR wymagany”).

---

# 11. Profil oglądania

### BR-026 — Wybór profilu

Użytkownik powinien móc wybrać profil oglądania. Domyślne profile:

- **Binging** — oglądanie wielu odcinków dziennie (np. X godzin dziennie);
- **Daily** — 1 (lub X) odcinek dziennie;
- **Weekly** — 1 (lub X) odcinek tygodniowo;
- **Weekendowy** — X odcinków w każdy weekend;
- **Własny** — użytkownik definiuje liczbę odcinków lub godzin w wybranym okresie.

**Profilem domyślnym jest Binging.** Użytkownik może go zmienić w dowolnym momencie.

Dokładne wartości liczbowe profili (np. ile godzin dziennie oznacza binging) pozostają do ustalenia.

### BR-026A — Domyślne kategorie

Przy pierwszym wyświetleniu wyników:

- profil oglądania = binging;
- zakres = cały serial (wszystkie dostępne sezony);
- data rozpoczęcia = dziś;
- brak wymagań co do jakości, wersji językowej i reklam;
- wynik = najtańszy możliwy plan spośród wszystkich ofert.

Zmiana dowolnej kategorii zawęża lub zmienia wynik i natychmiast przelicza cenę finalną.

### BR-027 — Zakres do obejrzenia

Użytkownik powinien móc wybrać, co chce obejrzeć:

- cały serial;
- wybrane sezony;
- od wybranego odcinka do końca.

### BR-028 — Data rozpoczęcia

Użytkownik może wskazać datę rozpoczęcia oglądania. Domyślnie: dziś.

### BR-029 — Wyliczenie czasu oglądania

Na podstawie zakresu, długości odcinków, profilu oglądania i harmonogramu publikacji system wylicza:

- przewidywaną liczbę dni oglądania;
- przewidywaną datę zakończenia;
- liczbę okresów rozliczeniowych potrzebnych na każdej platformie.

### BR-030 — Ograniczenie harmonogramu publikacji

Dla seriali w emisji tempo oglądania nie może być szybsze niż tempo publikacji odcinków (binging nie skraca czasu, jeżeli odcinki wychodzą co tydzień).

---

# 11A. Plan oglądania (harmonogram) — kluczowy moduł

### BR-076 — Generowanie harmonogramu

Na podstawie profilu oglądania, zakresu, długości odcinków, daty rozpoczęcia i harmonogramu publikacji system układa **plan oglądania**: listę odcinków przypisanych do konkretnych dni.

### BR-077 — Nałożenie na cykle rozliczeniowe

Harmonogram jest nakładany na cykle rozliczeniowe danej platformy i planu. Cykl rozpoczyna się w dniu wykupienia subskrypcji (zakładany dzień pierwszego odcinka), chyba że platforma rozlicza się inaczej.

### BR-078 — Liczba cykli

Liczba płatnych cykli = liczba cykli, w których przypada co najmniej jeden zaplanowany odcinek (zgodnie z RB-004 — rozpoczęty cykl jest płatny w całości).

### BR-079 — Cena finalna

Cena finalna = suma opłat za wszystkie cykle potrzebne do zrealizowania planu oglądania (z uwzględnieniem zmian ceny w trakcie, jeżeli są znane).

### BR-080 — Wizualizacja planu

Użytkownik powinien widzieć plan oglądania w czytelnej formie (np. oś czasu / kalendarz), na której zaznaczone są:

- dni oglądania i odcinki;
- granice cykli rozliczeniowych;
- opłata za każdy cykl;
- data zakończenia oglądania;
- zalecana data rezygnacji z subskrypcji;
- cena finalna.

### BR-081 — Plan dla każdej oferty

Harmonogram i cena finalna są wyliczane osobno dla każdej porównywanej oferty, ponieważ platformy mogą mieć różne okresy rozliczeniowe, różne sezony i różne harmonogramy publikacji.

### BR-082 — Edycja planu

Użytkownik powinien móc modyfikować plan oglądania, np.:

- zmienić profil (binging / daily / weekly / weekendowy / własny);
- zmienić liczbę odcinków na sesję;
- wskazać dni tygodnia, w które ogląda (do decyzji, czy w MVP);
- zmienić datę rozpoczęcia.

Każda zmiana przelicza harmonogram, liczbę cykli i cenę finalną.

### BR-083 — Sugestia oszczędności

Jeżeli niewielka zmiana planu (np. obejrzenie 2 ostatnich odcinków wcześniej, przesunięcie startu) pozwala zmieścić się w mniejszej liczbie cykli, system może to zasugerować, np.:

> „Oglądając 2 odcinki tygodniowo zamiast 1, zmieścisz się w 2 cyklach i zapłacisz 30 zł mniej.”

Sugestia jest wyłącznie informacją — nie zmienia planu bez decyzji użytkownika.

### BR-084 — Przerwy w subskrypcji

Jeżeli w harmonogramie występuje długa przerwa bez odcinków (np. przerwa między połowami sezonu w emisji), system może wyliczyć wariant z rezygnacją i ponownym wykupieniem subskrypcji (do decyzji, czy w MVP).

### Przykład

> Serial: 10 odcinków, wszystkie dostępne. Profil: weekly (1 odcinek / tydzień). Start: 21.10.
> Odcinki przypadają na: 21.10, 28.10, … , 23.12 (10 tygodni).
> Cykle miesięczne: 21.10–20.11, 21.11–20.12, 21.12–20.01 → **3 cykle**.
> Cena finalna: 3 × cena miesięczna planu.
> Ten sam serial przy bingingu: **1 cykl** → 1 × cena miesięczna.

---

# 12. Kalkulacja kosztu

### BR-031 — Całkowity koszt obejrzenia

Dla każdej pasującej oferty system wylicza całkowity koszt obejrzenia wybranego zakresu serialu.

### BR-032 — Założenie rezygnacji

Kalkulacja domyślnie zakłada, że użytkownik:

- wykupuje subskrypcję na czas oglądania;
- rezygnuje po zakończeniu oglądania (o ile plan na to pozwala).

Użytkownik powinien widzieć to założenie i móc je zmienić (np. „mam już tę platformę” → koszt dodatkowy 0).

### BR-033 — Platformy posiadane przez użytkownika

Użytkownik powinien móc wskazać platformy, które już opłaca. Dla nich koszt dodatkowy wynosi 0, a oferta jest wyróżniona.

### BR-034 — Porównanie okresów rozliczeniowych

Jeżeli platforma oferuje plan miesięczny i roczny, system powinien wyliczyć, który jest korzystniejszy przy danym czasie oglądania.

### BR-035 — Subskrypcja vs zakup

Jeżeli istnieje oferta transakcyjna, system powinien porównać ją z subskrypcją.

### BR-036 — Promocje osobno

Koszt z uwzględnieniem promocji lub okresu próbnego powinien być prezentowany osobno od kosztu standardowego, z informacją o warunkach promocji.

### BR-037 — Przejrzystość kalkulacji

Dla każdego wyniku użytkownik powinien móc zobaczyć rozbicie, np.:

> 30 odcinków × 50 min, 1 odcinek dziennie → 30 dni → 1 miesiąc × 49 zł = **49 zł**

### BR-038 — Brak fałszywej precyzji

Wynik jest **szacunkiem** opartym na deklarowanym tempie oglądania. System nie może prezentować go jako gwarantowanej kwoty.

### BR-039 — Koszt na godzinę

System może dodatkowo pokazywać koszt w przeliczeniu na godzinę oglądania jako wskaźnik pomocniczy.

---

# 13. Wyniki porównania

### BR-040 — Lista wyników

Po wybraniu serialu i preferencji użytkownik widzi listę ofert (platforma + plan) spełniających jego wymagania.

### BR-041 — Zawartość wyniku

Każdy wynik powinien zawierać co najmniej:

- platformę i plan;
- całkowity szacowany koszt;
- liczbę okresów rozliczeniowych;
- efektywną jakość;
- dostępne wersje językowe;
- informację o reklamach;
- informację o dostępności częściowej (jeżeli dotyczy);
- datę ostatniej weryfikacji danych;
- przycisk przejścia do platformy.

### BR-042 — Sortowanie

Domyślne sortowanie: rosnąco według całkowitego kosztu. Użytkownik może zmienić sortowanie (np. jakość, cena miesięczna).

### BR-043 — Rekomendacja

System może wyróżnić ofertę „najkorzystniejszą dla Ciebie”, wyłącznie na podstawie jawnych kryteriów (koszt przy spełnionych wymaganiach).

### BR-044 — Oferty niepasujące

Oferty, które nie spełniają wymagań użytkownika, mogą być pokazane osobno z informacją, czego nie spełniają (np. „brak lektora PL”).

### BR-045 — Brak wyników

Jeżeli żadna oferta nie spełnia wymagań, system powinien to jasno komunikować i zasugerować, które wymaganie poluzować.

### BR-046 — Serial niedostępny

Jeżeli serial nie jest dostępny legalnie na żadnej platformie w regionie, system powinien to jasno komunikować.

---

# 14. Konto użytkownika (opcjonalne)

### BR-047 — Korzystanie bez konta

Wyszukiwanie, filtrowanie i kalkulacja muszą działać bez zakładania konta.

### BR-048 — Funkcje konta

Po zalogowaniu użytkownik może (zakres do decyzji, częściowo poza MVP):

- zapisać domyślne preferencje (język, jakość, profil oglądania);
- zapisać posiadane platformy;
- tworzyć listę seriali do obejrzenia;
- otrzymywać powiadomienia o zmianie ceny lub dostępności.

### BR-049 — Zapamiętanie preferencji bez konta

Preferencje gościa mogą być zapamiętane lokalnie w przeglądarce.

---

# 15. Zarządzanie danymi (panel redakcyjny)

### BR-050 — Panel redakcyjny

Redaktor powinien mieć panel do dodawania i edycji seriali, platform, planów, cen, dostępności i wersji językowych.

### BR-051 — Data weryfikacji

Każdy rekord ceny i dostępności powinien mieć datę ostatniej weryfikacji oraz źródło.

### BR-052 — Dane przeterminowane

System powinien oznaczać dane niezweryfikowane dłużej niż określony czas (np. 30 dni) i informować redakcję.

### BR-053 — Zgłoszenie błędu

Użytkownik powinien móc zgłosić nieaktualną cenę lub dostępność z poziomu wyniku.

### BR-054 — Import danych

System powinien być przygotowany na import danych z zewnętrznych źródeł (API dostawców, pliki), jeżeli zostanie wybrany taki model.

---

# 16. Role i uprawnienia

## 16.1. Gość

### BR-055 — Zakres gościa

Gość może wyszukiwać seriale, ustawiać preferencje, przeglądać wyniki, korzystać z kalkulatora, przechodzić do platform i zgłaszać błędy.

## 16.2. Użytkownik zarejestrowany

### BR-056 — Zakres użytkownika

Jak gość, a dodatkowo zapisane preferencje, posiadane platformy, lista do obejrzenia i powiadomienia (zgodnie z decyzją o zakresie).

## 16.3. Redaktor treści

### BR-057 — Zakres redaktora

Redaktor może zarządzać katalogiem seriali, platformami, planami, cenami, dostępnością i wersjami językowymi oraz obsługiwać zgłoszenia błędów.

## 16.4. Administrator

### BR-058 — Zakres administratora

Administrator zarządza kontami wewnętrznymi, rolami, konfiguracją (np. domyślne profile oglądania, próg przeterminowania danych), oznaczeniami treści sponsorowanych i ma dostęp do statystyk.

### BR-059 — Audyt zmian

Zmiany cen i dostępności powinny być możliwe do audytu (kto, kiedy, wartość poprzednia, wartość nowa).

---

# 17. Model biznesowy i neutralność

### BR-060 — Źródło przychodu

Model przychodu pozostaje do ustalenia (np. linki afiliacyjne, reklamy, plan premium dla użytkownika).

### BR-061 — Neutralność rankingu

Kolejność wyników wynika wyłącznie z kryteriów użytkownika. Współpraca komercyjna z platformą nie może zmieniać rankingu.

### BR-062 — Oznaczenie treści komercyjnych

Treści sponsorowane, reklamy i linki afiliacyjne muszą być wyraźnie oznaczone.

---

# 18. Reguły biznesowe

### RB-001
Wynik porównania jest zawsze liczony dla konkretnego serialu, zakresu, profilu oglądania i zestawu wymagań.

### RB-002
Oferta niespełniająca twardych wymagań użytkownika (język, jakość, reklamy) nie jest wynikiem pasującym.

### RB-003
Efektywna jakość = niższa z jakości udostępnienia serialu i maksymalnej jakości planu.

### RB-004
Liczba okresów rozliczeniowych jest zaokrąglana w górę (rozpoczęty okres jest płatny w całości), chyba że platforma rozlicza się inaczej.

### RB-005
Brak informacji o wersji językowej lub jakości nie jest równoznaczny z jej dostępnością ani brakiem.

### RB-006
Cena promocyjna nie zastępuje ceny standardowej w domyślnym wyniku.

### RB-007
Platforma oznaczona przez użytkownika jako posiadana ma koszt dodatkowy 0.

### RB-008
Tempo oglądania nie może przekroczyć tempa publikacji odcinków.

### RB-009
Każda cena jest przypisana do regionu, waluty i okresu obowiązywania.

### RB-010
Zmiana ceny nie usuwa historii cen.

### RB-011
Każdy wynik pokazuje datę ostatniej weryfikacji danych.

### RB-012
Ranking wyników nie zależy od relacji komercyjnych z platformami.

### RB-013
Domyślny wynik to najtańszy możliwy plan przy profilu binging, bez filtrów jakości, języka i reklam.

### RB-014
Cena finalna zawsze wynika z harmonogramu oglądania nałożonego na cykle rozliczeniowe — nie z samej ceny miesięcznej.

### RB-015
Każda zmiana kategorii lub planu oglądania powoduje ponowne wyliczenie harmonogramu i ceny finalnej.

---

# 19. Analityka produktowa

### BR-063 — Statystyki

Administrator powinien móc analizować co najmniej:

- najczęściej wyszukiwane seriale;
- najczęściej wybierane profile oglądania i wymagania językowe;
- liczbę przejść do poszczególnych platform;
- wyszukiwania bez wyników (seriale niedostępne lub brakujące w katalogu);
- liczbę zgłoszeń błędów i czas ich obsługi;
- odsetek danych przeterminowanych.

### BR-064 — Prywatność analityki

Analityka powinna być realizowana zgodnie z RODO i zgodą użytkownika na cookies.

---

# 20. Scenariusze użycia

### UC-001 — Najtańszy sposób obejrzenia serialu

> Użytkownik wpisuje tytuł serialu, wybiera profil „Daily” i wymaganie „lektor PL”. Widzi listę platform posortowaną według łącznego kosztu, z rozbiciem kalkulacji.

### UC-002 — Binging w weekend

> Użytkownik chce obejrzeć sezon w jeden weekend. System pokazuje, że wystarczy jeden okres rozliczeniowy, i wskazuje najtańszy plan spełniający wymagania.

### UC-003 — Wymagana jakość

> Użytkownik wybiera „minimum 4K”. System pomija plany podstawowe i pokazuje tylko plany, w których serial jest faktycznie dostępny w 4K.

### UC-004 — Mam już platformę

> Użytkownik zaznacza, że ma już platformę A. Jeżeli serial jest tam dostępny, system pokazuje koszt dodatkowy 0 zł i wyróżnia tę opcję.

### UC-005 — Serial w emisji

> Serial publikuje odcinki co tydzień. Użytkownik wybiera binging, ale system informuje, że oglądanie potrwa co najmniej do daty ostatniego odcinka, i liczy koszt odpowiednio.

### UC-006 — Brak wersji językowej

> Użytkownik wymaga dubbingu PL. Żadna platforma go nie oferuje. System informuje o tym i podpowiada, że z lektorem PL serial jest dostępny na dwóch platformach.

### UC-007 — Zgłoszenie nieaktualnej ceny

> Użytkownik zauważa, że cena planu jest inna niż na stronie platformy, i zgłasza błąd. Redaktor weryfikuje i aktualizuje cenę.

### UC-008 — Wynik domyślny

> Użytkownik wpisuje tytuł i od razu, bez ustawiania niczego, widzi najtańszy plan dla bingingu wraz z ceną finalną. Dopiero potem zaznacza „lektor PL” i wynik się przelicza.

### UC-009 — Serial oglądany weekly przez kilka cykli

> Użytkownik zmienia profil na weekly. Serial 10-odcinkowy rozkłada się na 10 tygodni. System pokazuje na osi czasu 3 cykle rozliczeniowe, opłatę za każdy z nich i cenę finalną, a także podpowiada, że przy 2 odcinkach tygodniowo wystarczą 2 cykle.

---

# 21. Jakość danych

### BR-065 — Kompletność

System powinien pokazywać redakcji, które seriale mają niekompletne dane (brak długości odcinków, wersji językowych, jakości).

### BR-066 — Źródło i data

Każdy istotny rekord posiada źródło, czas utworzenia, ostatniej modyfikacji i ostatniej weryfikacji.

### BR-067 — Rozróżnienie wartości

System musi odróżniać:

- cena 0 (np. w pakiecie / darmowe);
- brak ceny;
- dostępność potwierdzona;
- niedostępność potwierdzona;
- brak informacji.

---

# 22. Powiadomienia (poza MVP / do decyzji)

### BR-068 — Alert cenowy

Zarejestrowany użytkownik może otrzymać powiadomienie o obniżce ceny lub promocji dla serialu z listy.

### BR-069 — Alert dostępności

Użytkownik może otrzymać powiadomienie, gdy serial pojawi się na platformie lub ma z niej zniknąć.

### BR-070 — Przypomnienie o rezygnacji

System może przypomnieć użytkownikowi o planowanym końcu oglądania i możliwości rezygnacji z subskrypcji.

---

# 23. Prywatność, prawo i bezpieczeństwo

### BR-071 — Minimalizacja danych

System zbiera wyłącznie dane niezbędne do działania.

### BR-072 — RODO i cookies

Serwis musi działać zgodnie z RODO, w tym z zarządzaniem zgodami na cookies.

### BR-073 — Prawa do materiałów

Grafiki, opisy i logotypy muszą być wykorzystywane zgodnie z licencją lub zasadami źródła danych.

### BR-074 — Tylko legalne źródła

Serwis prezentuje wyłącznie legalne, licencjonowane źródła oglądania.

### BR-075 — Informacja o charakterze danych

Serwis musi informować, że ceny mają charakter informacyjny, a ostateczne warunki określa platforma.

---

# 24. Zakres MVP

W pierwszej wersji priorytetem jest możliwość:

1. wyszukania serialu po tytule;
2. wyświetlenia karty serialu;
3. wyświetlenia platform i planów, na których serial jest dostępny;
4. ustawienia wymagań językowych (dubbing / lektor / napisy);
5. ustawienia minimalnej jakości;
6. ustawienia wymagania „bez reklam”;
7. wyboru profilu oglądania (binging / daily / weekly / własny);
8. wyboru zakresu (cały serial / sezony);
9. wyświetlenia domyślnego wyniku (najtańszy plan, binging) bez konfiguracji;
10. wygenerowania planu oglądania i nałożenia go na cykle rozliczeniowe;
11. wizualizacji planu oglądania z granicami cykli i ceną finalną;
12. wyliczenia ceny finalnej z rozbiciem na cykle;
13. wskazania posiadanych platform;
14. sortowania wyników według kosztu;
15. przejścia na stronę platformy;
16. wyświetlenia daty weryfikacji danych;
17. zgłoszenia nieaktualnych danych;
18. panelu redakcyjnego do zarządzania danymi;
19. obsługi jednego regionu (PL) i jednej waluty (PLN);
20. działania na urządzeniach mobilnych i desktopie.

---

# 25. Poza zakresem pierwszego MVP

- filmy (tylko seriale w MVP);
- wiele regionów i walut;
- porównanie kosztu dla listy wielu seriali jednocześnie (optymalizacja rotacji subskrypcji);
- konta użytkowników z listą do obejrzenia;
- alerty cenowe i dostępności;
- oferty łączone z operatorami (telewizja, telefon, internet) w kalkulacji;
- rekomendacje seriali na podstawie gustu;
- recenzje i oceny użytkowników;
- aplikacja mobilna natywna;
- automatyczny scraping cenników (do decyzji);
- integracje z kontami użytkownika na platformach;
- eksport planu oglądania do kalendarza (Google / iCal);
- współdzielenie kosztów konta (rodzina / znajomi).

---

# 26. Kryteria akceptacji MVP

### AC-001
Użytkownik może wyszukać serial po tytule polskim lub oryginalnym.

### AC-002
Karta serialu pokazuje liczbę sezonów, odcinków i łączny czas trwania.

### AC-003
Dla serialu wyświetlana jest lista platform i planów, na których jest dostępny.

### AC-004
Użytkownik może wymagać dubbingu, lektora lub napisów w wybranym języku.

### AC-005
Oferty bez wymaganej wersji językowej nie są pokazywane jako pasujące.

### AC-006
Użytkownik może wymagać minimalnej jakości; system stosuje regułę efektywnej jakości.

### AC-007
Użytkownik może wybrać profil oglądania i zakres serialu.

### AC-008
System wylicza liczbę dni oglądania i liczbę okresów rozliczeniowych.

### AC-009
System wylicza całkowity koszt dla każdej pasującej oferty i pokazuje rozbicie kalkulacji.

### AC-010
Wyniki są domyślnie posortowane rosnąco według całkowitego kosztu.

### AC-011
Platforma oznaczona jako posiadana ma koszt dodatkowy 0.

### AC-012
Dla seriali w emisji czas oglądania uwzględnia harmonogram publikacji.

### AC-013
Każdy wynik pokazuje datę ostatniej weryfikacji danych.

### AC-014
Brak informacji o wersji językowej / jakości jest odróżniany od potwierdzonego braku.

### AC-015
Użytkownik może przejść na stronę wybranej platformy.

### AC-016
Użytkownik może zgłosić nieaktualne dane.

### AC-017
Redaktor może dodać i edytować serial, platformę, plan, cenę i dostępność.

### AC-018
Zmiana ceny zachowuje historię cen.

### AC-019
Treści sponsorowane są wyraźnie oznaczone i nie wpływają na ranking.

### AC-020
Serwis działa bez konieczności zakładania konta.

### AC-021
Po wybraniu serialu, bez ustawiania kategorii, użytkownik widzi najtańszy możliwy plan dla profilu binging.

### AC-022
System generuje plan oglądania (odcinki przypisane do dni) dla wybranego profilu.

### AC-023
Plan oglądania jest nakładany na cykle rozliczeniowe; użytkownik widzi granice cykli, opłatę za każdy cykl i cenę finalną.

### AC-024
Dla serialu 10-odcinkowego oglądanego weekly (1 odcinek / tydzień) od 21.10 przy planie miesięcznym system wylicza 3 cykle i cenę finalną = 3 × cena miesięczna.

### AC-025
Zmiana kategorii lub profilu oglądania przelicza harmonogram i cenę finalną bez przeładowania strony.

---

# 27. Kluczowe pytania otwarte

1. Jaki jest rynek docelowy — tylko Polska, czy od początku kilka krajów?
2. Czy w MVP tylko seriale, czy również filmy?
3. Skąd pochodzą dane o cenach i dostępności — ręczna redakcja, zewnętrzny dostawca (API), scraping, umowy z platformami?
4. Jak często dane muszą być weryfikowane?
5. Jaki jest model przychodu — afiliacja, reklamy, plan premium, brak (projekt hobbystyczny / portfolio)?
6. Które platformy wchodzą do MVP?
7. Jakie dokładne wartości mają profile (np. binging = ile godzin / odcinków dziennie)? *(Ustalone: binging jest profilem domyślnym.)*
8. Czy profil „własny” definiujemy w odcinkach, czy w godzinach?
9. Czy kalkulacja ma zakładać rezygnację po obejrzeniu, czy ciągłą subskrypcję?
10. Czy uwzględniamy okresy próbne w rekomendacji?
11. Czy uwzględniamy oferty łączone z operatorami (np. platforma w pakiecie z internetem / telefonem)?
12. Czy uwzględniamy zakup / wypożyczenie odcinków i sezonów?
13. Czy konto użytkownika jest w MVP?
14. Czy chcemy porównywać koszt dla wielu seriali naraz (np. „chcę obejrzeć te 5 seriali w pół roku — jak rotować subskrypcje?”)?
15. Czy pokazujemy historię cen użytkownikowi?
16. Czy wersje językowe zbieramy z dokładnością do sezonu, czy tylko do serialu?
17. Czy liczba ekranów / współdzielenie konta ma wpływać na kalkulację (koszt na osobę)?
18. Jakie języki interfejsu — tylko polski?
19. Czy informacja „znika z platformy za X dni” jest w MVP?
20. Czy serwis ma pokazywać seriale niedostępne w regionie (z informacją), czy je ukrywać?
21. Czy cykl rozliczeniowy liczymy od dnia wykupienia subskrypcji (rocznica), czy według kalendarza platformy?
22. Czy w planie oglądania użytkownik wybiera konkretne dni tygodnia (np. weekly = każda sobota)?
23. Czy sugestie oszczędności (BR-083) mają być w MVP?
24. Czy przy przerwach w harmonogramie liczymy wariant z rezygnacją i ponownym wykupieniem (BR-084)?

---

# 28. Rekomendowany model interpretacji wyniku

Najważniejszą zasadą jest oddzielenie:

1. **ceny planu** (cennik platformy);
2. **kosztu obejrzenia serialu** (cena × liczba okresów potrzebnych przy tempie użytkownika).

Cenflix prezentuje przede wszystkim punkt 2.

Przykład:

> Platforma A kosztuje 30 zł / mies., platforma B 45 zł / mies. Przy oglądaniu 1 odcinka tygodniowo serial zajmie 5 miesięcy na A (150 zł). Ten sam serial na B jest dostępny tylko w całości, ale przy bingingu zajmie 1 miesiąc (45 zł). Najtańsza opcja zależy od stylu oglądania, a nie od cennika.

To rozróżnienie powinno być konsekwentnie zachowane w interfejsie i komunikacji.

---

# 29. Definicja sukcesu projektu

Projekt osiąga podstawowy cel, jeżeli użytkownik w krótkim czasie (kilka kliknięć) dowiaduje się, gdzie i za ile najtaniej obejrzy wybrany serial w wymaganej wersji językowej i jakości, i ufa temu wynikowi.

Podstawowy scenariusz sukcesu:

> użytkownik wpisuje tytuł → od razu widzi najtańszy plan dla bingingu i cenę finalną → opcjonalnie zmienia kategorie (język, jakość, reklamy) i profil oglądania → widzi plan oglądania rozłożony na cykle rozliczeniowe i przeliczoną cenę finalną → przechodzi na stronę wybranej platformy.

Proponowane miary sukcesu:

- odsetek wyszukiwań zakończonych wyświetleniem wyników;
- odsetek wyników zakończonych przejściem do platformy;
- odsetek danych zweryfikowanych w ostatnich 30 dniach;
- liczba zgłoszeń błędnych danych w stosunku do liczby wyszukiwań;
- powracający użytkownicy.

Docelową wartością platformy jest zaufane, aktualne i spersonalizowane źródło odpowiedzi na pytanie: **„Gdzie i za ile najtaniej obejrzę ten serial tak, jak lubię?”**
