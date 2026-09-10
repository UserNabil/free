# Prompt — « Cockpit Freelance »

> Copie tout ce qui suit le séparateur et colle-le dans une nouvelle conversation Claude (Cowork ou Claude Code).
> Les paramètres chiffrés ont été vérifiés en septembre 2026 (sources en fin de document). Ils sont volontairement
> placés dans un fichier de config éditable : ils changent chaque année et certains sont issus de baromètres, pas de
> textes officiels.

---

## CONTEXTE

Je suis développeur full-stack (.NET Core, Angular, React, Flutter, PostgreSQL, Azure), salarié unique développeur
de mon département, basé en Île-de-France. Je prépare un passage en freelance : je n'ai pas encore choisi de statut
juridique et je ne maîtrise pas encore tous les mécanismes (cotisations, TVA, TJM, prospection). Mon scénario de
sortie envisagé est une rupture conventionnelle.

J'ai besoin d'un outil personnel unique — un « cockpit » — pour :
1. comprendre et comparer les statuts avant de décider,
2. suivre les démarches sans rien oublier,
3. **trouver des missions et piloter la prospection de façon rigoureuse** (c'est la priorité opérationnelle),
4. suivre l'argent une fois lancé.

## CE QUE TU DOIS CONSTRUIRE

Une application web mono-page, publiée comme Artifact Claude, avec **persistance des données** (capability `db`)
et **une intégration Claude** (capability `claude`) qui me permet d'ajouter/compléter des choses depuis la page.
Charge la skill `artifact-capabilities` avant d'écrire la page pour connaître le contrat exact des capabilities,
puis la skill `artifact-design`.

Si les capabilities `db`/`claude` ne sont pas disponibles sur mon compte : construis la même app en HTML autonome
avec persistance `localStorage` + export/import JSON, et dis-le-moi explicitement.

## CONTRAINTE DIRECTRICE — NE PAS SURCHARGER

C'est la contrainte la plus importante. **5 onglets, pas un de plus.** Chaque écran doit tenir sur un écran sans
scroll infini. Règles :

- Aucun champ « au cas où » : si je ne remplis pas un champ toutes les semaines, il ne doit pas exister.
- Pas de graphiques décoratifs. Un graphique n'existe que s'il répond à une question que je me pose vraiment.
- Pas de dashboard d'accueil séparé : l'onglet Prospection **est** l'accueil.
- Pas d'authentification, pas de multi-utilisateur, pas d'onboarding, pas de mode démo.
- Pas de gestion documentaire, pas d'édition de factures PDF, pas de compta en partie double.
- Saisie rapide : tout ajout d'élément se fait en ≤ 4 champs, le reste est optionnel et replié.

## LES 5 ONGLETS

### 1. Prospection (écran par défaut)

L'objectif : savoir chaque matin qui relancer et où j'en suis.

- **Kanban de pistes** en 6 colonnes : `À contacter` → `Candidature envoyée` → `Échange en cours` →
  `Proposition / TJM discuté` → `Gagné` → `Perdu`.
- Une piste = client final ou intermédiaire, source, poste, TJM visé, TJM proposé, date de dernier contact,
  date de prochaine relance, note libre.
- **Bandeau « À faire aujourd'hui »** en haut : pistes dont la relance est due ou en retard, triées par retard.
  Si une piste n'a pas bougé depuis X jours (X paramétrable, défaut 7), elle est marquée « dormante ».
- **Compteurs de performance** discrets, sur une ligne : candidatures envoyées ce mois, taux de réponse,
  taux de passage entretien → proposition, TJM moyen proposé. Ce sont mes vrais indicateurs de performance,
  pas de la décoration.
- **Registre des canaux** : liste éditable des sources de missions avec pour chacune le nombre de pistes
  générées et le nombre gagnées, pour que je sache où investir mon temps. Pré-remplis-la avec les canaux
  usuels du marché français en 2026, catégorisés :
  - *Plateformes* : Malt, Free-Work, Comet, LeHibou, Talent.io, Crème de la Crème, StaffMe Pro, Codeur.
  - *Intermédiaires / ESN et cabinets de placement* (le canal dominant en régie IDF sur du .NET/Angular).
  - *Réseau direct* : anciens collègues, clients de mon employeur actuel (attention clause de non-concurrence
    et de non-sollicitation — à vérifier dans mon contrat, mets-le en checklist), LinkedIn.
  - *Communautés* : groupes Slack/Discord tech FR, meetups, forums.
  Ne présente pas ces canaux comme des recommandations : ce sont des entrées à valider et à mesurer par mes
  propres chiffres.

### 2. Simulateur TJM → net

Un seul écran, deux zones : mes hypothèses à gauche, la comparaison des 4 statuts à droite.

**Entrées :** TJM HT, jours facturés par an (défaut 200 — mets une aide indiquant que 220 jours est un plafond
théorique qui ignore l'intercontrat, les congés et l'administratif), frais professionnels annuels,
salaire net mensuel actuel (référence de comparaison).

**Sorties — un tableau comparatif des 4 statuts**, chacun avec : CA HT annuel, prélèvements sociaux,
impôt estimé, **revenu net annuel et mensuel**, et l'écart en % avec ma situation salariée actuelle.
Ajoute pour chaque statut 2 lignes qualitatives : *protection chômage* et *complexité administrative*.

**Paramètres de calcul 2026** (à externaliser dans un objet `PARAMS_2026` éditable en haut du code, avec un
commentaire `// vérifié 09/2026 — à réviser chaque janvier`) :

| Statut | Paramètres |
|---|---|
| **Micro-entreprise (BNC non réglementé)** | Plafond CA : 83 600 €. Cotisations sociales : 25,6 % du CA. CFP : 0,2 %. Abattement fiscal forfaitaire : 34 %. Versement libératoire optionnel : 2,2 % du CA (sous conditions de RFR). CFE exonérée l'année de création. Franchise TVA : seuil de base 37 500 €, seuil majoré 41 250 €. ACRE : exonération de 25 % (taux applicable depuis le 01/07/2026), à demander sous 60 jours. |
| **EURL à l'IS (gérant TNS)** | Cotisations ≈ 45 % de la rémunération nette. Cotisations minimales ≈ 1 135 €/an même sans rémunération. IS : 15 % jusqu'à 42 500 € de bénéfice, 25 % au-delà. Dividendes : PFU 31,4 %, **et** cotisations TNS sur la part dépassant 10 % du capital social. |
| **SASU à l'IS (président assimilé salarié)** | Cotisations ≈ 80 % de la rémunération nette (≈ 1 800 € de coût pour 1 000 € net). Zéro cotisation si aucune rémunération versée. IS : même barème. Dividendes : PFU 31,4 %, sans cotisations sociales. Pas d'assurance chômage. |
| **Portage salarial** | Frais de gestion : 5 à 10 % du CA HT (paramétrable, défaut 7 %). Taux de restitution net observé : 47 à 52 % du CA HT (salaire net + frais professionnels remboursés). Statut salarié : cotise à l'assurance chômage et à la retraite cadre. |

Le PFU 2026 est de 31,4 % (12,8 % IR + 18,6 % prélèvements sociaux) — ne code pas 30 %.

**Impératif d'honnêteté du simulateur** : affiche sous le tableau une ligne de réserve indiquant que ce sont des
ordres de grandeur, que l'impôt sur le revenu dépend de mon foyer fiscal (je suis marié, donc quotient familial
et revenus du conjoint entrent en jeu) et qu'une simulation d'expert-comptable est nécessaire avant décision.
Prévois un champ optionnel « revenus du foyer » pour affiner l'IR si tu implémentes le barème progressif ;
sinon, indique clairement que l'IR est calculé sur mes seuls revenus.

