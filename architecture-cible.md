# Architecture cible OpenClaw (phase 5)

Dernière mise à jour : 2026-11-04
Statut : version 1.3 (clôture de séance rendue automatique : au fil de l'eau, par inactivité, filet du soir), **validée par Dominic le 2026-11-04 (D-015)** à l'exception des cinq questions de la section 10, encore ouvertes. Section 7 corrigée (développement avec le CEO, production en place) et scénario de clôture / reprise ajouté à sa demande.

Base : décisions D-006 à D-014, principes P-1 à P-31, `recouvrements.md`, `openclaw-capacites.md`. Règle de conception : s'adapter à la façon de travailler de Dominic et à sa vision (D-012, D-013), pas transposer ses projets actuels un à un.

## 1. En une page

Un **serveur virtuel Linux** fait tourner OpenClaw en permanence. Dessus, **un seul agent au départ : le CEO**. Dominic lui parle depuis son PC (interface web) et son téléphone (Telegram). Le CEO a pour mémoire un **dépôt Git** qui contient l'organisation de l'entreprise, une fiche par projet (les 20 du registre importées), une fiche par département, les connaissances communes en un seul exemplaire, et son journal. Il fait le **rituel du lundi**, répond à « on en est où », rappelle les échéances et les projets dormants, et fait avancer le projet du jour. Quand une tâche le demande, il délègue à un **sous-agent** équipé des règles et compétences du département concerné, puis rend compte. Il ne déclenche rien vers l'extérieur sans GO (P-20) et aucune automatisation n'est ajoutée à l'installation (D-010).

L'entreprise est écrite dès le départ comme un **organigramme** (CEO, chefs d'équipe par département, agents spécialisés, D-013), mais seul le CEO tourne au jour 1; les équipes s'activent une à la fois (P-31). Le format des fichiers est celui qu'attend Paperclip, pour y brancher l'organisation plus tard (D-014).

Règle de partage (corrigée le 2026-11-04 sur remarque de Dominic) : **tout ce qui est en développement se travaille avec le CEO dès le jour 1**, parce que c'est là que l'état compte; **les machines de production** (montage, rédaction Patreon, extraction, remarketing, studio anonyme, tâches planifiées) restent où elles sont et ne remontent au CEO que leurs résultats, puis migrent une équipe à la fois (P-24). Aucun projet en développement ne se travaille hors du CEO, sauf si son état vit dans un fichier que le CEO lit.

## 2. Vue d'ensemble

```
Dominic ── PC (interface web OpenClaw) ──┐
        └─ téléphone (Telegram) ─────────┤
                                         ▼
           ┌──────────── Serveur virtuel Linux (Gateway OpenClaw) ────────────┐
           │  Agent CEO                                                       │
           │   workspace = dépôt Git « siege » (organisation, projets,        │
           │   départements, commun, mémoire, journal)                        │
           │   sous-agents à la demande (brief = AGENTS.md du département)    │
           │   cron : préparation du lundi (rappel, pas d'action)             │
           │   skills : gmail (lecture), github, recherche web, validateur    │
           └──────────────────────────────────────────────────────────────────┘
                 │ lecture             │ lecture / écriture         │ plus tard
                 ▼                     ▼                            ▼
           Gmail (IMAP, lecture)   GitHub (siege, studio-explainer,   PC Windows
           Resend (lecture)        netblb-site, commerce, OpenClaw)   comme « node »
                                                                      (DaVinci, fichiers)

Restent tels quels (état connu du CEO) : Claude Desktop + DaVinci (CL-01, CL-02);
Maître studio-explainer (Claude) ; projets ChatGPT (clients, Patreon, extraction,
Drop shiping) ; tâches planifiées ChatGPT ; Metricool ; Thinkific ; Higgsfield.
```

## 3. L'organigramme (écrit dès le jour 1, activé une équipe à la fois)

