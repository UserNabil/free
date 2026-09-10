# Cockpit Freelance

Outil personnel unique pour préparer et piloter un passage en freelance (développeur full-stack, Île-de-France).

Application web mono-page, sans dépendance, publiée comme Artifact Claude.
Le fichier `cockpit-freelance.html` est autonome : il contient tout le code, le style et les paramètres.

## Les 5 onglets

| Onglet | Ce qu'il sert à faire |
|---|---|
| **Prospection** (accueil) | Kanban de pistes en 6 colonnes, bandeau « à faire aujourd'hui », 4 compteurs de performance, registre des canaux |
| **Simulateur TJM** | Comparaison micro-entreprise / EURL / SASU / portage : net annuel, net mensuel, écart avec le salaire actuel |
| **Démarches** | Checklist chronologique en 4 phases, de la relecture du contrat de travail au lancement |
| **Missions & facturation** | Missions, jours consommés, factures, alerte sur les factures échues, jauges de plafonds micro |
| **Échéances & trésorerie** | Échéances sur 12 mois glissants, provision automatique par encaissement, trésorerie à 6 mois |

Les réglages (statut actif, hypothèses, paramètres de calcul) sont derrière l'icône ⚙, pas dans un 6ᵉ onglet.

## Persistance

- Publié comme Artifact avec la capability `db` : les données sont stockées côté serveur et suivent les rechargements.
- Ouvert hors de ce contexte, l'application bascule d'elle-même sur `localStorage`, avec export/import JSON depuis les réglages.

Collections : `leads`, `channels`, `checklist`, `missions`, `invoices`, `workdays`, `deadlines`, `fixedCosts`, `settings`.
Chaque enregistrement porte un `id` et un `updatedAt`.

## Intégration Claude

Bouton « Demander à Claude » (capability `sample`) accessible depuis chaque onglet, qui transmet le contexte de
l'onglet courant : extraction de pistes depuis un texte collé, message d'approche, explication d'une échéance,
synthèse de la semaine. Toute donnée écrite par Claude est proposée avant insertion et reste modifiable.

## Paramètres de calcul

Tous les chiffres de calcul sont regroupés dans l'objet `PARAMS_2026`, en tête de script, avec la mention
`vérifié 09/2026 — à réviser chaque janvier` et une annotation de fiabilité par valeur. Ils sont modifiables
depuis l'écran de réglages sans toucher au code.

**Le simulateur donne des ordres de grandeur, pas des montants certains.** L'impôt sur le revenu dépend du foyer
fiscal. Une simulation d'expert-comptable reste nécessaire avant toute décision de statut.

Valeurs dont la fiabilité est la plus faible, à faire confirmer en priorité :

1. le barème de l'impôt sur le revenu (indexé chaque année, non confirmé pour 2026) ;
2. le taux de cotisations TNS appliqué aux dividendes d'EURL au-delà de 10 % du capital ;
3. les ratios de cotisations SASU (~80 % du net) et EURL (~45 % du net), ordres de grandeur commerciaux ;
4. le taux de restitution en portage salarial (47–52 %), observé et non réglementaire.

Le cahier des charges d'origine est conservé dans `besoin.md`.
