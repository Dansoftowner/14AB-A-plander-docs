# Dokumentáció készítése

A dokumentációk megírása `Markdown` formátumban történik.  

## Struktúra
A projekt dokumentációja a [plander-docs](https://github.com/Dansoftowner/14AB-A-plander-docs) repository-ban található.

A könyvtár-struktúra a következő:
- `/dev-docs`: kifejezetten a fejlesztéssel kapcsolatos útmutatások, leírások (code-style, workflow stb.)
- `/docs`: a program működésével, felépítésével és használatával kapcsolatos dokumentációk 

## Nyelv
A dokumentáció magyar nyelven készül, de a későbbiekben nem lehet kizárni az angol nyelvű dokumentumok elkészültét.

## Fájl elnevezési konvenciók
Habár maga a dokumentáció magyar nyelvű, minden másban az angol nyelvet igyekszünk használni. Ebből fakadóan a `markdown` fájlok neveit is angolul fogalmazzuk meg, és az `UPPER_SNAKE_CASE` elnevezési konvenciót alkalmazzuk.  
Pl. `MY_DOCUMENT.md` _(az `.md` kiterjesztést viszont kis betűvel írjuk)._

## Egyéb erőforrások
Előfordulhat, hogy a dokumentációhoz más erőforrások (pl.: képek) is szükségesek. Ezeket minding a dokumentációs fájl könyvtárában található `/assets` alkönyvtárba helyezzük el.

## Markdown használati utasítások
Mivel a dokumentációk megírásában is egységre törekedünk, bizonyos `markdown` jelölőkről is közös megállapodás született.

### Bold, Italic
A félkövér, illetve dőlt betűstílus jelölésére a `_` helyett `*`-t használunk.  
Pl.:
```md
**bold** *italic*
```

### Unordered list
A felsorolások esetében `*` helyett `-`-t használunk.  
Pl.: 
```md
- Item 1
- Item 2
```

### Code snipets

A szakmai/idegen nyelvű kifejezések és a beszúrt kódrészletek legyenek jelölve, ahol lehetséges.  
Pl.: `Buffer.concat()`
```md
`Buffer.concat()`
```

A hosszabb kódrészletek beszúrása esetén jelöljük az adott programozási/leíró nyelv típusát. Pl.:
```md
    ```js
    console.log('Running')
    ```
```

### HTML tags
Amennyiben lehetséges, **kerüljük a HTML tagek használatát a markdown fájlokban!**

---

*Minden egyéb markdown jelölő szabadon használható, hivatalos útmutató [itt](https://www.markdownguide.org/basic-syntax/) található.*
