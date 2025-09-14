# Digitális Technika Alapjai

Ez a dokumentum bemutatja a digitális technika legfontosabb alapfogalmait, különös hangsúlyt fektetve a számábrázolásokra, az átváltásokra és az adattárolás kontextusára.

---

## 1. Alapvető Fogalmak

A digitális rendszerek diszkrét (nem folytonos) jeleket dolgoznak fel. A modern számítógépek esetében ez a diszkrét jel két állapotot vehet fel: **0** (alacsony feszültségszint) és **1** (magas feszültségszint).

* **Bit (Binary Digit):** A digitális információ legkisebb egysége. Értéke **0** vagy **1** lehet.
* **Bájt (Byte):** 8 bitből álló adategység. Ez a legalapvetőbb címezhető memóriarekesz a legtöbb számítógép-architektúrában. Egy bájt 2^8 = **256** különböző értéket tud felvenni (0-tól 255-ig).
* **Gépi szó (Word):** A processzor (CPU) által egyszerre feldolgozható bitek száma. A modern gépek általában 32 vagy 64 bites szavakkal dolgoznak, ami jelentősen befolyásolja a teljesítményüket és a megcímezhető memória méretét.
* **Logikai kapuk:** A digitális áramkörök építőelemei (pl. ÉS, VAGY, NEM), amelyek a Boole-algebra szabályai szerint végeznek műveleteket a bemeneti biteken.

---

## 2. Számábrázolások és Jelentőségük

A számítógépek a **kettes (bináris)** számrendszert használják, mivel a két állapot (0 és 1) könnyen reprezentálható elektronikusan. Az emberek számára azonban a hosszú bináris számsorok nehezen olvashatók. Ezért használunk más, könnyebben kezelhető számrendszereket, mint a **tizenhatos (hexadecimális)** és a **nyolcas (oktális)**, amelyek egyszerűen átválthatók binárisra.

### Számrendszerek Áttekintése

| Számrendszer | Alapszám | Használt számjegyek | Példa |
| :--- | :---: | :--- | :--- |
| **Decimális** | 10 | `0, 1, 2, 3, 4, 5, 6, 7, 8, 9` | `157₁₀` |
| **Bináris** | 2 | `0, 1` | `10011101₂` |
| **Oktális** | 8 | `0, 1, 2, 3, 4, 5, 6, 7` | `235₈` |
| **Hexadecimális** | 16 | `0-9, A, B, C, D, E, F` | `9D₁₆` |

> **Megjegyzés:** Az A-F betűk a hexadecimális rendszerben a 10-15 értékeket képviselik.

---

## 3. Átváltások a Számrendszerek Között

Az átváltások megértése kulcsfontosságú a digitális rendszerek működésének megértéséhez.

### Átváltás 10-es számrendszerből

#### Decimális → Bináris
**Módszer:** Az ismételt osztás módszere. Osszuk a számot 2-vel, és írjuk fel a maradékot. Az eljárást addig folytatjuk, amíg a hányados 0 nem lesz. A bináris számot a maradékok fordított sorrendben történő felírásával kapjuk meg.

**Példa: 157₁₀ átváltása binárisra**
* 157 / 2 = 78, maradék **1**
* 78 / 2 = 39, maradék **0**
* 39 / 2 = 19, maradék **1**
* 19 / 2 = 9, maradék **1**
* 9 / 2 = 4, maradék **1**
* 4 / 2 = 2, maradék **0**
* 2 / 2 = 1, maradék **0**
* 1 / 2 = 0, maradék **1**

A maradékokat alulról felfelé olvasva: **`10011101₂`**.

#### Decimális → Hexadecimális
**Módszer:** Ugyanaz, mint a binárisnál, csak 16-tal osztunk. A 10-15 közötti maradékokat A-F betűkkel helyettesítjük.

**Példa: 157₁₀ átváltása hexadecimálisra**
* 157 / 16 = 9, maradék **13** (ami **D**)
* 9 / 16 = 0, maradék **9**

A maradékokat alulról felfelé olvasva: **`9D₁₆`**.

### Átváltás 10-es számrendszerbe

**Módszer:** A helyiértékek módszere. Minden számjegyet megszorzunk a számrendszer alapjának megfelelő hatványával, majd ezeket összeadjuk.

