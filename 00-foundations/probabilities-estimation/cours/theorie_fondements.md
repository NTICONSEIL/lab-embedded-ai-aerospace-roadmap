# Fondements théoriques — Probabilités et estimation

> **À lire avant les notebooks** `01_bayes_intro`, `02_gaussian_distributions`, `03_bayesian_sequential_update`  
> **Niveau :** L1-L2  
> **Objectif :** comprendre les concepts manipulés dans le code, pas seulement les exécuter

---

## Introduction

Les trois notebooks de ce module construisent progressivement un estimateur capable de suivre la position d'un drone malgré des mesures GPS bruitées.

Pour que le code ait du sens, il faut d'abord comprendre les idées qu'il met en œuvre. Ce document les explique dans l'ordre où elles apparaissent.

---

## 1. Le théorème de Bayes

### L'idée centrale

Imaginons que vous cherchez à estimer la position d'un drone. Vous avez deux sources d'information :

1. **Ce que vous croyez** avant de regarder le capteur, par exemple, vous savez que le drone était à 100 m il y a une seconde et avançait à 2 m/s. C'est votre **croyance a priori**.
2. **Ce que le capteur vous dit** : le GPS lit 104 m. C'est la **mesure**.

Le théorème de Bayes est la règle qui dit comment combiner ces deux sources pour former une **croyance mise à jour** (le **posterior**) :

$$P(\text{état} \mid \text{mesure}) \propto P(\text{mesure} \mid \text{état}) \times P(\text{état})$$

En français :

```
Croyance après mesure  ∝  Vraisemblance de la mesure  ×  Croyance avant mesure
     (posterior)                  (likelihood)                  (prior)
```

Le symbole $\propto$ signifie "proportionnel à", on normalise ensuite pour que la probabilité totale vaille 1.

### Les trois termes expliqués

**Le prior $P(\text{état})$**  
C'est tout ce que l'on sait *avant* de regarder la mesure. Dans le cas du drone : "je pense qu'il est autour de 102 m, avec une incertitude de 5 m."

**La likelihood $P(\text{mesure} \mid \text{état})$**  
C'est la question : "si le drone était vraiment à cette position, quelle probabilité aurait-on d'obtenir cette mesure GPS ?" Un GPS précis rend les mesures proches de la vraie position très probables, les mesures lointaines très improbables.

**Le posterior $P(\text{état} \mid \text{mesure})$**  
C'est le résultat : la distribution mise à jour après avoir incorporé la mesure. C'est ce que le filtre produit à chaque pas de temps.

### Ce que ça change concrètement

Le posterior est toujours un compromis entre prior et mesure, tiré vers la source **la plus précise** :

- Si le GPS est très précis (petit $\sigma$) → le posterior se rapproche de la mesure GPS
- Si le GPS est bruité (grand $\sigma$) → le posterior reste proche du prior
- Les deux ont la même précision → le posterior est à mi-chemin, et **plus précis que les deux**

> **Point clé :** fusionner deux sources d'information réduit toujours l'incertitude. C'est la garantie fondamentale de l'estimation bayésienne.

---

## 2. La distribution gaussienne

### Pourquoi la gaussienne ?

Le théorème de Bayes s'applique à n'importe quelle distribution. Mais en pratique, on fait presque toujours l'hypothèse que le bruit est **gaussien** (loi normale). Pourquoi ?

Premièrement, c'est souvent vrai : le **théorème central limite** dit que la somme de nombreuses perturbations indépendantes converge vers une gaussienne, quelle que soit la distribution de chaque perturbation. Le bruit d'un capteur étant la somme de nombreuses sources (thermique, électronique, mécanique...), il est souvent bien modélisé par une gaussienne.

Deuxièmement, c'est mathématiquement commode : le produit de deux gaussiennes est une gaussienne. Cela signifie que si le prior est gaussien et que la likelihood est gaussienne, le posterior l'est aussi — et on peut le calculer analytiquement, sans approximation numérique.

### Définition

Une variable aléatoire $X$ suit une loi normale $\mathcal{N}(\mu, \sigma^2)$ si sa densité de probabilité est :

$$f(x) = \frac{1}{\sigma\sqrt{2\pi}} \exp\!\left(-\frac{(x - \mu)^2}{2\sigma^2}\right)$$

Les deux paramètres suffisent à décrire entièrement la distribution :

| Paramètre | Symbole | Rôle |
|-----------|---------|------|
| Moyenne | $\mu$ | Centre de la distribution — meilleure estimation |
| Variance | $\sigma^2$ | Dispersion — incertitude au carré |
| Écart-type | $\sigma$ | Racine de la variance — incertitude en unité physique |

