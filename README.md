# Credit-Risk Modelling with Default of Credit Card Clients

---

# 1. Project Information

- **Project Title:** Credit-Risk Modelling with Default of Credit Card Clients
- **Group Name:** Lilia & Victoria
- **Group Members:**  
  - Student 1 – Lilia Mecellem
  - Student 2 – Victoria Oladele

- **Course Name:** AI In Finance
- **Instructor:** Nicolas De Roux & Mohamed EL FAKIR
- **Submission Date:** 20/04/2026

---

# 2. Project Description

Le secteur financier est confronté à un enjeu majeur : la gestion du risque de crédit, c'est-à-dire la probabilité qu'un emprunteur ne soit pas en mesure d'honorer ses obligations de remboursement. Avec la démocratisation croissante des cartes de crédit, les banques sont exposées à des volumes de défauts potentiels de plus en plus importants, susceptibles de générer des pertes significatives et de déstabiliser leurs portefeuilles. Ce projet s'attaque à la prédiction du défaut de paiement des clients de cartes de crédit à partir du dataset UCI Default of Credit Card Clients, collecté auprès d'une banque taïwanaise sur la période avril–septembre 2005. Le problème est à la fois financièrement critique et techniquement stimulant : il s'agit d'une tâche de classification déséquilibrée où la classe minoritaire (les clients en défaut) concentre le coût métier le plus élevé. Résoudre ce problème bénéficie à de nombreux acteurs : les directions du risque souhaitant ajuster leurs politiques d'octroi de crédit, les gestionnaires de portefeuille cherchant à surveiller leur exposition en temps réel, et les régulateurs qui exigent des modèles de risque robustes et explicables.

---

# 3. Project Goal

Le projet vise à construire un système de classification binaire capable de prédire si un client de carte de crédit va faire défaut de paiement le mois suivant, à partir de son profil socio-démographique, de son historique de remboursement et de ses données financières. Une solution est considérée réussie si elle identifie correctement la majorité des vrais défauts (recall élevé sur la classe 1) tout en maintenant une précision raisonnable, minimisant les détections manquées sans générer trop de fausses alarmes. L'objectif final est un modèle à la fois statistiquement performant et opérationnellement utilisable dans un contexte bancaire réel, où le coût d'un défaut non détecté dépasse largement celui d'un client solvable incorrectement signalé.

---

# 4. Task Definition

Variables d'entrée : 21 variables sélectionnées, organisées en trois familles :
Profil client : LIMIT_BAL, SEX, EDUCATION, MARRIAGE, AGE
Historique de paiement : PAY_0, PAY_2, PAY_3, PAY_4, PAY_5, PAY_6 (statut de remboursement mensuel, codé de -2 à 9)
Données financières : BILL_AMT1–6 (montants facturés mensuels), PAY_AMT1–6 (montants payés mensuels)

Variable cible : default_payment_next_month — binaire (0 = pas de défaut, 1 = défaut)

Métriques d'évaluation :
AUC-ROC (principale) : mesure la capacité de discrimination globale indépendamment du seuil de décision
F1-score (classe 1) : équilibre précision et recall sur la classe minoritaire
Recall (classe 1) : proportion de vrais défauts correctement détectés — critique d'un point de vue métier
Précision (classe 1) : fiabilité des prédictions de défaut

---

# 5. Dataset Description

