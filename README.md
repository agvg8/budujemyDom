# budujemy - planer budowy/mieszkania/remontu

## Uruchomienie
1. pip install -r requirements.txt
2. python app.py
3. otwórz http://127.0.0.1:5000

WAŻNE: usuń domybudujesz.db przed uruchomieniem - struktura tabel się zmieniła
(nowe tabele Material, Task; nowa kolumna Item.will_change; inny domyślny
zestaw sekcji/pokoi).

## Co nowego w tej turze
- Typ projektu wpływa teraz na to, co się seeduje:
  - dom: pełne 5 sekcji (stan 0, surowy otwarty, surowy zamknięty, instalacje,
    wykończenie), instalacje = elektryczna+wodno-kanalizacyjna+ogrzewanie.
  - mieszkanie/remont: tylko 2 sekcje (instalacje, wykończenie), instalacje =
    tylko elektryczna+wodno-kanalizacyjna, bo reszta stanu budynku już istnieje.
- Instalacje w mieszkaniu/remoncie mają na starcie pytanie "czy będzie
  zmieniana?" (domyślnie nie). Jeśli przełączysz na "będzie zmieniana",
  podstrona dzieli się na 2 sekcje:
  - materiały: kafelki (nazwa, sklep, wycena, opis, link), edytowalne,
    usuwalne, z checkboxem "liczy się do budżetu" (3 kwadratowe przyciski:
    klucz/kosz/sakiewka, tak samo jak wszędzie indziej).
  - usługi: dokładnie ten sam mechanizm co w projekcie domu (materiał+
    robocizna, grupy, "wykonamy to sami") - budżet pozycji to materiały
    (zawsze sumowane) + widełki z usług (min-max alternatyw).
- Wykończenie: domyślne, stałe pokoje to teraz tylko strefa dzienna, kuchnia,
  łazienka (wcześniej było 5, w tym korytarz i sypialnia) - resztę dodaje
  użytkownik przez "+ dodaj więcej". Dotyczy to każdego typu projektu.
- Każdy pokój w wykończeniu ma teraz 4 sekcje: wykonawcy (jak wcześniej -
  usługi z tagami), zadania (prosta lista do zrobienia z checkboxem),
  produkty (kafelki materiałów - ten sam mechanizm co materiały w
  instalacjach), inspiracje (bez zmian).

## Model danych (dodatki)
- Item.will_change: None = nie dotyczy (dom, pokoje), False/True = instalacje
  w mieszkaniu/remoncie.
- Material: nazwa, sklep, wycena, opis, link, include_in_budget - używany
  zarówno w zakładce "materiały" instalacji jak i w "produktach" pokoju.
- Task: prosty tekst + done, per pokój.
- item_options()/segment_range() liczą teraz: materiały (zawsze sumowane) +
  widełki z usług (min/max alternatyw z grup wariantów), a dla pokoi:
  wyceny wykonawców + materiały.

## Czego jeszcze nie ma
- reklamy w wersji darmowej + subskrypcja 10 zł/mies bez reklam
- reset hasła przez e-mail, weryfikacja e-maila
- edycja nazwy/metrażu/budżetu domu po utworzeniu
- limit rozmiaru/kompresja uploadowanych zdjęć

## Poprawka: pokoje wg typu projektu
- dom: pełny, oryginalny zestaw 5 pokoi (korytarz, kuchnia, strefa dzienna,
  sypialnia, łazienka).
- mieszkanie/remont: 3 pokoje (strefa dzienna, kuchnia, łazienka).
- Kategorie inspiracji tworzą się automatycznie dopasowane do faktycznych
  pokoi danego typu (nie osobna, sztywna lista).
- "wykonamy to sami" usunięte z pozycji typu "urząd" (dokument+opłata) -
  tam nie ma sensu, zostaje przy materiał+wykonawca i szacowana wycena.
