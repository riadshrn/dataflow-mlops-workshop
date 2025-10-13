# TD 1 – Flux de données : Préparation du jeu de données pour le modèle de churn

## Objectif
L’objectif de cette première partie est de **préparer les données brutes** issues du dataset *Bank Customer Churn Prediction* afin de :
- Comprendre la structure et le contenu du jeu de données.
- Créer des **champs dérivés** utiles à la modélisation.
- Diviser les données en **jeux d’entraînement et de validation**.
- Générer des **fichiers cibles (.qvd et .txt)** qui serviront pour l’entraînement et l’évaluation du modèle.

---

## Étape 1 — Création du flux de données

1. Dans Qlik Cloud, accédez à **Create → Data Flow**.  
2. Nommez votre flux :  `Churn_data_flow`
3. Cliquez sur **Open Editor** pour accéder à l’éditeur visuel du flux.

---

## Étape 2 — Importation du dataset

1. Dans le **Data Catalog**, cliquez sur **Upload data file**.  
2. Importez le fichier : `Bank Customer Churn Prediction.csv`
   Vous pouvez le télécharger :
   - depuis Kaggle : [Bank Customer Churn Dataset](https://www.kaggle.com/datasets/gauravtopre/bank-customer-churn-dataset)  
   - ou directement depuis le repo GitHub du TD :  [Data](https://github.com/riadshrn/dataflow-mlops-workshop/tree/main/data)
3. Une fois chargé, **visualisez les colonnes** dans le flux.  
   - Analysez les types de données.  
   - Vérifiez s’il y a des **valeurs manquantes**.  
   - Consultez sur [Kaggle](https://www.kaggle.com/datasets/gauravtopre/bank-customer-churn-dataset) la **définition de chaque colonne** pour comprendre le jeu de données.

---

## Étape 3 — Ajout de champs calculés

1. Ajoutez un **processeur "Calcul des champs"** juste après la source de données.  
2. Créez les champs suivants :
<br>2.1. Créez un champ binaire `is_churn` indiquant si le client a quitté la banque (1) ou non (0). 
<br>2.2. Créez un champ aléatoire `rand_key` qui permettra plus tard de séparer le dataset en deux parties (entraînement / validation)

##### Indication :
- `is_churn = IF(churn = 1, 1, 0)`
- `rand_key = Rand()`

---

## Étape 4 — Création d’un fork

1. Ajoutez un **processeur "Fork"** pour séparer le flux en deux branches :
2. **Branche 1 :** `Analyse de la distribution de la variable cible`
<br>Dans la **première sortie** du fork :
<br>2.1. Ajoutez un **Aggregate**.  
<br>2.2. Paramétrez : **Group By** : `is_churn` / **Opération** : `count(is_churn)`  
<br>2.3. Ajoutez une **cible de fichier** après cette agrégation : `Churn_Dispertion_table.qvd`
    - Remarque :
    Qlik Data Flow ne visualise que les **1000 premières lignes**.  
Pour voir les résultats complets, consultez le fichier cible généré dans votre **espace d’accueil Qlik** après exécution `Run Flux`.

    - Resultats attendus :
    `is_churn = 0 → 7963 clients`
    `is_churn = 1 → 2037 clients`
- *Ce déséquilibre pourrait biaiser un modèle classique, mais Qlik AutoML détecte et corrige automatiquement ce type de situation en ajustant les poids d’apprentissage des classes.*


3. **Branche 2 :** `Préparation pour l’entraînement / validation`
<br>3.1. Ajoutez un nouveau **Calcul de champs** dans la deuxième sortie du Fork
<br>3.2. Créez les champs suivants 
<br>3.2.1. Créez un champ `split` permettant de diviser le dataset en deux parties  `train`: 80 % pour l’entraînement / `val`: 20 % pour la validation.
<br>3.2.2  Créez un champ `balance_ratio` qui normalise le solde (balance) par rapport au salaire estimé (estimated_salary)
<br>3.2.3 Créez un champ `log_salary` permettant de stabiliser les valeurs extrêmes du salaire en appliquant une transformation logarithmique.
   

##### Indication :
- `split = IF(rand_key <= 0.8, 'train', 'val')` 
- `balance_ratio = balance / (estimated_salary + 1)`
- `log_salary = LOG(estimated_salary + 1)`

---

## Étape 7 — Séparation des jeux de données

Ajoutez un **Filtre** :
- Champ : `split`
- Condition : `split = "train"`

1. **Première sortie (train)** : Ajoutez une **Cible** : `Churn_train.qvd`

2. **Deuxième sortie (val)** 
<br>2.1. Ajoutez un **Sélectionneur de champs** et conservez uniquement :
   `country`, `age`, `balance`, `products_number`, `active_member`, `balance_ratio`, `customer_id`, `is_churn`
*(Ces colonnes correspondent aux paramètres du modèle AutoML de la partie 2.)* 
<br>2.2. Ajoutez un **Filtre** supplémentaire pour limiter les lignes : `15560000 < customer_id < 15576000` 
<br>2.3. Ajoutez enfin une **Cible de fichier** : `Churn_val_Filtred.txt`
    - Cela permet de réduire le volume d’échantillons pendant la phase de test automatique.

---

## Étape 8 — Exécution du flux

1. Cliquez sur **Run flow** en haut à droite.  
2. Une fois le flux exécuté, revenez à la page d’accueil Qlik Cloud.  
3. Vérifiez la présence des fichiers générés :
   - `Churn_Dispertion_table.qvd`
   - `Churn_train.qvd`
   - `Churn_val_Filtred.txt`

---

## Résultat attendu

![Flux final Qlik](/images/dataflow_final.png)
