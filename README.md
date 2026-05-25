# Notatki: formularze w Androidzie

Te notatki powstaly na podstawie dwoch Twoich repozytoriow:

- [lek16_oceny](https://github.com/F4Mythical/lek16_oceny)
- [lek15_ksiazka](https://github.com/F4Mythical/lek15_ksiazka)

Cel tych notatek jest prosty: wyjasnic, jak dziala formularz w Android Studio w wersji latwej do ogarniecia.

## Co tu znajdziesz

1. [01_xml_formularza.md](./01_xml_formularza.md)  
   Duzy zbior gotowych fragmentow XML pod formularz oraz od razu kawalki Java pokazujace, jak podlaczyc `Spinner`, `SeekBar`, `RadioGroup` i `ListView`.

2. [02_mainactivity_java.md](./02_mainactivity_java.md)  
   Najdluzsza notatka. Jest tu duzo praktyki: `ini()`, `Spinner`, `SeekBar`, `CheckBox`, `Switch`, `RadioGroup`, `ListView`, walidacja, dodawanie obiektow i czyszczenie formularza.

3. [03_konstruktory.md](./03_konstruktory.md)  
   Konstruktory z wieloma przykladami wartosci, tworzenie obiektow w `MainActivity` i dodatkowo wyjasnienie `toString()`.

4. [04_adaptery_i_item_xml.md](./04_adaptery_i_item_xml.md)  
   Rozbudowana notatka o `ListView`, adapterach, `getView()`, `convertView`, `notifyDataSetChanged()` i o tym, po co jest `item_book.xml`.

5. [05_triki.md](./05_triki.md)  
   Gotowe pomysly na akcje `onClick`, `onLongClick`, funkcje do liczenia oraz dodatkowe reakcje na `Spinner`, `SeekBar`, `CheckBox`, `Switch` i wpisywanie tekstu.

6. [06_przykladowe.md](./06_przykladowe.md)  
   Jeden pelny przyklad dla ukladu hormonalnego: uklad zadania, `activity_main.xml`, `MainActivity.java`, dodatkowy plik `item_hormon.xml` i adapter `HormoneAdapter.java`.

## Najwazniejsza mysl

W obu Twoich projektach schemat jest bardzo podobny:

1. Robisz wyglad formularza w XML.
2. W `MainActivity` pobierasz widoki przez `findViewById(...)`.
3. Odczytujesz dane wpisane przez uzytkownika.
4. Tworzysz obiekt, np. `Book` albo `Grade`.
5. Dodajesz go do listy.
6. Adapter pokazuje te dane w `ListView`.

## Dobra kolejnosc czytania

Najlepiej czytaj tak:

`02 MainActivity -> 01 XML -> 03 Konstruktory -> 04 Adaptery -> 05 Triki -> 06 Przykladowe`

## Wazna uwaga

W Twoich repozytoriach jest uzyty glownie `ListView`, nie `RecyclerView`.  
Mimo to w notatkach krotko wyjasniam tez `RecyclerView`, zebys wiedzial czym sie rozni i po co istnieje.
