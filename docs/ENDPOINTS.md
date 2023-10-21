# Végpontok

> A jegyzet folyamatosan bővül

## `GET` `/api/associations` - Összes egyesület lekérdezése

Az összes regisztrált egyesületet le tudjuk ezzel kérdezni.  
**Ez egy nyilvános végpont**, úgyhogy a szándékos túlterhelés elkerülése érdekében a percenkénti kérések száma korlátozott (? pontosan milyen mértékben ?).

#### ?:
- A pagináció hogyan legyen implementálva?

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
