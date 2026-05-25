# 01. XML formularza

W tym pliku nie skupiamy sie na teorii, tylko na praktyce:

- jaki fragment XML wpisac,
- co on robi,
- jak od razu podlaczyc to w Java.

Najwiecej przykladow jest wzietych z Twoich projektow `lek15_ksiazka` i `lek16_oceny`.

## Uklad calego formularza

W obu projektach bardzo dobra baza jest taki uklad:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:id="@+id/main"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="16dp">

        <!-- tutaj kolejne elementy formularza -->

    </LinearLayout>

</ScrollView>
```

Taki start jest wygodny, bo:

- formularz moze byc dlugi,
- ekran da sie przewijac,
- elementy ukladaja sie jeden pod drugim.

## `EditText` - wpisywanie danych

Przyklad z formularza ksiazki:

```xml
<EditText
    android:id="@+id/etTytul"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Tytul"
    android:inputType="text"
    android:layout_marginBottom="8dp" />

<EditText
    android:id="@+id/etAutor"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Autor"
    android:inputType="text"
    android:layout_marginBottom="8dp" />

<EditText
    android:id="@+id/etCena"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Cena"
    android:inputType="numberDecimal"
    android:layout_marginBottom="12dp" />
```

Podlaczenie w Java:

```java
private EditText etTytul, etAutor, etCena;

private void ini() {
    etTytul = findViewById(R.id.etTytul);
    etAutor = findViewById(R.id.etAutor);
    etCena = findViewById(R.id.etCena);
}
```

Odczyt wartosci:

```java
String tytul = etTytul.getText().toString();
String autor = etAutor.getText().toString();
double cena = Double.parseDouble(etCena.getText().toString());
```

Walidacja:

```java
if (etTytul.getText().toString().trim().isEmpty()) {
    Toast.makeText(this, "Wpisz tytul", Toast.LENGTH_SHORT).show();
    return;
}
```

## `Spinner` - gotowy wybor z listy

### XML

W `activity_main.xml`:

```xml
<Spinner
    android:id="@+id/spinnerGatunki"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginBottom="12dp" />
```

W `strings.xml`:

```xml
<string-array name="gatunki">
    <item>Thriller</item>
    <item>Romans</item>
    <item>Fantasy</item>
    <item>Sci-Fi</item>
    <item>Horror</item>
</string-array>
```

### Java - podlaczenie spinnera

```java
private Spinner spinnerGatunki;

private void ini() {
    spinnerGatunki = findViewById(R.id.spinnerGatunki);
}
```

### Java - wypelnienie danych

```java
String[] gatunki = getResources().getStringArray(R.array.gatunki);

ArrayAdapter<String> spinnerAdapter = new ArrayAdapter<>(
        this,
        android.R.layout.simple_spinner_item,
        gatunki
);

spinnerAdapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
spinnerGatunki.setAdapter(spinnerAdapter);
```

### Java - pobranie wybranego elementu

```java
String gatunek = spinnerGatunki.getSelectedItem().toString();
```

### Dodatkowy przyklad: reakcja na wybor

```java
spinnerGatunki.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
    @Override
    public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
        String wybranyGatunek = parent.getItemAtPosition(position).toString();
        Toast.makeText(MainActivity.this, "Wybrano: " + wybranyGatunek, Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onNothingSelected(AdapterView<?> parent) {
    }
});
```

Ten fragment nie musi byc zawsze, ale dobrze wiedziec, ze spinner moze od razu reagowac na zmiane wyboru.

## `CheckBox` - wiele wyborow naraz

### XML

```xml
<CheckBox
    android:id="@+id/checkBoxPapier"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Papier" />

<CheckBox
    android:id="@+id/checkBoxMobi"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Mobi" />

<CheckBox
    android:id="@+id/checkBoxAudiobook"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Audiobook" />

<CheckBox
    android:id="@+id/checkBoxPdf"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="PDF" />
```

### Java - podlaczenie

```java
private CheckBox checkBoxPapier, checkBoxMobi, checkBoxAudiobook, checkBoxPdf;

private void ini() {
    checkBoxPapier = findViewById(R.id.checkBoxPapier);
    checkBoxMobi = findViewById(R.id.checkBoxMobi);
    checkBoxAudiobook = findViewById(R.id.checkBoxAudiobook);
    checkBoxPdf = findViewById(R.id.checkBoxPdf);
}
```

### Java - pobranie zaznaczen

```java
StringBuilder dostepnosc = new StringBuilder();

