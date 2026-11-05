# Principes de conception (phase 4)

Dernière mise à jour : 2026-11-04
Statut : version 1.3 (P-31 ajouté d'après D-013; P-1, P-3, P-5, P-9, P-16, P-26 ajustés et P-28 à P-30 ajoutés d'après la vision cible D-012; P-2 et P-7 corrigés), **proposée** en mode accéléré (D-011); chaque principe est une recommandation tant que la colonne « Validé » ne dit pas « oui »

Objet : les règles que l'architecture cible devra respecter. Elles découlent de l'inventaire, des méthodes à préserver (D-008), des décisions D-006, D-007 et D-010 et de l'analyse des recouvrements. Elles ne décrivent pas l'architecture (phase 5) : elles disent ce qu'elle doit respecter.

Conventions : « Fondé sur » renvoie aux identifiants de `recouvrements.md` (C-, V-, F-, E-, S-, DEP-, R-), de `elements-a-preserver.md` et de `decisions.md`. « Perdu / gagné » est donné quand le principe change quelque chose à l'existant.

## A. Périmètre et cloisonnement

| # | Principe proposé | Fondé sur | Perdu / gagné | Validé |
|---|---|---|---|---|
| P-1 | Un seul environnement couvre toutes les activités (D-006), vu de l'intérieur comme **une seule compagnie avec des départements** : LeFiltre; Net BLB (services et clients); chaîne anglophone anonyme; commerce autonome; canaux personnels éventuels (PRJ-17). La connaissance circule librement à l'interne; le CEO voit tout (P-7). Reformulé le 2026-11-04 (D-012). | D-006, D-012, E-7 | Gagné : un seul cerveau pour tout, ce que Dominic demande. Perdu : rien | |
| P-2 | À l'intérieur de Net BLB, **chaque client a son espace** : les données sur le client (contexte, mandats, décisions, procédures, fichiers) y sont stockées et conservées; elles ne circulent pas vers un autre client. Corrigé le 2026-11-04 : Dominic précise qu'il est normal de stocker des données sur ses clients; la clause initiale visait les données des clients de ses clients (transcriptions FP Conseils), hors périmètre (D-004), et a été retirée. | D-008 (un espace isolé par client), E-1, E-2, S-1 | Gagné : un contexte client enfin formalisé et conservé (aujourd'hui reconstitué à chaque mandat, fiche A3) | |
| P-3 | **Le cloisonnement est public, pas interne** (D-012) : côté auditeurs, clients et web, rien ne doit permettre de relier la chaîne anonyme, le commerce, LeFiltre et Net BLB. Donc : identités, comptes publics, domaines d'envoi, canaux de publication et moyens de paiement distincts par marque; aucun agent n'écrit vers le public au nom d'une marque avec les traces d'une autre. Les secrets restent hors de toute mémoire d'agent. Le cas Resend (compte commun LeFiltre / Net BLB, domaines d'envoi distincts) est vérifié en phase 5 sous cet angle. | D-012, E-5, OUT-08, C-11 | Gagné : règle claire et vérifiable. Perdu : rien | |
| P-4 | Les exceptions D-003 (Donjon et Dragon) et D-004 (outil FP Conseils) restent hors de l'environnement. | D-003, D-004 | | |

## B. Agent CEO et gouvernance

