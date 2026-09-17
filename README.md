# Gatti — app Android

Il gioco "Gatti" (puzzle stile Queens con i gatti) in un'app Android nativa che mostra
il gioco in una WebView a schermo pieno. Tutto offline: nessun permesso, nessuna rete.

## Build con GitHub Actions

1. Carica il contenuto di questa cartella nella radice del repo.
2. Tab **Actions** → la build parte a ogni push su `main`/`master`, oppure **Run workflow**.
3. Scarica l'artifact **Gatti-apk**. Contiene due APK, **entrambi firmati e installabili**:
   - `Gatti-debug.apk`
   - `Gatti-release.apk` (firmato con una chiave generata dal workflow)

## Installazione sul telefono

L'artifact di GitHub si scarica come file ZIP: va **prima estratto**, poi si apre il file
`.apk`. Se si prova ad aprire lo ZIP (o un APK non firmato) Android risponde
"App non installata: il pacchetto sembra non valido".

Sul telefono serve consentire l'installazione da origini sconosciute all'app che apre il file
(Chrome, File, Drive...).

## Firma

Il workflow genera una chiave con `keytool` e firma l'APK di release, poi verifica entrambi
gli APK con `apksigner verify`. Per pubblicare su Play Store conviene usare una tua chiave
persistente: mettila nei Secrets del repo e sostituisci i valori nel blocco `env:` del workflow.
La chiave va conservata: gli aggiornamenti devono essere firmati con la stessa.

## Nota sulle classi Kotlin duplicate

Alcune versioni di AndroidX trascinano `kotlin-stdlib-jdk7/jdk8` 1.6.x che duplicano le
classi già incluse in `kotlin-stdlib` 1.8+, facendo fallire `checkDebugDuplicateClasses`.
In `app/build.gradle` una `resolutionStrategy` rimappa quei moduli su `kotlin-stdlib:1.9.24`.

## Aggiornare il gioco

Sostituisci `app/src/main/assets/gatti.html` con la versione più recente e fai push.
Se aggiorni un'app già installata, alza anche `versionCode` in `app/build.gradle`.
