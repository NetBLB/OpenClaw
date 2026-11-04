# Relations et recouvrements (phase 3)

Dernière mise à jour : 2026-11-04
Statut : version 1.1, corrigée le 2026-11-04 après validation par une IA indépendante (21 énoncés nuancés, 2 citations corrigées, 3 recommandations déguisées retirées, 6 omissions ajoutées); à compléter avec Dominic

Objet : vue transversale de l'écosystème. Où la même connaissance est répétée, où la même fonction est reproduite, où l'information est enfermée, de quoi chaque système dépend sans le dire, et où Dominic répète du contexte à la main.

Règles de lecture :
- (O) observation constatée dans les fichiers, captures ou déclarations; (H) hypothèse non confirmée.
- Ce document ne contient **aucune recommandation d'architecture** et **aucune décision**. Quand une observation appelle un choix, elle est formulée comme une question pour la phase 4 (préfixe « Q4 »).
- Le fait qu'une chose soit dupliquée n'est pas en soi un défaut : certaines duplications sont voulues (test comparatif, cloisonnement). Le document les distingue.
- Sources : `inventaire.md` (sections 8 et 10), `workflows-actuels.md`, `elements-a-preserver.md` (D-008), fichiers d'instructions lus en phase 2.

## 1. Connaissances répétées en plusieurs copies

Connaissances qui existent dans plusieurs jeux d'instructions, entretenues à la main, sans source unique observée.

