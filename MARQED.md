# MarQed fork — wat hier afwijkt van upstream

Fork van **[jaredrhod/barehands](https://github.com/jaredrhod/barehands)**
(AGPL-3.0-or-later). Alle eer voor het origineel gaat naar Jared
Rhodenizer. Fork-punt: tag `fork-point-f1f8c9e`.

Upstream bijwerken: `git fetch upstream && git merge upstream/main`.

## Waarom deze fork bestaat: het bord haalde zijn hersenen van internet

Upstream laadt bij élke start drie dingen van externe servers:

| bron | wat | omvang |
|---|---|---|
| `cdn.jsdelivr.net` | three.js 0.160.0 + twee addons | 1,4 MB |
| `cdn.jsdelivr.net` | MediaPipe tasks-vision 0.10.14 + WASM | 18,5 MB |
| `storage.googleapis.com` | het `hand_landmarker`-model | 7,5 MB |

Dat betekent: geen internet is geen bord, en "welke versie draait er"
is een vraag die het CDN beantwoordt, niet de repo. Voor een installatie
die op een tweede machine reproduceerbaar moet zijn, is dat geen basis.

## Wat er is veranderd

Alle drie zijn **gevendord** in `vendor/`, op de exacte versies die
upstream noemde, en `stage.html` wijst er nu naar. Er staat geen enkele
externe URL meer in de pagina. `server.py` geeft `.mjs` en `.wasm` een
expliciet content-type, want een module die Chrome als
`application/octet-stream` binnenkrijgt, wordt geweigerd — en welk type
je krijgt hing af van de `/etc/mime.types` van de machine.

```
vendor/three/three.module.js                        1,3 MB
vendor/three/addons/loaders/GLTFLoader.js           108 KB
vendor/three/addons/utils/BufferGeometryUtils.js     32 KB   (GLTFLoader trekt hem mee)
vendor/three/addons/environments/RoomEnvironment.js   4 KB
vendor/mediapipe/vision_bundle.mjs                  136 KB
vendor/mediapipe/wasm/*                              18 MB
vendor/models/hand_landmarker.task                  7,5 MB
```

## Wat hier NIET gemeten is

De server, het `present`-werkwoord (`bin/board.sh`, geeft 204),
`bin/board-state.sh` en het serveren van alle vendor-bestanden met de
juiste content-types zijn geverifieerd. **De gebaren zelf niet**: die
vragen een webcam, Chrome en een mens die zijn hand beweegt. Dat is een
handmatige proef; hij staat nog open.

## Licentie

Ongewijzigd: **AGPL-3.0-or-later**. De gevendorde bibliotheken houden hun
eigen licentie: three.js (MIT), MediaPipe tasks-vision (Apache-2.0), het
hand_landmarker-model (Apache-2.0).
