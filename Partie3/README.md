# Apprentissages

## Partie 3: Ajouter de la persistance et du hot-reloading au guestbook

L'objectif est de ne plus perdre les messages Redis à chaque redémarrage et d'activer le rechargement automatique du code Go sans devoir tout rebuild manuellement.

### Problème rencontré et pourquoi il est survenu

Dès que je faisais un `docker compose down`, la liste des messages du guestbook se vidait complètement au relancement car Redis tournait sans volume persistant. 

Côté développement, la moindre modif dans `main.go` m'obligeait à couper le conteneur, relancer un build complet de l'image et redémarrer Compose. De plus, notre image finale (`Dockerfile`) étant basée sur `scratch`, elle ne contient ni compilateur Go ni outil pour faire du rechargement à chaud.

### Solution appliquée et pourquoi cette solution fonctionne

Pour garder les messages, j'ai déclaré un volume nommé `redis_data` monté sur `/data` dans le service Redis. Les données restent ainsi stockées côté hôte même si le conteneur est supprimé.

Pour le rechargement à chaud :
- J'ai fait un `Dockerfile.dev` basé sur `golang:alpine` qui installe Air.
- J'ai configuré `.air.toml` pour surveiller les fichiers et relancer le binaire dans `tmp/`.
- J'ai créé `docker-compose.dev.yaml` avec un bind-mount (`.:/app`) pour que les modifs faites sur ma machine soient directement visibles dans le conteneur sans rebuild.

En lançant `docker compose -f docker-compose.dev.yaml up`, le serveur se recompile tout seul dès que je sauvegarde.

### Ce que j'ai appris

- Utiliser un volume Docker nommé pour rendre Redis persistant.
- Pourquoi séparer le Dockerfile de prod (image minimale `scratch`) du Dockerfile de dev (avec Go et Air).
- Monter le dossier local en volume (`.:/app`) pour le hot-reloading.
