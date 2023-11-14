# Adatbázis

## Kollekciók

### associations (Egyesületek)

Az egyesületet adatait tárolja.

**Mezők:**

- `_id`\*: ObjectId
- `name`\* (Név): String
- `location`\* (Cím): String
- `certificate`\* (Tanúsítvány): String
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

## members (Tagok)

A tagok adatait tárolja.

**Mezők:**

*Jelölések: `**` - minden esetben kötelező, `*` - csak abban az esetben kötelező, ha a felhasználó felregisztrált állapotban van (ha az `isRegistered` értéke `true`)*

- **`_id`**\*\*: ObjectId
- **`isRegistered`**\*\*: Boolean 
  - Értéke `true`, ha a tag felregisztrált, `false` ha csak meghívott állapotban van
- **`email`**\*\* (Email): String
  - **Egyesületen belül egyedi**
- **`association`**\*\* (Egyesületazonosító): ObjectId
  - **Külső kulcsként** szolgál az egyesület azonosítására.
- **`username`**\* (Felhasználónév): String
  - **Egyesületen belül egyedi.**
- **`password`**\* (Jelszó): String
  - Titkosított formában van tárolva (Bcrypt hash)
  - [Jelszó megkötések](#jelszó-megkötések)
- **`name`**\* (Név): String
  - Teljes név
- **`address`**\* (Lakcím): String
- **`idNumber`**\* (Személy-igazolványszám): String
  - **Egyesületen belül egyedi**
- **`phoneNumber`**\* (Telefonszám): String
- **`guardNumber`** (OPSZ-igazolványszám): String
  - Polgárőr igazolvány száma
  - 3 szekcióból áll, ebből az első 2 az egyesületet azonosítja (lsd. [`associations` kollekcióban](#associations-egyesületek) a `certificate` mezőt), a 3. az egyedet
- **`roles`**\* (Rangok): Array[String]
  - A tag rangjait tároló tömb
  - Lehetséges értékek:
    - `member` - az "egyszerű" tag jelölése
    - `president` - az egyesületvezető rang jelölése
  - Azért tároljuk tömbben, hogy a rangok listája könnyen bővíthető legyen (skálázhatóság) 
- **`preferences`** (Beállítások): Object


#### Jelszó megkötések

- Minimum 8 karakter
- Kell kis- és nagybetű
- Kell legalább egy szám
