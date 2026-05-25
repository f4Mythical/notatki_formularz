# 06. Przykladowe

Ten plik zawiera tylko jeden pelny przyklad:  
`Uklad hormonalny - quiz`

Wszystko jest rozpisane w 5 czesciach:

1. uklad zadania,
2. `activity_main.xml`,
3. `MainActivity.java`,
4. plik dodatkowy `item_hormon.xml`,
5. adapter `HormoneAdapter.java`.

## 1. Uklad

To jest skrot zadania w prostym zapisie:

```txt
// 1. Naglowek
TextView text="Uklad hormonalny - quiz"

// 2. EditText - wpisz nazwe hormonu
EditText hint="Wpisz nazwe hormonu trzustki" inputType=text

// 3. Spinner - wybierz gruczol
Spinner entries=["Przysadka","Tarczyca","Trzustka","Nadnercza","Szyszynka","Przytarczyce","Jajniki","Jadra"]

// 4. RadioGroup - typ hormonu
RadioGroup
RadioButton text="Steroidowy"
RadioButton text="Niesteroidowy"

// 5. CheckBox - dzialanie hormonu
CheckBox text="Obniza poziom glukozy"
CheckBox text="Podwyzsza poziom wapnia"
CheckBox text="Przyspiesza metabolizm"
CheckBox text="Reguluje rytm snu"
CheckBox text="Wywoluje skurcze macicy"

// 6. EditText - opis dzialania
EditText hint="Opisz dzialanie wybranego hormonu" inputType=textMultiLine lines=3

// 7. Button - sprawdz odpowiedzi
Button text="Sprawdz" onClick=sprawdzOdpowiedzi()

// 8. TextView - wynik
TextView id=tvWynik visibility=gone

// 9. ListView - lista hormonow i gruczolow
ListView id=listHormony

// 10. Button - resetuj formularz
Button text="Resetuj" onClick=resetujFormularz()
```

## 2. XML z `ListView`

To moze byc pelny plik `activity_main.xml`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="16dp">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Uklad hormonalny - quiz"
            android:textSize="20sp"
            android:textStyle="bold"
            android:layout_marginBottom="16dp" />

        <EditText
            android:id="@+id/etHormon"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Wpisz nazwe hormonu trzustki"
            android:inputType="text"
            android:layout_marginBottom="12dp" />

        <Spinner
            android:id="@+id/spinnerGruczol"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="12dp" />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Typ hormonu"
            android:textStyle="bold"
            android:layout_marginBottom="6dp" />

        <RadioGroup
            android:id="@+id/radioGroupTyp"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="12dp">

            <RadioButton
                android:id="@+id/radioSteroidowy"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Steroidowy" />

            <RadioButton
                android:id="@+id/radioNiesteroidowy"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Niesteroidowy" />

        </RadioGroup>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Dzialanie hormonu"
            android:textStyle="bold"
            android:layout_marginBottom="6dp" />

        <CheckBox
            android:id="@+id/checkGlukoza"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Obniza poziom glukozy" />

        <CheckBox
            android:id="@+id/checkWapn"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Podwyzsza poziom wapnia" />

        <CheckBox
            android:id="@+id/checkMetabolizm"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Przyspiesza metabolizm" />

        <CheckBox
            android:id="@+id/checkSen"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Reguluje rytm snu" />

        <CheckBox
            android:id="@+id/checkSkurcze"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Wywoluje skurcze macicy"
            android:layout_marginBottom="12dp" />

        <EditText
            android:id="@+id/etOpis"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Opisz dzialanie wybranego hormonu"
            android:inputType="textMultiLine"
            android:minLines="3"
            android:gravity="top"
            android:layout_marginBottom="12dp" />

        <Button
            android:id="@+id/btnSprawdz"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Sprawdz"
            android:layout_marginBottom="8dp" />

        <TextView
            android:id="@+id/tvWynik"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textStyle="bold"
            android:visibility="gone"
            android:padding="8dp"
            android:layout_marginBottom="12dp" />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Lista hormonow"
            android:textStyle="bold"
            android:layout_marginBottom="8dp" />

        <ListView
            android:id="@+id/listHormony"
            android:layout_width="match_parent"
            android:layout_height="220dp"
            android:layout_marginBottom="12dp" />

        <Button
            android:id="@+id/btnResetuj"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Resetuj" />

    </LinearLayout>