**Repères marché (bloc séparé, en lecture seule, sourcé et daté)** — baromètre Free-Work, données à septembre 2026,
développeur fullstack : TJM moyen 410 € en direct / 430 € via intermédiaire ; Île-de-France par ancienneté :
3–4 ans 401 €, 5–10 ans 536 €, 11–15 ans 577 €. Précise que ce sont des données déclaratives d'un baromètre,
pas une garantie de marché.

### 3. Démarches

Checklist chronologique en 4 phases repliables, avec case à cocher, échéance et note. Je dois pouvoir ajouter
mes propres lignes. Contenu de départ :

- **Phase 1 — Avant d'annoncer quoi que ce soit** : relire mon contrat de travail (clause de non-concurrence,
  de non-sollicitation, d'exclusivité) ; vérifier mes droits ARE (simulation France Travail) ; constituer une
  trésorerie de sécurité ; tester le marché avec 5 candidatures réelles pour valider mon TJM ; décider du statut.
- **Phase 2 — Rupture conventionnelle** : préparer l'argumentaire ; entretien(s) obligatoire(s) ; signature de la
  convention ; délai de rétractation de 15 jours calendaires ; homologation DREETS (15 jours ouvrables) ;
  inscription France Travail ; solde de tout compte et attestation employeur.
  ⚠️ Marque explicitement que la rupture conventionnelle **ne peut pas être imposée** : elle nécessite l'accord
  de l'employeur, et un refus est un scénario à préparer (démission = pas d'ARE, sauf démission-reconversion).
