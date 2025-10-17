# 1. Fejezet – Dinamikus listák használata C#-ban (`List<T>`)

Ebben a fejezetben megismerjük a C# egyik leggyakrabban használt adattípusát,  
a **generikus listát** (`List<T>`). Ez egy **dinamikus tömb**, amely képes automatikusan növekedni,  
amikor új elemeket adunk hozzá.

---

## 🧩 1.1. Alapötlet – Mi az a `List<T>`?

A `List<T>` (ejtsd: "List of T") egy **gyűjtemény**, ami több azonos típusú értéket képes tárolni.  
A `T` helyére a típus kerül: például `List<int>` egész számokat tárol.

Ellentétben a sima tömbökkel (`int[]`), a lista:
- **növekedhet** automatikusan (nem kell előre megadni a méretet),
- tud **elemeket törölni, beszúrni, rendezni**,
- és sok hasznos **metódust** kínál (pl. `.Add()`, `.Remove()`, `.Sort()` stb.).

---

## 🧩 1.2. A program kezdeti szerkezete

Hozzunk létre egy új konzolos projektet (**fa1_lista**) és írjuk be a következő kódot:

```csharp
using System;
using System.Collections.Generic; // A List használatához szükséges

namespace fa1_lista
{
    internal class Program
    {
        static void Main(string[] args)
        {
            // Egy üres egész számokat tartalmazó lista létrehozása
            List<int> lista = new List<int>(90); 
            
            // A lista elemeinek száma (Count) és a kapacitás (Capacity)
            Console.WriteLine($"db={lista.Count}  kapacitás={lista.Capacity}");
            
            Console.ReadLine();
        }
    }
}
```

> 💡 **Tipp:**  
> - A `Count` megmutatja, hány elem van *valójában* a listában.  
> - A `Capacity` azt mutatja, **mennyi hely van előre lefoglalva** a memóriában.  
> - Ha több elemet adunk hozzá, mint amennyi a kapacitás, a lista automatikusan **növeli a memóriaterületét**.

> ⚙️ **C++ párhuzam:**  
> Ez nagyon hasonlít a C++-os `std::vector<int>` típushoz, amely szintén dinamikusan nő, ha `push_back()`-kel új elemet adunk hozzá.

---

## 🧩 1.3. Elemek hozzáadása – véletlenszámokkal feltöltés

Most töltsük fel a listát 100 véletlen számmal 0 és 99 között.

```csharp
Random rnd = new Random(); // Véletlenszám-generátor
for (int i = 0; i < 100; i++)
{
    lista.Add(rnd.Next(100)); // Hozzáadunk egy új véletlen számot
}
```

A lista most automatikusan megnöveli a **kapacitását**,  
ha az meghaladja a kezdeti értéket (90).

---

> ⚠️ **Figyelem!**  
> A lista **kapacitása** nem mindig egyezik meg a **hosszával (Count)**.  
> Például:
> - 0 elem után: `Count = 0`, `Capacity = 90`  
> - 100 elem után: `Count = 100`, `Capacity = 180` (a rendszer automatikusan duplázza, ha elfogy a hely)

> 💭 **Memóriában:**  
> - A lista egy **belső tömböt** használ.  
> - Ha megtelik, egy **nagyobb tömböt** foglal, majd átmásolja az eddigi elemeket.  
> - Ezért nem érdemes minden egyes hozzáadás után kiírni a kapacitást, csak ha tanulmányozni szeretnénk a működését.

---

## 🧩 1.4. Lista tartalmának kiírása

Miután feltöltöttük a listát, kiírjuk az összes elemet egy `foreach` ciklussal:

```csharp
Console.WriteLine("A lista elemei:");
foreach (int elem in lista)
{
    Console.Write(elem + " ");
}
Console.WriteLine(); // új sor a végén
```

A program eddig ezt csinálja:

```csharp
using System;
using System.Collections.Generic;

namespace fa1_lista
{
    internal class Program
    {
        static void Main(string[] args)
        {
            List<int> lista = new List<int>(90);
            Console.WriteLine($"db={lista.Count}  kapacitás={lista.Capacity}");

            Random rnd = new Random();
            for (int i = 0; i < 100; i++)
            {
                lista.Add(rnd.Next(100));
            }

            Console.WriteLine("A lista elemei:");
            foreach (int elem in lista)
            {
                Console.Write(elem + " ");
            }
            Console.WriteLine();

            Console.ReadLine();
        }
    }
}
```

Kimenet (példa):

```
db=0  kapacitás=90
A lista elemei:
34 99 2 18 45 73 56 12 7 64 ... (összesen 100 szám)
```

---

## 🧩 1.5. Lista rendezése

A listák rendezésére a `.Sort()` metódus szolgál.  
Ez **növekvő sorrendbe** rendezi az elemeket.

```csharp
lista.Sort();
```

Ezután újra kiírjuk a lista elemeit:

```csharp
Console.WriteLine("A lista elemei rendezve:");
foreach (int elem in lista)
{
    Console.Write(elem + " ");
}
Console.WriteLine();
```

> 💡 **Megjegyzés:**  
> A `.Sort()` **in-place** rendezi a listát, tehát **nem készít új példányt**,  
> hanem az eredeti listát módosítja.

> 📘 **C++ megfelelő:**  
> ```cpp
> vector<int> lista = {3, 1, 4};
> sort(lista.begin(), lista.end());
> ```

---

## 🧩 1.6. Elem elérése index alapján

A listák **indexelhetők**, akárcsak a tömbök.

```csharp
Console.WriteLine($"lista[50]={lista[50]}");
```

Ez a parancs a lista 51. elemét írja ki (hiszen az index 0-tól indul).

---

## 🧩 1.7. A teljes kód (összefoglalva)

```csharp
using System;
using System.Collections.Generic;

namespace fa1_lista
{
    internal class Program
    {
        static void Main(string[] args)
        {
            // Lista létrehozása 90-es kezdeti kapacitással
            List<int> lista = new List<int>(90);
            Console.WriteLine($"db={lista.Count}  kapacitás={lista.Capacity}");

            // Lista feltöltése 100 véletlen számmal
            Random rnd = new Random();
            for (int i = 0; i < 100; i++)
            {
                lista.Add(rnd.Next(100));
            }

            // Lista elemeinek kiírása
            Console.WriteLine("A lista elemei:");
            foreach (int elem in lista)
            {
                Console.Write(elem + " ");
            }
            Console.WriteLine();

            // Rendezés
            lista.Sort();

            // Rendezett lista kiírása
            Console.WriteLine("A lista elemei rendezve:");
            foreach (int elem in lista)
            {
                Console.Write(elem + " ");
            }
            Console.WriteLine();

            // Egy konkrét elem kiírása
            Console.WriteLine($"lista[50]={lista[50]}");

            // Végső állapot (elemszám és kapacitás)
            Console.WriteLine($"db={lista.Count}  kapacitás={lista.Capacity}");
            Console.ReadLine();
        }
    }
}
```

---

## 🧠 Összefoglalás

A `List<T>` egy **rugalmas, dinamikus adatszerkezet**, amely:
- automatikusan kezeli a méretét (nem kell előre megadni),
- gyors hozzáférést biztosít index alapján,
- és egyszerűen rendezhető, kereshető, bővíthető.

> ⚙️ **Tipp a gyakorlatra:**  
> Ha tudod, hogy körülbelül mennyi adatot fogsz tárolni, érdemes előre beállítani a `Capacity`-t,  
> így elkerülheted a felesleges memóriaátmásolásokat.

> 📚 **Továbblépés:**  
> Következő témák lehetnek:
> - `.Remove()` és `.Insert()` metódusok használata  
> - `.Find()` és `.Contains()` keresés a listában  
> - komplex típusok listája (pl. `List<Versenyzo>`)

---

🎯 **Feladat gyakorlásra:**  
Próbáld ki, mi történik, ha:
1. Nem adsz meg kezdeti kapacitást a listának.  
2. A lista minden 10. elemét törlöd (`lista.RemoveAt(i)`).  
3. A `.Reverse()` metódussal megfordítod a listát.
