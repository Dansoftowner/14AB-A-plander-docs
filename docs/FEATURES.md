# Plander - Szolgálat tervező és naplózó

A kezdetleges, eddig eltervezett funkciók felsorolása. A még bizonytalan funkciók `?`-el vannak jelölve a dokumentációban.

## Design elemek
- Bejelentkező oldal
- Főoldal
- Szolgálattervező oldal
- Napló
- Beállítások
- Admin fül(ez az egyesületek elnökeinek szól)

## A komponensek funkciói
### Bejelentkezés
- Az összes felvett egyesület legördülő listában jelenik meg
- Az egyesület kiválasztása után a szolgálati igazolvány első pár számjegye (*ami az egyesületet, települést jelenti*) automatikusan beíródik.
- Remember me funkció
- Sikeres bejelentkezés után átirányítás a kezdőlapra (*Főoldal*)
  
### Főoldal - Webes verzió!
- A képernyő baloldalán a menüpontok választhatóak ki
   - Admin esetén az `admin fül` fül is megjelenik
- A jobb oldalon egy üzenőfal van, ide mindenki(*vagy csak adott `role`*) írhat fontos közleményeket
- Aki a bejelentkezés idejében szolgálatban van, lássa ezt a főoldalon valahol

### Főoldal - kisméret (*mobil*)
- Menüsáv jelenik meg, benne:
  - Menü felirat
  - Hamburgermenü, ami a teljes oldalra kinyílik és az összes elérhető menüpont megjelenik
- A kinyílt menü az egyik fenti sarokban lévő `x` gombbal lehet elrejteni, vagy ha kiválasztunk egy menüpontot
- A főoldal alapesetben csak az üzenőfalon megjelenő friss hírek látszanak majd

### Szolgálat tervezés
- Calendar kinézet, mindenki számára megtekinthető, de nem mindenki szerkesztheti
  - A napon belül az időintervallum is kiválasztható
- Az egyesület vezetője tervezhet szolgálatot havi szinten
- Az elkészült beosztást közzéteszi, melyről a többi tag értesül (*üzenőfal, tel: értesítés*)
  
### Napló
- A napló elérése nem korlátozott
- Kinézetről kép beküldve
- A szolgálat elején új bejegyzés nyitása, végén lezárás
- A lezárás után az egyszerű `user` ***nem módosíthatja*** azt már, elírás esetén magasabb szintű felhasználó kell
- Az elkészült bejegyzések egyesével nyomtathatóak szükség esetén, erre egy sablon lesz létrehozva
- Csak a legutóbbi 5 szolgálat tekinthető meg

### Beállítások = Frontenden Profil fül 
- téma állítása
- nyelv állítása
- minden felhasználó tudja módosítani saját profiljának adatát (*pl. telszám, lakcím*)

### Admin fül
- A felhasználó jogosultságától függően tud itt dolgozni - sima `user`-nél nem jelenik meg
- Csoportvezető / egyesületvezető
  - Képes a saját egyesületében kezelni a tagokat (*felvenni, módosítani...*)

## Technikai funkciók

### Új tag regisztrálása
 Az egyesület vezetője egy űrlap segítségével tud új tagokat felvenni, ilyenkor azonban még nem biztos, hogy az újonnan regisztrált tag rendelkezik *polgárőrségi igazolvánnyal*. Ebben az esetben az egyesület vezetője állíthat egy ideiglenes jelszót, amit később meg kell változtatni a felhasználónak biztonsági okokból. Ha van igazolvány akkor az az alap jelszó, melyet szintén érdemes megváltoztatni. Felhasználónév választható.

 ### Napló internet nélkül probléma
Lehetővé kell tenni, hogy szolgálatvégzés alatt is módosítható legyen a szolgálati napló. Nem feltétezhető mobilnet, emiatt offline állapotban is lehessen szerkeszteni a naplóbejegyzést, majd amikor van internet kapcsolat (*vélhetően a szolgálat végén*), ez töltődjön fel az adatbázisra. Alapesetben, amikor a szolgálatot ellátó személy rendelkezik internet kapcsolattal, a lezárás után töltődik fel a bejegyzés az adatbázisra.

### Profilok
Minden felhasználó hozzáfér saját személyes adataihoz,és beállításaihoz, amik a profiljához vannak kapcsolva. Ezeket a Profil(*=korábbi Beállítások*) fül alatt tehetimeg.


