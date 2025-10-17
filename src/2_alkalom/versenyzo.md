# 1. Fejezet – Fájlbeolvasás és objektumok kezelése C#-ban

Ebben a fejezetben egy **versenyzőket kezelő** C# programot írunk.  
A cél, hogy megtanuljuk a **fájlbeolvasás**, **tömbkezelés** és **alapvető objektumhasználat** lépéseit.  
Lépésről lépésre haladunk: minden magyarázat után láthatod az aktuális, futtatható C# kódrészt is.

---

## 🧩 1.1. A program célja

A program feladata: beolvasni egy szövegfájlból (``versenyzok.txt``) a versenyzők adatait,  
és később objektumokban eltárolni őket.

Minden sor a fájlban így néz ki:

``#12 Nagy Gábor 399 400 390``

Ebben:
- **#12** → a versenyző azonosítója,  
- **Nagy Gábor** → a versenyző neve,  
- **399 400 390** → az egyes ugrások pontszámai.

Először hozzunk létre egy új **Console Application** projektet,  
és a `Program.cs` fájlba írjuk be a következő alapkódot:

```csharp
using System;
using System.IO;
using System.Linq;

namespace fa2_versenyzoOsztaly
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Versenyzők adatainak feldolgozása..."); // Ellenőrzés: fut-e a program
            Console.ReadLine(); // A program vár, hogy lássuk az üzenetet
        }
    }
}
```

> 💡 **Tipp:**  
> Érdemes minden új programot ilyen „Hello World” típusú ellenőrzéssel kezdeni.  
> Ha ez működik, biztos, hogy a projekt beállításai rendben vannak.

---

## 🧩 1.2. Fájlbeolvasás előkészítése – `StreamReader`

Most bevezetjük a **fájlkezelést**.  
A C# nyelvben a `StreamReader` osztályt használjuk, amely lehetővé teszi,  
hogy fájlokat **soronként vagy teljes tartalmukban** beolvassunk.

A következő kód megnyitja a fájlt, és beolvassa a sorokat egy tömbbe:

```csharp
using System;
using System.IO;
using System.Linq;

namespace fa2_versenyzoOsztaly
{
    internal class Program
    {
        static void Main(string[] args)
        {
            // A fájl megnyitása olvasásra
            StreamReader olvas = new StreamReader("versenyzok.txt");

            // A teljes fájl tartalmát egyetlen szöveggé olvassuk
            string tartalom = olvas.ReadToEnd();

            // A tartalmat sorokra bontjuk az új sor jel ('\n') mentén
            string[] sorok = tartalom.Split('\n');

            // Teszt: írjuk ki, hány sor van a fájlban
            Console.WriteLine("A fájlban ennyi sor található: " + sorok.Length);

            olvas.Close(); // Fájl bezárása
            Console.ReadLine(); // Várakozás a kilépésre
        }
    }
}
```

> ⚙️ **Mi történik a háttérben?**  
> - A `StreamReader` egy **folyamot (stream)** nyit a fájlhoz.  
>   Olyan, mintha csövön keresztül olvasnánk a fájlból karakterenként.  
> - A `ReadToEnd()` metódus **egyszerre** beolvassa a teljes fájlt egyetlen stringbe.  
> - A `Split('\n')` függvény **sorokra bontja** a szöveget.  
>   Így egy tömbünk lesz, ahol minden elem egy-egy versenyző adatait tartalmazza.

> 💬 **C++ párhuzam:**  
> Ugyanez C++-ban így nézne ki:  
> ```cpp
> ifstream f("versenyzok.txt");
> string sor;
> while(getline(f, sor)) {
>     cout << sor << endl;
> }
> ```
> Látható, hogy a logika hasonló, csak a C#-ban objektumorientáltabb a megközelítés.

---

## 🧩 1.3. Sorok feldolgozása – az adatok szétbontása

Most, hogy beolvastuk a sorokat,  
minden sorból külön-külön ki fogjuk nyerni az adatokat:

- az azonosítót (`id`),  
- a nevet (`nev`),  
- és az ugrások pontszámait (`szamok`).

Ehhez a `Split(' ')` függvényt használjuk, amely szóközök mentén bontja szét a sztringet.

Íme a következő bővített kódrészlet:

```csharp
using System;
using System.IO;
using System.Linq;

namespace fa2_versenyzoOsztaly
{
    internal class Program
    {
        static void Main(string[] args)
        {
            StreamReader olvas = new StreamReader("versenyzok.txt");
            string[] sorok = olvas.ReadToEnd().Split('\n');

            for (int i = 0; i < sorok.Length; i++)
            {
                string[] darabok = sorok[i].Split(' '); // A sor feldarabolása szóközöknél

                int id = int.Parse(darabok[0].Trim('#')); // Azonosító átalakítása int-té (eltávolítjuk a # jelet)
                string nev = darabok[1] + " " + darabok[2]; // A név két részből áll

                Console.WriteLine($"Azonosító: {id}, Név: {nev}");
            }

            olvas.Close();
            Console.ReadLine();
        }
    }
}
```

> 🧠 **Mi történik a memóriában?**
> - A `sorok` tömbben minden egyes sor külön helyet foglal.  
> - Minden iterációban a `darabok` tömb egy új ideiglenes változó,  
>   amely tartalmazza az adott sor feldarabolt elemeit.  
> - Az `id` és `nev` lokális változók minden körben új értéket kapnak,  
>   majd a ciklus végén újra felülíródnak.

---

### 📁 Mintafájl – `versenyzok.txt`

A program akkor fog működni, ha a projekt könyvtárában található ez a fájl:

```
#12 Nagy Gábor 399 400 390
#7 Kis Csaba 403 392
#24 Juan Valerio 390 407 377 398
#3 Gipsz Jakab 390 375
```

> 💡 **Tipp:**  
> Visual Studio-ban a fájlra jobb klikk → **Properties** →  
> „Copy to Output Directory” → **Copy always**  
> Így a program futtatásakor mindig elérhető lesz a fájl.

---

## ✅ Eddigi eredmény

Ha most futtatod a programot, a kimenet hasonló lesz:

```
Azonosító: 12, Név: Nagy Gábor
Azonosító: 7, Név: Kis Csaba
Azonosító: 24, Név: Juan Valerio
Azonosító: 3, Név: Gipsz Jakab
```

Ez azt jelenti, hogy a fájlbeolvasás már működik,  
és az adatok feldarabolását is helyesen végeztük el.

---

**A következő részben (2. fejezet)**:  
létrehozzuk a **Versenyzo osztályt**,  
és megtanuljuk, hogyan lehet a beolvasott adatokat objektumokba szervezni,  
majd formázott módon kiírni őket a képernyőre.


# 2. Fejezet – A Versenyzo osztály és az objektumok kezelése

Most, hogy sikeresen beolvastuk a fájl sorait, ideje **objektumokat** létrehozni,  
amelyek egy-egy versenyző adatait tárolják.  
Ez a C# egyik legfontosabb eleme: az **osztályok** (`class`) használata.

---

## 🧩 2.1. Új osztály létrehozása – `Versenyzo.cs`

A projektben (jobb klikk → *Add → Class...*) hozzunk létre egy új fájlt:  
**Versenyzo.cs**, majd írjuk bele a következőt:

```csharp
using System;
using System.Linq;

namespace fa2_versenyzoOsztaly
{
    internal class Versenyzo
    {
        private int Id;               // Azonosító (pl. 12)
        private string Nev;           // Név (pl. Nagy Gábor)
        private int[] Eredmenyek;     // Ugrások eredményei (pl. [399, 400, 390])

        // Konstruktor – az objektum létrehozásakor fut le
        public Versenyzo(int id, string nev, int[] szamok)
        {
            this.Id = id;
            this.Nev = nev;
            this.Eredmenyek = szamok;
        }
    }
}
```

> 💬 **Magyarázat:**  
> - Az `internal` kulcsszó azt jelenti, hogy az osztály a **saját névtérben** (namespace) látható.  
> - A `private` mezők (Id, Nev, Eredmenyek) **kívülről nem érhetők el közvetlenül**, ez az **adatvédelem** alapja.  
> - A konstruktor (`public Versenyzo(...)`) az az **alapértelmezett beállító függvény**, ami akkor fut, amikor `new Versenyzo(...)`-t hívunk.

> 💡 **C++ párhuzam:**  
> Ugyanez C++-ban így nézne ki:
> ```cpp
> class Versenyzo {
>     int Id;
>     string Nev;
>     vector<int> Eredmenyek;
> public:
>     Versenyzo(int id, string nev, vector<int> szamok) {
>         Id = id;
>         Nev = nev;
>         Eredmenyek = szamok;
>     }
> };
> ```

