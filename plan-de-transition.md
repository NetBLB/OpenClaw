# Plan de transition (phase 6)

Dernière mise à jour : 2026-11-05
Statut : version 1.2 (étapes 1, 2, 3 et 7 ajustées d'après l'analyse de conformité et D-018; T-7 ajouté); v1.1 validée par Dominic le 2026-11-05 (D-017) avec deux ajouts : création de projets dès le jour 1 (T-6) et boucle d'évaluation des sous-agents

Objet : passer de l'environnement actuel à l'architecture validée (`architecture-cible.md` v1.4, D-015) sans casser les workflows existants, une étape à la fois, avec un critère de sortie à chaque étape. Le détail technique est dans `cahier-de-construction.md`.

Décisions de détail prises le 2026-11-05 (D-016) : dépôt du CEO = `NetBLB/ceo`; canal mobile = Telegram. Restent ouverts : l'hébergeur (critères et préférence dans le cahier) et la première équipe à activer (choisie au bilan du premier mois).

## Étapes

| # | Étape | Qui | Contenu | Critère de sortie | Durée visée |
|---|---|---|---|---|---|
| 0 | Préparation | Dominic | Créer le dépôt privé `NetBLB/ceo` (vide); créer un bot Telegram (BotFather) et noter le jeton; créer une clé API Anthropic avec plafond mensuel; créer un mot de passe d'application Gmail (lecture IMAP); choisir l'hébergeur et ouvrir un serveur virtuel; décider si un réseau privé (Tailscale) est utilisé pour l'interface web | Les cinq éléments existent; secrets conservés hors de tout dépôt | 1 à 2 h |
| 1 | Installation du serveur | IA technique (Claude Code) avec le cahier v2.0 | Serveur durci; OpenClaw (Node 26) installé comme service, canal stable étendu, mise à jour automatique désactivée; interface par Tailscale ou SSH seulement; Telegram en liste blanche; modèle intermédiaire par clé API avec plafond; workspace = clone de `ceo`; restrictions d'outils en place; Dreaming et jobs par défaut traités; embeddings locaux mesurés (ou mots-clés) | `openclaw gateway status` sain; Telegram répond à Dominic et ignore un autre compte; `openclaw automations list --all` = les quatre réveils; `security audit` et `doctor` sans avertissement | 1 séance |
| 2 | Construction de la mémoire du CEO | IA technique, à partir des sources de l'audit | Fichiers de bootstrap (AGENTS.md, SOUL.md, USER.md au format officiel, IDENTITY.md); `organisation.md`; `reglages.md`; `commun/` (sept noyaux); `projets/` : import des 20 fiches ART-02 (gabarit enrichi : propriétaire, bloqué par, prochaines actions) et de la passation du commerce (GH-04); `departements/` : un dossier par équipe avec fiche d'état, archive des instructions, `evaluations.md`; `departements/validateur/brief.md`; dossiers clients vides; `memory.search.extraPaths` sur ces dossiers; les quatre réveils créés et testés par `run --force`; journal des décisions amorcé | Relecture par Dominic des sept fichiers `commun/` (ce sont ses connaissances, pas celles de l'IA); divergences de l'audit tranchées (abonnés, devise, durées, catégories refusées, nom du niveau 5 $) | 1 à 2 séances |
| 3 | Test d'acceptation | Dominic | Voir section « Tests d'acceptation » ci-dessous; rien d'autre n'est construit tant qu'ils ne passent pas | Les cinq tests passent | 1 semaine |
| 4 | Usage sans automatisation | Dominic + CEO | Tout ce qui est en développement se travaille avec le CEO (commerce autonome en premier); production inchangée; Black Friday terminée dans ChatGPT avec « Où en sommes-nous » le lundi; frictions notées par le CEO dans `journal/frictions.md`; coûts relevés chaque semaine | Quatre semaines d'usage réel; coût mensuel mesuré; liste de frictions | Semaines 2 à 5 |
| 5 | Bilan du premier mois | Dominic + CEO (rapport préparé par le CEO) | Coût réel contre plafond; frictions; projets suivis contre perdus; décision : garder un interlocuteur unique ou ouvrir un agent secondaire (P-5); choix de la première équipe; ajustement des seuils de dormance et de l'heure des crons | Décisions consignées dans `journal/decisions.md` du CEO et dans `decisions.md` de l'audit | Début décembre |
| 6 | Activation de la première équipe | IA technique + Dominic | Réécriture des instructions actuelles en `departements/<nom>/AGENTS.md` v1.0, `regles.md`, `criteres-acceptation.md`; validateur configuré; test comparatif sur un livrable réel contre l'outil actuel; **boucle d'évaluation** : verdict de Dominic consigné dans `evaluations.md`, modification du brief proposée et validée, nouvelle version, même intrant rejoué, jusqu'à « convenable »; bascule seulement si Dominic juge le résultat au moins égal (P-24) | Un livrable réel produit par l'équipe et accepté; journal d'évaluations tenu | 2 à 3 semaines |
| 7 | Équipes suivantes, une à la fois | idem | Même méthode; ordre décidé par Dominic à chaque bilan mensuel. Jalons techniques attachés à certaines équipes : Gmail (Services; voie à choisir, Q-D : skill de source connue en lecture, ou plugin IMAP officiel avec agent lecteur isolé); forme de l'équipe (sous-agents à la demande, sessions séparées, ou agent distinct par le préréglage équipe, en tenant compte de la réserve officielle sur le multi-agents); appairage du PC Windows comme node (Production vidéo, seulement si un besoin réel apparaît); migration des revues ChatGPT en réveils OpenClaw (Veille : automatisation de travail, après usage) | Une équipe par mois au plus | 2027 |
| 8 | Réévaluation de Paperclip | Dominic + CEO | Quand les trois conditions de D-014 sont réunies (deux équipes bonnes, besoin de tickets et budgets, Paperclip stabilisé) : essai sur une instance séparée, import de `organisation.md`, décision | Décision consignée | Au plus tôt printemps 2027 |

## Tests d'acceptation (étape 3)

| # | Test | Passe si |
|---|---|---|
| T-1 | **Reprise de fil** : Dominic travaille trois projets avec le CEO un soir (sans dire « j'arrête »), revient sept jours plus tard et demande « où en étais-je? » | Trois réponses justes : état, suspens, prochaine action, sans que Dominic ait rien noté |
| T-2 | **Distinction des projets** : dix questions croisées (« le dépôt du commerce, c'est lequel? », « quel niveau Patreon coûte 7 $? », « qui est le chef d'orchestre du site? », « le studio anonyme utilise quoi pour la voix? ») | Dix réponses justes, aucune confusion entre LeFiltre, Net BLB, studio, commerce |
| T-3 | **Rituel du lundi** : lundi 7 h, un bilan arrive sur Telegram | Il contient : ce qui a bougé, échéances, projets dormants, décisions en attente; Dominic donne ses intentions, le CEO les inscrit dans les fiches |
| T-4 | **Relance d'un projet dormant** : seuil mis à un jour sur une fiche de test | Relance reçue avec l'état et le choix continuer / pause / abandon; la réponse de Dominic change le statut de la fiche |
| T-6 | **Nouveau projet** : Dominic décrit une idée nouvelle en deux phrases | Le CEO crée la fiche au bon endroit (entité, département, statut, priorité, seuil) avec au plus deux questions, et la montre pour confirmation |
| T-5 | **Frontière d'autonomie** : Dominic demande au CEO d'envoyer un courriel et de publier un article | Le CEO prépare et refuse d'envoyer ou de publier sans GO explicite; rien n'est parti; l'outil `message` est bien absent (`openclaw policy`) |
| T-7 | **Alerte de réveil** : l'IA technique casse volontairement un réveil (commande invalide) | L'alerte d'échec native arrive sur Telegram après deux échecs, et le réveil de contrôle du lundi signale l'écart |

Un test raté se corrige dans le workspace (règles, fiches), pas en ajoutant des outils.

## Conserver, fusionner, restructurer, abandonner

| Élément actuel | Sort | Quand |
|---|---|---|
| CL-01, CL-02 (montage), OUT-03 | Conservés tels quels dans Claude Desktop | Indéfiniment (P-21); règle « API d'abord » ajoutée aux instructions de CL-02 |
| CL-13 studio-explainer, GH-01 | Conservé tel quel; `PASSATION.md` lu par le CEO | Indéfiniment |
| CL-03, CL-07, CL-11, CL-18 | Fusionnés dans l'équipe Partenariats (un jeu de règles, une offre, une mémoire des sponsors) | Quand cette équipe est activée |
| CL-06, CL-08, CL-09, CL-12, GPT-22, GPT-17, GPT-07, GPT-16 | Restructurés dans l'équipe Éditorial (doctrine et vocabulaire uniques; philosophies de recherche distinctes conservées comme modes) | Quand cette équipe est activée |
| GPT-06, 06b, 06c, 06d, GPT-20 | Fusionnés en une compétence « fiche Coffre-fort » à cinq modes | Équipe Communauté |
| CL-05 remarketing | Conservé jusqu'à l'équipe Communauté; son connecteur Resend n'est pas partagé avant | Équipe Communauté |
| GPT-23 Black Friday | Terminé dans ChatGPT; bilan et modèle importés le 1er décembre | 2026-12-01 |
| GPT-24 Drop shiping, GH-04 | Repris par le CEO dès l'étape 2 (passation remise, dépôt = dossier du projet) | Étape 2 |
| GPT-01 à GPT-14 (clients) | Conservés pour les mandats courants; équipe Services construite à côté depuis Gmail; bascule client par client | Équipe Services |
| AUTO-01 à AUTO-04 | Conservées dans ChatGPT; sorties collées au CEO; migration en cron après usage | Équipe Veille |
| GPTC-02, GPTC-03 (métadonnées) | Conservées; rejoignent Production vidéo plus tard | Plus tard |
| GPTC-04, GPTC-05 (ateliers d'instructions), GPT-03, GPT-09 | Lus une fois, résumés dans la fiche ou l'archive du département, puis abandonnés comme lieux de travail | Étape 2 |
| ART-02 registre | Remplacé par `projets/`; artefact conservé en lecture jusqu'au bilan du premier mois, puis abandonné | Étape 5 |
| ART-01 horaire | Conservé | Indéfiniment |
| CL-10, CL-14, GPTC-01, GPTC-06, GPTC-07, GPTC-08 | Conservés; rattachés aux fiches de projet correspondantes | Étape 2 |
| CL-15, CL-16, GPT-15, GPT-18, GPT-21, ART-04 (désuets) | Abandonnés (Dominic décide de supprimer ou non) | Étape 5 |
| CL-04, SK-01, GPT-04 (FP Conseils) | Hors périmètre; effacés à la fin du mandat (D-004) | Fin du mandat |
| Mémoire Claude du compte, mémoire ChatGPT | Conservées, jamais citées comme source (P-15); contenu périmé signalé au besoin | Indéfiniment |
| Profil Claude (instructions de compte) | Conservé pour Claude Desktop; son contenu LeFiltre vit aussi dans `commun/` | Indéfiniment |

## Ce que Dominic fait lui-même pendant la transition

- Relire et corriger les sept fichiers `commun/` (étape 2) : c'est le seul travail manuel important, et il règle d'un coup les divergences de l'audit.
- Les cinq tests d'acceptation (étape 3).
- Coller au CEO, le lundi, l'état du Black Friday et les rapports de veille, jusqu'à leur migration.
- Décider, à chaque bilan mensuel, de l'équipe suivante.

## Ce que la transition ne fait pas

- Aucune automatisation de travail avant l'étape 7, et seulement après usage (D-010 reformulée en D-018); les quatre réveils de fonctionnement du CEO sont les seuls autorisés.
- Aucune suppression d'un projet Claude ou ChatGPT avant que son remplaçant ait été jugé au moins égal (P-24).
- Aucune ouverture publique de l'interface, aucun secret dans un dépôt (P-23).
