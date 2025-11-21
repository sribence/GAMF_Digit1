

# C\# Komplex Számok Feladat Oktatóanyag - OOP és Interfészek

Ez az oktatóanyag egy C\# konzolalkalmazás fejlesztését mutatja be, amely komplex számok matematikai modellezését végzi. A projekt célja nem csupán a számítások elvégzése, hanem a modern objektumorientált programozás (OOP) eszköztárának – öröklődés, polimorfizmus, operátor túlterhelés, interfészek – gyakorlati alkalmazása.

A tananyag során lépésről lépésre építjük fel a kódot, különös tekintettel a **C++ nyelvről érkezők** számára fontos különbségekre.

## Bevezetés

A projekt három fő komponensből áll:

1.  **`AdatXY`**: Egy absztrakt ősosztály, amely általános 2D koordinátákat kezel.
2.  **`Komplex`**: A származtatott osztály, amely megvalósítja a komplex számok logikáját (abszolút érték, síknegyedek, összeadás).
3.  **`Program`**: A vezérlő kód, amely fájlból olvas, adatokat dolgoz fel és statisztikát készít.

A tananyag során megismerkedünk az alábbi C\# koncepciókkal:

  * Absztrakt osztályok és `protected` láthatóság
  * Property-k (Tulajdonságok) és validáció
  * Konstruktor hívási lánc (`base`) és Copy Konstruktor
  * Operátor túlterhelés (`operator +`)
  * Indexelő (Indexer) használata
  * Interfész implementáció (`IComparable`) a rendezéshez

-----

## 1\. Feladat: Az alapok lefektetése (`AdatXY` ősosztály)

Első lépésként létrehozunk egy általános, absztrakt osztályt. Ez az osztály fogja tárolni az adatokat (x és y komponensek), de nem engedi, hogy közvetlenül példányosítsák, mivel a "működését" a leszármazottakra bízza.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace fa2_Komplex2
{
    /*
     Definiáljon egy AdatXY nevű osztályt, amely egy olyan adatot tárol, 
     amelynek két double típusú komponense van, például 2d vektor vagy egy komplex szám.
     */
    abstract internal class AdatXY
    {
        // Örökölhető x és y double változókban tárolja az adat komponenseit.
        // A 'protected' kulcsszó biztosítja, hogy a leszármazott osztályok (Komplex)
        // közvetlenül hozzáférjenek, de a külvilág (Program) ne lássa őket.
        protected double x, y;
        
        // Publikus tagjai:
        // Inicializáló konstruktor: A konstruktor paraméterként vegyen át két double értéket
        // és adja át az osztálytagoknak.
        protected AdatXY(double x, double y)
        {
            this.x = x;
            this.y = y;
        }

        // Absztrakt Abs() metódus: paraméterlistája üres, visszatérési értéke double.
        // Ez egy "ígéret": minden leszármazottnak kötelező megírnia, hogyan számolja a saját nagyságát.
        abstract public double Abs();
    }
}
```

**Magyarázat:**

  * [cite\_start]**`abstract class`**: Ez az osztály egy sablon[cite: 8]. Nem hozhatunk létre belőle objektumot (`new AdatXY()` hiba lenne), de származtathatunk belőle.
  * [cite\_start]**`protected` mezők**: Az enkapszuláció (bezárás) elvét követve az adatokat elrejtjük, de a leszármazottaknak engedélyt adunk a módosításra[cite: 5].
  * **`abstract method`**: Az `Abs()` metódusnak itt nincs törzse. Ez a **polimorfizmus** alapja: a rendszer tudja, hogy minden `AdatXY`-nak van abszolút értéke, de a kiszámítás módja attól függ, hogy Vektor vagy Komplex számról van szó.

**Információs blokk:**

> **C\# vs. C++: Absztrakt osztályok**
>
> C++-ban az absztrakt osztályt a **tiszta virtuális függvények** (pure virtual functions) jelölik:
>
> ```cpp
> class AdatXY {
> protected:
>     double x, y;
> public:
>     AdatXY(double x, double y) : x(x), y(y) {}
>     virtual double Abs() = 0; // C++ tiszta virtuális függvény
> };
> ```
>
> C\#-ban az `abstract` kulcsszó explicit módon jelöli mind az osztályt, mind a metódust, ami olvashatóbbá teszi a szándékot.

-----

## 2\. Feladat: A Komplex szám modellje (`Komplex` osztály I. rész)

Most létrehozzuk a `Komplex` osztályt, amely az `AdatXY`-ból származik. Először definiáljuk a szükséges adattagokat és az állapotokat leíró Enum típust.

```csharp
namespace fa2_Komplex2
{
    // Definiáljon a névtérbe egy Síknegyed enum típust.
    // Ez besorolja, hogy a komplex szám a koordináta-rendszer melyik részén van.
    enum Siknegyed
    {
        Valós, Imaginárius,
        JobbFelső, JobbAlsó, BalFelső, BalAlsó
    }

