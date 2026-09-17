# Gatti — app Android

Il gioco "Gatti" (puzzle stile Queens con i gatti) in un'app Android nativa che mostra
il gioco in una WebView a schermo pieno. Tutto offline: nessun permesso, nessuna rete.

## Build con GitHub Actions

1. Carica il contenuto di questa cartella nella radice del repo.
2. Tab **Actions** → la build parte a ogni push su `main`/`master`, oppure **Run workflow**.
3. Scarica l'artifact **Gatti-apk**: `Gatti-debug.apk` (installabile subito) e
   `Gatti-release-unsigned.apk` (da firmare per la pubblicazione).

Non serve il Gradle wrapper: il workflow usa Java 17 + Gradle 8.7 (AGP 8.5.2).

## Nota sulle classi Kotlin duplicate

Alcune versioni di AndroidX trascinano `kotlin-stdlib-jdk7/jdk8` 1.6.x che duplicano le
classi già incluse in `kotlin-stdlib` 1.8+, e il task `checkDebugDuplicateClasses`
fallisce. In `app/build.gradle` c'è una `resolutionStrategy` che rimappa quei moduli su
`kotlin-stdlib:1.9.24`, così la build passa.

## Aggiornare il gioco

Sostituisci `app/src/main/assets/gatti.html` con la versione più recente e fai push.
