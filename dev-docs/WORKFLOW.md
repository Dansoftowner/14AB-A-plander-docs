# WORKFLOW

A projekt három fő részből áll: backend, frontend és mobil. Ezeket külön repositorykban kezeljük, 
és mindegyik repó neve a `14-AB-A-plander-` előtaggal kezdődik 
(`14-AB-A-plander-backend`, `14-AB-A-plander-frontend`, `14AB-A-plander-mobile`).

## GitHub Project használata

Ahhoz, hogy átfogóan tudjuk a megoldandó feladatokat rendszerezni, a [Vizsgaremek-14AB-A
](https://github.com/users/Dansoftowner/projects/1) nevű GitHub projekt felületét használjuk.  
**Itt hozzuk létre az issue-kat, nem közvetlenül a repókban!**

### Label-ek (címkék) használata

  A labeleket minden, a projekthez társított repository-ban eszerint a minta alapján kell beállítani, a fejlesztés és hibakeresés megkönnyítésének érdekében!

#### A label-ek és jelentéseik
-  `documentation` - Dokumentációkkal kapcsolatos megfigyelések jelölésére használható
-  `enhancement` - Új funkcióval kapcsolatos kérés
-  `bug` - Egy meglévő funkcióval felmerülő hiba/ bug
-  `wontfix`- A ki nem javítandó elem megjelölésére szolgál
-  `invalid` - A felmerült hiba érvénytelennek jelölve a fejlesztő által
-  `question` - Egyszerű felmerülő kérdés
-  `attention` - Az adott probléma további figyelmet igényel
-  `frontend` - A jelölővel a frontend-del kapcsolatos témákat jelöljük meg
-  `backend` - A backend-del kapcsolatos jelölésekre alkalmas
-  `mobile` - A mobilverzió azonosítására alkalmas jelölő

**A fentieken kívűl más label használata esetén nem biztosított a probléma megoldása!**

## Branch-ek
- `main`: ebbe a branch-be kerül a legújabb release, **ide közvetlenül sosem dolgozunk!**
- `dev`: ezt használjuk a "main-line" munkára, innen hajtunk végre *merge*-t a `main`-be, amikor elkészül a következő verzió

**Közvetlenül a `dev`-be sem *commit*olunk, mindenki ideiglenes branch-et hoz létre a saját módosításához.**
Amikor valaki végzett egy feladattal, akkor készít egy *pull request*-et (PR) a `dev` branch-be. ***(Fontos, hogy a PR-ban csak az adott feladattal kapcsolatos változtatások legyenek.)***
Ahhoz, hogy a *merge* megtörténhessen, minden esetben a *reviewer*-nek jóvá kell hagynia a módosításokat, de ha hibát vesz észre, élhet azzal a jogával is, hogy további módosításokat vár el a *pull request* feladójától.  
**Miután a *merge* megtörtént, az ideiglenes branch-et kitöröljük.**
> Ha az adott fejlesztést egy GitHub Issue keretében végezzük el, a GitHub automatikusan felajánlja
> az ideiglenes branch létrehozását, majd törlését is. A legtöbb esetben érdemes élni ezzel a lehetőséggel.

Minden új stabil verzió esetén létrehozunk egy GitHub Release-t.

### Branch elnevezési konvenciók
A branchek nevei a `kebab-case` elnevezési konvenciót követik, számokkal lehetőleg ne kezdődjenek.  
A branch nevének logikailag tükröznie kell a hivatását! (pl.: `implement-pdf-export`)

## Commit üzenetek
A *commit* üzeneteket angolul fogalmazzuk meg, az első szó nagy kezdőbetűs.
Az üzeneteket [`gitmoji`](https://gitmoji.dev/)-val kezdjük. Ügyeljünk arra, hogy redundás információt
ne tartalmazzon az üzenet (rossz példa: `:bug: Fix bug`).

Az üzenetek tömören, lényegre törően legyenek megfogalmazva (ez nyilvánvalóan csak akkor lehetséges, ha a *commit* tartalma logikailag koherens).