    // A Komplex osztály származik az AdatXY-ból (inheritance).
    internal class Komplex : AdatXY
    {
        // Privát adattagok a specifikus tulajdonságokhoz.
        private Siknegyed sikn;
        private double absz;

        // Property-k (Tulajdonságok):
        // Ellenőrzött hozzáférést biztosítanak a privát mezőkhöz.
        
        public Siknegyed Sikn
        {
            get { return sikn; }
            set { sikn = value; }
        }

        // Az absz esetén validációt végzünk: csak pozitív értéket engedünk beírni.
        public double Absz
        {
            get { return absz; }
            set { if (value > 0) absz = value; [cite_start]} // Validáció a setterben [cite: 16]
        }
        
        // ... (A konstruktorok és metódusok a következő lépésben jönnek)
    }
}
```

**Magyarázat:**

  * [cite\_start]**`enum Siknegyed`**: A `class`-on kívül, de a `namespace`-en belül definiáljuk, így az egész projektben használható [cite: 9-10]. Növeli a kód olvashatóságát (nem `0, 1, 2` számkódokat használunk).
  * **`Property` (Tulajdonság)**: A C\# egyik legerősebb nyelvi eleme. A `public double Absz` kívülről változónak tűnik, de valójában metóduspár (`get`, `set`). [cite\_start]Itt helyeztük el a logikát, hogy az abszolút érték ne lehessen negatív[cite: 16].

**Információs blokk:**

> **C\# vs. C++: Getter/Setter**
>
> C++-ban hagyományosan külön metódusokat írunk:
>
> ```cpp
> void setAbsz(double value) {
>     if (value > 0) absz = value;
> }
> ```
>
> C\#-ban a Property szintaxis (`set { ... }`) ezt egységesíti. Az `value` kulcsszó automatikusan tartalmazza az értékadás jobb oldalán álló adatot.

-----

## 3\. Feladat: Konstruktorok és Inicializálás

Egy objektum "születése" kritikus pont. Itt kell biztosítanunk, hogy az ősosztály is megkapja az adatait, és a származtatott osztály is kiszámolja a sajátjait.

```csharp
        // ... (Komplex osztály folytatása)

        // Inicializáló konstruktor:
        // 1. Átveszi az x és y értékeket.
        // 2. A ': base(x, y)' hívással továbbadja őket az AdatXY konstruktorának.
        // 3. Meghívja a belső metódusokat az állapotok beállításához.
        public Komplex(double x, double y) : base(x, y)
        {
            absz = Abs();             // Kiszámoljuk az abszolút értéket
            sikn = MelyikSiknegyed(); // Meghatározzuk a síknegyedet
        }

        // Copy-konstruktor (Másoló konstruktor):
        // Egy létező Komplex objektum adatait másolja át az új példányba.
        // Fontos: Az 'x' és 'y' eléréséhez a 'k.x' formát használjuk (az ősosztályban protected).
        public Komplex(Komplex k) : base(k.x, k.y)
        {
            absz = k.absz;
            sikn = k.sikn;
        }

        // Felüldefiniált Abs() metódus (az abstract megvalósítása):
        // Pitagorasz-tétel: gyök(x^2 + y^2)
        public override double Abs()
        {
            return Math.Sqrt(x * x + y * y);
        }

        // Segédmetódus a síknegyed meghatározására (logikai feltételek)
        private Siknegyed MelyikSiknegyed()
        {
            if (x == 0) return Siknegyed.Imaginárius;
            if (y == 0) return Siknegyed.Valós;
            if (x > 0 && y > 0) return Siknegyed.JobbFelső;
            if (y > 0 && x < 0) return Siknegyed.BalFelső;
            if (x > 0 && y < 0) return Siknegyed.JobbAlsó;
            return Siknegyed.BalAlsó;
        }
