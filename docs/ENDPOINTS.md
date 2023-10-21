# Végpontok

> A jegyzet folyamatosan bővül

## `GET` `/api/associations` - Összes egyesület lekérdezése

**Query parameters:**

- `offset` - elhagyandó dokumentumok száma (alapértelmezett: 0)
- `limit` - maximum megjelenítendő dokumemntumok száma (alapértelmezett: 10, maximum: 40)
- `projection`
  - `lite` (alapértelmezett): csak az `_id` és `name` mezők megjelenítése
  - `full`: az összes mező megjelenítése
- `orderBy`: a mező neve, ami alapján a dokumentumokat rendezni kívánjuk, a csökkenő sorrendet `-` karakter jelöli (alapértelmezett: `name`)
- `q`: a `name` mező alapján való keresés (*case-insensitive*)

Az összes regisztrált egyesületet le tudjuk ezzel kérdezni.  
**Ez egy nyilvános végpont**, úgyhogy a szándékos túlterhelés elkerülése érdekében a percenkénti kérések száma korlátozott (? pontosan milyen mértékben ?).

Pl.:

```rest
GET /api/associations?offset=10&limit=5&projection=full&orderBy=name
```

A válasz formátuma:

```json
{
  "meta": {
    "total": 50,
    "offset": 10,
    "limit": 5
  },
  "items": [
    {
        "_id": "652f7b95fc13ae3ce86c7cdf",
        "name": "Baráti Polgárõr Egyesület",
        "location": "9081 Győrújbarát Fő u. 1",
        "officialIdentifier": "08/0001"
    },
    ... 4 more items ...
  ]
}
```

## `POST` `/api/members` - új tag hozzáadása

**Required http headers:**

- `x-auth-token` - a tagot azonosító token

Az **egyesületvezető ranggal rendelkező** tagok ezen a végponton kereszttül
tudják regisztrálni a további tagokat.

#### ?:

- Mik azok a szükséges adatok, amelyeknek a kérés törzsében szerepelniük kell, vagy opcionálisak? (Mert lehet bizonyos adatok megadását a csoportvezető a tagra bízza)
- Mi legyen a válasz törzsében?

## `GET` `/api/members` - az egyesület tagjainak lekérdezése

**Required http headers:**

- `x-auth-token` - a tagot azonosító token

Az adott egyesülethez tartozó összes tag adatait adja vissza.
Értelemszerűen csak egy bejelentkezett tag hívhatja meg ezt a végpontot,
hogy a csoporttársait meg tudja tekinteni.

#### ?:

- A pagináció hogyan legyen implementálva?

## `GET` `/api/members/{id}` -
