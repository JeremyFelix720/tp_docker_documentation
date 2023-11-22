# PROCEDURE DOCKER

## ACCEDER AUX IMAGES DE NOTRE PROJET EN LOCAL

Installer Docker Desktop si ce n'est pas déjà fait :
https://www.docker.com/products/docker-desktop

> Ouvrir "Docker Desktop"
> Ouvrir notre dossier de projet dans Visual Studio Code.

Vérifier que Docker a bien été installé :
```bash
docker version
```

Rajouter un fichier "Dockerfile" (sans extention) à la racine de notre projet avec le contenu suivant :
```ts
FROM node:18-alpine // alpine = la version la plus légère de node.
WORKDIR /todo // Sur windows, il y a un / devant le nom du dossier.
COPY package.json .
RUN npm install
COPY . . // Copie de ce projet (en se plaçant à la racine) dans Docker.
CMD ["node", "src/index.js"] //
```

Créer une "image" de notre projet d'après les fichier présents dans notre dossier de projet actuel :
```bash
docker build -t project_name .
```

Executer une image de notre projet sur un port spécifique (On peut créer autant d'images de notre projet indépendantes - avec des noms différents - sur des ports différents en changeant le 1er chiffre du port local et le 2ème du port du contenair Docker) :
```bash
docker run -dp 3002:3000 project_name
```

> Rentrer l'URL suivante dans un navigateur (pour voir notre projet executé sur ce port) :
http://localhost:3002

Voir le détail pour chacune des images (contenair id, image name, command, creation date, status, ports, etc.) :
```bash
docker ps
```

Arreter l'execution d'une image (ex. : pour la supprimer) :
```bash
docker stop contenair_id
```

Supprimer une image :
```bash
docker rm contenair_id
```

## PARTAGE DE NOS IMAGES

[Se connecter à "Docker hub"](https://hub.docker.com/u/jeremyfelix777) :
```bash
docker login -u jeremyfelix777
```
OU
Depuis Docker Hub, cliquer sur "Se connecter"

**Créer une image à partir de son dossier courant** (indiquer son nom d'utilisateur Docker Hub avant permettra de le pusher ensuite ; le "v1" est le nom du tag du projet qui doit s'incrémenter à chaque fois que l'on fait les 2 commandes suivantes - sinon l'autre qui s'appelle pareil sera écrasé sur "Docker Hub")
```bash
docker build -t jeremyfelix777/project_name:v1 .
```

**Pusher l'image :**
```bash
docker push jeremyfelix777/project_name:v1 .
```

> L'image est prête à être partagée si j'envoie le lien de la page à des collègues pour qu'il acceder à la version de notre projet.

____
