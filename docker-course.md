# 🐳 Cours Complet Docker — Fiche Unique

## 1. Introduction
Docker est une plateforme de **conteneurisation** permettant d’empaqueter et d’exécuter des applications dans des environnements isolés appelés **conteneurs**.  
Objectifs : portabilité, reproductibilité, rapidité, scalabilité.

---

## 2. Pourquoi Docker ?
- Déploiements cohérents entre machines.
- Environnements reproductibles.
- Isolation des dépendances.
- Démarrage rapide (secondes).
- Léger comparé aux machines virtuelles.

---

## 3. Concepts Fondamentaux

### Image
- Modèle immuable contenant OS minimal + dépendances + code.
- Construite en couches (layers).
- Versionnée et stockée dans un **registry**.

### Conteneur
- Instance d’une image.
- Processus isolé avec un système de fichiers dédié.
- Éphémère sauf si volumes.

### Dockerfile
Fichier décrivant la construction d’une image.  
Exemple :
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "start"]
```

### Registry
- Docker Hub (public)
- GitHub Container Registry
- GitLab Registry
- AWS ECR, GCP GCR, Azure ACR

---

## 4. Architecture Docker
- **Client Docker** : CLI (`docker`).
- **Daemon dockerd** : gère conteneurs, images, volumes, réseaux.
- **Registry** : stockage d’images.

---

## 5. Cycle de Vie d’un Conteneur
`created → running ↔ paused → stopped → removed`

Commandes associées :
```bash
docker create
docker start
docker stop
docker restart
docker rm
```

---

## 6. Commandes Essentielles

### Images
```bash
docker images
docker pull nginx
docker build -t mon-app .
docker rmi mon-app
```

### Conteneurs
```bash
docker run hello-world
docker run -d -p 8080:80 --name web nginx
docker ps
docker ps -a
docker stop web
docker rm web
docker logs web
docker exec -it web bash
```

### Nettoyage
```bash
docker system prune
docker volume prune
docker image prune
```

---

## 7. Volumes (Persistance)
Les volumes permettent de **conserver les données** indépendamment du cycle de vie du conteneur.

Types :
- **Volumes Docker** (recommandé)
- **Bind mounts** (montage local)
- **tmpfs** (RAM)

Exemple :
```bash
docker run -d -v data:/var/lib/mysql mysql
```

---

## 8. Réseaux Docker
Types :
- **bridge** (par défaut)
- **host**
- **overlay** (Swarm)
- **none**

Créer un réseau :
```bash
docker network create mon-reseau
docker run -d --network mon-reseau nginx
```

---

## 9. Docker Compose (Multi‑conteneurs)
Permet de définir une application complète via `docker-compose.yml`.

Exemple :
```yaml
version: "3"
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  api:
    build: ./api
    environment:
      - NODE_ENV=production
  db:
    image: postgres
    volumes:
      - dbdata:/var/lib/postgresql/data

volumes:
  dbdata:
```

Commandes :
```bash
docker compose up -d
docker compose down
docker compose logs -f
```

---

## 10. Dockerfile — Instructions Principales
- `FROM` : image de base
- `RUN` : exécute une commande
- `COPY` / `ADD` : copie des fichiers
- `WORKDIR` : répertoire de travail
- `EXPOSE` : port exposé
- `CMD` : commande par défaut
- `ENTRYPOINT` : point d’entrée

---

## 11. Optimisation des Images
- Utiliser des images légères (`alpine`).
- Multi‑stage builds :
```dockerfile
FROM node:18 AS build
WORKDIR /app
COPY . .
RUN npm install && npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
```
- Minimiser les layers.
- Nettoyer les caches.

---

## 12. Sécurité Docker
- Ne pas exécuter en root.
- Scanner les images (Trivy, Clair).
- Utiliser des images officielles.
- Limiter les capacités Linux (`--cap-drop`).

---

## 13. Docker vs Machine Virtuelle
| Critère | Conteneur | VM |
|--------|-----------|----|
| OS | Partage le noyau | OS complet |
| Poids | Mo | Go |
| Démarrage | Secondes | Minutes |
| Isolation | Processus | Matériel |
| Performance | Native | Légère perte |

---

## 14. Docker Swarm (Orchestration)
Fonctionnalités :
- Clustering
- Load balancing
- Services répliqués

Commandes :
```bash
docker swarm init
docker service create --replicas 3 nginx
```

---

## 15. Kubernetes (Comparaison)
Docker = conteneurisation  
Kubernetes = orchestration avancée  
Fonctionnalités :
- Autoscaling
- Self‑healing
- Networking avancé
- ConfigMaps, Secrets

---

## 16. Bonnes Pratiques
- Images petites et sécurisées.
- Variables d’environnement dans `.env`.
- Pas de secrets dans les images.
- Utiliser des healthchecks.
- Versionner les images (`app:1.0.3`).

---

## 17. TL;DR — Résumé Final
- **Image = modèle**, **Conteneur = instance**.
- Dockerfile = **recette**.
- Compose = **orchestration simple**.
- Volumes = **persistance**.
- Réseaux = **communication**.
- Docker = rapide, léger, portable.
