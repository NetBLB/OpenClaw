# Principes de conception (phase 4)

Dernière mise à jour : 2026-11-04
Statut : version 1.1 (P-2 et P-7 corrigés sur déclaration de Dominic), **proposée** en mode accéléré (D-011); chaque principe est une recommandation tant que la colonne « Validé » ne dit pas « oui »

Objet : les règles que l'architecture cible devra respecter. Elles découlent de l'inventaire, des méthodes à préserver (D-008), des décisions D-006, D-007 et D-010 et de l'analyse des recouvrements. Elles ne décrivent pas l'architecture (phase 5) : elles disent ce qu'elle doit respecter.

Conventions : « Fondé sur » renvoie aux identifiants de `recouvrements.md` (C-, V-, F-, E-, S-, DEP-, R-), de `elements-a-preserver.md` et de `decisions.md`. « Perdu / gagné » est donné quand le principe change quelque chose à l'existant.

## A. Périmètre et cloisonnement

| # | Principe proposé | Fondé sur | Perdu / gagné | Validé |
|---|---|---|---|---|
| P-1 | Un seul environnement couvre toutes les activités (D-006), organisé en **quatre espaces** cloisonnés : LeFiltre; Net BLB (services et clients); chaîne anglophone anonyme; commerce autonome IA. Un cinquième espace « Dominic » porte ce qui est commun à tout (identité, préférences, gouvernance). | D-006, E-7, Lecture 4, D-008 (isolation par client) | Gagné : l'exigence d'isolation de GPT-24 est satisfaite par construction. Perdu : rien de l'existant | |
| P-2 | À l'intérieur de Net BLB, **chaque client a son espace** : les données sur le client (contexte, mandats, décisions, procédures, fichiers) y sont stockées et conservées; elles ne circulent pas vers un autre client. Corrigé le 2026-11-04 : Dominic précise qu'il est normal de stocker des données sur ses clients; la clause initiale visait les données des clients de ses clients (transcriptions FP Conseils), hors périmètre (D-004), et a été retirée. | D-008 (un espace isolé par client), E-1, E-2, S-1 | Gagné : un contexte client enfin formalisé et conservé (aujourd'hui reconstitué à chaque mandat, fiche A3) | |
| P-3 | **Secrets, comptes et connecteurs sont rattachés à un espace**, jamais partagés par défaut. Le cas Resend (compte commun LeFiltre / Net BLB) est traité explicitement en phase 5 : soit deux comptes, soit un seul connecteur avec règle d'usage écrite. | E-5, OUT-08, C-11, passation GPT-24 (sécurité) | Gagné : fin de l'exposition de Resend à tout projet Claude. Perdu : simplicité d'un compte unique | |
| P-4 | Les exceptions D-003 (Donjon et Dragon) et D-004 (outil FP Conseils) restent hors de l'environnement. | D-003, D-004 | | |

## B. Agent CEO et gouvernance

| # | Principe proposé | Fondé sur | Perdu / gagné | Validé |
|---|---|---|---|---|
| P-5 | L'agent CEO est le **point d'entrée principal mais non exclusif** : Dominic continue de parler directement aux outils spécialisés quand c'est plus simple (monteur DaVinci, recherchiste). Le CEO n'intercepte pas ces échanges; il en reçoit l'état. | D-007, Q-058 (a), R-1, D-010 | Gagné : pas de goulot. Perdu : rien | |
| P-6 | Le CEO **coiffe** les chefs d'orchestre par projet; il ne les remplace pas. Un projet d'envergure garde son propre chef d'orchestre (ou en reçoit un), qui rend compte au CEO. | D-007, F-2, C-10, Q-058 (b) | Gagné : les cinq chefs d'orchestre actuels ont un modèle unique. Perdu : rien | |
| P-7 | Le CEO **a accès à tout** (déclaré par Dominic le 2026-11-04 : « il a accès à tout ») : l'état de chaque activité et projet, les dossiers clients, le contenu de travail des espaces, les méthodes et principes. Le cloisonnement des espaces (P-1 à P-3) s'applique entre les outils et agents spécialisés, pas au CEO, qui est le seul à voir l'ensemble. Les secrets (clés, mots de passe) restent hors de toute mémoire d'agent, CEO compris (P-23). | D-007 (1), Q-058 (c) et (e) | Gagné : un seul point de vue complet, celui que Dominic a aujourd'hui dans sa tête | |
| P-8 | La **gouvernance de Dominic est écrite une seule fois** et héritée par le CEO et par tout chef d'orchestre : analyse → recommandation → validation explicite de Dominic → exécution; distinction décision / action; aucune décision importante sans validation; prochaine action unique; registre des décisions avec statut (proposée / validée / abandonnée) et date. | C-10, D-000, D-007, passations CL-13 et GPT-24 | Gagné : fin de la réécriture à chaque projet. Perdu : les variantes locales, sauf si écrites comme telles | |
| P-9 | La **proactivité du CEO est bornée** : il rappelle (échéances, questions ouvertes, décisions en attente, tâches de maintenance), il ne déclenche aucune action externe de sa propre initiative. Fréquence et canal des rappels fixés par Dominic et modifiables en tête de configuration. | D-007 (3), Q-058 (f), D-010, D-008 (paramètres en tête) | | |

## C. Connaissances et mémoire

| # | Principe proposé | Fondé sur | Perdu / gagné | Validé |
|---|---|---|---|---|
| P-10 | **Une source unique par noyau de connaissance partagée**, versionnée dans GitHub et référencée par les agents au lieu d'être copiée : identité et signature (C-1); marque, audience, offre commerciale et chiffres d'audience (C-2 à C-6, source déclarée : Media Kit); structure Patreon (C-15); doctrine éditoriale (C-7); gouvernance (C-10, P-8); frontière autonomie / autorisation (C-11, P-14). | Section 1 de recouvrements.md, Q4-1, R-9 | Gagné : fin des divergences (165 000 / 170 000, EUR / USD, « Le Lecteur »). Perdu : la liberté de formuler différemment par outil, sauf exception déclarée | |
| P-11 | Les **spécificités voulues restent locales** : philosophies de recherche différentes de CL-06 (sans contradictoire) et CL-09 (avec); gabarit figé du programme hardware; règles de montage. Un outil peut déroger à un noyau commun à condition de l'écrire. | Fiche A4, C-7, Q4-1 | | |
| P-12 | **Un seul vocabulaire** pour classer la solidité d'un fait (confirmé / à nuancer / non confirmé / à retirer, ou équivalent), défini dans la doctrine éditoriale et utilisé par tous les outils de recherche et de fact-checking. | C-8, Q4-6 | Gagné : lisibilité entre outils. Perdu : les trois vocabulaires actuels (dont celui de CL-09, le plus fin) | |
| P-13 | **Les décisions et l'état d'un projet vivent hors des conversations** : dans le dépôt de l'espace (journal des décisions, état, passation), jamais seulement dans la mémoire d'une conversation. Les conversations Claude et ChatGPT sont des interfaces de travail. Les documents de référence aujourd'hui enfermés dans des conversations (document maître Patreon, instructions V1.2, conversation IA Chef, Media Kit, tableaux de bord de campagne) sont rapatriés dans le dépôt de leur espace lors de la transition. | S-2 à S-5, S-10, Q4-4, D-001, D-008 (GitHub source de vérité, passation) | Gagné : survie à la perte de contexte. Coût : rapatriement initial | |
| P-14 | Les **instructions de tout agent de l'environnement sont versionnées** dans le dépôt de son espace (régime A étendu), avec changelog relu par Dominic avant adoption. Les projets qui restent dans Claude ou ChatGPT sans migrer gardent leur régime actuel. | V-1 à V-5, Q4-2, D-008 | Gagné : plus d'instructions dont l'origine est perdue (V-2). Coût : discipline de commit | |
| P-15 | Les **mémoires automatiques des plateformes** (mémoire Claude du compte, mémoire ChatGPT, mémoires de projet) ne sont pas des sources de vérité : elles peuvent être lues, jamais citées comme référence. L'état fiable est dans le dépôt. | OUT-11, OUT-12, S-13, S-14, Q-059 (contenu périmé constaté) | | |
| P-16 | Le **registre unique des idées** (aujourd'hui ART-02) est conservé comme méthode; sa forme est décidée en phase 5 (artefact tel quel, lu par le CEO, ou rapatrié). Le CEO s'appuie sur lui pour l'état des projets. | D-008 (registre unique), S-11, Q-035 | | |
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
| P-26 | **Coûts** : exploiter d'abord les forfaits existants (Claude gros volume, ChatGPT de base); toute consommation par API est plafonnée et visible; un modèle local n'est envisagé que pour des tâches simples et après mesure. | Section 6c (contexte de coûts, pistes R), D-010 | | |
| P-27 | **Passation par document structuré** reste la méthode de relais entre IA, y compris vers et depuis OpenClaw : état, décisions avec statut, non-décidé, questions ouvertes, prochaine tâche. | D-008 (élément 19), F-3 | | |

## Questions Q4 : réponses proposées

| Question | Réponse proposée par les principes |
|---|---|
| Q4-1 noyaux à unifier | Oui pour les six noyaux de P-10; non pour les spécificités de P-11 |
| Q4-2 régime versionné | Étendu à tout agent de l'environnement; inchangé pour ce qui ne migre pas (P-14) |
| Q4-3 cloisonnement | Quatre espaces + un espace commun; clients isolés dans Net BLB; secrets par espace (P-1 à P-3) |
| Q4-4 documents à sauver | Les cinq nommés en P-13, rapatriés à la transition |
| Q4-5 points de contrôle | Colonne « Dominic seul » de 7b, jamais contournée (P-19); manutention réexaminée après usage (P-18) |
| Q4-6 vocabulaire commun | Oui (P-12) |
| Q4-7 mémoire des sponsors | Oui, minimale (P-17) |
| Q-058 agent CEO | Principal non exclusif; coiffe; a accès à tout; gouvernance héritée; rappels bornés (P-5 à P-9) |

## Hors de ce document

- La forme concrète (agents, espaces de travail, Skills, dépôts, modèles, connecteurs) : phase 5, après vérification des capacités réelles d'OpenClaw.
- L'ordre de migration : phase 6.