Vue d'ensemble
Le dataset utilisé est le UCI Default of Credit Card Clients, disponible sur : https://archive.ics.uci.edu/ml/datasets/default+of+credit+card+clients . Il contient 30 000 observations et 24 variables au total, dont 23 variables explicatives et 1 variable cible. Les données ont été collectées auprès de clients de cartes de crédit d'une banque taïwanaise sur la période avril–septembre 2005. La variable cible est default_payment_next_month, qui vaut 1 si le client fait défaut le mois suivant, et 0 sinon.
Description des Variables
Les variables sont organisées en trois grandes familles. Le profil client regroupe LIMIT_BAL (limite de crédit, numérique), SEX (sexe, catégorielle), EDUCATION (niveau d'éducation, ordinale de 1 à 4), MARRIAGE (statut marital, catégorielle) et AGE (âge en années, numérique). L'historique de paiement est capturé par les variables PAY_0 à PAY_6, qui représentent le statut de remboursement mensuel de septembre à avril 2005, codé de -2 (avance sur paiement) à 9 (retard de 9 mois ou plus) — ce sont des variables ordinales. Enfin, les données financières comprennent BILL_AMT1 à BILL_AMT6 (montants facturés chaque mois, numériques) et PAY_AMT1 à PAY_AMT6 (montants effectivement payés chaque mois, numériques).
Variable Cible
La variable cible est default_payment_next_month. Elle indique si le client n'a pas effectué son paiement minimum lors du mois suivant la période d'observation. C'est une variable binaire : elle vaut 0 si le client a payé (pas de défaut) et 1 si le client n'a pas payé (défaut).
Types de Variables
Le dataset contient principalement des variables numériques continues (LIMIT_BAL, AGE, BILL_AMT1–6, PAY_AMT1–6), des variables ordinales (EDUCATION, MARRIAGE, PAY_0 à PAY_6) et des variables catégorielles nominales (SEX). La variable cible est binaire.
Distribution des Données
Le dataset présente un déséquilibre de classes marqué : 78 % des clients n'ont pas fait défaut contre 22 % qui ont fait défaut. Un modèle naïf prédisant toujours 0 atteindrait 78 % d'accuracy sans détecter un seul défaut, ce qui illustre pourquoi l'accuracy seule est une métrique trompeuse ici. Parmi les signaux prédictifs clés, PAY_0 affiche des taux de défaut allant de ~13 % pour les clients à jour à plus de 50 % pour ceux en retard sévère, et LIMIT_BAL est inversement corrélé au risque. Plusieurs variables BILL_AMT sont fortement corrélées entre elles, ce qui a motivé l'étape de sélection de variables.
Qualité des Données
Aucune valeur manquante n'a été détectée dans le dataset. En revanche, des valeurs de catégories non documentées ont été identifiées dans EDUCATION (valeurs 0, 5, 6) et MARRIAGE (valeur 0), non décrites dans la documentation officielle — elles ont été regroupées dans la catégorie "autre". Le déséquilibre de classes (78/22) constitue le principal défi structurel et a été traité via des stratégies spécifiques à chaque modèle. Une forte corrélation inter-variables entre BILL_AMT1 et BILL_AMT6 a également été observée, justifiant la sélection de variables en amont. Aucun doublon n'a été détecté.


---

# 6. Data Preprocessing

1. Traitement des valeurs de catégories non documentées
Les valeurs 0, 5 et 6 dans EDUCATION et la valeur 0 dans MARRIAGE ne sont pas définies dans la documentation du dataset. Elles ont été regroupées dans la catégorie "autre" existante (EDUCATION → 4, MARRIAGE → 3) afin d'éviter d'introduire du bruit lors de l'encodage.

3. Sélection de variables via Lasso + Random Forest
Pour réduire la dimensionnalité et éliminer les variables redondantes ou peu informatives, deux méthodes complémentaires ont été combinées. La régression Lasso (pénalité L1) annule les coefficients des variables peu utiles, capturant la pertinence linéaire. L'importance des variables par Random Forest (réduction d'impureté de Gini) capte les relations non linéaires. Toute variable jugée importante par au moins l'une des deux méthodes a été retenue, avec conservation systématique des 5 variables de profil client. Résultat : 21 variables finales, limitant le surapprentissage.

4. Normalisation / Mise à l'échelle
Un StandardScaler a été appliqué aux variables numériques pour la régression logistique et le réseau de neurones MLP, tous deux sensibles aux différences d'échelle entre features. XGBoost et Random Forest étant invariants à l'échelle, aucune normalisation n'a été appliquée pour ces modèles.