| # | Connaissance | Copies observées | Divergences constatées | Statut |
|---|---|---|---|---|
| C-1 | Identité et signature (Dominic Lessard, Net BLB inc., LeFiltre, dom@lefiltre.com) | Profil Claude, CL-03, CL-07, CL-11; aussi dans la mémoire Claude du compte (OUT-11, observée hors dépôt) | Aucune vue | O |
| C-2 | Contexte de marque et d'audience LeFiltre (thématiques, 40-65 ans, Europe francophone, positionnement calme et pédagogique) | Profil Claude, CL-03, CL-07, CL-11, CL-12, CL-09 (ton), CL-08 (ton), GPT-23 | Abonnés : 165 000+ (CL-11, profil) vs 170 000+ (CL-03, CL-07, CL-12); âge : « 45 ans et plus » (CL-03) vs « 40 à 65 » (ailleurs) vs « 40 ans et plus, retraités » (GPT-23); géographie formulée différemment dans chacun des quatre jeux sponsors (fiche A1); non vérifié pour CL-09, CL-08, GPT-23 | O |
| C-3 | Chiffres d'audience (abonnés, vues, Patreon) | Media Kit (CL-18, source déclarée), CL-03, CL-07, CL-11, profil | Déjà listées en fiche A1; CL-03 interdit de citer des chiffres Patreon alors que CL-07 en cite un | O |
| C-4 | Grille tarifaire sponsor | Profil Claude, CL-03, CL-11 | Devise (EUR vs USD), durée du Tier 3 (90-120 s vs 60-90 s), Formule Dédiée absente de CL-11 et du profil | O |
| C-5 | Catégories de sponsors refusées | Profil, CL-03, CL-07, CL-11 | Quatre listes différentes; aucune liste officielle (déclaré, Q-043) | O |
| C-6 | Conditions non négociables et droits non inclus | CL-03 (complet), CL-11 (en creux, via les red flags) | CL-03 ne fixe ni délai de paiement ni durée d'exclusivité; CL-11 les traite comme bloquants au-delà de 30 jours | O |
| C-7 | Doctrine éditoriale : sources primaires d'abord, ne jamais inventer, liens directs | CL-06, CL-09, CL-08; principe « ne jamais inventer » aussi dans CL-04 / SK-01 et CL-13 (validateur) | Hiérarchie des sources écrite deux fois (CL-06, CL-09) avec des philosophies différentes et voulues (sans / avec recherche contradictoire) | O |
| C-8 | Classement de la solidité d'une affirmation | CL-06 (confirmé / en test / annoncé / progressif / rumeur / secondaire), CL-09 (✅ ⚠️ ❓ ❌ en six nuances), CL-08 (confirmé / à nuancer / inexact) | Trois vocabulaires pour la même opération (la grille du validateur de CL-13 classe la gravité des problèmes, pas la solidité des faits : autre chose) | O |
| C-9 | Interdiction du tiret cadratin | Profil Claude, CL-08; aussi dans la mémoire Claude du compte (OUT-11) | Absente de CL-09 et GPT-22 (instructions lues); non vérifié pour CL-12 (instructions vues en partie seulement) | O/H |
| C-10 | Règles de gouvernance d'un chef d'orchestre (analyse → recommandation → validation → exécution; décision vs action; tableau de bord; prochaine action unique; mémoire des décisions) | CL-13 (orchestration), site NetBLB (IA Chef, H : non vue), CL-17 (présent audit), GPT-23, GPT-24 (passation lue, instructions non lues) | Patron commun réécrit de zéro à chaque fois; la liste exacte des règles varie (GPT-24 : registre de décisions et « une question décisive à la fois », sans tableau de bord ni prochaine action unique observés); numérotations de décisions indépendantes (D-001… dans GPT-24 et dans l'audit) | O/H |
| C-11 | Règles de sécurité d'agents (lecture seule → brouillons → exécution approuvée → autonomie bornée; secrets hors dépôt; méfiance envers l'injection) | GPT-24 (gradation lecture seule → brouillons → exécution approuvée → autonomie bornée; secrets hors dépôt; injection), CL-13 (briefs : interdiction d'inventer une consigne, aucune dépense sans GO), CL-01 (API d'abord, pas de contrôle sans autorisation), CL-05 (exécuter via connecteur sauf approbation requise) | Règles différentes selon le projet, qui relèvent toutes de la frontière entre autonomie et autorisation; seule GPT-24 a une gradation complète; CL-02 n'a pas la règle « API d'abord » que Dominic veut générale (D-008) | O |
| C-12 | Structure et format des publications Patreon | GPT-22 (définit), CL-08 (préserve), GPT-17 (H) | GPT-22 porte une contradiction interne sur la ligne d'appel (résolue : ajoutée par Patreon) | O |
| C-13 | Règles métier FP Conseils | CL-04 et SK-01 (deux copies quasi identiques, comparées texte à texte); GPT-04 (mêmes règles déclarées, texte non vu) | Aucune signalée | O (CL-04, SK-01), H (GPT-04); hors périmètre (D-004); cité comme exemple du patron |
| C-14 | Bible et gabarit studio-explainer | v1.5 : deux copies vérifiées identiques (md5, Scripteur et Validateur), troisième attestée par la règle de recopie dans les trois projets; v1.6 : dossier `commun/` unique dans le dépôt | Duplication résolue par conception | O, cas résolu |
| C-15 | Structure Patreon (niveaux, prix, collections, Coffre-fort) | PAtreon.docx, plan de refonte, courriels de bienvenue, GPT-23, GPT-22 (format), CL-05 (redirection vers Patreon), CL-14 (catégories de l'horaire) | Niveau 5 $ nommé « Le Lecteur » dans GPT-23 et dans le plan de refonte alors que le nom officiel est « L'Éveil » (section 6e) | O |

Lecture transversale (O) : les connaissances C-1 à C-6 forment un **noyau « marque, audience, offre commerciale »** répété dans au moins cinq endroits, avec une source déclarée (le Media Kit, lui-même dans un chat Claude) qui n'alimente rien automatiquement. C-7 à C-9 forment un **noyau « doctrine éditoriale »** répété dans trois projets d'écriture avec des vocabulaires différents. C-10 forme un **noyau « gouvernance de chef d'orchestre »** réécrit pour chacun des cinq chefs d'orchestre; C-11 (frontière autonomie / autorisation des agents) est écrit, différemment, dans quatre projets qui ne sont pas tous des chefs d'orchestre. C-15 (structure Patreon) est répété dans au moins cinq documents et instructions avec un reliquat de nom.

Q4-1 : parmi ces noyaux, lesquels Dominic veut-il voir maintenus en un seul endroit, et lesquels doivent rester propres à chaque outil (par exemple, les philosophies de recherche volontairement différentes de CL-06 et CL-09)?

## 2. Instructions dupliquées et dérives de version

| # | Situation | Détail | Statut |
|---|---|---|---|
| V-1 | Atelier ChatGPT en avance sur le projet Claude | CL-09 est en V1; GPT-03 est en V1.2; écart jugé négligeable par Dominic | O |
| V-2 | Instructions dont la conversation d'origine est perdue | CL-03, CL-07, CL-11 : rédigées par ChatGPT, conversation perdue; la copie dans Claude est la seule; aucune version antérieure | O |
| V-3 | Historique d'évolution des instructions vivant côté ChatGPT, dans l'atelier qui les a rédigées | Conversations épinglées : CL-01 (GPTC-04), CL-02 (GPTC-05); projets ChatGPT : CL-09 (GPT-03), CL-10 (GPT-09); conversation liée dans ART-02 : CL-17 (« IA de départ ») | O |
| V-4 | Instructions versionnées dans un dépôt | Seulement CL-13 (briefs, bible numérotée, changelog) et le site NetBLB (`AGENTS.md`); les instructions v1.5 de CL-15 et CL-16 sont archivées dans GH-01; aucun des 13 autres projets Claude n'a d'équivalent | O |
| V-5 | Un seul projet porte un numéro de version dans son nom | CL-09 « V1 »; les autres sont modifiés sans trace (Q-011b) | O |
| V-6 | Duplication volontaire pour test comparatif | CL-04 / GPT-04 (client choisit la plateforme) | O, voulue |
| V-7 | Projets désuets conservés en place | CL-15, CL-16, GPT-15, GPT-18, GPT-21, ART-04; Dominic ne supprime pas (H, Q-013) | O/H |
| V-8 | Instructions qui renvoient à des fichiers absents | GPT-22 exige de s'inspirer d'« images jointes » alors qu'aucune image n'est jointe aux projets ChatGPT (fiche A4, Q-053) | O |
| V-9 | Instructions de projet qui se désignent comme un Skill | CL-02 (« ce Skill »); existence d'un Skill séparé non établie (Q-004) | O/H |

Lecture (O) : il existe **deux régimes de conservation des instructions**. Régime A (deux systèmes) : instructions dans un dépôt, versionnées, avec historique. Régime B (tout le reste) : instructions dans le projet, historique dans une conversation ChatGPT quand elle existe encore, aucune version antérieure. Le régime A est classé « à préserver absolument » (D-008) pour les systèmes qui l'ont; rien n'est décidé pour les autres.

Q4-2 : le régime A doit-il rester limité aux deux systèmes qui l'ont, ou s'appliquer à d'autres projets, et lesquels? (Question de principe, pas de mise en œuvre.)

## 3. Fonctions identiques reproduites dans plusieurs IA

| # | Fonction | Occurrences | Ce qui les distingue | Statut |
|---|---|---|---|---|
| F-1 | Recherche et fact-checking | CL-06 (amont, tous formats), CL-09 (intégré au pipeline éditorial), CL-08 (fact-check d'articles), CL-13 validateur (vérification des faits d'un script) | Format de sortie et philosophie de recherche; compétence de base commune (H) | O (occurrences), H (compétence commune) |
| F-2 | Chef d'orchestre de projet avec gouvernance | CL-13, IA Chef (site, non vue), CL-17, GPT-23, GPT-24 (instructions non vues) | Le projet orchestré; mêmes principes rédigés indépendamment pour les trois vus (O), supposés pour les deux autres (H) | O/H |
| F-3 | Passation d'une IA à une autre | CL-13 (PASSATION.md + zip + prompt), GPT-24 (PASSATION-COMPLETE.md + instructions + prompt) | Plateforme; structure quasi identique | O |
| F-4 | Analyse de contrat | CL-11 (contrats sponsors, usage interne), « décrypteur de contrat » publié sur Patreon via GPTC-08 (communauté) | Public visé | O |
| F-5 | Idées de vidéos YouTube | GPT-07 (actif), GPT-21 (archivé) | Succession dans le temps (H) | O/H |
| F-6 | Rédaction d'articles Patreon | GPT-22 (tous), GPT-17 (sujets IA, peu utilisé) | Sujet; GPT-17 en déclin | O |
| F-7 | Extraction d'un ancien article vers une fiche Coffre-fort | GPT-06, 06b, 06c, 06d, GPT-20 (cinq projets); variantes singulier / pluriel nommées dans le plan de refonte, existence actuelle non établie (Q-037b) | Type de fiche produite; même mécanique | O |
| F-8 | Rédaction d'instructions personnalisées | ChatGPT pour les projets Claude (V-3), GPTC-08 pour les outils offerts à la communauté | Destinataire (Dominic vs membres Patreon) | O |
| F-9 | Montage dans DaVinci Resolve | CL-01 (Shorts), CL-02 (vidéo longue) | Produit; même accès machine, même connecteur, règles de sécurité communes | O |
| F-10 | Métadonnées de publication | GPTC-02 (Shorts → Metricool), GPTC-03 (vidéo longue → YouTube) | Destination; même type de travail | O |
| F-11 | Surveillance planifiée | AUTO-01 (site, hebdomadaire), AUTO-04 (Patch Tuesday, fréquence non précisée) | Sujet; même mécanisme (tâche ChatGPT) | O |
| F-12 | Outil HTML personnel synchronisé par copier-coller | ART-01 (horaire, Claude), tableau de bord refonte Patreon (ChatGPT) | Plateforme; même patron | O |

Lecture : F-1, F-2, F-3 sont des **candidats compétences transversales** au sens des instructions du projet (une même capacité réutilisée dans plusieurs contextes) (H). F-7 est le cas le plus net d'une **mécanique unique déclinée en cinq projets** par le seul type de sortie (O). F-4 et F-8 montrent une même compétence servie à deux publics (Dominic, communauté Patreon) (O). Aucune conclusion de fusion : la séparation recherche / rédaction / édition avec Dominic entre chaque étape est classée « à clarifier » dans `elements-a-preserver.md` (non confirmée, D-008); selon la réponse, elle pèsera sur F-1.

## 4. Clients, entités et partenaires présents à plusieurs endroits

| # | Entité | Emplacements | Lien entre eux | Statut |
|---|---|---|---|---|
| E-1 | FP Conseils | GPT-05 (client, mandats), CL-04 + SK-01 (outil prototype), GPT-04 (copie de test) | Aucun lien technique; D-004 sortira CL-04, SK-01, GPT-04 du périmètre | O |
| E-2 | Chaque client Net BLB | Gmail (vérité des mandats), projet ChatGPT (travail), dossier Windows (archive partielle) | Aucun lien; copier-coller de Dominic | O |
| E-3 | Un même sponsor | CL-03 (négociation), CL-07 (hardware), CL-11 (contrat), ART-03 (grille de test) : Keychron dans CL-03, CL-07 et ART-03; Internxt et Higgsfield dans CL-03 et CL-11 | Aucune mémoire partagée; copier-coller (Q-044b); c'est Dominic qui choisit à la main CL-03 ou CL-07 selon la demande (Q-009) | O |
| E-4 | Higgsfield | Fournisseur (OUT-04, chaîne anonyme) et sponsor (CL-03, CL-11) | Double relation non reliée | O |
| E-5 | Resend | Un seul compte pour LeFiltre (CL-05) et Net BLB (formulaire du site); connecteur Claude global au compte | Le connecteur expose les deux entités à tout projet Claude | O |
| E-6 | LeFiltre et Net BLB dans le profil Claude | Le profil (appliqué à tous les projets Claude) décrit LeFiltre et sa grille sponsor; il s'applique aussi aux projets Net BLB (CL-04) et à l'audit | Contexte LeFiltre injecté partout | O |
| E-7 | Commerce autonome (GPT-24) | Exige « aucune mémoire commune implicite » et comptes séparés de LeFiltre et Net BLB | Première exigence d'isolation entre activités de Dominic | O |

Lecture (O) : l'isolation est forte **entre clients** (un projet chacun, classé à préserver) et faible **entre les activités de Dominic** (profil commun, Resend commun, Higgsfield des deux côtés). GPT-24 formule une exigence d'isolation entre activités; le commerce n'existe pas encore, et D-006 classe cette exigence comme principe candidat pour la phase 4 (H sur son application au reste de l'écosystème).

