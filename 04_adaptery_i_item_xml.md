# 04. Adaptery, `ListView` i `item_book.xml`

Ten temat jest bardzo wazny, bo same dane w liscie nie wystarcza.  
Trzeba jeszcze umiec pokazac je na ekranie. I wlasnie tym zajmuje sie adapter.

## 1. O co chodzi z adapterem

Masz trzy rzeczy:

1. dane, np. `List<Book>` albo `List<Grade>`
2. widok listy, czyli `ListView`
3. adapter pomiedzy nimi

Adapter bierze dane z listy i zamienia je na widok jednego elementu na ekranie.

## 2. Najprostszy schemat pracy z `ListView`

W `MainActivity` zwykle wyglada to tak:

```java
private List<Book> listaKsiazek = new ArrayList<>();
private BookAdapter adapter;
private ListView listViewRekordy;
```

Potem:

```java
adapter = new BookAdapter(this, listaKsiazek);
listViewRekordy.setAdapter(adapter);
```

A kiedy do listy dojdzie nowy element:

```java
listaKsiazek.add(0, ksiazka);
adapter.notifyDataSetChanged();
```

To jest absolutna podstawa `ListView`.

## 3. Co robi `setAdapter(...)`

Przyklad:

```java
listViewRekordy.setAdapter(adapter);
```

Ta jedna linijka mowi:

- ta lista ma korzystac z tego adaptera,
- adapter ma pobierac dane z przekazanej listy,
- adapter ma tworzyc widoki wierszy.

Bez `setAdapter(...)` `ListView` nic nie pokaze.

## 4. Najprostszy adapter - gotowy jeden `TextView`

W projekcie ocen masz taka linijke w konstruktorze:

```java
super(context, android.R.layout.simple_list_item_1, grades);
```

To znaczy, ze Android daje gotowy prosty widok jednego elementu listy:

- jeden wiersz,
- jeden `TextView`.

To bardzo dobre na start.

Przyklad bardzo prosty:

```java
ArrayAdapter<String> adapter = new ArrayAdapter<>(
        this,
        android.R.layout.simple_list_item_1,
        new String[]{"Ala", "Ola", "Ela"}
);

listView.setAdapter(adapter);
```

To juz dziala bez tworzenia wlasnej klasy adaptera.

## 5. Po co wlasny adapter, skoro jest `ArrayAdapter`

Bo czasem jeden `TextView` to za malo.

W Twoim projekcie `lek15_ksiazka` jeden rekord ma wiele danych:

- tytul,
- autor,
- gatunek,
- cena,
- dostepnosc,
- kategoria,
- status.

Dlatego zwykly prosty wiersz nie wystarcza i powstaje:

- `BookAdapter`
- `item_book.xml`

To bardzo czesty krok w Androidzie.

## 6. `GradeAdapter` - prostszy adapter z Twojego projektu

Twoj adapter ocen:

```java
public class GradeAdapter extends ArrayAdapter<Grade> {

    public GradeAdapter(Context context, List<Grade> grades) {
        super(context, android.R.layout.simple_list_item_1, grades);
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        if (convertView == null) {
            convertView = LayoutInflater.from(getContext())
                    .inflate(android.R.layout.simple_list_item_1, parent, false);
        }

        Grade grade = getItem(position);
        TextView textView = convertView.findViewById(android.R.id.text1);

        String doSredniej = "";
        if (grade.isCzyDoSredniej()) {
            doSredniej = " | do sredniej";
        }

        textView.setText(
                grade.getGrades() + " | waga: " + grade.getWeight()
                        + " | " + grade.getGradeType() + doSredniej
        );

        return convertView;
    }
}
```

## 7. Rozpisanie `GradeAdapter` linijka po linijce

### `extends ArrayAdapter<Grade>`

```java
public class GradeAdapter extends ArrayAdapter<Grade>
```

To znaczy:

- ten adapter obsluguje obiekty typu `Grade`,
- kazdy element listy to jedna ocena.

### Konstruktor

```java
public GradeAdapter(Context context, List<Grade> grades) {
    super(context, android.R.layout.simple_list_item_1, grades);
}
```

Do adaptera przekazujesz:

- `context` - zwykle `this` z aktywnosci,
- `grades` - liste ocen.

### `getView(...)`

```java
public View getView(int position, View convertView, ViewGroup parent)
```

To jest serce adaptera.

Parametry:

- `position` - numer elementu na liscie
- `convertView` - stary widok do ponownego uzycia
- `parent` - rodzic, czyli `ListView`

### Tworzenie widoku, gdy go nie ma

```java
if (convertView == null) {
    convertView = LayoutInflater.from(getContext())
            .inflate(android.R.layout.simple_list_item_1, parent, false);
}
```

To znaczy:

- jesli nie ma gotowego widoku,
- stworz nowy z XML.

### Pobranie konkretnej oceny

