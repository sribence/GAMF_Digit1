# C# Kerítés Feladat Oktatóanyag - Új struktúra

Ez az oktatóanyag egy C# programot mutat be, amely egy utca kerítésadatait dolgozza fel. A korábbi verziót átdolgoztuk, hogy a feladatokat lépésről lépésre, a `feladatok.txt` fájlban leírtak szerint, 1-6 pontban mutassuk be. Minden feladat részletes magyarázatokkal, kódrészletekkel és példákkal lesz illusztrálva, mintha egy kezdő programozónak tanítanánk. A kódok mindig a teljes, eddig elkészült állapotot mutatják, így követhető a fejlődés.

## Bevezetés

Ez a projekt egy C# konzolalkalmazás, amely egy utca kerítéseinek adatait kezeli. Az adatok egy `kerites.txt` nevű fájlból kerülnek beolvasásra, és a program különféle feladatokat hajt végre ezekkel az adatokkal. A projekt három fő C# fájlból áll: `Kerites.cs`, `Utca.cs` és `Program.cs`.

A tananyag során megismerkedünk az alábbi C# koncepciókkal:
*   Osztályok és objektumok
*   Névterek (Namespaces)
*   Mezők (Fields), Tulajdonságok (Properties), Konstruktorok (Constructors)
*   Listák (`List<T>`) és Indexerek
*   Fájl I/O (`StreamReader`, `StreamWriter`)
*   Hibakezelés (`try-catch`)
*   Konzol I/O (`Console.WriteLine`, `Console.ReadLine`)
*   String manipuláció és konverzió

## 1. Feladat: Adatbeolvasás és tárolás (`kerites.txt`)

Ez az első lépés a program működésének megértéséhez. Itt beolvassuk a kerítések adatait egy fájlból, és elmentjük őket az alkalmazásunkban.

### A `Kerites` Osztály: Egy kerítés szegmens modellje

A `Kerites` osztály egyetlen kerítés szegmens adatait tárolja. Gondoljunk rá úgy, mint egy "kerítés darab" tervrajzára.

```csharp
// innentől új kód
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace fa2_KeritesFeladat
{
    // Az 'internal' kulcsszó azt jelenti, hogy ez az osztály csak ugyanazon a projekten belül érhető el.
    internal class Kerites
    {
        // Mezők (Fields): Privát változók, amelyek az objektum belső állapotát tárolják.
        // Ezeket csak az osztályon belülről lehet elérni, ami az enkapszuláció alapelve.
        private int oldal;       // Az utca melyik oldalán van a kerítés (0: páros, 1: páratlan).
        private int szelesseg;  // A kerítés szegmens szélessége méterben.
        private char szin;      // A kerítés színe (pl. 'R' piros, 'B' kék, '#' vagy ':' sérült állapot).
        private int hazszam;    // Az ehhez a kerítéshez tartozó házszám.

        // Tulajdonságok (Properties): Nyilvános hozzáférési pontok a privát mezőkhöz.
        // A 'get' hozzáférő lehetővé teszi az érték olvasását az osztályon kívülről.
        // Nincs 'set' hozzáférő, így az értékek az objektum létrehozása után nem módosíthatók közvetlenül.
        public int Oldal { get { return oldal; } }
        public int Szelesseg { get { return szelesseg; } }
        public int Hazszam { get { return hazszam; } }
        public char Szin { get { return szin; } }

        // Konstruktor: Egy speciális metódus, amely az osztály új példányának (objektumának) létrehozásakor hívódik meg.
        // Itt inicializáljuk a mezőket a megadott értékekkel.
        public Kerites(int oldal, int szelesseg, char szin, int hazszam)
        {
            // A 'this' kulcsszó az aktuális objektumpéldányra utal.
            // Megkülönbözteti az osztály mezőit (pl. this.oldal) a konstruktor paramétereitől (pl. oldal).
            this.oldal = oldal;
            this.szelesseg = szelesseg;
            this.szin = szin;
            this.hazszam = hazszam;
        }
    }
}
```

**Magyarázat:**
*   **Névtér (`namespace fa2_KeritesFeladat`)**: Kódunkat egy logikai csoportba, a `fa2_KeritesFeladat` névtérbe helyezzük. Ez segít elkerülni a névütközéseket, ha más kódokkal dolgozunk.
*   **Osztály (`internal class Kerites`)**: Létrehoztunk egy `Kerites` nevű osztályt. Az `internal` hozzáférési módosító azt jelenti, hogy az osztály csak a projektünkön belül használható.
*   **Mezők (`private int oldal;`)**: A `private` kulcsszó biztosítja, hogy ezek a változók (mezők) csak a `Kerites` osztályon belülről legyenek elérhetők. Ez az **enkapszuláció** egyik alapvető eleme, amely megvédi az objektum belső állapotát.
*   **Tulajdonságok (`public int Oldal { get { return oldal; } }`)**: A `public` tulajdonságok (properties) lehetővé teszik az osztályon kívüli hozzáférést a privát mezőkhöz. A `get` rész csak olvashatóságot biztosít. Ez egy "getter" metódus C# stílusban.
*   **Konstruktor (`public Kerites(...)`)**: Ez a speciális metódus akkor fut le, amikor egy új `Kerites` objektumot hozunk létre (pl. `new Kerites(...)`). A `this` kulcsszó segít megkülönböztetni a mezőket a paraméterektől.

**Információs blokk:**

> **C# vs. C++: Osztályok és Tagok**
>
> C++-ban hasonlóan definiálnánk egy osztályt, de a hozzáférési módosítók (public, private) és a tulajdonságok (properties) szintaxisa eltérő:
>
> ```cpp
> class Kerites {
> private: // Privát tagok
>     int oldal;
>     int szelesseg;
>     char szin;
>     int hazszam;
> public: // Nyilvános tagok
>     // Konstruktor: C++-ban initializer list-et használunk
>     Kerites(int o, int s, char sz, int h) : oldal(o), szelesseg(s), szin(sz), hazszam(h) {}
>
>     // Getter metódusok (C# tulajdonságok megfelelői)
>     int getOldal() const { return oldal; }
>     int getSzelesseg() const { return szelesseg; }
>     int getHazszam() const { return hazszam; }
>     char getSzin() const { return szin; }
> };
> ```
>
> A C# tulajdonságai (properties) tömörebb módon teszik lehetővé az adatok elérését, mint a C++ explicit getter metódusai.

