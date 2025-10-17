# C# Tananyag – Kerítés Feladat, 1. Feladat

## 1. Feladat – Kerítés adatok beolvasása

### 1.1 Alfeladat – A fájl szerkezete és a cél

A `kerites.txt` fájl minden sora egy telket ír le a következő formátumban:

```

Oldal Szelesseg Szin

```

- **Oldal** – 0 = páros, 1 = páratlan  
- **Szelesseg** – a telek mérete méterben (int)  
- **Szin** – a kerítés színe vagy állapota (char, pl. `#` festetlen, `A-Z` színek)

**Példa `kerites.txt` tartalom:**

```

1 4 A
0 5 B
1 3 #

```

A cél: minden sort beolvassunk, és **egy objektumot** hozzunk létre belőle (`Kerites`), majd egy **listába** tegyük, hogy a program később feldolgozhassa.

> **Tippek kezdőknek:**  
> - C++-ban ezt `ifstream`-pel és `std::vector<Kerites>`-szel oldanánk meg.  
> - C#-ban `StreamReader`-rel olvasunk, és `List<Kerites>`-be tároljuk a telkeket.  
> - A Garbage Collector automatikusan kezeli a memóriafelszabadítást, így nem kell `delete`-et használnunk.

---

### 1.2 Alfeladat – Kerites osztály létrehozása

A `Kerites` osztály minden telek adatait tartalmazza: oldal, szélesség, szín, házszám.  

**Fontos:** a konstruktorban állítjuk be az értékeket, a mezőket kívülről **csak olvasni lehet** (`get` property).

### Kód (`Kerites.cs`)

```csharp
// innentől új kód

using System;

namespace fa2_KeritesFeladat
{
internal class Kerites
{
private int oldal;       // 0 = páros, 1 = páratlan
private int szelesseg;   // telek szélessége méterben
private char szin;       // kerítés színe vagy állapota
private int hazszam;     // a telek házszáma


    // Olvasható tulajdonságok
    public int Oldal { get { return oldal; } }
    public int Szelesseg { get { return szelesseg; } }
    public int Hazszam { get { return hazszam; } }
    public char Szin { get { return szin; } }

    // Konstruktor – objektum létrehozása
    public Kerites(int oldal, int szelesseg, char szin, int hazszam)
    {
        this.oldal = oldal;       // mező = paraméter
        this.szelesseg = szelesseg;
        this.szin = szin;
        this.hazszam = hazszam;
    }
}


}

```

---

### 1.3 Alfeladat – Utca osztály és a lista kezelése

Az `Utca` osztály tartalmazza az összes telket egy listában:

- `List<Kerites> telkek` – a lista, ahol a telkeket tároljuk  
- `Db` – telkek száma  
- Indexer (`this[int index]`) – lehetővé teszi, hogy a telkekhez **lista indexeléssel** hozzáférjünk (`utca[0]`, `utca[1]`, …)

#### Fájl beolvasás lépései:

1. `StreamReader` megnyitja a fájlt.  
2. Soronként olvasunk a fájlból.  
3. A sort felbontjuk a szóközök mentén (`Split(' ')`).  
4. Konvertáljuk az adatokat:  
   - `int oldal = Convert.ToInt32(egysordarabok[0])`  
   - `int szelesseg = Convert.ToInt32(egysordarabok[1])`  
   - `char szin = Convert.ToChar(egysordarabok[2])`  
5. Számoljuk a házszámot: páros oldalnál +2, páratlan oldalnál +2  
6. Létrehozunk egy új `Kerites` objektumot és hozzáadjuk a listához.

> **Memória szempont:**  
> - Minden `Kerites` objektum a heap-en jön létre.  
> - A lista csak a referencia címét tartja, nem másolja a teljes objektumot.

### Kód (`Utca.cs` – 1. feladat)