```

**Magyarázat:**

  * **`base(x, y)`**: Ez felel meg a C++ *initializer list* ősosztály hívásának. [cite\_start]Mivel az `AdatXY` mezői (`x, y`) nem privátok, hanem `protected`-ek, a `Komplex` osztály látja őket, de a helyes inicializálást az ősre bízzuk[cite: 17].
  * **Konstruktor Logika**: Figyeljük meg, hogy a konstruktor nem csak adatot tárol, hanem *számol* is (`Abs()`, `MelyikSiknegyed()`). [cite\_start]Így az objektum létrejötte után azonnal konzisztens állapotban van[cite: 18].
  * **Copy Konstruktor**: Mivel a `Komplex` referencia típus (`class`), ha csak simán átadnánk (`Komplex a = b`), mindkét változó ugyanarra a memóriahelyre mutatna. [cite\_start]A Copy konstruktorral létrehozunk egy *új* példányt, azonos adatokkal (Deep Copy)[cite: 19].

**Memória:** Objektum létrehozásakor (`new Komplex(...)`) a Heap-en foglalódik hely. A `x` és `y` az `AdatXY` részében, az `absz` és `sikn` a `Komplex` részében tárolódik, de egyetlen összefüggő memóriablokkban.

-----

## 4\. Feladat: OOP "Cukorka" – Operátorok és Indexelők

A C\# lehetővé teszi, hogy saját típusaink úgy viselkedjenek, mint a beépített típusok. Összeadhassuk őket `+` jellel, vagy tömbként érjük el az adataikat.

```csharp
        // ... (Komplex osztály folytatása)

        // Felüldefiniált ToString(): A konzolra írást formázza.
        // Formátum: "x + y*i abs: ..., típusa: ..."
        public override string ToString()
        {
            return string.Format($"{x} + {y}*i abs: {absz}, típusa: {sikn}");
        }

        // Indexelő (Indexer):
        // Lehetővé teszi az objektum adatainak elérését tömbszerűen: obj[0], obj[1]...
        public double this[int index]
        {
            get
            {
                switch (index)
                {
                    case 0: return x;
                    case 1: return y;
                    case 2: return absz;
                    default:
                        // Kivétel dobása, ha rossz az index (Hibakezelés)
                        throw new IndexOutOfRangeException("Nem jó az index…");
                }
            }
        }

        // Statikus operátor túlterhelés (+):
        // Két Komplex objektum összeadása matematikai szabályok szerint.
        // Fontos: Mindig 'public static' kell legyen.
        public static Komplex operator +(Komplex a, Komplex b)
        {
            // Új objektumot adunk vissza az összegekből
            return new Komplex(a.x + b.x, a.y + b.y);
        }
```

**Magyarázat:**

  * **`this[int index]`**: Ez az *Indexer*. Kívülről az objektum tömbnek látszik. [cite\_start]Hasznos, ha iterálni akarunk a belső adatokon anélkül, hogy ismernénk a Property neveket[cite: 24]. [cite\_start]A `throw` kulcsszóval jelezzük a futtató környezetnek, ha hiba történt (kivételkezelés)[cite: 25].
  * **`operator +`**: A C++-hoz hasonlóan túlterhelhető. [cite\_start]Lényeges különbség, hogy C\#-ban ez kötelezően `static` metódus, amely két példányt kap paraméterként és egy *új* példánnyal tér vissza[cite: 26].

**Információs blokk:**

> **C\# vs. C++: Operátorok**
>
> C++-ban az operátor lehet tagfüggvény (`member function`) vagy globális barát függvény (`friend`).
>
> ```cpp
> // C++ példa
> Komplex operator+(const Komplex& other) const { ... }
> ```
>
> C\#-ban ez mindig statikus osztálymetódus, ami tisztábbá teszi a szintaxist (szimmetrikus a két operandusra nézve).

-----

## 5\. Feladat: Rendezhetőség megvalósítása (`IComparable`)

Ahhoz, hogy a `Sort()` függvény működjön a listánkon, meg kell mondanunk a C\#-nak, mitől "nagyobb" egyik komplex szám a másiknál.

```csharp
    // Az osztály definícióját kiegészítjük az interfész implementációval:
    internal class Komplex : AdatXY, IComparable<Komplex>
    {
        // ... (Korábbi kódok)

        // Az IComparable<Komplex> interfész egyetlen metódusa.
        // Visszatérési értéke:
        // < 0: ha az aktuális példány kisebb
        // 0  : ha egyenlőek
        // > 0: ha az aktuális példány nagyobb
        public int CompareTo(Komplex k)
        {
            [cite_start]// A feladat szerint az abszolút érték dönt [cite: 27]
            return absz.CompareTo(k.absz); 
        }
    }
```

**Magyarázat:**

  * **Interfész (`interface`)**: Ez egy szerződés. Ha kiírjuk, hogy `IComparable<Komplex>`, akkor *kötelező* megírnunk a `CompareTo` metódust.
  * **Logika**: Mivel az `absz` egy `double` típus, annak már van beépített `CompareTo` metódusa, így delegálhatjuk neki a feladatot. Ez elegáns és hibamentes megoldás.

-----

## 6\. Feladat: A program "összerakása" és Fájlkezelés (`Program.cs`)

Végül a `Program` osztályban felhasználjuk az elkészült építőkockákat.

```csharp
using System;
using System.Collections.Generic;
using System.IO; // Fájlkezeléshez szükséges

