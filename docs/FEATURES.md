# Plander - Szolgálat tervező és naplózó

A kezdetleges, eddig eltervezett funkciók felsorolása. A még bizonytalan funkciók `?`-el vannak jelölve a dokumentációban.

## Design elemek
- Bejelentkező oldal
- Főoldal
- Szolgálattervező oldal
- Napló
- Beállítások
- Admin fül

## A komponensek funkciói
### Bejelentkezés
- Az összes felvett egyesület legördülő listában jelenik meg
- Az egyesület kiválasztása után a szolgálati igazolvány első pár számjegye (*ami az egyesületet, települést jelenti*) automatikusan beíródik.
- Remember me`?` funkció
- Sikeres bejelentkezés után átirányítás a kezdőlapra (*Főoldal*)
  
### Főoldal - Webes verzió!
- A képernyő baloldalán a menüpontok választhatóak ki
   - Admin esetén a beállítások fül is megjelenik
- A jobb oldalon egy üzenőfal van, ide mindenki(*vagy csak adott `role`*) írhat fontos közleményeket
- Aki a bejelentkezés idejében szolgálatban van, lássa ezt a főoldalon valahol

### Szolgálat tervezés
- Calendar kinézet, mindenki számára megtekinthető, de nem mindenki szerkesztheti
  - A napon belül az időintervallum is kiválasztható
- Az egyesület vezetője tervezhet szolgálatot havi szinten
- Az elkészült beosztást közzéteszi, melyről a többi tag értesül (*üzenőfal, tel: értesítés*)
  
### Napló
- A napló elérése nem korlátozott
- Kinézetről kép beküldve
- A szolgálat elején új bejegyzés nyitása, végén lezárás
- A lezárás után az egyszerű `user` ***nem módosíthatja*** azt már
- Az elkészült bejegyzések egyesével nyomtathatóak szükség esetén, erre egy sablon lesz létrehozva

### Beállítások
- téma állítása
- nyelv`?` állítása
- jelszómódosítás vagy akár teljes profil szerkesztése (*pl. új lakcím miatt*)`?`

### Admin fül
- A felhasználó jogosultságától függően tud itt dolgozni - sima `user`-nél nem jelenik meg
- Csoportvezető / egyesületvezető
  - Képes a saját egyesületében kezelni a tagokat (*felvenni, módosítani...*)
- Admin -***nem végleges név***
  - képes új egyesületek rögzítésére, amire senki más nem

## Technikai funkciók

### Új tag regisztrálása
 Az egyesület vezetője egy űrlap segítségével tud új tagokat felvenni, ilyenkor azonban még nem biztos, hogy az újonnan regisztrált tag rendelkezik *polgárőrségi igazolvánnyal*, ami az alapértelmezett jelszó. Ebben az esetben az egyesület vezetője állíthat egy ideiglenes jelszót, amit később meg kell változtatnia a tagnak. `?`