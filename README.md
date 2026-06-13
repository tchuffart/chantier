# Chantier — Plugin TiddlyWiki de gestion de réunions de chantier

Plugin [TiddlyWiki 5](https://tiddlywiki.com/) destiné aux architectes et maîtres d'œuvre pour gérer les réunions de chantier et produire des comptes-rendus imprimables.

## Fonctionnalités

### Tableau de bord

Le tableau de bord central regroupe toutes les informations du chantier en onglets :

- **Vue d'ensemble** — liste des points ouverts (reportés des réunions précédentes et nouveaux), avec accès rapide à l'ajout de points
- **Intervenants** — annuaire des entreprises et contacts, classés par catégorie (Maîtrise d'ouvrage, Maîtrise d'œuvre, Entreprises)
- **Lots** — liste des lots de travaux avec leurs intervenants associés
- **Réunions** — historique des réunions de chantier
- **Références** — documents, normes, plans et autres références permanentes du projet
- **Fiche de rappel** — synthèse imprimable des points en cours
- **Paramètres** — configuration du projet (nom, adresse, maître d'ouvrage, logos)

Un en-tête de projet s'affiche en haut du tableau de bord avec le nom du chantier, son adresse et les informations du maître d'ouvrage.

### Réunions de chantier

Chaque réunion comporte :

- **Points** : décisions, actions et observations numérotés automatiquement, avec statut (ouvert / pour rappel / résolu), lot associé, intervenant responsable et délai
- **Présences** : suivi par intervenant (présent, excusé, absent, convoqué)
- **Références spécifiques** à la réunion

Les points des réunions précédentes encore ouverts sont automatiquement reportés dans les réunions suivantes.

### Compte-rendu imprimable

Un bouton « Imprimer CR » ouvre une prévisualisation au format A4 comprenant :

- En-tête avec logos (architecte et MOA), nom du chantier, numéro et date de réunion, lieu
- Tableau des intervenants groupés par catégorie, avec statut de présence
- Liste des références permanentes du projet
- Points reportés des réunions précédentes
- Points nouveaux de la réunion
- Points résolus

La mise en page est optimisée pour l'impression (police 9pt, tableaux 8pt, marges 15 mm).

## Installation

### Prérequis

- [Node.js](https://nodejs.org/) ≥ 14
- TiddlyWiki 5 (`npm install -g tiddlywiki`)

### Lancer la démo

```bash
npm install
npm run demo
```

Ouvre le wiki sur `http://localhost:8080` avec des données d'exemple (chantier de médiathèque avec intervenants, lots, réunions et points).

### Intégrer le plugin dans un wiki existant

Copier le dossier `plugins/UncleTom/chantier/` dans le dossier `plugins/` de votre édition TiddlyWiki, puis ajouter dans `tiddlywiki.info` :

```json
{
  "plugins": [
    "UncleTom/chantier"
  ]
}
```

## Structure du projet

```
plugins/
  UncleTom/
    chantier/          Plugin principal
    chantier-fr-FR/    Traductions françaises
editions/
  demo/                Wiki de démonstration avec données d'exemple
```

## Licence

MIT — © UncleTom
