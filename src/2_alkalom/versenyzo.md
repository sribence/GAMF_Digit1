# 1. Fejezet – Fájlbeolvasás és objektumok kezelése C#-ban

Ebben a fejezetben egy **versenyzőket kezelő** C# programot vizsgálunk meg lépésről lépésre.  
A program célja: beolvasni egy szövegfájlból a versenyzők adatait, eltárolni őket objektumokban,  
majd meghatározni és kiírni a legjobb eredményt elért versenyzőt.

---

## 🧩 1. Magyarázat

### 1.1. A program célja
A program a `versenyzok.txt` fájlból olvassa be a versenyzők adatait.  
Minden sor egy versenyzőt ír le az alábbi formátumban:

```
#12 Nagy Gábor 399 400 390
```


Itt:
- **#12** → a versenyző azonosítója (id),
- **Nagy Gábor** → a versenyző neve,
- **399 400 390** → a versenyző ugrásainak eredményei.

A program ezeket az adatokat **Versenyzo** objektumokba tölti be,  
majd megkeresi azt, akinek a **legjobb ugrása (legmagasabb pontszáma)** a legnagyobb.

---

### 1.2. Fájlbeolvasás – `StreamReader`

A fájl olvasását a `StreamReader` osztály végzi:

```
StreamReader olvas = new StreamReader("versenyzok.txt");
```


Ez az objektum a fájl **folytonos beolvasását** teszi lehetővé.  
A `ReadToEnd()` metódus az **egész fájl tartalmát** beolvassa egyszerre egy nagy sztringbe.  
Ezt a sztringet **sorokra bontjuk** a `Split('\n')` segítségével, így minden sor egy versenyzőt képvisel.

> 💡 **Tipp:**  
> A C++ nyelvben a fájlolvasás tipikusan `ifstream`-mel történik (pl. `ifstream f("versenyzok.txt");`),  
> és a `getline()` függvényt használjuk a soronkénti beolvasáshoz.  
> C#-ban a `StreamReader` a hasonló szerepű eszköz.

---

### 1.3. Sorok feldolgozása és darabolás

A `Split(' ')` függvény szóközök mentén darabolja a sort.  
Az így kapott **darabok tömbében** az elemek például a következők:

```
["#12", "Nagy", "Gábor", "399", "400", "390"]
```


Innen:
- Az első elem az `id`, amit először meg kell tisztítani a `#` jeltől.  
  Ezért: ```id = int.Parse(darabok[0].Trim('#'));```  
  → `Trim('#')` eltávolítja a `#` karaktert,  
  → `int.Parse()` átalakítja az így kapott `"12"` szöveget egész számmá (`int` típus).

- A második és harmadik elem a név részei:

```
nev = darabok[1] + " " + darabok[2
```


- A többi érték az eredmények listája.  
  Ezekből új `int` tömböt készítünk, és `int.Parse()` segítségével számmá alakítjuk őket.

> ⚠️ **Fontos:**  
> Ha a fájlban valamelyik sor hibás formátumú lenne (például hiányzik egy szám),  
> a `Parse()` hiba dobna (`FormatException`).  
> Ilyen esetekben érdemes `try-catch` blokkot használni hibakezelésre.

---

### 1.4. Objektumok létrehozása – `new` és `Versenyzo` osztály

A `Versenyzo` osztály egy **saját típus**, ami három adatot tárol:
- `Id` (egész szám),
- `Nev` (szöveg),
- `Eredmenyek` (egész számok tömbje).

Amikor a `new Versenyzo(id, nev, szamok)` utasítást végrehajtjuk,
a **memóriában egy új objektum** jön létre.  
Ez az objektum külön memóriaterületen tárolja az adatait, és a változó (`versenyzo`) csak egy **hivatkozást (referenciát)** tartalmaz erre az objektumra.

> 🧠 **Memória analógia:**  
> Olyan ez, mintha egy szekrényt (objektumot) hoznánk létre,  
> és a `versenyzo` változó csak a címkét (címet) tartalmazná, ami megmondja, hol van a szekrény.

---

### 1.5. A legjobb ugrás meghatározása

A `Versenyzo` osztályban van egy `legjobbUgras()` metódus, amely a **legnagyobb számot** adja vissza az eredmények közül:


```

return Eredmenyek.Max();
```



Ez a `Max()` a **LINQ** könyvtár (`System.Linq`) része,  
és bármilyen számokat tartalmazó kollekció (lista, tömb) esetén megadja a legnagyobbat.

> 💡 **C++ megfelelője:**  
> C++-ban a `std::max_element` függvényt használnánk hasonló célra.

A főprogram ezután minden versenyző `legjobbUgras()` értékét összehasonlítja,  
és eltárolja a legnagyobbat.

---

### 1.6. Kimenet és `ToString()`

Amikor a program kiírja az objektumokat (`Console.WriteLine(item);`),  
a C# automatikusan meghívja a **`ToString()` metódust**,  
amely megadja, **hogyan nézzen ki szövegként az objektum**.

A mi esetünkben ez például:

```
12 Nagy Gábor eredményei: 399, 400, 390, legjobb: 400
```




Ez teszi az eredményt emberi olvasásra alkalmas formátumúvá.

---