Q4-3 : quelles activités doivent être cloisonnées entre elles (LeFiltre / Net BLB / chaîne anonyme / commerce), et à quel niveau (connaissances, comptes et connecteurs, secrets)?

## 5. Silos : où l'information est enfermée

| # | Information | Où elle vit | Qui peut y accéder | Statut |
|---|---|---|---|---|
| S-1 | Vérité des mandats clients | Gmail | Dominic seulement; aucune IA | O |
| S-2 | Document maître de la refonte Patreon | Vraisemblablement la conversation ChatGPT de la refonte (Q-038) | ChatGPT dans cette conversation; aucun autre emplacement connu | O/H |
| S-3 | Instructions V1.2 de la vidéo éditoriale | Projet ChatGPT GPT-03 | Pas déployées dans CL-09 | O |
| S-4 | Conversation « IA Chef » du site NetBLB | Conversation ChatGPT (lien dans ART-02) | Non vue par l'audit | O |
| S-5 | Media Kit et chiffres d'audience de référence | Chat Claude hors projet (CL-18) | Recopiés à la main vers quatre jeux d'instructions | O |
| S-6 | Horaire hebdomadaire et son état | Artefact Claude (ART-01), stockage navigateur (H) | Dominic via l'artefact; l'IA via « copier l'état » | O/H |
| S-7 | Style personnel d'écriture de Dominic | Historique du projet CL-08 (appris, jamais écrit); déclaré désuet | À réapprendre si le projet est recréé (H) | O/H |
| S-8 | Mémoire de travail du montage (plans, pipeline technique, dossier lié « production ») | Fichiers écrits par Claude dans CL-01 / CL-02 et sur le PC | Claude; contenu inconnu de Dominic | O |
| S-9 | Historique des sponsors (ententes conclues, refusées, contrats analysés) | Nulle part de connu (Q-044b) | Personne | O/H |
| S-10 | État des campagnes et tableaux de bord des chefs d'orchestre | Conversation ou mémoire du projet ChatGPT (GPT-23) | Le projet seulement | O |
| S-11 | Registre des idées de projets | Base de données de l'artefact ART-02 | Dominic; extractible par l'audit; dates de modification récentes, mais au moins une fiche désynchronisée du réel (PRJ-08, Q-035) | O |
| S-12 | Déduplication du remarketing | Fichiers `historique thinkific*` de CL-05 (H) | CL-05 | H (B2 non faite) |
| S-13 | Contexte client reconstitué par ChatGPT | Mémoire de projet ChatGPT (opaque) | Le projet; efficacité réelle inconnue | H |
| S-14 | Mémoires de compte des deux plateformes | Mémoire Claude du compte (OUT-11 : fichiers sur LeFiltre, signature, identité visuelle, préférences, observés depuis cette session) et mémoire ChatGPT du compte (OUT-12 : a servi à ChatGPT pour décrire l'environnement de Dominic dans la conversation d'origine, section 6c, avec des éléments non vérifiés, Q-023, Q-024b) | Chaque plateforme, à l'insu partiel de Dominic; ni versionnées ni exportées | O (existence), H (contenu ChatGPT) |