**Memória:** Amikor létrehozunk egy `Kerites` objektumot (`new Kerites(...)`), a .NET futtatókörnyezet a memóriában (a "heap"-en) foglal helyet az objektum mezőinek tárolására. A konstruktor inicializálja ezeket a memóriaterületeket a megadott értékekkel.

### Az `Utca` Osztály: Több kerítés kezelése és fájlbeolvasás

Az `Utca` osztály az egész utcát reprezentálja, és a benne lévő összes `Kerites` objektumot kezeli. Ez az osztály felelős a `kerites.txt` fájl beolvasásáért is.

```csharp
// innentől új kód
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

// ... namespace fa2_KeritesFeladat ...

    internal class Utca
    {
        // A List egy dinamikus tömb, amely képes növekedni vagy zsugorodni.
        // Itt tároljuk a Kerites objektumokat, amelyek az utca telkeit reprezentálják.
        private List<Kerites> telkek;

        // Indexer: Lehetővé teszi a lista elemeinek tömbszerű elérését (pl. utca[0]).
        // A 'this' kulcsszó az Utca objektumra magára utal.
        public Kerites this[int index]
        {
            get { return telkek[index]; } // Visszaadja a Kerites objektumot a megadott indexen.
        }

        // Tulajdonság (Property): Visszaadja az utca telkeinek számát.
        public int Db
        {
            get { return telkek.Count; } // A 'Count' a List osztály beépített tulajdonsága.
        }

        // 1. feladat: Konstruktor a kerítésadatok fájlból való beolvasásához.
        // Ez a metódus felelős a 'kerites.txt' fájl tartalmának feldolgozásáért.
        public Utca(string fajlnev)
        {
            telkek = new List<Kerites>(); // Inicializáljuk az üres listát a telkek tárolására.
            int paros = 0; // Számláló a páros házszámok generálásához.
            int plan = -1; // Számláló a páratlan házszámok generálásához.

            // A 'using' utasítás biztosítja, hogy a StreamReader (fájlolvasó) erőforrásai
            // automatikusan felszabaduljanak a blokk végén, még hiba esetén is.
            using (StreamReader olvas = new StreamReader(fajlnev))
            {
                // Végigmegyünk a fájl minden során, amíg el nem érjük a végét.
                while (!olvas.EndOfStream)
                {
                    string egysor = olvas.ReadLine(); // Beolvas egy sort a fájlból.
                    // A sort szóközök alapján darabokra bontjuk.
                    // Példa: "0 5 R" -> egysordarabok tömb lesz: ["0", "5", "R"]
                    string[] egysordarabok = egysor.Split(' ');

                    // A beolvasott string darabokat konvertáljuk a megfelelő adattípusokra.
                    int oldal = Convert.ToInt32(egysordarabok[0]);     // Az első elem az utca oldala (0 vagy 1).
                    int szelesseg = Convert.ToInt32(egysordarabok[1]); // A második elem a kerítés szélessége.
                    char szin = Convert.ToChar(egysordarabok[2]);      // A harmadik elem a kerítés színe.

                    int hazszam; // Deklaráljuk a házszám változót.

                    // A házszámot az utca oldala alapján határozzuk meg.
                    if (oldal == 0) // Ha a páros oldalon van a telek (oldal == 0)
                    {
                        paros += 2;     // Növeljük a páros házszám számlálót 2-vel (pl. 0 -> 2, 2 -> 4).
                        hazszam = paros; // Az új páros házszámot rendeljük hozzá.
                    }
                    else // Ha a páratlan oldalon van a telek (oldal == 1)
                    {
                        plan += 2;      // Növeljük a páratlan házszám számlálót 2-vel (pl. -1 -> 1, 1 -> 3).
                        hazszam = plan; // Az új páratlan házszámot rendeljük hozzá.
                    }

                    // Létrehozunk egy új Kerites objektumot a feldolgozott adatokkal,
                    // és hozzáadjuk a telkek listájához.
                    telkek.Add(new Kerites(oldal, szelesseg, szin, hazszam));
                }
            }
        }
    }
}
```

**Magyarázat:**
*   **Lista (`private List<Kerites> telkek;`)**: A `List<T>` egy dinamikusan bővülő gyűjtemény, amelyben `Kerites` objektumokat tárolunk. Ez hasonló a C++ `std::vector`-jához.
*   **Indexer (`public Kerites this[int index]`)**: Ez lehetővé teszi, hogy az `Utca` objektum elemeihez tömbszerűen férjünk hozzá (pl. `utca[0]`). A `get` hozzáférő a lista adott indexű elemét adja vissza.
*   **Tulajdonság (`public int Db { get { return telkek.Count; } }`)**: A `Db` tulajdonság (valószínűleg a "darab" rövidítése) visszaadja a `telkek` lista elemeinek számát, azaz az utca telkeinek számát.
*   **Konstruktor (`public Utca(string fajlnev)`)**:
    *   Ez a konstruktor felelős a `kerites.txt` fájl beolvasásáért.
    *   **`StreamReader` és `using`**: A `StreamReader` fájlok olvasására szolgál. A `using` blokk biztosítja, hogy a fájlkezelő erőforrásai automatikusan bezáródjanak, ami jó gyakorlat.
    *   **`olvas.ReadLine()`**: Egy sort olvas be a fájlból.
    *   **`egysor.Split(' ')`**: A beolvasott sort szóközök mentén darabokra bontja. Ez egy tömböt eredményez (pl. `["0", "5", "R"]`).
    *   **`Convert.ToInt32()` és `Convert.ToChar()`**: Ezek a metódusok a string darabokat a megfelelő adattípusokra (egész szám, karakter) konvertálják.
    *   **Házszám generálás**: A kód az `oldal` (0 vagy 1) alapján számítja ki a házszámokat, biztosítva a páros és páratlan oldalak megfelelő számozását (2, 4, 6... és 1, 3, 5...).
    *   **`telkek.Add(new Kerites(...))`**: Minden beolvasott sorból létrehoz egy `Kerites` objektumot, és hozzáadja a `telkek` listához.

