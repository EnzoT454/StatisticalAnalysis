# Statistical Analysis & Bias Evaluation in Real-World Data

#### IFT-3700 – Science des Données

Ce projet contient deux études statistiques indépendantes :
1.  **Reddit Weekends** – Comparaison de l’activité Reddit en semaine vs week-end    
2.  **Chess Ratings** – Analyse du biais de surreprésentation dans les classements Elo
    
Le projet met l’accent sur :
*   tests statistiques paramétriques et non paramétriques    
*   hypothèses de normalité    
*   théorème central limite   
*   visualisation de distributions    
*   tests de permutation 
*   interprétation critique des résultats
    
## Structure du projet

```text
.
├── reddit_weekends.py
├── reddit_weekends.ipynb
├── chess_rating.py
├── chess_rating.ipynb
├── data/
│   ├── reddit-counts.json.gz
│   ├── standard_oct22frl_xml.xml
│   └── standard_oct22frl_xml.zip
├── requirements.txt
└── README.md
```

## Outils utilisés

*   Python 3.10+    
*   NumPy    
*   Pandas    
*   SciPy    
*   Matplotlib    
*   Seaborn    
*   tqdm   
*   JupyterLab
    

## Installation & Exécution

1- Créer un environnement virtuel

```bash
python3 -m venv venvsource venv/bin/activate
```

2- Installer les dépendances

```bash
pip install -r requirements.txt
```


3- Lancer les notebooks

```bash
jupyter lab
```


4- Exécuter les scripts directement

### Reddit Weekends

```bash
python3 reddit_weekends.py data/reddit-counts.json.gz
```

## Partie 1 – Reddit Weekends

### Question

> Y a-t-il une différence significative entre le nombre de commentaires Reddit en semaine et le week-end (r/canada, 2012–2013) ?

### Méthodologie

1.  Filtrage du subreddit canada  
2.  Filtrage des années 2012–2013   
3.  Séparation weekday vs weekend 
4.  Test t de Student  
5.  Vérification des hypothèses :   
    *   normalité (scipy.stats.normaltest)
    *   variance égale (scipy.stats.levene)       
6.  Transformations (log, sqrt, etc.) 
7.  Application du Théorème Central Limite  
8.  Test non paramétrique Mann–Whitney U 

### Graphes

#### 1- Histogramme des données originales

Montre la distribution biaisée des commentaires.
```text
images/reddit_no_transform.png
```

#### 2- Histogramme après transformation log

Montre amélioration partielle de normalité.
```text  
images/reddit_log_transform.png
```

#### 3- Histogramme après CLT (moyennes hebdomadaires)

Montre distribution plus proche de normale.
```text
images/reddit_clt.png
```

### Résultats clés

*   Test T initial → invalide (normalité échoue)
*   Transformation log → améliore partiellement
*   CLT → hypothèses satisfaites
*   Mann-Whitney U → p ≈ 8.6e-53
    

### Conclusion

Il existe une différence statistiquement significative entre les jours de semaine et les week-ends.
En moyenne, **plus de commentaires sont publiés en semaine que le week-end** sur r/canada.
La méthode la plus robuste ici est l’approche basée sur le **Théorème Central Limite**, car elle respecte mieux les hypothèses du test t.

## Partie 2 – Chess Ratings

### Question

> Peut-on conclure que les hommes sont meilleurs aux échecs parce que les meilleurs joueurs sont des hommes ?

### Méthodologie

1.  Parsing du fichier XML FIDE  
2.  Nettoyage des données :
    *   suppression NaN    
    *   conversion types numériques   
    *   filtrage joueurs < 20 ans 
3.  Binning des scores Elo (largeur 50, 1000–2900)  
4.  Comparaison distributions :
    *   counts bruts  
    *   counts normalisés   
5.  Tests de permutation :
    *   simulation de groupes aléatoires
    *   comparaison des maxima   

### Graphes

#### 1- Distribution Elo – counts bruts

```text  
images/chess_counts.png
```

#### 2- Distribution Elo – counts normalisés

```text  
images/chess_counts_normalized.png
```

#### 3- Histogramme des différences simulées (permutation)

```text  
images/chess_permutation_hist.png
```

### Résultats clés

*   Différence moyenne simulée (permutations) ≈ 86 Elo
*   Différence réelle max M-F = 181 Elo
    

Une partie importante de la différence peut être expliquée **uniquement par la taille du groupe masculin plus grand**.

### Conclusion

Comparer uniquement les meilleurs joueurs est statistiquement trompeur lorsque les groupes ont des tailles différentes.
La différence observée n’implique pas nécessairement une supériorité intrinsèque d’un groupe.

## Interprétation Critique

Les données sont une représentation partielle du monde réel.

Limites :
*   Biais de participation
*   Facteurs sociaux et culturels
*   Opportunités inégales
*   Elo mesure performance actuelle, pas potentiel

## Résumé du Projet

Ce projet montre que :

*   Les tests statistiques doivent respecter leurs hypothèses.
*   Transformer les données peut aider, mais ne garantit rien.
*   Le Théorème Central Limite est puissant.
*   Les tests non paramétriques sont utiles quand les hypothèses échouent.
*   Comparer des maxima dans des groupes de tailles différentes peut mener à des conclusions biaisées.
*   Les données ne racontent jamais toute l’histoire.








