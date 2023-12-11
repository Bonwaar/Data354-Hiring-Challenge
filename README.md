# Data354-Hiring-Challenge
 
## Projet ETL et Tableau de Bord AirQuino

## Aperçu

Ce projet implique l'extraction des données horaires de l'API AirQuino, leur traitement pour calculer les valeurs moyennes de CO et PM2.5 par jour pour chaque capteur, et le stockage des résultats dans différentes bases de données (MongoDB, Cassandra et MySQL). De plus, un tableau de bord Superset est créé pour visualiser les données.

## Prérequis

Avant de commencer, assurez-vous d'avoir installé les éléments suivants sur votre machine :

- Docker
- Python avec les packages pymongo, cassandra-driver et MySQLdb
- MongoDB
- Cassandra
- WampServer (pour MySQL)

## Configuration

1. Clonez le dépôt Superset :

    ```bash
    git clone https://github.com/apache/superset.git
    ```

2. Accédez au répertoire Superset :

    ```bash
    cd superset-latest
    ```

3. Téléchargez les images Docker requises et démarrez Superset :

    ```bash
    docker-compose -f docker-compose-non-dev.yml pull
    docker-compose -f docker-compose-non-dev.yml up
    ```

4. Créez une base de données MongoDB nommée `sensors_data` avec une collection nommée `data`.

5. Créez un espace de clés Cassandra nommé `sensors_data` avec une stratégie de réplication simple et un facteur de réplication de 3. Utilisez l'espace de clés et créez une famille de colonnes nommée `data` avec les champs date, sensor, PM2.5 et CO.

6. Installez WampServer et créez une base de données MySQL pour les données des capteurs.

## Processus ETL

Pour effectuer le processus ETL, suivez ces étapes :

1. Exécutez le notebook Jupyter (`etl.ipynb` dans le dépôt) contenant les fonctions `extract()`, `transform()`, `load_in_mongo()`, `load_in_cassandra()` et `load_in_mysql()`.

2. La fonction `extract()` récupère les données horaires de l'API AirQuino.

3. La fonction `transform()` calcule les valeurs moyennes journalière de CO et PM2.5 pour chaque capteur.

4. Les fonctions `load_in_mongo()`, `load_in_cassandra()` et `load_in_mysql()` stockent les données transformées dans MongoDB, Cassandra et MySQL, respectivement.

## Tableau de Bord Superset

1. Connectez votre base de données MySQL à Superset.
*host*: `host.docker.internal`
*port*: `3306`
*username*: `root`
*pwd*: ``

2. Importez le tableau de bord  `Dashboard.zip` du dépôt dans Apache Superset.

3. Explorez et visualisez les données à l'aide de Superset.

## Stratégie de Déploiement

### Mise en Production de l'ETL

1. **Containerisation** : Mettez le processus ETL en conteneurs avec Docker pour assurer la cohérence dans différents environnements.

2. **Orchestration** : Utilisez un outil d'orchestration comme Kubernetes pour déployer et gérer les conteneurs ETL de manière évolutive et fiable.

3. **Monitoring et Logging** : Mettez en place la surveillance et les journaux pour suivre les performances et identifier les problèmes en production.

### Automatisation de l'Exécution de l'ETL

1. **Planification avec Apache Airflow** : Planifiez l'exécution du processus ETL toutes les heures en utilisant un planificateur de tâches tel qu'Apache Airflow.

2. **Configuration d'Airflow** : Configurez Airflow pour déclencher l'exécution du notebook Jupyter, assurant ainsi l'automatisation et la cohérence du processus ETL.

3. **Surveillance du Pipeline ETL** : Surveillez le pipeline ETL pour recevoir des alertes en cas d'échecs et garantir la fiabilité du processus automatisé.

## Difficultés rencontrées

1. **Connextion de apache superset à ma base de données mongodb**

2. **Connextion de apache superset à ma base de données cassandra**

Ces difficultés m'ont contraint à utiliser une bd relationnelle pour la connecter à apache superset