Lecture (O) : trois familles de silos. **Silos hors de portée de toute IA** (S-1). **Documents de référence qui vivent dans une conversation** (S-2, S-3, S-4, S-5, S-10) : ils existent, fonctionnent, mais ne sont retrouvables que si la conversation l'est. **Mémoire opaque écrite par l'IA** (S-8, S-13, S-14) : fonctionnelle, non cartographiée par Dominic. Le registre ART-02 (S-11) est le seul index qui relie des éléments des deux plateformes; il contient par projet un statut, un avancement, une prochaine action et un journal, mais pas les décisions détaillées ni l'état de travail, qui restent dans les conversations et les passations.

Q4-4 : quels documents vivant dans des conversations Dominic considère-t-il comme des références à sauver (par exemple le document maître Patreon), et lesquels peuvent rester où ils sont?

## 6. Dépendances implicites

Choses dont un système dépend sans que ce soit écrit dans ses instructions, ou écrit sans que Dominic en ait la carte.

| # | Système | Dépend de | Écrit où | Statut |
|---|---|---|---|---|
| DEP-1 | CL-01, CL-02 | Claude Desktop ouvert sur le PC; connecteur local « DaVinci Resolve Studio » (API de scripting); accès complet à la machine; dossier lié « production » | Partiellement (CL-02 : « utilise le connecteur ») | O |
| DEP-2 | CL-01 | Actif `Power Bins / Master / LeFiltreCOM.mov`; comportement de Metricool (premier frame = vignette); dossier `_claude_stills` | Oui, dans CL-01 | O |
| DEP-3 | CL-05 | Export manuel mensuel depuis Thinkific; connecteur Resend; fichiers d'historique pour la déduplication | Partiellement | O/H |
| DEP-4 | CL-13 | Crédits Higgsfield (600 à 650 par vidéo); voix et style verrouillés; outils Python du dépôt; dépôt GitHub accessible à la conversation | Oui, dans le dépôt | O |
| DEP-5 | AUTO-01 | Windsor.ai (GSC + GA4), propriété « NetBLB - GA4 », nommage des événements GA4 | Oui, dans la tâche | O |
| DEP-6 | Site NetBLB | Codex avec accès au dépôt; déploiement manuel Plesk; Turnstile, Resend, GTM | Oui, dans le dépôt | O |
| DEP-7 | Tous les projets Claude | Instructions de profil Claude (contexte LeFiltre, grille sponsor, exigences de réponse) | Nulle part dans les projets | O |
| DEP-8 | Projets Claude dont les instructions viennent de ChatGPT | Existence de la conversation ChatGPT d'origine pour toute évolution | Nulle part | O |
| DEP-9 | GPT-24 et le présent audit | Accès GitHub en écriture depuis l'IA (à vérifier pour ChatGPT; acquis pour Claude après autorisation) | GPT-24 : oui (« inspecter avant d'écrire ») | O |
| DEP-10 | Chaîne de publication | Copier-coller de Dominic entre chaque maillon (voir section 7) | Nulle part | O |
| DEP-11 | CL-01 | Le résultat de CL-02 : les Shorts sont tirés de la vidéo longue, dans le même projet Resolve (fiche A2) | Implicite | O |
| DEP-12 | Tous les projets Claude | Modèle Fable 5.1 avec effort Élevé (observé sur chaque projet); forfait Claude à gros volume justifié par le montage (section 6c) | Nulle part | O |
| DEP-13 | GPT-23 | Tâche planifiée AUTO-02 qui l'a déclenché ou rappelé (H) | Nulle part | H |

