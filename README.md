# Cockpit Freelance

Outil personnel unique pour préparer et piloter un passage en freelance (développeur full-stack, Île-de-France).

Application web mono-page, sans dépendance, publiée comme Artifact Claude.
Le fichier `cockpit-freelance.html` est autonome : il contient tout le code, le style et les paramètres.

## Les 8 onglets

| Onglet | Ce qu'il sert à faire |
|---|---|
| **Aujourd'hui** (accueil) | Le copilote : rang, niveau, série, quêtes du jour, ce qui vous attend, ce qui est en attente d'un tiers, avancement des 4 phases |
| **Aventure** | Chemin de 21 jalons en 4 chapitres, avec 4 carrefours de décision qui écrivent un vrai réglage |
| **Prospection** | Kanban de pistes en 6 colonnes, bandeau « à faire aujourd'hui », 4 compteurs de performance, registre des canaux |
| **Simulateur TJM** | Comparaison micro-entreprise / EURL / SASU / portage, **projection pluriannuelle** ARCE / maintien ARE / sans aide, fiches des aides mobilisables |
| **Démarches** | Checklist en 4 phases à **trois états** (à faire / en attente d'un retour / fait), avec compte à rebours sur les délais |
| **Missions & facturation** | Missions, jours consommés, factures, alerte sur les factures échues, jauges de plafonds micro |
| **Échéances & trésorerie** | Échéances sur 12 mois glissants, provision automatique par encaissement, trésorerie à 6 mois |
| **Vitrine** | Projets personnels, CV et portfolio composés par glisser-déposer, exportables en pages HTML autonomes |

Les réglages (statut actif, hypothèses, droits ARE, paramètres de calcul) sont derrière l'icône ⚙.

## Progression

L'XP est adossée à des actions réelles, jamais à du temps passé dans l'application : piste créée, candidature
envoyée, piste qui avance d'une colonne, démarche accomplie, facture encaissée. Les **quêtes du jour** sont
régénérées chaque matin à partir de la situation réelle (relances dues, factures échues, démarches restantes).
Le **rang** ne dépend pas de l'XP mais de l'avancement objectif du dossier : *Salarié en veille* → *En préparation*
→ *Décidé* → *En négociation* → *Immatriculé* → *Freelance lancé* → *Freelance confirmé*.

Une démarche qui dépend d'un tiers (homologation DREETS, délai de rétractation, immatriculation, réponse ARCE)
se met **en attente** : elle sort de la liste des choses à faire et déclenche un compte à rebours, signalé en
rouge quand le délai habituel est dépassé.

L'onglet **Aventure** trace le parcours comme un chemin de 21 jalons en 4 chapitres (1 510 XP au total). Aucun
jalon ne se débloque en cliquant : chacun est adossé à un fait réel de l'application (une démarche cochée, un
SIRET obtenu, une facture encaissée). Quatre d'entre eux sont des **carrefours de décision** — statut juridique,
ARCE ou maintien de l'ARE, franchise de TVA ou assujettissement, canal de prospection principal — et le choix
retenu écrit réellement le réglage correspondant. Le jeu et l'outil sont la même chose.

## Ce qui n'est pas possible

Relier les comptes **LinkedIn, Malt, Free-Work ou Indeed** pour postuler ou synchroniser les missions n'est pas
réalisable : un Artifact publié ne peut émettre aucune requête réseau sortante, Malt et Free-Work n'exposent pas
d'API publique, l'API Indeed est fermée à cet usage, et la candidature par API LinkedIn est réservée à des
partenaires accrédités. La seule passerelle réelle est le **courrier électronique**, puisque ces plateformes
notifient toutes par e-mail.

## Projection pluriannuelle et aides

Le simulateur compare trois scénarios sur l'horizon choisi : **ARCE** (capital, 60 % du reliquat des droits),
**maintien de l'ARE** (allocation mensuelle réduite de 70 % du revenu d'activité déclaré) et **aucune aide**,
avec l'ACRE appliquée la première année. Le choix ARCE / maintien étant définitif, l'écart cumulé est affiché
explicitement. L'arbitrage bascule autour de 40 à 50 jours facturés la première année : au-delà, l'ARCE l'emporte.

Renseignez votre **ARE journalière** et vos **jours de droits** relevés sur France Travail : sans eux, l'application
estime l'ARE à partir du salaire brut, ce qui est nettement moins fiable.

## Simulateur ARE intégré

L'onglet Simulateur reproduit la mécanique de France Travail : salaire journalier de référence, ARE brute et nette,
durée d'indemnisation, puis les trois différés qui décalent le premier paiement (congés payés, part supra-légale de
l'indemnité de rupture, délai d'attente de 7 jours). Le bouton « Reporter dans mes droits » alimente la projection
pluriannuelle avec le résultat.

Il calcule aussi le **plancher légal de l'indemnité de rupture conventionnelle** à partir de l'ancienneté, et met en
évidence l'arbitrage caché de la négociation : chaque euro au-dessus de ce plancher repousse l'ARE d'un jour par
tranche de 95,80 €, jusqu'à un plafond de 150 jours. Passé ce plafond, l'indemnité supplémentaire ne coûte plus rien
en différé.

Ce simulateur ne remplace pas la simulation officielle : les paramètres de l'ARE sont revalorisés chaque année et le
salaire journalier de référence y est approché par le brut ÷ 365.

## Vitrine

- **Projets** : la liste ordonnée par glisser-déposer alimente le CV et le portfolio.
- **CV** : blocs typés (expérience, projet, formation, compétences, texte libre) réordonnables, aperçu en direct,
  export en page HTML imprimable en PDF depuis le navigateur.
- **Portfolio** : sections composables et masquables, aperçu en direct, export en **page HTML autonome** sans
  dépendance, déposable chez n'importe quel hébergeur.

Ce n'est pas un éditeur visuel libre : on compose à partir de blocs typés et on les ordonne, ce qui suffit pour un CV
et un portfolio de développeur et garantit un rendu cohérent.

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

1. **les paramètres de l'ARE** (partie fixe, plancher, durée maximale, coefficient de minoration) : revalorisés
   chaque juillet et dépendants de la conjoncture. Saisissez votre relevé France Travail réel plutôt que
   l'estimation ;
2. **le traitement fiscal de l'ARCE** : modélisé comme non imposable, à confirmer par un expert-comptable ;
3. le barème de l'impôt sur le revenu (indexé chaque année, non confirmé pour 2026) ;
4. le taux de cotisations TNS appliqué aux dividendes d'EURL au-delà de 10 % du capital ;
5. les ratios de cotisations SASU (~80 % du net) et EURL (~45 % du net), ordres de grandeur commerciaux ;
6. le taux de restitution en portage salarial (47–52 %), observé et non réglementaire.

L'ACRE est appliquée aux sociétés (EURL, SASU) par la même exonération que la micro-entreprise : c'est une
approximation, les mécanismes réels diffèrent selon le statut.

Le cahier des charges d'origine est conservé dans `besoin.md`.
