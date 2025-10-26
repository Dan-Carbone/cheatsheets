# Installation et Configuration d'un Cluster Kafka

Ce guide explique comment installer et configurer un cluster Kafka simple avec un broker et Zookeeper sur une machine Linux.

---

## 1. Prérequis

- Java 8 ou supérieur installé.
- Une machine Linux (testé sur Ubuntu/Debian et CentOS/RHEL).

---

## 2. Télécharger et Installer Kafka

### Télécharger Kafka

```bash
wget https://archive.apache.org/dist/kafka/3.4.0/kafka_2.13-3.4.0.tgz
tar -xzf kafka_2.13-3.4.0.tgz
cd kafka_2.13-3.4.0
```

---

## 3. Démarrer Zookeeper

Kafka utilise Zookeeper pour gérer son cluster. Démarrez Zookeeper en arrière-plan :

```bash
bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
```

---

## 4. Démarrer un Broker Kafka

Démarrez un broker Kafka en arrière-plan :

```bash
bin/kafka-server-start.sh -daemon config/server.properties
```

---

## 5. Vérifier que Kafka et Zookeeper sont en cours d'exécution

```bash
jps
```
Vous devriez voir des processus nommés `QuorumPeerMain` (Zookeeper) et `Kafka`.

---

## 6. Configuration de Base

### Modifier `config/server.properties`

Éditez le fichier pour personnaliser l'écoute et les répertoires de logs :

```bash
nano config/server.properties
```

Exemple de modifications :
```ini
listeners=PLAINTEXT://:9092
log.dirs=/tmp/kafka-logs
```

Redémarrez Kafka après toute modification :

```bash
bin/kafka-server-stop.sh
bin/kafka-server-start.sh -daemon config/server.properties
```

---

## 7. Créer un Topic

Créez un topic nommé `mon-topic` avec un facteur de réplication de 1 :

```bash
bin/kafka-topics.sh --create --topic mon-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
```

---

## 8. Lister les Topics

```bash
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

---