if (checkBoxPapier.isChecked()) {
    dostepnosc.append("Papier ");
}
if (checkBoxMobi.isChecked()) {
    dostepnosc.append("Mobi ");
}
if (checkBoxAudiobook.isChecked()) {
    dostepnosc.append("Audiobook ");
}
if (checkBoxPdf.isChecked()) {
    dostepnosc.append("PDF ");
}

String wynikDostepnosci = dostepnosc.toString().trim();
```

To jest bardzo praktyczny wzor, bo od razu skladasz ladny tekst do pokazania na liscie.

## `Switch` - tak/nie

### XML

```xml
<Switch
    android:id="@+id/switchCzyNowa"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Czy nowa"
    android:layout_marginBottom="12dp" />
```

### Java

```java
private Switch switchCzyNowa;

private void ini() {
    switchCzyNowa = findViewById(R.id.switchCzyNowa);
}

boolean czyNowa = switchCzyNowa.isChecked();
```

Praktyczne uzycie:

```java
tvStatus.setText(ksiazka.isCzNowa() ? "NOWOSC" : "");
```

Czyli `Switch` daje Ci `true` albo `false`, a Ty potem decydujesz, co pokazac.

## `SeekBar` + `TextView` obok

To bardzo dobry zestaw z Twoich projektow.

### XML

```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal">

    <SeekBar
        android:id="@+id/seekBarPromocja"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:max="90"
        android:progress="15" />

    <TextView
        android:id="@+id/tvPromocjaWartosc"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:text="15%" />

</LinearLayout>
```

### Java - podlaczenie

```java
private SeekBar seekBarPromocja;
private TextView tvPromocjaWartosc;

private void ini() {
    seekBarPromocja = findViewById(R.id.seekBarPromocja);
    tvPromocjaWartosc = findViewById(R.id.tvPromocjaWartosc);
}
```

### Java - aktualizacja przy przesuwaniu

```java
seekBarPromocja.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
    @Override
    public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
        tvPromocjaWartosc.setText(progress + "%");
    }

    @Override
    public void onStartTrackingTouch(SeekBar seekBar) {
    }

    @Override
    public void onStopTrackingTouch(SeekBar seekBar) {
    }
});
```

### Drugi przyklad z ocen

W ocenach chcesz wartosc od 1 do 11, ale `SeekBar` startuje od 0.  
Dlatego w Java bylo:

```java
int waga = seekBarWaga.getProgress() + 1;
tvWagaWartosc.setText(String.valueOf(progress + 1));
```

To bardzo wazne, bo pokazuje, ze czasem trzeba poprawic wartosc z paska.

## `RadioGroup` i `RadioButton`

### XML

```xml
<RadioGroup
    android:id="@+id/radioGroupKategoria"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal">

    <RadioButton
        android:id="@+id/radio18plus"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="18+" />

    <RadioButton
        android:id="@+id/radio12plus"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="12+" />

    <RadioButton
        android:id="@+id/radio9plus"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="9+" />

    <RadioButton
        android:id="@+id/radio0plus"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="0+" />

</RadioGroup>
```

### Java - podlaczenie

```java
private RadioGroup radioGroupKategoria;
private RadioButton radio18plus, radio12plus, radio9plus, radio0plus;

private void ini() {
    radioGroupKategoria = findViewById(R.id.radioGroupKategoria);
    radio18plus = findViewById(R.id.radio18plus);
    radio12plus = findViewById(R.id.radio12plus);
    radio9plus = findViewById(R.id.radio9plus);
    radio0plus = findViewById(R.id.radio0plus);
}
```

### Java - odczyt wyboru

```java
String kategoriaWiekowa = "";

if (radio18plus.isChecked()) {
    kategoriaWiekowa = "18+";
} else if (radio12plus.isChecked()) {
    kategoriaWiekowa = "12+";
} else if (radio9plus.isChecked()) {
    kategoriaWiekowa = "9+";
} else if (radio0plus.isChecked()) {
    kategoriaWiekowa = "0+";
}
```

### Drugi przyklad: sprawdzenie czy cokolwiek wybrano

W ocenach bylo to bardzo sensownie zrobione:

```java
if (grupaTypu.getCheckedRadioButtonId() == -1) {
    Toast.makeText(this, "Uzupelnij kategorie oceny!", Toast.LENGTH_SHORT).show();
    return;
}
```

To warto zapamietac, bo jest przydatne w zadaniach szkolnych.

## `Button` - wyslanie formularza

### XML

```xml
<Button
    android:id="@+id/btnWyslij"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Wyslij"
    android:layout_gravity="center_horizontal"
    android:layout_marginBottom="16dp" />
```

### Java

```java
private Button btnWyslij;

private void ini() {
    btnWyslij = findViewById(R.id.btnWyslij);
}