---

## 🧩 2.2. Objektumok létrehozása a beolvasott adatokból

Most visszatérünk a **Program.cs**-hez, és a korábbi kódot bővítjük úgy,  
hogy minden sorból létrejöjjön egy új `Versenyzo` objektum.

A kód az alábbi formát kapja:

```csharp
using System;
using System.IO;
using System.Linq;

namespace fa2_versenyzoOsztaly
{
    internal class Program
    {
        static void Main(string[] args)
        {
            int id;
            string nev;
            int[] szamok;

            // Fájl megnyitása
            StreamReader olvas = new StreamReader("versenyzok.txt");
            string[] sorok = olvas.ReadToEnd().Split('\n');

            // Versenyzők tömbjének létrehozása
            Versenyzo[] versenyzok = new Versenyzo[sorok.Length];

            // Sorok feldolgozása
            for (int i = 0; i < versenyzok.Length; i++)
            {
                string[] darabok = sorok[i].Split(' ');

                id = int.Parse(darabok[0].Trim('#')); // '#' jel eltávolítása, szám konvertálása
                nev = darabok[1] + " " + darabok[2];  // név összefűzése

                // Ugrások eredményeinek beolvasása
                szamok = new int[darabok.Length - 3];
                for (int j = 0; j < szamok.Length; j++)
                {
                    szamok[j] = int.Parse(darabok[j + 3]);
                }

                // Új versenyző objektum létrehozása
                Versenyzo v = new Versenyzo(id, nev, szamok);
                versenyzok[i] = v;
            }

            olvas.Close();

            Console.WriteLine("Beolvasott adatok (Versenyzo objektumok):");
            foreach (Versenyzo v in versenyzok)
            {
                Console.WriteLine(v); // Itt majd a ToString() metódus formázza az adatokat
            }

            Console.ReadLine();
        }
    }
}
```

> 🧠 **Mi történik a memóriában?**
> - A `versenyzok` tömb minden eleme egy **Versenyzo típusú objektumra mutat**.  
> - Minden `Versenyzo` példány külön tárolja az `Id`, `Nev`, és `Eredmenyek` értékeit.  
> - A `new` kulcsszó a heap-en foglal helyet, tehát a `Versenyzo` objektumok a **memória dinamikus részén** jönnek létre.

---

## 🧩 2.3. Az adatok kiírása – `ToString()` metódus

Ahhoz, hogy a `Console.WriteLine(v)` helyesen jelenítse meg az adatokat,  
felül kell írnunk a `ToString()` metódust az osztályban.

Nyissuk meg újra a **Versenyzo.cs** fájlt, és egészítsük ki a következővel:

```csharp
public override string ToString()
{
    string s = $"{Id} {Nev} eredményei: ";
    for (int i = 0; i < Eredmenyek.Length; i++)
    {
        s += Eredmenyek[i] + ", ";
    }
    return s;
}
```

Így az osztályunk teljes jelenlegi állapota:

```csharp
using System;
using System.Linq;

namespace fa2_versenyzoOsztaly
{
    internal class Versenyzo
    {
        private int Id;
        private string Nev;
        private int[] Eredmenyek;

        public Versenyzo(int id, string nev, int[] szamok)
        {
            this.Id = id;
            this.Nev = nev;
            this.Eredmenyek = szamok;
        }

        public override string ToString()
        {
            string s = $"{Id} {Nev} eredményei: ";
            for (int i = 0; i < Eredmenyek.Length; i++)
            {
                s += Eredmenyek[i] + ", ";
            }
            return s;
        }
    }
}
```

> 💡 **Megjegyzés:**  
> A `ToString()` egy beépített metódus az **Object** osztályban,  
> amely minden C# osztály „őse”.  
> Ha felülírjuk (`override`), megadhatjuk, hogyan jelenjen meg az objektum szövegként.

> ⚙️ **C++ párhuzam:**  
> Ez hasonlít az `operator<<` túlterhelésére C++-ban:
> ```cpp
> friend ostream& operator<<(ostream& os, const Versenyzo& v) {
>     os << v.Id << " " << v.Nev << " eredményei: ";
>     for (int x : v.Eredmenyek) os << x << ", ";
>     return os;
> }
> ```

---

## ✅ Eddigi eredmény

Most már a program ki tudja írni az összes versenyző adatait,  
egyedi formázással, így:

```
Beolvasott adatok (Versenyzo objektumok):
12 Nagy Gábor eredményei: 399, 400, 390, 
7 Kis Csaba eredményei: 403, 392, 
24 Juan Valerio eredményei: 390, 407, 377, 398, 
3 Gipsz Jakab eredményei: 390, 375,
```

---

**A következő részben (3. fejezet):**  
Megírjuk a `legjobbUgras()` metódust, megtanuljuk a **LINQ** használatát (`Max()`),  
és a program végül **meghatározza a győztest** a legnagyobb ugrás alapján.

# 3. Fejezet – Legjobb ugrás meghatározása és a győztes kiírása

Eddig már sikeresen:
- beolvastuk a versenyzők adatait egy fájlból,  
- létrehoztuk a `Versenyzo` osztályt,  
- és ki is tudtuk írni az összes adatot.

Most a cél:
1. Minden versenyzőnél megállapítani a **legjobb ugrást**  
2. Megkeresni a **verseny győztesét** (akinél a legnagyobb érték található)  
3. A végén **kiírni a győztest** a konzolra.

---

## 🧩 3.1. A legjobb ugrás meghatározása – `legjobbUgras()` metódus

A `Versenyzo` osztályhoz hozzáadunk egy új metódust,  
ami visszaadja a `Eredmenyek` tömb legnagyobb elemét.

Nyissuk meg a **Versenyzo.cs** fájlt, és egészítsük ki az alábbi kóddal:

```csharp
public int legjobbUgras()
{
    return Eredmenyek.Max(); // A Max() a LINQ könyvtár része, a legnagyobb értéket adja vissza
}
```

Ezután a `ToString()` metódust is kiegészítjük, hogy a kiírásban szerepeljen a legjobb ugrás is:

```csharp
public override string ToString()
{
    string s = $"{Id} {Nev} eredményei: ";
    for (int i = 0; i < Eredmenyek.Length; i++)
    {
        s += Eredmenyek[i] + ", ";
    }
    s += "legjobb: " + legjobbUgras();
    return s;
}
```

A teljes osztály most így néz ki:

```csharp
using System;
using System.Linq;

namespace fa2_versenyzoOsztaly
{
    internal class Versenyzo
    {
        private int Id;
        private string Nev;
        private int[] Eredmenyek;

        public Versenyzo(int id, string nev, int[] szamok)
        {
            this.Id = id;
            this.Nev = nev;
            this.Eredmenyek = szamok;
        }

        public int legjobbUgras()
        {
            return Eredmenyek.Max();
        }

        public override string ToString()
        {
            string s = $"{Id} {Nev} eredményei: ";
            for (int i = 0; i < Eredmenyek.Length; i++)
            {
                s += Eredmenyek[i] + ", ";
            }
            s += "legjobb: " + legjobbUgras();
            return s;
        }
    }
}
```

> 💡 **Mi történik a háttérben?**  
> A `Eredmenyek` egy `int` tömb. A `Max()` függvény a LINQ (`System.Linq`) könyvtárból való,  
> és végigmegy a tömb elemein, majd visszaadja a **legnagyobb értéket**.  
>  
> **Memóriában:**  
> - A `Eredmenyek` tömb egy *heap* területen tárolódik.  
> - A `Max()` csak *végigolvassa* a tömböt (nem másolja), ezért hatékony.  
>  
> **C++ megfelelője:**
> ```cpp
> int legjobbUgras() {
>     return *max_element(Eredmenyek.begin(), Eredmenyek.end());
> }
> ```

---

## 🧩 3.2. A győztes meghatározása a `Main()` függvényben

Most, hogy minden `Versenyzo` tudja a saját legjobb eredményét,  
már csak annyi a dolgunk, hogy a tömb elemei közül **kiválasszuk a legjobbat**.

Nyissuk meg a **Program.cs** fájlt, és egészítsük ki az alábbi kóddal  
a már meglévő kód végén (a `foreach` ciklus után):

```csharp
Versenyzo legjobb = versenyzok[0]; // Kezdetben az első versenyző a legjobb
for (int i = 0; i < versenyzok.Length; i++)
{
    if (legjobb.legjobbUgras() < versenyzok[i].legjobbUgras())
    {
        legjobb = versenyzok[i]; // Ha találunk jobbat, frissítjük
    }
}

Console.WriteLine("\nA verseny győztese: " + legjobb);
```