| Niveau | Rôle | Périmètre | Ce qu'il reprend de l'existant | Activation |
|---|---|---|---|---|
| Propriétaire | Dominic | Idées, décisions, tournage, mise en ligne, contrôle qualité | | Jour 1 |
| CEO | « Direction » | Fil conducteur de tout; registre des projets; rituel du lundi; rappels; réception des revues de veille; délégation; journal des décisions | Registre ART-02; les cinq chefs d'orchestre (gouvernance C-10 écrite une fois); tableau de bord Black Friday; « IA Chef » du site | Jour 1 |
| Chef d'équipe | LeFiltre · Éditorial | Recherche, fact-checking, vidéos éditoriales, revalorisation, articles Patreon, édition | CL-06, CL-08, CL-09, CL-12, GPT-22, GPT-17, GPT-07, GPT-16 | Équipe 1 (première à activer : la plus riche en règles déjà écrites, doctrine C-7 à C-9) |
| Chef d'équipe | LeFiltre · Partenariats | Négociation, programme hardware, analyse de contrat, media kit, mémoire des sponsors (P-17) | CL-03, CL-07, CL-11, CL-18, ART-03 | Équipe 2 (doctrine sponsor C-4 à C-6 en un exemplaire; corrige les divergences) |
| Chef d'équipe | LeFiltre · Communauté et ventes | Structure Patreon, Coffre-fort (extraction), outils IA pour membres, formations et remarketing, campagnes | GPT-06 à 06d, GPT-20, GPTC-07, GPTC-08, CL-05, GPT-23, structure 6e | Équipe 3 (CL-05 garde son connecteur Resend tant qu'il n'est pas migré) |
| Chef d'équipe | LeFiltre · Production vidéo | Montage, Shorts, métadonnées de publication | CL-01, CL-02, GPTC-02, GPTC-03, GPTC-04, GPTC-05 | **Reste dans Claude Desktop** (P-21); l'équipe n'existe d'abord que comme fiche d'état et dossier de leçons |
| Chef d'équipe | Net BLB · Services | Un dossier par client (Gmail en lecture, P-29), mandats, procédures, propositions | GPT-01, 02, 05, 08, 10, 11, 12, 13, 14; OUT-09, OUT-10 | Équipe 4 (premier bénéfice concret de Gmail connecté) |
| Chef d'équipe | Net BLB · Site et marketing | Site netblb.com, SEO/GEO, questionnaire PM, LinkedIn | GH-03, AUTO-01, OUT-05, PRJ-08, PRJ-18 | Plus tard; AUTO-01 reste une tâche ChatGPT dont le CEO reçoit le rapport |
| Chef d'équipe | Studio anonyme | Chaîne anglophone | CL-13, GH-01, CL-15, CL-16 | **Reste dans Claude** (Maître + sous-agents, méthode à préserver); fiche d'état seulement; identité publique séparée (P-3) |
| Chef d'équipe | Commerce autonome | Dropshipping | GPT-24 (passation), GH-04 | **Repris par le CEO dès le jour 1** : la passation de GPT-24 a été écrite pour un relais, elle est remise au CEO; le dépôt GH-04 devient le dossier du projet, lu et écrit par le CEO (ou un sous-agent « chef du commerce » avec le brief de la passation). Premier projet test du critère du jour 1 |
| Chef d'équipe | Veille | Patch Tuesday, actualité, revues SEO | AUTO-01, AUTO-04, GPT-16 | Les tâches ChatGPT restent (production); le CEO en reçoit les sorties (collées par Dominic au début); migration en cron OpenClaw = automatisation, donc après usage (D-010) |
| Transversal | Validateur | Relit un livrable avec les règles seulement | CL-13 validateur, validation de l'audit | Jour 1, comme compétence (skill) disponible au CEO |

Lecture : dix départements sur le papier, **un agent qui tourne**. Chaque département est d'abord un dossier (fiche d'état, règles, connaissances); il devient une équipe active quand ses règles, son validateur et son critère d'acceptation sont écrits et que Dominic le décide (P-31).

## 4. Le dépôt « siege » : la mémoire du CEO