#### Bináris → Decimális
**Példa: `10011101₂` átváltása decimálisra**
`1*2⁷ + 0*2⁶ + 0*2⁵ + 1*2⁴ + 1*2³ + 1*2² + 0*2¹ + 1*2⁰`
`128 + 0 + 0 + 16 + 8 + 4 + 0 + 1 = **157₁₀**`

#### Hexadecimális → Decimális
**Példa: `9D₁₆` átváltása decimálisra**
`9 * 16¹ + D * 16⁰`
`9 * 16 + 13 * 1 = 144 + 13 = **157₁₀**`

### Gyors átváltások (Bináris ↔ Hexadecimális/Oktális)

Ezek az átváltások különösen hasznosak, mivel a 16 és a 8 a 2 hatványai.

#### Bináris ↔ Hexadecimális
**Módszer:** A bináris számot jobbról balra haladva **4-es csoportokra** bontjuk, és minden csoportot átírunk egy hexadecimális számjeggyé.

**Példa: `10011101₂` → Hexadecimális**
1.  Csoportosítás: `1001 1101`
2.  Átváltás:
    * `1001₂` = 8 + 1 = `9₁₆`
    * `1101₂` = 8 + 4 + 1 = 13 = `D₁₆`
3.  Eredmény: **`9D₁₆`**

**Példa: `9D₁₆` → Bináris**
Minden hexadecimális számjegyet átírunk 4 bites bináris számmá.
* `9₁₆` = `1001₂`
* `D₁₆` = `1101₂`
Eredmény: **`10011101₂`**

#### Bináris ↔ Oktális
**Módszer:** Ugyanaz, mint a hexadecimálisnál, de itt **3-as csoportokra** bontunk.

**Példa: `10011101₂` → Oktális**
1.  Csoportosítás (balról 0-val kiegészítve, hogy meglegyen a 3 bit): `010 011 101`
2.  Átváltás:
    * `010₂` = `2₈`
    * `011₂` = `3₈`
    * `101₂` = `5₈`
3.  Eredmény: **`235₈`**

**Példa: `235₈` → Bináris**
* `2₈` = `010₂`
* `3₈` = `011₂`
* `5₈` = `101₂`
Eredmény: **`010011101₂`** (a vezető 0 elhagyható)

---


## 3. Kódrendszerek

A számrendszereken kívül különböző **kódrendszereket** is használunk az információ digitális ábrázolására, melyek speciális célokat szolgálnak.

### 3.1. Binárisan Kódolt Decimális (BCD)
* **Jellemzők:** Minden egyes decimális számjegyet külön kódolunk 4 bites bináris számmal.
* **Alkalmazás:** Digitális kijelzők, pénztárgépek, ahol a decimális számolás és megjelenítés a legfontosabb.
* **Példa:** `157₁₀` BCD kódja: `0001 0101 0111` (az 1, 5 és 7 decimális jegyek bináris megfelelői)
* **Hátrány:** Kevésbé hatékony a tárolás szempontjából, mint a tiszta bináris ábrázolás (pl. 157₁₀ tiszta binárisan 8 bit, BCD-ben 12 bit).

### 3.2. Stibitz (Excess-3) Kód
* **Jellemzők:** Egy nem súlyozott BCD kód. A decimális számjegyek bináris kódját úgy kapjuk meg, hogy a decimális értékhez hozzáadunk 3-at, majd az eredményt 4 biten binárisan kódoljuk.
* **Előny:** Önkomplementáló (az 1-esek és 0-k felcserélésével megkapjuk a kilences komplemensét, ami hasznos a kivonás műveletnél), és tartalmaz legalább egy 1-est minden számjegy ábrázolásában, ami hibadetektálásra is alkalmassá teszi.
* **Példa:** `4₁₀`
    * `4 + 3 = 7`
    * `7` binárisan (4 biten): `0111`
    * Tehát `4₁₀` Stibitz kódja: `0111`
* **Példa:** `7₁₀`
    * `7 + 3 = 10`
    * `10` binárisan (4 biten): `1010`
    * Tehát `7₁₀` Stibitz kódja: `1010`