Így a **Program.cs** fájl teljes, futtatható verziója most így néz ki:

```csharp
using System;
using System.IO;
using System.Linq;

namespace fa2_versenyzoOsztaly
{
    internal class Program
    {
        static void Main(string[] args)
        {
            int id;
            string nev;
            int[] szamok;

            StreamReader olvas = new StreamReader("versenyzok.txt");
            string[] sorok = olvas.ReadToEnd().Split('\n');
            Versenyzo[] versenyzok = new Versenyzo[sorok.Length];

            for (int i = 0; i < versenyzok.Length; i++)
            {
                string[] darabok = sorok[i].Split(' ');
                id = int.Parse(darabok[0].Trim('#'));
                nev = darabok[1] + " " + darabok[2];
                szamok = new int[darabok.Length - 3];
                for (int j = 0; j < szamok.Length; j++)
                {
                    szamok[j] = int.Parse(darabok[j + 3]);
                }
                Versenyzo v = new Versenyzo(id, nev, szamok);
                versenyzok[i] = v;
            }

            olvas.Close();

            Console.WriteLine("Beolvasott adatok:");
            foreach (Versenyzo v in versenyzok)
            {
                Console.WriteLine(v);
            }

            // Győztes meghatározása
            Versenyzo legjobb = versenyzok[0];
            for (int i = 0; i < versenyzok.Length; i++)
            {
                if (legjobb.legjobbUgras() < versenyzok[i].legjobbUgras())
                {
                    legjobb = versenyzok[i];
                }
            }

            Console.WriteLine("\nA verseny győztese: " + legjobb);
            Console.ReadLine();
        }
    }
}
```

> ⚙️ **Tipp:**  
> A `Console.WriteLine(legjobb);` azért működik, mert a `Versenyzo` osztályban felülírtuk a `ToString()` metódust.  
> Ha ezt nem tettük volna meg, akkor csak a típusnevet írná ki, például:
> ```
> fa2_versenyzoOsztaly.Versenyzo
> ```

> ⚠️ **Figyelem a fájlformátumra!**  
> A `Split('\n')` soronként választja szét az adatokat,  
> tehát ügyelj rá, hogy a `versenyzok.txt` fájl **Unix vagy Windows új sor karakterrel** végződjön.  
> Ha a beolvasás nem tökéletes, érdemes `Trim()`-et használni:  
> `string[] sorok = olvas.ReadToEnd().Trim().Split('\n');`

---

## 🧩 3.3. Mintafájl és programkimenet

### 📄 versenyzok.txt
```
#12 Nagy Gábor 399 400 390
#7 Kis Csaba 403 392
#24 Juan Valerio 390 407 377 398
#3 Gipsz Jakab 390 375
```

### 🖥️ Programkimenet
```
Beolvasott adatok:
12 Nagy Gábor eredményei: 399, 400, 390, legjobb: 400
7 Kis Csaba eredményei: 403, 392, legjobb: 403
24 Juan Valerio eredményei: 390, 407, 377, 398, legjobb: 407
3 Gipsz Jakab eredményei: 390, 375, legjobb: 390

A verseny győztese: 24 Juan Valerio eredményei: 390, 407, 377, 398, legjobb: 407
```

---

## 🎓 Összefoglalás

Ebben a részben megtanultuk:

✅ hogyan keresünk legnagyobb értéket egy tömbben (`Max()`),  
✅ hogyan hívunk metódusokat objektumokon (`legjobbUgras()`),  
✅ és hogyan találjuk meg a legjobb elemet egy lista/tömb közül.

> 💡 **Extra tipp (LINQ verzió):**  
> Ugyanezt meg lehetne írni egyetlen sorban is:
> ```
> Versenyzo legjobb = versenyzok.OrderByDescending(x => x.legjobbUgras()).First();
> ```
>  
> Ez a **LINQ** segítségével rendezi a versenyzőket a legjobb ugrás szerint,  
> és a `First()` kiválasztja a legjobbat.

---

🥳 **Gratulálok!**  
Most már egy teljes, objektumorientált C# programot készítettél,  
ami fájlból olvas adatokat, objektumokat hoz létre,  
és kiértékeli a legjobb eredményt.

Kiváló alap a következő lépésekhez: fájlírás, hibakezelés (`try-catch`), vagy GUI-készítés (WPF, WinForms)!