btnWyslij.setOnClickListener(v -> wyslijFormularz());
```

Albo druga wersja z projektu ocen:

```java
btnDodaj.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        dodajOcene();
    }
});
```

Obie wersje sa poprawne.

## `ListView` - pokazanie wielu rekordow

To jedna z najwazniejszych rzeczy w Twoich projektach.

### XML

```xml
<ListView
    android:id="@+id/listViewRekordy"
    android:layout_width="match_parent"
    android:layout_height="200dp" />
```

Drugi przyklad:

```xml
<ListView
    android:id="@+id/lvGrades"
    android:layout_width="match_parent"
    android:layout_height="400dp"
    android:layout_marginTop="12dp" />
```

### Java - podlaczenie `ListView`

```java
private ListView listViewRekordy;

private void ini() {
    listViewRekordy = findViewById(R.id.listViewRekordy);
}
```

### Java - przygotowanie danych i adaptera

```java
private List<Book> listaKsiazek = new ArrayList<>();
private BookAdapter adapter;

adapter = new BookAdapter(this, listaKsiazek);
listViewRekordy.setAdapter(adapter);
```

### Java - dodanie nowego rekordu

```java
Book ksiazka = new Book(
        tytul,
        autor,
        gatunek,
        czyNowa,
        cena,
        promocja,
        wynikDostepnosci,
        kategoriaWiekowa
);

listaKsiazek.add(0, ksiazka);
adapter.notifyDataSetChanged();
```

To jest komplet: XML + Java + dodanie danych.

### Dodatkowy przyklad: klikniecie elementu listy

```java
listViewRekordy.setOnItemClickListener((parent, view, position, id) -> {
    Book kliknietaKsiazka = listaKsiazek.get(position);
    Toast.makeText(this, kliknietaKsiazka.getTytul(), Toast.LENGTH_SHORT).show();
});
```

To nie bylo w repo, ale jest bardzo przydatne, bo pokazuje co mozna robic z `ListView` dalej.

## `item_book.xml` - osobny wyglad jednego elementu listy

W projekcie `lek15_ksiazka` masz bardzo wazny plik:

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

To jest szablon jednego wiersza na liscie.

Pozniej adapter robi:

```java
convertView = LayoutInflater.from(getContext())
        .inflate(R.layout.item_book, parent, false);
```

I dzieki temu kazdy rekord ksiazki ma ladniejszy wyglad niz zwykle jedno pole tekstowe.

## Fragment formularza ocen - wszystko razem

Przyklad zblizony do Twojego XML z ocen:

```xml
<CheckBox
    android:id="@+id/cbSrednia"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Czy liczy sie do sredniej" />

<Spinner
    android:id="@+id/spinnerGrades"
    android:layout_width="match_parent"
    android:layout_height="48dp"
    android:entries="@array/grades_array" />

<SeekBar
    android:id="@+id/seekBarWeight"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:max="10"
    android:progress="0" />

<RadioGroup
    android:id="@+id/radioGroupType"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <RadioButton
        android:id="@+id/radioSpr"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Sprawdzian" />

    <RadioButton
        android:id="@+id/radioOdp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Odpowiedz" />

    <RadioButton
        android:id="@+id/radioAkty"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Aktywnosc" />

</RadioGroup>

<Button
    android:id="@+id/btnDodaj"
    android:layout_width="match_parent"
    android:layout_height="64dp"
    android:text="Dodaj ocene" />

<ListView
    android:id="@+id/lvGrades"
    android:layout_width="match_parent"
    android:layout_height="400dp" />
```

Java do tego fragmentu:

```java
spinnerOcena = findViewById(R.id.spinnerGrades);
seekBarWaga = findViewById(R.id.seekBarWeight);
grupaTypu = findViewById(R.id.radioGroupType);
radioAktywnosc = findViewById(R.id.radioAkty);
radioOdpowiedz = findViewById(R.id.radioOdp);
radioSprawdzian = findViewById(R.id.radioSpr);
doSredniej = findViewById(R.id.cbSrednia);
btnDodaj = findViewById(R.id.btnDodaj);
listaOcen = findViewById(R.id.lvGrades);
```

## Co warto umiec po tym pliku

Po ogarnieciu tego pliku powinienes umiec:

1. Zrobic formularz z `EditText`, `Spinner`, `CheckBox`, `RadioButton`, `SeekBar` i `Button`.
2. Podlaczyc te elementy w `MainActivity`.
3. Odczytac dane z formularza.
4. Dodac rekord do `ListView`.
5. Zrobic osobny `item.xml` dla jednego wiersza listy.

Jesli to rozumiesz, to masz juz bardzo dobra baze pod zadania o formularzach w Androidzie.
