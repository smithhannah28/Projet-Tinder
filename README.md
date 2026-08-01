# Speed Dating — Analyse statistique des facteurs de succès

Projet réalisé dans le cadre du **Bloc 2 — Analyse exploratoire, descriptive et inférentielle de données**  
Formation Data Analyst

---

## Contexte

Ce projet analyse les facteurs qui influencent le succès lors de sessions de speed dating, à partir d'un jeu de données collecté à la **Columbia Business School** entre 2002 et 2004.

Les participants se rencontraient lors de soirées de speed dating et évaluaient chaque partenaire sur plusieurs critères. Ils renseignaient également leurs propres perceptions avant, juste après, et plusieurs semaines après l'événement.

---

## Dataset

| Variable | Valeur |
|---|---|
| Source | Columbia Business School (2002–2004) |
| Rencontres | 8 378 |
| Participants | 551 |
| Vagues | 21 |
| Variables | 195 |

Le fichier de données source (`Speed_Dating_Data.csv`) et sa documentation (`Speed_Dating_Data_Key.doc`) ne sont pas inclus dans ce dépôt en raison de leur taille. Ils sont disponibles sur [Kaggle](https://www.kaggle.com/datasets/annavictoria/speed-dating-experiment).

---

## Thèmes & Questions

### Thème 1 — La Perception de Soi

**Q1 — Les personnes qui s'évaluent mieux obtiennent-elles plus de correspondances ?**
- Variable : `perception_de_soi` = moyenne(attr3_1, sinc3_1, intel3_1, fun3_1, amb3_1)
- Test : Corrélation de Pearson
- Résultat : **H0 rejetée** — r = 0,148 · p = 0,042 *(corrélation faible mais significative)*

**Q2 — Vaut-il mieux être confiant, réaliste ou modeste ?**
- Variable : `biais` = perception_de_soi − évaluation_reçue → 3 groupes
- Test : ANOVA one-way
- Résultat : **H0 non rejetée** — F = 2,097 · p = 0,124 *(aucune différence significative entre les groupes)*

**Q3 — Le succès modifie-t-il la perception de soi après l'événement ?**
- Variables : `perception_de_soi` mesurée à T2 (juste après) et T3 (3–4 semaines après)
- Test : Corrélation de Pearson
- Résultat : **H0 non rejetée** — T2 : r = 0,029 · p = 0,649 · T3 : r = 0,077 · p = 0,227

---

### Thème 2 — Facteurs Saisonniers

> Printemps : vagues 6–9 & 18–21 (n = 201) · Automne : vagues 1–5 & 10–17 (n = 350)

**Q1 — Le nombre de correspondances diffère-t-il selon la saison ?**
- Test : Test t de Welch
- Résultat : **H0 non rejetée** — t = −0,519 · p = 0,604

**Q2 — La sélectivité varie-t-elle selon la saison ?**
- Variable : `taux_oui` = nombre de décisions positives / nombre de rencontres
- Test : Test t de Welch
- Résultat : **H0 non rejetée** — t = −0,022 · p = 0,983

**Q3 — Les critères de sélection varient-ils selon la saison ?**
- Variables : 6 traits évalués sur 100 points (attractivité, sincérité, intelligence, fun, ambition, intérêts communs)
- Test : Test t de Welch par trait
- Résultat : **3 traits significatifs sur 6**

| Trait | Printemps | Automne | Résultat |
|---|---|---|---|
| Attractivité | 19,62 | 24,47 | Automne > Printemps ★ |
| Sincérité | 17,52 | 17,16 | ns |
| Intelligence | 19,57 | 20,52 | ns |
| Fun | 17,79 | 17,25 | ns |
| Ambition | 12,26 | 9,96 | Printemps > Automne ★ |
| Intérêts communs | 13,24 | 11,02 | Printemps > Automne ★ |

---

## Méthodologie

### Nettoyage & Préparation

- Encodage MacRoman (caractères spéciaux)
- Imputation des valeurs manquantes par la médiane par individu (`iid`)
- Agrégation à une ligne par participant (`drop_duplicates`)
- Création de variables dérivées : `perception_de_soi`, `biais`, `taux_oui`

### Tests statistiques utilisés

| Test | Usage | Seuil |
|---|---|---|
| Corrélation de Pearson | Relation entre deux variables continues | α = 0,05 |
| ANOVA one-way | Comparaison de 3 groupes indépendants | α = 0,05 |
| Test t de Welch | Comparaison de 2 groupes indépendants | α = 0,05 |
| Shapiro-Wilk | Vérification de la normalité | α = 0,05 |

> La normalité n'est pas vérifiée pour la plupart des variables (Shapiro-Wilk p < 0,001). Les tests restent robustes pour des échantillons de taille n > 30 grâce au théorème central limite.

---

## Principaux résultats

- La perception de soi est liée au succès, mais faiblement (r = 0,148, ~2 % de variance expliquée)
- Être confiant ou modeste ne change pas le nombre de correspondances obtenues
- Le succès lors d'une session de speed dating ne modifie pas la perception de soi, même plusieurs semaines après
- La saison n'influence ni le nombre de correspondances ni le comportement de sélection
- En revanche, les critères de sélection *déclarés* varient selon la saison, alors que les comportements réels restent stables

---

## Limites

- Groupes saisonniers déséquilibrés : n = 201 (printemps) vs n = 350 (automne)
- Données auto-déclarées — biais de désirabilité sociale possible
- Causalité non établie entre perception de soi et correspondances
- Population spécifique (étudiants d'une école de commerce américaine) — faible généralisabilité

---

## Pistes d'analyses futures

- **Régression multiple** : modéliser le succès en combinant plusieurs variables simultanément
- **Analyse par genre** : les effets de la perception de soi diffèrent-ils entre hommes et femmes ?
- **Analyse temporelle** : évolution de la perception de soi sur plusieurs vagues
- **Clustering** : segmenter les profils de participants selon leurs préférences déclarées

---

## Stack technique

![Python](https://img.shields.io/badge/Python-3.10-blue)
![Pandas](https://img.shields.io/badge/Pandas-2.0-blue)
![NumPy](https://img.shields.io/badge/NumPy-1.24-blue)
![SciPy](https://img.shields.io/badge/SciPy-1.10-blue)
![Plotly](https://img.shields.io/badge/Plotly-5.x-blue)

```
pandas · numpy · scipy.stats · plotly.express
```