namespace fa2_Komplex2
{
    internal class Program
    {
        static void Main(string[] args)
        {
            // Lista létrehozása a dinamikus beolvasáshoz
            List<Komplex> list = new List<Komplex>();
            try
            {
                // Fájl megnyitása olvasásra. A 'c:\\Munka\\adat.txt' elérési út.
                StreamReader sr = new StreamReader("c:\\Munka\\adat.txt");
                
                // Beolvasás a fájl végéig
                while (!sr.EndOfStream)
                {
                    string line = sr.ReadLine(); // "1.2 4.0"
                    string[] d = line.Split();   // ["1.2", "4.0"]
                    
                    // Konverzió és objektum létrehozás
                    double x = double.Parse(d[0]);
                    double y = double.Parse(d[1]);
                    Komplex c = new Komplex(x, y);
                    
                    list.Add(c); // Hozzáadás a listához
                }
                sr.Close(); // Erőforrás lezárása

                Console.WriteLine("a komplex számok listája");
                foreach (var l in list)
                {
                    Console.WriteLine(l); // Itt hívódik meg a ToString() automatikusan
                }

                [cite_start]// Lista átmásolása Tömbbe (Copy konstruktorral) [cite: 36-37]
                Komplex[] ctmb = new Komplex[list.Count];
                for (int i = 0; i < list.Count; i++)
                {
                    ctmb[i] = new Komplex(list[i]); // Mélymásolás
                }

                // Rendezés
                // Az Array.Sort automatikusan használja a CompareTo metódusunkat.
                Array.Sort(ctmb);
                
                Console.WriteLine("a komplex számok tömbje rendezve");
                foreach (var c in ctmb)
                {
                    Console.WriteLine(c);
                }

                [cite_start]// Operátor és Indexelő tesztelése [cite: 39-40]
                Console.WriteLine("A legkisebb és legnagyobb számok összege ");
                Komplex legkisebb = ctmb[0];
                Komplex legnagyobb = ctmb[ctmb.Length - 1];
                
                // Itt hívódik meg az 'operator +'
                Console.WriteLine(legkisebb + legnagyobb);

                Console.WriteLine("indexelő tesztelése");
                // A legnagyobb szám komponenseinek kiírása ciklussal (obj[i])
                for (int i = 0; i < 3; i++)
                {
                    Console.WriteLine(legnagyobb[i]); 
                }
            }
            catch (FileNotFoundException e)
            {
                Console.WriteLine(e.Message); // Specifikus hiba: nincs meg a fájl
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message); // Általános hiba
            }
            Console.ReadLine(); // Várakozás, hogy ne záródjon be azonnal az ablak
        }
    }
}
```

**Magyarázat:**

  * **`List<T>` vs. Tömb**: A fájlbeolvasásnál `List`-et használunk, mert nem tudjuk előre, hány sor van. Utána tömbbe másoljuk az adatokat a feladat kérésére, bemutatva a **Copy konstruktor** hasznosságát (így a tömb elemei független másolatok a listától).
  * **Kivételkezelés (`try-catch`)**: A fájlműveletek veszélyesek (hiányzó fájl, zárolt fájl). A `try` blokkba tesszük a kritikus kódot, a `catch` blokkban pedig elegánsan kezeljük a hibát ahelyett, hogy a program összeomlana ("elszállna").

**Információs blokk:**

> **C\# vs. C++: Memóriakezelés és Fájlok**
>
>   * **Fájlkezelés**: A `StreamReader` hasonló a C++ `std::ifstream`-hez.
>   * **Garbage Collection**: C\#-ban nem kell `delete`-et hívnunk a `new Komplex()` után. A .NET Garbage Collector (GC) automatikusan felszabadítja a memóriát, amikor az objektumra már nincs szükség.

-----

## Összegzés

Ebben a leckében sikeresen modelleztük a komplex számokat C\#-ban. Láthattuk, hogyan teszi az **öröklődés** (`AdatXY` -\> `Komplex`) a kódot újrafelhasználhatóvá, hogyan garantálják a **Property-k** az adatok érvényességét, és hogyan teszik az **operátorok** és **indexelők** kényelmessé a használatot. A programunk képes fájlt olvasni, adatokat rendezni és matematikai műveleteket végezni, mindezt robusztus hibakezeléssel ellátva.