```java
Grade grade = getItem(position);
```

Na przyklad:

- `position = 0` -> pierwsza ocena
- `position = 1` -> druga ocena

### Ustawienie tekstu

```java
TextView textView = convertView.findViewById(android.R.id.text1);
textView.setText(...);
```

Czyli:

- bierzesz `TextView` z wiersza,
- wpisujesz do niego dane z obiektu `Grade`.

## 8. `BookAdapter` - adapter z wlasnym `item_book.xml`

Twoj drugi adapter:

```java
public class BookAdapter extends ArrayAdapter<Book> {

    public BookAdapter(Context context, List<Book> books) {
        super(context, 0, books);
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        Book ksiazka = getItem(position);

        if (convertView == null) {
            convertView = LayoutInflater.from(getContext())
                    .inflate(R.layout.item_book, parent, false);
        }

        TextView tvTytul = convertView.findViewById(R.id.tvTytul);
        TextView tvAutor = convertView.findViewById(R.id.tvAutor);
        TextView tvGatunek = convertView.findViewById(R.id.tvGatunek);
        TextView tvCena = convertView.findViewById(R.id.tvCena);
        TextView tvDostepnosc = convertView.findViewById(R.id.tvDostepnosc);
        TextView tvKategoria = convertView.findViewById(R.id.tvKategoria);
        TextView tvStatus = convertView.findViewById(R.id.tvStatus);

        tvTytul.setText(ksiazka.getTytul());
        tvAutor.setText("Autor: " + ksiazka.getAutor());
        tvGatunek.setText("Gatunek: " + ksiazka.getGatunek());

        if (ksiazka.getPromocja() > 0) {
            double cenaPo = ksiazka.getCena() * (1 - ksiazka.getPromocja() / 100.0);
            tvCena.setText(String.format("%.2f zl (-%d%% -> %.2f zl)",
                    ksiazka.getCena(), ksiazka.getPromocja(), cenaPo));
        } else {
            tvCena.setText(String.format("%.2f zl", ksiazka.getCena()));
        }

        tvDostepnosc.setText("Dostepna: " + ksiazka.getDostepnosc());
        tvKategoria.setText("Wiek: " + ksiazka.getKategoriaWiekowa());
        tvStatus.setText(ksiazka.isCzNowa() ? "NOWOSC" : "");

        return convertView;
    }
}
```

## 9. Co tu jest mocniejsze niz w `GradeAdapter`

W `BookAdapter` nie masz juz jednego pola tekstowego.  
Masz caly wlasny mini-layout.

To daje Ci wiecej kontroli:

- inny rozmiar tekstu,
- kilka linii,
- kolory,
- osobny status,
- rozna prezentacja ceny.

To juz wyglada bardziej jak prawdziwa aplikacja.

## 10. `item_book.xml` - po co jest osobny plik

`item_book.xml` to wyglad jednego wiersza listy.

Na przyklad:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="10dp">

    <TextView
        android:id="@+id/tvTytul"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textStyle="bold" />

    <TextView
        android:id="@+id/tvAutor"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

    <TextView
        android:id="@+id/tvCena"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
</LinearLayout>
```

W praktyce ten plik mowi:

- tu bedzie tytul,
- tu autor,
- tu cena,
- tu inne pola.

Adapter tylko wypelnia te miejsca.

## 11. Jak adapter laczy sie z `item_book.xml`

Najwazniejsza linijka:

```java
convertView = LayoutInflater.from(getContext())
        .inflate(R.layout.item_book, parent, false);
```

To znaczy:

- wez plik `item_book.xml`,
- zamien go na prawdziwy widok,
- przygotuj jeden rekord listy.

Potem:

```java
TextView tvTytul = convertView.findViewById(R.id.tvTytul);
tvTytul.setText(ksiazka.getTytul());
```

I tak samo dla reszty pol.

## 12. `position`, `getItem(position)` i konkretne dane

Przyklad:

```java
Book ksiazka = getItem(position);
```

Jesli:

- `position = 0`, bierzesz pierwszy obiekt
- `position = 1`, bierzesz drugi obiekt
- `position = 2`, bierzesz trzeci obiekt

To bardzo wazne, bo `getView()` jest wykonywane dla kazdego elementu osobno.

## 13. `convertView` - po co to istnieje

Gdyby za kazdym razem tworzyc nowy widok od zera, lista dzialalaby wolniej.

Dlatego Android daje `convertView`.

Schemat:

```java
if (convertView == null) {
    convertView = LayoutInflater.from(getContext())
            .inflate(R.layout.item_book, parent, false);
}
```

Czyli:

- jest gotowy stary widok? to uzyj go ponownie,
- nie ma? utworz nowy.

To oszczedza pamiec i przyspiesza liste.

## 14. `notifyDataSetChanged()` - bez tego lista sie nie odswiezy

Przyklad:

```java
listaKsiazek.add(0, ksiazka);
adapter.notifyDataSetChanged();
```

Albo:

```java
listaOcenDanych.add(new Grade("5", 2, "Spr", true));
adapter.notifyDataSetChanged();
```

Bez `notifyDataSetChanged()` nowy rekord moze nie pojawic sie od razu na ekranie.

## 15. Prosty przyklad `ListView` bez wlasnego adaptera

Na start mozna tez zrobic tak:

```java
ListView listView;
ArrayList<String> dane = new ArrayList<>();

