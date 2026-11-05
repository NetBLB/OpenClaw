# OpenClaw : capacités réelles (lecture de la documentation)

Dernière mise à jour : 2026-11-04
Statut : lecture partielle (pages « Agent runtime », « Agent workspace », « Memory », « Skills », « Tools » et articles tiers); page « Multi-agent routing » non encore lue. À compléter avant la phase 5. Faits seulement; aucune décision d'architecture ici.

Objet : savoir contre quoi on conçoit. Dominic ne connaît pas OpenClaw (déclaré le 2026-11-04); l'audit non plus avant cette lecture. Ce document évite de concevoir contre des fonctions supposées.

## Ce qu'est OpenClaw

- Un service persistant (« Gateway ») qui fait tourner un ou plusieurs agents IA, route les messages entre canaux de discussion et agents, et maintient l'état des sessions. Il tourne sur une machine (PC local, serveur, cloud) et doit rester allumé pour être proactif.
- Chaque **agent** a un **workspace** : un dossier de fichiers Markdown qui est à la fois ses instructions, sa mémoire et ses compétences. « Tout est fichier » : lisible, modifiable, versionnable dans Git (la documentation recommande explicitement de faire du workspace un dépôt Git).

## Fichiers d'un agent (workspace)

| Fichier | Rôle |
|---|---|
| `AGENTS.md` | Instructions d'exploitation : règles, priorités, façon d'utiliser la mémoire |
| `SOUL.md` | Persona, limites, ton |
| `IDENTITY.md` | Nom, style |
| `USER.md` | Profil et préférences stables de l'utilisateur, sous forme de directives |
| `MEMORY.md` | Mémoire long terme curatée (faits durables, décisions); chargée au début de chaque session principale |
| `memory/AAAA-MM-JJ.md` | Notes quotidiennes; celles d'aujourd'hui et d'hier chargées automatiquement; le reste indexé pour recherche |
| `skills/` | Compétences propres au workspace (priorité la plus haute) |

Ces fichiers sont injectés dans le contexte au premier tour d'une session. Les gros fichiers sont tronqués. La configuration (`~/.openclaw/openclaw.json`), les identifiants et les transcriptions de sessions vivent **hors** du workspace et ne doivent pas être versionnés.

## Mémoire

- Trois couches : `USER.md` (préférences), `MEMORY.md` (faits et décisions durables), `memory/` (journal quotidien). Outils de l'agent : lecture d'un fichier mémoire, recherche dans la mémoire (embeddings, OpenAI par défaut).
- Avant qu'une conversation soit compactée, OpenClaw force un tour silencieux pour sauver le contexte important dans la mémoire (« flush »).
- « Dreaming » : consolidation en arrière-plan qui promeut les éléments qualifiés des notes quotidiennes vers `MEMORY.md`; journal relisible dans `DREAMS.md`.
- Imports possibles depuis Claude Code et Codex (dossiers `memory/imports/`).
- Principe énoncé par la documentation : le modèle ne se souvient que de ce qui est écrit sur disque; pas d'état caché.

## Compétences (Skills)

