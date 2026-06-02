# Semaine 1 — Probabilités, bruit et estimation

> **Module :** `00-foundations/probabilities-estimation`  
> **Niveau :** Débutant — aucun prérequis mathématique avancé  
> **Durée estimée :** 2 heures de lecture + exercices  

---

## Pourquoi cette semaine est indispensable

Tout le reste du parcours — filtre de Kalman, EKF, fusion multi-capteurs, navigation autonome, IA embarquée — repose sur une seule question :

> **Comment exploiter des mesures imparfaites pour produire la meilleure estimation possible ?**

Cette semaine pose les fondations pour répondre à cette question.

---

## 1. Pourquoi une mesure n'est jamais parfaite

Dans le monde réel, aucun capteur n'est exactement juste.

Prenons un GPS qui mesure la position d'un véhicule immobile.  
Position réelle : **100 m**

| Mesure n° | Valeur lue (m) |
|-----------|---------------|
| 1 | 98 |
| 2 | 101 |
| 3 | 100 |
| 4 | 102 |
| 5 | 99 |

Le véhicule n'a pas bougé. Pourtant les mesures sont toutes différentes.

Cette variation s'appelle le **bruit de mesure**.

---

## 2. D'où vient le bruit ?

Le bruit regroupe toutes les perturbations qui affectent un capteur.

**Pour un GPS :**
- erreurs de réception satellite
- réflexions du signal sur les bâtiments
- conditions atmosphériques

**Pour une IMU (centrale inertielle) :**
- vibrations mécaniques
- dérive gyroscopique
- bruit électronique interne

**Pour une caméra :**
- variations de luminosité
- bruit du capteur photographique
- artefacts de compression vidéo

> **À retenir :** le bruit n'est pas un défaut de fabrication. C'est une propriété physique inévitable de tout système de mesure.

---

## 3. Modéliser le bruit : la variable aléatoire

Pour travailler mathématiquement avec le bruit, on l'écrit comme une variable aléatoire :

```
Mesure = Valeur réelle + Bruit
```

Exemples concrets :

| Valeur réelle | Bruit tiré au hasard | Mesure obtenue |
|---------------|---------------------|---------------|
| 100 m | +2 m | 102 m |
| 100 m | −3 m | 97 m |
| 100 m | +0.5 m | 100.5 m |

On ne connaît jamais la valeur exacte du bruit à l'instant $k$.  
En revanche, on peut **caractériser son comportement statistique**.  
C'est exactement ce que font la moyenne, la variance et la covariance.

---

## 4. La moyenne — première estimation

La première façon de réduire l'effet du bruit est de **moyenner plusieurs mesures**.

Soit les 5 mesures GPS : `98, 101, 100, 102, 99`

$$\bar{x} = \frac{98 + 101 + 100 + 102 + 99}{5} = \frac{500}{5} = 100$$

La moyenne vaut exactement 100 m — la vraie valeur.

