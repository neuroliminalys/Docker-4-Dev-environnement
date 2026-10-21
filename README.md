# Docker-4-Dev-environnement

# Bind mount vs Volume

1. **Qu'arrive-t-il aux fichiers écrits dans un conteneur lorsqu'on l'arrête, puis lorsqu'on le supprime et le recrée ?** Si un volume a été crée, il persiste quand le conteneur stoppe (sauf avec `docker stop -v` ou `docker rm -f new-db`).
2. **Qu'est-ce qu'un volume Docker et comment permet-il de conserver des données ?** Un volume docker est un espace réservé à la persistence des données dans le répertoire de fichiers de l'installation Docker. Il est stocké sur le PC hôte via le répertoire spécifié, en le liant à un espace à l'intérieur du conteneur.
3. **Qu'est-ce qu'un bind mount, quelle est la différence avec un volume et dans quels cas utiliser chacun ?** Un bind mount est un espace du PC hôte monté dans le conteneur. C'est-à-dire que c'est un "tunnel" qui permet la communication entre l'hôte et le conteneur. Si un bind mount est utilisé avec un répertoire qui n'est pas vide, le contenu est masqué. Usecases :
   - Je veux stocker une base de données locale au projet mais qui ne s'efface pas à chaque démontage => volumes
   - Je veux conteneuriser une application mais récupérer un résultat (qui n'a aucune incidence sur le fonctionnement de mon application) sur mon PC en dehors du conteneur => bind mount


## Lancer un script JS unique avec Docker

### La commande
```docker run -it --rm -p 3000:3000 --name my-running-script `
-v ${PWD}:/usr/src/app `
-w /usr/src/app `
node:24 `
npm run dev```

### Explications
`docker run -it` : lance un conteneur en mode interactif, ce qui pemet d'exécuter des commandes à l'intérieur du terminal de commande directement dans le conteneur
`--rm` : supprime automatiquement le conteneur lors de son arrêt
`-p 3000:3000` : configure le port sortant 3000 (hôte) par rapport au port du conteneur 3000 (interne au conteneur)
`--name my-running-script` : nomme l'image "my-running-script
`-v ${PWD}:/usr/src/app` : crée un bind mount sur PWD (répertoire d'où est lancée la commande. Note : la commande est $"PWD" sous Linux et ${PWD} sous Powershell) et le répertoire /usr/src/app du conteneur
`-w /usr/src/app` : ou `--workdir` définit le répertoire de travail du conteneur. Cela permet de se placer directement dans /usr/src/app quand le conteneur est lancé
`node:24` : utilise l'image officielle Docker de Node en version 24
`npm run dev` : commande du package.json, ce qui permet d'exécuter des services directement, par exemple Nodemon ou Vite