**Információs blokk:**

> **C# vs. C++: Listák és Fájl I/O**
>
> *   **Listák**: A C# `List<T>` hasonló a C++ `std::vector<T>`-jéhez. Mindkettő dinamikus tömb, amely automatikusan kezeli a memóriát.
> *   **Fájl Olvasás**: A C# `StreamReader` analóg a C++ `std::ifstream`-jével. Mindkettőt fájlokból való olvasásra használják. A C# `using` utasítása kényelmes módja annak biztosítására, hogy az olyan erőforrások, mint a fájlfolyamok, megfelelően legyenek bezárva, hasonlóan a C++ RAII (Resource Acquisition Is Initialization) elvéhez.
> *   **String Felbontás**: A C# `string.Split()` hasonló a C++ `std::stringstream` és `std::getline` használatához a tokenek kinyeréséhez egy stringből.

**Memória:** Amikor a `new List<Kerites>()` hívás történik, memóriát foglalunk magának a lista objektumnak. Ahogy a `Kerites` objektumokat hozzáadjuk a `telkek.Add()` segítségével, minden `Kerites` objektumhoz memóriát foglalunk a "heap"-en, és a lista ezekre az objektumokra tartja a hivatkozásokat. A `StreamReader` és `StreamWriter` is fájlkezelőket és belső pufferöket kezel, amelyeket a `using` blokk befejezésekor vagy az objektumok eldobásakor (dispose) felszabadítanak.

---

## 2. Feladat: Eladott telkek száma

Ebben a lépésben megtudjuk, hány telek került eladásra az utcában. Ehhez az `Utca` osztály `Db` tulajdonságát használjuk.

```csharp
// innentől új kód
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace fa2_KeritesFeladat
{
    // ... (Kerites osztály itt található) ...

    internal class Utca
    {
        private List<Kerites> telkek;

        public Kerites this[int index]
        {
            get { return telkek[index]; }
        }

        // Tulajdonság: Visszaadja az utca telkeinek számát.
        public int Db
        {
            get { return telkek.Count; } // A 'Count' a List osztály beépített tulajdonsága.
        }

        // 1. feladat: Konstruktor a kerítésadatok fájlból való beolvasásához.
        public Utca(string fajlnev)
        {
            telkek = new List<Kerites>();
            int paros = 0, plan = -1;
            using (StreamReader olvas = new StreamReader(fajlnev))
            {
                while (!olvas.EndOfStream)
                {
                    string egysor = olvas.ReadLine();
                    string[] egysordarabok = egysor.Split(' ');
                    int oldal = Convert.ToInt32(egysordarabok[0]);
                    int szelesseg = Convert.ToInt32(egysordarabok[1]);
                    char szin = Convert.ToChar(egysordarabok[2]);
                    int hazszam;
                    if (oldal == 0)
                    {
                        paros += 2;
                        hazszam = paros;
                    }
                    else
                    {
                        plan += 2;
                        hazszam = plan;
                    }
                    telkek.Add(new Kerites(oldal, szelesseg, szin, hazszam));
                }
            }
        }
        // ... (További feladatok itt következnek) ...
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            try
            {
                // 1. feladat: Létrehozzuk az Utca objektumot a 'kerites.txt' fájlból.
                Utca utca = new Utca("kerites.txt");

                // 2. feladat: Kiírjuk az eladott telkek számát.
                Console.WriteLine("2. feladat");
                // Az 'utca.Db' tulajdonság visszaadja a telkek számát a List<Kerites> Count tulajdonságán keresztül.
                Console.WriteLine("Az eladott telkek száma: {0}", utca.Db);
                
                // ... (További feladatok itt következnek) ...
            }
            catch (Exception e)
            {
                Console.WriteLine("Hiba!");
                Console.WriteLine(e.Message);
            }
            Console.ReadLine();
        }
    }
}
```

**Magyarázat:**
*   A `Program.cs` fájlban létrehoztuk az `Utca` objektumot az 1. feladatban.
*   A `Console.WriteLine("2. feladat");` kiírja a feladat sorszámát.
*   Az `utca.Db` használatával hozzáférünk az `Utca` osztály `Db` tulajdonságához, amely a `telkek` lista méretét adja vissza. Ez a szám az eladott telkek számát jelenti.
*   A `{0}` egy helyőrző a `Console.WriteLine` metódusban, amelyet a `utca.Db` értékével helyettesítünk.

**Információs blokk:**

> **C# vs. C++: Tulajdonságok és Konzol I/O**
>
> *   **Tulajdonságok (Properties)**: A C# tulajdonságai (pl. `Db`) egy kényelmes módot kínálnak az adatok elérésére, hasonlóan a C++ getter metódusaihoz, de rövidebb szintaxissal.
> *   **Konzol I/O**: A `Console.WriteLine` és `Console.Write` a C# standard kimeneti metódusai, hasonlóan a C++ `std::cout`-hoz. A `Console.ReadLine` pedig a C++ `std::cin`-hez hasonlóan vár felhasználói bemenetet.

---

## 3. Feladat: Az utolsó telek adatai

Most megtudjuk, hogy az utolsó eladott telek az utca melyik oldalán található, és mi a házszáma. Ehhez az `Utca` osztály indexerét és a `Kerites` osztály tulajdonságait használjuk.