// innentől új kód
```csharp

using System;
using System.Collections.Generic;
using System.IO;

namespace fa2_KeritesFeladat
{
internal class Utca
{
private List<Kerites> telkek; // az összes telek listája


    public Kerites this[int index]
    {
        get { return telkek[index]; } // indexer
    }

    public int Db
    {
        get { return telkek.Count; } // telkek száma
    }

    // Konstruktor – beolvassa a fájlt és feltölti a listát
    public Utca(string fajlnev)
    {
        telkek = new List<Kerites>();
        int paros = 0, plan = -1; // házszámok kezdőértékei
        using (StreamReader olvas = new StreamReader(fajlnev))
        {
            while (!olvas.EndOfStream)
            {
                string egysor = olvas.ReadLine();
                string[] egysordarabok = egysor.Split(' ');

                int oldal = Convert.ToInt32(egysordarabok[0]);  // típuskonverzió
                int szelesseg = Convert.ToInt32(egysordarabok[1]);
                char szin = Convert.ToChar(egysordarabok[2]);

                int hazszam;
                if (oldal == 0) // páros oldal
                {
                    paros += 2;
                    hazszam = paros;
                }
                else // páratlan oldal
                {
                    plan += 2;
                    hazszam = plan;
                }

                // Új objektum létrehozása és lista feltöltése
                telkek.Add(new Kerites(oldal, szelesseg, szin, hazszam));
            }
        }
    }
}


}

```


## 2. Feladat – Eladott telkek számának kiírása

### Magyarázat

1. Már létrehoztuk az `Utca` objektumot (`Utca utca = new Utca("kerites.txt");`), amelynek `telkek` listája tartalmazza az összes telket.  
2. A feladat az, hogy a **telkek számát** kiírjuk. Ehhez használjuk a `Db` property-t (`utca.Db`), amely visszaadja a lista hosszát (`telkek.Count`).  
3. A `Console.WriteLine` segítségével jelenítjük meg az értéket.  

> **Tippek és C# vs C++:**  
> - C++-ban a lista (`vector`) méretét `telkek.size()`-val kérdeznénk le.  
> - A C# `Count` property-t használ a `List<T>` típus.  
> - A string formázásban a `{0}` jelölést helyettesíti a `Console.WriteLine("Az eladott telkek száma: {0}", utca.Db)`.

### Kód – 2. feladat

// innentől új kód
```csharp

Console.WriteLine("2. feladat");
Console.WriteLine("Az eladott telkek száma: {0}", utca.Db); // Kiírja a lista hosszát, azaz a telkek számát

```

---

## 3. Feladat – Az utolsó telek adatai

### Magyarázat

1. Az utolsó telek a lista **utolsó eleme**: `utca[utca.Db - 1]`.  
   - Miért `Db - 1`? Mert a lista **0-tól indexelődik**, tehát a 100 elemű lista utolsó indexe 99.  
2. Meg kell határozni:
   - Melyik **oldalon** van (páros = 0, páratlan = 1)  
   - Mi a **házszáma**  

3. A `if` szerkezet segít eldönteni, hogy kiírjuk: „páros” vagy „páratlan”.  
4. A `Console.WriteLine` kiírja a házszámot is.  

> **Memória és típusok:**  
> - A `utca[utca.Db - 1]` visszaad egy **Kerites objektum referenciát**.  
> - `Oldal` és `Hazszam` property-k **csak olvashatóak**, így nem módosíthatók kívülről.  
> - C++-ban az utolsó elem eléréséhez `telkek.back()` vagy `telkek[telkek.size() - 1]` lenne a megfelelő.

### Kód – 3. feladat

// innentől új kód
```csharp

Console.WriteLine("3. feladat");
Console.Write("A ");
if (utca[utca.Db - 1].Oldal == 0)
{
Console.Write("páros"); // ha Oldal = 0, akkor páros oldal
}
else
{
Console.Write("páratlan"); // ha Oldal = 1, akkor páratlan oldal
}
Console.WriteLine(" oldalon adták el az utolsó telket.");
Console.WriteLine("Az utolsó telek házszáma: {0} ", utca[utca.Db - 1].Hazszam);

```

---


## 4. Feladat – Szomszédos telkek azonos színének keresése

### Magyarázat

A feladat lényege: találjunk egy páratlan oldalon lévő telket, amelynek **szomszédos telkével megegyezik a kerítés színe**.

