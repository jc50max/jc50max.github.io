# Pain Dur — PWA + TWA Android

Modèle V3 (cinétique non linéaire levure / bactérie), porté en PWA hors ligne
et emballé en Trusted Web Activity. Mise en service : suivre le guide pas à pas fourni à part.

## Arborescence

- `web/` : l'appli (page, manifest, service worker, icônes).
- `android/` : coque TWA (android-browser-helper 2.4.0, AGP 8.7.3, compileSdk 35, Gradle 8.9).
- `.github/workflows/`
  - `deploy-pages.yml` (« 1 - Publier le site ») : publie `web/` sur GitHub Pages et génère
    `/.well-known/assetlinks.json` à partir de la clé stockée dans les secrets.
  - `create-keystore.yml` (« 2 - Créer la clé de signature ») : génère la clé, à lancer dépôt **privé**.
  - `build-twa.yml` (« 3 - Compiler l'APK ») : APK + AAB signés.

## Domaine

Par défaut `<propriétaire>.github.io` (dépôt nommé `<propriétaire>.github.io`).
Domaine personnalisé : créer la variable de dépôt `TWA_HOST` (Settings → Secrets and variables →
Actions → Variables) et configurer le domaine dans Settings → Pages.

## Secrets

`KEYSTORE_B64`, `KEYSTORE_PASSWORD`, `KEY_ALIAS` (`paindur`), `KEY_PASSWORD`.
Sans secrets, la compilation produit un APK de debug, non vérifiable par assetlinks.

## Play Store

Play App Signing re-signe l'AAB : ajouter l'empreinte SHA-256 de la clé de signature d'appli
(Play Console) dans `sha256_cert_fingerprints` de `deploy-pages.yml`.

## Mises à jour

Modifier `web/`, incrémenter `VERSION` dans `web/sw.js`. Recompiler l'APK seulement si nom,
icône ou domaine changent (incrémenter `versionCode`).