Un nouveau dépôt GitHub privé (nom proposé : `NetBLB/siege`; Dominic choisit) est le workspace de l'agent CEO. C'est la méthode déjà éprouvée sur studio-explainer (D-008), appliquée à la direction.

```
siege/
  AGENTS.md            règles d'exploitation du CEO : gouvernance (P-8), rituel du lundi,
                       quand déléguer, quoi écrire en mémoire, frontière d'autonomie (P-20)
  SOUL.md              posture : associé stratégique, calme, direct, challenge les idées,
                       propose une « prochaine action » unique
  USER.md              Dominic : identité, préférences (français, jamais de tiret cadratin,
                       structure claire, pas de blabla), horaires, canaux
  MEMORY.md            faits durables et décisions transversales (curaté)
  memory/AAAA-MM-JJ.md journal quotidien automatique d'OpenClaw
  organisation.md      l'organigramme de la section 3 (rôles, périmètres, lignes
                       hiérarchiques, statut actif / dormant) ; source de la page visuelle
                       et du futur import Paperclip
  projets/             une fiche par projet (20 importées de ART-02 + les projets
                       de production courants) : statut, priorité, avancement, prochaine
                       action, échéance, journal, liens vers les IA et dépôts concernés
  departements/<nom>/  AGENTS.md (brief du chef d'équipe et des sous-agents), regles.md,
                       connaissances propres, criteres-acceptation.md, lecons/
  commun/              un seul exemplaire des noyaux partagés (P-10) :
                       identite.md, marque-audience.md (chiffres datés, source Media Kit),
                       offre-sponsor.md, patreon.md, doctrine-editoriale.md (avec le
                       vocabulaire unique de solidité, P-12), gouvernance.md, autonomie.md
  skills/              compétences du CEO : validateur, gmail-lecture, github, recherche
  journal/decisions.md décisions validées par Dominic (même format que l'audit)
  PASSATION.md         réécrit à chaque séance importante (P-27)
```

Règles : tout fichier de `commun/` et `departements/` porte une version et une ligne d'historique; le CEO peut commettre ses propres changements (gabarit officiel AGENTS.md) mais un changement de règle est relu par Dominic avant adoption (changelog, D-008). Aucun secret dans le dépôt (P-23). Les mémoires automatiques des plateformes ne sont jamais citées comme source (P-15).

Import initial (phase 6) : les 20 fiches ART-02; les instructions des projets Claude et ChatGPT lues en phase 2, **réécrites** en règles de département (pas copiées); les documents enfermés dans des conversations (document maître Patreon, V1.2, IA Chef, Media Kit, tableau Black Friday) rapatriés (P-13).

## 5. Composants OpenClaw, un par un