### 3.3. Gray-kód
* **Jellemzők:** Olyan bináris kód, ahol két egymást követő számkód (bináris szó) között **mindig csak egy bitben** van eltérés. Emiatt **hibatűrő**, mivel a mechanikai vagy elektronikus érzékelőknél nem fordul elő egyszerre több bit változása.
* **Előny:** Kiválóan alkalmas analóg-digitális átalakítóknál, szöghelyzet-érzékelőknél és kódoló tárcsáknál, ahol a pozícióváltás során bekövetkező hibás leolvasások elkerülése kritikus.
* **Átváltás binárisból Gray-kódba:**
    1.  A legmagasabb helyiértékű bit (MSB) marad azonos.
    2.  A többi bitet úgy kapjuk meg, hogy az eredeti bináris szám `i`-edik bitjét **kizáró VAGY (XOR)** művelettel összehasonlítjuk az `i-1`-edik bitjével.
* **Példa: `0101₂` átváltása Gray-kódba**
    * MSB (balról az első): `0` marad `0`
    * Második bit: `0 XOR 1 = 1`
    * Harmadik bit: `1 XOR 0 = 1`
    * Negyedik bit: `0 XOR 1 = 1`
    * Eredmény: `0101₂` Gray-kódja: `0111`

* **Példa: Gray-kód táblázat (3 biten)**
    | Decimális | Bináris | Gray-kód |
    | :---: | :---: | :---: |
    | 0 | 000 | 000 |
    | 1 | 001 | 001 |
    | 2 | 010 | 011 |
    | 3 | 011 | 010 |
    | 4 | 100 | 110 |
    | 5 | 101 | 111 |
    | 6 | 110 | 101 |
    | 7 | 111 | 100 |

### 3.4. N-ből n-kódok
* **Jellemzők:** Olyan kódok, ahol minden érvényes kódszó pontosan `n` darab 1-es bitet tartalmaz az `N` lehetséges bithelyből. Ezáltal könnyen ellenőrizhető a hibátlan adatátvitel, mivel a hibás kódok más számú 1-est tartalmaznának.
* **Alkalmazás:** Hibadetektálás, megbízhatóság növelése.
* **Példa:** 3-ból 2-es kód: A `011`, `101`, `110` érvényes kódszavak, mert mindegyik pontosan két 1-est tartalmaz.

### 3.5. Johnson-kód (Forgókód)
* **Jellemzők:** Egy sorrendi (ciklikus) kód, amelyet gyakran használnak számlálóknál és léptetőregisztereknél. Egy `n` bites Johnson-kód `2n` különböző állapotot tud megjeleníteni. Jellemzője, hogy minden lépésben csak egy bit változik, hasonlóan a Gray-kódhoz, de más mintázatban.
* **Felépítés:** Kezdődik `n` darab 0-val, majd a jobb szélső bit 1-es lesz, utána a következő, és így tovább, amíg az összes bit 1-es nem lesz. Ezután a bal szélső bit 0-ra vált, majd a következő, és így tovább, amíg az összes bit 0-ra nem vált vissza.
* **Példa:** 3 bites Johnson-kód (2*3 = 6 állapot):
    `000` -> `001` -> `011` -> `111` -> `110` -> `100` -> `000` (ciklikus)

### 3.6. ASCII kód (American Standard Code for Information Interchange)

* **Jellemzők:** A legelterjedtebb karakterkódolási szabvány, amely a latin ábécé betűit (kis- és nagybetűk), számjegyeket, írásjeleket és vezérlőkaraktereket társít 7 bites bináris kódokhoz (összesen 128 karakter).
* **Alkalmazás:** Szöveges adatok tárolása és továbbítása számítógépek között, programkódok, dokumentumok.
* **Példa karakterek:**
    * `A` = `01000001₂` (decimálisan 65)
    * `a` = `01100001₂` (decimálisan 97)
    * `0` = `00110000₂` (decimálisan 48)

* **ASCII táblázat:**
    * *Itt lesz beillesztve a teljes ASCII táblázat, amikor a dokumentumot véglegesítjük.*