```csharp
// innentől új kód
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace fa2_KeritesFeladat
{
    internal class Kerites
    {
        private int oldal;
        private int szelesseg;
        private char szin;
        private int hazszam;
        public int Oldal { get { return oldal; } }
        public int Szelesseg { get { return szelesseg; } }
        public int Hazszam { get { return hazszam; } }
        public char Szin { get { return szin; } }

        public Kerites(int oldal, int szelesseg, char szin, int hazszam)
        {
            this.oldal = oldal;
            this.szelesseg = szelesseg;
            this.szin = szin;
            this.hazszam = hazszam;
        }
    }

    internal class Utca
    {
        private List<Kerites> telkek;

        // Indexer: Lehetővé teszi a lista elemeinek tömbszerű elérését (pl. utca[0]).
        public Kerites this[int index]
        {
            get { return telkek[index]; }
        }

        public int Db
        {
            get { return telkek.Count; }
        }

        public Utca(string fajlnev)
        {
            telkek = new List<Kerites>();
            int paros = 0, plan = -1;
            using (StreamReader olvas = new StreamReader(fajlnev))
            {
                while (!olvas.EndOfStream)
                {
                    string egysor = olvas.ReadLine();
                    string[] egysordarabok = egysor.Split(' ');
                    int oldal = Convert.ToInt32(egysordarabok[0]);
                    int szelesseg = Convert.ToInt32(egysordarabok[1]);
                    char szin = Convert.ToChar(egysordarabok[2]);
                    int hazszam;
                    if (oldal == 0)
                    {
                        paros += 2;
                        hazszam = paros;
                    }
                    else
                    {
                        plan += 2;
                        hazszam = plan;
                    }
                    telkek.Add(new Kerites(oldal, szelesseg, szin, hazszam));
                }
            }
        }
        // ... (További feladatok itt következnek) ...
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            try
            {
                Utca utca = new Utca("kerites.txt");

                Console.WriteLine("2. feladat");
                Console.WriteLine("Az eladott telkek száma: {0}", utca.Db);

                // 3. feladat: Megjelenítjük az utolsó telek adatait.
                Console.WriteLine("3. feladat");
                Console.Write("A "); // Kiírjuk az "A " szöveget.

                // Az 'utca.Db - 1' az utolsó elem indexét adja meg (mivel a listák 0-tól indexeltek).
                // Az 'utca[index]' használatával kapjuk meg az utolsó Kerites objektumot.
                if (utca[utca.Db - 1].Oldal == 0) // Ellenőrizzük az oldal tulajdonságát.
                {
                    Console.Write("páros"); // Ha 0, akkor páros oldal.
                }
                else
                {
                    Console.Write("páratlan"); // Ha 1, akkor páratlan oldal.
                }
                // Kiírjuk az utolsó telek házszámát is.
                Console.WriteLine(" oldalon adták el az utolsó telket.");
                Console.WriteLine("Az utolsó telek házszáma: {0} ", utca[utca.Db - 1].Hazszam);

                // ... (További feladatok itt következnek) ...
            }
            catch (Exception e)
            {
                Console.WriteLine("Hiba!");
                Console.WriteLine(e.Message);
            }
            Console.ReadLine();
        }
    }
}
```

**Magyarázat:**
*   Az `utca.Db - 1` kiszámítja az utolsó elem indexét a listában. Mivel a listák nullától indexeltek, az utolsó elem indexe mindig eggyel kisebb, mint a lista mérete.
*   Az `utca[utca.Db - 1]` az `Utca` osztály indexerét használja, hogy hozzáférjen az utolsó `Kerites` objektumhoz.
*   Ezután a `Kerites` objektum `Oldal` és `Hazszam` tulajdonságait használjuk az információk kiírásához.
*   Az `if/else` szerkezet dönti el, hogy "páros" vagy "páratlan" szót írjunk ki az `Oldal` értékétől függően.

**Információs blokk:**

> **C# vs. C++: Indexerek és Feltételes logikák**
>
> *   **Indexerek**: A C# indexerei (pl. `this[int index]`) lehetővé teszik az osztálykészítőknek, hogy az osztály példányait tömbként lehessen használni. A C++-ban ezt általában egy `operator[]` túlterhelésével érjük el.
> *   **Feltételes logikák**: Az `if-else` szerkezet mindkét nyelvben alapvető a döntéshozatalhoz. A C# `Console.Write` és `Console.WriteLine` metódusai a kimenet vezérlésére szolgálnak.

---

## 4. Feladat: Szomszédos kerítések színe

Itt egy olyan házszámot kell találnunk a páratlan oldalon, amelynek szomszédjánál azonos színű a kerítés. Ehhez az `Utca` osztály `Feladat4` metódusát használjuk.