1. A telkeket már egy `List<Kerites>`-ben tároljuk (`telkek`).  
2. Végig kell menni a listán (`for` ciklus), és **ellenőrizni minden szomszédot**:
   - `telkek[i]` és `telkek[i + 1]` oldala egyaránt páratlan legyen (Oldal = 1)  
   - Színük megegyezzen (`Szin`)  
   - A szín ne legyen `#` (festetlen) vagy `:` (hiányzó)  
3. Ha találunk ilyet, **a házszámot visszaadjuk** (`telkek[i].Hazszam`).  
4. Ha nincs ilyen telek, -1-et adunk vissza.  

> **Tippek és C# vs C++:**  
> - C++-ban ugyanígy egy `for` ciklust és `vector[i]` indexelést használhatnánk.  
> - A `return` kilép a függvényből, és azonnal visszaadja az értéket.  
> - A `&&` operátor logikai ÉS, a `!=` operátor nem egyenlő.

> **Memória és logika:**  
> - A `telkek[i]` és `telkek[i+1]` referenciák ugyanarra a memóriaterületre mutatnak, amelyet a `Utca` objektum tárol a listában.  
> - Nem készítünk másolatot, csak olvasunk az objektumból.

### Kód – 4. feladat

// innentől új kód
```csharp

public int Feladat4()
{
Console.WriteLine("4. feladat");
for (int i = 0; i < Db - 1; i++)
{
// Ellenőrizzük, hogy a két szomszédos telek páratlan oldalon van-e és azonos színű
if (telkek[i].Oldal + telkek[i + 1].Oldal == 2 && // mindkettő páratlan (1+1=2)
telkek[i].Szin == telkek[i + 1].Szin &&     // azonos szín
telkek[i].Szin != '#' && telkek[i].Szin != ':') // nem festetlen vagy hiányzó
{
return telkek[i].Hazszam; // visszaadjuk az első találat házszámát
}
}
return -1; // ha nincs találat
}

```

---

### Példa működésre

Tegyük fel, hogy a páratlan oldal telkei a következő színekkel rendelkeznek:  

```csharp

i=0: Szin=A, i=1: Szin=B, i=2: Szin=B, i=3: Szin=C

```

- `i=0` és `i=1`: `A != B` → nem egyezik  
- `i=1` és `i=2`: `B == B` → egyezik, visszaadjuk `telkek[1].Hazszam`

> **Tippek kezdőknek:**  
> - Az első találatot adjuk vissza, a többivel nem foglalkozunk.  
> - A `for (int i = 0; i < Db - 1; i++)` azért `Db-1`, hogy ne lépjünk túl a lista határán (`i+1` nem létezne a végén).

---

Ez a függvény a **lista feldolgozását** és a **feltételes logikai vizsgálatot** gyakoroltatja.  


## 5. Feladat – Házszámhoz tartozó kerítés színe / új szín választása

### Magyarázat

1. A felhasználótól bekérünk egy **házszámot** (`Console.ReadLine()`), és konvertáljuk `int`-re (`int.Parse`).  
2. Ellenőrizzük, hogy a házszám **érvényes**: nem lehet kisebb 1-nél vagy nagyobb, mint az utolsó telek házszáma (`utca[utca.Db-1].Hazszam`).  
   - Hibás bevitel esetén `try-catch` blokkba esik, és **alapértelmezett értéket adunk** (`szam = 83`).  
3. Megkeressük a **megadott házszámhoz tartozó telek indexét** a listában.  
4. Kiírjuk a **kerítés színét vagy állapotát** (`telkek[i].Szin`).  
5. Új színt választunk a következő szabályok alapján:  
   - Különböznie kell a **szomszédoktól** (alsó és felső szomszéd)  
   - Különböznie kell a **jelenlegi színtől**  
   - Véletlenszerűen generáljuk `Random`-dal (`A-D`).  
6. Ha a ház az első vagy második páros/páratlan, nincs alsó szomszéd.  
7. A véletlenszerű új szín `while` ciklusban generálódik, amíg a feltételek teljesülnek.  

> **Tippek és C# vs C++:**  
> - C++-ban a `rand()`-ot használnánk, és a típuskonverzióhoz `static_cast<char>` kellene.  
> - A C# `Random` objektuma automatikusan kezeli a véletlenszám-generálást.  
> - A `try-catch` fontos a **hibakezelés** miatt: a felhasználó adhat rossz bemenetet.

