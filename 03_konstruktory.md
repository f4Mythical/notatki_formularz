# 03. Konstruktory i `toString()`

## Konstruktor na prostym przykladzie

Konstruktor uruchamia sie wtedy, kiedy tworzysz nowy obiekt przez `new`.

Przyklad:

```java
Book ksiazka = new Book("Hobbit", "Tolkien", "Fantasy", true, 39.99, 10, "Papier PDF", "12+");
```

Tutaj:

- `Book` to nazwa klasy,
- `new Book(...)` tworzy nowy obiekt,
- wartosci w nawiasie trafiaja do konstruktora.

## Konstruktor klasy `Book`

Przyklad z Twojego projektu:

```java
public class Book {
    private String tytul;
    private String autor;
    private String gatunek;
    private boolean czNowa;
    private double cena;
    private int promocja;
    private String dostepnosc;
    private String kategoriaWiekowa;

    public Book(String tytul, String autor, String gatunek, boolean czNowa,
                double cena, int promocja, String dostepnosc, String kategoriaWiekowa) {
        this.tytul = tytul;
        this.autor = autor;
        this.gatunek = gatunek;
        this.czNowa = czNowa;
        this.cena = cena;
        this.promocja = promocja;
        this.dostepnosc = dostepnosc;
        this.kategoriaWiekowa = kategoriaWiekowa;
    }
}
```

To oznacza, ze przy tworzeniu obiektu musisz podac wszystkie potrzebne dane od razu.

## Konstruktor klasy `Grade`

Drugi przyklad:

```java
public class Grade {
    private String grades;
    private int weight;
    private String gradeType;
    private boolean czyDoSredniej;

    public Grade(String grades, int weight, String gradeType, boolean czyDoSredniej) {
        this.grades = grades;
        this.weight = weight;
        this.gradeType = gradeType;
        this.czyDoSredniej = czyDoSredniej;
    }
}
```

Tutaj jeden obiekt `Grade` oznacza jedna konkretna ocene.

## Co robi `this`

Bardzo wazny fragment:

```java
this.tytul = tytul;
```

Lewa strona:

- pole klasy

Prawa strona:

- parametr przekazany do konstruktora

To samo w innych linijkach:

```java
this.autor = autor;
this.cena = cena;
this.promocja = promocja;
```

## Przyklady tworzenia obiektow w `MainActivity`

### Przyklad 1 - ksiazka wpisana recznie

```java
Book ksiazka1 = new Book(
        "Wladca Pierscieni",
        "J.R.R. Tolkien",
        "Fantasy",
        true,
        49.99,
        15,
        "Papier PDF",
        "12+"
);
```

### Przyklad 2 - druga ksiazka

```java
Book ksiazka2 = new Book(
        "Metro 2033",
        "Dmitry Glukhovsky",
        "Sci-Fi",
        false,
        34.50,
        0,
        "Mobi Audiobook",
        "18+"
);
```

### Przyklad 3 - ocena

```java
Grade ocena1 = new Grade(
        "5",
        3,
        "Spr",
        true
);
```

### Przyklad 4 - inna ocena

```java
Grade ocena2 = new Grade(
        "4",
        1,
        "Akt",
        false
);
```

Takie przyklady sa dobre do testow, zanim jeszcze zrobisz pelny formularz.

## Tworzenie obiektu z danych z formularza

To juz jest wersja prawdziwa z `MainActivity`.

```java
String tytul = etTytul.getText().toString();
String autor = etAutor.getText().toString();
String gatunek = spinnerGatunki.getSelectedItem().toString();
boolean czNowa = switchCzyNowa.isChecked();
double cena = Double.parseDouble(etCena.getText().toString());
int promocja = seekBarPromocja.getProgress();

Book ksiazka = new Book(
        tytul,
        autor,
        gatunek,
        czNowa,
        cena,
        promocja,
        dostepnosc.toString().trim(),
        kategoriaWiekowa
);
```

To jest najwazniejszy sens konstruktora w Twoich projektach:

- najpierw zbierasz dane z widokow,
- potem jednym `new Book(...)` zamieniasz je na obiekt.

## Konstruktor adaptera

Konstruktor to nie tylko model `Book` albo `Grade`.

Adapter tez ma konstruktor.

Przyklad:

```java
public BookAdapter(Context context, List<Book> books) {
    super(context, 0, books);
}
```

