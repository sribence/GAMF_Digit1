# Digitális Technika 2. Gyakorlat

Ez a dokumentum a "Digitális Technika 2." kurzus második gyakorlatának anyagát foglalja össze. A fő témakörök a számrendszer-átváltások, a Boole-algebra alapjai, az áramutas és feszültség alapú logika.

---

## 1. Számrendszerek és Kódolás

A digitális rendszerek alapja a bináris számrendszer. Az adatok hatékony tárolására és feldolgozására különböző kódolási sémákat használunk.

### Számrendszer-átváltás

Gyakori feladat a tízes (decimális) számrendszerben megadott számok átváltása kettes (bináris), nyolcas (oktális) vagy tizenhatos (hexadecimális) számrendszerbe.

> [cite_start]**Példa: 389 átváltása** [cite: 5, 7]
> A `389`-es decimális számot ismételt osztással tudjuk átváltani bináris formátumra.
>
> ```
> 389 / 2 = 194, maradék 1
> 194 / 2 = 97,  maradék 0
> 97  / 2 = 48,  maradék 1
> 48  / 2 = 24,  maradék 0
> 24  / 2 = 12,  maradék 0
> 12  / 2 = 6,   maradék 0
> 6   / 2 = 3,   maradék 0
> 3   / 2 = 1,   maradék 1
> 1   / 2 = 0,   maradék 1
> ```
>
> A maradékokat alulról felfelé olvasva kapjuk meg a bináris számot: `110000101`.
>
> [cite_start]* **Bináris (8-as csoportosítás):** `110 000 101` → `605` (Oktális) [cite: 25]
> [cite_start]* **Bináris (16-os csoportosítás):** `0001 1000 0101` → `185` (Hexadecimális) [cite: 29]

### BCD (Binárisan Kódolt Decimális) Kódolás

[cite_start]A BCD kódolás lényege, hogy a decimális szám minden egyes számjegyét külön-külön, 4 biten kódoljuk binárisan[cite: 17].

> [cite_start]**Példa: 3725 BCD kódja** [cite: 18, 19]
>
> * **3** → `0011`
> * **7** → `0111`
> * **2** → `0010`
> * **5** → `0101`
>
> [cite_start]Tehát a `3725` BCD kódja: `0011 0111 0010 0101`[cite: 19].

---

## 2. Boole-algebra

A Boole-algebra a digitális áramkörök tervezésének matematikai alapja. Logikai műveleteket definiál, amelyeket logikai kapuk valósítanak meg.

### Alapvető Logikai Műveletek

| Művelet | Jelölés | Leírás | Igazságtábla (A, B → Q) |
| :--- | :---: | :--- | :---: |
| **Jelismétlő** | `Q = A` | [cite_start]A kimenet megegyezik a bemenettel. [cite: 32] | `0→0`, `1→1` |
| **Inverter (NOT)** | `Q = Ā` | [cite_start]A kimenet a bemenet negáltja (ellentettje). [cite: 43] | `0→1`, `1→0` |
| **VAGY (OR)** | `Q = A + B` | [cite_start]A kimenet akkor IGAZ, ha legalább az egyik bemenet IGAZ. [cite: 36] | `00→0`, `01→1`, `10→1`, `11→1` |
| **ÉS (AND)** | `Q = A * B` | [cite_start]A kimenet csak akkor IGAZ, ha minden bemenet IGAZ. [cite: 41] | `00→0`, `01→0`, `10→0`, `11→1` |

### Származtatott Logikai Műveletek

| Művelet | Jelölés | Leírás | Igazságtábla (A, B → Q) |
| :--- | :---: | :--- | :---: |
| **Invertált VAGY (NOR)** | `Q = A + B` | [cite_start]Az OR művelet kimenetének negáltja. [cite: 45] | `00→1`, `01→0`, `10→0`, `11→0` |
| **Invertált ÉS (NAND)** | `Q = A * B` | [cite_start]Az AND művelet kimenetének negáltja. [cite: 47] | `00→1`, `01→1`, `10→1`, `11→0` |
| **Antivalencia (XOR)** | `Q = A ⊕ B` | [cite_start]A kimenet akkor IGAZ, ha a bemenetek különbözőek. [cite: 52] | `00→0`, `01→1`, `10→1`, `11→0` |
| **Ekvivalencia (XNOR)** | `Q = A ⊕ B` | [cite_start]A kimenet akkor IGAZ, ha a bemenetek megegyeznek. [cite: 49] | `00→1`, `01→0`, `10→0`, `11→1` |


---

## 3. Minterm Alak

A minterm egy olyan szorzatkifejezés, amely egy logikai függvény összes változóját (vagy annak negáltját) pontosan egyszer tartalmazza. Egy logikai függvény felírható a mintermjeinek összegeként.

> **Példa:**
> Egy `Q` kimenetű logikai függvényt vizsgálva, ha `A=0`, `B=1` esetén `Q=1`, és `A=1`, `B=1` esetén `Q=1`, akkor a függvény:
> [cite_start]`Q = (Ā * B) + (A * B)` [cite: 78]
>
> Egyszerűsítve:
> `Q = B * (Ā + A)`
> `Q = B * 1`
> [cite_start]`Q = B` [cite: 89]

---

## 4. Logikai Áramkörök

A logikai műveleteket fizikailag áramkörökkel, úgynevezett logikai kapukkal valósítjuk meg.

### [cite_start]Áramutas Logika [cite: 62]

Ebben a megközelítésben a logikai értékeket kapcsolók állása reprezentálja. A zárt kapcsoló IGAZ, a nyitott HAMIS értéket jelent.

* **VAGY (OR):** Két párhuzamosan kapcsolt kapcsoló. [cite_start]Az áram akkor folyik, ha legalább az egyik zárva van. [cite: 65]
* **ÉS (AND):** Két sorba kapcsolt kapcsoló. [cite_start]Az áram csak akkor folyik, ha mindkettő zárva van. [cite: 66]

### [cite_start]Feszültség Logika [cite: 70]

[cite_start]A modern digitális rendszerek, mint a TTL (Transistor-Transistor Logic)[cite: 81], feszültségszintekkel kódolják a logikai értékeket.

> **INFO:** A TTL logika egy elterjedt digitális áramkör-család. [cite_start]Jellemzően 5V-os tápfeszültséget használ. [cite: 90]

A TTL rendszerben a feszültségszintek a következők:

| Logikai Szint | Feszültségtartomány | Leírás |
| :--- | :---: | :--- |
| **Magas (High)** | `2.5V` - `5V` | Az `IGAZ` (1) logikai értéket reprezentálja. |
| **Tiltott sáv** | `0.8V` - `2.5V` | Bizonytalan állapot, az áramkörök nem tudják egyértelműen értelmezni. [cite_start]Kerülendő. [cite: 93] |
| **Alacsony (Low)** | `0V` - `0.8V` | A `HAMIS` (0) logikai értéket reprezentálja. |