```csharp
// innentől új kód
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace fa2_KeritesFeladat
{
    internal class Kerites
    {
        private int oldal;
        private int szelesseg;
        private char szin;
        private int hazszam;
        public int Oldal { get { return oldal; } }
        public int Szelesseg { get { return szelesseg; } }
        public int Hazszam { get { return hazszam; } }
        public char Szin { get { return szin; } }

        public Kerites(int oldal, int szelesseg, char szin, int hazszam)
        {
            this.oldal = oldal;
            this.szelesseg = szelesseg;
            this.szin = szin;
            this.hazszam = hazszam;
        }
    }

    internal class Utca
    {
        private List<Kerites> telkek;

        public Kerites this[int index]
        {
            get { return telkek[index]; }
        }

        public int Db
        {
            get { return telkek.Count; }
        }

        public Utca(string fajlnev)
        {
            telkek = new List<Kerites>();
            int paros = 0, plan = -1;
            using (StreamReader olvas = new StreamReader(fajlnev))
            {
                while (!olvas.EndOfStream)
                {
                    string egysor = olvas.ReadLine();
                    string[] egysordarabok = egysor.Split(' ');
                    int oldal = Convert.ToInt32(egysordarabok[0]);
                    int szelesseg = Convert.ToInt32(egysordarabok[1]);
                    char szin = Convert.ToChar(egysordarabok[2]);
                    int hazszam;
                    if (oldal == 0)
                    {
                        paros += 2;
                        hazszam = paros;
                    }
                    else
                    {
                        plan += 2;
                        hazszam = plan;
                    }
                    telkek.Add(new Kerites(oldal, szelesseg, szin, hazszam));
                }
            }
        }

        // 4. feladat: Ellenőrzi, hogy van-e szomszédos kerítés azonos színnel.
        public int Feladat4()
        {
            // Végigmegyünk a telkeken, kivéve az utolsót, hogy össze tudjuk hasonlítani az aktuális telket a következővel.
            for (int i = 0; i < Db - 1; i++)
            {
                // Feltételek:
                // 1. Az aktuális telek (i) és a következő telek (i+1) azonos oldalon legyenek.
                //    (telkek[i].Oldal + telkek[i+1].Oldal == 2) azt jelenti, hogy mindkettő 1-es (páratlan) vagy mindkettő 0-s (páros).
                // 2. Azonos színűek legyenek a kerítések.
                // 3. A szín ne legyen '#' (törött) vagy ':' (sérült).
                if ((telkek[i].Oldal + telkek[i + 1].Oldal == 2) &&
                    telkek[i].Szin == telkek[i + 1].Szin && telkek[i].Szin != '#' && telkek[i].Szin != ':')
                {
                    // Ha minden feltétel teljesül, visszaadjuk az aktuális telek házszámát.
                    return telkek[i].Hazszam;
                }
            }
            // Ha nem találtunk ilyen párt, -1-et adunk vissza.
            return -1;
        }
        // ... (További feladatok itt következnek) ...
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            try
            {
                Utca utca = new Utca("kerites.txt");

                Console.WriteLine("2. feladat");
                Console.WriteLine("Az eladott telkek száma: {0}", utca.Db);

                Console.WriteLine("3. feladat");
                Console.Write("A ");
                if (utca[utca.Db - 1].Oldal == 0)
                {
                    Console.Write("páros");
                }
                else
                {
                    Console.Write("páratlan");
                }
                Console.WriteLine(" oldalon adták el az utolsó telket.");
                Console.WriteLine("Az utolsó telek házszáma: {0} ", utca[utca.Db - 1].Hazszam);

                // 4. feladat: Kiírjuk az eredményt a Feladat4 metódus hívásával.
                Console.WriteLine("4. feladat");
                // A metódus visszaadja a házszámot, ha talál egyező szomszédos kerítést, különben -1-et.
                Console.WriteLine("A szomszédossal egyezik a kerítés színe: " + utca.Feladat4());

                // ... (További feladatok itt következnek) ...
            }
            catch (Exception e)
            {
                Console.WriteLine("Hiba!");
                Console.WriteLine(e.Message);
            }
            Console.ReadLine();
        }
    }
}
```

**Magyarázat:**
*   Az `Utca` osztályban a `Feladat4` metódus egy ciklusban (`for`) végigmegy a telkeken.
*   Minden iterációban összehasonlítja az aktuális telek (`telkek[i]`) és a következő telek (`telkek[i+1]`) adatait.
*   A feltételek ellenőrzik, hogy mindkét telek azonos oldalon legyen (`Oldal + Oldal == 2`), azonos színűek legyenek (`Szin == Szin`), és a szín ne legyen speciális karakter (`#` vagy `:`).
*   Ha ezek a feltételek teljesülnek, a metódus azonnal visszaadja az aktuális telek házszámát (`telkek[i].Hazszam`).
*   Ha a ciklus lefut anélkül, hogy ilyen párt talált volna, a metódus -1-et ad vissza.
*   A `Program.cs` fájlban a `Console.WriteLine("A szomszédossal egyezik a kerítés színe: " + utca.Feladat4());` sor hívja meg ezt a metódust és írja ki az eredményt.

**Információs blokk:**

> **C# vs. C++: Ciklusok és Feltételek**
>
> *   **Ciklusok**: A C# `for` ciklus hasonló a C++ `for` ciklushoz. Mindkettő lehetővé teszi egy kódblokk ismételt végrehajtását egy megadott feltétel alapján.
> *   **Feltételek**: A C# `if` és `else` szerkezetek, valamint a logikai operátorok (`&&` az AND-ra, `!=` az "nem egyenlő"-re) a C++-ban is hasonlóan működnek. A `||` (OR) operátor is elérhető.

---

## 5. Feladat: Házszám alapján színjavaslat

Ez a feladat interaktívabb: bekérünk egy házszámot a felhasználótól, majd megkeressük a hozzá tartozó kerítés színét, és javasolunk egy új színt, amely különbözik a szomszédokétól.

