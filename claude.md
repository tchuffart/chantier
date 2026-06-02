# Chantier — Plugin TiddlyWiki de gestion de comptes-rendus de chantier

## Contexte
Plugin TiddlyWiki indépendant pour gérer les comptes-rendus de réunions de chantier.
Un fichier HTML par chantier/opération.
Inspiré de l'architecture et de l'esthétique du plugin Projectify (tchuffart/Projectify).

## Dépôt
- GitHub : `tchuffart/chantier`
- Branche principale : `main`

## Conventions
- Préfixe des classes CSS : `ch-`
- Préfixe des champs TiddlyWiki : `ch-`
- Préfixe des titres de tiddlers système : `$:/plugins/UncleTom/chantier/`
- Auteur du plugin : `UncleTom`
- Macros lingo : `\define lingo-base() $:/language/chantier/`
- Langues : `fr-FR` (principale) et `en-GB`

## Architecture des tiddlers

### Types de tiddlers (tags)
| Tag | Description | Champs clés |
|---|---|---|
| `intervenant` | Participant au chantier | `ch-entreprise`, `ch-lot`, `ch-role`, `ch-email`, `ch-tel` |
| `lot` | Corps d'état (gros-oeuvre, VRD...) | `ch-entreprise` |
| `reunion` | Réunion de chantier | `ch-numero`, `ch-date`, `ch-lieu` |
| `presence` | Présence d'un intervenant à une réunion | `ch-reunion`, `ch-intervenant`, `ch-statut` (present/excuse/absent), `ch-convoque` |
| `point` | Point / minute du CR | `ch-numero` (ex: 12.2), `ch-reunion-creation`, `ch-reunion-resolution`, `ch-lots`, `ch-responsables`, `ch-echeance`, `ch-statut` |
| `reference` | Référence permanente | `ch-categorie` (reglementation/materiau/securite/altimetrie/acces/choix-produit), `ch-statut` = reference |

### Statuts d'un point
- `ouvert` — affiché dans tous les CR suivants
- `pour-rappel` — affiché en surbrillance (retard signalé)
- `reference` — affiché en permanence, jamais clôturé, groupé par catégorie
- `resolu` — affiché dans le CR de résolution, archivé ensuite

### Numérotation des points
Format `12.2` = réunion 12, point 2. Le numéro est immuable pour la traçabilité.

## Structure du CR imprimable
1. En-tête : logo architecte (gauche) + logo maître d'ouvrage (droite) + nom chantier / n° réunion / date / lieu
2. Tableau des intervenants : Nom | Entreprise | Lot | Rôle | Présent | Excusé | Convoqué prochaine réunion
3. Références permanentes (groupées par catégorie)
4. Points reportés des réunions précédentes (ouverts + pour rappel)
5. Nouveaux points créés cette réunion
6. Points résolus cette séance

## Tableau des présences
Vue séparée : intervenants × réunions, statuts P/E/A par case.

## Structure du plugin

```
plugins/
└── UncleTom/
    ├── chantier/
    │   ├── plugin.info
    │   └── tiddlers/
    │       ├── language/en-GB/index.multids
    │       ├── macros/filters.tid
    │       ├── styles/stylesheet.tid
    │       └── ui/
    │           ├── buttons/
    │           │   └── IntervenantActions.tid  ✅
    │           ├── forms/
    │           │   └── AddIntervenant.tid       ✅
    │           ├── intervenant/
    │           │   ├── Intervenant.tid          ✅
    │           │   ├── IntervenantItem.tid      ✅
    │           │   └── Intervenants.tid         ✅
    │           ├── lot/                         🔲 étape 3
    │           ├── reunion/                     🔲 étape 4
    │           ├── point/                       🔲 étape 5
    │           ├── reference/                   🔲 étape 6
    │           ├── dashboard/                   🔲 étape 7
    │           └── print/                       🔲 étape 8
    └── chantier-fr-FR/
        ├── plugin.info
        └── language/index.multids
```

## Plan de développement
- [x] Étape 1 — Squelette du plugin, fichiers de langue FR/EN, macros filtres, styles
- [x] Étape 2 — Interface intervenants (liste, formulaire, vue, actions)
- [ ] Étape 3 — Gestion des lots
- [ ] Étape 4 — Création et navigation entre réunions
- [ ] Étape 5 — Saisie des présences
- [ ] Étape 6 — Création et suivi des points
- [ ] Étape 7 — Références permanentes (avec catégories)
- [ ] Étape 8 — Dashboard
- [ ] Étape 9 — Vue CR imprimable avec en-tête logos

## Éditions
- `editions/demo/` — build de démonstration (`npm run build-demo`)

## Notes techniques
- Internationalisation via macro `lingo` dès la conception
- Styles CSS dédiés à l'impression via `@media print`
- La session cloud ne peut pas pousser directement sur GitHub (403)
  → Workflow : commit cloud → `git pull` + `git push` depuis le PC avec `$env:GH_TOKEN`
