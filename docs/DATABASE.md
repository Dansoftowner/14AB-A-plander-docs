# Adatbázis

A Plander NoSQL (MongoDB) adatbázist használ az adatok tárolásához.

## Kollekciók

### associations (Egyesületek)

Az egyesületet adatait tárolja.

**Mezők:**

- `_id`\*: ObjectId
- `name`\* (Név): String
  - **Egyedi**
- `location`\* (Cím): String
- `certificate`\* (Tanúsítvány): String
  - **Egyedi**
  - Az egyesület azonosítója
  - Ebből az első szekció a megye azonosítója, a második szekció azonosítja magát az egyesületet
  - Pl.: "08/0001"

Példa dokumentum:

```json
{
  "_id": "652f7b95fc13ae3ce86c7cdf",
  "name": "Baráti Polgárõr Egyesület",
  "location": "9081 Győrújbarát Fő u. 1",
  "certificate": "08/0001"
}
```

### members (Tagok)

A tagok adatait tárolja.

**Mezők:**

_Jelölések: `**` - minden esetben kötelező, `*`- csak abban az esetben kötelező, ha a felhasználó felregisztrált állapotban van (ha az`isRegistered`értéke`true`)_

- **`_id`**\*\*: ObjectId
- **`isRegistered`**\*\*: Boolean
  - Értéke `true`, ha a tag felregisztrált, `false` ha csak meg van hívva de **megerősítetlen** állapotban van.
- **`association`**\*\* (Egyesületazonosító): ObjectId
  - **Külső kulcsként** szolgál az egyesület azonosítására.
- **`email`**\*\* (Email): String
  - **Egyesületen belül egyedi**.
- **`username`**\* (Felhasználónév): String
  - **Egyesületen belül egyedi.**
