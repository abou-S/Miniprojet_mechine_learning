📊 Benchmark et Interprétation des Modèles (8 Composantes Principales)
Après réduction dimensionnelle en conservant 8 composantes principales, voici les performances obtenues pour chaque modèle :

Modèle	Précision (classe 1)	Rappel (classe 1)	F1-score (classe 1)	Accuracy
KNN	0.88	0.47	0.61	0.95
Arbre de Décision	0.60	0.80	0.69	0.94
Random Forest	0.82	0.77	0.79	0.97
Régression Logistique (0.7)	0.59	0.87	0.70	0.94
XGBoost	0.80	0.93	0.86	0.98
📌 Remarque : Les valeurs de précision, rappel et F1-score concernent ici spécifiquement la classe positive (1).

📌 Interprétation des Résultats
XGBoost se démarque nettement comme le meilleur modèle :

Précision élevée (0.80) et excellent rappel (0.93).
F1-score équilibré (0.86) et accuracy globale de 98%.
Idéal pour maximiser à la fois la détection des cas positifs et la réduction des faux positifs.
Random Forest arrive en deuxième position :

Bonne précision (0.82) et rappel satisfaisant (0.77).
F1-score à 0.79 et accuracy de 97%.
Régression Logistique (seuil ajusté à 0.7) :

Rappel élevé (0.87), intéressant pour détecter un maximum de cas positifs.
Précision plus faible (0.59), ce qui augmente le nombre de faux positifs.
F1-score de 0.70.
Arbre de Décision :

Bon rappel (0.80) mais précision limitée (0.60).
F1-score de 0.69, performance correcte mais en retrait.
KNN :

Meilleure précision (0.88), mais rappel très faible (0.47), ce qui le rend moins fiable pour détecter efficacement la classe 1.
F1-score de 0.61, le plus faible du benchmark.
✅ Conclusion
XGBoost est le modèle à privilégier dans ce contexte, offrant le meilleur équilibre entre précision et rappel pour la détection de la classe positive.
Random Forest constitue une alternative intéressante avec des performances globales solides.
Régression Logistique reste acceptable, notamment pour maximiser le rappel après ajustement du seuil.
Les modèles KNN et Arbre de Décision montrent des performances plus limitées et sont moins adaptés à ce problème.
📌 Remarque personnelle :

En testant mes modèles avec 9 composantes principales, j’ai constaté qu'ils atteignaient tous des scores parfaits (100%) sur l’ensemble des métriques (précision, rappel, f1-score, etc.) sur les données de test. Après analyse, j’explique ce phénomène par deux raisons principales :

Les 9 premières composantes semblent capturer l’intégralité de l’information discriminante présente dans le dataset. Autrement dit, elles résument toute la variance utile pour différencier les classes. Cela rend la séparation dans l’espace réduit très simple pour les algorithmes.

Le dataset semble être soit très structuré, soit naturellement bien séparé (linéairement ou quasi-linéairement) après réduction. En projetant les données sur ces 9 composantes, la frontière de décision devient quasi triviale pour tous les modèles, d’où ces performances parfaites.

