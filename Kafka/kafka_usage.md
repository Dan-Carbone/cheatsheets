# Utilisation de Kafka : Producteurs et Consommateurs

Ce guide explique comment utiliser Kafka pour envoyer et recevoir des messages avec des producteurs et consommateurs en ligne de commande.

---

## 1. Démarrer un Producteur

Envoyez des messages au topic `mon-topic` :

```bash
bin/kafka-console-producer.sh --topic mon-topic --bootstrap-server localhost:9092
```

Tapez vos messages et appuyez sur Entrée pour les envoyer.

---

## 2. Démarrer un Consommateur

Lisez les messages du topic `mon-topic` :

```bash
bin/kafka-console-consumer.sh --topic mon-topic --from-beginning --bootstrap-server localhost:9092
```

---

## 3. Utilisation Avancée

### Créer un Producteur avec un Fichier d'Entrée

```bash
bin/kafka-console-producer.sh --topic mon-topic --bootstrap-server localhost:9092 < mon-fichier.txt
```

### Créer un Consommateur avec Sortie dans un Fichier

```bash
bin/kafka-console-consumer.sh --topic mon-topic --from-beginning --bootstrap-server localhost:9092 > sortie.txt
```

---

## 4. Gestion des Topics

### Supprimer un Topic

```bash
bin/kafka-topics.sh --delete --topic mon-topic --bootstrap-server localhost:9092
```

### Décrire un Topic

```bash
bin/kafka-topics.sh --describe --topic mon-topic --bootstrap-server localhost:9092
```

---

## 5. Bonnes Pratiques pour la Production

- **Réplication** : Augmentez le `replication-factor` pour la tolérance aux pannes.
- **Partitions** : Ajustez le nombre de partitions en fonction de votre charge.
- **Sécurité** : Configurez SSL/SASL pour sécuriser les communications.
- **Monitoring** : Utilisez des outils comme Prometheus et Grafana pour surveiller Kafka.

---
