## TP — Premières commandes Docker


# 1. Premier conteneur

```bash
docker run hello-world
```

# 2. Serveur Nginx en arrière-plan

```bash
docker run -d -p 8081:80 --name web nginx:1.27
```

# 3. Vérifier et consulter les logs

```bash
docker ps
docker logs web
```

# 4. Entrer dans le conteneur

```bash
docker exec -it web sh
```
Une fois dans le conteneur

```bash
ls /usr/share/nginx/html
exit
```

# 5. Arrêter et supprimer
```bash
docker stop web
docker ps -a
docker rm web
```