Lecture (O) : les systèmes qui ont un dépôt (DEP-4, DEP-6) ont leurs dépendances écrites; les autres les portent en partie dans les instructions, en partie dans les habitudes de Dominic (DEP-7, DEP-8, DEP-10, DEP-11).

## 7. Points où Dominic répète du contexte à la main

| # | Point de répétition | Fréquence | Ce qui est transmis | Statut |
|---|---|---|---|---|
| R-1 | Message de démarrage d'une session de montage (nom du projet Resolve, timeline, rappel des instructions) | Chaque montage | Même message | O |
| R-2 | Vidéo longue → GPTC-03 (transcription collée) → description YouTube | Chaque vidéo | Transcription | O |
| R-3 | Shorts → GPTC-02 → Metricool | Chaque lot de Shorts | 1 à 10 vidéos, puis titres et descriptions | O |
| R-4 | CL-06 → feuille de notes ou autre IA | Chaque vidéo documentée | Dossier de recherche | O |
| R-5 | GPT-22 → CL-08 → Patreon | Parfois (Q-055b) | Article complet, deux fois | O |
| R-6 | Ancien article → projet d'extraction → Coffre-fort | Chaque fiche | Article, puis fiche | O |
| R-7 | Sponsor : CL-07 ou CL-03 → CL-11 | Chaque contrat | Contrat; historique inconnu (Q-044b) | O/H |
| R-8 | Courriel ou transcription de rencontre → projet ChatGPT client | Chaque mandat | Texte brut | O |
| R-9 | Chiffres d'audience ressaisis dans quatre jeux d'instructions | À chaque mise à jour | Chiffres | O |
| R-10 | « Copier l'état » des outils HTML vers l'IA | Non observée | État de l'outil | O (mécanisme), fréquence inconnue |
| R-11 | Passation complète vers une nouvelle IA | À chaque perte de contexte | Document de passation | O |
| R-12 | Leçons de montage → atelier ChatGPT → instructions de CL-02 | Occasionnel | Exemples, puis règle | H (Q-047b) |