Oraz:

```java
public GradeAdapter(Context context, List<Grade> grades) {
    super(context, android.R.layout.simple_list_item_1, grades);
}
```

To znaczy:

- do adaptera przekazujesz `context`,
- przekazujesz liste danych,
- adapter ma potem na czym pracowac.

Tworzenie adaptera w `MainActivity`:

```java
adapter = new BookAdapter(this, listaKsiazek);
```

Albo:

```java
adapter = new GradeAdapter(this, listaOcenDanych);
```

## Konstruktor `ArrayAdapter` dla spinnera

Jeszcze jeden praktyczny konstruktor:

```java
ArrayAdapter<String> spinnerAdapter = new ArrayAdapter<>(
        this,
        android.R.layout.simple_spinner_item,
        gatunki
);
```

Tutaj tez tworzysz obiekt przez konstruktor:

- `this` - aktywnosc,
- `android.R.layout.simple_spinner_item` - wyglad jednego elementu,
- `gatunki` - dane.

Czyli konstruktory sa praktycznie wszedzie, nie tylko w klasach modelu.

## Gettery a konstruktor

Konstruktor zapisuje dane do obiektu.  
Gettery pozniej te dane odczytuja.

Przyklad:

```java
public String getTytul() {
    return tytul;
}
```

Przeplyw wyglada tak:

1. `new Book(...)` zapisuje wartosci,
2. adapter robi `ksiazka.getTytul()`,
3. tytul trafia do `TextView`.

## `toString()` - bardzo wazna metoda

`toString()` zamienia obiekt na tekst.

Przyklad prosty:

```java
@Override
public String toString() {
    return tytul + " | " + autor + " | " + cena + " zl";
}
```

Jesli zrobisz taka metode w klasie `Book`, to obiekt mozna latwo pokazac jako napis.

## Po co `toString()` w praktyce

Przyklad:

```java
Book ksiazka = new Book("Hobbit", "Tolkien", "Fantasy", true, 39.99, 10, "Papier", "12+");
System.out.println(ksiazka);
```

Jesli masz `toString()`, to program wydrukuje ladny tekst.  
Jesli nie masz `toString()`, to zwykle zobaczysz cos malo przydatnego, np. nazwe klasy i dziwny adres w pamieci.

## `toString()` a `ListView`

To bardzo przydatna rzecz do zapamietania.

Jesli masz zwykly `ArrayAdapter<Book>` bez wlasnego adaptera, to Android czesto probuje pokazac obiekt jako tekst.  
Wtedy uzywa wlasnie `toString()`.

Przyklad:

```java
ArrayAdapter<Book> adapter = new ArrayAdapter<>(
        this,
        android.R.layout.simple_list_item_1,
        listaKsiazek
);
listViewRekordy.setAdapter(adapter);
```

W takim przypadku dobra metoda `toString()` jest bardzo przydatna.

## Przyklad `toString()` dla `Grade`

```java
@Override
public String toString() {
    return grades + " | waga: " + weight + " | " + gradeType;
}
```

Mozesz nawet dodac wersje rozszerzona:

```java
@Override
public String toString() {
    String doSredniej = czyDoSredniej ? " | do sredniej" : "";
    return grades + " | waga: " + weight + " | " + gradeType + doSredniej;
}
```

To jest bardzo podobne do tego, co i tak robi Twoj `GradeAdapter`.

## Konstruktor pusty i konstruktor z parametrami

Najczesciej w Twoich projektach uzywasz konstruktora z parametrami:

```java
new Book(...)
new Grade(...)
```

Ale istnieje tez konstruktor pusty:

```java
public Book() {
}
```

Wtedy tworzysz obiekt tak:

```java
Book ksiazka = new Book();
```

A potem ustawiasz dane osobno setterami.  
W Twoich repo tego nie ma, ale dobrze wiedziec, ze taka opcja istnieje.

## Co warto zapamietac

1. Konstruktor uruchamia sie przy `new`.
2. Konstruktor modelu ustawia dane jednego obiektu.
3. Konstruktor adaptera ustawia dane potrzebne do wyswietlania listy.
4. `this` oznacza pole aktualnego obiektu.
5. Gettery odczytuja to, co konstruktor wczesniej zapisal.
6. `toString()` zamienia obiekt na tekst.
7. `toString()` jest bardzo przydatne przy prostych adapterach i debugowaniu.