5. Gestion du déséquilibre de classes
Face au ratio 78/22, trois stratégies ont été adoptées selon le modèle :
class_weight='balanced' pour la régression logistique et le Random Forest
scale_pos_weight ≈ 3,52 (ratio négatifs/positifs) pour XGBoost, intégré nativement dans l'algorithme
SMOTE appliqué exclusivement sur le jeu d'entraînement pour le MLP, afin d'éviter toute fuite d'information vers le jeu de test

6. Séparation train/test
Un split stratifié 80/20 a été appliqué (24 000 observations en entraînement / 6 000 en test), préservant le taux de défaut de 22 % dans les deux ensembles et garantissant une évaluation non biaisée sur des données jamais vues.

---

# 7. Modeling Approach

Modèles Utilisés
Quatre algorithmes ont été entraînés et comparés :
Régression Logistique — modèle linéaire interprétable servant de baseline ; class_weight='balanced'
Random Forest — ensemble d'arbres de décision capturant les non-linéarités ; class_weight='balanced'
XGBoost — gradient boosting sur arbres de décision ; gestion native du déséquilibre via scale_pos_weight ; référence sur les données tabulaires
MLP (Perceptron Multicouche) — réseau de neurones dense entraîné sur données suréchantillonnées par SMOTE


Stratégie de Modélisation
La régression logistique a servi de modèle baseline — simple, interprétable, établissant un seuil minimal de performance à dépasser. Les trois modèles non linéaires ont ensuite été évalués pour déterminer si la complexité supplémentaire apportait des gains significatifs. Chaque modèle a reçu une stratégie de gestion du déséquilibre adaptée à son architecture, plutôt qu'une approche uniforme. Une cross-validation stratifiée à 3 folds a été réalisée sur le jeu d'entraînement pour chaque modèle, afin d'obtenir des estimations de performance stables avant l'évaluation finale. Aucun tuning extensif des hyperparamètres n'a été effectué dans cette itération — les modèles ont été entraînés avec des paramètres raisonnables et des corrections du déséquilibre, le tuning étant identifié comme axe d'amélioration futur.

Métriques d'Évaluation 
AUC-ROC : Métrique de classement principale. Elle mesure la capacité du modèle à séparer les classes sur tous les seuils de décision possibles, la rendant robuste au déséquilibre et au choix de seuil.
F1-score (classe 1) : Moyenne harmonique de la précision et du recall sur la classe défaut. Appropriée quand faux positifs et faux négatifs ont tous deux un coût, dans un contexte déséquilibré.
Recall (classe 1) : Proportion de vrais défauts correctement identifiés. Particulièrement important ici car manquer un vrai défauteur (faux négatif) est généralement plus coûteux qu'une fausse alarme dans le domaine du risque de crédit.
Précision (classe 1) : Proportion de défauts prédits qui sont de vrais défauts. Complémentaire au recall — ensemble ils définissent le compromis précision/recall que la banque doit calibrer selon son appétit au risque.

L'accuracy a été délibérément exclue comme métrique principale en raison du déséquilibre de classes.

---

# 8. Project Structure

Le projet est organisé autour d'un notebook principal autoportant, credit_risk_project_final_Lilia_Victoria.ipynb, qui contient l'intégralité du pipeline : chargement des données, analyse exploratoire, prétraitement, sélection de variables, entraînement des modèles, cross-validation et évaluation finale. Le dataset brut default_of_credit_card_clients.xls doit être placé à la racine du projet pour que le notebook puisse le charger directement. La présentation Modelisation-du-Risque-de-Credit.pdf accompagne le notebook et synthétise les résultats sous forme de slides. Un fichier README.md documente le projet et fournit les instructions d'installation et de lancement.

---

# 9. Installation

Pour reproduire le projet, il suffit d'avoir Python 3.8 ou supérieur installé ou Google Colab, puis d'installer les dépendances via la commande pip install pandas numpy scikit-learn xgboost imbalanced-learn matplotlib seaborn openpyxl, ou via pip install -r requirements.txt si un fichier requirements est fourni. Le notebook se lance ensuite avec jupyter notebook credit_risk_project_final_Lilia_Victoria.ipynb et s'exécute de bout en bout dans l'ordre des cellules.
