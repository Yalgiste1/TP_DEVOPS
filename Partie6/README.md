# Apprentissages

## Partie 6: Déployer JupyterLab et rendre les notebooks persistants

L'objectif de cet exercice est de lancer un environnement JupyterLab avec Docker Compose, d'y charger des datasets et de conserver son travail entre les redémarrages.

### Problème rencontré et pourquoi il est survenu

Sans volumes Docker, tous les notebooks créés ainsi que les librairies Python installées dans le conteneur étaient effacés à chaque `docker compose down`.

### Solution appliquée et pourquoi cette solution fonctionne

Dans `docker-compose.yaml` :
- J'ai utilisé l'image `quay.io/jupyter/datascience-notebook:python-3.12` avec un token d'accès (`JUPYTER_TOKEN`).
- J'ai créé deux volumes nommés : `jupyter_notebooks` pour le dossier de travail `/home/jovyan/work`, et `jupyter_data` pour `/home/jovyan/.local` afin de persister les packages installés.
- J'ai monté les dossiers `datasets/` et `examples/` en lecture seule (`:ro`) pour que les fichiers de données soient accessibles sans risquer d'être modifiés par erreur.

### Ce que j'ai appris

- Déployer JupyterLab simplement avec Docker Compose.
- Séparer la persistance des fichiers utilisateur (`work/`) et des packages installés (`.local/`).
- Protéger les jeux de données en les montant en lecture seule (`:ro`).