```csharp
// innentől új kód
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace fa2_KeritesFeladat
{
    internal class Kerites
    {
        private int oldal;
        private int szelesseg;
        private char szin;
        private int hazszam;
        public int Oldal { get { return oldal; } }
        public int Szelesseg { get { return szelesseg; } }
        public int Hazszam { get { return hazszam; } }
        public char Szin { get { return szin; } }

        public Kerites(int oldal, int szelesseg, char szin, int hazszam)
        {
            this.oldal = oldal;
            this.szelesseg = szelesseg;
            this.szin = szin;
            this.hazszam = hazszam;
        }
    }

    internal class Utca
    {
        private List<Kerites> telkek;

        public Kerites this[int index]
        {
            get { return telkek[index]; }
        }

        public int Db
        {
            get { return telkek.Count; }
        }

        public Utca(string fajlnev)
        {
            telkek = new List<Kerites>();
            int paros = 0, plan = -1;
            using (StreamReader olvas = new StreamReader(fajlnev))
            {
                while (!olvas.EndOfStream)
                {
                    string egysor = olvas.ReadLine();
                    string[] egysordarabok = egysor.Split(' ');
                    int oldal = Convert.ToInt32(egysordarabok[0]);
                    int szelesseg = Convert.ToInt32(egysordarabok[1]);
                    char szin = Convert.ToChar(egysordarabok[2]);
                    int hazszam;
                    if (oldal == 0)
                    {
                        paros += 2;
                        hazszam = paros;
                    }
                    else
                    {
                        plan += 2;
                        hazszam = plan;
                    }
                    telkek.Add(new Kerites(oldal, szelesseg, szin, hazszam));
                }
            }
        }

        // 4. feladat: Ellenőrzi, hogy van-e szomszédos kerítés azonos színnel.
        public int Feladat4()
        {
            for (int i = 0; i < Db - 1; i++)
            {
                if ((telkek[i].Oldal + telkek[i + 1].Oldal == 2) &&
                    telkek[i].Szin == telkek[i + 1].Szin && telkek[i].Szin != '#' && telkek[i].Szin != ':')
                {
                    return telkek[i].Hazszam;
                }
            }
            return -1;
        }

        // 5. feladat: Meghatároz egy lehetséges új színt egy adott házszámhoz.
        public char Feladat5(int szam)
        {
            int keresett = 0; // Az adott házszámú telek indexe.

            // Megkeressük az adott házszámú telek indexét a listában.
            for (int i = 0; i < Db; i++)
            {
                if (telkek[i].Hazszam == szam)
                {
                    Console.WriteLine("A kerítés színe / állapota: " + telkek[i].Szin); // Kiírjuk a jelenlegi színt/állapotot.
                    keresett = i; // Tároljuk az indexet.
                    break; // Kilépünk a ciklusból, mert megtaláltuk.
                }
            }

            int also = 0; // Az alsó szomszéd indexe.
            int felso = 0; // A felső szomszéd indexe.

            // Megkeressük az alsó szomszéd indexét.
            // Ha a házszám 1 vagy 2, akkor nincs alsó szomszédja azonos oldalon a listánkban,
            // ezért a jelenlegi telek indexét használjuk (ezzel elkerüljük a hibát).
            if (szam == 1 || szam == 2)
            {
                also = keresett;
            }
            else
            {
                // Ellenkező esetben keressük meg a 'szam - 2' házszámú telket.
                // Ez feltételezi, hogy a házszámok 2-vel növekednek az utca mindkét oldalán.
                while (telkek[also].Hazszam != szam - 2)
                    also++;
            }

            // Megkeressük a felső szomszéd indexét.
            // Előre haladva addig keresünk, amíg meg nem találjuk a 'szam + 2' házszámú telket.
            // Biztosítjuk, hogy 'felso' ne lépjen ki a lista határain.
            while (felso < Db - 1 && telkek[felso].Hazszam != szam + 2)
                felso++;

            // Véletlen szín generálása 'A' és 'D' között (ASCII értékek 65-68).
            Random x = new Random();
            char uj = (char)x.Next(65, 69); // Generál egy karaktert 'A' és 'D' között.

            // Biztosítjuk, hogy az új szín eltérjen az aktuális telek színétől és a szomszédok színeitől.
            // Ez a ciklus addig generál új véletlen színeket, amíg egy érvényeset nem talál.
            while (telkek[also].Szin == uj || telkek[felso].Szin == uj || telkek[keresett].Szin == uj)
            {
                uj = (char)x.Next(65, 69);
            }
            return uj; // Visszaadjuk az érvényes új színt.
        }
        // ... (További feladatok itt következnek) ...
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            try
            {
                Utca utca = new Utca("kerites.txt");

                Console.WriteLine("2. feladat");
                Console.WriteLine("Az eladott telkek száma: {0}", utca.Db);

                Console.WriteLine("3. feladat");
                Console.Write("A ");
                if (utca[utca.Db - 1].Oldal == 0)
                {
                    Console.Write("páros");
                }
                else
                {
                    Console.Write("páratlan");
                }
                Console.WriteLine(" oldalon adták el az utolsó telket.");
                Console.WriteLine("Az utolsó telek házszáma: {0} ", utca[utca.Db - 1].Hazszam);

                Console.WriteLine("4. feladat");
                Console.WriteLine("A szomszédossal egyezik a kerítés színe: " + utca.Feladat4());

                // 5. feladat: Bekérjük a felhasználó házszámát, és javasolunk egy színt.
                Console.WriteLine("5. feladat");
                int szam; // Változó a felhasználó által megadott házszám tárolására.
                try
                {
                    Console.Write("Adjon meg egy házszámot! "); // Kérjük a felhasználót.
                    // Beolvassuk a felhasználó által beírt sort, és megpróbáljuk integer-ré alakítani.
                    szam = int.Parse(Console.ReadLine());

                    // Érvényesítjük a megadott házszámot.
                    // Ellenőrizzük, hogy a házszám 1 és az utolsó telek házszáma között van-e.
                    if (szam < 1 || szam > utca[utca.Db - 1].Hazszam)
                    {
                        // Ha érvénytelen, kivételt dobunk egy egyedi üzenettel.
                        throw new Exception("Hibás érték! A házszám értéke 83 lett. ");
                    }
                }
                catch (Exception e) // Elkapjuk az esetleges hibákat (pl. nem szám bevitele).
                {
                    // Ha hiba történik, beállítunk egy alapértelmezett házszámot, és kiírjuk a hibaüzenetet.
                    szam = 83; // Alapértelmezett házszám, ha a bemenet hibás.
                    Console.WriteLine(e.Message); // Kiírjuk a hibaüzenetet.
                }
                // Meghívjuk a Feladat5 metódust a megadott házszámmal, és kiírjuk a javasolt színt.
                Console.WriteLine("Egy lehetséges festési szín: " + utca.Feladat5(szam));

                // ... (További feladatok itt következnek) ...
            }
            catch (Exception e)
            {
                Console.WriteLine("Hiba!");
                Console.WriteLine(e.Message);
            }
            Console.ReadLine();
        }
    }
}
```