</ScrollView>
```

## 3. `MainActivity.java`

Tutaj jest kompletna logika do tego formularza.

```java
package com.example.hormony;

import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.EditText;
import android.widget.ListView;
import android.widget.RadioButton;
import android.widget.RadioGroup;
import android.widget.Spinner;
import android.widget.TextView;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {

    private EditText etHormon, etOpis;
    private Spinner spinnerGruczol;
    private RadioGroup radioGroupTyp;
    private RadioButton radioSteroidowy, radioNiesteroidowy;
    private CheckBox checkGlukoza, checkWapn, checkMetabolizm, checkSen, checkSkurcze;
    private Button btnSprawdz, btnResetuj;
    private TextView tvWynik;
    private ListView listHormony;

    private List<String> listaHormonow = new ArrayList<>();
    private HormoneAdapter adapter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ini();
        ustawSpinner();
        ustawListe();

        btnSprawdz.setOnClickListener(v -> sprawdzOdpowiedzi());
        btnResetuj.setOnClickListener(v -> resetujFormularz());

        listHormony.setOnItemClickListener((parent, view, position, id) -> {
            String element = listaHormonow.get(position);
            String[] parts = element.split("\\\\|");

            if (parts.length == 2) {
                spinnerGruczol.setSelection(znajdzPozycjeGruczolu(parts[0]));
                etHormon.setText(parts[1]);
                Toast.makeText(this, "Wczytano: " + parts[1], Toast.LENGTH_SHORT).show();
            }
        });

        listHormony.setOnItemLongClickListener((parent, view, position, id) -> {
            String usuniety = listaHormonow.remove(position);
            adapter.notifyDataSetChanged();
            Toast.makeText(this, "Usunieto: " + usuniety.replace("|", " -> "), Toast.LENGTH_SHORT).show();
            return true;
        });
    }

    private void ini() {
        etHormon = findViewById(R.id.etHormon);
        etOpis = findViewById(R.id.etOpis);
        spinnerGruczol = findViewById(R.id.spinnerGruczol);
        radioGroupTyp = findViewById(R.id.radioGroupTyp);
        radioSteroidowy = findViewById(R.id.radioSteroidowy);
        radioNiesteroidowy = findViewById(R.id.radioNiesteroidowy);
        checkGlukoza = findViewById(R.id.checkGlukoza);
        checkWapn = findViewById(R.id.checkWapn);
        checkMetabolizm = findViewById(R.id.checkMetabolizm);
        checkSen = findViewById(R.id.checkSen);
        checkSkurcze = findViewById(R.id.checkSkurcze);
        btnSprawdz = findViewById(R.id.btnSprawdz);
        btnResetuj = findViewById(R.id.btnResetuj);
        tvWynik = findViewById(R.id.tvWynik);
        listHormony = findViewById(R.id.listHormony);
    }

    private void ustawSpinner() {
        String[] gruczoly = {
                "Przysadka",
                "Tarczyca",
                "Trzustka",
                "Nadnercza",
                "Szyszynka",
                "Przytarczyce",
                "Jajniki",
                "Jadra"
        };

        ArrayAdapter<String> spinnerAdapter = new ArrayAdapter<>(
                this,
                android.R.layout.simple_spinner_item,
                gruczoly
        );

        spinnerAdapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
        spinnerGruczol.setAdapter(spinnerAdapter);
    }

    private void ustawListe() {
        listaHormonow.add("Trzustka|Insulina");
        listaHormonow.add("Tarczyca|Tyroksyna");
        listaHormonow.add("Szyszynka|Melatonina");
        listaHormonow.add("Jajniki|Estrogen");
        listaHormonow.add("Nadnercza|Adrenalina");

        adapter = new HormoneAdapter(this, listaHormonow);
        listHormony.setAdapter(adapter);
    }

    private void sprawdzOdpowiedzi() {
        if (etHormon.getText().toString().trim().isEmpty()) {
            Toast.makeText(this, "Wpisz nazwe hormonu", Toast.LENGTH_SHORT).show();
            return;
        }

        if (radioGroupTyp.getCheckedRadioButtonId() == -1) {
            Toast.makeText(this, "Wybierz typ hormonu", Toast.LENGTH_SHORT).show();
            return;
        }

        String hormon = etHormon.getText().toString().trim();
        String gruczol = spinnerGruczol.getSelectedItem().toString();
        String opis = etOpis.getText().toString().trim();

        int punkty = 0;
        StringBuilder komentarz = new StringBuilder();

        if (hormon.equalsIgnoreCase("Insulina")) {
            punkty++;
        } else {
            komentarz.append("- Poprawna nazwa hormonu to Insulina\n");
        }

        if (gruczol.equals("Trzustka")) {
            punkty++;
        } else {
            komentarz.append("- Poprawny gruczol to Trzustka\n");
        }

        if (radioNiesteroidowy.isChecked()) {
            punkty++;
        } else {
            komentarz.append("- Insulina jest hormonem niesteroidowym\n");
        }

        if (checkGlukoza.isChecked()) {
            punkty++;
        } else {
            komentarz.append("- Zaznacz: Obniza poziom glukozy\n");
        }

        if (!opis.isEmpty()) {
            punkty++;
        } else {
            komentarz.append("- Dopisz krotki opis dzialania hormonu\n");
        }

        tvWynik.setVisibility(View.VISIBLE);
        tvWynik.setText("Wynik: " + punkty + "/5\n" + komentarz.toString().trim());
    }

    private void resetujFormularz() {
        etHormon.setText("");
        etOpis.setText("");
        spinnerGruczol.setSelection(0);
        radioGroupTyp.clearCheck();
        checkGlukoza.setChecked(false);
        checkWapn.setChecked(false);
        checkMetabolizm.setChecked(false);
        checkSen.setChecked(false);
        checkSkurcze.setChecked(false);
        tvWynik.setText("");
        tvWynik.setVisibility(View.GONE);
    }

    private int znajdzPozycjeGruczolu(String nazwa) {
        for (int i = 0; i < spinnerGruczol.getCount(); i++) {
            if (spinnerGruczol.getItemAtPosition(i).toString().equals(nazwa)) {
                return i;
            }
        }
        return 0;
    }
}
```

## 4. Plik dodatkowy - `item_hormon.xml`

To jest dodatkowy plik layoutu dla jednego elementu listy.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="10dp">

    <TextView
        android:id="@+id/tvGruczol"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textStyle="bold"
        android:textSize="16sp" />

    <TextView
        android:id="@+id/tvHormon"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="14sp"
        android:layout_marginTop="2dp" />

</LinearLayout>
```

