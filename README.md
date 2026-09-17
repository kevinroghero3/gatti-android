# Gatti — app Android

Il gioco "Gatti" (puzzle stile Queens con i gatti) racchiuso in un'app Android nativa
che mostra il gioco in una WebView a schermo pieno. Tutto è offline: nessun permesso,
nessuna connessione richiesta.

## Come ottenere l'APK con GitHub Actions

1. Crea un repository su GitHub (anche privato) e carica **tutto il contenuto di questa cartella**
   nella radice del repo (quindi `settings.gradle`, `build.gradle`, `app/`, `.github/`).
2. Vai nel tab **Actions** del repo: la build parte automaticamente a ogni push sul branch
   `main`/`master`. Puoi anche avviarla a mano con **Run workflow** (workflow_dispatch).
3. Al termine (circa 2-4 minuti) apri la run e scarica l'artifact **Gatti-apk**, che contiene:
   - `Gatti-debug.apk` → installabile subito sul telefono
   - `Gatti-release-unsigned.apk` → da firmare con la tua chiave se vuoi pubblicarlo
4. Sul telefono abilita "Installa app da origini sconosciute" e apri l'APK debug.

Non serve il Gradle wrapper: il workflow usa `gradle/actions/setup-gradle` con Gradle 8.7.

## Struttura

```
settings.gradle                  configurazione del progetto e repository
build.gradle                     plugin Android Gradle 8.5.2
gradle.properties
app/build.gradle                 applicationId com.gatti.game, minSdk 23, targetSdk 34
app/src/main/AndroidManifest.xml app a schermo intero, solo portrait
app/src/main/java/...            MainActivity: WebView con JavaScript e localStorage
app/src/main/assets/gatti.html   il gioco completo (HTML, CSS, JS e sprite incorporate)
app/src/main/res/mipmap-*        icone dell'app generate dalla sprite del gatto
.github/workflows/android.yml    build automatica dell'APK
```

## Note tecniche

- `setDomStorageEnabled(true)`: i progressi (livelli completati e tavole sbloccate) vengono
  salvati in `localStorage`, quindi restano anche dopo la chiusura dell'app.
- Zoom, selezione del testo e long press di sistema sono disattivati per non interferire
  con i comandi touch del gioco (tap = ×, doppio tap = gatto, trascina = tante ×).
- Lo schermo resta acceso durante la partita.
- Il tema segue la modalità chiara/scura del telefono, come nella versione web.

## Aggiornare il gioco

Basta sostituire `app/src/main/assets/gatti.html` con la versione più recente del file HTML
e fare push: Actions ricostruisce l'APK.
