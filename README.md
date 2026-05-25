# Notatki: formularze w Androidzie

Repo zawiera notatki do formularzy w Androidzie zapisane w kilku plikach `.md`.

To jest plik nawigacyjny:  
ma pomagac szybko znalezc odpowiedni temat, a nie tlumaczyc wszystko od zera.

## Szybka nawigacja

1. [01_xml_formularza.md](./01_xml_formularza.md)  
   Gotowe fragmenty `XML` oraz od razu dopiete przyklady `Java` do takich elementow jak `EditText`, `Spinner`, `CheckBox`, `RadioGroup`, `SeekBar`, `Button` i `ListView`.

2. [02_mainactivity_java.md](./02_mainactivity_java.md)  
   Najwazniejszy plik do nauki logiki formularza: `ini()`, `findViewById(...)`, `Spinner`, `ListView`, walidacja, przyciski, dodawanie danych i czyszczenie formularza.

3. [03_konstruktory.md](./03_konstruktory.md)  
   Konstruktory, tworzenie obiektow, przyklady wartosci, gettery i `toString()`.

4. [04_adaptery_i_item_xml.md](./04_adaptery_i_item_xml.md)  
   `ListView`, adaptery, `getView()`, `convertView`, `notifyDataSetChanged()` oraz rola pliku typu `item_...xml`.

5. [05_triki.md](./05_triki.md)  
   Gotowe dodatki do projektu: `onClick`, `onLongClick`, funkcje liczace, reakcje na `Spinner`, `SeekBar`, `CheckBox`, `Switch` i wpisywanie tekstu.

6. [06_przykladowe.md](./06_przykladowe.md)  
   Pelny przyklad dla tematu `Uklad hormonalny - quiz`, rozpisany jako: uklad zadania, `activity_main.xml`, `MainActivity.java`, dodatkowy plik i adapter.  
   Zawiera tez trzy warianty `ListView`:
   z `item.xml`, z `toString()`, oraz z adapterem bez `item.xml`.

## Od czego zaczac

Jesli celem jest szybkie ogarniecie formularzy:

`02 MainActivity -> 01 XML -> 04 Adaptery -> 03 Konstruktory`

Jesli celem jest zrobienie gotowego zadania:

`06 Przykladowe -> 05 Triki -> 02 MainActivity`

## Gdzie szukac konkretnych rzeczy

- `Spinner`:
  [01_xml_formularza.md](./01_xml_formularza.md) i [02_mainactivity_java.md](./02_mainactivity_java.md)
- `ListView`:
  [01_xml_formularza.md](./01_xml_formularza.md), [04_adaptery_i_item_xml.md](./04_adaptery_i_item_xml.md), [06_przykladowe.md](./06_przykladowe.md)
- `onClick` i `onLongClick`:
  [05_triki.md](./05_triki.md)
- `toString()`:
  [03_konstruktory.md](./03_konstruktory.md) i [06_przykladowe.md](./06_przykladowe.md)
- pelny przyklad projektu:
  [06_przykladowe.md](./06_przykladowe.md)

## Uwagi

- W notatkach glownie pojawia sie `ListView`, bo na nim najlatwiej pokazac podstawy formularzy i adapterow.
- `RecyclerView` jest wspomniany pomocniczo, ale nie jest tu glownym tematem.
- W wielu miejscach sa pokazane dwie drogi:
  wersja prostsza i wersja bardziej rozbudowana.
- Przy listach warto znac trzy poziomy trudnosci:
  zwykly `ArrayAdapter`, `toString()`, oraz wlasny adapter.

## Zrodla ukladu materialu

Material jest oparty na przykladach podobnych do prostych projektow szkolnych Android:

- [lek16_oceny](https://github.com/F4Mythical/lek16_oceny)
- [lek15_ksiazka](https://github.com/F4Mythical/lek15_ksiazka)
