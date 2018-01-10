# Monitoring avec la stack Telegraf InfluxDB Grafana

> Ce projet contient les fichiers docker-compose et de configuration pour la mise en place d'une supervision complète

## Présentation de la stack technique

- __InfluxDB__ : InfluxDB est système de gestion de base de données temporelle. Ce type de base est tout à fait adaptée pour le stockage de mesures.
- __Grafana__ : Propose une interface web permettant d'interpréter les données stockées dans InfluxDB. Cet outil permet également de mettre en place de l'alerting.
- __Telegraf__ : Agent permettant de récupérer des mesures au moyen de plugins. Il est ici utilisé pour remonter les metrics système et du démon Docker. Il est également utilisé pour vérifier l'état des sites en mesurant le temps de réponse.

## Contenu du projet

- __configuration__ : Contient un exemple de configuration pour _Telegraf_
- __monitoring-agent-only__ : Contient un fichier `docker-compose.yml` permettant le déploiement de l'agent _Telegraf_
- __monitoring-server__ : Contient un fichier `docker-compose.yml` permettant le déploiement de la stack complète (InfluxDB, Grafana et Telegraf)

> __Remarque__ : Pour le projet docker-compose __monitoring-server__ la configuration s'effectue en renommant le fichier `.env.sample` en `.env` et en valorisant chaque paramètre.

## Mise en place

> A effectuer uniquement lors de la première installation du serveur

Lors de l'installation du serveur (InfluxDB + Grafana) il est nécessaire de modifier la durée de rétention qui est créée par défaut. En effet, de base, les données ont une durée de rétention illimitée. Il est possible de modifier celle-ci de la manière suivante :

```bash
# On entre dans le conteneur InfluxDB
docker exec -it monitoring_influxdb_1 bash

# On démarre un client InfluxDB en administrateur
influx -username admin -password <password>

# On sélectionne la base telegraf
use telegraf

# On modifie la durée de rétention (ici à 4 semaines)
ALTER RETENTION POLICY "autogen" ON "telegraf" DURATION 4w REPLICATION 1
```