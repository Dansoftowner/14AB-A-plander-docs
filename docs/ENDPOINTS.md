# Végpontok

> A jegyzet folyamatosan bővül

### Áttekintés

[**Associations (Egyesületek)**](#associations-egyesületek)
|       |                                                      |                                                   |
|-------|------------------------------------------------------|---------------------------------------------------|
| `GET` | [`/api/associations/`](#get-apiassociations)         | Összes egyesület lekérdezése.                     |
| `GET` | [`/api/associations/{id}`](#get-apiassociationsid)   | Egy adott egyesület adatainak lekérdezése.        |
| `GET` | [`/api/associations/mine`](#get-apiassociationsmine) | Az adott tag egyesületének adatainak lekérdezése. |

[**Members (Tagok)**](#members-tagok)
|         |                                                                                                                   |                                                             |
|---------|-------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|
| `GET`   | [`/api/members/`](#get-apimembers)                                                                                | Az adott tag egyesületébe tartozó összes tag lekérdezése.   |
| `GET`   | [`/api/members/{id}`](#get-apimembersid)                                                                          | Egy adott tag adatainak lekérése az azonosítója alapján.    |
| `GET`   | [`/api/members/me`](#get-apimembersme)                                                                            | Egy tag ezen keresztül tudja lekérdezni a saját adatait.    |
| `GET`   | [`/api/members/username/{username}`](#get-apimembersusernameusername)                                             | Egy adott tag adatainak lekérése a felhasználóneve alapján. |
| `GET`   | [`/api/members/exists/username/{associationId}/{username}`](#get-apimembersexistsusernameassociationidusername)   | Felhasználónév elérhetőségének ellenőrzése.                 |
| `GET`   | [`/api/members/register/{id}/{registrationToken}`](#get-apimembersregisteridregistrationtoken)                    | Egy meghívott tag elérhető adatainak lekérdezése.           |
| `POST`  | [`/api/members/`](#post-apimembers)                                                                               | Egy tag meghívása az egyesületbe.                           |
| `POST`  | [`/api/members/register/{id}/{registrationToken}`](#post-apimembersregisteridregistrationtoken)                   | Egy meghívott tag regisztrálása.                            |
| `POST`  | [`/api/members/forgotten-password`](#post-apimembersforgotten-password)                                           | Elfelejtett jelszó helyreállítása.                          |
| `POST`  | [`/api/members/forgotten-password/{id}/{restorationToken}`](#post-apimembersforgotten-passwordidrestorationtoken) | Új jelszó beállítása az elfelejtett jelszó helyett.         |
| `PATCH` | [`/api/members/email/{id}`](#patch-apimembersemailid)                                                             | Egy tag email címének módosítása.                           |
| `PATCH` | [`/api/members/email/mine`](#patch-apimembersemailmine)                                                           | Egy tag ezen keresztül tudja módosítani saját email címét.  |
| `PATCH` | [`/api/members/credentials/mine`](#patch-apimemberscredentialsmine)                                               | Egy tag felhasználónevének/jelszavának módosítása.          |
| `PATCH` | [`/api/members/{id}`](#patch-apimembersid)                                                                        | Egy tag adatainak módosítása.                               |
| `PATCH` | [`/api/members/me`](#patch-apimembersme)                                                                          | Egy tag ezen keresztül tudja módosítani a saját adatait.    |
| `PATCH` | [`/api/members/transfer-my-roles/{id}`](#patch-apimemberstransfer-my-rolesid)                                     | Egyesületvezető jog átruházása.                             |



## Associations (Egyesületek)

### `GET` `/api/associations`

Az összes regisztrált egyesület lekérdezése.

**Query parameters:**

- `offset` - elhagyandó dokumentumok száma (alapértelmezett: 0)
- `limit` - maximum megjelenítendő dokumentumok száma (alapértelmezett: 10, maximum: 40)
- `projection`
  - `lite` (alapértelmezett): csak az `_id` és `name` mezők megjelenítése
  - `full`: az összes mező megjelenítése
- `orderBy`: a mező neve, ami alapján a dokumentumokat rendezni kívánjuk, a csökkenő sorrendet `-` karakter jelöli (alapértelmezett: `name`)
- `q`: a `name` mező alapján való keresés (_case-insensitive_)

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

### `GET` `/api/associations/{id}`

Egy adott egyesület adatainak lekérdezése.

**Parameters:**

- `id` - az egyesület azonosítója

**Query parameters:**

- `projection`
  - `lite` (alapértelmezett): csak az `_id` és `name` mezők megjelenítése
  - `full`: az összes mező megjelenítése

Pl.:

```rest
GET /api/associations/652f7b95fc13ae3ce86c7cdf?projection=full
```

A válasz formátuma:

```json
{
  "_id": "652f7b95fc13ae3ce86c7cdf",
  "name": "Baráti Polgárõr Egyesület",
  "location": "9081 Győrújbarát Fő u. 1",
  "officialIdentifier": "08/0001"
}
```

### `GET` `/api/associations/mine`

Az adott tag egyesületének adatainak lekérdezése. (*Gyakorlatilag egy egyszerűsített változata az előzőleg bemutatott végpontnak, de ez a token-ből nyeri ki az id-t.*)

**Required http headers:**

- `x-auth-token` - a tagot azonosító token

Pl.:

```rest
GET /api/associations/mine
x-auth-token: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo
```

A válasz formátuma:

```json
{
  "_id": "652f7b95fc13ae3ce86c7cdf",
  "name": "Baráti Polgárõr Egyesület",
  "location": "9081 Győrújbarát Fő u. 1",
  "officialIdentifier": "08/0001"
}
```

## Members (tagok)

#### Egy tag által megtekinthető más tagok adatai:

- `_id`
- `username`
- `officialIdentifier` (csak `manager` ranggal)
- `name`
- `address` (csak `manager` ranggal)
- `idNumber` (csak `manager` ranggal)
- `email`
- `phoneNumber`
- `roles`
- `isVerified` (ez a jellemző ugyan látható, de a nem megerősített tagok csak a `manager` ranggal rendelkezők számára jelennek meg)

### `GET` `/api/members`

Az adott tag egyesületébe tartozó összes tag lekérdezése.

**Required http headers:**

- `x-auth-token` - a tagot azonosító token

**Query parameters:**

- `offset` - elhagyandó dokumentumok száma (alapértelmezett: 0)
- `limit` - maximum megjelenítendő dokumentumok száma (alapértelmezett: 10, maximum: 40)
- `projection`
  - `lite` (alapértelmezett): csak az `_id`, `username`, `name`, `email`, `phoneNumber`, `isVerified` és `roles` mezők megjelenítése
  - `full`: az összes (a tag által megtekinthető, lsd: [fent](#egy-tag-által-megtekinthető-más-tagok-adatai)) mező megjelenítése
- `orderBy`: a mező neve, ami alapján a dokumentumokat rendezni kívánjuk, a csökkenő sorrendet `-` karakter jelöli (alapértelmezett: `name`)
- `q`: a `name` mező alapján való keresés (_case-insensitive_)

Pl.:

```rest
GET /api/members?offset=10&limit=5&projection=lite&orderBy=name
x-auth-token: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo
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
    "_id": "652f85c4fc13ae3d596c7cdf",
    "username": "ccolebeck0",
    "name": "András Kovács",
    "email": "ahilhouse0@disqus.com",
    "phoneNumber": "+34 (829) 635-5692",
    "isVerified": true,
    "roles": ["member", "manager"]
    }
    ... 4 more items ...
  ]
}
```

### `GET` `/api/members/{id}`

Egy adott tag adatainak lekérése az azonosítója alapján.

**Parameters:**

- `id` - a lekérendő tag azonosítója

**Required http headers:**

- `x-auth-token` - a tagot azonosító token

**Query parameters:**

- `projection`
  - `lite` (alapértelmezett): csak az `_id`, `username`, `name`, `email`, `phoneNumber`, `isVerified` és `roles` mezők megjelenítése
  - `full`: az összes (a tag által megtekinthető, lsd: [fent](#egy-tag-által-megtekinthető-más-tagok-adatai)) mező megjelenítése

Ha a megjelenítendő tag létezik az azonosítója alapján, **de nem ugyanabba az egyesületbe tartozik**, mint a kérés küldője (akit a _token_ azonosít), akkor az adatai nem kérhetőek le.

> Ha az azonosító alapján megjelenítendő tag ugyanaz, mint aki a kérést küldte, akkor jogosult az egyébként rangja alapján nem feltétlenül látható mezők megtekintésére is.

Pl.:

```rest
GET /api/members/652f85c4fc13ae3d596c7cdf?projection=lite
x-auth-token: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo
```

A válasz formátuma:

```json
{
  "_id": "652f85c4fc13ae3d596c7cdf",
  "username": "ccolebeck0",
  "name": "András Kovács",
  "email": "ahilhouse0@disqus.com",
  "phoneNumber": "+34 (829) 635-5692",
  "isVerified": true,
  "roles": ["member", "manager"]
}
```

### `GET` `/api/members/me`

Egy tag ezen keresztül tudja lekérdezni a saját adatait. (*Gyakorlatilag egy egyszerűsített változata az [előzőleg bemutatott végpontnak](#get-apimembersid), de ez a token-ből nyeri ki az id-t.*)

### `GET` `/api/members/username/{username}`

Egy adott tag adatainak lekérése a felhasználóneve alapján.

**Parameters:**

- `username` - a lekérendő tag felhasználóneve

**Required http headers:**

- `x-auth-token` - a tagot azonosító token

**Query parameters:**

- `projection`
  - `lite` (alapértelmezett): csak az `_id`, `username`, `name`, `email`, `phoneNumber`, `isVerified` és `roles` mezők megjelenítése
  - `full`: az összes (a tag által megtekinthető, lsd: [fent](#egy-tag-által-megtekinthető-más-tagok-adatai)) mező megjelenítése

Mivel a tag felhasználóneve csak az egyesületen belül egyedi, a kérés küldője (akit a _token_ azonosít) ugyanabba az egyesületbe kell tartozzon, különben az adatok nem kérhetőek le.

> Ha az felhasználónév alapján megjelenítendő tag ugyanaz, mint aki a kérést küldte, akkor jogosult az egyébként rangja alapján nem feltétlenül látható mezők megtekintésére is.

Pl.:

```rest
GET /api/members/username/ccolebeck0?projection=lite
x-auth-token: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo
```

A válasz formátuma:

```json
{
  "_id": "652f85c4fc13ae3d596c7cdf",
  "username": "ccolebeck0",
  "name": "András Kovács",
  "email": "ahilhouse0@disqus.com",
  "phoneNumber": "+34 (829) 635-5692",
  "isVerified": true,
  "roles": ["member", "manager"]
}
```

### `GET` `/api/members/exists/username/{associationId}/{username}`

Ezen a végponton keresztül egyszerűen le lehet ellenőrizni, hogy az adott felhasználónév az egyesületen belül elérhető vagy nem.

**Parameters:**
- `associationId` - egyesület azonosítója
- `username` - az ellenőrizni kívánt felhasználónév

Pl.:

```rest
GET /api/members/exists/username/652f7b95fc13ae3ce86c7cdf/exampleUsrName1
```

A válasz formátuma:

```json
false
```
*Egyszerű json `boolean` értékkel.*

### `GET` `/api/members/register/{id}/{registrationToken}`

Ez a végpont egy **meghívott tag** egyesületvezető által megadott adatait adja vissza. *Csak megerősítetlen, még regisztráció előtt álló tagok esetében releváns.* 

**Parameters:**
- `id` - a tag azonosítója
- `registrationToken` - a **regisztrációs token**

Pl.:

```rest
GET /api/members/register/652f85c4fc13ae3d596c7cde/4vcmk1uzezrx7uc9bb5a19iu0cbsgd
```

A válasz formátuma:  
```json
{
  "_id": "652f85c4fc13ae3d596c7cde",
  "associationId": "652f7b95fc13ae3ce86c7cdf",
  "email:": "member@example.com",
  "isVerified": false
} 
```
(Ha az egyesületvezető egyéb adatokat is megadott a tag meghívásakor az e-mail címen kívül, akkor természetesen azok is szerepelnek a válasz törzsében.)

### `POST` `/api/members`

Az **egyesületvezető ranggal rendelkező** tagok ezen a végponton kereszttül
tudják **meghívni** a további tagokat.

**Required http headers:**

- `x-auth-token` - a tagot azonosító token

**Kérés formátuma:**  
Content-Type: `application/json`

- `email*`
- *`officialIdentifier`* 
- *`name`*
- *`address`*
- *`idNumber`*
- *`phoneNumber`*

Mivel az egyesületvezetőtől nem várható el az, hogy ő maga adja meg a regisztrálandó tag összes adatát, neki **csak az e-mail címet** kell kötelezően megadnia.  

A felhasználónevet és jelszót viszont kizárólag csak a tag tudja megadni később (addig ún. *unverified/megerősítetlen* állapoban van rögzítve az adatbázisban).

**A végpont meghívásakor a meghívott tag automatikusan e-mailt kap egy regisztrációs linkkel, amin keresztül később véglegesíteni tudja magát.**

Pl.:

```rest
POST /api/members/
Content-Type: application/json
x-auth-token: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo

{
  "email:": "member@example.com"
}
```

A válasz formátuma:  
Status: `201`  
Tartalom: [A beillesztett rekord]
```json
{
  "_id": "652f85c4fc13ae3d596c7cde",
  "email:": "member@example.com",
  "isVerified": false
} 
```


### `POST` `/api/members/register/{id}/{registrationToken}`

A **meghívott tagok** ezen a végponton keresztül tudnak **regisztrálni** és a hiányzó adataikat pótolni.

**Parameters:**
- `id` - a tag azonosítója
- `registrationToken` - a **regisztrációs token**

**Kérés formátuma:**  
Content-Type: `application/json`

- *`username*`*
- *`password*`*
- *`officialIdentifier`* 
- *`name*`*
- *`address*`*
- *`idNumber*`*
- *`phoneNumber*`*

Pl.:

```rest
POST /api/members/register/652f85c4fc13ae3d596c7cde/4vcmk1uzezrx7uc9bb5a19iu0cbsgd
Content-Type: application/json

{
  "username": "superguard01",
  "password": "SuperSafe@41a",
  "officialIdentifier": "4148009",
  "name": "Horváth Csaba",
  "address": "9029 Csontvár Sonka Utca 5.",
  "idNumber": "594771CQ",
  "phoneNumber": "+256 (776) 361-0286"
}
```

A válasz formátuma:  
```json
{
  "_id": "652f85c4fc13ae3d596c7cde",
  "email:": "member@example.com",
  "username": "superguard01",
  "officialIdentifier": "4148009",
  "name": "Horváth Csaba",
  "address": "9029 Csontvár Sonka Utca 5.",
  "idNumber": "594771CQ",
  "phoneNumber": "+256 (776) 361-0286",
  "isVerified": true
} 
```

### `POST` `/api/members/forgotten-password`

Ez a végpont használható arra, hogy a tagok helyreállítsák az elfelejtett jelszavukat.  

**Kérés formátuma:**  
Content-Type: `application/json`

- *`associationId`* - az egyesület azonosítója
- *`email`* - a jelszavát helyreállítani kivánó tag e-mail címe

Ha a megadott e-mail címmel valóban létezik tag, akkor arra a címre egy helyreállító link kerül küldésre. 

Pl.:

```rest
POST /api/members/forgotten-password
Content-Type: application/json

{
  "associationId": "652f7b95fc13ae3ce86c7cdf",
  "email": "member@example.com"
}
```

A válasz formátuma:
```json
{
  "email": "member@example.com"
} 
```

### `POST` `/api/members/forgotten-password/{id}/{restorationToken}`

A jelszavukat elfelejtett tagok ezen a végponton keresztül tudnak új jelszót megadni.

**Parameters:**
- `id` - a tag azonosítója
- `restorationToken` - a helyreállítási token

**Kérés formátuma:**  
Content-Type: `application/json`

- *`password`* - az egyesület azonosítója

Pl.:

```rest
POST /api/members/forgotten-password/652f85c4fc13ae3d596c7cde/rbzdaewl4tc74xiu5tdd
Content-Type: application/json

{
  "password": "IWontForgetItAgain123"
}
```

?: A válasz formátuma

### `PATCH` `/api/members/email/{id}`

A tagok ezen a végponton keresztül tudják megváltoztatni az e-mail címüket, illetve az **egyesületvezetők** ezen a végponton keresztül tudják megváltoztatni a még nem megerősített (csak meghívott) tagok e-mail címét.

**Parameters:**
- `id` - a módosítandó tag azonosítója

**Required http headers:**

- `x-auth-token` - a tagot azonosító token  

A végpont működéséről a következőket mondhatjuk el:

* Ha a módosítandó tag létezik az azonosítója alapján, **de nem ugyanabba az egyesületbe tartozik**, mint a kérés küldője (akit a _token_ azonosít), akkor az e-mail címe nem módosítható.

* Ha a kérés küldője **nem egyesületvezető**, egy **másik tag** e-mail címét **nem módosíthatja**.

* A kérés küldője a saját e-mail címét módosíthatja (ebben az esetben a saját *id*-t adja meg).

* Ha egy tag meg akarja változtatni a saját e-mail címét, **az e-mail cím nem fog azonnal frissülni** az adatbázisban, csak miután a tag megnyitja az új e-mail címre kapott linket.

* Ha a kérés küldője **egyesületvezető**, **csak megerősítetlen** (`unverified`) tag e-mail címét módosíthatja.

* Ha egy **egyesületvezető** megváltoztatja egy megerősítetlen tag **e-mail címét**, újabb regisztrációs levél kerül kézbesítésre.

**Kérés formátuma:**  
Content-Type: `application/json`

- `email*`

Pl.:

```rest
PATCH /api/members/email/652f85c4fc13ae3d596c7cde
Content-Type: application/json
x-auth-token: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo

{
  "email": "newemail@example.com"
}
```

A válasz formátuma:

```json
{
  "_id": "652f85c4fc13ae3d596c7cde"
}
```

### `PATCH` `/api/members/email/mine`

A tagok ezen a végponton keresztül tudják megváltoztatni az e-mail címüket. (*Gyakorlatilag egy egyszerűsített változata az [előzőleg bemutatott végpontnak](#patch-apimembersemailid), de ez a token-ből nyeri ki az id-t.*)

### `PATCH` `/api/members/credentials/mine`

A tagok ezen a végponton keresztül tudják megváltoztatni a felhasználónevüket és/vagy e-mail címüket.

**Required http headers:**

- `x-auth-token` - a tagot azonosító token  
- `x-auth-password` - **mivel ez egy kockázatos művelet, az aktuális jelszó újbóli megadása kötelező, a token nem elég** 

**Kérés formátuma:**  
Content-Type: `application/json`

- `username` - az új felhasználónév (***egyesületen belül egyedinek kell lennie***)
- `password` - az új jelszó

Értelemszerűen csak akkor van a kérésnek értelme, ha a felhasználónév és/vagy jelszó meg van adva.

Pl.:

```rest
PATCH /api/members/credentials/652f85c4fc13ae3d596c7cde
Content-Type: application/json
x-auth-token: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo
x-auth-password: OldPassword123

{
  "password": "NewPassword"
}
```

A válasz formátuma:

```json
{
  "_id": "652f85c4fc13ae3d596c7cde",
  "updated": [ "password" ]
}
```


### `PATCH` `/api/members/{id}`

Egy **egyesületvezető** ezen a végponton keresztül tudja módosítani a **megerősítetlen** tagok adatait, illetve egy tag a saját adatait. 

**Parameters:**
- `id` - a módosítandó tag azonosítója

**Required http headers:**

- `x-auth-token` - a tagot azonosító token  

A végpont működéséről a következőket mondhatjuk el:

* Ha a módosítandó tag létezik az azonosítója alapján, **de nem ugyanabba az egyesületbe tartozik**, mint a kérés küldője (akit a _token_ azonosít), akkor az adatai nem kérhetőek le.

* Ha a kérés küldője **nem egyesületvezető**, egy **másik tag** adatait **nem módosíthatja**.

* Ha a kérés küldője nem egyesületvezető, de **megegyezik** az azonosítója alapján a **módosítandó taggal**, akkor az adatait módosíthatja.

* Ha a kérés küldője **egyesületvezető**, **csak megerősítetlen** (`unverified`) tag adatait módosíthatja.  
Kívételt jelent:
  - `preferences` - az adott tag egyéni beállításait csak az adott tag módosíthatja

* **A rangokat nincs lehetőség ezen a végponton keresztül módosítani.**

**Kérés formátuma:**  
Content-Type: `application/json`

- *`username`*
- *`password`*
- *`officialIdentifier`* 
- *`name`*
- *`address`*
- *`idNumber`*
- *`phoneNumber`*
- *`preferences`*

Pl.:

```rest
PATCH /api/members/652f85c4fc13ae3d596c7cde
Content-Type: application/json
x-auth-token: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo

{
  "address": "7200 Igazváros Valóság Utca 5",
  "idNumber": "1232IQ",
  "phoneNumber": "+1020113301"
}
```

A válasz formátuma:
```json
{
  "_id": "652f85c4fc13ae3d596c7cde",
  "email:": "member@example.com",
  "username": "superguard01",
  "officialIdentifier": "4148009",
  "name": "Horváth Csaba",
  "address": "7200 Igazváros Valóság Utca 5",
  "idNumber": "1232IQ",
  "phoneNumber": "+1020113301",
  "isVerified": true
} 
```

### `PATCH` `/api/members/me`

Egy tag ezen keresztül tudja módosítani a saját adatait. (*Gyakorlatilag egy egyszerűsített változata az [előzőleg bemutatott végpontnak](#patch-apimembersid), de ez a token-ből nyeri ki az id-t.*)

### `PATCH` `/api/members/transfer-my-roles/{id}`

Az **egyesületvezető** ezen a végponton keresztül tud felruházni *egyszerű tag*ot *egyesületvezető ranggal* (`manager`).

**Parameters:**

- `id` - a felruházni kivánt tag azonosítója

**Query parameters:**

- `copy` - ha értéke `true`, akkor az tag rangja továbbra is megmarad, ha éréke `false`, akkor a tag rangja törlődik, és "átszáll" a felruházott tagra (alapértelmezett érték: `false`)

**Required http headers:**

- `x-auth-token` - a tagot azonosító token
- `x-auth-password` - **mivel ez egy kockázatos művelet, az aktuális jelszó újbóli megadása kötelező, a token nem elég** 

A végpont működéséről a következőket mondhatjuk el:


* Ha a módosítandó tag létezik az azonosítója alapján, **de nem ugyanabba az egyesületbe tartozik**, mint a kérés küldője (akit a _token_ azonosít), akkor a kérés nem érvényes.
* Ha a kérést küldő tag **nem egyesületvezető**, a kérésnek **nincs jelentősége**
* Habár ez egy `patch` kérés, a törzsben semmit sem kell küldeni

Pl.:

```rest
PATCH /api/members/transfer-my-roles/652f85c4fc13ae3d596c7cde
Content-Type: application/json
x-auth-token: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo
x-auth-password: SuperSafe123
```

A válasz formátuma:
```json
{
  "_id": "652f85c4fc13ae3d596c7cde",
  "roles": ["member", "manager"]
} 
```
*A felruházott tag azonosítója, és jogai.*