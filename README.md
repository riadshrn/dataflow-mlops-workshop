# Qlik Dataflow MLOps Workshop

🚀 **Atelier pratique** sur la création d’un pipeline de données automatisé et l’expérimentation de modèles de Machine Learning dans **Qlik Cloud**.

---

## 🧭 Vue d’ensemble

Cet atelier s’articule autour de **trois grands axes complémentaires** :

1️⃣ **Flux de données** — Construction et préparation du pipeline de données dans Qlik Sense.  
2️⃣ **Expérimentation ML** — Entraînement, évaluation et interprétation d’un modèle de prédiction de churn.  
3️⃣ **Automatisation** — Déploiement des prédictions, alertes automatiques et intégration avec Google Sheets.

---

## 🎯 Objectifs

L’objectif de cet atelier est de comprendre **les facteurs qui influencent le départ des clients** (“churn”) et de construire un modèle capable de **prédire quels clients risquent de partir**.  
C’est un cas d’usage classique en **analyse prédictive et fidélisation client**, très utilisé dans les secteurs de la **banque**, des **télécoms** et de la **distribution**.

Sur le plan technique, l’atelier vise à :

- Construire un **flux de données complet (ETL)** dans Qlik Sense.  
- Entraîner et évaluer un **modèle de prédiction de churn** avec Qlik Predict.  
- Mettre en place une **chaîne d’automatisation** intégrant alertes, reporting et synchronisation avec Google Sheets.

---

#### 🧠 Contexte
L’objectif est de comprendre **les facteurs qui influencent le départ des clients** (“churn”) et de construire un modèle capable de **prédire quels clients risquent de partir**.  
C’est un cas d’usage classique en **analyse prédictive et fidélisation client**, très utilisé dans la banque, les télécoms ou la distribution.

## 🧩 Thèmes abordés

### 1️⃣ Flux de données (Data Pipeline)
- **Import initial du dataset brut** : `Bank Customer Churn Dataset.csv`.  
- **Nettoyage et préparation** :
  - Gestion des valeurs manquantes et des types de données.
  - Standardisation des libellés.
  - Création de nouvelles variables dérivées (ex. `balance_ratio`, `active_member`).
- **Découpage en sous-jeux** :
  - `churn_train.csv` (pour l’entraînement du modèle)
  - `churn_val.csv` (pour la validation/test)

➡️ Cette étape permet de construire un **pipeline de données propre et reproductible**, base de toute expérimentation ML et automatisation ultérieure.

### 2️⃣ Expérimentation ML & Déploiement (Qlik Predict)
- Entraînement de modèles de classification (is_churn = 0/1).  
- Évaluation (Accuracy, Recall, Precision, F1-score, AUC/ROC).  
- Génération de prédictions sur le jeu de validation.  
- Interprétation des résultats (SHAP values).  

### 3️⃣ Automatisation & Intégration
- Alertes e-mail personnalisées (SMTP Gmail + HTML).  
- Écriture automatique des prédictions dans **Google Sheets**.  
- Rafraîchissement et visualisation dans une app analytique Qlik (Matrice de confusion, KPI, Importance des variables).  

---

## 🧠 Compétences démontrées

- Data engineering et automatisation avec **Qlik Application Automation**  
- Machine Learning opérationnel (MLOps) avec **Qlik Predict**  
- Intégration inter-apps (**Google Sheets**, **SMTP**, **Qlik Cloud Services**)  
- Visualisation analytique et reporting dynamique  