Ten plik zapisujesz jako:

`res/layout/item_hormon.xml`

## 5. Adapter - `HormoneAdapter.java`

Ten adapter obsluguje `ListView` i korzysta z pliku `item_hormon.xml`.

```java
package com.example.hormony;

import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ArrayAdapter;
import android.widget.TextView;

import java.util.List;

public class HormoneAdapter extends ArrayAdapter<String> {

    public HormoneAdapter(Context context, List<String> hormony) {
        super(context, 0, hormony);
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        if (convertView == null) {
            convertView = LayoutInflater.from(getContext())
                    .inflate(R.layout.item_hormon, parent, false);
        }

        String element = getItem(position);
        String[] parts = element.split("\\\\|");

        TextView tvGruczol = convertView.findViewById(R.id.tvGruczol);
        TextView tvHormon = convertView.findViewById(R.id.tvHormon);

        if (parts.length == 2) {
            tvGruczol.setText("Gruczol: " + parts[0]);
            tvHormon.setText("Hormon: " + parts[1]);
        } else {
            tvGruczol.setText(element);
            tvHormon.setText("");
        }

        return convertView;
    }
}
```

## Jak to dziala razem

Schemat jest taki:

1. `activity_main.xml` robi wyglad ekranu,
2. `MainActivity.java` zbiera dane i sprawdza odpowiedzi,
3. `item_hormon.xml` robi wyglad jednego wiersza listy,
4. `HormoneAdapter.java` wypelnia ten wiersz danymi,
5. `ListView` pokazuje liste hormonow i gruczolow.

To jest juz gotowy, pelny przyklad podobny do tego, co robiles w `lek15_ksiazka` i `lek16_oceny`.

## 6. Wersja prostsza bez `item_hormon.xml` i bez wlasnego adaptera

Mozesz tez zrobic ten sam `ListView` prosciej:

- bez pliku `item_hormon.xml`,
- bez klasy `HormoneAdapter`,
- z uzyciem `toString()`.

To dobre rozwiazanie, gdy chcesz zrobic szybciej prostsze zadanie.

### 6.1 Dodatkowa klasa `Hormone.java`

W tej wersji zamiast `List<String>` lepiej uzyc normalnej klasy.

```java
package com.example.hormony;

public class Hormone {
    private String gruczol;
    private String hormon;

    public Hormone(String gruczol, String hormon) {
        this.gruczol = gruczol;
        this.hormon = hormon;
    }

    public String getGruczol() {
        return gruczol;
    }

    public String getHormon() {
        return hormon;
    }

    @Override
    public String toString() {
        return gruczol + " -> " + hormon;
    }
}
```

Najwazniejsze tutaj jest `toString()`.

To wlasnie ta metoda sprawia, ze `ListView` umie pokazac obiekt jako zwykly tekst.

### 6.2 Jak zmienia sie `MainActivity`

Zamiast:

```java
private List<String> listaHormonow = new ArrayList<>();
private HormoneAdapter adapter;
```

masz:

```java
private List<Hormone> listaHormonow = new ArrayList<>();
private ArrayAdapter<Hormone> adapter;
```

### 6.3 Ustawienie listy bez wlasnego adaptera

Zamiast poprzedniej metody `ustawListe()` mozesz zrobic tak:

```java
private void ustawListe() {
    listaHormonow.add(new Hormone("Trzustka", "Insulina"));
    listaHormonow.add(new Hormone("Tarczyca", "Tyroksyna"));
    listaHormonow.add(new Hormone("Szyszynka", "Melatonina"));
    listaHormonow.add(new Hormone("Jajniki", "Estrogen"));
    listaHormonow.add(new Hormone("Nadnercza", "Adrenalina"));

    adapter = new ArrayAdapter<>(
            this,
            android.R.layout.simple_list_item_1,
            listaHormonow
    );

    listHormony.setAdapter(adapter);
}
```

Tutaj Android sam bierze wynik `toString()` i pokazuje go w jednym `TextView`.

### 6.4 Klikniecie elementu w tej wersji

```java
listHormony.setOnItemClickListener((parent, view, position, id) -> {
    Hormone element = listaHormonow.get(position);

    etHormon.setText(element.getHormon());
    spinnerGruczol.setSelection(znajdzPozycjeGruczolu(element.getGruczol()));

    Toast.makeText(this, "Wybrano: " + element.toString(), Toast.LENGTH_SHORT).show();
});
```

### 6.5 Long click w tej wersji

```java
listHormony.setOnItemLongClickListener((parent, view, position, id) -> {
    Hormone usuniety = listaHormonow.remove(position);
    adapter.notifyDataSetChanged();
    Toast.makeText(this, "Usunieto: " + usuniety.toString(), Toast.LENGTH_SHORT).show();
    return true;
});
```

### 6.6 Jak wtedy wyglada element listy

W wersji z `item_hormon.xml` jeden rekord moze miec dwa osobne `TextView`:

- jeden na gruczol,
- drugi na hormon.

W wersji z `toString()` rekord jest prostszy i wyglada mniej wiecej tak:

```txt
Trzustka -> Insulina
Tarczyca -> Tyroksyna
Szyszynka -> Melatonina
```

Czyli:

- mniej kodu,
- mniej plikow,
- prostsza lista,
- ale tez mniejsze mozliwosci wygladu.

### 6.7 Kiedy wybrac ktora wersje

Wersja z `item_hormon.xml` i `HormoneAdapter`:

- lepsza, gdy chcesz ladniejszy wyglad,
- lepsza, gdy rekord ma kilka pol,
- bardziej podobna do prawdziwej aplikacji.

Wersja z `toString()`:

- lepsza do szybkiego zadania,
- dobra na prostszy sprawdzian,
- dobra, gdy wystarczy jeden napis w wierszu.

## 7. Wersja posrednia: jest adapter, ale nie ma `item.xml`

To jest bardzo dobry wariant, bo:

- masz wlasny adapter,
- mozesz sam ustawic tekst w wierszu,
- ale nie tworzysz osobnego pliku `item_hormon.xml`.

To jest praktycznie styl podobny do `lek16_oceny`.