Lecture (O) : le copier-coller est le **mécanisme d'intégration universel** de l'écosystème. Dominic contrôle chaque transfert, et chaque maillon est manuel. R-1, R-9, R-10 et R-11 sont des répétitions de contexte au sens strict; R-2 à R-8 sont des transferts de données dans une chaîne de production; R-12 est une boucle d'apprentissage (H). Les trois ne posent pas la même question. Savoir si ces maillons sont une perte de temps ou un contrôle voulu relève de Dominic (sections « ce qui fonctionne bien / ce qui fait perdre du temps » de `workflows-actuels.md`, encore vides).

Q4-5 : parmi ces points, lesquels Dominic considère-t-il comme des pertes de temps réelles, et lesquels comme des points de contrôle qu'il veut garder (lié à l'élément « séparation recherche / rédaction / édition », à clarifier)?

## 8. Ce qui devrait potentiellement être partagé, et ce qui devrait probablement rester isolé

Hypothèses de travail pour la phase 4, dérivées des sections précédentes. Rien n'est décidé.

**Connaissances aujourd'hui répétées, candidates à une discussion de partage (H)** : le noyau « marque, audience, offre commerciale » (C-1 à C-6, dont les chiffres d'audience); la doctrine éditoriale (C-7) et le classement de solidité des faits (C-8, trois vocabulaires); la gouvernance de chef d'orchestre (C-10) et la frontière autonomie / autorisation (C-11); la structure Patreon (C-15); les leçons de montage (R-12). Questions associées : faut-il un vocabulaire commun de solidité des faits (Q4-6)? Faut-il une mémoire des sponsors, qui n'existe pas aujourd'hui (S-9, Q4-7)?

