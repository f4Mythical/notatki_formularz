# 02. MainActivity w Java

To jest najwazniejszy plik z calego projektu, bo tutaj:

- laczysz XML z Java,
- ustawiasz spinner,
- reagujesz na klikniecia,
- odczytujesz dane,
- tworzysz obiekty,
- dodajesz je do listy,
- odswiezasz `ListView`,
- czyscisz formularz.

W Twoich dwoch projektach `MainActivity` robi prawie caly glowny przeplyw programu.

## 1. Zmienne na gorze klasy

Przyklad zblizony do `lek15_ksiazka`:

```java
public class MainActivity extends AppCompatActivity {

    private EditText etTytul, etAutor, etCena;
    private Spinner spinnerGatunki;
    private Switch switchCzyNowa;
    private SeekBar seekBarPromocja;
    private TextView tvPromocjaWartosc;
    private CheckBox checkBoxPapier, checkBoxMobi, checkBoxAudiobook, checkBoxPdf;
    private RadioGroup radioGroupKategoria;
    private RadioButton radio18plus, radio9plus, radio12plus, radio0plus;
    private Button btnWyslij;
    private ListView listViewRekordy;

    private List<Book> listaKsiazek = new ArrayList<>();
    private BookAdapter adapter;
}
```

Tutaj widac dwie glowne grupy:

### Widoki z formularza

To wszystko, co masz w XML:

- `EditText`
- `Spinner`
- `Switch`
- `SeekBar`
- `CheckBox`
- `RadioGroup`
- `Button`
- `ListView`

### Dane programu

To wszystko, co trzyma informacje:

- `List<Book> listaKsiazek`
- `BookAdapter adapter`

Bez tego nie pokazesz danych na liscie.

## 2. `onCreate()` - glowny start ekranu

Przyklad oparty na Twoim projekcie:

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    ini();

    String[] gatunki = getResources().getStringArray(R.array.gatunki);
    ArrayAdapter<String> spinnerAdapter = new ArrayAdapter<>(
            this,
            android.R.layout.simple_spinner_item,
            gatunki
    );
    spinnerAdapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
    spinnerGatunki.setAdapter(spinnerAdapter);

    adapter = new BookAdapter(this, listaKsiazek);
    listViewRekordy.setAdapter(adapter);

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

    btnWyslij.setOnClickListener(v -> wyslijFormularz());
}
```

To jest bardzo dobry wzor, bo w jednym miejscu ustawiasz najwazniejsze rzeczy:

1. ladowanie XML,
2. podlaczenie widokow,
3. ustawienie spinnera,
4. ustawienie adaptera do `ListView`,
5. ustawienie listenera do `SeekBar`,
6. ustawienie listenera do przycisku.

## 3. `ini()` - bardzo wazna metoda

W Twoich projektach to jedna z najpraktyczniejszych metod.

Przyklad:

```java
private void ini() {
    etTytul = findViewById(R.id.etTytul);
    etAutor = findViewById(R.id.etAutor);
    etCena = findViewById(R.id.etCena);
    spinnerGatunki = findViewById(R.id.spinnerGatunki);
    switchCzyNowa = findViewById(R.id.switchCzyNowa);
    seekBarPromocja = findViewById(R.id.seekBarPromocja);
    tvPromocjaWartosc = findViewById(R.id.tvPromocjaWartosc);
    checkBoxPapier = findViewById(R.id.checkBoxPapier);
    checkBoxMobi = findViewById(R.id.checkBoxMobi);
    checkBoxAudiobook = findViewById(R.id.checkBoxAudiobook);
    checkBoxPdf = findViewById(R.id.checkBoxPdf);
    radioGroupKategoria = findViewById(R.id.radioGroupKategoria);
    radio18plus = findViewById(R.id.radio18plus);
    radio9plus = findViewById(R.id.radio9plus);
    radio12plus = findViewById(R.id.radio12plus);
    radio0plus = findViewById(R.id.radio0plus);
    btnWyslij = findViewById(R.id.btnWyslij);
    listViewRekordy = findViewById(R.id.listViewRekordy);
}
```

To jest bardzo dobre rozwiazanie, bo:

- `onCreate()` jest krotsze,
- latwiej cos znalezc,
- wszystkie `findViewById(...)` masz w jednym miejscu.

Warto sie tego trzymac.

## 4. Spinner w `MainActivity`

W XML masz sam `Spinner`, ale to `MainActivity` daje mu dane.

Przyklad:

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

Ten kod robi kilka rzeczy:

1. pobiera tablice z `strings.xml`,
2. tworzy adapter spinnera,
3. ustawia wyglad rozwijanej listy,
4. podpina adapter do spinnera.

Pobranie wybranego elementu:

```java
String gatunek = spinnerGatunki.getSelectedItem().toString();
```

Dodatkowy praktyczny przyklad:

```java
if (spinnerGatunki.getSelectedItemPosition() == 0) {
    Toast.makeText(this, "Wybrano pierwszy gatunek", Toast.LENGTH_SHORT).show();
}
```

Jeszcze jeden przyklad reakcji:

```java
spinnerGatunki.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
    @Override
    public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
        String wybrany = parent.getItemAtPosition(position).toString();
        tvPromocjaWartosc.setText("Wybrany: " + wybrany);
    }

    @Override
    public void onNothingSelected(AdapterView<?> parent) {
    }
});
```

Nie musisz tego zawsze dawac, ale to pokazuje, ze `Spinner` moze byc aktywny od razu, a nie dopiero przy kliknieciu przycisku.

## 5. `SeekBar` w `MainActivity`

Przyklad z ksiazek:

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

Przyklad z ocen:

```java
seekBarWaga.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
    @Override
    public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
        tvWagaWartosc.setText(String.valueOf(progress + 1));
    }

    @Override
    public void onStartTrackingTouch(SeekBar seekBar) {
    }

    @Override
    public void onStopTrackingTouch(SeekBar seekBar) {
    }
});
```

Tu widac bardzo wazna rzecz:

- czasem pokazujesz `progress`,
- czasem `progress + 1`.

Czyli `MainActivity` nie tylko bierze wartosc, ale czasem ja poprawia do potrzeb zadania.

## 6. Przycisk i wywolanie metody

Z obu projektow masz dwa poprawne style:

```java
btnWyslij.setOnClickListener(v -> wyslijFormularz());
```

albo:

```java
btnDodaj.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        dodajOcene();
    }
});
```

Pierwsza wersja jest krotsza.  
Druga wersja jest bardziej klasyczna i czesto spotykana w starszych przykladach.

## 7. Odczyt danych z `EditText`

Bardzo czesty fragment:

```java
String tytul = etTytul.getText().toString();
String autor = etAutor.getText().toString();
String cenaTekst = etCena.getText().toString();
double cena = Double.parseDouble(cenaTekst);
```

Bezpieczniejsza wersja:

```java
String cenaTekst = etCena.getText().toString().trim();