### 7.1 Klasa `Hormone`

Mozesz zostawic taka sama klase jak w wersji z `toString()`:

```java
package com.example.hormony;

public class Hormone {
    private String gruczol;
    private String hormon;

    public Hormone(String gruczol, String hormon) {
        this.gruczol = gruczol;
        this.hormon = hormon;
    }

    public String getGruczol() {
        return gruczol;
    }

    public String getHormon() {
        return hormon;
    }
}
```

Tutaj `toString()` nie jest juz potrzebne, bo tekst ukladasz bezposrednio w adapterze.

### 7.2 Adapter bez `item.xml`

```java
package com.example.hormony;

import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ArrayAdapter;
import android.widget.TextView;

import java.util.List;

public class HormoneSimpleAdapter extends ArrayAdapter<Hormone> {

    public HormoneSimpleAdapter(Context context, List<Hormone> hormony) {
        super(context, android.R.layout.simple_list_item_1, hormony);
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        if (convertView == null) {
            convertView = LayoutInflater.from(getContext())
                    .inflate(android.R.layout.simple_list_item_1, parent, false);
        }

        Hormone hormon = getItem(position);
        TextView textView = convertView.findViewById(android.R.id.text1);

        textView.setText(hormon.getGruczol() + " -> " + hormon.getHormon());

        return convertView;
    }
}
```

To jest adapter, ale bez dodatkowego layoutu `item_hormon.xml`.

### 7.3 Jak wyglada `MainActivity`

Na gorze klasy:

```java
private List<Hormone> listaHormonow = new ArrayList<>();
private HormoneSimpleAdapter adapter;
```

Ustawienie listy:

```java
private void ustawListe() {
    listaHormonow.add(new Hormone("Trzustka", "Insulina"));
    listaHormonow.add(new Hormone("Tarczyca", "Tyroksyna"));
    listaHormonow.add(new Hormone("Szyszynka", "Melatonina"));
    listaHormonow.add(new Hormone("Jajniki", "Estrogen"));
    listaHormonow.add(new Hormone("Nadnercza", "Adrenalina"));

    adapter = new HormoneSimpleAdapter(this, listaHormonow);
    listHormony.setAdapter(adapter);
}
```

Klikniecie elementu:

```java
listHormony.setOnItemClickListener((parent, view, position, id) -> {
    Hormone element = listaHormonow.get(position);
    etHormon.setText(element.getHormon());
    spinnerGruczol.setSelection(znajdzPozycjeGruczolu(element.getGruczol()));
});
```

Long click:

```java
listHormony.setOnItemLongClickListener((parent, view, position, id) -> {
    listaHormonow.remove(position);
    adapter.notifyDataSetChanged();
    Toast.makeText(this, "Usunieto element", Toast.LENGTH_SHORT).show();
    return true;
});
```

### 7.4 Jak wyglada rekord na liscie

W tej wersji kazdy rekord bedzie po prostu jednym tekstem, np.:

```txt
Trzustka -> Insulina
Tarczyca -> Tyroksyna
Szyszynka -> Melatonina
```

### 7.5 Czym to sie rozni od wersji z `toString()`

Wersja z `toString()`:

- nie ma wlasnego adaptera,
- uzywa zwyklego `ArrayAdapter`,
- tekst skladasz w klasie modelu.

Wersja z adapterem bez `item.xml`:

- ma wlasny adapter,
- nadal nie ma osobnego pliku item,
- tekst skladasz w `getView()`.

To daje Ci troche wiecej kontroli niz samo `toString()`.

### 7.6 Kiedy warto wybrac ten wariant

Ten wariant jest dobry, gdy:

- nauczyciel chce, zeby byl adapter,
- ale nie potrzebujesz rozbudowanego wygladu,
- wystarczy jeden napis w wierszu,
- chcesz cos podobnego do `GradeAdapter`.

## 8. Co zapamietac z tego przykladu

Masz teraz dwie poprawne wersje listy:

1. wersja pelna:
   `item_hormon.xml` + `HormoneAdapter.java`
2. wersja prostsza:
   `toString()` + `ArrayAdapter`
3. wersja posrednia:
   wlasny adapter + `android.R.layout.simple_list_item_1`

Obie sa poprawne.  
Po prostu wybierasz te, ktora lepiej pasuje do zadania.
