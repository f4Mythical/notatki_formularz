# 05. Triki

Ten plik to zbior gotowych pomyslow, ktore mozesz dopinac do formularza albo `ListView`.

Podzielilem to na:

1. akcje na klikniecie,
2. akcje na long click,
3. funkcje do liczenia,
4. dodatkowe akcje nie tylko na klikniecie.

Wiele przykladow pasuje od razu do projektu `lek16_oceny`, ale czesc spokojnie przerobisz tez na `lek15_ksiazka`.

## 1. Akcje na klikniecie

Te akcje najczesciej dopinasz po:

```java
listaOcen.setAdapter(adapter);
```

albo:

```java
listViewRekordy.setAdapter(adapter);
```

### 1.1 Pokaz szczegoly elementu w `Toast`

```java
listaOcen.setOnItemClickListener((parent, view, position, id) -> {
    Grade ocena = listaOcenDanych.get(position);
    Toast.makeText(this,
            "Ocena: " + ocena.getGrades()
                    + ", waga: " + ocena.getWeight()
                    + ", typ: " + ocena.getGradeType(),
            Toast.LENGTH_SHORT).show();
});
```

To najprostsza akcja: klikam element i pokazuje jego dane.

### 1.2 Przelacz `czyDoSredniej`

Wymaga metody `setCzyDoSredniej(boolean)` w klasie `Grade`.

```java
listaOcen.setOnItemClickListener((parent, view, position, id) -> {
    Grade ocena = listaOcenDanych.get(position);
    ocena.setCzyDoSredniej(!ocena.isCzyDoSredniej());
    adapter.notifyDataSetChanged();
    odswiezSrednia();
});
```

To bardzo fajny trik, bo jeden klik zmienia stan oceny.

### 1.3 Zaladuj dane z listy z powrotem do formularza

```java
listaOcen.setOnItemClickListener((parent, view, position, id) -> {
    Grade ocena = listaOcenDanych.get(position);

    spinnerOcena.setSelection(Integer.parseInt(ocena.getGrades()) - 1);
    seekBarWaga.setProgress(ocena.getWeight() - 1);
    doSredniej.setChecked(ocena.isCzyDoSredniej());

    if (ocena.getGradeType().equals("Spr")) {
        radioSprawdzian.setChecked(true);
    } else if (ocena.getGradeType().equals("Odp")) {
        radioOdpowiedz.setChecked(true);
    } else {
        radioAktywnosc.setChecked(true);
    }
});
```

To jest bardzo przydatne, gdy chcesz zrobic cos podobnego do edycji.

### 1.4 Podbij wage o 1

Wymaga metody `setWeight(int)` w klasie `Grade`.

```java
listaOcen.setOnItemClickListener((parent, view, position, id) -> {
    Grade ocena = listaOcenDanych.get(position);
    ocena.setWeight(ocena.getWeight() + 1);
    adapter.notifyDataSetChanged();
    odswiezSrednia();
    Toast.makeText(this, "Nowa waga: " + ocena.getWeight(), Toast.LENGTH_SHORT).show();
});
```

Mozesz tez zrobic wersje bezpieczna:

```java
if (ocena.getWeight() < 10) {
    ocena.setWeight(ocena.getWeight() + 1);
}
```

### 1.5 Obniz wage o 1

Wymaga `setWeight(int)`.

```java
listaOcen.setOnItemClickListener((parent, view, position, id) -> {
    Grade ocena = listaOcenDanych.get(position);

    if (ocena.getWeight() > 1) {
        ocena.setWeight(ocena.getWeight() - 1);
        adapter.notifyDataSetChanged();
        odswiezSrednia();
    }
});
```

### 1.6 Zmien typ oceny cyklicznie

Wymaga `setGradeType(String)`.

```java
listaOcen.setOnItemClickListener((parent, view, position, id) -> {
    Grade ocena = listaOcenDanych.get(position);

    switch (ocena.getGradeType()) {
        case "Spr":
            ocena.setGradeType("Odp");
            break;
        case "Odp":
            ocena.setGradeType("Akt");
            break;
        default:
            ocena.setGradeType("Spr");
            break;
    }

    adapter.notifyDataSetChanged();
});
```

### 1.7 Skopiuj element

