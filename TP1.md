# Un conteneur qui affiche un message puis s'arrête
''''
docker run hello-world
''''
# Un serveur web lancé en arrière-plan, joignable sur le port 8081
docker run -d -p 8081:80 --name web nginx:1.27

# Les conteneurs en cours, puis ce que le serveur a écrit
docker ps
docker logs web

# Entrer dans le conteneur, regarder les fichiers du site, ressortir
docker exec -it web sh
ls /usr/share/nginx/html
exit

# Arrêter, constater qu'il existe encore, puis le supprimer
docker stop web
docker ps -a
docker rm web