if (cenaTekst.isEmpty()) {
    Toast.makeText(this, "Wpisz cene", Toast.LENGTH_SHORT).show();
    return;
}

double cena = Double.parseDouble(cenaTekst);
```

To jest lepsze niz od razu `Double.parseDouble(...)`, bo najpierw sprawdzasz, czy pole nie jest puste.

## 8. Walidacja formularza

W `MainActivity` walidacja jest bardzo wazna.

Przyklad:

```java
if (etTytul.getText().toString().trim().isEmpty()) {
    Toast.makeText(this, "Wpisz tytul", Toast.LENGTH_SHORT).show();
    return;
}

if (etAutor.getText().toString().trim().isEmpty()) {
    Toast.makeText(this, "Wpisz autora", Toast.LENGTH_SHORT).show();
    return;
}

if (etCena.getText().toString().trim().isEmpty()) {
    Toast.makeText(this, "Wpisz cene", Toast.LENGTH_SHORT).show();
    return;
}
```

W projekcie ocen dobrze bylo zrobione tez sprawdzenie `RadioGroup`:

```java
if (grupaTypu.getCheckedRadioButtonId() == -1) {
    Toast.makeText(this, "Uzupelnij kategorie oceny!", Toast.LENGTH_SHORT).show();
    return;
}
```

To sa przyklady prostych warunkow, ktore bardzo czesto pojawiaja sie na lekcjach.

## 9. `CheckBox` i skladanie tekstu

Przyklad z `lek15_ksiazka`:

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
```

Finalna wartosc:

```java
String wynikDostepnosci = dostepnosc.toString().trim();
```

Jesli nic nie zaznaczono, mozna dodac jeszcze taka wersje:

```java
if (wynikDostepnosci.isEmpty()) {
    wynikDostepnosci = "Brak";
}
```

To jest dobry dodatek, zeby na liscie nie bylo pustego miejsca.

## 10. `Switch` i wartosc logiczna

Przyklad:

```java
boolean czyNowa = switchCzyNowa.isChecked();
```

Mozesz to wykorzystac od razu przy tworzeniu obiektu:

```java
Book ksiazka = new Book(
        tytul,
        autor,
        gatunek,
        switchCzyNowa.isChecked(),
        cena,
        promocja,
        wynikDostepnosci,
        kategoriaWiekowa
);
```

Albo osobno:

```java
boolean czyNowa = switchCzyNowa.isChecked();
String status = czyNowa ? "Nowa ksiazka" : "Uzywana albo zwykla";
Toast.makeText(this, status, Toast.LENGTH_SHORT).show();
```

## 11. `RadioButton` i `RadioGroup`

Przyklad z ksiazki:

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

