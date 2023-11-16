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
- Az egyesület kiválasztása után a szolgálati igazolvány első pár számjegye (_ami az egyesületet, települést jelenti_) automatikusan beíródik
- `Remember me` funkció
- Sikeres bejelentkezés után átirányítás a kezdőlapra (_Főoldal_)
- Bejelentkezést követően a felhasználó vagy **"egyszerű" tag**, vagy **egyesületvezető** jogokkal rendelkezik
- ["Elfelejtett jelszó helyreállítása" funkció](#elfelejtett-jelszó-helyreállítása)

### Főoldal - Webes verzió!

- A képernyő baloldalán a menüpontok választhatóak ki
  - Admin esetén az `Admin fül` fül is megjelenik
- A jobb oldalon egy üzenőfal van, ide mindenki írhat fontos közleményeket vagy egyszerű üzeneteket
- A tag bejelentkezés után ezen az oldalomn láthatja, ha aktuálisan szolgálatban van

### Főoldal - kisméret (_mobil_)

- Menüsáv jelenik meg, benne:
  - Menü felirat
  - Hamburgermenü, ami a teljes oldalra kinyílik és az összes elérhető menüpont megjelenik
- A kinyílt menü az egyik fenti sarokban lévő `x` gombbal lehet elrejteni, vagy ha kiválasztunk egy menüpontot
- A főoldalon alapesetben csak az üzenőfalon megjelenő friss hírek látszanak majd

### Szolgálattervező oldal

- Calendar kinézet, mindenki számára megtekinthető, de csak az egyesületvezető szerkesztheti
  - Az aktuális napon belül az időintervallum is kiválasztható
- Az egyesület vezetője tervezhet szolgálatot havi szinten
- Az elkészült beosztást közzéteszi, melyről a többi tag értesül (_üzenőfal, tel: értesítés_)

### Szolgálati napló

- A napló elérése nem korlátozott
- Kinézetről kép [itt](https://github.com/Dansoftowner/14AB-A-Others/tree/main/UI_design) látható
- A szolgálat elején a szolgálatot teljesítő tag új bejegyzést hoz létre, amit a szolgálat végén zár le
- A lezárás után az egyszerű `tag` **_nem módosíthatja_** azt már, elírás esetén `egyesületvezetőre` kell
- Az elkészült bejegyzések egyesével nyomtathatóak szükség esetén, erre egy sablon lesz létrehozva
- Csak a legutóbbi 5 szolgálat tekinthető meg
- Az oldalon csak az adott egyesület jelentései jelennek meg

### Beállítások = Frontenden Profil fül

- téma állítása
- nyelv állítása
- minden felhasználó tudja módosítani saját profiljának adatát (_pl. telszám, lakcím_)

### Tagok oldal

- A felhasználó jogosultságától függően tud itt dolgozni, a `user` meg tudja tekinteni az egyesületében lévő más tagokat
- Csoportvezető / egyesületvezető
  - Képes a saját egyesületében kezelni a tagokat (_felvenni, módosítani..._)

## Technikai funkciók

### Új tag regisztrálása

Az egyesület vezetője egy űrlap segítségével tud új tagokat felvenni,
de **csak a meghívandó tag e-mail címét köteles megadnia.** A meghívott tag erre az e-mail címre kap egy **regisztrációs linket**, amin keresztül meg tudja adni a személyes adatait, illetve a **felhasználónevét és jelszavát**. A még nem regisztrált, csak meghívott tag ún. "megerősítetlen" (`unverified`) állapotban van.

Az egyesületvezetője is megadhat bizonyos személyes adatokat a tag meghívásakor, de ezt a tag felülírhatja, illetve a **felhasználónevet/jelszót csak és kizárólag a meghívott tag adhatja meg a regisztrációkor**.

### Napló internet nélkül probléma

Lehetővé kell tenni, hogy a szolgálatvégzés alatt is módosítható legyen a szolgálati napló. Nem feltétezhető mobilnet, emiatt offline állapotban is lehessen szerkeszteni a naplóbejegyzést, majd amikor van internetkapcsolat (_vélhetően a szolgálat végén_), ez töltődjön fel az adatbázisra. Alapesetben, amikor a szolgálatot ellátó személy rendelkezik internet kapcsolattal, a lezárás után töltődik fel a bejegyzés az adatbázisra.

### Profilok

Minden felhasználó hozzáfér saját személyes adataihoz, és beállításaihoz, amik a profiljához vannak kapcsolva. Ezeket a Profil(_=korábbi Beállítások_) fül alatt tekinthet meg.

#### E-mail cím megváltoztatása

Ha egy tag meg akarja változtatni az e-mail címét, akkor az új címére kap egy **verifikációs linket**. Miután ezt a linket megnyitotta, az e-mail címe frissül az adatbázisban. Egészen addig viszont a régi e-mail címe megmarad.

#### Felhasználónév vagy jelszó megváltoztatása

Ha egy tag meg akarja változtatni a felhasználónevét vagy jelszavát, akkor ezt **csak azzal a feltétellel teheti meg**, hogy a **régi jelszót is meg kell adnia**.

### Bejelentkezés asztali webhelyen

A bejelentkező fülön az aktuálisan kiválasztott egyesület logója jelenik meg.

### Elfelejtett jelszó helyreállítása

Ha egy tag helyre akarja állítani az elfelejtett jelszavát, akkor a bejelentkező lapon található "Elfelejtette a jelszavát?" linkre kattintva egy mezőben megadhatja a fiókjához tartozó e-mail címét.

Ha ez az e-mail cím valóban hozzá van rendelve az adott egyesületben a taghoz, akkor erre az e-mail címre egy **helyreállítási link** kiküldésre kerül.

A helyreállítási linkre kattintva a tag egy űrlapban megadhatja az új jelszavát (jelszó, jelszó megerősítési mezőkkel).
