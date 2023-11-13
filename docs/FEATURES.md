# Plander - Szolgálat tervező és naplózó

A Plander alkalmazás egy szolgálattervező és naplózó program, amely a polgárőrség munkáját hivatott megkönnyíteni azzal (több más funkció mellett), hogy míg az egyesületvezetők digitálisan tervezhetik meg a munkaszolgálatokat, addig az egyesületi tagok digitálisan látják és naplózhatják szolgálataikat.

A dokumentum a kezdetleges, eddig eltervezett funkciók felsorolását tartalmazza. A még bizonytalan funkciók `?`-el vannak jelölve a dokumentációban.

## Oldalak
- Bejelentkező oldal
- Főoldal
- Szolgálattervező oldal <!-- oldal és fül helyett oldal -->
- Szolgálati napló
- Beállítások oldal
- Tagok

## A komponensek funkciói
### Bejelentkező oldal
- A bejelentkezés egy felhasználónév-jelszó páros megadásával történik
- Az összes felvett egyesület legördülő listában jelenik meg
- Az egyesület kiválasztása után a szolgálati igazolvány első pár számjegye (*ami az egyesületet, települést jelenti*) automatikusan beíródik
- `Remember me` funkció
- Sikeres bejelentkezés után átirányítás a kezdőlapra (*Főoldal*)
- Bejelentkezést követően a felhasználó vagy **"egyszerű" tag**, vagy **egyesületvezető** jogokkal rendelkezik
  
### Főoldal - Webes verzió!
- A képernyő baloldalán a menüpontok választhatóak ki
   - Admin esetén az `Admin fül` fül is megjelenik
- A jobb oldalon egy üzenőfal van, ide mindenki írhat fontos közleményeket vagy egyszerű üzeneteket
- A tag bejelentkezés után ezen az oldalomn láthatja, ha aktuálisan szolgálatban van

### Főoldal - kisméret (*mobil*)
- Menüsáv jelenik meg, benne:
  - Menü felirat
  - Hamburgermenü, ami a teljes oldalra kinyílik és az összes elérhető menüpont megjelenik
- A kinyílt menü az egyik fenti sarokban lévő `x` gombbal lehet elrejteni, vagy ha kiválasztunk egy menüpontot
- A főoldalon alapesetben csak az üzenőfalon megjelenő friss hírek látszanak majd

### Szolgálattervező oldal
- Calendar kinézet, mindenki számára megtekinthető, de csak az egyesületvezető szerkesztheti 
  - Az aktuális napon belül az időintervallum is kiválasztható
- Az egyesület vezetője tervezhet szolgálatot havi szinten
- Az elkészült beosztást közzéteszi, melyről a többi tag értesül (*üzenőfal, tel: értesítés*)
  
### Szolgálati napló
- A napló elérése nem korlátozott
- Kinézetről kép [itt](https://github.com/Dansoftowner/14AB-A-Others/tree/main/UI_design) látható
- A szolgálat elején a szolgálatot teljesítő tag új bejegyzést hoz létre, amit a szolgálat végén zár le
- A lezárás után az egyszerű `tag` ***nem módosíthatja*** azt már, elírás esetén `egyesületvezetőre` kell
- Az elkészült bejegyzések egyesével nyomtathatóak szükség esetén, erre egy sablon lesz létrehozva
- Csak a legutóbbi 5 szolgálat tekinthető meg
- Az oldalon csak az adott egyesület jelentései jelennek meg

### Beállítások = Frontenden Profil fül 
- téma állítása
- nyelv állítása
- minden felhasználó tudja módosítani saját profiljának adatát (*pl. telszám, lakcím*)

### Tagok oldal
- A felhasználó jogosultságától függően tud itt dolgozni, a `user` meg tudja tekinteni az egyesületében lévő más tagokat
- Csoportvezető / egyesületvezető
  - Képes a saját egyesületében kezelni a tagokat (*felvenni, módosítani...*)

## Technikai funkciók

### Új tag regisztrálása
 Az egyesület vezetője egy űrlap segítségével tud új tagokat felvenni, ilyenkor azonban még nem biztos, hogy az újonnan regisztrált tag rendelkezik *polgárőrségi igazolvánnyal*. Ebben az esetben az egyesület vezetője állíthat egy ideiglenes jelszót, amit később meg kell változtatni a felhasználónak biztonsági okokból. Ha van igazolványa, akkor az az alap jelszó, melyet szintén érdemes megváltoztatni. A felhasználónevet minden tag szabadon választja.

 ### Napló internet nélkül probléma
Lehetővé kell tenni, hogy a szolgálatvégzés alatt is módosítható legyen a szolgálati napló. Nem feltétezhető mobilnet, emiatt offline állapotban is lehessen szerkeszteni a naplóbejegyzést, majd amikor van internetkapcsolat (*vélhetően a szolgálat végén*), ez töltődjön fel az adatbázisra. Alapesetben, amikor a szolgálatot ellátó személy rendelkezik internet kapcsolattal, a lezárás után töltődik fel a bejegyzés az adatbázisra.

### Profilok
Minden felhasználó hozzáfér saját személyes adataihoz, és beállításaihoz, amik a profiljához vannak kapcsolva. Ezeket a Profil(*=korábbi Beállítások*) fül alatt tekinthet meg.

### Bejelentkezés asztali webhelyen
***Felvetés*** A bejelentkező fülön az aktuálisan kiválasztott egyesület logója jelenik meg. Ebben az esetben ezt is adatbázisban kéne tárolni egységes méretben es formátumban.