### 1.7. Példafájl – `versenyzok.txt`
```
#12 Nagy Gábor 399 400 390
#7 Kis Csaba 403 392
#24 Juan Valerio 390 407 377 398
#3 Gipsz Jakab 390 375
```




---

### 1.8. Példakimenet


```
Beolvasott adatok:
12 Nagy Gábor eredményei: 399, 400, 390, legjobb: 400
7 Kis Csaba eredményei: 403, 392, legjobb: 403
24 Juan Valerio eredményei: 390, 407, 377, 398, legjobb: 407
3 Gipsz Jakab eredményei: 390, 375, legjobb: 390

A verseny győztese: 24 Juan Valerio eredményei: 390, 407, 377, 398, legjobb: 407
```

---

## 💬 2. Információs blokk

> ⚙️ **C# vs C++ különbségek röviden**
> - **Osztály létrehozása:**  
>   C#-ban minden referencia típus (`class`) alapértelmezetten heap-en jön létre,  
>   míg C++-ban választhatunk stack (automatikus) vagy heap (`new`) között.  
>
> - **Beolvasás:**  
>   C# `StreamReader` vs. C++ `ifstream`.  
>
> - **Tömbök:**  
>   C#-ban a tömb mindig objektum, amelyet a `new` kulcsszóval hozunk létre.  
>   C++-ban lehet statikus vagy dinamikus is (`int arr[5]` vagy `new int[5]`).
>
> - **`ToString()` metódus:**  
>   C# automatikusan meghívja, ha egy objektumot kiírunk a konzolra.  
>   C++-ban ezt `operator<<` túlterheléssel érnénk el.

---

## 🧮 3. Teljes kód

A program két osztályból áll:  
**Program.cs** – a fő program,  
**Versenyzo.cs** – a versenyző osztály.

---

### Program.cs

```
// innentől új kód.
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace fa2_versenyzoOsztaly
{
    internal class Program
    {
        static void Main(string[] args)
        {
            int id; // Versenyző azonosítója
            string nev; // Versenyző neve
            int[] szamok; // Eredmények tömbje

            // Fájl beolvasása
            StreamReader olvas = new StreamReader("versenyzok.txt");
            string[] sorok = olvas.ReadToEnd().Split('\n'); // Sorokra bontás
            Versenyzo[] v = new Versenyzo[sorok.Length]; // Versenyzők tömbje

            for (int i = 0; i < v.Length; i++)
            {
                string[] darabok = sorok[i].Split(' '); // Szóköz mentén darabolás
                id = int.Parse(darabok[0].Trim('#')); // # eltávolítása és szám konvertálása
                nev = darabok[1] + " " + darabok[2]; // Név összefűzése
                szamok = new int[darabok.Length - 3]; // Eredmények száma (3 első elem nem tartozik ide)

                for (int j = 0; j < szamok.Length; j++)
                {
                    szamok[j] = int.Parse(darabok[j + 3]); // Szöveges szám konvertálása int típusra
                }

                Versenyzo versenyzo = new Versenyzo(id, nev, szamok); // Új objektum létrehozása
                v[i] = versenyzo; // Hozzáadjuk a tömbhöz
            }

            olvas.Close(); // Fájl bezárása

            Console.WriteLine("Beolvasott adatok:");
            foreach (Versenyzo item in v)
            {
                Console.WriteLine(item); // ToString() automatikusan meghívódik
            }

            // Legjobb versenyző keresése
            Versenyzo legjobb = v[0];
            for (int i = 0; i < v.Length; i++)
            {
                if (legjobb.legjobbUgras() < v[i].legjobbUgras())
                {
                    legjobb = v[i];
                }
            }

            Console.WriteLine("\nA verseny győztese: " + legjobb);
            Console.ReadLine();
        }
    }
}
```
---
##Versenyzo.cs
```
// innentől új kód.
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace fa2_versenyzoOsztaly
{
    internal class Versenyzo
    {
        private int Id; // Azonosító
        private string Nev; // Név
        private int[] Eredmenyek; // Eredmények tömbje

        // Konstruktor – új versenyző létrehozása
        public Versenyzo(int id, string nev, int[] szamok)
        {
            this.Id = id;
            this.Nev = nev;
            this.Eredmenyek = szamok;
        }

        // Szöveges megjelenítés
        public override string ToString()
        {
            string s = $"{Id} {Nev} eredményei: ";
            for (int i = 0; i < Eredmenyek.Length; i++)
            {
                s += Eredmenyek[i] + ", ";
            }
            s += "legjobb: " + legjobbUgras(); // Legjobb ugrás hozzáfűzése
            return s;
        }

        // Legjobb ugrás meghatározása
        public int legjobbUgras()
        {
            return Eredmenyek.Max(); // LINQ függvény – legnagyobb érték a tömbben
        }
    }
}
```
---
#🏁 Összefoglalás

Ezzel a programmal megtanultuk:
- hogyan lehet fájlból adatokat beolvasni C#-ban (StreamReader);
- hogyan hozunk létre saját osztályt és objektumokat (class, new);
- hogyan dolgozunk tömbökkel és LINQ függvényekkel (Max());
- és hogyan formázzuk az objektumok szöveges megjelenítését (ToString()).

Ez a gyakorlat remek alapot ad az objektumorientált gondolkodás és a fájlkezelés megértéséhez C# nyelvben.