- **`password`**\* (Jelszó): String
  - Hash-elt formában van tárolva (Bcrypt hash).
  - [Jelszó megkötések](#jelszó-megkötések)
- **`name`**\* (Név): String
  - Teljes név.
- **`address`**\* (Lakcím): String
- **`idNumber`**\* (Személy-igazolványszám): String
  - **Egyesületen belül egyedi**
- **`phoneNumber`**\* (Telefonszám): String
- **`guardNumber`** (OPSZ-igazolványszám): String
  - Polgárőr igazolvány száma
  - 3 szekcióból áll, ebből az első 2 az egyesületet azonosítja (lsd. [`associations` kollekcióban](#associations-egyesületek) a `certificate` mezőt), a 3. az egyedet.
- **`roles`**\*\* (Rangok): Array[String]
  - A tag rangjait tároló tömb.
  - Lehetséges értékek:
    - `member` - az "egyszerű" tag jelölése
    - `president` - az egyesületvezető rang jelölése
  - Azért tároljuk tömbben, hogy a rangok listája könnyen bővíthető legyen (skálázhatóság).
- **`preferences`** (Beállítások): Object
  - Egy tetszőleges objektum, ami tárol bizonyos felhasználói beállításokat.
  - Tipikusan a front-end dönti el, hogy mit akar itt tárolni.
  - **Nincs sémája**.

#### Jelszó megkötések

- Minimum 8 karakter
- Kell kis- és nagybetű
- Kell legalább egy szám

Példa dokumentumok:

```json
{
  "_id": "652f866cfc13ae3ce86c7ce7",
  "isRegistered": true,
  "association": "652f7b95fc13ae3ce86c7cdf",
  "email": "bverchambre0@alibaba.com",
  "username": "gizaac0",
  "password": "$2a$12$V1yb5jg1tSHwJA21yZuFh.dboiJ93dExP48bZQhurylJ85V53yoKi",
  "name": "Horváth Csaba",
  "address": "929 Brentwood Hill",
  "idNumber": "594771CQ",
  "phoneNumber": "+256 (776) 361-0286",
  "guardNumber": "08/0001/009226",
  "roles": ["member", "manager"],
  "preferences": { "ui.theme": "dark", "ui.language": "en_EN" }
}
```

```json
{
  "_id": "652f866cfc13ae3ce86c7ce6",
  "isRegistered": false,
  "association": "652f7b95fc13ae3ce86c7cdf",
  "email": "aarobbab@bob.com",
  "roles": ["member", "manager"]
}
```

### _registrationTokens (Regisztrációs token-ek)_

Ez a kollekció tárolja a **regisztrációs token**eket, amelyek a regisztráció előtt álló tagok azonosítására szükségesek.

**Mezők:**

- `_id`\*: ObjectId
- `memberId`\*: ObjectId
  - A tag azonosítója.
- `token`\*: String
  - A random generált token string.
  - Hash-elt formában van tárolva (BCrypt hash).

### _restorationTokens (Helyreállítási token-ek)_

Ez a kollekció tárolja a **helyreállítási token**eket, amelyek azon tagok azonosítására szükségesek, akik az elfelejtett jelszavuk helyreállítása előtt állnak.

**Mezők:**

- `_id`\*: ObjectId
- `memberId`\*: ObjectId
  - A tag azonosítója.
- `token`\*: String
  - A random generált token string.
  - Hash-elt formában van tárolva (BCrypt hash).

### assignments (Beosztások)

Ez a kollekció tárolja el a beosztásokat (naptári eseményeket, amelyeken min. 1 tag szolgál).

**Mezők:**

- `_id`\*: ObjectId
- `title` (Esemény neve): String
  - Minimum 5, maximum 255 karakter
- `location` (Helyszín): String
  -  Minimum 2, maximum 255 karakter
- `association`\* (Egyesületazonosító): ObjectId
  - **Külső kulcsként** szolgál az egyesület azonosítására.
- `start`\* (Kezdés ideje): Date
  - Korábbi időpont kell legyen mint az `end`
- `end`\* (Befejezés ideje): Date
  - Későbbi időpont kell legyen mint a `start`
- `assignees` (Beosztott tagok): [Object]
  - A beosztott tagok beágyazott objektumokként vannak eltárolva egy tömbben
  - Ezek a beágyazott dokumentumok nem tartalmaznak az adott tagról minden információt
    - Az `_id`-n kívül a `name` mező van eltárolva
    - Egy beágyazott tag azonosítója refererál a `members` kollekcióban található tag azonosítójára
  - A dokumentum beágyazás a következő előnyökkel bír:
    - A beosztott tagokról el van tárolva egy pillanatkép (`snapshot`),
      így ha a tag később törölve is lesz a rendszerből, a beosztások visszatekinthetőek lesznek (akár évekig, ha szükséges)
    - Gyorsabb lekérdezéseket tesz lehetővé
- `report` (Jelentés azonosítója): ObjectId
    - Egyedi, mert egy jelentés csak egy beosztáshoz tartozhat

### reports (Jelentések)

- `_id`\*: ObjectId
- `reporter`\* (A jelentés készítőjének azonosítója): ObjectId
- `method`\* (Szolgálat módja): String
  - Lehetséges értékek: `vehicle` (Gépkocsi), `bicycle` (Kerékpár), `pedestrian` (Gyalogos)
- `purpose`\* (Szolgálat célja): String
  - pl.: rendezvénybiztosítás, iskolaszolgálat, stb.
- `licensePlateNumber` (Szolgálatot ellátó gépkocsi rendszáma): String
  - **Opcionáls**, csak akkor van jelentősége ha a szolgálat módja `vehicle`
- `startKm` (KM Óra állása induláskor): Number
  - **Opcionális**, csak akkor van jelentősége ha a szolgálat módja `vehicle`
- `endKm` (KM Óra állása befejezéskor): Number
  - **Opcionális**, csak akkor van jelentősége ha a szolgálat módja `vehicle`
- `externalOrganization` (Külső szerv neve): String
  - **Opcionális**, csak akkor van jelentősége ha a szolgálat során történt más külső szervvel együttműködés
- `externalRepresentative` (Külső szerv képviselője): String
  - **Opcionális**, csak akkor van jelentősége ha a szolgálat során történt más külső szervvel együttműködés
  - _De csak abban az esetben lehet meghatározva, ha a külső szerv neve is meg van adva!_
- `description` (Rövid leírás): String
  - Maximum **1240** karakter
- `submittedAt` (Jelentés elküldésének ideje): Date
