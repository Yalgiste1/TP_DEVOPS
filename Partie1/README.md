# Apprentissages

## Partie 1: Conteneuriser l'application guestbook

L'objectif de cet exercice est de tester l'application Go localement, de créer son image Docker puis de la lancer avec Docker Compose.

### Problème rencontré et pourquoi il est survenu

Mon premier Dockerfile utilisait `golang:alpine` et l'image pesait plus de 300 Mo. Quand j'ai voulu utiliser une image `scratch` pour la réduire, le conteneur ne se lançait pas parce que le binaire dépendait de bibliothèques C système. Enfin, l'interface web ne chargeait pas les styles car j'avais oublié de copier le dossier `public/`.

### Solution appliquée et pourquoi cette solution fonctionne

J'ai utilisé un build multi-étapes dans le `Dockerfile`. J'ai compilé le code avec `CGO_ENABLED=0` dans une première étape `builder` pour obtenir un binaire statique, puis je l'ai copié avec le dossier `public/` dans une image `scratch`. J'ai ensuite créé le fichier `docker-compose.yaml` pour exposer le port 3000 et lancer l'application avec `docker compose up`.

L'image finale ne fait plus que 15 Mo.

### Ce que j'ai appris

- Utiliser le multi-stage build pour séparer la compilation de l'image finale.
- Compiler en statique avec `CGO_ENABLED=0`.
- Utiliser l'image `scratch` pour réduire fortement la taille de l'image.
- Lancer un conteneur simplement avec `docker compose up`.