### Kód – 5. feladat

// innentől új kód
```csharp

Console.WriteLine("5. feladat");
int szam;
try
{
Console.Write("Adjon meg egy házszámot! ");
szam = int.Parse(Console.ReadLine()); // string → int konverzió
if (szam < 1 || szam > utca[utca.Db - 1].Hazszam)
{
throw new Exception("Hibás érték! A házszám értéke 83 lett. ");
}
}
catch (Exception e)
{
szam = 83; // alapértelmezett házszám
Console.WriteLine(e.Message);
}

// Megkeressük a telek indexét
int keresett = 0;
for (int i = 0; i < utca.Db; i++)
{
if (utca[i].Hazszam == szam)
{
Console.WriteLine("A kerítés színe / állapota: " + utca[i].Szin);
keresett = i;
break;
}
}

// Véletlenszerű új szín választása
int also = 0, felso = 0;
if (szam == 1 || szam == 2) also = keresett; // nincs alsó szomszéd
else
{
while (utca[also].Hazszam != szam - 2) also++;
}
while (felso < utca.Db - 1 && utca[felso].Hazszam != szam + 2) felso++;

Random x = new Random();
char uj = (char)x.Next(65, 69); // A-D
while (utca[also].Szin == uj || utca[felso].Szin == uj || utca[keresett].Szin == uj)
{
uj = (char)x.Next(65, 69);
}

Console.WriteLine("Egy lehetséges festési szín: " + uj);

```

---

## 6. Feladat – Páratlan oldal utcaképének fájlba írása

### Magyarázat

1. Cél: létrehozni egy **szöveges utcakép** fájlt (`utcakep.txt`), amely két sorból áll:
   - Első sor: páratlan oldal kerítés színei az adott telek szélességében (`Szelesseg` karakter)  
   - Második sor: a házszámokat a telek kezdőpontjához igazítva  
2. Végigmegyünk a telkeken (`for i=0..Db-1`)  
   - Ha a telek páratlan (`Oldal == 1`), kiírjuk a **színt** a megfelelő szélességig (`while j < Szelesseg`)  
   - Ezután új sor (`WriteLine()`)  
3. Második sorban írjuk ki a **házszámokat**, és a házszám hosszához igazítjuk a szóközöket, hogy vizuálisan illeszkedjen a színekhez  
4. A `StreamWriter` automatikusan létrehozza a fájlt, vagy felülírja, ha már létezik  

> **Tippek és C# vs C++:**  
> - C++-ban `ofstream`-ot használnánk, a szóközök és karakterek kiírása hasonló logika.  
> - A `using` blokk automatikusan bezárja a fájlt, így nem kell külön `Close()`-t hívni.  

### Kód – 6. feladat

// innentől új kód
```csharp
 
using (StreamWriter iras = new StreamWriter("utcakep.txt"))
{
// Első sor: színek
for (int i = 0; i < utca.Db; i++)
{
if (utca[i].Oldal == 1) // páratlan oldal
{
int j = 0;
while (j < utca[i].Szelesseg)
{
iras.Write(utca[i].Szin); // kerítés színe
j++;
}
}
}
iras.WriteLine();

```
// Második sor: házszámok
for (int i = 0; i < utca.Db; i++)
{
    if (utca[i].Oldal == 1) // páratlan oldal
    {
        int j = 1;
        iras.Write(utca[i].Hazszam); // házszám kiírása
        if (utca[i].Hazszam > 9) j++;  // kétjegyű házszám
        if (utca[i].Hazszam > 99) j++; // háromjegyű házszám
        while (j < utca[i].Szelesseg)
        {
            iras.Write(" "); // szóköz a telek szélességhez igazítva
            j++;
        }
    }
}
```

}
Console.WriteLine("Fájlba írás megtörtént");

```

---

### Összefoglalás

- **5. feladat:** felhasználói házszám bekérése, hibakezelés, kerítés színének kiírása, új szín választása  
- **6. feladat:** páratlan oldal utcaképének készítése és fájlba írása  

> Ezek a feladatok gyakoroltatják a **felhasználói bemenetek kezelését**, **feltételes logikát**, **Random osztályt**, valamint a **fájlba írást és formázást**.

