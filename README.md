import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ListView;
import android.widget.Toast;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {
    private ArrayList<Produkt> wszystkieProdukty = new ArrayList<>();
    private ListView listView;
    private EditText editText;
    private EditText editTextCena;
    private Button button;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        wszystkieProdukty.add(new Produkt("niezbadan planeta",200));
        wszystkieProdukty.add(new Produkt("poszukiwanie planety x",120));
        wszystkieProdukty.add(new Produkt("sowy",50));
        wszystkieProdukty.add(new Produkt("maszyna Turinga",130));
        listView = findViewById(R.id.listView2);
        editText = findViewById(R.id.editText);
        editTextCena = findViewById(R.id.editTextNumberDecimal);
        button = findViewById(R.id.button);
        ArrayAdapter<Produkt> adapter = new ArrayAdapter<>(
                this,
                android.R.layout.simple_list_item_1,
                wszystkieProdukty
        );
        listView.setAdapter(adapter);
        listView.setOnItemClickListener(
                new AdapterView.OnItemClickListener() {
                    @Override
                    public void onItemClick(AdapterView<?> adapterView, View view, int i, long l) {
                        Toast.makeText(MainActivity.this,
                                String.valueOf(wszystkieProdukty.get(i).getCena()),
                                Toast.LENGTH_SHORT).show();
                    }
                }
        );
        button.setOnClickListener(
                new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        String p = editText.getText().toString();
                        int cena = Integer.valueOf(editTextCena.getText().toString());
                        wszystkieProdukty.add(new Produkt(p,cena));
                        adapter.notifyDataSetChanged();
                    }
                }
        );
    }
}









package pl.zabrze.zs10.listy3psroda;
public class Produkt {
    private String nazwa;
    private float cena;

    public Produkt(String nazwa, float cena) {
        this.nazwa = nazwa;
        this.cena = cena;
    }

    public String getNazwa() {
        return nazwa;
    }

    public float getCena() {
        return cena;
    }

    @Override
    public String toString() {
        return "Produkt:" + nazwa ;
    }
}







import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.os.Handler;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;
import android.widget.TextView;

import java.util.ArrayList;

public class TimerActivity extends AppCompatActivity {
    private Button buttonStart, buttonStop, buttonZapisz, buttonReset;
    private TextView textViewCzas;
    private ListView listViewCzas;
    private boolean uruchomiony;
    private int sekundy;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_timer);
        textViewCzas = findViewById(R.id.textView5);
        buttonStart = findViewById(R.id.buttonStart);
        buttonStop = findViewById(R.id.buttonStop);
        buttonZapisz = findViewById(R.id.buttonZapisz);
        buttonReset = findViewById(R.id.buttonReset);
        listViewCzas = findViewById(R.id.listViewCzasy);
        ArrayList<String> czasy = new ArrayList<>();
        ArrayAdapter<String> adapter = new ArrayAdapter<>(
                this,
                android.R.layout.simple_list_item_1,
                czasy
        );
        listViewCzas.setAdapter(adapter);
        buttonZapisz.setOnClickListener(
                new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        String czas = wyswietlCZas(sekundy);
                        czasy.add(czas);
                        adapter.notifyDataSetChanged();
                    }
                }
        );
        listViewCzas.setOnItemClickListener(
                new AdapterView.OnItemClickListener() {
                    @Override
                    public void onItemClick(AdapterView<?> adapterView, View view, int i, long l) {
                        czasy.remove(i);
                        adapter.notifyDataSetChanged();
                    }
                }
        );
        buttonStart.setOnClickListener(
                new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        uruchomiony = true;
                    }
                }
        );
        buttonStop.setOnClickListener(
                new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        uruchomiony = false;
                    }
                }
        );
        buttonReset.setOnClickListener(
                new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        sekundy = 0;
                        wyswietlCZas(0);
                    }
                }
        );
        starTimer();
    }
    private void starTimer(){
        final Handler handler = new Handler();
        handler.post(new Runnable() {
            @Override
            public void run() {
                if(uruchomiony)
                    sekundy++;
                wyswietlCZas(sekundy);
                handler.postDelayed(this,1000);
            }
        });

    }
    private String wyswietlCZas(int sek){
        int sekundy = sek%60;
        int minuty = (sek/60)%60;
        int godziny = sek/3600;
        textViewCzas.setText(String.format("%02d:%02d:%02d",godziny,minuty,sekundy));
        return String.format("%02d:%02d:%02d",godziny,minuty,sekundy);
    }
}






import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.RadioButton;
import android.widget.RadioGroup;
import android.widget.TextView;

public class WybierzZawodActivity extends AppCompatActivity {

    private RadioGroup radioGroup;

    private TextView textView;
    private Button button;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_wybierz_zawod);
        radioGroup  = findViewById(R.id.radioGroup);
        button = findViewById(R.id.button4);
        textView = findViewById(R.id.textView4);
        String opisy[] = new String[]{
                "programista to zawód dla wytrwałych, którzy lubią logiczne myślenie i długie godziny przed komputerem",
                "informatyk to zawód dla osób lubiących montaż komputera i sieci",
                "elektronik to zawód o zapachu cyny, bez lutownicy nie podchodź"
        };
        button.setOnClickListener(
                new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        int idKlikniete = radioGroup.getCheckedRadioButtonId();
                        if(idKlikniete == R.id.radioButton){
                            textView.setText(opisy[0]);
                        }
                        else if(idKlikniete == R.id.radioButton2){
                            textView.setText(opisy[1]);
                        }
                        else{
                            textView.setText(opisy[2]);
                        }
                    }
                }
        );


    }
}






package pl.zabrze.zs10.listy3psroda;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

public class ZgadywanieActivity extends AppCompatActivity {
    private Button buttonPodpowiedz, buttonZgadywanie;
    private EditText editText;
    private Integer wylosowanaLiczba;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_zgadywanie);
        wylosowanaLiczba = (int)(Math.random()*100)+1;
        buttonPodpowiedz = findViewById(R.id.button3);
        buttonZgadywanie = findViewById(R.id.button2);
        editText = findViewById(R.id.editTextTextPersonName);
        buttonZgadywanie.setOnClickListener(
                new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        Integer liczba = Integer.valueOf(editText.getText().toString());
                        if(liczba == wylosowanaLiczba){
                            Toast.makeText(ZgadywanieActivity.this, "Gratulacje", Toast.LENGTH_SHORT).show();
                        }
                        else if(liczba<wylosowanaLiczba){
                            Toast.makeText(ZgadywanieActivity.this, "Wpisano za mało", Toast.LENGTH_SHORT).show();
                        }
                        else {
                            Toast.makeText(ZgadywanieActivity.this, "Wpisano za dużo", Toast.LENGTH_SHORT).show();
                        }
                    }
                }
        );
        buttonPodpowiedz.setOnClickListener(
                new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        Toast.makeText(ZgadywanieActivity.this,
                                "Wylosowano "+wylosowanaLiczba.toString(),
                                        Toast.LENGTH_SHORT)
                                .show();
                    }
                }
        );
    }
}