Przyklad z ocen:

```java
int wybraneRadioId = grupaTypu.getCheckedRadioButtonId();
String typOceny;

if (wybraneRadioId == R.id.radioSpr) {
    typOceny = "Spr";
} else if (wybraneRadioId == R.id.radioOdp) {
    typOceny = "Odp";
} else {
    typOceny = "Akt";
}
```

To dwa rozne style:

- sprawdzanie `isChecked()`,
- sprawdzanie `getCheckedRadioButtonId()`.

Oba sa dobre.  
Drugi styl jest czesto wygodniejszy, gdy chcesz najpierw sprawdzic, czy cos wybrano.

## 12. Tworzenie obiektu na podstawie formularza

To jest moment, w ktorym dane z widokow zamieniaja sie w jeden obiekt.

Przyklad z ksiazka:

```java
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

Przyklad z ocena:

```java
Grade grade = new Grade(
        ocena,
        waga,
        typOceny,
        liczDoSredniej
);
```

To bardzo wazne, bo tu widac sens modelu:

- formularz zbiera dane,
- obiekt trzyma dane,
- adapter pokazuje dane.

## 13. `ListView` w `MainActivity`

W obu Twoich projektach lista jest bardzo wazna.

Najpierw tworzysz pusta liste:

```java
private List<Book> listaKsiazek = new ArrayList<>();
```

Potem adapter:

```java
adapter = new BookAdapter(this, listaKsiazek);
```

Potem podpinasz do `ListView`:

```java
listViewRekordy.setAdapter(adapter);
```

Potem dodajesz dane:

```java
listaKsiazek.add(0, ksiazka);
adapter.notifyDataSetChanged();
```

### Co daje `add(0, ksiazka)`?

Nowy element trafia na poczatek listy.

Czyli najnowsza ksiazka jest u gory.

### Co daje zwykle `add(ksiazka)`?

Nowy element trafia na koniec listy.

To tez jest poprawne.

### Dodatkowy praktyczny przyklad z klikaniem elementu

```java
listViewRekordy.setOnItemClickListener((parent, view, position, id) -> {
    Book kliknieta = listaKsiazek.get(position);
    Toast.makeText(this, "Kliknieto: " + kliknieta.getTytul(), Toast.LENGTH_SHORT).show();
});
```

### Dodatkowy przyklad z dlugim kliknieciem

```java
listViewRekordy.setOnItemLongClickListener((parent, view, position, id) -> {
    listaKsiazek.remove(position);
    adapter.notifyDataSetChanged();
    Toast.makeText(this, "Usunieto element", Toast.LENGTH_SHORT).show();
    return true;
});
```

To nie bylo w repo, ale jest bardzo przydatne, bo pokazuje, ze `ListView` umie nie tylko wyswietlac, ale tez reagowac na akcje uzytkownika.

## 14. Pelny przyklad metody `wyslijFormularz()`

Rozbudowany przyklad oparty na Twoim projekcie:

```java
private void wyslijFormularz() {
    if (etTytul.getText().toString().trim().isEmpty()) {
        Toast.makeText(this, "Wpisz tytul", Toast.LENGTH_SHORT).show();
        return;
    }

    if (etAutor.getText().toString().trim().isEmpty()) {
        Toast.makeText(this, "Wpisz autora", Toast.LENGTH_SHORT).show();
        return;
    }

    if (etCena.getText().toString().trim().isEmpty()) {
        Toast.makeText(this, "Wpisz cene", Toast.LENGTH_SHORT).show();
        return;
    }

    String tytul = etTytul.getText().toString();
    String autor = etAutor.getText().toString();
    String gatunek = spinnerGatunki.getSelectedItem().toString();
    boolean czNowa = switchCzyNowa.isChecked();
    double cena = Double.parseDouble(etCena.getText().toString());
    int promocja = seekBarPromocja.getProgress();

    StringBuilder dostepnosc = new StringBuilder();
    if (checkBoxPapier.isChecked()) dostepnosc.append("Papier ");
    if (checkBoxMobi.isChecked()) dostepnosc.append("Mobi ");
    if (checkBoxAudiobook.isChecked()) dostepnosc.append("Audiobook ");
    if (checkBoxPdf.isChecked()) dostepnosc.append("PDF ");

    String kategoriaWiekowa = "";
    if (radio18plus.isChecked()) kategoriaWiekowa = "18+";
    else if (radio12plus.isChecked()) kategoriaWiekowa = "12+";
    else if (radio9plus.isChecked()) kategoriaWiekowa = "9+";
    else if (radio0plus.isChecked()) kategoriaWiekowa = "0+";

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

    listaKsiazek.add(0, ksiazka);
    adapter.notifyDataSetChanged();
    wyczyscFormularz();
    Toast.makeText(this, "Dodano ksiazke!", Toast.LENGTH_SHORT).show();
}
```

To jest prawie caly najwazniejszy przeplyw programu w jednym miejscu.

## 15. Pelny przyklad metody `dodajOcene()`

Drugi wazny przyklad z repo ocen:

```java
private void dodajOcene() {
    if (grupaTypu.getCheckedRadioButtonId() == -1) {
        Toast.makeText(this, "Uzupelnij kategorie oceny!", Toast.LENGTH_SHORT).show();
        return;
    }

    String ocena = spinnerOcena.getSelectedItem().toString();
    int waga = seekBarWaga.getProgress() + 1;

    int wybraneRadioId = grupaTypu.getCheckedRadioButtonId();
    String typOceny;

    if (wybraneRadioId == R.id.radioSpr) {
        typOceny = "Spr";
    } else if (wybraneRadioId == R.id.radioOdp) {
        typOceny = "Odp";
    } else {
        typOceny = "Akt";
    }

    boolean liczDoSredniej = doSredniej.isChecked();

    listaOcenDanych.add(new Grade(ocena, waga, typOceny, liczDoSredniej));
    adapter.notifyDataSetChanged();

    resetFormularz();
    odswiezSrednia();
}
```

Tutaj widac bardzo ladnie:

- pobranie danych z `Spinner`,
- pobranie danych z `SeekBar`,
- sprawdzenie `RadioGroup`,
- pobranie danych z `CheckBox`,
- utworzenie obiektu `Grade`,
- odswiezenie listy,
- wyczyszczenie formularza,
- przeliczenie sredniej.

## 16. Liczenie sredniej w `MainActivity`

To juz nie jest samo wyswietlanie formularza, ale bardzo dobry przyklad pracy na liscie danych.

```java
private void odswiezSrednia() {
    double suma = 0;
    int sumaWag = 0;

    for (Grade ocena : listaOcenDanych) {
        if (ocena.isCzyDoSredniej()) {
            suma += Double.parseDouble(ocena.getGrades()) * ocena.getWeight();
            sumaWag += ocena.getWeight();
        }
    }

    if (sumaWag > 0) {
        tvSrednia.setText(String.format("Srednia ocen: %.2f", suma / sumaWag));
    } else {
        tvSrednia.setText("Srednia ocen: brak ocen");
    }
}
```

Ten fragment warto znac, bo pokazuje:

- petle `for`,
- warunek `if`,
- pobieranie danych z obiektu,
- aktualizacje `TextView`.

To znaczy, ze `MainActivity` nie tylko zbiera dane, ale tez moze cos obliczac.

## 17. Czyszczenie formularza

Przyklad z ksiazek:

```java
private void wyczyscFormularz() {
    etTytul.setText("");
    etAutor.setText("");
    etCena.setText("");
    spinnerGatunki.setSelection(0);
    switchCzyNowa.setChecked(false);
    seekBarPromocja.setProgress(0);
    tvPromocjaWartosc.setText("0%");
    checkBoxPapier.setChecked(false);
    checkBoxMobi.setChecked(false);
    checkBoxAudiobook.setChecked(false);
    checkBoxPdf.setChecked(false);
    radioGroupKategoria.clearCheck();
}
```

Przyklad z ocen:

```java
private void resetFormularz() {
    spinnerOcena.setSelection(0);
    seekBarWaga.setProgress(0);
    grupaTypu.clearCheck();
    doSredniej.setChecked(false);
    tvWagaWartosc.setText("1");
}
```

To tez warto umiec, bo nauczyciele bardzo czesto zwracaja uwage, czy formularz wraca do stanu poczatkowego.

## 18. Najczestszy schemat `MainActivity` przy formularzach

W praktyce bardzo czesto bedziesz robil taki uklad:

1. deklaracja widokow i danych,
2. `setContentView(...)`,
3. `ini()`,
4. ustawienie spinnera,
5. ustawienie adaptera do `ListView`,
6. ustawienie listenerow,
7. metoda do wyslania danych,
8. metoda do czyszczenia formularza,
9. ewentualnie metoda dodatkowa, np. liczenie sredniej.

## 19. Co warto umiec z `MainActivity`

Po ogarnieciu tego pliku dobrze umiec:

1. napisac `ini()` z `findViewById(...)`,
2. ustawic `Spinner` przez `ArrayAdapter`,
3. podpiac przycisk `setOnClickListener(...)`,
4. odczytac dane z `EditText`, `CheckBox`, `Switch`, `RadioButton`, `SeekBar`,
5. stworzyc obiekt `Book` albo `Grade`,
6. dodac obiekt do listy i odswiezyc `ListView`,
7. wyczyscic formularz,
8. zrobic prosta walidacje.

Jesli to umiesz, to spokojnie zrobisz bardzo duzo prostych i srednich zadan z formularzy w Androidzie.