**Magyarázat:**
*   **Felhasználói bemenet**: A `Console.Write` és `Console.ReadLine` segítségével kérünk be egy házszámot a felhasználótól.
*   **`int.Parse()`**: Ezzel a metódussal próbáljuk meg a beolvasott stringet integer-ré (egész számmá) alakítani.
*   **Hibakezelés (`try-catch`)**: A `try-catch` blokk azért fontos, mert ha a felhasználó nem számot ír be, a `int.Parse()` hibát dobna. A `catch` blokk elkapja ezt a hibát, beállít egy `szam = 83` alapértelmezett értéket, és kiírja a hibaüzenetet. Ez megakadályozza a program összeomlását.
*   **Szomszédok keresése**: A `Feladat5` metódusban először megkeressük a megadott házszámú telek indexét (`keresett`). Ezután meghatározzuk az alsó (`also`) és felső (`felso`) szomszédok indexét. Fontos, hogy a házszámok 2-vel növekednek, így a szomszédok házszámai `szam - 2` és `szam + 2` lesznek. Külön kezeljük az 1-es és 2-es házszámokat, mert azoknak nincs alsó szomszédjuk az adott oldalon.
*   **Szín generálás**: A `Random` osztály segítségével generálunk egy véletlen karaktert ('A' és 'D' között).
*   **Szín érvényesítés**: Egy `while` ciklusban addig generálunk új véletlen színeket, amíg az nem különbözik az aktuális telek színétől (`telkek[keresett].Szin`), az alsó szomszéd színétől (`telkek[also].Szin`), és a felső szomszéd színétől (`telkek[felso].Szin`).
*   A `Program.cs` fájlban a `Console.WriteLine("Egy lehetséges festési szín: " + utca.Feladat5(szam));` sor hívja meg a metódust és írja ki a javasolt színt.

**Információs blokk:**

> **C# vs. C++: Bemenetkezelés, Hibakezelés és Véletlenszám generálás**
>
> *   **Bemenetkezelés**: A C# `Console.ReadLine()` és `int.Parse()` hasonló a C++ `std::cin` és `std::stoi()` párosához a felhasználói bemenet és konverzió kezelésében.
> *   **Hibakezelés**: A C# `try-catch` blokk a C++ `try-catch` blokkjának felel meg a kivételek kezelésére. A `throw new Exception(...)` a C++ `throw Exception(...)` megfelelője.
> *   **Véletlenszám generálás**: A C# `Random` osztály hasonló a C++ `<random>` könyvtárban található generátorokhoz. A `x.Next(min, max)` metódus egy véletlen számot generál a `min` (beleértve) és `max` (nem beleértve) tartományban.

---

## 6. Feladat: Utcakép generálása (`utcakep.txt`)

Az utolsó feladatban vizuálisan megjelenítjük az utca páratlan oldalát egy fájlban. Ez a `Utca` osztály `Feladat6` metódusával történik.

```csharp
// innentől új kód
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace fa2_KeritesFeladat
{
    internal class Kerites
    {
        private int oldal;
        private int szelesseg;
        private char szin;
        private int hazszam;
        public int Oldal { get { return oldal; } }
        public int Szelesseg { get { return szelesseg; } }
        public int Hazszam { get { return hazszam; } }
        public char Szin { get { return szin; } }

        public Kerites(int oldal, int szelesseg, char szin, int hazszam)
        {
            this.oldal = oldal;
            this.szelesseg = szelesseg;
            this.szin = szin;
            this.hazszam = hazszam;
        }
    }

    internal class Utca
    {
        private List<Kerites> telkek;

        public Kerites this[int index]
        {
            get { return telkek[index]; }
        }

        public int Db
        {
            get { return telkek.Count; }
        }

        public Utca(string fajlnev)
        {
            telkek = new List<Kerites>();
            int paros = 0, plan = -1;
            using (StreamReader olvas = new StreamReader(fajlnev))
            {
                while (!olvas.EndOfStream)
                {
                    string egysor = olvas.ReadLine();
                    string[] egysordarabok = egysor.Split(' ');
                    int oldal = Convert.ToInt32(egysordarabok[0]);
                    int szelesseg = Convert.ToInt32(egysordarabok[1]);
                    char szin = Convert.ToChar(egysordarabok[2]);
                    int hazszam;
                    if (oldal == 0)
                    {
                        paros += 2;
                        hazszam = paros;
                    }
                    else
                    {
                        plan += 2;
                        hazszam = plan;
                    }
                    telkek.Add(new Kerites(oldal, szelesseg, szin, hazszam));
                }
            }
        }

        public int Feladat4()
        {
            for (int i = 0; i < Db - 1; i++)
            {
                if ((telkek[i].Oldal + telkek[i + 1].Oldal == 2) &&
                    telkek[i].Szin == telkek[i + 1].Szin && telkek[i].Szin != '#' && telkek[i].Szin != ':')
                {
                    return telkek[i].Hazszam;
                }
            }
            return -1;
        }

        public char Feladat5(int szam)
        {
            int keresett = 0;
            for (int i = 0; i < Db; i++)
            {
                if (telkek[i].Hazszam == szam)
                {
                    Console.WriteLine("A kerítés színe / állapota: " + telkek[i].Szin);
                    keresett = i;
                }
            }
            int also = 0, felso = 0;
            if (szam == 1 || szam == 2) also = keresett;
            else
            {
                while (telkek[also].Hazszam != szam - 2)
                    also++;
            }
            while (felso < Db - 1 && telkek[felso].Hazszam != szam + 2)
                felso++;
            Random x = new Random();
            char uj = (char)x.Next(65, 69);
            while (telkek[also].Szin == uj || telkek[felso].Szin == uj || telkek[keresett].Szin == uj)
            {
                uj = (char)x.Next(65, 69);
            }
            return uj;
        }

        // 6. feladat: Írja ki az utcakép.txt fájlban a páratlan oldal utcaképét.
        public void Feladat6(string fajlnev)
        {
            // A StreamWriter segítségével írunk a megadott fájlba.
            using (StreamWriter iras = new StreamWriter(fajlnev))
            {
                // Első sor: A páratlan oldali kerítések színeinek vizualizációja.
                for (int i = 0; i < Db; i++) // Végigmegyünk az összes telken.
                {
                    if (telkek[i].Oldal == 1) // Csak a páratlan oldali telkeket dolgozzuk fel.
                    {
                        int j = 0;
                        // A kerítés színét ismételjük a szélességének megfelelően.
                        while (j < telkek[i].Szelesseg)
                        {
                            iras.Write(telkek[i].Szin); // Kiírjuk a színt.
                            j++;
                        }
                    }
                }
                iras.WriteLine(); // Új sorra ugrik a színek után.

                // Második sor: A házszámok kiírása a páratlan oldalon, igazítva a kerítésekhez.
                for (int i = 0; i < Db; i++)
                {
                    if (telkek[i].Oldal == 1) // Csak a páratlan oldali telkeket dolgozzuk fel.
                    {
                        int j = 1; // Szóközök számlálója a pozicionáláshoz.
                        iras.Write(telkek[i].Hazszam); // Kiírjuk a házszámot.

                        // A házszám számjegyeinek számától függően igazítjuk a szóközöket,
                        // hogy a házszám nagyjából a kerítés alatt legyen.
                        if (telkek[i].Hazszam > 9) j++;  // Kétjegyű házszám esetén több hely kell.
                        if (telkek[i].Hazszam > 99) j++; // Háromjegyű házszám esetén még több.

                        // Kitöltjük a maradék helyet szóközökkel a kerítés szélességéig.
                        while (j < telkek[i].Szelesseg)
                        {
                            iras.Write(" ");
                            j++;
                        }
                    }
                }
            }
        }
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            try
            {
                Utca utca = new Utca("kerites.txt");

                Console.WriteLine("2. feladat");
                Console.WriteLine("Az eladott telkek száma: {0}", utca.Db);

                Console.WriteLine("3. feladat");
                Console.Write("A ");
                if (utca[utca.Db - 1].Oldal == 0)
                {
                    Console.Write("páros");
                }
                else
                {
                    Console.Write("páratlan");
                }
                Console.WriteLine(" oldalon adták el az utolsó telket.");
                Console.WriteLine("Az utolsó telek házszáma: {0} ", utca[utca.Db - 1].Hazszam);

                Console.WriteLine("4. feladat");
                Console.WriteLine("A szomszédossal egyezik a kerítés színe: " + utca.Feladat4());

                Console.WriteLine("5. feladat");
                int szam;
                try
                {
                    Console.Write("Adjon meg egy házszámot! ");
                    szam = int.Parse(Console.ReadLine());
                    if (szam < 1 || szam > utca[utca.Db - 1].Hazszam)
                    {
                        throw new Exception("Hibás érték! A házszám értéke 83 lett. ");
                    }
                }
                catch (Exception e)
                {
                    szam = 83;
                    Console.WriteLine(e.Message);
                }
                Console.WriteLine("Egy lehetséges festési szín: " + utca.Feladat5(szam));

                // 6. feladat: Létrehozzuk és kiírjuk az utcakép vizualizációját az "utcakep.txt" fájlba.
                Console.WriteLine("6. feladat");
                utca.Feladat6("utcakep.txt"); // Meghívjuk a Feladat6 metódust.
                Console.WriteLine("Fájlba írás megtörtént"); // Visszajelzés a sikeres írásról.
            }
            catch (Exception e)
            {
                Console.WriteLine("Hiba!");
                Console.WriteLine(e.Message);
            }
            Console.ReadLine();
        }
    }
}
```

