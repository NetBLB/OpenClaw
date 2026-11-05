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

Sources : docs.openclaw.ai (concepts/agent, concepts/agent-workspace, concepts/memory, tools/skills, tools, reference/templates/AGENTS); articles tiers (dev.to, hackernoon, betterclaw.io, openclawmcp.com, learnopenclaw.com); issue GitHub openclaw/openclaw n° 16053.
