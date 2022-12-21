# Env repository

[![Deploy to Digital Ocean](https://github.com/Horsty80/test-github-env/actions/workflows/digital_ocean.yml/badge.svg?branch=main&event=push)](https://github.com/Horsty80/test-github-env/actions/workflows/digital_ocean.yml)

## Prérequis :

- Docker
- Github actions

## Les principes :

Dans ce repository se trouve la liste des applications qui sont déployés en suivant le `trunk base development`.

Lorsque la version d'une application dans `value.yml` sera modifié, une github action présente sur ce repo, sera trigger.

Cette github action récupére le livrable en question, via réconciliation avec le numéro de version, qui se trouve dans la registry de github.

Ce livrable sera déployé sur l'environnement désigné en overridant les fichiers de conf si besoin et en assignant les bonnes variables d'env.

## Le détails :

- Des github actions permettent de déployer sur digital ocean
- Chaque répertoire représente une application (front ou back)
- Dans chaque répertoire, chaque environnement est représenté par un repertoire à son nom (dev / prod / staging / etc.)
- Dans chaque environment on trouve :
  - des fichiers de config (public) permettant de versionner une configuration pour chaque application si besoin
  - `value.yml` ou l'on retrouve :
    - les informations du serveur où le container sera déployé - le repository de l'application ou
    - de la config lié au déploiement comme l'auto déploiement
    - la version actuellement déployé
    - les hooks à triggers (comme e2e / sonar / lighthouse)
- Les fichiers de config front devront respecter un standard cela permet de le remplacer dans le container et ainsi d'utiliser le même build sur chaque environnement :

```js
// config
window.myAppName = { attribute: "value" };
```

- Pour les fichiers de config back (à déterminer)

## Autres :

- le numéro de version déployé correspond au package en suivant se pattern (à confirmer) `${nom_de_l_app}-${hash_de_commit}`
- Exemple avec `another-web-app`, qui posséde un container front, est sur le `tag` : `1.0`, le dernier commit sur le trunk - `main` est le n°`323` et le short commit id est le `abcde12345` donnera comme version à déployer -> `another-web-app-front:1.0.323-abcde12345.main`

## Amélioration :

- Utilisation de Helm pour faire des templates pour la configuration