- **Phase 3 — Création** : immatriculation via le guichet unique INPI ; obtention SIRET ; compte bancaire dédié ;
  **RC Pro** (quasi systématiquement exigée par les clients et les plateformes) ; prévoyance et mutuelle ;
  choix de l'outil de facturation ; décision sur l'option TVA ; ACRE si éligible (60 jours) ; expert-comptable
  si société.
- **Phase 4 — Lancement** : profil LinkedIn repositionné (titre, bannière, section « à propos » orientée offre) ;
  profils plateformes ; portfolio ou site vitrine ; CV freelance et « one-pager » d'offre ; modèles de contrat de
  prestation et de devis ; grille tarifaire écrite (TJM, majoration urgence, conditions de règlement).

Chaque item porte un lien officiel quand il en existe un (INPI, URSSAF, service-public.fr, France Travail).
**N'invente aucune URL** : si tu n'es pas certain d'un lien, mets le nom de l'organisme sans lien.

### 4. Missions & facturation

- Liste des missions : client, TJM négocié, date de début, date de fin prévue, jours vendus, jours consommés,
  statut. Une barre d'avancement jours consommés / jours vendus.
- Saisie des jours travaillés par mois et par mission (grille simple, pas de timesheet à la demi-journée).
- Factures : numéro, mission, montant HT, TVA, date d'émission, date d'échéance, date d'encaissement.
  **Alerte sur les factures échues non payées** — c'est le vrai risque du freelance.
- Trois indicateurs en haut : CA facturé sur l'année en cours, CA encaissé, en attente d'encaissement.
- **Jauge de plafonds** si le statut sélectionné est micro-entreprise : progression du CA vers 37 500 € (TVA)
  et vers 83 600 € (sortie du régime), avec projection de la date de dépassement au rythme actuel.

### 5. Échéances & trésorerie

- Calendrier des échéances récurrentes selon le statut choisi : déclaration URSSAF (mensuelle ou trimestrielle,
  au choix), TVA si assujetti, acomptes d'IS et liasse si société, CFE (décembre), déclaration de revenus (mai/juin).
  Génère les occurrences sur 12 mois glissants.
- **Provision automatique** : à chaque encaissement, calcule et affiche ce que je dois mettre de côté
  (cotisations + impôt estimé selon le statut actif), et le solde réellement disponible. C'est le point qui fait
  échouer la plupart des débuts en freelance.
- Vue trésorerie sur 6 mois : encaissements prévus (factures émises + missions en cours) moins échéances connues
  moins charges fixes mensuelles saisies.

## INTÉGRATION CLAUDE

Un bouton « Demander à Claude » accessible depuis chaque onglet, qui ouvre un panneau latéral et transmet le
contexte de l'onglet courant. Cas d'usage à supporter explicitement :

- « Ajoute ces 5 pistes que je viens de trouver » → Claude crée les entrées structurées dans le kanban.
- « Rédige un message d'approche pour cette piste » → utilise les données de la piste.
- « Explique-moi cette échéance » / « qu'est-ce que la CFE ? » → réponse courte et factuelle.
- « Que dois-je faire cette semaine ? » → synthèse des relances dues, échéances proches et factures en retard.

Toute donnée écrite par Claude passe par le même modèle que la saisie manuelle et reste éditable.
Le panneau doit indiquer clairement quand une réponse est une estimation et non une règle vérifiée.

## MODÈLE DE DONNÉES

Utilise ces collections, en JSON, avec un `id` et un `updatedAt` sur chaque enregistrement :
`leads`, `channels`, `checklist`, `missions`, `invoices`, `workdays`, `deadlines`, `fixedCosts`, `settings`.

