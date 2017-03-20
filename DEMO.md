# learn@Softeam Voting App Demo

## Mise en place Dockerfile du Java Backend

1. Le Dockerfile
2. Build de l'image
```
$ cd worker
$ docker build -t worker:v1 .
...
```
_Download des dépendances au premier build_

3. Exemple de run : démarrage du worker
```
$ docker run worker:v1
Waiting for redis
...
```
_normal, pas de serveur redis ..._

4. Exemple de modification du code
```
$ docker build -t worker:v1 .
$ docker run worker:v1
```

## Docker Compose

1. On va pouvoir démarrer les différents composants
```
$ cat docker-compose.yml
$ docker-compose up
```
_Avec ou sans les networks ? montrer l'utilité ?_

2. c'est testable
http://localhost:5000

3. On modifie le code app.py
_reload magique !!_

## Swarm mode / compose stack

1. création d'un cluster

2. démarrage du stack

3. scaling