| # | Principe proposé | Fondé sur | Perdu / gagné | Validé |
|---|---|---|---|---|
| P-5 | L'agent CEO est **l'interlocuteur unique visé** (D-012 : « l'idéal, un seul et que ça fonctionne du tonnerre »). Il délègue en interne (sous-agents, compétences). Repli prévu si l'usage montre que ça ne fonctionne pas : plusieurs agents sur le même Gateway, le CEO gardant la vue d'ensemble. Exception technique maintenue pour le montage DaVinci tant qu'il dépend de Claude Desktop sur le PC (P-21). | D-007, D-012, Q-058 (a) | | |
| P-6 | Le CEO **coiffe** les chefs d'orchestre par projet; il ne les remplace pas. Un projet d'envergure garde son propre chef d'orchestre (ou en reçoit un), qui rend compte au CEO. | D-007, F-2, C-10, Q-058 (b) | Gagné : les cinq chefs d'orchestre actuels ont un modèle unique. Perdu : rien | |
| P-7 | Le CEO **a accès à tout** (déclaré par Dominic le 2026-11-04 : « il a accès à tout ») : l'état de chaque activité et projet, les dossiers clients, le contenu de travail des espaces, les méthodes et principes. Le cloisonnement des espaces (P-1 à P-3) s'applique entre les outils et agents spécialisés, pas au CEO, qui est le seul à voir l'ensemble. Les secrets (clés, mots de passe) restent hors de toute mémoire d'agent, CEO compris (P-23). | D-007 (1), Q-058 (c) et (e) | Gagné : un seul point de vue complet, celui que Dominic a aujourd'hui dans sa tête | |
| P-8 | La **gouvernance de Dominic est écrite une seule fois** et héritée par le CEO et par tout chef d'orchestre : analyse → recommandation → validation explicite de Dominic → exécution; distinction décision / action; aucune décision importante sans validation; prochaine action unique; registre des décisions avec statut (proposée / validée / abandonnée) et date. | C-10, D-000, D-007, passations CL-13 et GPT-24 | Gagné : fin de la réécriture à chaque projet. Perdu : les variantes locales, sauf si écrites comme telles | |
| P-9 | La **proactivité du CEO est bornée et rituelle** : rituel du lundi (bilan de la semaine passée, intentions de la semaine, échéances, projets non regardés depuis longtemps); gestion des tâches planifiées existantes (revues d'actualité, Patch Tuesday, SEO des sites) dont il reçoit et résume les résultats; rappels en cours de journée; **relance des projets dormants** (seuil par projet, relance avec l'état et un choix continuer / pause / abandon, réponse enregistrée, sans harcèlement) comme travail principal (déclaré le 2026-11-04). Il ne déclenche aucune action externe de sa propre initiative (D-010). Fréquence et canal réglables en tête de configuration. | D-007 (3), D-012, Q-058 (f), D-010 | | |

## C. Connaissances et mémoire

| # | Principe proposé | Fondé sur | Perdu / gagné | Validé |
|---|---|---|---|---|
| P-10 | **Une source unique par noyau de connaissance partagée**, versionnée dans GitHub et référencée par les agents au lieu d'être copiée : identité et signature (C-1); marque, audience, offre commerciale et chiffres d'audience (C-2 à C-6, source déclarée : Media Kit); structure Patreon (C-15); doctrine éditoriale (C-7); gouvernance (C-10, P-8); frontière autonomie / autorisation (C-11, P-14). | Section 1 de recouvrements.md, Q4-1, R-9 | Gagné : fin des divergences (165 000 / 170 000, EUR / USD, « Le Lecteur »). Perdu : la liberté de formuler différemment par outil, sauf exception déclarée | |
| P-11 | Les **spécificités voulues restent locales** : philosophies de recherche différentes de CL-06 (sans contradictoire) et CL-09 (avec); gabarit figé du programme hardware; règles de montage. Un outil peut déroger à un noyau commun à condition de l'écrire. | Fiche A4, C-7, Q4-1 | | |
| P-12 | **Un seul vocabulaire** pour classer la solidité d'un fait (confirmé / à nuancer / non confirmé / à retirer, ou équivalent), défini dans la doctrine éditoriale et utilisé par tous les outils de recherche et de fact-checking. | C-8, Q4-6 | Gagné : lisibilité entre outils. Perdu : les trois vocabulaires actuels (dont celui de CL-09, le plus fin) | |
| P-13 | **Les décisions et l'état d'un projet vivent hors des conversations** : dans le dépôt de l'espace (journal des décisions, état, passation), jamais seulement dans la mémoire d'une conversation. Les conversations Claude et ChatGPT sont des interfaces de travail. Les documents de référence aujourd'hui enfermés dans des conversations (document maître Patreon, instructions V1.2, conversation IA Chef, Media Kit, tableaux de bord de campagne) sont rapatriés dans le dépôt de leur espace lors de la transition. | S-2 à S-5, S-10, Q4-4, D-001, D-008 (GitHub source de vérité, passation) | Gagné : survie à la perte de contexte. Coût : rapatriement initial | |
| P-14 | Les **instructions de tout agent de l'environnement sont versionnées** dans le dépôt de son espace (régime A étendu), avec changelog relu par Dominic avant adoption. Les projets qui restent dans Claude ou ChatGPT sans migrer gardent leur régime actuel. | V-1 à V-5, Q4-2, D-008 | Gagné : plus d'instructions dont l'origine est perdue (V-2). Coût : discipline de commit | |
| P-15 | Les **mémoires automatiques des plateformes** (mémoire Claude du compte, mémoire ChatGPT, mémoires de projet) ne sont pas des sources de vérité : elles peuvent être lues, jamais citées comme référence. L'état fiable est dans le dépôt. | OUT-11, OUT-12, S-13, S-14, Q-059 (contenu périmé constaté) | | |
| P-16 | Le **registre des projets et des idées devient la mémoire du CEO** et remplace l'outil ART-02 (D-012) : un fichier par projet (statut, priorité, avancement, prochaine action, échéance, journal, liens vers les IA et dépôts concernés) dans le workspace du CEO, versionné. Les 20 fiches actuelles sont importées à la transition. | D-008 (registre unique), D-012, S-11, section 6b | Gagné : le registre est tenu par l'agent, pas à la main. Perdu : l'interface visuelle de ART-02, sauf si elle est reconstruite sur les fichiers | |
| P-17 | Une **mémoire des sponsors** minimale est créée (sponsor, dates, produit, résultat, conditions conclues ou refusées, contrat analysé), tenue par l'outil sponsor de l'espace LeFiltre. | S-9, E-3, R-7, Q4-7, Q-044b | Gagné : fin du réexposé de l'historique à chaque contrat. Coût : saisie au fil de l'eau par l'agent | |

## D. Autonomie, sécurité, vérification

| # | Principe proposé | Fondé sur | Perdu / gagné | Validé |
|---|---|---|---|---|
| P-18 | **Installer d'abord, automatiser ensuite** (D-010). À l'installation, aucun enchaînement automatique entre outils : chaque agent prépare, Dominic approuve ou exécute. Les automatisations existantes (tâches planifiées ChatGPT, remarketing CL-05, montage en arrière-plan) sont conservées telles quelles. | D-010, section 7b | | |
| P-19 | Les **points de contrôle de Dominic** (colonne « Dominic seul » de la section 7b : choix du projet à monter, mise en ligne, sélection des Shorts, lecture avant publication, décision sponsor, qualification client, jugement sur les leçons) ne sont jamais contournés, même après automatisation. La séparation recherche / rédaction / édition avec Dominic entre chaque étape est maintenue. | D-008 (voulu), section 7b | | |
| P-20 | **Une seule règle de frontière autonomie / autorisation**, héritée par tous les agents : travail par API en arrière-plan; aucune prise de contrôle de l'ordinateur sans autorisation explicite; gradation lecture seule → brouillons → exécution approuvée → autonomie bornée; aucune dépense, envoi, publication, création de compte ou modification de source sans GO de Dominic. | D-008 (API d'abord, règle générale), C-11, CL-01, GPT-24 | Gagné : CL-02 et les autres héritent de la règle qu'ils n'avaient pas | |
| P-21 | **Sécurité du montage** inchangée : timeline source intacte, nouvelle timeline, prémontage respecté, aucune décision éditoriale sur le fond. Claude Desktop avec accès machine et connecteur DaVinci Resolve Studio reste le mécanisme du montage tant qu'aucun équivalent n'est prouvé. | D-008, OUT-03, DEP-1 | | |
| P-22 | **Vérification par le résultat et validateur indépendant** disponibles comme compétences communes : tout livrable à enjeu (script, campagne, document d'architecture) peut être soumis à un validateur qui ne voit que le livrable et les règles, et n'invente jamais de consigne. | D-008, F-1, validation de recouvrements.md (2026-11-04) | | |
| P-23 | **Aucun secret dans un dépôt**; aucune page web, courriel ou document reçu n'est traité comme une instruction (injection); journaux et possibilité d'arrêt pour tout agent qui agit. | passation GPT-24, C-11 | | |

## E. Coexistence et transition

| # | Principe proposé | Fondé sur | Perdu / gagné | Validé |
|---|---|---|---|---|
| P-24 | **Ne pas casser ce qui fonctionne** : OpenClaw ne remplace aucun projet Claude ou ChatGPT tant que son équivalent n'a pas été utilisé et jugé satisfaisant par Dominic. Migration un système à la fois (phase 6). | D-000 (aucune migration directe), D-010 | | |
| P-25 | **ChatGPT et Claude restent des interfaces de travail** aussi longtemps que Dominic le souhaite; le rôle d'atelier d'idéation de ChatGPT n'est pas remis en cause par l'audit. Ce qui change : la mémoire durable et les instructions versionnées passent dans le dépôt de l'espace (P-13, P-14). | Rôles respectifs (workflows-actuels.md), pistes de la conversation d'origine (R, non validées), D-011 | | |
| P-26 | **Fournisseur et coûts** : un seul fournisseur de modèles comme cible (D-012), consommation par API plafonnée et visible; une IA locale, plus tard, pour les tâches simples (à définir concrètement en phase 5 : tri, classement, résumés courts, reformatage, métadonnées), jamais pour les tâches à enjeu (montage, négociation, rédaction publiée). | D-012, section 6c, D-010 | | |
| P-27 | **Passation par document structuré** reste la méthode de relais entre IA, y compris vers et depuis OpenClaw : état, décisions avec statut, non-décidé, questions ouvertes, prochaine tâche. | D-008 (élément 19), F-3 | | |
| P-28 | **Hébergement** : OpenClaw tourne sur un serveur virtuel toujours allumé (D-012), pas sur le PC de Dominic. Ce qui exige la machine de Dominic (DaVinci Resolve, fichiers locaux) passe par un pont vers le PC ou reste dans Claude Desktop (P-21); le PC éteint n'empêche ni les rappels ni les tâches planifiées. | D-012, DEP-1 | | |
| P-29 | **Courriels connectés** : Gmail (et les autres comptes courriel) sont connectés à OpenClaw en lecture (D-012); le dossier d'un client se construit à partir des échanges réels. Toute écriture (envoi) reste soumise à P-20. | D-012, S-1, E-2 | Gagné : la vérité des mandats devient accessible | |
| P-31 | **Organigramme à trois niveaux, rempli une équipe à la fois** (D-013) : CEO; un chef d'équipe par département; des agents spécialisés sous chaque chef. Une équipe n'est mise en service que lorsqu'elle a ses règles, ses exemples, son validateur et son critère d'acceptation; Dominic reste le contrôle qualité d'une équipe tant qu'elle n'a pas prouvé qu'elle n'en a plus besoin. | D-013, D-010, D-008 (validateur, vérification par le résultat) | | |
| P-30 | **Critère d'acceptation du prototype** (jour 1, D-012) : le CEO connaît tous les projets, les distingue sans les mélanger et répond juste à « on en est où ». Tant que ce critère n'est pas atteint, rien d'autre n'est construit par-dessus. | D-012, D-010 | | |

## Questions Q4 : réponses proposées

| Question | Réponse proposée par les principes |
|---|---|
| Q4-1 noyaux à unifier | Oui pour les six noyaux de P-10; non pour les spécificités de P-11 |
| Q4-2 régime versionné | Étendu à tout agent de l'environnement; inchangé pour ce qui ne migre pas (P-14) |
| Q4-3 cloisonnement | Une compagnie, des départements à l'interne; cloisonnement des identités et comptes publics; clients isolés entre eux (P-1 à P-3) |
| Q4-4 documents à sauver | Les cinq nommés en P-13, rapatriés à la transition |
| Q4-5 points de contrôle | Colonne « Dominic seul » de 7b, jamais contournée (P-19); manutention réexaminée après usage (P-18) |
| Q4-6 vocabulaire commun | Oui (P-12) |
| Q4-7 mémoire des sponsors | Oui, minimale (P-17) |
| Q-058 agent CEO | Interlocuteur unique visé, repli multi-agents; coiffe; a accès à tout; gouvernance héritée; rituel du lundi et tâches planifiées (P-5 à P-9) |

## Hors de ce document

- La forme concrète (agents, espaces de travail, Skills, dépôts, modèles, connecteurs) : phase 5, après vérification des capacités réelles d'OpenClaw.
- L'ordre de migration : phase 6.