**Candidats à l'isolation (H)** : chaque client Net BLB (D-008, à préserver); les données de clients de clients (transcriptions FP Conseils, hors périmètre); le commerce autonome vis-à-vis de LeFiltre (exigence GPT-24); la chaîne anonyme vis-à-vis de LeFiltre et Net BLB (entité distincte, anonymat voulu); les secrets et comptes (Resend, Higgsfield, GitHub); les philosophies de recherche volontairement différentes (CL-06 vs CL-09).

**Cas à trancher par Dominic (H)** : le profil Claude, aujourd'hui injecté dans tous les projets y compris Net BLB et l'audit; le compte Resend commun; la relation entre le registre ART-02 et l'inventaire de l'audit; le rôle du futur agent CEO (D-007) face à ces cloisonnements (Q-058).

## 9. Synthèse de la phase 3

Cinq constats structurants, tous (O) :

1. **Trois noyaux de connaissances** sont répétés à la main : marque et offre commerciale (cinq à huit endroits, avec des divergences réelles), doctrine éditoriale (trois projets, trois vocabulaires), gouvernance de chef d'orchestre (cinq chefs d'orchestre). S'y ajoute la structure Patreon, répétée avec un reliquat de nom.
2. **Deux régimes de conservation** coexistent : versionné dans un dépôt (studio-explainer, site NetBLB) et non versionné dans le projet avec historique dans une conversation (tout le reste). Le premier est classé à préserver; le second n'a pas été jugé.
3. **Le copier-coller par Dominic est l'intégration universelle** : fiable, contrôlé, manuel. Une partie est du contrôle voulu, une partie est de la répétition.
4. **L'isolation est forte entre clients et faible entre activités**; le commerce autonome introduit une exigence nouvelle d'isolation entre activités.
5. **Plusieurs documents de référence vivent dans des conversations** (refonte Patreon, V1.2, IA Chef, Media Kit, tableaux de bord de campagne), et une partie de la mémoire des systèmes est écrite par l'IA sans carte côté Dominic (fichiers de montage, mémoires de projet et de compte des deux plateformes).

Sept questions pour la phase 4 : Q4-1 (noyaux à unifier ou non), Q4-2 (régime versionné : limité ou étendu), Q4-3 (cloisonnement entre activités), Q4-4 (documents de référence à sauver), Q4-5 (points de contrôle vs pertes de temps), Q4-6 (vocabulaire commun de solidité des faits ou non), Q4-7 (mémoire des sponsors ou non).

Compléments possibles à cette analyse, non bloquants : lecture des fichiers du remarketing (S-12), lecture de `AGENTS.md` du site (régime A), instructions de GPT-24, conversation « IA Chef ».