> **Propriété clé :** plus on accumule de mesures, plus la moyenne se rapproche de la valeur réelle (sous réserve que le bruit soit centré en zéro — ce qu'on appelle un bruit **non biaisé**).

---

## 5. La variance — mesurer la précision

Deux capteurs peuvent avoir la même moyenne mais une qualité très différente.

**Capteur A** (GPS de qualité) :
```
100, 101, 99, 100, 100
```

**Capteur B** (GPS bon marché) :
```
90, 110, 95, 105, 100
```

Les deux ont une moyenne proche de 100.  
Mais le capteur B fluctue énormément — il est **beaucoup moins fiable**.

La **variance** mesure cette dispersion autour de la moyenne :

$$\sigma^2 = \frac{1}{N} \sum_{i=1}^{N} (x_i - \bar{x})^2$$

| Capteur | Mesures | Variance |
|---------|---------|----------|
| A | 100, 101, 99, 100, 100 | 0.4 m² |
| B | 90, 110, 95, 105, 100 | 50 m² |

> **Règle intuitive :** variance faible = capteur précis = on lui fait plus confiance.

Le filtre de Kalman utilise exactement cette règle pour décider **quelle mesure croire davantage**.

---

## 6. L'écart-type — l'incertitude en unité physique

La variance est en m² (mètres carrés). C'est difficile à interpréter directement.

**L'écart-type** est la racine carrée de la variance :

$$\sigma = \sqrt{\sigma^2}$$

Il a la **même unité que la mesure**.

Exemple :
```
GPS : position = 100 ± 2 m
```
Cela signifie : les mesures se situent généralement à moins de **2 mètres** de la vraie valeur.

| Grandeur | Unité |
|----------|-------|
| Position | m |
| Variance | m² |
| Écart-type σ | m ✓ (lisible) |

> **En pratique :** quand on dit qu'un GPS a une précision de 3 m, on parle de son écart-type.

---

## 7. La distribution gaussienne — la forme du bruit

Dans la plupart des systèmes physiques, si l'on trace l'histogramme des mesures, on obtient une **courbe en cloche** : la **distribution gaussienne** (ou loi normale).

```
        ████
      ████████
    ████████████
  ████████████████
 ▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁
 ←    -3σ  -2σ  -σ  μ  +σ  +2σ  +3σ    →
```

**Ce que ça signifie :**
- la grande majorité des mesures est proche de la moyenne μ
- les erreurs importantes sont rares
- les très grandes erreurs sont exceptionnelles

**La règle des 1-2-3 sigma :**

| Intervalle | Probabilité |
|-----------|------------|
| μ ± 1σ | 68 % des mesures |
| μ ± 2σ | 95 % des mesures |
| μ ± 3σ | 99.7 % des mesures |

> **Hypothèse centrale du filtre de Kalman :** le bruit est gaussien. Si cette hypothèse est vérifiée, le filtre est optimal.

---

## 8. La covariance — le lien entre deux variables

Jusqu'ici on a parlé d'une seule mesure à la fois (la position).  
En navigation, on suit souvent **plusieurs grandeurs simultanément** : position et vitesse, par exemple.

Ces grandeurs peuvent être **liées** entre elles.

**Exemple concret :**

Un drone freine brusquement. Son erreur de position et son erreur de vitesse évoluent ensemble : si on surestime la position, on surestime aussi la vitesse.  
→ Les deux erreurs sont **corrélées positivement**.

La **covariance** mesure cette relation :

| Cas | Covariance | Interprétation |
|-----|-----------|----------------|
| Les deux erreurs augmentent ensemble | > 0 (positive) | corrélées |
| Une augmente quand l'autre diminue | < 0 (négative) | anti-corrélées |
| Indépendantes | ≈ 0 | non liées |

> **Pourquoi c'est crucial :** dans un filtre de Kalman, la **matrice de covariance P** contient toutes ces relations. Elle dit au filtre non seulement "combien" il est incertain, mais aussi "dans quelle direction" cette incertitude est structurée.

---

## 9. Synthèse — ce que le filtre a besoin de savoir

Pour combiner intelligemment les mesures de plusieurs capteurs, le filtre a besoin de connaître pour chacun :

| Information | Ce qu'elle dit | Outil mathématique |
|-------------|---------------|-------------------|
| Valeur mesurée | Ce que le capteur a lu | Mesure z |
| Biais moyen | Est-ce que le capteur se trompe systématiquement ? | Moyenne |
| Précision | À quel point les mesures fluctuent | Variance σ² |
| Unité compréhensible | L'incertitude exprimée comme la mesure | Écart-type σ |
| Relations entre variables | Est-ce que deux erreurs évoluent ensemble ? | Covariance |

Ces informations permettent de calculer une **estimation optimale** : celle qui minimise l'erreur quadratique moyenne.

---

## 10. Applications aéronautiques et spatiales

Ces notions sont au cœur des systèmes les plus critiques :

| Système | Capteurs fusionnés | Ce qui est estimé |
|---------|-------------------|------------------|
| Drone autonome | GPS + IMU + baromètre | position, vitesse, altitude |
| Avion commercial | GPS + centrale inertielle + radio-altimètre | trajectoire, attitude |
| Satellite | Gyroscopes + capteurs stellaires | orientation dans l'espace |
| Lanceur spatial | IMU + GPS + télémétrie | trajectoire de montée |
| Voiture autonome | LiDAR + caméra + GPS + IMU | position, obstacles |

Dans tous ces cas, le système doit **décider quelle source croire** à chaque instant.  
C'est exactement le rôle du filtre de Kalman — et tout repose sur les notions de cette semaine.

---

## Résumé

| Notion | Définition | Rôle dans le filtre |
|--------|-----------|---------------------|
| Bruit | Perturbation aléatoire d'une mesure | Ce que le filtre cherche à réduire |
| Moyenne | Estimation centrale | La valeur que le filtre produit |
| Variance σ² | Dispersion autour de la moyenne | Mesure de confiance dans une source |
| Écart-type σ | √variance, même unité que la mesure | Incertitude lisible (ex : ±2 m) |
| Distribution gaussienne | Courbe en cloche, bruit physique typique | Hypothèse centrale du filtre de Kalman |
| Covariance | Lien entre deux variables | Contenu de la matrice P du filtre |

---

## Prochaine étape

**Semaine 2 — Modèles d'état et représentation dynamique des systèmes**

On va voir comment décrire mathématiquement l'évolution d'un système dans le temps :  
comment un drone passe d'une position à une autre, et comment cette évolution modifie notre incertitude.
