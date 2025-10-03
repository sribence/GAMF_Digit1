# C# Tananyag — Alapok és Gyakorlati Példák (részletes, kezdőknek)

Az anyag célja, hogy lépésről lépésre, **kezdő szemszögből** magyarázza el a megadott C# program részeit.  
Minden fejezet tartalmaz:

- **Részletes magyarázatot** (kezdőknek, miért csináljuk, mire figyelj)  
- **Információs blokkot** (C# vs C++ összehasonlítás blockquote formában)  
- **Teljes kódot** (minden fejezetnél a korábbi részeket is tartalmazza — tehát a teljes program), a kódban soronként kommentekkel és mindig ott egy `// innentől új kód` megjegyzéssel, ahol a fejezet újdonsága kezdődik.

---

## 1. fejezet — Az első program: "Helló Világ!"

### Részletes magyarázat (kezdőknek)
Amikor programozni kezdünk, az első program gyakran egy olyan rövid kód, amely kiír egy egyszerű üzenetet a képernyőre. Ennek a gyakorlata több célt szolgál:

1. **Megmutatja, hogy a fejlesztői környezet rendben van** — le tudod fordítani és futtatni a programot.
2. **Megtanítja az alapvető programstruktúrát** — láthatod, hogy egy C# programnak van `namespace`-e, osztálya, és `Main` metódusa (a program belépési pontja).
3. **Megszokod a konzol kiíratást** — később hibakereséskor vagy a felhasználónak való visszajelzésnél gyakran írunk ki információt.
4. **Bizonyíték arra, hogy a változtatások tényleg látszanak** — minden további fejlesztésnél ellenőrizheted, működik-e a kinyomtatás.

Fontos dolgok, amire figyelj:
- `using System;` beemeli a .NET `System` névteret, amely tartalmazza a `Console` osztályt.
- A `Main` metódus a program kezdőpontja — innen indul a kód végrehajtása.
- `Console.WriteLine()` kiír egy sort és automatikusan újsort tesz a végére; `Console.Write()` csak kiír, de nem tesz új sort.
- Ha konzolos alkalmazást Visual Studio-ból futtatsz "F5"-el, a konzol ablak bezárulhat a végén — ezért gyakran hagynak bent `Console.ReadLine()`-t vagy `Console.ReadKey()`-t a program végén, hogy láthasd az eredményt.

> **C# vs C++**
> - C#: `Console.WriteLine("...")` — beépített egyszerű konzolkiírás.  
> - C++: `std::cout << "..." << std::endl;` — hasonló cél, de más névtér, más szintaxis.

### Kód (teljes program)  
```csharp
using System; // beimportáljuk a System névteret (Console osztály innen érkezik)

namespace fa1
{
    internal class Program
    {
        static void Main(string[] args) // a program belépési pontja
        {
            Console.WriteLine("Helló Világ!"); // kiír egy sort: Helló Világ!
            // Figyeld meg: a kimenet után új sor kezdődik

            Console.ReadLine(); // várakozás: így látod a konzol kimenetét futtatás után
            // Ha nem használod, egyes környezetek automatikusan bezárják a konzolt a végén
        }
    }
}
```

---

## 2. fejezet — Felhasználói bemenet: név bekérése

### Részletes magyarázat (kezdőknek)
Most megtanuljuk, hogyan kérjünk be adatot a felhasználótól. Egy program akkor lesz interaktív, ha nem csak statikus üzeneteket tol a képernyőre, hanem képes fogadni az ember bemenetét. Ennek alapjai:

1. **Prompt (felszólítás)**: először jelezzük a felhasználónak, hogy mit várunk el — pl. `Console.Write("Adja meg a nevét: ")`. A `Write` azért jó, mert a kurzor ugyanazon a soron marad, így a felhasználó azonnal a kérdés után írhat.
2. **Beolvasás**: `Console.ReadLine()` beolvassa a felhasználó által begépelt teljes sort (amikor Entert nyom). Ez mindig **string**-et ad vissza.
3. **Változóban tárolás**: az olvasott értéket egy változóba mentjük (`string nev = Console.ReadLine();`), hogy később felhasználhassuk.
4. **Visszajelzés**: kiírjuk a felhasználónak, amit beírt — ez megerősítés és szemléltetés.

Mire figyelj:
- A `ReadLine()` mindig string, még akkor is, ha számszerű karaktereket írnak be. Később kell konvertálni, ha számként akarod kezelni.
- Lehet, hogy a felhasználó semmit nem ír (üres string). Érdemes valós programban ellenőrizni (pl. `if (string.IsNullOrWhiteSpace(nev)) { ... }`), de itt csak bemutatjuk az alapműveletet.
- Gyakorlás: próbáld ki beírni nevet, szóközöket is — `ReadLine()` megőrzi a szóközöket.

> **C# vs C++**
> - C#: `Console.ReadLine()` — beolvassa a teljes sort stringként.  
> - C++: `std::cin >> nev;` csak az első szóig olvas, szóköznál megáll — ha teljes sort akarsz, `std::getline(std::cin, nev);` a megoldás.

### Kód (teljes program — tartalmazza az előző fejezetet is)  
```csharp
using System; // System névtér: Console stb.

namespace fa1
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Helló Világ!"); // alap kiírás

            // innentől új kód
            Console.Write("Adja meg a nevét: "); // kérdést ír ki, kurzor a sor végén marad
            string nev = Console.ReadLine();     // beolvassa a felhasználó által begépelt sort és eltárolja "nev"-ben
            Console.WriteLine("Helló {0}!", nev); // visszaírja a nevet (helyettesítő formátummal)
            // Figyeld meg: ha a felhasználó 'János'-t ír be, a kimenet: "Helló János!"

            Console.ReadLine(); // várás a program végén, hogy lásd az eredményt
        }
    }
}
```

---

## 3. fejezet — Számok bekérése és típuskonverzió

### Részletes magyarázat (kezdőknek)
A `ReadLine()` mindig stringet ad, de sok esetben számokra van szükségünk (összeadás, ciklusok száma, stb.). A stringet **át kell alakítani** számokká. Két alapvető dologra figyelj:

1. **Konverzió módjai**:
   - `int.Parse(string)` — ha a string pontosan egy egész számot tartalmaz, akkor visszakapjuk az `int` értéket. Ha hibás a formátum (pl. betűk vagy üres string), **kivétel (exception)** keletkezik.
   - `double.Parse(string)` — lebegőpontos számokra (tört számok), hasonló viselkedéssel.

2. **Mi történik, ha a felhasználó hibázik?**
   - `Parse` **kihajítja** a `FormatException`-t, ha nem számot ad meg. A valós alkalmazásokban ezért biztonságosabb a `TryParse`, amely visszaad egy `bool`-t és nem dob kivételt:
     - `int.TryParse(beolvasott, out int eredmeny)` — ha sikeres, `eredmeny` a szám; ha nem, a metódus `false`-t ad, így kezelni tudod a hibát.
   - Itt a tananyagi kód egyszerűsítve `Parse`-t használ — de a magyarázatnál figyelmeztetünk a hibakezelésre.

3. **Locale / tizedesjel**:
   - A `double.Parse` a rendszer kultúrájától függően vár `.` vagy `,` tizedesjelet — pl. magyar Windows általában `,`-t vár. Ezt figyelembe kell venni, de egyszerű oktatási példa miatt most nem változtatjuk a kultúrát.

Mire figyelj:
- Próbálj meg nem számot beírni — a program hibát dob. Ez segít megérteni, miért fontos a `TryParse` a robosztus szoftverekben.
- Példa: ha beírod `5` és `3.14` (vagy `3,14` a rendszered szerint), akkor a program kiírja a megadott számokat.

> **C# vs C++**
> - C#: string -> szám `int.Parse`, `double.Parse` (kivételt dob hibás formátumnál).  
> - C++: `std::cin >> iszam;` közvetlenül a beolvasáskor konvertál, de ha hibás input érkezik, a stream hibajelzést ad és kezelni kell (`cin.fail()`), vagy olvashatsz stringet és konvertálhatsz manuálisan.

### Kód (teljes program — tartalmazza az előző fejezeteket is)  
```csharp
using System; // System: Console, stb.

namespace fa1
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Helló Világ!"); // bevezető kiírás

            Console.Write("Adja meg a nevét: "); // név bekérése
            string nev = Console.ReadLine();     // név eltárolása
            Console.WriteLine("Helló {0}!", nev); // visszaköszönés

            // innentől új kód
            Console.Write("Adjon meg egy egész számot: "); // kérdés a felhasználónak
            int iszam = int.Parse(Console.ReadLine());    // a beolvasott stringet egész számmá alakítjuk
            // FIGYELJ: ha a felhasználó nem egész számot ír, a Parse kivételt dob. Tanulóknak: később nézd meg az int.TryParse-t!

            Console.Write("Adjon meg egy nem egész számot: "); // kérdés a tört számhoz
            double dszam = double.Parse(Console.ReadLine());  // string -> double konverzió
            // FIGYELJ: tizedesjel kultúrfüggő lehet (',' vagy '.'), attól függően, milyen a rendszered beállítása

            Console.WriteLine($"{nev} számai: {iszam} {dszam}"); // kiírja a beolvasott számokat és a nevet

            Console.ReadLine(); // várakozás, hogy lásd az eredményt
        }
    }
}
```

---

## 4. fejezet — Osztás és maradék (miért vizsgáljuk ezt, mire figyelj)

### Részletes magyarázat (kezdőknek)
Most megnézzük az osztás különböző viselkedését, mert ez az a pont, ahol sok kezdő elbizonytalanodik. Az oka az eltérésnek a **típusok** és a **típuskonverzió**:

1. **Egész osztás (`int / int`)**  
   - Ha két egész típusú értéket osztasz (`10 / 3`), a nyelv *egész osztást* végez: a tört rész eldobódik (truncation), az eredmény egész lesz. Tehát `10 / 3` = `3`.  
   - Sok kezdőt meglep, mert matematikailag `10/3` ~ `3,333...`, de a program egész osztásnál csak az egész részt tartja meg.

2. **Maradék (`%`)**  
   - A `%` operátor a **maradékot** adja vissza: `10 % 3` = `1`, mert 3 * 3 = 9, marad 1.  
   - Hasznos, ha pl. páros/páratlan vizsgálatot végzel (`x % 2`), vagy ciklikus indexelésnél.

3. **Lebegőpontos osztás (`double / int` vagy `double / double`)**  
   - Ha legalább az egyik operandus `double` (vagy `float`), akkor lebegőpontos osztást kapsz: `(double)10 / 3` = `3.3333333333333335` (a számítás dupla pontossággal).  
   - Ha csak a végső megjelenítésnél szeretnél kerekítést, használhatod a formázást: `{érték:N2}` — ez 2 tizedesjegyre kerekít (pl. `3.33`).

4. **Légy óvatos: osztás nulla értékkel**  
   - `int` osztásnál: `10 / 0` kivételt dob (`DivideByZeroException`).  
   - `double` osztásnál: `10.0 / 0.0` végeredmény `Infinity` (nem dob kivételt), de ez függ a környezettől; mindenesetre kezelni kell a nullával való osztást.

5. **Miért nézzük meg ezt most?**  
   - Mert a számítások helyessége attól függ, hogy **milyen típusú adatokkal dolgozol**. Ha véletlenül egész típusokat használsz, amikor tört eredményre van szükséged, a logika hibás lesz. Sok hiba és furcsa viselkedés ebből ered.

Mire figyelj teszteléskor:
- Írj ki `10 / 3`, `10 % 3`, `(double)10 / 3`, és formázd `N2`-vel is — nézd meg a különbséget.
- Próbáld ki, mit csinál a program, ha `iszam` értéke `0` — különösen figyeld, hogy nem az egész osztás miatt lesz gond, mert itt például mi nem osztunk `iszam`-mal, de a tudatosság fontos.

> **C# vs C++**
> - Mindkét nyelvben a fenti szabályok hasonlóan érvényesek: egész osztás egész eredményt ad, `%` maradékot ad, és ha lebegőpontos osztást akarsz, legalább az egyik operandust konvertáld lebegőpontra.  
> - Típuskonverziók szintaxisa hasonló, de C++-ban van a `static_cast<double>(10)` formája is.

### Kód (teljes program — tartalmazza az előző fejezeteket is)  
```csharp
using System; // System névtér

namespace fa1
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Helló Világ!"); // bemutató kiírás

            Console.Write("Adja meg a nevét: "); // név bekérése
            string nev = Console.ReadLine();     // név mentése
            Console.WriteLine("Helló {0}!", nev); // visszaköszönés

            Console.Write("Adjon meg egy egész számot: "); // egész szám bekérése
            int iszam = int.Parse(Console.ReadLine());    // string -> int konverzió (Parse dobhat kivételt hibás inputnál)

            Console.Write("Adjon meg egy nem egész számot: "); // tört szám bekérése
            double dszam = double.Parse(Console.ReadLine());  // string -> double konverzió

            Console.WriteLine($"{nev} számai: {iszam} {dszam}"); // kiírás

            // innentől új kód
            Console.WriteLine($"10 / 3 = {10 / 3}  egész osztás"); // int / int -> egész eredmény (3)
            // Figyeld meg: mivel mindkét operandus egész, a tört rész elvész (10/3 => 3)

            Console.WriteLine($"10 % 3 = {10 % 3}  maradék"); // maradék operátor (1)
            // Figyeld meg: ez hasznos pl. páros/páratlanság ellenőrzésre (x % 2 == 0 -> páros)

            Console.WriteLine($"10 / 3 = {(double)10 / 3}  hányados"); // explicit cast -> lebegőpontos osztás
            // Figyeld meg: (double)10 konvertálja a 10-et dupla pontosságú lebegőpontra,
            // így a / művelet lebegőpontos osztás lesz és a pontosabb tört eredményt kapjuk (3.3333333333333335)

            Console.WriteLine($"10 / 3 = {(double)10 / 3:N2}  hányados 2 tizedesjegyre kerekítve"); // formázás N2
            // Figyeld meg: N2 formátum 2 tizedesjegyre kerekít. Kimenet: 3,33 (vagy 3.33 a kultúrától függően)

            Console.ReadLine(); // vár a futtatás végén
        }
    }
}
```

---

## 5. fejezet — Piramis rajzolása for-ciklussal (ciklusok, ágyazott ciklusok, formázás részletesen)

### Részletes magyarázat (kezdőknek)
Ebben a fejezetben olyan feladatot oldunk meg, ami szemlélteti a ciklusok használatát és azt, hogyan lehet egymásba ágyazott ciklusokkal „képet” vagy mintát készíteni a konzolra. A feladat: az `iszam` értékéig (például 5-ig) rajzoljunk egy piramist:

Példa `iszam = 5` esetén a várt vizuális kimenet (egyszerűsítve):
```
1
2 2
3 3 3
4 4 4 4
5 5 5 5 5
```

**Mi történik a kódban (logika lépésről lépésre):**

1. **Külső ciklus — sorok:**  
   `for (int i = 1; i <= iszam; i++)` — ez azt mondja, hogy `i` lesz az aktuális sor száma (és az a szám is, amit kiírunk). Az első sor `i=1`, az utolsó `i=iszam`.

2. **Első belső ciklus — bal oldali szóközök:**  
   Ahhoz, hogy a piramis középre kerüljön, a sor elejére `iszam - i` darab szóközt kell írni. Ez az, ami „feltolja” a kisebb sorokat középre.

3. **Második belső ciklus — számok írása:**  
   Ekkor `i` alkalommal kiírjuk az `i` számot, szóközökkel elválasztva: `Console.Write(i + " ");`. Így a 3. sor három darab `3`-at tartalmaz.

4. **Formázási finomságok (egyjegyű vs többjegyű számok):**  
   A kód egyszerű módszerrel kompenzálja az egyjegyű számok miatti eltolódást (`if (i < 10) Console.Write(" ");`). Ez a trükk két szóközt illeszt oda, ahol kell — nem tökéletes megoldás minden esetre (pl. 10+ értékeknél más, rugalmasabb megoldás kellene), de egyszerűen szemlélteti a problémát: **a számok szélessége befolyásolja az igazítást**.

5. **Miért jó ez a gyakorlat?**  
   - Gyakoroljuk az ágyazott ciklusokat.  
   - Megértjük, hogyan áll össze a vizuális alakzat soronként.  
   - Rávilágít a formázás (szóközök, számjegyek szélessége) fontosságára.

Mire figyelj teszteléskor:
- Próbáld ki különböző `iszam` értékekkel (1, 3, 5, 9, 12). Figyeld meg, hogy 10 vagy nagyobb számnál a jelenlegi egyszerű igazítás már nem lesz tökéletes.
- Nagy `iszam` érték esetén számolj a konzol szélességével — a sorok túl hosszúak lehetnek.

> **C# vs C++**
> - A `for` szerkezet azonos mindkét nyelvben, a `Console.Write`/`Console.WriteLine` C#-ban helyettesítője az `std::cout`-nak C++-ban.  
> - Az alaplogika (szóközök, ismétlések) teljesen ugyanaz.

### Kód (teljes program — tartalmazza az előző részeket is; a piramis résznél van a `// innentől új kód` komment)  
```csharp
using System; // System névtér: Console, stb.

namespace fa1
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Helló Világ!"); // alap kiírás

            Console.Write("Adja meg a nevét: "); // név bekérése
            string nev = Console.ReadLine();     // név tárolása
            Console.WriteLine("Helló {0}!", nev); // visszaköszönés

            Console.Write("Adjon meg egy egész számot: "); // egész szám bekérése
            int iszam = int.Parse(Console.ReadLine());    // Parse: string -> int (figyelj a hibakezelésre)

            Console.Write("Adjon meg egy nem egész számot: "); // tört szám bekérése
            double dszam = double.Parse(Console.ReadLine());  // string -> double (kulturális tizedesjel figyelem)

            Console.WriteLine($"{nev} számai: {iszam} {dszam}"); // kiírjuk a beolvasott értékeket

            Console.WriteLine($"10 / 3 = {10 / 3}  egész osztás"); // int / int -> 3 (tört rész eldobva)
            Console.WriteLine($"10 % 3 = {10 % 3}  maradék");     // maradék: 1
            Console.WriteLine($"10 / 3 = {(double)10 / 3}  hányados"); // lebegőpontos osztás: 3.3333333333333335
            Console.WriteLine($"10 / 3 = {(double)10 / 3:N2}  hányados 2 tizedesjegyre kerekítve"); // 3.33

            // innentől új kód
            // Piramis kiíratása: a külső ciklus határozza meg a sorok számát (1..iszam)
            for (int i = 1; i <= iszam; i++) // i: aktuális sor száma, és az a szám, amit kiírunk
            {
                // sor elejére szóközök (aszimmetria beállítása a jobb megjelenésért)
                for (int j = 1; j <= iszam - i; j++)
                {
                    Console.Write(" "); // balra pakoljuk a szóközöket, hogy középre igazítsuk a számokat
                }

                // egyszerű igazítás: egyjegyű számoknál eggyel több szóközt teszünk, hogy ne "összecsússzanak"
                if (i < 10) Console.Write(" "); // kis trükk az esztétikusabb megjelenésért

                // i-szer kiírni az i számot, szóközzel elválasztva
                for (int k = 1; k <= i; k++)
                {
                    Console.Write(i + " "); // pl. ha i==3, akkor "3 " lesz többször
                    if (i < 10) Console.Write(" "); // további szóköz, hogy az egyjegyű számok vizuálisan egyformábbak legyenek
                }

                Console.WriteLine(); // sor végét jelzi és átlépünk a következő sorra
            }

            // Használati javaslat:
            // - Próbáld ki iszam = 3, 5, 9 értékekkel és nézd meg a kimenetet.
            // - Ha 10 vagy annál nagyobb számot adsz meg, a jelenlegi egyszerű "if (i < 10)" trükk már nem elég:
            //   egy jobb megoldás a formázott stringek vagy fixed-width (pl. {i,3}) használata lenne a helyes igazításhoz.

            Console.ReadLine(); // vár a program végén
        }
    }
}
```

---

# Rövid összefoglaló (miért fontosak ezek az alapok)
- A **kiírás** és **beolvasás** a legelső eszközeid a program és az ember közti kommunikációhoz.  
- A **típusok és típuskonverziók** (string ↔ szám) ismerete nélkül könnyen hibázol számításoknál.  
- A **ciklusok** és **ágyazott ciklusok** a minták és algoritmusok alapjai — a piramis példa jó belépő a gondolkozás elsajátításához.  
- Mindig ellenőrizd az inputot (TryParse), és figyeld a **típusok közötti különbségeket** (egész vs lebegőpontos osztás).

---