`settings` contient : statut actif (ou `undecided`), TJM cible, jours facturés/an, salaire de référence,
frais de gestion portage, délai de relance par défaut, et une copie de `PARAMS_2026` que je peux modifier
depuis l'interface (écran de réglages accessible par une icône, **pas** un 6ᵉ onglet).

## DESIGN

Interface dense mais calme, orientée outil de travail quotidien, pas plaquette marketing. Thème clair et sombre.
Utilisable sur mobile (je consulterai mes relances depuis mon téléphone). Interface **en français**, avec le
vocabulaire exact des organismes (URSSAF, CFE, TJM, ARE, RC Pro) — pas de traduction approximative.

## EXIGENCES DE RIGUEUR

- Aucun chiffre en dur ailleurs que dans `PARAMS_2026`, avec la date de vérification en commentaire.
- Le simulateur affiche ses hypothèses, pas seulement ses résultats : je dois pouvoir vérifier chaque calcul.
- Ne présente jamais une estimation comme un montant certain. Ne recommande pas un statut : présente les écarts
  et laisse la décision ouverte.
- Aucune URL inventée.

## LIVRAISON

1. Construis l'application.
2. Publie-la comme Artifact et donne-moi le lien.
3. Liste en fin de réponse : (a) ce que tu as implémenté, (b) ce que tu as volontairement laissé de côté pour
   respecter la contrainte anti-surcharge, (c) les chiffres de `PARAMS_2026` dont la fiabilité te paraît la plus
   faible et qu'il faut que je fasse confirmer par un expert-comptable.

---

## Variante — si tu préfères le construire toi-même

Le même prompt fonctionne pour Claude Code sur un projet .NET 8 + Angular + PostgreSQL : remplace la section
« CE QUE TU DOIS CONSTRUIRE » par ta stack, remplace les collections JSON par des entités EF Core (mêmes noms),
et remplace l'intégration Claude par un appel serveur à l'API Anthropic derrière un endpoint `/api/assistant`.
Le reste — contrainte anti-surcharge, paramètres 2026, exigences de rigueur — reste identique.

---

## Sources des paramètres 2026

- Plafonds micro-entreprise (203 100 € / 83 600 €, revalorisation triennale, art. 4 LF 2026) et taux de cotisations
  BNC 25,6 % / CIPAV 23,2 %, ACRE à 25 % au 01/07/2026 — [Compta Online](https://www.compta-online.com/regime-social-des-micro-entrepreneurs-ao8080), [Astrolabe Conseil](https://www.astrolabe-conseil.fr/micro-entreprise-taux-2026/)
- Franchise en base de TVA 2026 : 37 500 € / 41 250 € pour les services, seuil de 25 000 € finalement réservé au
  bâtiment — [Le Coin des Entrepreneurs](https://www.lecoindesentrepreneurs.fr/franchise-en-base-de-tva-nouvelles-regles-2026/)
- Charges SASU (~80 % du net) / EURL (~45 % du net), IS 15 % puis 25 % au-delà de 42 500 € — [LeFreelance](https://lefreelance.fr/articles/sasu-vs-eurl-freelance/)
- PFU 2026 à 31,4 % — [MeilleureSCPI](https://www.meilleurescpi.com/conseils/flat-tax-2026-le-guide-ultime-du-prelevement-forfaitaire-unique-pfu/), [S'investir](https://sinvestir.fr/pfu-changement/)
- Portage salarial : frais de gestion 5–10 %, taux de restitution 47–52 % — [Le Monde Informatique](https://www.lemondeinformatique.fr/publi_info/lire-quel-est-le-cout-moyen-du-portage-salarial-et-ou-obtenir-les-meilleures-offres-le-comparatif-2026-1343.html)
- TJM développeur fullstack, baromètre déclaratif à septembre 2026 — [Free-Work](https://www.free-work.com/fr/tech-it/developpeur-fullstack/rate-tjm-freelance)

Réserve : ces sources sont des cabinets et des éditeurs, pas des textes officiels. Les taux de cotisations et les
seuils sont cohérents entre plusieurs sources indépendantes ; les ratios SASU/EURL et le taux de restitution en
portage sont des ordres de grandeur commerciaux, à faire confirmer.