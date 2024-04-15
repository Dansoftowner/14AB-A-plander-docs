# Végpontok

> A jegyzet folyamatosan bővül

### Áttekintés

[**Authentication (Autentikáció)**](#authentication-autentikáció)
| | | |
|-------|------------------------------------------------------|---------------------------------------------------|
| `POST` | [`/api/auth/`](#post-apiauth) | Bejelentkezés. |

[**Associations (Egyesületek)**](#associations-egyesületek)
| | | |
|-------|------------------------------------------------------|---------------------------------------------------|
| `GET` | [`/api/associations/`](#get-apiassociations) | Összes egyesület lekérdezése. |
| `GET` | [`/api/associations/{id}`](#get-apiassociationsid) | Egy adott egyesület adatainak lekérdezése. |
| `GET` | [`/api/associations/mine`](#get-apiassociationsmine) | Az adott tag egyesületének adatainak lekérdezése. |

[**Members (Tagok)**](#members-tagok)
| | | |
|----------|-------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|
| `GET` | [`/api/members/`](#get-apimembers) | Az adott tag egyesületébe tartozó összes tag lekérdezése. |
| `GET` | [`/api/members/{id}`](#get-apimembersid) | Egy adott tag adatainak lekérése az azonosítója alapján. |
| `GET` | [`/api/members/me`](#get-apimembersme) | Egy tag ezen keresztül tudja lekérdezni a saját adatait. |
| `GET` | [`/api/members/username/{username}`](#get-apimembersusernameusername) | Egy adott tag adatainak lekérése a felhasználóneve alapján. |
| `GET` | [`/api/members/register/{id}/{registrationToken}`](#get-apimembersregisteridregistrationtoken) | Egy meghívott tag elérhető adatainak lekérdezése. |
| `POST` | [`/api/members/`](#post-apimembers) | Egy tag meghívása az egyesületbe. |
| `POST` | [`/api/members/register/{id}/{registrationToken}`](#post-apimembersregisteridregistrationtoken) | Egy meghívott tag regisztrálása. |
| `POST` | [`/api/members/forgotten-password`](#post-apimembersforgotten-password) | Elfelejtett jelszó helyreállítása. |
| `POST` | [`/api/members/forgotten-password/{id}/{restorationToken}`](#post-apimembersforgotten-passwordidrestorationtoken) | Új jelszó beállítása az elfelejtett jelszó helyett. |
| `PATCH` | [`/api/members/me/credentials`](#patch-apimembersmecredentials) | Egy tag e-mail címének, felhasználónevének vagy jelszavának módosítása. |
| `PATCH` | [`/api/members/{id}`](#patch-apimembersid) | Egy tag adatainak módosítása. |
| `PATCH` | [`/api/members/me`](#patch-apimembersme) | Egy tag ezen keresztül tudja módosítani a saját adatait. |
| `PATCH` | [`/api/members/transfer-my-roles/{id}`](#patch-apimemberstransfer-my-rolesid) | Egyesületvezető jog átruházása. |
| `DELETE` | [`/api/members/{id}`](#delete-apimembersid) | Tagok törlése |

[**Assignments (Beosztások)**](#assignments-beosztások)
| | | |
|----------|-------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|
| `GET` | [`/api/assignments/`](#get-apiassignments) | Az adott tag egyesületébe tartozó összes beosztás lekérdezése. |

## Authentication (Autentikáció)

Az autentikáció [Json Web Token](https://jwt.io/)-ek (JWT) által történik.
A webalkalmazás esetében a JWT **sütiben** van eltárolva,
a natív alkalmazások viszont a token-t egy `x-plander-auth` http fejlécben kell elküldjék.
Ezért azon végpontoknál, ahol a `x-plander-auth` fejléc elvártnak van jelölve,
ott ez csak a natív alkalmazásokra vonatkozik,
hiszen a webböngésző automatikusan elküldi a **sütiben** tárolt JWT-t.

> **!!!** A fenti leírásban az eredeti elképzelés van megfogalmazva. A későbbiekben azonban úgy döntöttünk,
> hogy a token nem lesz sütiben tárolva. Ennek az oka a "third-party" sütik állítólagos megszüntetése
> [lásd itt](https://developers.google.com/privacy-sandbox/3pcd). **!!!**

**Speciális esetek, amikor nem JWT-nel történik az autentikáció**:

- Amikor egy meghívott tag regisztrál (ilyenkor az email-ben küldött URL-ben található **regisztrációs token**-nel történik a hitelesítés)
- Egy tag elfelejtett jelszavának megváltoztatásakor (ilyenkor az email-ben küldött URL-ben található **helyreállítási token**-nel történik a hitelesítés)

### `POST` `/api/auth`

A bejelentkezéshez szükséges végpont.

**A kérés formátuma:**  
Content-Type: `application/json`

- `associationId*`: a szervezet azonosítója
- `user*`: a felhasználónév vagy email cím
- `password*`: a tag jelszava

**A válasz formátuma:**

- A `token` az `x-plander-auth` fejlécben van biztosítva
- A válasz törzsében a bejelentkezett tag adatai kerülnek vissza

Pl.:

```rest
POST /api/auth

{
  "associationId": "652f7b95fc13ae3ce86c7cdf",
  "user": "ccolebeck0",
  "password": "mypassword23"
}
```

A válasz:

```
Content-Type: `application/json`
x-plander-auth: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo

{
  "_id": "652f866cfc13ae3ce86c7ce7",
  "isRegistered": true,
  "email": "bverchambre0@alibaba.com",
  "username": "gizaac0",
  "name": "Reizinger Szabolcs",
  "address": "Hungary, 7300 PillaFalva Maniel utca 12.",
  "idNumber": "589376QN",
  "phoneNumber": "+86 (120) 344-7474",
  "guardNumber": "08/0019/161373",
  "roles": [
    "member",
    "president"
  ]
}
```

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
        "certificate": "08/0001"
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
  "certificate": "08/0001"
}
```

### `GET` `/api/associations/mine`

Az adott tag egyesületének adatainak lekérdezése. (_Gyakorlatilag egy egyszerűsített változata az előzőleg bemutatott végpontnak, de ez a token-ből nyeri ki az id-t._)

**Required http headers:**

- `x-plander-auth` - a tagot azonosító token

Pl.:

```rest
GET /api/associations/mine
x-plander-auth: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo
```

A válasz formátuma:

```json
{
  "_id": "652f7b95fc13ae3ce86c7cdf",
  "name": "Baráti Polgárõr Egyesület",
  "location": "9081 Győrújbarát Fő u. 1",
  "certificate": "08/0001"
}
```

## Members (tagok)

#### Egy tag által megtekinthető más tagok adatai:

- `_id`
- `username`
- `guardNumber` (csak `president` ranggal)
- `name`
- `address` (csak `president` ranggal)
- `idNumber` (csak `president` ranggal)
- `email`
- `phoneNumber`
- `roles`
- `isRegistered` (ez a jellemző ugyan látható, de a nem megerősített tagok csak a `president` ranggal rendelkezők számára jelennek meg)

### `GET` `/api/members`

Az adott tag egyesületébe tartozó összes tag lekérdezése.

**Required http headers:**

- `x-plander-auth` - a tagot azonosító token

**Query parameters:**

- `offset` - elhagyandó dokumentumok száma (alapértelmezett: 0)
- `limit` - maximum megjelenítendő dokumentumok száma (alapértelmezett: 10, maximum: 40)
- `projection`
  - `lite` (alapértelmezett): csak az `_id`, `username`, `name`, `email`, `phoneNumber`, `isRegistered` és `roles` mezők megjelenítése
  - `full`: az összes (a tag által megtekinthető, lsd: [fent](#egy-tag-által-megtekinthető-más-tagok-adatai)) mező megjelenítése
- `orderBy`: a mező neve, ami alapján a dokumentumokat rendezni kívánjuk, a csökkenő sorrendet `-` karakter jelöli (alapértelmezett: `name`)
- `q`: a `name` mező alapján való keresés (_case-insensitive_)

Pl.:

```rest
GET /api/members?offset=10&limit=5&projection=lite&orderBy=name
x-plander-auth: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo
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
    "isRegistered": true,
    "roles": ["member", "president"]
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

- `x-plander-auth` - a tagot azonosító token

**Query parameters:**

- `projection`
  - `lite` (alapértelmezett): csak az `_id`, `username`, `name`, `email`, `phoneNumber`, `isRegistered` és `roles` mezők megjelenítése
  - `full`: az összes (a tag által megtekinthető, lsd: [fent](#egy-tag-által-megtekinthető-más-tagok-adatai)) mező megjelenítése

Ha a megjelenítendő tag létezik az azonosítója alapján, **de nem ugyanabba az egyesületbe tartozik**, mint a kérés küldője (akit a _token_ azonosít), akkor az adatai nem kérhetőek le.

> Ha az azonosító alapján megjelenítendő tag ugyanaz, mint aki a kérést küldte, akkor jogosult az egyébként rangja alapján nem feltétlenül látható mezők megtekintésére is.

Pl.:

```rest
GET /api/members/652f85c4fc13ae3d596c7cdf?projection=lite
x-plander-auth: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo
```

A válasz formátuma:

```json
{
  "_id": "652f85c4fc13ae3d596c7cdf",
  "username": "ccolebeck0",
  "name": "András Kovács",
  "email": "ahilhouse0@disqus.com",
  "phoneNumber": "+34 (829) 635-5692",
  "isRegistered": true,
  "roles": ["member", "president"]
}
```

### `GET` `/api/members/me`

Egy tag ezen keresztül tudja lekérdezni a saját adatait. (_Gyakorlatilag egy egyszerűsített változata az [előzőleg bemutatott végpontnak](#get-apimembersid), de ez a token-ből nyeri ki az id-t._)

### `GET` `/api/members/username/{username}`

Egy adott tag adatainak lekérése a felhasználóneve alapján.

**Parameters:**

- `username` - a lekérendő tag felhasználóneve

**Required http headers:**

- `x-plander-auth` - a tagot azonosító token

**Query parameters:**

- `projection`
  - `lite` (alapértelmezett): csak az `_id`, `username`, `name`, `email`, `phoneNumber`, `isRegistered` és `roles` mezők megjelenítése
  - `full`: az összes (a tag által megtekinthető, lsd: [fent](#egy-tag-által-megtekinthető-más-tagok-adatai)) mező megjelenítése

Mivel a tag felhasználóneve csak az egyesületen belül egyedi, a kérés küldője (akit a _token_ azonosít) ugyanabba az egyesületbe kell tartozzon, különben az adatok nem kérhetőek le.

> Ha az felhasználónév alapján megjelenítendő tag ugyanaz, mint aki a kérést küldte, akkor jogosult az egyébként rangja alapján nem feltétlenül látható mezők megtekintésére is.

Pl.:

```rest
GET /api/members/username/ccolebeck0?projection=lite
x-plander-auth: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo
```

A válasz formátuma:

```json
{
  "_id": "652f85c4fc13ae3d596c7cdf",
  "username": "ccolebeck0",
  "name": "András Kovács",
  "email": "ahilhouse0@disqus.com",
  "phoneNumber": "+34 (829) 635-5692",
  "isRegistered": true,
  "roles": ["member", "president"]
}
```

### `GET` `/api/members/register/{id}/{registrationToken}`

Ez a végpont egy **meghívott tag** egyesületvezető által megadott adatait adja vissza. _Csak megerősítetlen, még regisztráció előtt álló tagok esetében releváns._

Tipikusan azután lesz ez a végpont meghívva, amiután a meghívott tag megnyitotta az e-mail címére kapott **regisztrációs linket**.

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
  "isRegistered": false
}
```

(Ha az egyesületvezető egyéb adatokat is megadott a tag meghívásakor az e-mail címen kívül, akkor természetesen azok is szerepelnek a válasz törzsében.)

### `POST` `/api/members`

Az **egyesületvezető ranggal rendelkező** tagok ezen a végponton kereszttül
tudják **meghívni** a további tagokat.

**Required http headers:**

- `x-plander-auth` - a tagot azonosító token

**Kérés formátuma:**  
Content-Type: `application/json`

- `email*`
- _`guardNumber`_
- _`name`_
- _`address`_
- _`idNumber`_
- _`phoneNumber`_

Mivel az egyesületvezetőtől nem várható el az, hogy ő maga adja meg a regisztrálandó tag összes adatát, neki **csak az e-mail címet** kell kötelezően megadnia.

A felhasználónevet és jelszót viszont kizárólag csak a tag tudja megadni később (addig ún. _unverified/megerősítetlen_ állapoban van rögzítve az adatbázisban).

**A végpont meghívásakor a meghívott tag automatikusan e-mailt kap egy regisztrációs linkkel, amin keresztül később véglegesíteni tudja magát.**

Pl.:

```rest
POST /api/members/
Content-Type: application/json
x-plander-auth: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo

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
  "isRegistered": false
}
```

### `POST` `/api/members/register/{id}/{registrationToken}`

A **meghívott tagok** ezen a végponton keresztül tudnak **regisztrálni** és a hiányzó adataikat pótolni.

Tipikusan azután lesz ez a végpont meghívva, amiután a meghívott tag megnyitotta az e-mail címére kapott **regisztrációs linket**.

**Parameters:**

- `id` - a tag azonosítója
- `registrationToken` - a **regisztrációs token**

**Kérés formátuma:**  
Content-Type: `application/json`

- _`username_`\*
- _`password_`\*
- _`guardNumber`_
- _`name_`\*
- _`address_`\*
- _`idNumber_`\*
- _`phoneNumber_`\*

Pl.:

```rest
POST /api/members/register/652f85c4fc13ae3d596c7cde/4vcmk1uzezrx7uc9bb5a19iu0cbsgd
Content-Type: application/json

{
  "username": "superguard01",
  "password": "SuperSafe@41a",
  "guardNumber": "4148009",
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
  "guardNumber": "4148009",
  "name": "Horváth Csaba",
  "address": "9029 Csontvár Sonka Utca 5.",
  "idNumber": "594771CQ",
  "phoneNumber": "+256 (776) 361-0286",
  "isRegistered": true
}
```

### `POST` `/api/members/forgotten-password`

Ez a végpont használható arra, hogy a tagok helyreállítsák az elfelejtett jelszavukat.

**Kérés formátuma:**  
Content-Type: `application/json`

- _`associationId`_ - az egyesület azonosítója
- _`email`_ - a jelszavát helyreállítani kivánó tag e-mail címe

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

Tipikusan azután lesz ez a végpont meghívva, amiután a tag megnyitotta az e-mail címére kapott **helyreállítási linket**.

**Parameters:**

- `id` - a tag azonosítója
- `restorationToken` - a helyreállítási token

**Kérés formátuma:**  
Content-Type: `application/json`

- _`password`_ - az egyesület azonosítója

Pl.:

```rest
POST /api/members/forgotten-password/652f85c4fc13ae3d596c7cde/rbzdaewl4tc74xiu5tdd
Content-Type: application/json

{
  "password": "IWontForgetItAgain123"
}
```

A válasz formátuma:  
Status: `204`

### `PATCH` `/api/members/me/credentials`

A tagok ezen a végponton keresztül tudják megváltoztatni a felhasználónevüket és/vagy e-mail címüket, illetve jelszavukat.

**Required http headers:**

- `x-plander-auth` - a tagot azonosító token
- `x-current-pass` - **mivel ez egy kockázatos művelet, az aktuális jelszó újbóli megadása kötelező, a token nem elég**

**Kérés formátuma:**  
Content-Type: `application/json`

- `username` - az új felhasználónév (**_egyesületen belül egyedinek kell lennie_**)
- `password` - az új jelszó

Értelemszerűen csak akkor van a kérésnek értelme, ha a felhasználónév és/vagy jelszó meg van adva.

Pl.:

```rest
PATCH /api/members/credentials/652f85c4fc13ae3d596c7cde
Content-Type: application/json
x-plander-auth: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo
x-current-pass: OldPassword123

{
  "password": "NewPassword"
}
```

A válasz formátuma:

```json
{
  "_id": "652f85c4fc13ae3d596c7cde",
  "updated": ["password"]
}
```

### `PATCH` `/api/members/{id}`

Egy **egyesületvezető** ezen a végponton keresztül tudja módosítani a **megerősítetlen** tagok adatait, illetve egy tag a saját adatait.

**Parameters:**

- `id` - a módosítandó tag azonosítója

**Required http headers:**

- `x-plander-auth` - a tagot azonosító token

A végpont működéséről a következőket mondhatjuk el:

- Ha a módosítandó tag létezik az azonosítója alapján, **de nem ugyanabba az egyesületbe tartozik**, mint a kérés küldője (akit a _token_ azonosít), akkor az adatai nem kérhetőek le.

- Ha a kérés küldője **nem egyesületvezető**, egy **másik tag** adatait **nem módosíthatja**.

- Ha a kérés küldője nem egyesületvezető, de **megegyezik** az azonosítója alapján a **módosítandó taggal**, akkor az adatait módosíthatja.

- Ha a kérés küldője **egyesületvezető**, **csak megerősítetlen** (`unverified`) tag adatait módosíthatja.  
  Kívételt jelent:

  - `preferences` - az adott tag egyéni beállításait csak az adott tag módosíthatja

- **A rangokat nincs lehetőség ezen a végponton keresztül módosítani.**

**Kérés formátuma:**  
Content-Type: `application/json`

- _`username`_
- _`password`_
- _`guardNumber`_
- _`name`_
- _`address`_
- _`idNumber`_
- _`phoneNumber`_
- _`preferences`_

Pl.:

```rest
PATCH /api/members/652f85c4fc13ae3d596c7cde
Content-Type: application/json
x-plander-auth: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo

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
  "guardNumber": "4148009",
  "name": "Horváth Csaba",
  "address": "7200 Igazváros Valóság Utca 5",
  "idNumber": "1232IQ",
  "phoneNumber": "+1020113301",
  "isRegistered": true
}
```

### `PATCH` `/api/members/me`

Egy tag ezen keresztül tudja módosítani a saját adatait. (_Gyakorlatilag egy egyszerűsített változata az [előzőleg bemutatott végpontnak](#patch-apimembersid), de ez a token-ből nyeri ki az id-t._)

### `PATCH` `/api/members/transfer-my-roles/{id}`

Az **egyesületvezető** ezen a végponton keresztül tud felruházni *egyszerű tag*ot _egyesületvezető ranggal_ (`president`).

**Parameters:**

- `id` - a felruházni kivánt tag azonosítója

**Query parameters:**

- `copy` - ha értéke `true`, akkor az tag rangja továbbra is megmarad, ha éréke `false`, akkor a tag rangja törlődik, és "átszáll" a felruházott tagra (alapértelmezett érték: `false`)

**Required http headers:**

- `x-plander-auth` - a tagot azonosító token
- `x-current-pass` - **mivel ez egy kockázatos művelet, az aktuális jelszó újbóli megadása kötelező, a token nem elég**

A végpont működéséről a következőket mondhatjuk el:

- Ha a módosítandó tag létezik az azonosítója alapján, **de nem ugyanabba az egyesületbe tartozik**, mint a kérés küldője (akit a _token_ azonosít), akkor a kérés nem érvényes.
- Ha a kérést küldő tag **nem egyesületvezető**, a kérésnek **nincs jelentősége**
- Habár ez egy `patch` kérés, a törzsben semmit sem kell küldeni

Pl.:

```rest
PATCH /api/members/transfer-my-roles/652f85c4fc13ae3d596c7cde
Content-Type: application/json
x-plander-auth: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo
x-current-pass: SuperSafe123
```

A válasz formátuma:

```json
{
  "_id": "652f85c4fc13ae3d596c7cde",
  "roles": ["member", "president"]
}
```

_A felruházott tag azonosítója, és jogai._

### `DELETE` `/api/members/{id}`

Az **egyesületvezető** ezen a végponton keresztül tudja eltávolítani az adott tagot az egyesületéből.

**Parameters:**

- `id` - a kitörlendő tag azonosítója

**Required http headers:**

- `x-plander-auth` - a tagot azonosító token
- `x-current-pass` - **mivel ez egy kockázatos művelet, az aktuális jelszó újbóli megadása kötelező, a token nem elég**

A végpont működéséről a következőket mondhatjuk el:

- Az egyesületvezető magát csak akkor tudja kitörölni, ha van más egyesületvezető az egyesületben
- Az egyesületvezető más egyesületvezetőket (ha vannak) nem tud kitörölni

Pl.:

```rest
DELETE /api/members/652f85c4fc13ae3d596c7cde
x-plander-auth: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo
x-current-pass: SuperSafe123
```

A válasz formátuma:

```json
{
  "_id": "652f85c4fc13ae3d596c7cde",
  "associationId": "652f7b95fc13ae3ce86c7ce3",
  "username": "lbaldery4",
  "guardNumber": "9487701",
  "name": "Váczi Károly",
  "address": "0 Marquette Hill",
  "idNumber": "866925HP",
  "email": "tbrucker4@umich.edu",
  "phoneNumber": "+84 (728) 209-0572",
  "roles": ["member", "president"]
}
```

## Assignments (Beosztások)

### `GET` `/api/assignments`

Egy tag ezen a végponton keresztül tudja lekérni az **egyesülethez tartozó** összes beosztást.

**Required http headers:**

- `x-plander-auth` - a tagot azonosító token

**Query parameters:**

- `start` - a dátum, amitől kezdve a beosztásokat látni akarjuk
- `end` - a dátum, ameddig a beosztásokat látni akarjuk
  > Ha a `start` és az `end` paraméter nincs megadva, akkor alapértelmezetten az aktuális hónap tartománya kerül alkalmazásra.
- `projection`
  - `lite` (alapértelmezett): csak az `_id`, `title`, `start` és `end` mezők megjelenítése
  - `full`: az összes mező megjelenítése
- `orderBy`: a mező neve, ami alapján a dokumentumokat rendezni kívánjuk, a csökkenő sorrendet `-` karakter jelöli (alapértelmezett: `start`)

Pl.:

```rest
GET /api/assignments?start=2022-01-01&end=2022-01-20&projection=full
x-plander-auth: eyJhbGciOiJIUzI1NiJ9.e30.ZRrHA1JJJW8opsbCGfG_HACGpVUMN_a9IV7pAx_Zmeo
```

A válasz:

```json
{
  "metadata": {
    "start": "2022-01-01",
    "end": "2022-01-20"
  },
  "items": [
    {
      "_id": "652fc6f2fc13ae3c0c6c8ab5",
      "title": "Főttcéklás laktanya őrzés",
      "location": "Quisque porta volutpat erat.",
      "start": "2022-01-02T09:06:36.000Z",
      "end": "2022-01-02T10:06:36.000Z",
      "assignees": [
        {
          "_id": "652f866cfc13ae3ce86c7ce7",
          "name": "Horváth Csaba"
        }
      ]
    },
    ... (more items)
  ]
}
```
