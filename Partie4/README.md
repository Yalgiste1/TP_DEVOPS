# Apprentissages

## Partie 4: Créer des stacks avec du hot-reloading

L'objectif est de tester le hot-reloading sur une autre stack que Go. J'ai choisi Python Flask.

### Problème rencontré et pourquoi il est survenu

Aucun problème particulier. Le rechargement automatique est déjà intégré nativement dans Flask avec le mode debug.

### Solution appliquée et pourquoi cette solution fonctionne

J'ai créé l'application dans `app.py` et son `Dockerfile`. Dans `docker-compose.yaml`, j'ai monté le dossier en volume (`.:/app`) et activé `FLASK_DEBUG=1`. Flask détecte les modifications et recharge le serveur tout seul.

### Ce que j'ai appris

- Utiliser le mode debug natif de Flask dans un conteneur.
- Monter le dossier du code en volume pour le développement.
