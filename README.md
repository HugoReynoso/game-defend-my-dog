# 🐶 Defend My Dog

Un puzzle game mobile-first originale: disegna una sola linea, trasformala in una barriera fisica e proteggi il cane da api, spuntoni e rocce fino allo scadere del timer.

## 🎮 Demo online

**[Gioca a Defend My Dog](https://hugoreynoso.github.io/game-defend-my-dog/)**

Il gioco è ottimizzato per smartphone in modalità portrait, ma supporta anche mouse e desktop. È installabile come PWA.

## Funzionalità

- 20 livelli progressivi e data-driven, distribuiti in 5 mondi
- difficoltà Facile, Media e Difficile con scenari e pericoli crescenti
- linea fisica continua con smoothing, limite di inchiostro e collisioni Matter.js
- sciami aggressivi con inseguimento, manovre laterali attorno alle difese, ricerca dei varchi e pressione sulle barriere mobili
- rocce dinamiche e spuntoni
- valutazione da 1 a 3 stelle, monete e sblocco progressivo
- salvataggio robusto in `localStorage`
- grafica cartoon originale disegnata via Canvas
- vibrazione, effetti sonori sintetizzati e particelle leggere
- PWA offline-ready, safe area iPhone e avviso rotazione
- retry immediato senza ricaricare la pagina
- pulsante Home sempre disponibile durante il livello
- riserva d'inchiostro più che raddoppiata per linee lunghe e soluzioni geometriche elaborate
- sistema di appoggi fisici: zero appoggi significa difesa mobile, un appoggio crea una leva, due appoggi stabilizzano la protezione
- appoggi rilevati in modo invisibile: la stabilità emerge direttamente dal comportamento fisico
- con zero o un solo appoggio la barriera cade con gravità naturale e si posa su cane, terreno e piattaforme
- due estremità ben appoggiate rendono la barriera stabile; le api continuano a premere sulle difese mobili
- piattaforme sospese sopra acqua e lava rendono pericolosa una protezione chiusa ma non ancorata
- acqua e lava capaci di eliminare il cane nei livelli ambientali

## Installazione e sviluppo

Richiede Node.js 20 o superiore.

```bash
npm install
npm run dev
```

Apri l'indirizzo indicato da Vite. Per provare da un telefono sulla stessa rete, usa l'indirizzo di rete mostrato nel terminale.

## Build di produzione

```bash
npm run build
npm run preview
```

La build viene generata in `dist/`. Il workflow in `.github/workflows/deploy.yml` pubblica automaticamente su GitHub Pages a ogni push su `main`.

## Struttura del progetto

```text
src/
├── game/
│   ├── config/       # dimensioni e palette condivisa
│   ├── entities/     # cane e api
│   ├── levels/       # tipi e dati dei 10 livelli
│   ├── scenes/       # menu, selezione, gioco, vittoria e sconfitta
│   ├── systems/      # disegno, audio e salvataggio
│   └── ui/           # HUD, timer e barra inchiostro
├── main.ts           # configurazione Phaser
└── style.css         # viewport, safe area e orientamento
public/               # manifest, icona e service worker
```

## Creare un nuovo livello

1. Apri `src/game/levels/index.ts`.
2. Aggiungi un oggetto conforme a `LevelData` nell'array `levels`.
3. Configura posizione del cane, piattaforme, alveari, tempo, inchiostro e soglie stelle.
4. Aggiungi facoltativamente `walls`, `spikes` e `rocks`.
5. Aggiorna il limite dei livelli nell'interfaccia e nel metodo `SaveManager.complete` se superi il livello 20.

Esempio di alveare:

```ts
{ x: 70, y: 155, beeCount: 6, beeType: 'normalBee', spawnDelay: 450 }
```

## Aggiungere un hazard

1. Definisci i suoi dati in `src/game/levels/types.ts`.
2. Crea l'entità in `src/game/entities/` mantenendo separati corpo Matter e rendering.
3. Istanziala in `GameScene.create()` tramite i dati del livello.
4. Assegna un `label` Matter specifico e aggiungilo alla regola di sconfitta nella collisione.
5. Implementa sempre `destroy()` o la pulizia allo shutdown della scena.

## Aggiungere un personaggio

1. Crea una nuova classe o skin accanto a `Dog.ts` con gli stessi stati visivi.
2. Aggiungi il nome a `unlockedCharacters` e selezionalo tramite `selectedCharacter`.
3. Fai scegliere a `GameScene` la skin salvata senza cambiare il corpo fisico condiviso.
4. Aggiungi la relativa scheda alla schermata Characters.

## Sostituire gli asset grafici

La versione attuale genera forme originali con `Phaser.GameObjects.Graphics`, quindi non dipende da immagini esterne. Per usare sprite finali:

1. metti i file ottimizzati in `public/assets/`;
2. caricali in una scena di preload;
3. sostituisci gradualmente i metodi di disegno delle entità con sprite o sprite sheet;
4. conserva dimensioni e origine coerenti con i corpi Matter;
5. usa WebP/AVIF per le immagini e file audio compressi per mantenere veloce il caricamento mobile.

## Tecnologia

TypeScript, Vite, Phaser 3, Matter.js, HTML5 Canvas e `localStorage`. Nessun backend e nessun asset copiato da altri giochi.

## Licenza

Codice e grafica del progetto sono originali. Consulta il proprietario del repository prima di ridistribuire il gioco o i suoi asset.
