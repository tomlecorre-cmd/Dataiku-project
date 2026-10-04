# Projet de Prédiction d'Attrition Client (Churn Bancaire)

![Dataiku DSS](https://img.shields.io/badge/Dataiku-DSS-00A9E0?style=for-the-badge)
![Random Forest](https://img.shields.io/badge/Random_Forest-2ea44f?style=for-the-badge)
![Régression Logistique](https://img.shields.io/badge/Régression_Logistique-2ea44f?style=for-the-badge)
![Classification Binaire](https://img.shields.io/badge/Classification_Binaire-8A2BE2?style=for-the-badge)
![ROC AUC](https://img.shields.io/badge/Évaluation-ROC_AUC-FF9900?style=for-the-badge)

Ce projet a été réalisé sur la plateforme Dataiku DSS. L'objectif principal est d'identifier de manière proactive les clients présentant un fort risque de clôture de leurs comptes, afin de permettre aux équipes commerciales de mettre en place des actions de rétention adaptées.

## 1. Préparation des données

L'ensemble de la préparation a été modélisé via le Flow Dataiku, en partant de 5 bases de données relationnelles brutes (informations clients, produits détenus, soldes, revenus et informations additionnelles).

* **Création de la variable cible :** Définition du statut de churn basée sur la présence d'une date de fin de contrat.
* **Agrégation de l'historique :** Utilisation des recettes Dataiku (Group, Prepare) pour écraser la profondeur historique des comptes et calculer des indicateurs statistiques par client (solde minimum atteint, somme des revenus, nombre de produits).
* **Création de la Master Table :** Jointures multiples (Left Join) pour consolider l'ensemble des indicateurs sur une maille client unique.
* **Correction de Data Leakage :** Identification et retrait de la variable de date de clôture lors de la phase de modélisation, celle-ci provoquant une fuite de données empêchant la généralisation de l'algorithme.

## 2. Modélisation et Évaluation

Le problème métier implique un fort déséquilibre des classes (environ 80 % de clients fidèles contre 20 % de départs). Les algorithmes ont donc été évalués spécifiquement avec la métrique ROC AUC, l'Accuracy classique étant biaisée sur ce type de répartition.

* **Mise en concurrence des modèles :** Plusieurs algorithmes de classification (Régression Logistique, Random Forest, etc.) ont été entraînés et comparés afin d'identifier la meilleure capacité prédictive.
* **Algorithme retenu :** Le modèle Random Forest a été sélectionné pour ses performances supérieures. Il atteint un ROC AUC de 0.998 sur l'approche Produit et de 0.867 sur l'approche Client global.
* **Optimisation métier :** Les seuils de probabilité ont été calibrés manuellement dans le but de minimiser les faux négatifs, l'oubli d'un client partant étant l'erreur ayant le coût financier le plus lourd pour la banque.

## 3. Recommandations Métier

L'analyse de l'importance des variables du modèle a permis de dégager plusieurs axes stratégiques :

* Le niveau d'équipement est le facteur de rétention principal. Les clients ne possédant qu'un seul produit présentent le risque de départ le plus critique.
* Le segment de clientèle "SILVER" concentre le plus grand volume de churn et doit être ciblé en priorité par les campagnes marketing.
* L'inactivité sur les plateformes digitales couplée à une chute du solde minimum sont des signaux avant-coureurs majeurs.

## 4. Accès aux fichiers et reproductibilité

L'ensemble des fichiers nécessaires (données brutes et/ou tables préparées) est hébergé directement à la racine de ce dépôt. 

Pour reproduire l'analyse ou explorer les données :
1. Cloner ce dépôt sur votre environnement local.
2. Pour une utilisation dans Dataiku DSS : créer un nouveau projet vide et importer ces fichiers en tant que nouveaux datasets.
3. Il est ensuite possible de recréer les étapes de préparation décrites dans ce document et d'entraîner vos propres modèles sur la base consolidée.