| Composant | Choix proposé | Pourquoi | Alternatives écartées |
|---|---|---|---|
| Hébergement | Serveur virtuel Linux (Ubuntu), 4 Go de RAM, 2 vCPU, 40 Go; OpenClaw installé comme service (`--install-daemon`); accès à l'interface web par tunnel privé (Tailscale ou SSH), jamais exposée publiquement | D-012 (serveur), proactivité 24 h, PC éteint sans conséquence (P-28); recommandations de sécurité de la documentation | PC Windows de Dominic (WSL2 possible mais éteint, et mélange avec le montage); hébergeur « OpenClaw géré » (moins de contrôle; à reconsidérer si l'entretien pèse) |
| Agent | Un agent `ceo`, workspace = clone de `siege` | D-012 (un interlocuteur), D-014 | Plusieurs agents dès le départ (repli prévu si l'usage l'exige, P-5) |
| Modèle | Anthropic par clé API; modèle de classe supérieure pour le CEO (jugement, synthèse), classe intermédiaire pour les sous-agents de routine; plafond mensuel fixé dans la console Anthropic et suivi dans l'interface OpenClaw | D-014 (fournisseur principal), politique Anthropic d'avril 2026 (abonnement non utilisable par OpenClaw), P-26 | Abonnement ChatGPT via Codex (gardé comme option secondaire pour des tâches de routine; à tester); modèle local (après mesure) |
| Canaux | Interface web (PC) + bot Telegram (téléphone); WhatsApp en option plus tard | Telegram = le canal le plus simple et le plus stable sur serveur selon toutes les sources; D-012 (PC et téléphone) | WhatsApp d'abord (lien par QR, plus fragile); Discord / Slack (pas dans les habitudes de Dominic) |
| Mémoire | Fichiers du dépôt (section 4) + mémoire native d'OpenClaw (MEMORY.md, journal quotidien, recherche) ; « Dreaming » laissé actif mais relu : ce qu'il promeut dans MEMORY.md est vérifié par Dominic au rituel du lundi | P-13, P-15; le modèle « tout est fichier » est celui de Dominic (GitHub source de vérité) | Mémoire uniquement dans OpenClaw sans dépôt (perd le versionnage et la relecture) |
| Délégation | Sous-agents (`sessions_spawn`) avec le brief `departements/<nom>/AGENTS.md`; profondeur 1 au départ (CEO → spécialiste); profondeur 2 (CEO → chef d'équipe → spécialiste) quand une équipe a plus d'un rôle actif | Patron validé par Dominic sur studio-explainer; P-31 | Agents permanents par département (coût, coordination; repli P-5) |
| Compétences | Skills du workspace : `validateur` (lit seulement livrable + règles, n'invente jamais de consigne), `gmail-lecture` (IMAP avec mot de passe d'application, lecture seule), `github`, `recherche-web`; plus tard un skill par département | P-22, P-29, P-20 (lecture seule d'abord) | Intégration Gmail complète par OAuth (écriture, étiquettes) : plus tard, si besoin |
| Proactivité | Deux crons au départ : lundi 7 h, préparation du bilan de la semaine (échéances, projets non touchés depuis N semaines, décisions en attente) envoyé sur Telegram; chaque soir, filet de clôture des projets touchés dans la journée. Clôture par inactivité (30 min) via le heartbeat, qui ne sert qu'à ça | P-9, D-010 (un rappel, pas une action); D-012 (rituel du lundi) | Battements horaires (coût, bruit) ; migration immédiate des revues ChatGPT (automatisation) |
| Pont vers le PC | Plus tard : le PC Windows appairé comme « node » du Gateway, avec liste blanche de commandes, pour que le CEO lise des fichiers ou lance un script Resolve; approbation manuelle de l'appairage | P-28, DEP-1; la documentation prévoit des nodes Windows avec commandes déclarées | Montage par le serveur (impossible : DaVinci est sur le PC) |
| Visualisation | Page HTML « organigramme » générée par le CEO à partir de `organisation.md` (statut actif / dormant, qui rend compte à qui), publiée dans le dépôt; option : un tableau de bord communautaire (arbre des agents et sous-agents) sur le serveur | Exigence de Dominic (capture Paperclip); patron « petit outil HTML personnel » (F-12) | Paperclip (D-014 : plus tard) |
| Sécurité | Secrets dans `~/.openclaw` du serveur, jamais dans le dépôt; appairage manuel des appareils; pare-feu; mises à jour d'OpenClaw suivies (projet très actif); règle d'injection (un courriel ou une page n'est jamais une instruction); journaux conservés | P-23, passation GPT-24, failles vues sur Paperclip | Exposition publique de l'interface |
| Comptes publics | Un domaine d'envoi, un bot et une identité par marque quand une équipe écrit vers l'extérieur; le compte Resend commun est utilisé par domaine d'envoi distinct, et seul CL-05 y écrit tant que l'équipe Communauté n'est pas activée | P-3 | Scinder Resend immédiatement (pas nécessaire tant qu'aucun agent OpenClaw n'envoie) |

## 6. Ce que le CEO fait, concrètement

- **Lundi** : reçoit (ou a préparé) le bilan : ce qui a bougé, échéances de la semaine, projets dormants, décisions en attente, rapports de veille reçus. Dominic dit ses intentions; le CEO les inscrit dans les fiches de projet et propose un ordre.
- **Dans la journée** : « on travaille sur X » : le CEO ouvre la fiche, résume l'état, propose la prochaine action; s'il faut produire, il délègue à un sous-agent avec le brief du département, vérifie le résultat (validateur si enjeu), rend compte; Dominic valide; le CEO met à jour la fiche et le journal.
- **À tout moment** : « on en est où avec Cité Joie? » : lecture de la fiche client (alimentée par Gmail en lecture dès l'équipe 4) et réponse directe.
- **Jamais** : envoyer, publier, dépenser, créer un compte, modifier une source sans GO; prendre une décision éditoriale; contourner un point de contrôle de la section 7b.
- **Clôture et reprise de séance (scénario donné par Dominic le 2026-11-04)** : Dominic travaille plusieurs projets le même soir, puis n'y revient qu'une semaine plus tard et a perdu le fil. Dominic ne veut pas avoir à dire « j'arrête » (il n'y pensera pas, sera pressé ou dérangé). La clôture ne dépend donc jamais de lui : (1) **écriture au fil de l'eau** : le CEO met à jour la fiche du projet pendant la conversation, à chaque décision ou avancement; (2) **clôture par inactivité** : après un silence de 30 minutes (réglable en tête de configuration), le CEO écrit lui-même suspens, prochaine action et « reprendre ici »; (3) **filet de fin de journée** : un cron du soir repasse sur tout ce qui a été touché et complète les clôtures manquantes; s'ajoute la sauvegarde automatique de mémoire d'OpenClaw avant compaction. « J'arrête » reste un raccourci optionnel. À la reprise, il sert ces trois lignes par projet avant toute autre chose. C'est la passation, faite par le CEO à chaque fois, sans effort de Dominic. **Test d'acceptation n° 1 du prototype** (P-30) : trois projets travaillés un soir, retour une semaine plus tard, « où en étais-je? », réponse juste pour les trois.
- **Passation** : à chaque séance importante, réécrit `PASSATION.md`; si une conversation sature ou si OpenClaw est réinstallé, rien n'est perdu.

## 7. Développement avec le CEO, production en place

**Travaillé avec le CEO dès le jour 1 (état dans `projets/` ou dans le dépôt du projet)** : le commerce autonome (passation GPT-24 remise au CEO, dépôt GH-04); les 11 idées et 4 conceptions du registre (application senior, analyse.lefiltre.com, questionnaire PM, formation IA pour chefs d'entreprise, canaux Shorts personnels, chroniqueur, guide Windows 11, etc.); le présent projet OpenClaw lui-même après la phase 6; toute nouvelle idée. Les ateliers d'idéation ChatGPT correspondants (GPT-09, « IA » de PRJ-11, PRJ-13…) sont lus une fois, résumés dans la fiche, puis cessent d'être le lieu de travail.

**Cas particulier** : la campagne Black Friday (GPT-23, 45 %, fin le 30 novembre) : terminée dans ChatGPT pour ne pas la déplacer en pleine course; « Où en sommes-nous » collé au CEO chaque lundi; bilan et modèle reproductible importés le 1er décembre. Basculée tout de suite si Dominic le préfère.

**Machines de production qui restent en place (et pourquoi)** : elles produisent des résultats, pas de l'état de projet; le CEO n'a besoin que du résultat.

| Élément | Reste | Raison | Le CEO en sait quoi |
|---|---|---|---|
| Montage DaVinci (CL-01, CL-02) | Claude Desktop sur le PC | Accès machine, API Resolve, forfait Claude valable; méthode à préserver | Fiche « Production vidéo » : vidéos en cours, leçons; Dominic lui dit quand une vidéo est sortie |
| Studio anonyme (CL-13) | Claude + GH-01 | Système le plus abouti, en test; identité séparée | Fiche d'état, lue depuis `PASSATION.md` du dépôt studio-explainer |
| Projets ChatGPT clients | ChatGPT (traitement des mandats courants) | Dossiers de mandats = production de services; migration vers l'équipe Services quand elle s'active | Fiches clients construites depuis Gmail (lecture) dès l'équipe 4; d'ici là, état inconnu du CEO sauf rapport de Dominic |
| Machines Patreon, extraction, rédaction | ChatGPT | Fonctionnent; migrer = réécrire des règles, une équipe à la fois | Fiche « Communauté » |
| Tâches planifiées ChatGPT | ChatGPT | Automatisations existantes conservées (D-010) | Dominic colle les rapports; plus tard, cron OpenClaw |
| Remarketing (CL-05) | Claude + Resend | Seul agent qui écrit vers l'extérieur; isolé | Fiche « Communauté » |
| Horaire éphémère (ART-01), registre (ART-02) | ART-01 reste; ART-02 est remplacé par `projets/` | D-012 | Le registre devient sa mémoire |

## 8. Coûts (estimation, H)

- Serveur virtuel : 5 à 15 $ par mois.
- API Anthropic : dépend de l'usage; un CEO consulté chaque jour avec quelques délégations par semaine se situe vraisemblablement entre 20 et 80 $ par mois au départ; un plafond est fixé avant la première conversation et l'usage réel est relu au bout d'un mois. Les forfaits « usage supplémentaire » ou les crédits API inclus dans certains forfaits Claude sont à vérifier pour le forfait de Dominic.
- Abonnements existants : inchangés (Claude pour le montage, ChatGPT pour ce qui y reste).
- Paperclip, IA locale : zéro tant qu'ils ne sont pas introduits.

## 9. Risques et parades

| Risque | Parade |
|---|---|
| Un projet en développement est travaillé ailleurs et son état échappe au CEO (remarque de Dominic) | Règle de la section 7 : développement = avec le CEO; production = en place; tout projet travaillé ailleurs doit avoir un état en fichier lisible par le CEO |
| Le CEO mélange les projets (échec du critère du jour 1, P-30) | Une fiche par projet, un dossier par département, nommage strict; test d'acceptation en phase 6 avant tout le reste |
| Dérive de la mémoire automatique (Dreaming promeut des erreurs) | Relecture hebdomadaire de MEMORY.md; les fichiers du dépôt priment (P-15) |
| Coût qui dérape | Plafond dans la console; heartbeat réduit; sous-agents sur modèle intermédiaire |
| Dominic retourne vers ChatGPT parce que c'est plus simple | Prévu (D-012) : rien n'est cassé; le CEO ne fait que gagner de la mémoire; mesurer après un mois |
| Faille ou mise à jour cassante d'OpenClaw | Interface non exposée; mises à jour suivies; dépôt = sauvegarde complète du workspace |
| Instructions réécrites perdent des nuances des projets actuels | Les instructions d'origine sont archivées dans `departements/<nom>/archive/` avant réécriture |
| Point « multi-agent routing » non vérifié dans la documentation officielle | Sans incidence au jour 1 (un agent); à vérifier avant d'activer un deuxième agent permanent |

## 10. Questions à Dominic avant la phase 6 (courtes)

1. Nom du dépôt du CEO : `NetBLB/siege` te convient, ou un autre nom?
2. Téléphone : Telegram d'abord, WhatsApp plus tard, ou l'inverse?
3. Serveur : as-tu déjà un hébergeur (celui de Plesk?), ou je propose Hetzner / DigitalOcean dans le cahier de construction?
4. Première équipe à activer après le CEO : Éditorial (ma proposition) ou Services Net BLB (bénéfice Gmail immédiat)?
5. Un seul domaine d'envoi doit-il être réservé au CEO pour ses rappels (ou Telegram suffit)?

## 11. Ce que la phase 6 produira

Un plan de transition en étapes (installation, test d'acceptation du CEO, import des fiches, activation de l'équipe 1, usage sans automatisation, bilan à un mois, activation suivante) et le cahier de construction remis à une IA technique.