* **Példa: A "DIGIT" szó ASCII kódolása**
    Vegyük a "DIGIT" szót, és kódoljuk karakterenként ASCII bináris formába (7 biten):

    * **D**
        * Decimális érték: `68`
        * Bináris kód (7 biten): `1000100₂`

    * **I**
        * Decimális érték: `73`
        * Bináris kód (7 biten): `1001001₂`

    * **G**
        * Decimális érték: `71`
        * Bináris kód (7 biten): `1000111₂`

    * **I**
        * Decimális érték: `73`
        * Bináris kód (7 biten): `1001001₂`

    * **T**
        * Decimális érték: `84`
        * Bináris kód (7 biten): `1010100₂`

    A "DIGIT" szó egybefüggő ASCII bináris reprezentációja tehát a bitek egymás után fűzésével:
    `1000100 1001001 1000111 1001001 1010100`
### 3.7 Számrendszerek és Kódrendszerek Összehasonlító Táblázata

Ez a táblázat áttekintést nyújt a leggyakrabban használt számrendszerek (decimális, bináris, hexadecimális), valamint a fontos kódrendszerek (Stibitz/Excess-3, Gray-kód) közötti megfeleltetésről.

| Decimális | Bináris (4 bit) | Hexadecimális | Stibitz (Excess-3) | Gray-kód (4 bit) |
| :-------: | :-------------: | :-----------: | :----------------: | :--------------: |
|    **0** |      0000       |       0       |        0011        |       0000       |
|    **1** |      0001       |       1       |        0100        |       0001       |
|    **2** |      0010       |       2       |        0101        |       0011       |
|    **3** |      0011       |       3       |        0110        |       0010       |
|    **4** |      0100       |       4       |        0111        |       0110       |
|    **5** |      0101       |       5       |        1000        |       0111       |
|    **6** |      0110       |       6       |        1001        |       0101       |
|    **7** |      0111       |       7       |        1010        |       0100       |
|    **8** |      1000       |       8       |        1011        |       1100       |
|    **9** |      1001       |       9       |        1100        |       1101       |
|    **10** |      1010       |       A       |      *érvénytelen* |       1111       |
|    **11** |      1011       |       B       |      *érvénytelen* |       1110       |
|    **12** |      1100       |       C       |      *érvénytelen* |       1010       |
|    **13** |      1101       |       D       |      *érvénytelen* |       1011       |
|    **14** |      1110       |       E       |      *érvénytelen* |       1001       |
|    **15** |      1111       |       F       |      *érvénytelen* |       1000       |

---

### Magyarázat a táblázathoz:

* **Decimális:** A 10-es alapú számrendszer, amit a hétköznapokban használunk.
* **Bináris:** A 2-es alapú számrendszer, a számítógépek "nyelve". Itt 4 bitet használunk az ábrázoláshoz 0-tól 15-ig.
* **Hexadecimális:** A 16-os alapú számrendszer. A 10-15 értékeket az A-F betűkkel jelölik. Gyak
---

## 4. Kontextus: Tárolási Kapacitások

A bináris rendszer hatással van arra is, ahogyan a tárolókapacitásokat mérjük. Bár a hétköznapi életben a `kilo`, `mega`, `giga` prefixumok 1000 hatványait jelentik, a számítástechnikában hagyományosan 2 hatványait (1024) jelölték velük.

| Prefixum | Decimális (SI) érték | Bináris (IEC) érték | Méret |
| :--- | :--- | :--- | :--- |
| **Kilobájt (KB)** | 10³ = 1000 bájt | **Kibibájt (KiB)** = 2¹⁰ = 1024 bájt | Egy rövid email |
| **Megabájt (MB)** | 10⁶ = 1 000 000 bájt | **Mebibájt (MiB)** = 2²⁰ = 1 048 576 bájt | Egy MP3 dal |
| **Gigabájt (GB)** | 10⁹ bájt | **Gibibájt (GiB)** = 2³⁰ bájt | Egy HD film |
| **Terabájt (TB)** | 10¹² bájt | **Tebibájt (TiB)** = 2⁴⁰ bájt | Egy modern merevlemez |

> **Gyakorlati jelentőség:** Ezért van az, hogy egy 1 TB-osnak hirdetett merevlemez a számítógép operációs rendszerében csak kb. 931 GB-osnak látszik. A gyártók 10-es (10¹²), míg az operációs rendszerek gyakran 2-es (2⁴⁰) hatványokkal számolnak. `10¹² / 2³⁰ ≈ 931.3 GiB`.