```java
listaOcen.setOnItemClickListener((parent, view, position, id) -> {
    Grade ocena = listaOcenDanych.get(position);

    listaOcenDanych.add(new Grade(
            ocena.getGrades(),
            ocena.getWeight(),
            ocena.getGradeType(),
            ocena.isCzyDoSredniej()
    ));

    adapter.notifyDataSetChanged();
    odswiezSrednia();
    Toast.makeText(this, "Skopiowano ocene", Toast.LENGTH_SHORT).show();
});
```

### 1.8 Pokaz pozycje kliknietego elementu

```java
listaOcen.setOnItemClickListener((parent, view, position, id) -> {
    Toast.makeText(this, "Kliknieto element nr " + (position + 1), Toast.LENGTH_SHORT).show();
});
```

### 1.9 Dla ksiazki: pokaz sam tytul

```java
listViewRekordy.setOnItemClickListener((parent, view, position, id) -> {
    Book ksiazka = listaKsiazek.get(position);
    Toast.makeText(this, "Tytul: " + ksiazka.getTytul(), Toast.LENGTH_SHORT).show();
});
```

### 1.10 Dla ksiazki: doladuj rekord do formularza

To wymaga podobnej logiki jak w ocenach.

```java
listViewRekordy.setOnItemClickListener((parent, view, position, id) -> {
    Book ksiazka = listaKsiazek.get(position);

    etTytul.setText(ksiazka.getTytul());
    etAutor.setText(ksiazka.getAutor());
    etCena.setText(String.valueOf(ksiazka.getCena()));
    seekBarPromocja.setProgress(ksiazka.getPromocja());
    tvPromocjaWartosc.setText(ksiazka.getPromocja() + "%");
    switchCzyNowa.setChecked(ksiazka.isCzNowa());
});
```

## 2. Akcje na long click

Long click bardzo dobrze pasuje do akcji mocniejszych, np. usuniecie albo reset jednego elementu.

### 2.1 Usun element z listy

```java
listaOcen.setOnItemLongClickListener((parent, view, position, id) -> {
    listaOcenDanych.remove(position);
    adapter.notifyDataSetChanged();
    odswiezSrednia();
    Toast.makeText(this, "Usunieto ocene", Toast.LENGTH_SHORT).show();
    return true;
});
```

### 2.2 Usun ksiazke z listy

```java
listViewRekordy.setOnItemLongClickListener((parent, view, position, id) -> {
    Book usunieta = listaKsiazek.remove(position);
    adapter.notifyDataSetChanged();
    Toast.makeText(this, "Usunieto: " + usunieta.getTytul(), Toast.LENGTH_SHORT).show();
    return true;
});
```

### 2.3 Wyzeruj wage oceny do 1

Wymaga `setWeight(int)`.

```java
listaOcen.setOnItemLongClickListener((parent, view, position, id) -> {
    Grade ocena = listaOcenDanych.get(position);
    ocena.setWeight(1);
    adapter.notifyDataSetChanged();
    odswiezSrednia();
    Toast.makeText(this, "Ustawiono wage 1", Toast.LENGTH_SHORT).show();
    return true;
});
```

### 2.4 Ustaw promocje ksiazki na 0

Wymaga `setPromocja(int)` w klasie `Book`.

```java
listViewRekordy.setOnItemLongClickListener((parent, view, position, id) -> {
    Book ksiazka = listaKsiazek.get(position);
    ksiazka.setPromocja(0);
    adapter.notifyDataSetChanged();
    Toast.makeText(this, "Usunieto promocje", Toast.LENGTH_SHORT).show();
    return true;
});
```

### 2.5 Oznacz wszystkie oceny jako do sredniej

```java
btnDodaj.setOnLongClickListener(v -> {
    for (Grade ocena : listaOcenDanych) {
        ocena.setCzyDoSredniej(true);
    }
    adapter.notifyDataSetChanged();
    odswiezSrednia();
    Toast.makeText(this, "Wszystkie oceny licza sie do sredniej", Toast.LENGTH_SHORT).show();
    return true;
});
```

To jest przyklad long click nie na `ListView`, tylko na przycisku.

## 3. Funkcje do liczenia

To sa dodatkowe metody, ktore mozesz wstawic do `MainActivity`.

## 3.1 Suma wszystkich wag