### La règle des 1-2-3 sigma

Une propriété essentielle de la gaussienne est la concentration de la probabilité autour de la moyenne :

| Intervalle | Probabilité |
|-----------|------------|
| $[\mu - \sigma,\ \mu + \sigma]$ | 68.3 % |
| $[\mu - 2\sigma,\ \mu + 2\sigma]$ | 95.4 % |
| $[\mu - 3\sigma,\ \mu + 3\sigma]$ | 99.7 % |

**Application directe :** si un GPS annonce une position de 100 m avec $\sigma = 3$ m, on est sûr à 95 % que la vraie position est entre 94 m et 106 m.

### La gaussienne multivariée

Quand l'état du système comporte plusieurs grandeurs (position *et* vitesse, par exemple), on travaille avec une **gaussienne multivariée** :

$$\mathbf{x} \sim \mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$$

- $\boldsymbol{\mu}$ est maintenant un **vecteur** de moyennes
- $\boldsymbol{\Sigma}$ est une **matrice de covariance**

La matrice $\boldsymbol{\Sigma}$ contient :
- sur la diagonale : la variance de chaque variable (son incertitude propre)
- hors diagonale : la covariance entre deux variables (leur corrélation)

**Exemple :** pour un état $[position, vitesse]$, la matrice de covariance est :

$$\boldsymbol{\Sigma} = \begin{bmatrix} \sigma_{pos}^2 & \sigma_{pos,vit} \\ \sigma_{pos,vit} & \sigma_{vit}^2 \end{bmatrix}$$

Un terme hors-diagonale non nul signifie que les erreurs de position et de vitesse sont liées — ce qui est physiquement réaliste.

### La transformation linéaire d'une gaussienne

Cette propriété est celle qui rend le filtre de Kalman exact.

**Si** $\mathbf{x} \sim \mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$ et $\mathbf{y} = \mathbf{A}\mathbf{x} + \mathbf{b}$ (transformation linéaire), **alors** :

$$\mathbf{y} \sim \mathcal{N}(\mathbf{A}\boldsymbol{\mu} + \mathbf{b},\ \mathbf{A}\boldsymbol{\Sigma}\mathbf{A}^T)$$

En clair : faire avancer un drone (appliquer un modèle de mouvement linéaire à son état) transforme la gaussienne en une autre gaussienne. La distribution reste gaussienne — on n'a pas besoin d'approximation.

C'est exactement ce que fait la **phase de prédiction** du filtre de Kalman.

---

## 3. La mise à jour séquentielle — le cycle predict–update

### Le problème du drone mobile

Un drone ne reste pas immobile. À chaque seconde, il se déplace. Le problème devient donc :

- comment estimer la position à l'instant $k$, sachant ce qu'on savait à l'instant $k-1$ et la nouvelle mesure GPS ?

La réponse est une **boucle récursive** en deux étapes, répétée indéfiniment :

```
┌──────────────────────────────────────────────────┐
│   Répéter à chaque pas de temps k :              │
│                                                  │
│   1. PREDICT  — propager l'état avec le modèle  │
│   2. UPDATE   — corriger avec la mesure GPS      │
└──────────────────────────────────────────────────┘
```

### Étape 1 — Predict (prédiction)

On utilise un **modèle de mouvement** pour anticiper où sera le drone au prochain instant. Si le drone avance de $u$ mètres par seconde :

$$\hat{x}_k^- = \hat{x}_{k-1} + u$$

L'incertitude, elle, **augmente** : le modèle n'est pas parfait (le moteur vibre, le vent pousse...). On ajoute un terme de **bruit de processus** $Q$ :

$$P_k^- = P_{k-1} + Q$$

Le signe $^-$ désigne l'état **prédit** (avant la mesure). L'incertitude grandit à chaque prédiction — c'est inévitable.

### Étape 2 — Update (mise à jour)

On dispose maintenant d'une mesure GPS $z_k$. On calcule d'abord le **gain de Kalman** $K$ :

$$K_k = \frac{P_k^-}{P_k^- + R}$$

où $R$ est la variance du bruit du GPS. Ce gain est un nombre entre 0 et 1 :

| Valeur de $K$ | Interprétation |
|--------------|----------------|
| $K \approx 1$ | On fait confiance au GPS (R petit, capteur précis) |
| $K \approx 0$ | On fait confiance au modèle (R grand, GPS bruité) |

On corrige ensuite l'estimation avec la **différence entre mesure et prédiction** (appelée *innovation*) :