**Magyarázat:**
*   **`StreamWriter`**: Ez az osztály fájlokba való írásra szolgál. A `using` blokk itt is biztosítja az erőforrások helyes kezelését.
*   **Két ciklus**: A `Feladat6` metódus két fő részből áll:
    1.  **Színek kiírása**: Az első ciklus végigmegy az összes telken. Ha egy telek a páratlan oldalon van (`telkek[i].Oldal == 1`), akkor a kerítés színét (`telkek[i].Szin`) annyiszor írja ki, ahány a szélessége (`telkek[i].Szelesseg`).
    2.  **Házszámok kiírása**: A második ciklus hasonlóan jár el, de most a házszámokat (`telkek[i].Hazszam`) írja ki. A házszámok aláírásához a szélességhez igazítjuk a szükséges szóközöket. Figyelembe vesszük, hogy a házszámok lehetnek 1, 2 vagy 3 számjegyűek, és ennek megfelelően több vagy kevesebb szóközt írunk ki.
*   **`iras.Write()` és `iras.WriteLine()`**: Ezek a metódusok írnak a fájlba. A `Write` nem tesz új sort, míg a `WriteLine` igen.
*   A `Program.cs` fájlban a `utca.Feladat6("utcakep.txt");` hívás indítja el a fájlba írást, és a `Console.WriteLine("Fájlba írás megtörtént");` ad visszajelzést.

**Információs blokk:**

> **C# vs. C++: Fájl I/O és Szövegformázás**
>
> *   **Fájl I/O**: A C# `StreamWriter` hasonló a C++ `std::ofstream`-hez a fájlokba írásban. A `using` blokk itt is kulcsfontosságú az erőforrások kezelésében.
> *   **Szövegformázás**: A házszámok igazításához szükséges szóközök kiszámítása egy gyakori feladat a szöveges kimenetek formázásakor, mindkét nyelvben hasonló logikával oldható meg.

---

## Összegzés

Gratulálunk! Sikeresen végigvettük a C# kerítés feladatot, a `kerites.txt` fájl beolvasásától kezdve a vizuális utcakép generálásáig. Megismertük az alapvető C# fogalmakat, mint az osztályok, objektumok, fájlkezelés, hibakezelés és a kód lépésről lépésre történő felépítése.

```
<task_progress>
- [x] Check for célod.txt
- [x] Read content of célod.txt
- [x] Plan the task based on content
- [x] Obtain C# code from user (by listing files)
- [x] Identify C# files
- [x] Read Kerites.cs
- [x] Read Program.cs
- [x] Read Utca.cs
- [x] Generate Markdown content (incorporating user feedback on sample data)
- [x] Write Markdown content to a file
- [x] Read feladatok.txt to understand task breakdown
- [x] Re-architect tutorial.md based on 6 tasks
- [x] Present new plan to user
</task_progress>
</write_to_file>
