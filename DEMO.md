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
    * On a choisi Amazon Web Service, on pourrait utiliser Microsoft Azure, Digital Ocean, (bientôt ?) Google Cloud Platform
      ```
      $ ls -l ~/.aws
      $ docker-machine create --driver amazonec2 --amazonec2-region eu-west-1 --amazonec2-security-group myswarm aws01
      ```
      VM EC2 créée, docker provisonné, clefs déployées

    * liste des machines distantes
      ```
      $ docker-machine ls
      ```

    * ssh sur un noeud
      ```
      $ docker-machine ssh aws01
      ```

    * initialisation du manager
      ```
      docker-machine ssh aws01 "sudo docker swarm init"
      ```
    * initialisation des noeuds
      ```
      docker-machine ssh aws02 "sudo docker swarm join ..."
      ```
    * le cluster est prêt
      ```
      $ eval $(docker-machine env aws01)
      $ docker node ls
      ```

2. démarrage des services sur le cluster
   * _on backstage : build + push emerout/examplevotingapp... sur docker.io_
   * création du docker-stack.yml
   * déploiement sur le cluster
      ```
      $ docker stack deploy -c docker-stack.yml vote
      ```
   * c'est démarré
       ```
       $ docker stack ls
       $ docker stack ps vote
       $ docker service ls
       ```


3. test de l'application
   * voici l'url du service : http://54.171.122.125 ou http://bit.do/eyc
   * vous pouvez voter !!!
   *  
     ```
     $ curl -s 'http://54.171.122.125/' -XPOST --data 'vote=a' | grep Processed
     ```
4. scaling
   * modification du fichier compose
   * via commande docker
     ```
     docker service scale vote_vote=10
     ```