$$\hat{x}_k = \hat{x}_k^- + K_k\,(z_k - \hat{x}_k^-)$$

Et l'incertitude **diminue** grâce à la mesure :

$$P_k = (1 - K_k)\,P_k^-$$

### Le rôle de Q et R

Ces deux paramètres sont les principaux réglages du filtre :

| Paramètre | Signification | Effet si trop grand | Effet si trop petit |
|-----------|--------------|--------------------|--------------------|
| $Q$ | Incertitude du modèle de mouvement | Le filtre suit trop le GPS, ignore le modèle | Le filtre suit trop le modèle, ignore le GPS |
| $R$ | Incertitude de la mesure GPS | Le filtre ignore le GPS | Le filtre croit trop le GPS |

Trouver le bon rapport $Q/R$ est l'une des décisions les plus importantes lors de la conception d'un filtre.

### La cohérence du filtre

Comment savoir si le filtre est bien réglé ? On calcule l'**erreur normalisée** :

$$\varepsilon_k = \frac{x_k^{\text{vrai}} - \hat{x}_k}{\sqrt{P_k}}$$

Si le filtre est cohérent, $\varepsilon_k$ doit se comporter comme un bruit gaussien centré : $\varepsilon_k \sim \mathcal{N}(0, 1)$.

- Si $|\varepsilon_k| > 2$ trop souvent (plus de 5 % du temps) → le filtre est **trop confiant** (P sous-estimé)
- Si $|\varepsilon_k|$ est toujours très proche de 0 → le filtre est **trop pessimiste** (P surestimé)

Ce test de cohérence sera réutilisé dans tous les modules suivants.

---

## Connexion avec le filtre de Kalman

Tout ce qui vient d'être décrit **est** le filtre de Kalman, dans sa version scalaire (une seule dimension).

Le passage au filtre de Kalman complet (Module 01) ne fait qu'étendre ces équations à plusieurs dimensions simultanément, en remplaçant les scalaires par des vecteurs et des matrices.

| Ce module (1D) | Filtre de Kalman (nD) |
|----------------|----------------------|
| $\hat{x}^- = \hat{x} + u$ | $\hat{\mathbf{x}}^- = \mathbf{A}\hat{\mathbf{x}} + \mathbf{B}\mathbf{u}$ |
| $P^- = P + Q$ | $\mathbf{P}^- = \mathbf{A}\mathbf{P}\mathbf{A}^T + \mathbf{Q}$ |
| $K = P^-/(P^-+R)$ | $\mathbf{K} = \mathbf{P}^-\mathbf{H}^T(\mathbf{H}\mathbf{P}^-\mathbf{H}^T + \mathbf{R})^{-1}$ |
| $\hat{x} = \hat{x}^- + K(z - \hat{x}^-)$ | $\hat{\mathbf{x}} = \hat{\mathbf{x}}^- + \mathbf{K}(\mathbf{z} - \mathbf{H}\hat{\mathbf{x}}^-)$ |
| $P = (1-K)P^-$ | $\mathbf{P} = (\mathbf{I} - \mathbf{K}\mathbf{H})\mathbf{P}^-$ |

La structure est identique. Les matrices ne font qu'étendre la logique à plusieurs dimensions.

---

## Résumé des notions clés

| Notion | Définition courte | Où elle apparaît |
|--------|-------------------|-----------------|
| Prior | Croyance avant la mesure | Bayes, predict step |
| Likelihood | Probabilité d'observer la mesure si l'état est connu | Bayes, update step |
| Posterior | Croyance après la mesure | Résultat du filtre |
| Gaussienne $\mathcal{N}(\mu, \sigma^2)$ | Distribution en cloche, paramétrée par moyenne et variance | Représentation de l'état |
| Matrice de covariance $\boldsymbol{\Sigma}$ | Incertitude multidimensionnelle + corrélations | Matrice P du filtre |
| Transformation linéaire | $\mathbf{y} = \mathbf{A}\mathbf{x} + \mathbf{b}$ préserve la gaussianité | Étape predict |
| Innovation | Écart entre mesure et prédiction : $z - \hat{x}^-$ | Étape update |
| Gain de Kalman $K$ | Dosage confiance modèle / confiance capteur | Étape update |
| Bruit de processus $Q$ | Incertitude du modèle de mouvement | Réglage du filtre |
| Bruit de mesure $R$ | Incertitude du capteur | Réglage du filtre |
| Cohérence | $\varepsilon \sim \mathcal{N}(0,1)$ — P est bien calibré | Validation du filtre |