```java
private int policzSumeWag() {
    int suma = 0;

    for (Grade ocena : listaOcenDanych) {
        suma += ocena.getWeight();
    }

    return suma;
}
```

Uzycie:

```java
Toast.makeText(this, "Suma wag: " + policzSumeWag(), Toast.LENGTH_SHORT).show();
```

### 3.2 Suma ocen liczonych jako liczby

```java
private double policzSumeOcen() {
    double suma = 0;

    for (Grade ocena : listaOcenDanych) {
        suma += Double.parseDouble(ocena.getGrades());
    }

    return suma;
}
```

### 3.3 Srednia zwykla bez wag

```java
private double policzSredniaZwykla() {
    if (listaOcenDanych.isEmpty()) {
        return 0;
    }

    double suma = 0;

    for (Grade ocena : listaOcenDanych) {
        suma += Double.parseDouble(ocena.getGrades());
    }

    return suma / listaOcenDanych.size();
}
```

### 3.4 Srednia wazona tylko z zaznaczonych

To podobne do Twojego projektu, ale jako osobna metoda:

```java
private double policzSredniaWazona() {
    double suma = 0;
    int sumaWag = 0;

    for (Grade ocena : listaOcenDanych) {
        if (ocena.isCzyDoSredniej()) {
            suma += Double.parseDouble(ocena.getGrades()) * ocena.getWeight();
            sumaWag += ocena.getWeight();
        }
    }

    if (sumaWag == 0) {
        return 0;
    }

    return suma / sumaWag;
}
```

### 3.5 Najwieksza ocena

```java
private double znajdzNajwyzszaOcene() {
    double max = 0;

    for (Grade ocena : listaOcenDanych) {
        double wartosc = Double.parseDouble(ocena.getGrades());
        if (wartosc > max) {
            max = wartosc;
        }
    }

    return max;
}
```

### 3.6 Najnizsza ocena

```java
private double znajdzNajnizszaOcene() {
    if (listaOcenDanych.isEmpty()) {
        return 0;
    }

    double min = Double.parseDouble(listaOcenDanych.get(0).getGrades());

    for (Grade ocena : listaOcenDanych) {
        double wartosc = Double.parseDouble(ocena.getGrades());
        if (wartosc < min) {
            min = wartosc;
        }
    }

    return min;
}
```

### 3.7 Ile ocen jest liczonych do sredniej

```java
private int policzOcenyDoSredniej() {
    int licznik = 0;

    for (Grade ocena : listaOcenDanych) {
        if (ocena.isCzyDoSredniej()) {
            licznik++;
        }
    }

    return licznik;
}
```

### 3.8 Ile ksiazek jest nowych

```java
private int policzNoweKsiazki() {
    int licznik = 0;

    for (Book ksiazka : listaKsiazek) {
        if (ksiazka.isCzNowa()) {
            licznik++;
        }
    }

    return licznik;
}
```

### 3.9 Suma cen wszystkich ksiazek

```java
private double policzSumeCenKsiazek() {
    double suma = 0;

    for (Book ksiazka : listaKsiazek) {
        suma += ksiazka.getCena();
    }

    return suma;
}
```

### 3.10 Cena po promocji dla jednej ksiazki

```java
private double policzCenePoPromocji(Book ksiazka) {
    return ksiazka.getCena() * (1 - ksiazka.getPromocja() / 100.0);
}
```

### 3.11 Srednia promocja ksiazek

```java
private double policzSredniaPromocje() {
    if (listaKsiazek.isEmpty()) {
        return 0;
    }

    int suma = 0;

    for (Book ksiazka : listaKsiazek) {
        suma += ksiazka.getPromocja();
    }

    return (double) suma / listaKsiazek.size();
}
```

## 4. Przykladowe uzycie funkcji na klikniecie

### 4.1 Pokaz sume wag

```java
btnDodaj.setOnClickListener(v -> {
    Toast.makeText(this, "Suma wag: " + policzSumeWag(), Toast.LENGTH_SHORT).show();
});
```

### 4.2 Pokaz srednia zwykla

```java
btnDodaj.setOnClickListener(v -> {
    Toast.makeText(this,
            String.format("Srednia zwykla: %.2f", policzSredniaZwykla()),
            Toast.LENGTH_SHORT).show();
});
```

