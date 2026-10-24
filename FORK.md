# zadoli fork — loadN_name entitásból

Egyetlen eltérés az upstreamtől (slipx06/sunsynk-power-flow-card): a
`load.loadN_name` (N = 1..6) lehet **entitás-azonosító** is. Ha az érték egy
létező HA-entitás, és az állapota nem `unavailable`/`unknown`, a kártya az
entitás állapotát írja ki névként (ugyanaz a minta, mint a `loadN_icon`-nál).
Különben a szöveg marad. Kód: `src/index.ts`, `resolveLoadName`.

Mellékjavítás: `rollup.config.mjs` — a `typescript()` pluginnek explicit
`include`/`exclude` kell, különben a build csendben kihagyja a TS-t.

## Ágak

- `load-name-entity-v<verzió>` — az aktuális, élesben futó változat
  (most: `load-name-entity-v7.4.2`).
- `load-name-entity` — az eredeti, v7.3.x-re épült változat (archív).

## Ha a HACS frissítés felülírja (a nevek helyett entitás-id látszik)

```bash
git fetch upstream --tags
git checkout -b load-name-entity-v<új> v<új>
git cherry-pick <az előző load-name-entity-v* ág feat-commitja>
npm install && npm run build
gzip -9 -k -f dist/sunsynk-power-flow-card.js
git push -u origin load-name-entity-v<új>
```

Telepítés (kézzel, a HA gépen): a `dist/sunsynk-power-flow-card.js` és
`.js.gz` felülírja a
`/opt/homeassistant/config/www/community/sunsynk-power-flow-card/` alattiakat
(a `.gz`-t is, mert azt szolgálja ki a HA), utána böngészőben kemény
frissítés, a companion appban frontend-cache ürítés.
