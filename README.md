<div align="center">

# ECE Manager Pro
### Répartiteur de jury pour les Épreuves Communes de Contrôle — Physique Chimie

[![Lycée Antoine Watteau](https://img.shields.io/badge/Lycée-Antoine%20Watteau-2a6496?style=flat-square)](https://www.lycee-watteau.fr)
[![HTML5](https://img.shields.io/badge/HTML5-Standalone-e34f26?style=flat-square&logo=html5&logoColor=white)](https://developer.mozilla.org/fr/docs/Web/HTML)
[![Licence](https://img.shields.io/badge/Licence-MIT-22c55e?style=flat-square)](LICENSE)
[![Session](https://img.shields.io/badge/Session-2025–2026-818cf8?style=flat-square)](.)

**Application web mono-fichier pour générer automatiquement les plannings d'oral ECE en respectant la contrainte légale d'anonymat.**

[Fonctionnalités](#-fonctionnalités) · [Démo rapide](#-démo-rapide) · [Installation](#-installation) · [Format CSV](#-format-csv) · [Algorithme](#-algorithme) · [Captures](#-captures)

</div>

---

## Contexte

Les **Épreuves Communes de Contrôle (ECE)** en Physique Chimie imposent une règle réglementaire stricte : **un examinateur ne peut pas évaluer un élève qu'il a eu en cours durant l'année**. Dans un lycée avec plusieurs classes de Terminale et plusieurs enseignants, la répartition manuelle respectant cette contrainte est laborieuse et source d'erreurs.

**ECE Manager Pro** automatise cette tâche en quelques secondes : importez votre export PRONOTE, cliquez sur *Générer*, obtenez des fiches de salle prêtes à imprimer.

---

## ✨ Fonctionnalités

### Gestion du jury
- **Import CSV automatique** depuis un export PRONOTE (drag & drop ou clic)
- **Détection automatique** des examinateurs depuis le CSV
- **Ajout d'examinateurs externes** (intervenants n'ayant pas suivi les élèves)
- **Remplacement d'examinateur** : désigner un remplaçant pour un titulaire absent
- **Activation / désactivation** individuelle de chaque examinateur (absences, etc.)

### Paramétrage
| Paramètre | Valeur par défaut | Plage |
|---|---|---|
| Élèves max par examinateur | 4 | 1 – 12 |
| Nombre de sujets par examinateur | 2 | 1 – 4 |
| Noms des sujets | TP A, TP B… | Libres |

### Algorithme d'anonymisation
- **2 000 tentatives** de répartition aléatoire par génération
- Contrainte stricte : aucun élève face à son propre enseignant
- Distribution **équilibrée des sujets** (écart ≤ 1 élève par sujet)
- Regénération illimitée sans recharger le CSV
- Message d'erreur explicite si la capacité est insuffisante

### Interface
- **Mode clair / sombre** (thème persistant en localStorage)
- **Recherche en temps réel** dans la liste des élèves
- **Sélection individuelle ou globale** des élèves à répartir
- Fiches de salle formatées, prêtes à l'impression
- **Export CSV** du planning généré
- Application 100 % hors-ligne, aucun serveur requis

### Animation de génération
- Compte à rebours de 10 secondes avec arc SVG animé
- Personnage 3D animé depuis une spritesheet (8 animations : walk, idle, jump…)
- Système de particules en arrière-plan
- Progression des étapes en temps réel

---

## 🚀 Démo rapide

```
1. Ouvrir ECE_Manager_Pro.html dans un navigateur moderne
2. Glisser le fichier physique.csv dans la zone d'import
3. Vérifier / ajuster les examinateurs dans le panneau gauche
4. Cliquer sur ⚡ Générer le planning
5. Imprimer ou exporter les fiches de salle
```

> **Aucune installation requise.** L'application fonctionne entièrement dans le navigateur, sans dépendance externe ni connexion internet.

---

## 📁 Installation

Cloner ou télécharger le dépôt, puis placer les fichiers dans le même dossier :

```
votre-dossier/
├── ECE_Manager_Pro.html   ← application principale
├── spritesheet.png        ← sprite du personnage (animation)
└── physique.csv           ← votre export PRONOTE (optionnel au démarrage)
```

Ouvrir `ECE_Manager_Pro.html` dans Chrome, Firefox, Edge ou Safari.

---

## 📄 Format CSV

L'application attend un CSV issu de **PRONOTE** ou compatible avec la structure suivante :

```csv
Nom,Prénom,Classe,Professeur
DUPONT,Marie,TG1,M. MARTIN
LEFEBVRE,Lucas,TG1,M. MARTIN
BERNARD,Chloé,TG2,Mme DURAND
...
```

| Colonne | Description |
|---|---|
| `Nom` | Nom de famille de l'élève |
| `Prénom` | Prénom de l'élève |
| `Classe` | Classe (ex. TG1, TG2…) |
| `Professeur` | Nom de l'enseignant titulaire |

> Le séparateur peut être `,` ou `;`. L'encodage UTF-8 est recommandé.

---

## ⚙️ Algorithme

```
ENTRÉE  : liste d'élèves sélectionnés, liste d'examinateurs actifs
SORTIE  : affectation élève → examinateur → sujet

POUR chaque tentative (max 2 000) :
  1. Mélanger aléatoirement la liste des élèves (Fisher-Yates)
  2. POUR chaque élève :
       Chercher les examinateurs éligibles :
         - n'est pas le prof de cet élève  (contrainte légale)
         - n'a pas atteint la capacité max (équilibrage)
       Si aucun candidat → tentative échouée → recommencer
       Sinon → affecter l'examinateur le moins chargé
  3. Distribuer les sujets de façon équilibrée et mélangée
  4. Retourner le planning

SI 2 000 tentatives épuisées → afficher message d'erreur
```

**Complexité** : O(n × e) par tentative, avec n élèves et e examinateurs.

---

## 🖼 Captures

| Vue principale | Planning généré |
|---|---|
| *Panneau de gestion du jury + paramètres* | *Fiches de salle formatées* |

| Animation de génération | Mode sombre |
|---|---|
| *Compte à rebours + personnage animé* | *Thème dark complet* |

---

## 🛠 Stack technique

```
HTML5 / CSS3 / JavaScript ES2020   — aucun framework, aucune dépendance
Canvas API                          — moteur d'animation spritesheet
CSS Custom Properties               — thème clair / sombre
localStorage                        — persistance des examinateurs entre sessions
FileReader API                      — lecture CSV côté client
```

---

## 🔒 Données & confidentialité

- **Aucune donnée ne quitte le navigateur** : tout le traitement est local
- Seule la configuration des examinateurs (sans données élèves) est conservée en `localStorage`
- Les données élèves disparaissent à la fermeture de l'onglet
- Conforme au cadre des épreuves nationales et au RGPD

---

## 📋 Feuille de route

- [ ] Export PDF des fiches de salle (impression directe)
- [ ] Import multi-classes en une seule session
- [ ] Historique des plannings générés
- [ ] Mode multi-matières (SVT, autre discipline)
- [ ] API PRONOTE native (si disponible)

---

## 🤝 Contribution

Les contributions sont les bienvenues. Pour toute suggestion ou signalement de bug :

1. **Fork** le dépôt
2. Créer une branche `feature/ma-fonctionnalite`
3. Soumettre une **Pull Request**

---

## 👨‍🏫 Auteur

**DarkSATHI Li** — Professeur NSI & Physique Chimie  
Lycée Antoine Watteau — Valenciennes (Hauts-de-France)

[![YouTube](https://img.shields.io/badge/YouTube-DarkSATHI%20Li-ff0000?style=flat-square&logo=youtube)](https://youtube.com)
[![GitHub](https://img.shields.io/badge/GitHub-darksathili--jpg-181717?style=flat-square&logo=github)](https://github.com/darksathili-jpg)

---

## 📜 Licence

Distribué sous licence **MIT**. Voir [`LICENSE`](LICENSE) pour plus d'informations.

---

<div align="center">

*Fait avec ❤️ pour simplifier la vie des équipes pédagogiques*  
**Lycée Antoine Watteau — DarkSATHI Li**

</div>