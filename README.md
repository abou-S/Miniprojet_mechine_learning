# Projet Data — Analyse et Modélisation de l'Abandon Scolaire

Ce dépôt contient un projet d'analyse et de modélisation prédictive sur les données d'abandon scolaire. L'objectif est d'identifier les facteurs de risque et de développer des modèles de classification pour prédire l'abandon scolaire.

## Contenu du dépôt

- `MiniProjet_1_SeckAboubacrine.ipynb` — Notebook principal d'analyse et de modélisation.
- `Dataset_Abandon_Scolaire.csv` — Jeu de données sur l'abandon scolaire (2000 étudiants).
- `requirements.txt` — Liste des dépendances Python nécessaires.
- `env/` — Environnement virtuel Python (local) avec Jupyter et dépendances.
- `LICENSE` — Licence du projet.

## Prérequis

- Python 3.13 (recommandé, aligné avec l'environnement existant)
- macOS, Linux ou Windows (les commandes peuvent légèrement varier)

## Installation rapide

1) Cloner le dépôt (si nécessaire):

```bash
git clone <url-du-repo>
cd MinProjet
```

2) (Option A) Utiliser l'environnement virtuel fourni:

```bash
# Activer l'environnement virtuel existant
source env/bin/activate  # macOS / Linux
# Windows PowerShell : .\env\Scripts\Activate.ps1
```

3) (Option B) Recréer un nouvel environnement:

```bash
python3 -m venv env
source env/bin/activate
pip install --upgrade pip
# Installer les dépendances depuis requirements.txt
pip install -r requirements.txt
```

## Lancer le notebook

Depuis la racine du projet (avec l'environnement activé):

```bash
jupyter notebook
```

Ouvrez ensuite `MiniProjet_1_SeckAboubacrine.ipynb` dans le navigateur.

Alternatives:

```bash
# Lancer JupyterLab
jupyter lab

# Exécuter le notebook sans interface
jupyter nbconvert --to notebook --execute MiniProjet_1_SeckAboubacrine.ipynb \
  --output MiniProjet_1_SeckAboubacrine_executed.ipynb
```

## Contenu du notebook (aperçu détaillé)

### 1) EDA (Exploration des données)
- Chargement de `Dataset_Abandon_Scolaire.csv` et inspection (`head`, `info`, `describe`, valeurs manquantes).
- Visualisations: distributions de `Age`, `Nombre_retards`, `Note_moyenne`, répartition `Sexe`, `Situation_familiale`, et `Abandon`.
- Résumé descriptif: effectif 2000, moyenne d'âge ~20.6, présence moyenne ~84.6 %, notes ~13.4/20, ~8.4 % d'abandon.

### 2) Prétraitement et encodage
- Encodage de `Sexe` via `LabelEncoder`.
- Vectorisation de `Situation_familiale` avec `SentenceTransformer` CamemBERT (`dangvantuan/sentence-camembert-base`) et ajout des embeddings au DataFrame.
- Suppression de la colonne textuelle d'origine et normalisation des variables numériques (`StandardScaler`).
- Corrélations: `Abandon` est négativement corrélé à `Taux_presence` et `Note_moyenne`, positivement à `Nombre_retards`; `Age`/`Sexe` peu corrélés.

### 3) Réduction de dimension (PCA)
- PCA appliquée après embeddings pour compresser la forte dimensionalité.
- Conservation de 8 composantes principales capturant >90 % de la variance utile.

### 4) Modélisation et évaluation
- Split entraînement/test (80/20) sur les 8 composantes PCA.
- Modèles testés avec `GridSearchCV` (scoring `f1`):
  - KNN
  - Arbre de décision
  - Random Forest
  - Régression logistique (avec ajustement du seuil de décision et courbe ROC)
  - XGBoost

#### Tableau récapitulatif (classe positive = 1)

| Modèle                           | Précision | Rappel | F1-score | Accuracy |
|:---------------------------------|:---------:|:------:|:--------:|:--------:|
| KNN                              | 0.88      | 0.47   | 0.61     | 0.95     |
| Arbre de décision                | 0.60      | 0.80   | 0.69     | 0.94     |
| Random Forest                    | 0.82      | 0.77   | 0.79     | 0.97     |
| Régression logistique (seuil 0.7)| 0.59      | 0.87   | 0.70     | 0.94     |
| XGBoost                          | 0.80      | 0.93   | 0.86     | 0.98     |

### 5) Conclusions du notebook
- **XGBoost** est le meilleur compromis précision/rappel (F1 0.86, acc 0.98).
- **Random Forest** est une alternative robuste (F1 0.79, acc 0.97).
- **Régression logistique** avec seuil 0.7 maximise le rappel au prix de plus de faux positifs.
- Les performances des KNN et Arbre sont plus limitées pour la classe positive.
- Note méthodologique: avec 9 composantes PCA, tous les modèles atteignent 100 % en test — probablement dû à une séparation quasi parfaite de l'information discriminante capturée par ces composantes.

## Bonnes pratiques

- Activez l'environnement virtuel avant de lancer Jupyter.
- En cas d'erreurs d'import, installez/mettez à jour les dépendances dans l'env actif.
- Pour la reproductibilité, redémarrez le kernel et exécutez toutes les cellules dans l'ordre.

## Export des résultats

```bash
jupyter nbconvert --to html MiniProjet_1_SeckAboubacrine.ipynb
# ou
jupyter nbconvert --to pdf MiniProjet_1_SeckAboubacrine.ipynb
```

## Dépendances principales utilisées dans le notebook
- pandas, numpy, matplotlib, seaborn
- scikit-learn (prétraitement, PCA, modèles, métriques, GridSearchCV)
- sentence-transformers (CamemBERT)
- xgboost

## Licence

Ce projet est distribué sous la licence indiquée dans `LICENSE`.

## Auteur

- Seck Aboubacrine
