# Kódolási stílusok

A kód írásakor nagyon fontos, hogy egységességre törekedjünk. 

Természetesen az adott programozási nyelv (pl.: TypeScript) és az adott technológia (pl.: React) konvencióit betartjuk. Tehát ügyeljünk például arra, hogy a függvények/változók neveihez a megfelelő `case`-t használjuk (pl.: JavaScriptben a függvények nevei `camelCase`-ben íródnak).

## Általános 
- Értelmes, leíró változóneveket használunk (kivéve ciklusváltozók)
- A `boolean` típusú változók `is`, `has` vagy `can` előtaggal kezdődnek
- A függvényeknek művelet leíró neveket adunk, általában igéket
- Kommentekből a lehető legkevesebbet hagyunk a kódban, csak ott, ahol nagyon muszáj
  - **Kikomentelt kódot ne hagyjunk!**
- Az összetartozó kódrészeket egy üres sorral választjuk el más összetartozó kódrészektől 
- Nem ismételjük meg ugyanazt a logikát többször, törekedszünk a kód újrahasználhatóságára
- A behúzásokhoz **`Tab` helyett négy `Space`-t használunk**
- Kisebb fájlokat könnyebb megérteni, így amit lehetséges, külön fájlokban tárolunk 

## A kód nyelve

A kódban angol nyelvű kifejezéseket használunk. A változónevek és a kommentek is angolul íródnak.  
A felhasználói felület - még akkor is, ha csak egy nyelv igényelt - mindenképp internacionalizálható kell legyen (`i18n`),
hogy a későbbiekben ne okozzon nagy problémát egy másik felhasználói nyelv támogatása.

## JavaScript & TypeScript

- `var`-t nem használunk, kizárólag `const` és `let`-t


A kód formázásához [Prettier](https://prettier.io/)-t használunk.   
 
- Sorok végét nem zárjuk pontosvesszőkkel
- Szimpla idézőjeleket (`''`) használunk a _string literal_-okhoz
- String összefűzés helyett `template string`-et használunk (pl.: ``` `Number of items: ${counter}` ```)
- Többsoros, vesszővel elválasztott szerkezetek esetén kirakjuk az utolsó vesszőt ([miért?](https://www.30secondsofcode.org/js/s/the-case-for-trailing-commas/))  
  pl. `object literal`: 
  ```js
  const config = {
    encoding: 'UTF-8',
    useFs: true,
  }
  ```
- Egyéb útmutatások lsd.: [TS Style Guide](https://ts.dev/style/)

Ezeken túl a Prettier alapbeállításaira támaszkodunk.  

A Prettier config-ja a következő (`.prettierrc.json`):
```json
{
    "tabWidth": 2,
    "trailingComma": "all",
    "semi": false,
    "singleQuote": true,
    "printWidth": 88
}
```
***Commit*olás előtt mindenképp formázzuk a forráskódot**, a legjobb az, hogyha beállítjuk a VSCode-ban a mentéskor való automatikus formázást!

## REACT sajátosságok

A React kódoknál tsx fájlokat használunk, erre minden TypeScript sajátosság és megkötés érvényes.

- A komponensek esetén `PascalCase` névkonvenciót használunk
- A függvényeket arrow function-nal hozzuk létre
- A változók létrehozásakor ***nem*** adjuk meg a változó típusát, ezek alól kivételek azon függvények, melyek ezt kötelezővé teszik (*itt rosszabb esetben `any` típust  használunk*):
 ```ts
//rossz deklarálás
  const myVariable: number = 22
//jó
  const myGoodVariable = 22
  ```
- Egyéni css osztály létrehozásakor ügyeljünk az osztály elnevezési konvenciójára, mely kebab-case alapú, azonban szerepelnie kell az adott komponens nevének benne:
  `counter-header-elements`, itt a `counter` a komponens neve, ezután pedig az osztály neve, mely utal a használati helyére
### Könyvtár felépítés
- Minden komponens az `src/components/` mappába kerülnek, ezen belül minden komponens kap egy saját mappát, (pl.: `Counter.tsx` komponens esetén: `Counter` mappába kerül a `.tsx` és a `css` fájl, ha van)
- A saját hook-ok az `src/hooks` mappába kerülnek, a hook elnevezésének formátuma szigorú: `useHook`, a use szóval kezdődjön és utána a funkcióra utaló neve (pl.: Data fetch-hez használt hook: `useData`)
- Azon szolgáltatások, melyek nem befolyásolják a weboldal felépítését az `src/services` mappába kerülnek (pl.: `crop-image.ts`, ez a megjelenést nem befolyásolja, de fontos, mivel kisebb méretre vágja a képet)

A könyvtár felépítési szabályait ***be kell tartani*** az átlátható kód érdekében.
