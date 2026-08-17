# iOS appoldalak migrációja: Vitya Labs → Vitya Games

Frissítve: 2026. augusztus 17.

## Jelenlegi állapot

A három teljes appkönyvtár átkerült a Vitya Games GitHub Pages repójába. A
következő deploy után az új marketing-, support- és privacy-oldalak a Games
domainen lesznek, a régi Labs marketingoldalak pedig átirányítanak az új
helyükre.

Az App Store-ban jelenleg használt Labs support- és privacy-végpontokat még nem
szabad törölni. Ezek változatlanul elérhetők maradnak addig, amíg minden App
Store Connect lokalizáció az új URL-ekre nem áll át, és az átállás élesben nincs
ellenőrizve.

| App | Új marketingoldal | Új support URL | Új privacy URL |
|---|---|---|---|
| Hotdog Runner | `https://vitya-games.github.io/hotdog-runner/` | `https://vitya-games.github.io/hotdog-runner/support.html` | `https://vitya-games.github.io/hotdog-runner/privacy.html` |
| Gull Flyer | `https://vitya-games.github.io/gull-flyer/` | `https://vitya-games.github.io/gull-flyer/support.html` | `https://vitya-games.github.io/gull-flyer/privacy.html` |
| Grill Sitter | `https://vitya-games.github.io/grill-sitter/` | `https://vitya-games.github.io/grill-sitter/support.html` | `https://vitya-games.github.io/grill-sitter/privacy.html` |

A megtartott régi végpontok ugyanezekkel a fájlnevekkel a
`https://vitya-labs.github.io/<app>/` útvonalakon érhetők el.

## Élesítési sorrend

1. Publikáld először a Vitya Games repót, és várd meg a GitHub Pages deploy
   sikerét.
2. Nyisd meg mind a kilenc új URL-t (marketing, support és privacy minden
   appnál), mobil és asztali nézetben is. Ellenőrizd a képeket, az App Store
   gombokat, a Games főoldalra visszavezető linket és a support e-mailt.
3. Publikáld a Vitya Labs repó átirányításait. Ellenőrizd, hogy a három régi
   marketing-URL az új Games oldalra visz, miközben mind a hat régi
   `support.html`/`privacy.html` URL továbbra is közvetlenül 200-as választ ad.
4. Csak ezután kezdd el az App Store Connect metaadatok átírását.

## App Store Connect átállítás apponként

Az alábbiakat külön-külön végezd el a Hotdog Runner, Gull Flyer és Grill Sitter
App Store Connect rekordján.

1. **Support URL:** Apps → az app → az aktuális iOS-verzió → Version
   Information. Minden lokalizációban írd át a Support URL mezőt a fenti új
   Games support URL-re, majd mentsd. Az Apple referenciája szerint a Support
   URL verziószintű mező és bármikor szerkeszthető.
2. **Marketing URL:** ha a mező ki van töltve, ugyanitt állítsd az új Games
   marketingoldalra. Ellenőrizd az összes lokalizációt.
3. **Privacy Policy URL:** Apps → az app → App Privacy → Privacy Policy → Edit.
   Írd be az új Games privacy URL-t minden használt lokalizációhoz, majd mentsd.
   Ehhez Account Holder, Admin, App Manager vagy Marketing szerepkör kell.
4. Az Apple jelenlegi dokumentációja szerint a privacy URL módosítása a
   következő appverzióval lép élesbe. Emiatt hozz létre és küldj be egy új
   verziót akkor is, ha az app binárisa nem változik érdemben. A Support és
   Marketing URL átírását érdemes ugyanebbe a verzióváltásba rendezni.
5. A verzió megjelenése után nyisd meg az app publikus App Store-oldalát minden
   támogatott storefrontban/lokalizációban. Kattintsd végig a support- és
   privacy-hivatkozásokat, és ellenőrizd, hogy kizárólag
   `vitya-games.github.io` URL-re jutnak.
6. Az Apple szerint egyes appinformáció-változások megjelenése akár 24 órát is
   igénybe vehet. A régi Labs URL-eket ezalatt mindenképp tartsd életben.

Apple dokumentáció:

- [Support URL és platformverzió-információ](https://developer.apple.com/help/app-store-connect/reference/app-information/platform-version-information)
- [Privacy policy URL szerkesztése](https://developer.apple.com/help/app-store-connect/manage-app-information/manage-app-privacy)
- [Szerkeszthető és lokalizálható mezők](https://developer.apple.com/help/app-store-connect/reference/app-information/required-localizable-and-editable-properties)
- [Appinformáció módosítása és a legfeljebb 24 órás átfutás](https://developer.apple.com/help/app-store-connect/create-an-app-record/view-and-edit-app-information)

## A régi Labs végpontok eltávolításának kapuja

A régi Labs support/privacy URL-ek csak akkor törölhetők, ha mindegyik pont
teljesül:

- mindhárom app legfrissebb verziója megjelent;
- minden App Store-lokalizáció Games URL-t tartalmaz;
- a publikus App Store-oldalakról induló linkeket ténylegesen végigkattintottad;
- eltelt legalább 24 óra az utolsó metaadat-változás óta;
- a három app repójában és a weboldal-repókban nincs régi URL-találat;
- nincs más külső felület (weboldal, social bio, sajtóanyag vagy QR-kód), amely
  még a régi végpontot használja.

Ellenőrző keresés a workspace gyökeréből:

```sh
rg -n "vitya-labs\.github\.io/(hotdog-runner|gull-flyer|grill-sitter)" \
  vitya-games vitya-labs \
  --glob '!**/.git/**' --glob '!**/node_modules/**'
```

## Végső Labs-takarítás

1. A sikeres App Store-ellenőrzés után cseréld a hat régi Labs
   support/privacy-oldalt ideiglenes átirányításra az új Games megfelelőjükre.
   Tartsd az átirányításokat legalább egy teljes kiadási cikluson át.
2. Ellenőrizd újra a publikus App Store-oldalakat és a fenti `rg` keresést.
3. Ezután törölhető a Labs repóból a három teljes könyvtár:
   `hotdog-runner/`, `gull-flyer/`, `grill-sitter/`.
4. Távolítsd el a Hotdog Runner régi Labs projektkártyáját és a hozzá tartozó
   `site-config.js` bejegyzést, ha a Labs főoldalon sem szeretnél Games appot
   megjeleníteni. Jelenleg a kártya részletező linkje már az új Games oldalra
   mutat, ezért az átmeneti időszakban nem tart fenn régi marketing-URL-t.
5. Publikáld a Labs takarítást, majd futtasd le még egyszer az összes URL- és
   vizuális ellenőrzést.

Az app binárisokban a workspace jelenlegi állapota alapján nincs beégetett
`vitya-labs.github.io` support vagy privacy URL; az átállás App Store Connect
metaadat-munka. A bundle ID-ket (`com.vityalabs...`) emiatt nem kell és nem is
szabad csak a webes márkamigráció kedvéért megváltoztatni.