### 4.3 Pokaz najwyzsza i najnizsza ocene

```java
btnDodaj.setOnClickListener(v -> {
    String tekst = "Max: " + znajdzNajwyzszaOcene() + ", Min: " + znajdzNajnizszaOcene();
    Toast.makeText(this, tekst, Toast.LENGTH_SHORT).show();
});
```

### 4.4 Pokaz ile masz nowych ksiazek

```java
btnWyslij.setOnLongClickListener(v -> {
    Toast.makeText(this, "Nowe ksiazki: " + policzNoweKsiazki(), Toast.LENGTH_SHORT).show();
    return true;
});
```

## 5. Dodatkowe akcje nie tylko na klikniecie

Tu sa akcje, ktore dzieja sie nie tylko po nacisnieciu przycisku.

### 5.1 `SeekBar` - akcja przy przesuwaniu

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
        Toast.makeText(MainActivity.this, "Ustawiono promocje", Toast.LENGTH_SHORT).show();
    }
});
```

### 5.2 `Spinner` - akcja po zmianie wyboru

```java
spinnerGatunki.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
    @Override
    public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
        String wybrany = parent.getItemAtPosition(position).toString();
        Toast.makeText(MainActivity.this, "Wybrano: " + wybrany, Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onNothingSelected(AdapterView<?> parent) {
    }
});
```

### 5.3 `RadioGroup` - akcja po zmianie zaznaczenia

```java
radioGroupKategoria.setOnCheckedChangeListener((group, checkedId) -> {
    if (checkedId == R.id.radio18plus) {
        Toast.makeText(this, "Wybrano 18+", Toast.LENGTH_SHORT).show();
    } else if (checkedId == R.id.radio12plus) {
        Toast.makeText(this, "Wybrano 12+", Toast.LENGTH_SHORT).show();
    }
});
```

### 5.4 `CheckBox` - akcja po zaznaczeniu

```java
checkBoxPdf.setOnCheckedChangeListener((buttonView, isChecked) -> {
    if (isChecked) {
        Toast.makeText(this, "Zaznaczono PDF", Toast.LENGTH_SHORT).show();
    } else {
        Toast.makeText(this, "Odznaczono PDF", Toast.LENGTH_SHORT).show();
    }
});
```

### 5.5 `Switch` - akcja po zmianie stanu

```java
switchCzyNowa.setOnCheckedChangeListener((buttonView, isChecked) -> {
    String tekst = isChecked ? "Ksiazka oznaczona jako nowa" : "Ksiazka nie jest nowa";
    Toast.makeText(this, tekst, Toast.LENGTH_SHORT).show();
});
```

### 5.6 `TextWatcher` - akcja podczas wpisywania tekstu

```java
etTytul.addTextChangedListener(new TextWatcher() {
    @Override
    public void beforeTextChanged(CharSequence s, int start, int count, int after) {
    }

    @Override
    public void onTextChanged(CharSequence s, int start, int before, int count) {
        tvPromocjaWartosc.setText("Dlugosc: " + s.length());
    }

    @Override
    public void afterTextChanged(Editable s) {
    }
});
```

To juz jest bardzo fajny trik, bo pokazuje akcje dziejaca sie podczas wpisywania.

## 6. Co trzeba czasem dodac do klas modelu

Niektore triki wymagaja setterow.

### Do `Grade`

```java
public void setWeight(int weight) {
    this.weight = weight;
}

public void setGradeType(String gradeType) {
    this.gradeType = gradeType;
}

public void setCzyDoSredniej(boolean czyDoSredniej) {
    this.czyDoSredniej = czyDoSredniej;
}
```

### Do `Book`

```java
public void setPromocja(int promocja) {
    this.promocja = promocja;
}
```

## 7. Najwazniejsza mysl z tego pliku

W Androidzie formularz nie konczy sie na przycisku `Dodaj` albo `Wyslij`.

Mozesz dodawac:

1. klik na element listy,
2. long click,
3. dodatkowe funkcje do liczenia,
4. reakcje na `Spinner`, `SeekBar`, `RadioGroup`, `CheckBox`, `Switch`, `EditText`.

To wlasnie takie male triki sprawiaja, ze projekt wyglada bardziej jak prawdziwa aplikacja.
