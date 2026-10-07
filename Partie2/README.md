# Apprentissages

## Partie 2: Ajouter une base de données à votre service

L'objectif de cet exercice est d'ajouter un service Redis à l'application via Docker Compose pour pouvoir enregistrer les messages du guestbook.

### Problème rencontré et pourquoi il est survenu

Sans base de données, l'interface affichait le message "No database connection..." et les messages soumis ne s'enregistraient pas. Le code Go cherchait à joindre un serveur Redis via la variable `REDIS_HOST`, mais aucun service Redis n'était lancé.

### Solution appliquée et pourquoi cette solution fonctionne

J'ai ajouté un service `redis` avec l'image `redis:7` dans `docker-compose.yaml`. J'ai ensuite configuré `REDIS_HOST=redis` dans les variables d'environnement du service `guestbook`. Docker Compose permet alors au binaire d'accéder au service Redis a travers son nom de service.

### Ce que j'ai appris

- Interconnecter deux conteneurs (Guestbook et Redis) avec Docker Compose.
- Passer la variable d'environnement `REDIS_HOST` au conteneur Go.