- Un Skill = un dossier avec `SKILL.md` (mode d'emploi réutilisable : workflow, grille, séquence de commandes, contrainte). Même format que les Skills de Claude.
- Emplacements par ordre de priorité : skills du workspace; skills d'agent de projet; skills personnels (`~/.agents/skills`); skills gérés (`~/.openclaw/skills`); skills fournis; dossiers supplémentaires.
- **Partage entre agents** : `agents.defaults.skills` définit une liste de base héritée par tous les agents; un agent peut la remplacer ou n'avoir aucun skill. Installation possible pour un agent ou pour tous (`--global`).
- Distinction officielle : **outils** = actions appelables; **skills** = façon de travailler; **plugins** = capacités ajoutées (outils, fournisseurs de modèles, canaux, hooks).

## Plusieurs agents

- **Multi-agent routing** : plusieurs agents isolés sur un même Gateway, chacun avec son workspace, sa persona, son modèle, son historique de sessions; un canal ou un compte de messagerie peut être lié (« binding ») à un agent donné.
- **Sous-agents** : un agent peut lancer des sessions enfants en arrière-plan (`sessions_spawn`), non bloquantes, qui rendent un résultat structuré; profondeur de délégation configurable (1 par défaut; 2 nécessaire pour un patron orchestrateur); limite globale de parallélisme (8 par défaut).
- Outils de coordination : liste des agents, état de session, envoi d'un message à une autre session, objectifs (« goals »).
- Limite signalée par la communauté (issue GitHub ouverte) : les tâches planifiées (cron) sont au niveau du Gateway, pas scopées par agent; les « heartbeats » le sont.

## Proactivité

- **Cron** : tâches planifiées (syntaxe cron) qui déclenchent un tour d'agent ou lancent un sous-agent; résultats envoyés vers les canaux configurés.
- **Heartbeat** : réveil périodique de l'agent avec un prompt; par défaut sans maintenance de mémoire.
- Le gabarit officiel `AGENTS.md` autorise par défaut du « travail proactif sans demander » : organiser la mémoire, vérifier l'état des projets (git status), mettre à jour la documentation, commettre et pousser ses propres changements.

## Canaux et accès

- Canaux listés par la documentation : WhatsApp, Telegram, Discord, Slack, interface web, CLI, et d'autres; un même agent peut être joignable par plusieurs canaux.
- Modèles : plusieurs fournisseurs; routage de modèle par agent ou par tâche. OpenClaw peut aussi exécuter Claude Code ou Codex comme outils, en profitant de l'authentification de leurs abonnements (fait rapporté par une source tierce; à vérifier dans la documentation officielle).
- Outils natifs : fichiers (lecture, écriture, édition), exécution de commandes, navigateur (Chrome DevTools Protocol), MCP (ressources locales et distantes exposées comme capacités).
- Sandbox optionnel : sans sandbox, le workspace est le répertoire de travail par défaut mais pas une prison; les chemins absolus atteignent le reste de la machine.

## Points à vérifier avant la phase 5

- Page « Multi-agent routing » : isolation réelle (sessions, mémoire, identifiants) et partage possible entre agents (dossiers communs, mémoire commune).
- Authentification par abonnement Claude / ChatGPT vs clés API : ce qui est officiellement pris en charge et ce que ça coûte.
- Fonctionnement sur Windows (le PC de Dominic) vs Linux / macOS; prérequis pour un service toujours allumé.
- Accès à DaVinci Resolve et à Claude Desktop depuis un agent OpenClaw (MCP local, exécution de scripts).
- Portée exacte du cron en multi-agents (issue ouverte).

## Visualisation de l'organigramme et couche « entreprise » (ajout du 2026-11-04)

Question de Dominic : existe-t-il, comme dans Paperclip, une vue visuelle des agents et sous-agents?

- **OpenClaw natif** : aucune vue d'organigramme intégrée trouvée; l'interface officielle sert à discuter et configurer. La hiérarchie vit dans la configuration et les fichiers.
- **Tableaux de bord communautaires** (projets ouverts sur GitHub, maturité variable) : `mudrii/openclaw-dashboard` (arbre de hiérarchie parent → sous-agent → sous-sous-agent, activité des sous-agents, cron); `ChristianAlmurr/openclaw-dashboard` (« Mission Control » : vues Org Map, Grid, Feed, Constellation; Kanban; chat par agent); `wickedapp/openclaw-office` et `WW-AI-Lab/openclaw-office` (bureau virtuel animé, chaînes de délégation, console agents / canaux / skills / cron); `frexida/agentflow` (concepteur d'organigramme en glisser-déposer qui exporte une configuration OpenClaw, équipes et sous-équipes).
- **Paperclip** (`paperclipai/paperclip`, lancé en mars 2026, MIT, auto-hébergé, Node.js + React) : couche « entreprise » au-dessus des agents. Slogan : « Si OpenClaw est un employé, Paperclip est l'entreprise. » Fournit organigramme (rôles, lignes hiérarchiques, humains et agents), tickets, délégation montante et descendante, budgets par agent, approbations, journal d'audit, heartbeats (les agents se réveillent, traitent leurs tickets, rendent compte vers le haut), suivi des coûts. Adaptateur dédié `openclaw_gateway` (protocole WebSocket du Gateway OpenClaw) et adaptateur HTTP pour des instances distantes; accepte aussi Claude Code, Codex, Cursor, scripts. Le CEO Paperclip est le sommet de l'organigramme et recommande des embauches que l'humain approuve.
- **Capture fournie par Dominic (2026-11-04, interface Paperclip, espace « NetworkChu… », vraisemblablement la chaîne tech NetworkChuck)** : vue « Org Chart » avec un CEO (agent sur Claude Code), un CTO et deux ingénieurs (agents sur « Hermes Gateway », un autre système d'agents), chaque carte avec rôle, moteur et description de poste; boutons d'import / export d'organisation; menu : Tasks, Routines, Artifacts, Skills, Projects, Agents, Org, Timeline, Costs, Activity, Settings. Observations : (1) Paperclip mélange des agents de moteurs différents dans un même organigramme; (2) le menu couvre ce que D-012 demande (registre de projets, routines planifiées, coûts, journal d'activité); (3) Dominic a désigné cette vue comme « exactement ce que je veux dire » : la visualisation de l'organigramme est une exigence de la phase 5, pas un agrément.
- **Lecture (R, non décidée)** : la vision D-013 de Dominic (lui au sommet, un CEO, des chefs d'équipe, des employés) correspond au modèle de Paperclip, OpenClaw jouant l'employé. Deux options pour la phase 5 : (a) OpenClaw seul, avec un tableau de bord communautaire pour la visualisation; (b) OpenClaw comme main-d'œuvre sous Paperclip comme direction. Réserves : Paperclip est jeune et moins éprouvé; couche supplémentaire à installer, maintenir et sécuriser; à vérifier : maturité réelle, coût, sécurité de l'adaptateur, compatibilité avec D-010 (installer d'abord).

Sources : github.com/mudrii/openclaw-dashboard; github.com/ChristianAlmurr/openclaw-dashboard; github.com/wickedapp/openclaw-office; github.com/WW-AI-Lab/openclaw-office; frexida.github.io/agentflow; github.com/paperclipai/paperclip; paperclip.ing; articles tiers (towardsai, allclaw, theaienterprise, flowtivity).

## Compléments vérifiés le 2026-11-04 (soir) et recommandation sur Q-061

**Paperclip, faits complémentaires (O, sources ci-dessous)** : modèle centré sur les tickets (« issues ») et la boîte de réception d'approbation; seul le CEO a un battement (« heartbeat », 3 600 s par défaut) qui le réveille pour lire et déléguer les tickets; le « chat » avec un agent est qualifié d'expérimental par le projet lui-même (fonction demandée par la communauté depuis mars 2026, plugin tiers existant, « CEO Chat » à la feuille de route); lancé en mars 2026; deux failles critiques (exécution de commandes via import d'agent malveillant, CVSS 9,6) corrigées en août 2026; témoignages de septembre 2026 sur des boucles de récupération, des agents incapables de terminer la coordination, des lenteurs à l'échelle; flux continu de tickets ouverts (plusieurs par jour au 3 novembre 2026); chaque agent est une instance complète (Claude Code, Codex, OpenClaw…) qui consomme à chaque réveil; s'arrête quand la machine hôte s'éteint (serveur recommandé). Verdict des revues : approchable et visuellement convaincant, « pas encore prêt pour une entreprise sans humain ».

**Politique Anthropic (O)** : depuis le 4 avril 2026, les abonnements Claude Pro / Max ne couvrent plus l'usage par des outils tiers (OpenClaw inclus); options : clé API (paiement à l'usage, voie recommandée par la documentation d'OpenClaw) ou forfaits « usage supplémentaire »; les forfaits Max reçoivent depuis octobre 2026 des crédits API mensuels (à vérifier pour le forfait de Dominic). Le forfait Claude reste valable pour Claude Desktop et Claude Code (donc pour le montage DaVinci). Côté OpenAI, OpenClaw prend en charge l'authentification par abonnement ChatGPT via Codex.

**Recommandation de l'audit pour Q-061 (R, 2026-11-04, en attente de validation)** : *OpenClaw d'abord, conçu pour Paperclip, Paperclip plus tard.*
1. Installation : un agent CEO unique dans OpenClaw sur le serveur virtuel, joignable par le web et par un canal mobile; mémoire = registre des projets et des départements en fichiers versionnés; rituel du lundi et réception des revues planifiées; délégation par sous-agents avec compétences par département; aucune automatisation ajoutée (D-010). Répond au critère du jour 1 (D-012, P-30).
2. Raisons : exigence conversationnelle (force native d'OpenClaw, faiblesse actuelle de Paperclip); D-010 contre un outil construit autour de battements autonomes; patron déjà éprouvé par Dominic (Maître + sous-agents de studio-explainer); coût d'un seul agent.
3. Conçu comme une entreprise dès le départ : fichier d'organisation (CEO, chefs d'équipe, employés, lignes hiérarchiques), un `AGENTS.md` par équipe avec règles, validateur et critère d'acceptation : le format que Paperclip attend. Visualisation : tableau de bord communautaire (arbre des agents) ou page HTML générée par le CEO à partir du fichier d'organisation.
4. Bascule vers Paperclip quand : au moins deux équipes produisent un travail jugé bon par Dominic; besoin réel de tickets, budgets par agent et file d'approbation; Paperclip stabilisé (quelques mois sans faille ni boucle). Branchement par l'adaptateur `openclaw_gateway`, import de l'organigramme.
5. Perdu : vue d'organigramme officielle et budgets par agent dès le jour 1. Gagné : CEO conversationnel dès la semaine 1, coût maîtrisé, pas de dépendance à un projet instable, migration vers Paperclip sans refonte.
6. Fournisseur : Anthropic par clé API avec plafond mensuel pour le CEO et les équipes; forfait Claude conservé pour le montage dans Claude Desktop; abonnement ChatGPT éventuellement réutilisable dans OpenClaw via Codex pour des tâches secondaires (à vérifier); IA locale plus tard, après mesure. « Un fournisseur principal » plutôt que « un seul fournisseur strict ».

Sources complémentaires : github.com/paperclipai/paperclip (README, issues, discussions 1729 et 13396, issues 49, 2127, 2553, 5947); docs.paperclip.ing/guides/org/agents; paperclip.community « Chat with CEO »; thehackernews.com (2026-08-05); vibecoding.app, theaiarchitects.com, dev.to (revues); docs.openclaw.ai/concepts/oauth; shippingskool.com, kersai.com, openclaw.report, alternativeto.net (politique Anthropic, avril 2026).

Sources : docs.openclaw.ai (concepts/agent, concepts/agent-workspace, concepts/memory, tools/skills, tools, reference/templates/AGENTS); articles tiers (dev.to, hackernoon, betterclaw.io, openclawmcp.com, learnopenclaw.com); issue GitHub openclaw/openclaw n° 16053.