listView = findViewById(R.id.listViewRekordy);

dane.add("Adam");
dane.add("Ola");
dane.add("Basia");

ArrayAdapter<String> adapter = new ArrayAdapter<>(
        this,
        android.R.layout.simple_list_item_1,
        dane
);

listView.setAdapter(adapter);
```

To jest dobra baza do nauki, zanim wejdziesz w `BookAdapter`.

## 16. Praktyczny przeplyw: formularz -> lista -> adapter

W Twoich projektach wyglada to tak:

1. uzytkownik cos wpisuje lub zaznacza,
2. `MainActivity` odczytuje dane,
3. powstaje `Book` albo `Grade`,
4. obiekt trafia do `ArrayList`,
5. adapter dostaje sygnal `notifyDataSetChanged()`,
6. `ListView` pokazuje nowy rekord.

To bardzo wazny schemat do zapamietania.

## 17. Dodawanie elementu na poczatek albo koniec

Przyklad:

```java
listaKsiazek.add(ksiazka);
```

To da nowy rekord na koncu.

Przyklad:

```java
listaKsiazek.add(0, ksiazka);
```

To da nowy rekord na poczatku.

W `lek15_ksiazka` wybrales druga wersje i to jest bardzo sensowne, bo nowa ksiazka od razu jest widoczna na gorze.

## 18. Klikanie elementow `ListView`

To nie bylo w repo, ale bardzo warto znac.

### Zwykle klikniecie

```java
listViewRekordy.setOnItemClickListener((parent, view, position, id) -> {
    Book ksiazka = listaKsiazek.get(position);
    Toast.makeText(this, ksiazka.getTytul(), Toast.LENGTH_SHORT).show();
});
```

### Dlugie klikniecie i usuniecie

```java
listViewRekordy.setOnItemLongClickListener((parent, view, position, id) -> {
    listaKsiazek.remove(position);
    adapter.notifyDataSetChanged();
    Toast.makeText(this, "Usunieto: " + position, Toast.LENGTH_SHORT).show();
    return true;
});
```

To daje Ci kolejny poziom pracy z lista.

## 19. Co by bylo, gdyby uzyc `toString()`

Jesli klasa `Book` ma:

```java
@Override
public String toString() {
    return tytul + " | " + autor + " | " + cena + " zl";
}
```

To mozna zrobic prostsza liste bez wlasnego adaptera:

```java
ArrayAdapter<Book> adapter = new ArrayAdapter<>(
        this,
        android.R.layout.simple_list_item_1,
        listaKsiazek
);

listViewRekordy.setAdapter(adapter);
```

Wtedy `ListView` pokaze wynik `toString()`.

To jest dobra sztuczka do prostych testow albo malego zadania.

## 20. Co mozna jeszcze ulepszyc w adapterze

W prostych projektach to, co masz, jest okej.  
Ale dobrze wiedziec, co mozna zrobic pozniej:

1. dodac `ViewHolder`, zeby nie szukac `findViewById(...)` za kazdym razem,
2. dodac obrazki,
3. dodac przycisk usuwania w jednym wierszu,
4. zmieniac kolor rekordu zalezne od danych,
5. przejsc z `ListView` na `RecyclerView`.

Na ten moment nie musisz tego wszystkiego robic, ale dobrze wiedziec, w ktora strone idzie rozwoj.

## 21. Kiedy `ListView`, a kiedy `RecyclerView`

Na Twoim poziomie:

- `ListView` jest prostszy i szybciej go ogarnac,
- `RecyclerView` jest nowoczesniejszy.

Najwazniejsze jest to, ze idea adaptera zostaje podobna:

- masz dane,
- masz item XML,
- masz adapter,
- masz liste.

Jesli zrozumiesz `BookAdapter` i `GradeAdapter`, to wejscie w `RecyclerView` bedzie duzo latwiejsze.

## 22. Co warto umiec po tym pliku

1. utworzyc `ArrayList` z danymi,
2. stworzyc adapter,
3. podlaczyc adapter do `ListView`,
4. napisac `getView(...)`,
5. uzyc `item_book.xml`,
6. odswiezyc liste przez `notifyDataSetChanged()`,
7. obsluzyc klikniecie na elemencie listy,
8. rozumiec roznice miedzy prostym `ArrayAdapter` a wlasnym adapterem.

Jesli to rozumiesz, to masz juz bardzo mocna baze pod listy w Androidzie.
