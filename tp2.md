# Étape 2 — Héberger mon propre site
## méthode A: prêter un dossier au conteneur
 Afficher votre propre page web avec nginx, de deux façons. D'abord en prêtant un dossier de votre PC au conteneur, puis en construisant votre propre image.
 ### Créer un dossier tp-docker
 Créer le dossier à partir de l'interface graphique ou via powershell
 ```bash
mkdir tp-docker
cd tp-docker
```
### Créer la page
Créer le dossier site et la page index.html en copiant-collant ces commandes.

```bash
mkdir site
Set-Content -Path site\index.html -Encoding utf8 -Value '<!DOCTYPE html><html lang="fr"><head><meta charset="utf-8"><title>Mon site</title></head><body><h1>Bonjour les devs depuis Docker !</h1><p>Site de : ton prenom</p></body></html>'
```

Votre PC                                   Conteneur web
tp-docker/site/index.html      ──────►     /usr/share/nginx/html/index.html

```bash
docker run -d -p 8081:80 --name web -v "${PWD}/site:/usr/share/nginx/html" nginx:1.27
```
nginx affiche les pages qu'il trouve dans son dossier /usr/share/nginx/html. L'option -v remplace ce dossier par votre dossier site : le conteneur lit directement les fichiers de votre PC.
#### BONUS
| Morceau de la commande | Signification |
|---|---|
| `-d` | Lancer le conteneur en arrière-plan |
| `-p 8081:80` | Le port 8081 du PC mène au port 80 du conteneur |
| `--name web` | Donner le nom `web` au conteneur |
| `-v "${PWD}/site:/usr/share/nginx/html"` | Le dossier `site` du dossier courant (`${PWD}`) remplace le dossier des pages de Nginx. La valeur entière doit être entre guillemets. |
| `nginx:1.27` | L’image utilisée |

## Méthode B: construire sa propre image

Avec la méthode A, la page reste sur votre PC : le conteneur ne fonctionne que sur cette machine. Pour pouvoir livrer le site ailleurs, on copie la page à l'intérieur d'une image.
Créer le fichier site/Dockerfile, sans extension, qui contient ces deux lignes :
FROM nginx:1.27-alpine
COPY index.html /usr/share/nginx/html/index.html
Avec Powershell
```bash
Set-Content -Path site\Dockerfile -Encoding ascii -Value "FROM nginx:1.27-alpine`nCOPY index.html /usr/share/nginx/html/index.html"
```
Construire l'image, puis la lancer. Cette fois, pas d'option -v : la page est dans l'image.

```bash
docker build -t mon-site:2.0 site
docker images
```

```bash
docker run -d -p 8082:80 --name web2 mon-site:2.0
```
