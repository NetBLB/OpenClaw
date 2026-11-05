# Base de référence officielle sur OpenClaw

Date de consultation : 2026-11-05. Sources : uniquement docs.openclaw.ai (versions `.md` des pages), github.com/openclaw/openclaw (README, page des versions, recherche d'issues par API REST). Aucune source tierce n'a été utilisée; si un point vient d'ailleurs, il est marqué « tiers ». Ce document rapporte des faits; il ne contient aucune recommandation d'architecture.

Convention : « doc » = documentation officielle. Les clés de configuration sont celles de `~/.openclaw/openclaw.json` sauf mention contraire.

---

## 1. Fiche d'identité

Source : https://github.com/openclaw/openclaw ; https://docs.openclaw.ai/start/why-openclaw.md ; https://docs.openclaw.ai/releases.md ; https://github.com/openclaw/openclaw/releases

- Nom : OpenClaw. Slogan du README : « The AI that really does things. Any OS. Any Platform. The lobster way. »
- Nature : assistant/agent IA open source auto-hébergé, qui se branche sur les messageries existantes (Telegram, WhatsApp, Discord, Slack, Signal, iMessage, etc.), exécute des outils, planifie des tâches, et fonctionne avec de nombreux fournisseurs de modèles.
- Auteur et gouvernance : créé par Peter Steinberger et la communauté; piloté par l'OpenClaw Foundation, organisme indépendant 501(c)(3) financé par dons et partenaires (plus de trente organisations citées, dont Amazon, GitHub, Microsoft, OpenAI). Les versions sont signées sous l'identité de la Fondation. La page « Why OpenClaw » se présente comme « the Switzerland of AI » (neutralité entre laboratoires).
- Licence : MIT (l'ensemble du produit). Pas d'édition entreprise, pas de service hébergé, pas de palier payant, pas de jeton.
- Langage : TypeScript/Node.js (inféré du dépôt : tsconfig, exigence Node, workspace pnpm; la page GitHub ne l'affiche pas explicitement). Dossiers racine : `apps`, `docs`, `extensions`, `packages`, `skills`, `src`, `ui`, `deploy`, `security`, `CHANGELOG`, etc.
- Popularité GitHub (2026-11-05) : environ 393 000 étoiles, 82 700 forks, 122 560 commits.
- Exigences : Node.js 24.22.1+ ou 26.12.1+ (Node 26 recommandé). Plateformes : macOS, Linux, Windows (natif ou WSL2). Applications natives : macOS, iOS, Android, Windows, Linux.
- Installation : `curl -fsSL https://openclaw.ai/install.sh | bash` (macOS/Linux/WSL2), `iwr -useb https://openclaw.ai/install.ps1 | iex` (Windows), ou `npm install -g openclaw@latest --allow-scripts=openclaw`. Docker, Nix, Podman documentés.
- Version courante : la page d'accueil des docs affiche v2026.10.10; la page des versions liste v2026.11.1 (novembre 2026) en tête; la page GitHub Releases marque 2026.10.8 comme « Latest » (27 octobre), avec 2026.10.10-beta.4 en préversion et 2026.9.35 comme canal « extended-stable » (équivalent LTS, gateway seulement). Voir section 13 (incohérence mineure).
- Rythme de publication : 9 versions en septembre 2026, 10 en octobre, 1 début novembre (deux à trois par semaine). Schéma de version `AAAA.M.N`.
- Télémétrie : vérification quotidienne de version par défaut; statistiques anonymes en opt-in; `update.checkOnStart: false` désactive les deux.

---

## 2. Architecture

Source : https://docs.openclaw.ai/concepts/architecture.md ; https://docs.openclaw.ai/concepts/agent.md ; https://docs.openclaw.ai/concepts/agent-loop.md ; https://docs.openclaw.ai/concepts/session.md ; https://docs.openclaw.ai/nodes.md ; https://docs.openclaw.ai/tools.md ; https://docs.openclaw.ai/tools/plugin (index via docs)

### 2.1 Gateway
- Un seul processus démon de longue durée, le **Gateway**, « owns all messaging surfaces » (WhatsApp via Baileys, Telegram via grammY, Slack, Discord, Signal, iMessage, WebChat). Invariant : « One Gateway per host. It is the only place that opens a WhatsApp session. »
- API WebSocket typée (requêtes, réponses, événements poussés), validée par JSON Schema (schémas TypeBox). Écoute par défaut sur `127.0.0.1:18789`. Événements émis : `agent`, `chat`, `presence`, `health`, `heartbeat`, `cron`.
- Première trame obligatoire : `connect`. « Any non-JSON or non-connect first frame is a hard close. » « Events are not replayed. Clients must refresh on gaps. »
- Authentification partagée (jeton ou mot de passe) ou par en-têtes d'identité (Tailscale Serve, trusted-proxy). Toute connexion, locale ou distante, passe par l'auth du Gateway et par l'appairage d'appareil (device identity + jeton d'appareil).
- Démarrage : `openclaw gateway` (avant-plan) ; service via launchd/systemd. Un seul `GatewayScheduler` par noyau gère maintenance et réveils cron; les échéances sont persistées et reconstruites au démarrage.
- Le Gateway tourne toujours sur l'hôte; seul l'exécution des outils peut être déplacée dans un bac à sable.

### 2.2 Clients, Control UI, WebChat
- Clients de plan de contrôle : application macOS, CLI, interface web (Control UI), automatisations. Chacun tient une connexion WebSocket.
- **Control UI** : application monopage (Vite + Solid) servie par le Gateway à `http://<hôte>:18789/`. Panneaux : Chat, Sessions, Agents (sélecteur, création, mode équipe), Automations, Dashboards, Usage, Settings (Agents, Models, Memory, Skills, Labs, Activity, MCP, Meetings), panneaux latéraux Subagents, Processes, Files, Changes, Terminal, Browser, Desktop. `gateway.controlUi.enabled` peut être basculé à chaud. Un nouveau navigateur doit être appairé à la première connexion.
- **WebChat** : UI statique utilisant la même API WebSocket, sans port HTTP séparé; inclus dans le noyau.

### 2.3 Agents
- Un agent = un espace de travail (workspace), un répertoire d'état (`agentDir`, défaut `~/.openclaw/agents/<agentId>/agent`), des profils d'authentification, un registre de modèles et un magasin de sessions SQLite (`openclaw-agent.sqlite`). Sans configuration, un seul agent `main` existe; clé de session principale `agent:main:main`.
- Runtime intégré (« embedded agent runtime ») : boucle native qui assemble le prompt et câble les outils. OpenClaw peut aussi héberger des harnais externes comme runtimes (Codex, Claude CLI, ACP) tout en gardant canaux, sessions, politique et état.
- Boucle d'exécution : RPC `agent` renvoie immédiatement `runId`; un contrôleur de session sérialise les tours par session; modes de file d'attente par canal : steer (défaut : les messages en cours de tour sont injectés dans le tour actif), followup, collect, interrupt. Budget par tentative de modèle : `agents.defaults.timeoutSeconds` (défaut 172 800 s, soit 48 h).
- Un verrou sur le répertoire d'état empêche deux Gateway (ou `openclaw agent --local`) de posséder le même état.

### 2.4 Sessions
- Routage par source : messages directs = session partagée (`main`) par défaut; groupes = isolés par groupe; salons = isolés par salon; cron = session fraîche par exécution; webhooks = isolés par hook.
- `session.dmScope` : `main` (défaut), `per-peer`, `per-channel-peer` (recommandé si plusieurs personnes), `per-account-channel-peer`. `session.groupScope` : `per-group` (défaut) ou `main`.
- Stockage : lignes de session et transcriptions dans `~/.openclaw/agents/<agentId>/agent/openclaw-agent.sqlite`; archives dans `~/.openclaw/agents/<agentId>/sessions/`. L'ancien `sessions.json` et les JSONL ne servent plus qu'à la migration.
- `/new` et `/reset` démarrent une nouvelle session. `session.reset` : `none` (défaut), `daily` (`atHour`, défaut 4), `idle` (`idleMinutes`). Le temps d'inactivité ne compte que les interactions réelles : heartbeat, cron et exec ne le prolongent pas.
- Maintenance par défaut : `pruneAfter: "30d"`, `maxEntries: 5000`, `mode: "enforce"`.

### 2.5 Canaux
- Un canal est un plugin (noyau, bundlé, officiel ou externe) qui relie une messagerie au Gateway. Les comptes multiples par canal sont possibles; les **bindings** relient `(channel, accountId, peer, guildId)` à un `agentId`. Détail en section 8.

### 2.6 Nodes
- Un **node** est un appareil compagnon connecté au Gateway avec `role: "node"`, exposant des commandes invocables (`node.invoke`) : `system.run`, caméra, écran (enregistrement, computer use), notifications, `location.get`, SMS (Android), canvas (macOS). Les nodes « are peripherals, not gateways » : les messages de canaux arrivent au Gateway, pas aux nodes.
- Plateformes : app macOS en mode node, iOS, Android, watchOS, et un **node host headless** multiplateforme (Windows et Linux inclus), lancé via `openclaw node run` (avant-plan, service, ou tunnel SSH).
- Appairage par appareil : `openclaw devices approve <requestId>`; les requêtes expirent après 5 minutes. Chaque plateforme a une liste blanche de commandes par défaut; politique sous `gateway.nodes`.
- Les nodes peuvent aussi héberger des serveurs MCP, des skills, de l'inférence locale et des sessions de travail avec isolation conteneur optionnelle.

### 2.7 Outils, skills, plugins
- **Outils** : fonctions appelables par le modèle (fichiers `read/write/edit/apply_patch`, `exec/process`, `web_search/web_fetch`, `browser`, `message`, `sessions_*`, `subagents`, `automations` (alias `cron`), `memory_search/memory_get`, `gateway`, `nodes`, `plugins`, médias, etc.). Contrôlés par `tools.profile`, `tools.allow/deny/alsoAllow`, groupes (`group:fs`, `group:runtime`, `group:web`, `group:sessions`, `group:memory`, `group:messaging`, `group:automation`, `group:ui`, `group:nodes`, `group:agents`, `group:media`, `group:plugins`). Détail en section 10.
- **Skills** : fichiers Markdown `SKILL.md` qui expliquent au modèle comment et quand utiliser des outils. Les skills ne créent pas d'outils et ne donnent pas d'accès : « AGENTS.md guides tool use but does not determine which tools exist ». Détail en section 9.
- **Plugins** : code TypeScript chargé dans le processus Gateway (canaux, fournisseurs, moteurs de mémoire, moteurs de contexte, hooks). « Not a sandbox for native plugins, which run in-process. » Installation `openclaw plugins install @openclaw/<id>` ou `clawhub:<package>`.
- **MCP** : serveurs sous `mcp.servers` (stdio, SSE, Streamable HTTP), filtrage `toolFilter`, CLI `openclaw mcp add/doctor/status/probe/login/configure`. Les outils MCP respectent la politique d'outils; en bac à sable ils exigent une entrée dans `tools.sandbox.tools` (ex. `bundle-mcp`).
- **Context engine** : moteur enfichable qui assemble le contexte, compacte et gère le contexte des sous-agents. Défaut `legacy`; `plugins.slots.contextEngine` pour en choisir un autre.

---

## 3. Le workspace et ses fichiers

Source : https://docs.openclaw.ai/concepts/agent-workspace.md ; https://docs.openclaw.ai/concepts/system-prompt.md ; https://docs.openclaw.ai/reference/templates/AGENTS.md ; https://docs.openclaw.ai/reference/templates/USER.md ; https://docs.openclaw.ai/reference/templates/IDENTITY.md ; https://docs.openclaw.ai/reference/templates/BOOTSTRAP.md ; https://docs.openclaw.ai/reference/templates/HEARTBEAT.md ; https://docs.openclaw.ai/automation/hooks/bundled-hooks.md ; https://docs.openclaw.ai/help/faq/where-things-live-on-disk.md

### 3.1 Définition et emplacement
- « The workspace is the agent's home and its working directory for file tools and workspace context. Keep it private and treat it as memory. » Séparé de `~/.openclaw/` (config, identifiants, sessions).
- Défaut : `~/.openclaw/workspace`; `OPENCLAW_WORKSPACE_DIR` remplace; profil non défaut : `~/.openclaw-<profile>/workspace`. Multi-agent : `<agents.defaults.workspace>/<agentId>` si la racine est configurée, sinon `<state-dir>/workspace-<agentId>`. Clés : `agents.defaults.workspace`, `agents.entries.*.workspace`.
- C'est un répertoire de travail par défaut, « not a hard sandbox » : des chemins absolus atteignent le reste de l'hôte sauf si le bac à sable est activé. Les outils fichiers peuvent aussi écrire dans les worktrees Git liés du dépôt du workspace.
- `openclaw onboard`, `configure` ou `setup` créent le workspace et sèment les fichiers manquants; `agents.defaults.skipBootstrap: true` désactive ce semis (« If you already manage the workspace files yourself, disable bootstrap file creation »).
- `agents.defaults.workspaceFileMode` (`"0600"`, `"0620"`, `"0640"`, `"0660"`) fixe les permissions des fichiers mémoire générés.

### 3.2 Carte des fichiers (termes officiels)

| Fichier | Rôle officiel | Chargement |
|---|---|---|
| `AGENTS.md` | Instructions opérationnelles, conventions, section Tools (notes locales; « guidance only and does not control tool access ») | Chaque session; seul fichier transmis aux sous-agents |
| `SOUL.md` | Persona, ton, limites | Chaque session |
| `USER.md` | Modèle de l'utilisateur, préférences écrites comme directives | Chaque session, budget séparé de 4 000 caractères |
| `IDENTITY.md` | Nom, créature, vibe, emoji, avatar (lignes `- Label: valeur`) | Chaque session; synchronisé avec `openclaw agents set-identity` |
| `MEMORY.md` | Mémoire longue durée curée (faits et décisions durables, pas un journal) | Injecté s'il existe à la racine; « Only load MEMORY.md in the main, private session (not shared/group contexts) » |
| `memory/YYYY-MM-DD.md` | Journaux quotidiens (couche de travail) | Pas injectés à chaque tour; indexés pour `memory_search`; aujourd'hui et hier lus sur `/new` ou `/reset` |
| `BOOTSTRAP.md` | Rituel de première exécution, créé seulement dans un workspace neuf, à supprimer ensuite | Nouveaux workspaces seulement |
| `BOOT.md` | Liste de démarrage optionnelle exécutée au démarrage du Gateway si le hook `boot-md` est activé | Hook seulement |
| `DREAMS.md` | Journal de rêve (Dreaming), pour lecture humaine | Non injecté |
| `skills/` | Skills propres au workspace, priorité la plus haute | Catalogue de skills |
| `HEARTBEAT.md` | **Retiré** : plus créé ni lu; remplacé par le « scratch » des jobs d'automatisation (`openclaw automations scratch <jobId>`) | Aucun |
| `TOOLS.md` | « not a recognized runtime bootstrap file » | Aucun |

- Ordre dans le prompt système : les fichiers bootstrap sont injectés dans la section « Workspace Files » du prompt, après Tooling, Execution Bias, Care, Runtime Context, Skills, Messaging, Workspace, et avant Sandbox, Runtime, Reasoning. Le contenu stable (fichiers du workspace) est placé au-dessus d'une frontière de cache; le contenu volatil (catalogue d'outils, skills, rappel mémoire) en dessous.
- Le gabarit AGENTS.md dit : « Use runtime-provided startup context first »; relire les fichiers seulement sur demande ou si le contexte manque. Session start (requis) : lire `SOUL.md`, `USER.md`, les notes du jour et de la veille, plus `MEMORY.md` s'il existe.
- Les fichiers situés dans des sous-dossiers ne sont pas injectés automatiquement (page Standing orders : « Subdirectory files are not auto-injected »). Le hook bundlé `bootstrap-extra-files` peut injecter des fichiers supplémentaires correspondant à des motifs (`"paths": ["packages/*/AGENTS.md"]`), mais seulement des fichiers nommés `AGENTS.md`, `SOUL.md`, `IDENTITY.md`, `USER.md`, `BOOTSTRAP.md`, `MEMORY.md`, résolus à l'intérieur du workspace; les sous-agents ne gardent que `AGENTS.md`.

### 3.3 Limites de taille
- Par fichier bootstrap : 20 000 caractères (`agents.defaults.bootstrapMaxChars`); total : 60 000 (`agents.defaults.bootstrapTotalMaxChars`); `USER.md` : 4 000. Les fichiers trop grands sont tronqués dans le prompt, intacts sur disque; diagnostic via `/context list`, `/context detail`, `openclaw doctor`.
- Fichiers requis manquants : marqueur « missing file »; optionnels : ignorés. Éditeur et `agents.files.set` : 2 MiB UTF-8 par fichier maximum.
- Les fichiers de mémoire quotidienne doivent être en UTF-8 valide, sinon la maintenance s'arrête.

### 3.4 Git et ce qu'on ne versionne pas
- Section officielle « Git backup (recommended, private) » : dépôt privé; Git optionnel; `git init`, puis `git add AGENTS.md SOUL.md IDENTITY.md USER.md memory/`, commit, remote privé (GitHub/GitLab UI ou `gh`), cycle add/commit/push. `MEMORY.md` n'apparaît pas dans la commande d'exemple mais la doc ne l'exclut pas.
- « Do not commit secrets » : pas de clés API, jetons, mots de passe, rien de `~/.openclaw/`, pas de dumps bruts de chat, même en dépôt privé. Placeholders dans le workspace; secrets réels dans un gestionnaire de mots de passe, des variables d'environnement ou `~/.openclaw/`.
- `.gitignore` de départ : `.DS_Store`, `.env`, `**/*.key`, `**/*.pem`, `**/secrets*`.
- Hors workspace, à ne pas versionner : `openclaw.json`, `credentials/`, `state/` (SQLite partagée, `secrets.json`), `agents/<id>/agent/` (auth et SQLite par agent), `agents/<id>/sessions/`. FAQ : « Don't commit anything under ~/.openclaw, since it holds credentials, sessions, tokens, and encrypted secrets. » Sauvegarde complète = workspace et répertoire d'état séparément.
- Déplacement : cloner le dépôt, pointer `agents.entries.<agentId>.workspace`, vérifier avec `openclaw agents list`, copier les sessions à part si nécessaire.

### 3.5 Gabarits officiels (résumés)
- **AGENTS.md par défaut** : sections First run, Care defaults (« Don't repeat secrets in shared chats unless the user explicitly asks you to share them »; confirmer les actions destructrices non demandées; inspecter la config et l'état du planificateur avant de modifier, préférer fusionner), Existing solutions preflight (« Build custom only when existing options are unsuitable »), Session start (requis), Soul (requis), Shared spaces, Memory system (journaux `memory/YYYY-MM-DD.md`, `USER.md` directives, `MEMORY.md` faits durables; lire avant d'écrire; pas de secrets), Tools et Local notes, Backup tip (workspace en dépôt git privé), What OpenClaw does, Core skills, Usage notes.
- **SOUL.md par défaut** : Core Truths (« Be genuinely helpful, not performatively helpful »; « No personality is just a search engine with extra steps »), Boundaries (« Don't leak private things into shared or public spaces »; en groupe, parler en son nom), Vibe (« Concise when needed, thorough when it matters »), Continuity (« Each session, you wake up fresh »; prévenir l'utilisateur quand SOUL.md change). La page concept précise : AGENTS.md = règles d'exploitation; SOUL.md = voix, position, style; court et tranchant; pas de biographie, de changelog ni de politique de sécurité dedans.
- **USER.md** : une directive par entrée, verbe impératif (Always, Never, Prefer), ligne de métadonnées avec date d'observation et statut active/superseded; « Never append a contradictory active directive » : marquer l'ancienne comme remplacée et réécrire en place.
- **IDENTITY.md** : Name, Creature, Vibe, Emoji, Avatar; rempli lors de la première conversation.
- **BOOTSTRAP.md** : « This file is a ritual, not a gate »; quatre temps (nom, vibe, avatar, recommandations); écrit IDENTITY.md et SOUL.md, lance `openclaw agents set-identity`, puis se supprime.

---

## 4. Mémoire

Source : https://docs.openclaw.ai/concepts/memory.md ; https://docs.openclaw.ai/concepts/memory-search.md ; https://docs.openclaw.ai/concepts/compaction.md ; https://docs.openclaw.ai/concepts/dreaming.md ; https://docs.openclaw.ai/concepts/active-memory.md ; https://docs.openclaw.ai/help/faq/sandboxing-and-memory.md

### 4.1 Couches
- Principe : « The model only remembers what gets saved to disk; there is no hidden state. » Mémoire = Markdown dans le workspace.
- `USER.md` (profil, directives), `MEMORY.md` (couche curée : « not a raw transcript or archive »), `memory/YYYY-MM-DD.md` (couche de travail, indexée, non injectée à chaque tour), `DREAMS.md` (journal du Dreaming), `memory/imports/` (imports externes).
- FAQ : « Memory files live on disk and persist until deleted; the limit is your storage, not the model. » Si l'agent oublie : lui demander d'écrire en mémoire (« Reminding the model to store memories usually resolves it ») et vérifier que le Gateway utilise le même workspace à chaque exécution.
- Mémoires sensibles à l'action : noter les conditions d'action (approbation requise, contrainte temporaire, expiration, autorité de la source). « Memory preserves this context but does not enforce policy. »

### 4.2 Outils
- `memory_search` : recherche hybride (vecteur + mots-clés) sur les notes, et sur les transcriptions si l'indexation de sessions est activée. `memory_get` : lit un fichier ou une plage de lignes. `intent` : intentions conditionnées par événement (les rappels temporels passent par les automatisations).
- Rappel automatique de projet : budget de préparation d'une seconde, sinon le tour continue sans.
- CLI : `openclaw memory status`, `openclaw memory search "..."`, `openclaw memory index --force`, `openclaw memory reset`.

### 4.3 Recherche et embeddings
- `memory.search.provider` : défaut OpenAI (« OpenClaw uses OpenAI embeddings by default »); valeurs : `gemini`, `voyage`, `mistral`, `bedrock`, `deepinfra`, `local` (GGUF via `@openclaw/llama-cpp-provider`, environ 0,3 Go), `ollama`, `lmstudio`, `github-copilot`, `openai-compatible`, `none` (mots-clés seulement). Non défini ou `auto` : bascule en mots-clés si l'embedding échoue; fournisseur distant nommé explicitement qui échoue : mémoire déclarée indisponible.
- Classement : pertinence × décroissance temporelle (demi-vie 30 jours pour les notes datées; `MEMORY.md` et `USER.md` ne décroissent pas) × importance; réordonnancement MMR lambda 0,7.
- Racines par défaut : Markdown seulement; `memory.search.extraPaths` ajoute d'autres fichiers (et permet images/audio avec `gemini-embedding-2`). La page multi-agent : « Shared reference material should go in an explicit memory.search.extraPaths directory. » La recherche mémoire inter-agents a été retirée.
- Indexation des sessions : `experimental.sessionMemory: true` et `sources: ["sessions"]`; les transcriptions ne sont jamais injectées automatiquement.
- Moteurs : builtin SQLite (défaut; mots-clés, vecteur, hybride), PostgreSQL (plugin `memory-core`, extensions `unaccent`, `pg_trgm`, `vector`), Honcho, LanceDB (plugins). Plugin optionnel `memory-wiki` (claims, preuves, fraîcheur; `vault.scope: "agent"` pour un coffre par agent).

### 4.4 Flush avant compaction et compaction
- Flush : avant de compacter, « OpenClaw runs a silent turn that prompts the agent to save important context ». Activé par défaut; `agents.defaults.compaction.memoryFlush.enabled: false` pour désactiver. N'écrit que dans la note du jour; nécessite un workspace en écriture (les sessions bac à sable en lecture seule le sautent); un échec ne bloque pas la compaction. Issue ouverte #45608 demande le même flush avant `/new` et le reset quotidien (non implémenté selon l'issue).
- Compaction : résume les anciens messages en une entrée compacte dans la transcription; l'historique complet reste sur disque; « Compaction only changes what the model sees on the next turn ». Déclenchement automatique au plus petit de 75 % de la fenêtre ou fenêtre moins réserve; aussi sur erreur de dépassement. Modes `default` et `safeguard` (défaut pour les nouvelles configs). Clés sous `agents.defaults.compaction` : `enabled`, `model`, `mode`, `thinkingLevel`, `timeoutSeconds`, `keepRecentTokens` (20 000), `identifierPolicy` (`strict`), `maxActiveTranscriptBytes`, `notifyUser`. `/compact [focus]` force la compaction.

### 4.5 Dreaming
- Consolidation de fond par défaut (plugin `memory-core`), trois phases : light (tri, dédoublonnage), REM (thèmes, réflexions), deep (seule phase qui écrit dans `MEMORY.md`, via une complétion sans outils qui choisit ajouts, fusions, remplacements).
- Planification : un job cron géré, défaut `0 3 * * *`; en flotte multi-agent, nécessite `agents.defaults.systemAgent.agentId`. Gates : `minScore`, `minRecallCount`, `minUniqueQueries`; candidats `untrusted` ou `system` exclus; réécriture rejetée si elle perd plus de `maxPriorEntryLossFraction` (0,25) des entrées, repli en append-only.
- Clés : `plugins.entries.memory-core.config.dreaming.{enabled, frequency, timezone, model, phases.deep.*}`. Modèle = modèle par défaut de l'agent sauf `dreaming.model` (nécessite `subagent.allowModelOverride: true`). Revue : onglet Dreams de l'UI, `openclaw memory promote`, `/dreaming status|on|off`. Version précédente de `MEMORY.md` conservée en SQLite avant chaque réécriture. Issue ouverte #67413 : configuration de Dreaming par agent (demande de fonctionnalité).

### 4.6 Active Memory
- Voie de rappel profond pour les questions sur le passé; mode par défaut `escalate` : un sous-agent de rappel bloquant n'est lancé que si le message porte sur le passé et que la voie mémoire classique n'a pas trouvé de correspondance fiable. Ajoute un appel modèle et de la latence.

### 4.7 Imports et provenance
- Control UI, Settings → Import Memory : Codex (`~/.codex/memories`), Claude Code (auto-memory de projet), Hermes (`MEMORY.md`, `USER.md`). Stockés dans `memory/imports/`, indexés, séparés de `MEMORY.md`.
- Provenance : fichiers marqués non fiables ou distants sans identité de source valide exclus du contexte automatique (avertissement « memory provenance is untrusted »); `openclaw doctor` les liste.

---

## 5. Multi-agents et sous-agents

Source : https://docs.openclaw.ai/concepts/multi-agent.md ; https://docs.openclaw.ai/channels/channel-routing.md ; https://docs.openclaw.ai/cli/agents.md ; https://docs.openclaw.ai/tools/subagents.md et sous-pages nesting, tool-policy, tool-reference, operations, announce ; https://docs.openclaw.ai/help/faq/sessions-and-chats.md ; https://docs.openclaw.ai/help/faq/remote-gateways-and-nodes.md

### 5.1 Agents multiples (routage)
- « One Gateway process can run multiple isolated agents, each with its own workspace, state directory (agentDir), and SQLite-backed session history. » FAQ : « No. One Gateway hosts multiple agents » (pas besoin de plusieurs VPS); « No hard limits - dozens or even hundreds are fine », surveiller disque, coût en jetons et charge opérationnelle.
- Configuration : `agents.ownership: "explicit"`, `agents.entries.<id>` (`workspace`, `agentDir`, `model`, `name`, `identity`, `sandbox`, `tools`, `subagents`, `skills`, `groupChat.mentionPatterns`, `params`), `agents.defaults` (valeurs communes, `systemAgent.agentId` pour le trafic non lié et les opérations système, `authInheritance.agentId` source des identifiants). L'ancien `agents.list` est hérité et migré par `openclaw doctor --fix`.
- Avertissement : « Never reuse agentDir across agents » (collisions d'auth et de sessions). Les identifiants OAuth ne sont pas copiés; un agent secondaire lit à travers l'agent propriétaire d'héritage; seuls les profils statiques `api_key`/`token` se copient (`copyToAgents`).
- Isolation par agent : workspace (AGENTS.md, SOUL.md, USER.md, notes), répertoire d'état (auth, registre de modèles), magasin de sessions. Ce qui n'est pas automatiquement séparé : les stockages de plugins (ex. Memory Wiki sauf `vault.scope: "agent"`).
- Bindings : `{ agentId, match: { channel, accountId, peer: {kind, id}, guildId, roles, teamId }, session: { dmScope, groupScope } }`. Priorité du plus spécifique au moins spécifique : peer exact, peer parent, peer joker, guild+rôles, guild, team, compte, canal (`accountId: "*"`), propriétaire de repli. Tous les champs d'un binding doivent correspondre (ET). « Multiple agents without an owner require a matching binding; routing does not pick the first roster entry. » Sans correspondance ni propriétaire : message non distribué.
- Exemple officiel (WhatsApp) : agents `home` (workspace `~/.openclaw/workspace-home`) et `work` (`~/.openclaw/workspace-work`), comptes `personal` et `biz`, bindings `{ agentId: "home", match: { channel: "whatsapp", accountId: "personal" } }` et `{ agentId: "work", match: { channel: "whatsapp", accountId: "biz" } }`. Exemple sandbox/outils par agent : `mode: "off"` pour l'agent personnel, `mode: "all"` et `scope: "agent"` pour l'autre avec `allow: ["read"]`, `deny: ["exec", "write", "edit", "apply_patch"]`.
- Skills par agent : chargés depuis le workspace de l'agent et les racines partagées (`~/.openclaw/skills`), filtrés par `agents.defaults.skills` (base) et `agents.entries.*.skills` (remplace, ne fusionne pas; `[]` = aucun skill).
- Accès inter-agents : `tools.agentToAgent` activé par défaut; `allow` vide = toutes les paires; `enabled: false` bloque; `tools.sessions.visibility` restreint la visibilité des sessions.
- Commandes : `openclaw agents add <name>` (`--workspace`, `--agent-dir`, `--model`, `--role`, `--bind`), `openclaw agents list --bindings`, `openclaw agents delete <id>` (déplace vers la corbeille), `openclaw agents set-identity --from-identity`, `openclaw agents bind --agent work --bind telegram:ops`.
- **Préréglage équipe** : `openclaw agents team create --prefix <p> --workspace-root <dir>` crée un coordinateur et trois spécialistes (`researcher`, `writer`, `reviewer`) à partir de gabarits de rôle; le coordinateur a `subagents.allowAgents` = les trois spécialistes et `delegationMode: "prefer"`; chaque spécialiste a `subagents.allowAgents: []` et l'instruction de rendre ses résultats sans déléguer; `systemAgent.agentId` pointe vers le coordinateur s'il n'était pas défini. Chaque workspace : `<workspace-root>/<agentId>`.
- FAQ « one CEO and many agents » : possible via routage multi-agent et sous-agents (« one coordinator agent plus several worker agents with their own workspaces and models »), qualifié de « a fun experiment » et « token-heavy and often less efficient than one bot with separate sessions ».
- Deux Gateway (local + VPS) : « there is no built-in bot-to-bot bridge »; passer par un canal de chat partagé ou un script CLI `openclaw agent`.

### 5.2 Sous-agents
- Définition : « Sub-agents are background agent runs spawned from an existing agent run. » Session propre `agent:<agentId>:subagent:<uuid>`. « A subagent run ends; a session does not. » Par défaut le résultat est annoncé au demandeur.
- Objectifs : parallélisme (recherche, tâches longues), isolation par défaut (session séparée, bac à sable optionnel), surface d'outils difficile à détourner (« sub-agents do not get session or message tools by default »), profondeur configurable.
- Outil `sessions_spawn` : `task` (requis), `taskName`, `label`, `agentId` (autre agent, « only when allowed by subagents.allowAgents »), `cwd`, `projectId`/`projectGitUrl`, `runtime` (`subagent` ou `acp`), `model` (défaut : modèle de l'appelant ou `agents.defaults.subagents.model`), `runTimeoutSeconds`, `thinking`, `thread`, `mode` (`run` défaut, `session` avec `thread: true`), `cleanup` (`keep`/`delete`), `expectsCompletionMessage`, `completionTarget` (`parent`), `sandbox` (`inherit`/`require`), `context` (`isolated` défaut, `fork` = copie la transcription du parent, même agent seulement), `blind`, `visible`, `group`, `worktree*`. Les paramètres de livraison de canal (`channel`, `to`, etc.) ne sont pas acceptés.
- Contexte transmis : les sous-agents reçoivent uniquement `AGENTS.md` du workspace (prompt en mode `minimal` : sans rappel mémoire, sans identité utilisateur, sans messagerie). Le « brief » passe donc par `task`.
- Limites (sous `agents.defaults.subagents`) : `maxSpawnDepth` défaut 5 (1 à 5), `maxChildrenPerAgent` défaut 5 (1 à 20), `maxConcurrent` défaut 8 par session appelante, `runTimeoutSeconds` défaut 900 (0 = illimité), `announceTimeoutMs` 120 000. Swarm : `tools.swarm.maxConcurrent` 32, `maxChildrenPerGroup` 50, `maxTotalPerGroup` 200. Profondeur 0 = agent principal; 1 à 4 = orchestrateurs; 5 = feuille.
- Outils refusés aux sous-agents cachés quelle que soit la profondeur : `gateway`, `agents_list`, `session_status`, `progress_card`, `cron`, `message`, `sessions_send`, `conversations_*` (non contournable par `allow`). Au plafond de profondeur : aussi `subagents`, `sessions_list`, `sessions_history`, `sessions_spawn`. `tools.subagents.tools.deny/allow` restreint davantage (deny gagne). `subagents.delegateToolsTo` permet à un agent restreint de confier l'implémentation à un agent cible listé dans `allowAgents`. Issue ouverte #111631 : les sous-agents n'héritent pas du `tools.profile` du parent.
- Sessions visibles (`visible: true`) : persistantes, pilotables par l'utilisateur, sans les refus intégrés; la doc dit de les réserver aux cas où l'utilisateur veut une session séparée.
- Coordination : `sessions_yield` (« Ends the current model turn and waits for announced child completion events »; ne pas remplacer par du polling), `subagents` (actions `list`, `wait` jusqu'à 32 runIds, `cancel` en cascade), `sessions_send` avec `mode: "steer"` pour rediriger un run, `sessions_history` pour lire la transcription (blocs tronqués à 4 000 caractères, 80 Ko max). Chaîne d'annonce : chaque niveau ne voit que ses enfants directs; le parent synthétise puis annonce vers le haut.
- Annonce : le parent reçoit un bloc avec source, identifiants de session, statut (`ok`, `error`, `timeout`, `unknown` dérivé du runtime), contenu du résultat (plafond partagé 160 000 caractères), ligne de statistiques (durée, jetons, coût estimé si tarification configurée, chemin de transcription). Un `NO_REPLY` du parent clôt la complétion; un `NO_REPLY` de l'enfant déclenche la récupération « réponse manquante ». Issue ouverte #44925 : complétion de sous-agent perdue silencieusement sur timeout.
- Modèle et coût : chaque sous-agent a son propre contexte et sa propre consommation; la doc suggère un modèle moins cher via `agents.defaults.subagents.model` et de garder l'agent principal sur un modèle plus fort.
- `agents.defaults.subagents.delegationMode` : `"prefer"` dans la session principale, `"suggest"` ailleurs (défaut). Niveau de réflexion `ultra` ajoute une section d'orchestration quand `sessions_spawn` est disponible.
- Reset complet : annule les sous-agents natifs non finis de la session; `/stop` annule l'arbre des enfants.

---

## 6. Proactivité : automatisations (cron) et heartbeat

Source : https://docs.openclaw.ai/automation/cron-jobs.md et sous-pages how-it-works, schedules, payloads, delivery ; https://docs.openclaw.ai/gateway/heartbeat.md ; https://docs.openclaw.ai/automation/standing-orders.md ; https://docs.openclaw.ai/start/openclaw.md ; https://docs.openclaw.ai/help/faq/skills-and-automation.md

### 6.1 Modèle
- « Automations » = planificateur intégré; `openclaw automations` (alias `openclaw cron`). Les jobs, l'état et l'historique vivent dans la base SQLite d'état partagée. « Automations run inside the Gateway process, not inside the model, so the Gateway must be running for schedules to fire. »
- Types de planification : `at` (horodatage ISO ou relatif `20m`), `every` (`10m`, `1h`, `1d`), `cron` (5 ou 6 champs, `--tz`), `on-exit`, `stream`. « Timestamps without a timezone are treated as UTC »; cron sans `--tz` = fuseau de l'hôte. Les expressions à la minute 0 sont étalées jusqu'à 5 minutes (`--exact` pour désactiver). Jour du mois et jour de semaine en OU logique (comportement Vixie); `+` pour exiger les deux.
- Charges utiles (une par job) : `systemEvent` (`--system-event`, tour ordinaire dans la session principale), `agentTurn` (`--message`), `command` (processus hôte, sans modèle), `script` (code-mode headless). Sessions : `main`, `current`, `isolated` (« A new transcript/session id per run »; pas de contexte ambiant), `session:<id>` (session nommée persistante). Options : `--model`, `--fallbacks`, `--thinking`, `--light-context` (saute l'injection des fichiers bootstrap), `--tools` (`""` = aucun outil).
- **Contrat d'exécution sans surveillance** : « Isolated automation and hook agent turns are explicitly unattended »; personne pour répondre ou approuver; la réponse finale doit être le livrable; si rien à faire, `NO_REPLY`. Pour les runs cron, les cartes d'approbation exec ne vont qu'aux clients d'approbation connectés, pas aux canaux de chat.
- Livraison : `announce` (vers un chat, ajouté à l'historique de ce chat), `webhook`, `none`, `delivery.target: "owner"` (DM du propriétaire, résolu à la livraison). Exemple : `--announce --channel telegram --to "-1001234567890"`. Alertes d'échec après 2 échecs consécutifs (cooldown 1 h); auto-désactivation après 10 échecs d'exécution consécutifs; `failureAlert: false` pour couper. La langue de sortie doit être écrite dans le message du job (« Respond in Chinese; keep URLs... »).
- Budgets : `--timeout-seconds`, sinon 60 min (agent-turn), 10 min (command), 5 min (script). Les jobs `at` s'auto-suppriment après succès sauf `--keep-after-run`. Au redémarrage, les jobs en retard sont replanifiés, pas rejoués.
- Politiques : `activeHours` (fenêtre horaire avec fuseau), `idleOnly`, `payload.skipIfScratchEmpty`, `pacing.min/max` (l'agent propose `next_check`). `/loop [intervalle] <prompt>` (propriétaire seulement). Déclencheurs d'événement : script de condition `{ fire, message?, state? }` (30 s minimum, 5 appels d'outils, état 16 Ko); avertissement : ces scripts tournent « unattended with the agent's full tool policy, including exec »; `cron.triggers.enabled: false` pour désactiver.
- Promotion d'une demande répétée en job : l'agent vérifie `automations(action: "list")`, confirme en langage clair, crée le job livré dans la conversation courante, l'exécute une fois en mode `force` comme test visible, et le supprime si le test échoue.
- Webhooks entrants : `POST /hooks/wake`, `POST /hooks/agent`, `POST /hooks/<name>`; `hooks.enabled`, `hooks.token`. Dépannage FAQ : vérifier `cron.enabled`, l'absence de `OPENCLAW_SKIP_CRON`, que le Gateway reste debout, le fuseau, et `openclaw automations runs <jobId> --limit 50`.

### 6.2 Heartbeat (état actuel)
- « Heartbeat monitoring now uses ordinary automation jobs. » `HEARTBEAT.md` n'est plus créé ni lu; `openclaw doctor --fix` importe son contenu dans le « scratch » du job et archive le fichier sous `backups/heartbeat-migration/`. Les clés `agents.defaults.heartbeat` et `agents.entries.*.heartbeat` sont supprimées après migration.
- La configuration explicite crée un job « Proactive check » activé pour l'agent propriétaire, toutes les 30 minutes (une heure pour l'auth Anthropic OAuth/jeton selon la page assistant personnel), dans la conversation principale du propriétaire, répondant `NO_REPLY` s'il n'y a rien. Un scratch vide saute l'exécution. « Shorter intervals consume more tokens. » Les heartbeats ne tournent pas dans les sessions de groupe. Désactiver le job n'arrête pas les suivis événementiels (fin d'exec en arrière-plan).
- Suppression des doublons et saut sans route supprimés : les mises à jour répétées peuvent être livrées; une route manquante ne bloque plus l'exécution. Jeton `HEARTBEAT_OK` : retiré du texte à la livraison.
- Base d'état avancée au schéma 30 par la migration : sauvegarder avant mise à jour.

### 6.3 Standing orders (patron officiel)
- Page dédiée : des programmes permanents avec portée, déclencheurs, portes d'approbation et règles d'escalade. Stockés dans le workspace, de préférence dans `AGENTS.md` (injecté chaque session), ou dans un fichier séparé type `standing-orders.md` référencé depuis `AGENTS.md`. « Standing orders define what the agent is authorized to do. Automations define when it happens »; le prompt de l'automatisation renvoie à l'ordre permanent au lieu de le répéter. Exemples officiels : rapport hebdomadaire du vendredi, contenu et réseaux sociaux avec revue du propriétaire les 30 premiers jours, surveillance système. Pratiques : commencer étroit, sections « what not to do », boucle exécuter-vérifier-rapporter, trois tentatives avant escalade.

---

## 7. Modèles et authentification

Source : https://docs.openclaw.ai/providers/anthropic.md ; https://docs.openclaw.ai/providers/openai.md ; https://docs.openclaw.ai/providers/models.md ; https://docs.openclaw.ai/concepts/model-failover.md ; https://docs.openclaw.ai/help/faq-first-run/quick-start.md ; https://docs.openclaw.ai/help/faq-first-run/providers-and-hosting.md ; https://docs.openclaw.ai/help/faq/security-and-access-control.md

- Sélection : `agents.defaults.model.primary` au format `provider/model`, `agents.defaults.model.fallbacks`, alias, `agents.entries.*.model` (strict sauf `fallbacks` propre). « Credentials and model selection are separate » : une clé API authentifie, le modèle reste celui de `model.primary`. Environ 35 fournisseurs listés (Anthropic, OpenAI, Google, OpenRouter, Bedrock, Mistral, xAI, etc.); endpoints personnalisés et locaux (vLLM, Ollama, LM Studio) supportés via `models.providers.<id>`.
- Routage par tâche : modèle des sous-agents (`agents.defaults.subagents.model`), de la compaction (`agents.defaults.compaction.model`), du Dreaming (`dreaming.model`), par job cron (`--model`, `--fallbacks`), par hook Gmail (`hooks.gmail.model`), reviewer exec (`tools.exec.reviewer.model`), modèle de décision (`decisionModel`). Plafond par modèle : `tools.byProvider["provider/model"].exec.mode`.
- Failover : 16 tentatives sur le même modèle pour les limites de débit, puis rotation des profils d'auth du même fournisseur, puis repli sur `fallbacks`. Cooldowns : 30 s, 1 min, 5 min; échec de facturation ou d'auth permanent : désactivation 10 min. Profils : `api_key`, `oauth`, `token`; secrets dans la SQLite par agent; `auth.profiles` et `auth.order` ne portent que des métadonnées.
- **Anthropic** : trois routes. (1) Clé API : facturation à l'usage; « API-key auth uses separate pay-as-you-go billing and is preferable for shared automation ». (2) Claude CLI : réutilise la connexion Claude Code de l'hôte; OpenClaw appelle l'exécutable `claude` sans lire ni stocker les jetons; l'usage « draw from the signed-in subscription's limits ». (3) Setup token : `claude setup-token` donne un jeton `sk-ant-oat01-...`, ou `openclaw models auth login --provider anthropic --method setup-token`. La doc ne mentionne aucune interdiction; elle note qu'Anthropic a suspendu un plan de crédit Agent SDK le 15 juin 2026, que « Anthropic currently treats the claude -p path as subscription-plan usage subject to your plan's limits », que ce traitement « has changed over time », et conseille de « check current Anthropic billing docs before relying on subscription behavior ». Pour un serveur ou de la production : « an Anthropic API key remains the safer, more predictable choice ». Modèles listés : Opus 5.5 (`anthropic/claude-opus-5-5`, défaut en configuration neuve, alias `opus`), Opus 5, Sonnet 5.5 (`sonnet`), Sonnet 5, Fable 5.1 (`fable`), Mythos 5, Haiku 5.5; fenêtre d'entrée de 1 000 000 jetons, 128 000 en sortie. Cache de prompt (clé API) : `params.cacheRetention` `short` (5 min, défaut), `long` (1 h), `none`.
- **OpenAI** : un seul identifiant `openai` pour clé API et abonnement ChatGPT/Codex (OAuth). « OpenAI explicitly supports subscription OAuth usage in external tools and workflows like OpenClaw. » Défaut abonnement : `openai/gpt-6-astra`; « OpenClaw does not silently downgrade ». Les API non agent (embeddings par défaut, images) exigent `OPENAI_API_KEY` ou un profil clé API.
- FAQ modèles moins chers : oui pour des agents chat seulement avec entrées fiables; « Smaller tiers are more susceptible to instruction hijacking »; à éviter pour des agents outillés ou lisant du contenu non fiable, sinon restreindre les outils et bac à sable.
- Plafond de dépense : aucune clé de budget ou de plafond de coût n'a été trouvée dans les pages lues (voir section 13).
- Locaux : Ollama, LM Studio, llama.cpp pour l'inférence et les embeddings; exemple officiel « Local models only » avec LM Studio sur `127.0.0.1:1234`.

---

## 8. Canaux

Source : https://docs.openclaw.ai/channels.md ; https://docs.openclaw.ai/channels/telegram.md ; https://docs.openclaw.ai/channels/telegram/setup.md ; https://docs.openclaw.ai/channels/telegram/access-control.md ; https://docs.openclaw.ai/channels/whatsapp.md ; https://docs.openclaw.ai/channels/pairing.md ; https://docs.openclaw.ai/channels/groups.md

- Distribution : noyau (WebChat), bundlés (A2A, Reef, Telegram, X), officiels (Discord, Google Chat, iMessage, IRC, LINE, Matrix, Mattermost, Teams, Nextcloud Talk, Nostr, QQ, Signal, Slack, SMS, Synology, Twitch, WhatsApp, Zalo, etc.), externes (WeChat, WeCom, Yuanbao). Voice Call (Plivo, Telnyx, Twilio) en plugin officiel. « Text is supported everywhere; media and reactions vary by channel. » Installation : `openclaw plugins install @openclaw/<id>` ou `openclaw channels add`.
- Concepts communs : `dmPolicy` (`pairing` défaut, `allowlist`, `open` qui exige `allowFrom: ["*"]`, `disabled`), `allowFrom`, `groupPolicy` (`allowlist` défaut, `open`, `disabled`), `groupAllowFrom`, `groups` (allowlist de groupes, `"*"`), `requireMention` (défaut vrai en groupe; les messages sans mention sont gardés comme contexte), `mentionPatterns`, `joinIntro`.
- Appairage : code de 8 caractères majuscules, expire après 1 h, maximum 3 requêtes en attente par compte; approbation `openclaw pairing approve <channel> <CODE>` ou Control UI; « Approval grants direct-message access only ». Première approbation = premier propriétaire de commandes (`commands.ownerAllowFrom`); les suivantes n'ajoutent pas de propriétaires. Stockage : `~/.openclaw/state/openclaw.sqlite`.
- **Telegram** : « production-ready for bot DMs and groups via grammY »; canal le plus rapide à configurer (jeton seulement, pas de plugin). Jeton : `openclaw channels add --channel telegram --token <token>`, `botToken`, `tokenFile` (fichier régulier), ou `TELEGRAM_BOT_TOKEN` (compte par défaut seulement); précédence `tokenFile` > `botToken` > env. Long polling par défaut, webhook optionnel. `allowFrom` : identifiants numériques d'utilisateur (préfixes `telegram:`/`tg:` acceptés; `@username` hérité, résolu au mieux par doctor); « One-owner bots should use allowlist with numeric user IDs ». Groupes : identifiants négatifs (`-100...` pour les supergroupes) sous `groups`; sujets de forum : `groups.<chatId>.topics."*"`, chaque sujet a sa session; mode privé des bots à désactiver (`/setprivacy`) ou bot admin pour `requireMention: false`; `/whoami@<bot>` donne les identifiants. `historyLimit` 50. Multi-compte : fixer `channels.telegram.defaultAccount` et un binding par compte. `textChunkLimit`, `mediaMaxMb`. Exemple officiel :
  `{ channels: { telegram: { enabled: true, botToken: "123:abc", dmPolicy: "pairing", groups: { "*": { requireMention: true } } } } }`
- **WhatsApp** : « production-ready via WhatsApp Web (Baileys) »; plugin `@openclaw/whatsapp`; « Login is QR-only » (`openclaw channels login --channel whatsapp`); « A separate WhatsApp number is recommended »; un seul Gateway ouvre la session Baileys par hôte; `selfChatMode` (DM à soi-même traités comme entrée agent); `allowFrom` en E.164; `ask_user` indisponible; blocs de 4 000 caractères; médias 50 Mo; statuts et diffusions ignorés. La page ne traite pas du risque de bannissement.
- Sessions de groupe : clés propres; un binding peut mettre `session.groupScope: "main"`. Outils par groupe : `tools` et `toolsBySender` (clés `id:` recommandées). Les demandeurs restreints ne peuvent créer que des sous-agents cachés du même agent.

---

## 9. Skills et ClawHub

Source : https://docs.openclaw.ai/tools/skills.md ; https://docs.openclaw.ai/clawhub.md ; https://docs.openclaw.ai/help/faq/skills-and-automation.md ; https://docs.openclaw.ai/help/faq/security-and-access-control.md

- Format : dossier avec `SKILL.md` (frontmatter YAML + corps Markdown). Minimum :
  ```
  ---
  name: image-lab
  description: Generate or edit images via a provider-backed image workflow
  ---
  ```
  Autres clés : `homepage`, `user-invocable` (défaut vrai, commande slash), `disable-model-invocation`, `command-dispatch`/`command-tool`/`command-arg-mode`. `{baseDir}` référence le dossier du skill. Bloc `metadata.openclaw` : `requires.bins`, `requires.anyBins`, `requires.env`, `requires.config`, `always`, `os`, `primaryEnv`, `install`. Le nom vient du champ `name`, pas du dossier; découverte jusqu'à six niveaux.
- Précédence (le plus haut gagne) : `<workspace>/skills`, `<workspace>/.agents/skills`, `~/.agents/skills`, `<state-dir>/skills` (`~/.openclaw/skills`, « managed »), `workshop-skills` par agent, skills bundlés, puis `skills.load.extraDirs` et skills de plugins. `skills.load.watch` (défaut vrai; rescan de sécurité toutes les 30 min).
- Partage entre agents : les skills se chargent depuis le workspace de chaque agent et les racines partagées; `agents.defaults.skills` = base commune; `agents.entries.*.skills` remplace (ne fusionne pas); `[]` = aucun. « An allowlist does not restrict host shell access. » Dans le prompt : liste compacte `<available_skills>` avec emplacements, lue à la demande; budgets `skills.limits.maxSkillsPromptChars` et `agents.entries.*.skillsLimits.maxSkillsPromptChars`.
- Installation : `openclaw skills install @owner/<slug>` (dans `skills/` du workspace; `--global` pour le dossier managé partagé), `openclaw skills update --all`, `openclaw skills verify @owner/<slug>`, sources git (`git:owner/repo@ref`) et locales (`--as`). Archives téléversées : désactivées par défaut (`skills.install.allowUploadedArchives`).
- ClawHub (clawhub.ai) : registre public de skills et plugins, versions semver, scans de sécurité affichés avant installation; « open by default: anyone can upload » (compte GitHub assez ancien requis); CLI `clawhub` séparée (`login`, `skill publish`, `package publish`). Jusqu'à 2026.10.4, préfixe `clawhub:` requis pour les plugins. Télémétrie d'installation désactivable `CLAWHUB_DISABLE_TELEMETRY=1`.
- Sécurité : « Treat third-party skills as untrusted code, and read them before enabling. » FAQ : « Treat third-party skills and plugins as code you are choosing to trust »; les scans ne sont pas une frontière de sécurité; OpenClaw ne bloque pas le code local dangereux à l'installation. `skills.entries.*.env` et `apiKey` injectent des secrets dans le processus hôte pour le tour, pas dans le bac à sable. `security.installPolicy` peut exécuter une commande de politique locale avant installation. Les surcharges managées l'emportent sur les skills bundlés sans toucher au dépôt.

---

## 10. Sécurité et déploiement

Source : https://docs.openclaw.ai/gateway/security.md ; https://docs.openclaw.ai/gateway/sandboxing.md ; https://docs.openclaw.ai/tools/exec.md ; https://docs.openclaw.ai/tools/exec-approvals.md ; https://docs.openclaw.ai/tools/permission-modes.md ; https://docs.openclaw.ai/gateway/config-tools/tool-policy.md ; https://docs.openclaw.ai/gateway/secrets.md ; https://docs.openclaw.ai/gateway/remote.md ; https://docs.openclaw.ai/gateway/tailscale.md ; https://docs.openclaw.ai/vps.md ; https://docs.openclaw.ai/install/docker.md ; https://docs.openclaw.ai/concepts/multi-user.md ; https://docs.openclaw.ai/gateway/config-tools/github-identity.md ; https://docs.openclaw.ai/automation/imap.md

### 10.1 Modèle de confiance
- « Not secure by default. Sandboxing and exec approvals are off by default, and default use is a trusted single-operator assistant. » Une Gateway = une frontière de confiance (« a single operator, or a team whose members trust each other »); pas une frontière multi-locataire; utilisateurs mutuellement hostiles = Gateway, hôtes ou utilisateurs OS séparés. Sur un VPS partagé : runtime dédié avec utilisateur OS dédié, jamais connecté aux comptes Apple/Google personnels ni aux profils de navigateur ou de gestionnaire de mots de passe personnels.
- Défauts conservateurs : Gateway sur loopback, DM inconnus en appairage, groupes en allowlist derrière une porte de mention. Images conteneur : bind exposé donc auth obligatoire. « To check for drift, run openclaw security audit » (`--deep`; `openclaw security` applique les correctifs; `openclaw policy` inspecte la politique d'outils).
- Injection de prompt : contenu non fiable encapsulé; choisir le modèle en conséquence; garder les drapeaux de contournement hors production. « Agents with message-tool access can send across conversations and channel providers by default. » FAQ : le bot devrait avoir ses propres comptes (courriel, GitHub, numéro) : « Isolating the bot with separate accounts and phone numbers limits the damage if something goes wrong. »

### 10.2 Politique d'outils
- `tools.profile` : `minimal` (`session_status`, `gateway` maintenance), `coding` (fs, runtime, web, sessions, memory, cron, goals, `progress_card`, `ask_user`, skills, génération d'images, `view_image`), `messaging` (`message`, sessions, `subagents`, `ask_user`), `full` (pas de filtrage). « Full selects tools; it does not grant Full access execution permissions. »
- Couches dans l'ordre : profil, `tools.allow/deny` globaux (deny gagne, jokers `*`), `tools.alsoAllow` (incompatible avec `allow` dans la même portée), `tools.byProvider`, `tools.toolsBySender`, `tools.elevated` (exec hors bac à sable; par agent, ne peut que restreindre), porte bac à sable `tools.sandbox.tools`. Bloquer toute écriture : `deny: ["write", "edit", "apply_patch"]` (et `exec` reste capable d'écrire). `tools.codeMode` (`auto`, `true`; exécuteur `node` ou `quickjs`).

### 10.3 Exec et approbations
- `exec` : hôte `auto` (défaut; = `sandbox` si bac à sable actif, sinon `gateway`), `sandbox`, `gateway`, `node`; `tools.exec.timeoutSeconds` 1 800; processus en arrière-plan via `process` après `yieldMs` 10 000; `durable: true` (systemd 250+). « Sandboxing is off by default. » « Gateway and node exec run without approval prompts by default (mode=full). »
- `tools.exec.mode` : `deny`, `allowlist`, `ask` (allowlist + question sur échec), `auto` (reviewer natif rend `allow`/`deny`/`ask`; trois refus consécutifs escaladent à un humain), `full`. Document d'approbations hôte : `security` (`deny`/`allowlist`/`full`, défaut `full` sur gateway et node, `deny` en bac à sable), `ask` (`off`/`on-miss`/`always`), `askFallback` (défaut `deny`), `strictInlineEval`, `autoAllowSkills`. Politique effective = la plus stricte des deux. Allowlists par agent, stockées dans `exec_approvals_config` de la base d'état; motifs glob et `argPattern`. Flux : événement `exec.approval.requested` vers les clients opérateurs (Control UI, app macOS, réactions Matrix, `/approve`). Pour les runs cron : seulement vers les clients d'approbation connectés, pas les canaux.
- Recommandation de la page Permission modes : `auto` pour les agents de codage ayant besoin d'accès hôte.

### 10.4 Bac à sable
- Désactivé par défaut; `agents.defaults.sandbox` ou `agents.entries.*.sandbox`. « Only tool execution moves into the sandbox. The Gateway stays on the host. » « Not a perfect security boundary. » Modes `off`, `non-main` (sessions non principales en conteneur, DM principal sur l'hôte), `all`; `scope` `session`, `agent`, `shared`; `workspaceAccess` `none`/`ro`/`rw`; backends Docker (défaut), Podman, SSH, OpenShell, Crabbox; `docker.setupCommand`, `docker.binds` (« Use :ro for anything sensitive »), racine lecture seule, `network: "none"` dans l'exemple étendu. Exemple minimal : `mode: "non-main"`, `scope: "session"`, `workspaceAccess: "none"`. CLI : `openclaw sandbox list|explain|recreate`.
- Identité GitHub managée (`tools.github`) : OAuth device flow ou PAT, profil privé côté Gateway, jeton jamais dans la config ni les transcriptions; exclue du bac à sable sauf `allowInSandbox: true` (avertissement dans l'audit); `github_publish` commit, pousse, ouvre des PR mais « never merges on its own ».
- Lecteur de courriel restreint (patron officiel, pages Gmail et IMAP) : agent dédié `mail_reader`, bac à sable par message sans accès workspace, profil `minimal`, groupes fs/runtime/web/browser/cron/gateway/node refusés, `agentToAgent` désactivé ou restreint, `deliver: false`. Plugin IMAP bundlé : `plugins.entries.imap`, `allowedSenders` obligatoire (liste vide = compte désactivé), authentification DMARC `verified` par défaut, `pollSeconds` minimum 15, pas de backfill, pas de rejeu après échec. « Any link navigation, file write, shell command, or browser action triggered by an email counts as a failed boundary check. »

### 10.5 Secrets
- SecretRefs opt-in par identifiant : fournisseurs `env`, `file`, `exec` (recettes 1Password, Bitwarden `bws`, Vault, `pass`, `sops`), `store` (SQLite partagée). Le texte en clair reste lisible par l'agent s'il est dans des fichiers inspectables (`openclaw.json`, `.env`, `models.json`). Flux : `openclaw secrets audit --check`, `configure`, `apply`. Outil `secrets` masqué pour fournir un identifiant sans l'exposer. Variables `OPENCLAW_GATEWAY_TOKEN`, `OPENCLAW_GATEWAY_PASSWORD`, `TELEGRAM_BOT_TOKEN`, `ANTHROPIC_API_KEY` dans `.env` ou `env.vars`.

### 10.6 Exposition réseau et accès distant
- `gateway.bind` : `loopback` (défaut, recommandé), `lan`, `tailnet`, `custom`, `auto`; tout bind non loopback exige l'auth (`gateway.auth.mode` : `token` défaut, `password`, `trusted-proxy`, `none` à proscrire hors loopback). Runbook : « Keep the Gateway loopback-only unless a non-loopback bind is clearly needed »; préférer « Loopback + SSH/Tailscale Serve ». `wss://` obligatoire pour les hôtes publics; `gateway.remote.tlsFingerprint` pour l'épinglage.
- Tailscale : `gateway.tailscale.mode` `serve` (tailnet seulement, Gateway sur 127.0.0.1, HTTPS; « Prefer Tailscale Serve for HTTPS »), `funnel` (public, exige `password`, Tailscale 1.38.3+, ports 443/8443/10000), `off`. `gateway.auth.allowTailscale: true` : auth par en-tête `tailscale-user-login` vérifié via `tailscale whois`, pour Control UI et WebSocket seulement. « Avoid Funnel for browser control, and treat node pairing like operator access. » Tunnel SSH : `ssh -N -L 18789:127.0.0.1:18789 user@gateway-host`.
- Nodes : « Treat browser control like operator access: tailnet-only, plus deliberate node pairing. »

### 10.7 Déploiement serveur Linux
- Architecture : « The Gateway runs on the VPS and owns the state and workspace »; connexion depuis portable ou téléphone via Control UI, Tailscale ou SSH; sauvegarder état et workspace régulièrement (« the VPS is the source of truth »). FAQ : minimum 1 vCPU, 1 Go RAM, 500 Mo disque; recommandé 1 à 2 vCPU, 2 Go+; Ubuntu LTS le mieux testé; WhatsApp, Telegram, Slack, Mattermost, Discord « all work fine from a VPS ».
- Service : `openclaw onboard --install-daemon` installe une unité systemd utilisateur; `Restart=always`, `RestartSec=5`; recommandé `OPENCLAW_NO_RESPAWN=1`, `NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache`, `TimeoutStartSec=90` sur hôtes lents; `systemctl --user edit openclaw-gateway.service`. Admin : installer Tailscale, vérifier une seconde session SSH par IP Tailscale, puis restreindre le SSH public.
- Docker : images `ghcr.io/openclaw/openclaw` (miroir Docker Hub), tags version/`latest`/`main`/`extended-stable`, variantes `-browser`; services `openclaw-gateway` et `openclaw-cli`; volumes état (`/home/node/.openclaw`) et workspace; `/healthz`, `/startupz`, `/readyz`; une réplique par volume d'état; `openclaw doctor --fix --non-interactive` à chaque démarrage avec état conservé.
- Mise à jour et sauvegarde : CLI `openclaw backup`, `openclaw update`, `openclaw doctor`; triage : `openclaw triage`, `status`, `gateway probe`, `channels status --probe`, `logs --follow`. Logs dans `/tmp/openclaw/`.

### 10.8 Multi-utilisateur
- Mode multi-utilisateur = plusieurs personnes de confiance sur le même agent; propriété et participants sont des commodités, « not security boundaries ». Scopes `operator.read/write/sessions.write/admin`; comptes de modèles par personne; liens publics de transcription possibles. Recommandation 1 : « Don't rely on ownership for isolation. »

---

## 11. Conventions et « façon OpenClaw »

Source : pages citées en sections 3, 5, 6, 10 ; https://docs.openclaw.ai/concepts/soul.md ; https://docs.openclaw.ai/start/openclaw.md ; https://docs.openclaw.ai/concepts/system-prompt.md

Ce que la doc présente comme bonne pratique ou patron recommandé :

1. **Workspace = mémoire, en dépôt Git privé.** Fichiers de personnalité et de mémoire dans le workspace, jamais rien de `~/.openclaw/`; secrets en placeholders. Sauvegarder état et workspace séparément.
2. **Séparation des rôles entre fichiers.** `AGENTS.md` = règles d'exploitation et conventions; `SOUL.md` = voix et position (« shorter and sharper is better », pas de changelog ni de politique de sécurité); `USER.md` = directives sur l'utilisateur avec statut active/superseded; `MEMORY.md` = faits et décisions durables curés (pas un journal); `memory/YYYY-MM-DD.md` = journal brut; `IDENTITY.md` = identité synchronisée avec la config.
3. **Lire avant d'écrire, prévenir des changements.** Le gabarit AGENTS.md demande de lire les fichiers avant de les modifier et d'annoncer les changements de `SOUL.md`; inspecter config et planificateur existants et fusionner plutôt que remplacer; chercher des solutions existantes avant de construire sur mesure.
4. **Session principale privée vs espaces partagés.** `MEMORY.md` seulement en session principale; en groupe, l'agent parle en son nom; ne pas divulguer de données privées dans les espaces partagés; `dmScope: "per-channel-peer"` dès qu'il y a plusieurs humains.
5. **Standing orders dans AGENTS.md, cadence dans les automatisations.** Le « quoi » autorisé dans le workspace, le « quand » dans les jobs; les prompts de jobs renvoient aux ordres permanents; commencer étroit, sections « ce qu'il ne faut pas faire », escalade après trois tentatives.
6. **Runs non surveillés = livrables, pas de questions.** Un job isolé doit rendre le résultat ou `NO_REPLY`; il ne peut pas demander d'approbation dans le chat. Préférer cron aux boucles de sommeil; ne pas faire de polling sur les sous-agents, utiliser `sessions_yield` et les événements de complétion.
7. **Proactivité par défaut mais mesurée.** Un job « Proactive check » toutes les 30 minutes créé à l'installation; la doc dit de le revoir ou le désactiver pendant l'évaluation, et que des intervalles plus courts coûtent des jetons.
8. **Isolation par agent, pas par promesse.** Workspace, `agentDir` et sessions propres; jamais d'`agentDir` partagé; pour de la vraie séparation de confiance, Gateway/hôte/utilisateur OS séparés. Comptes dédiés pour le bot (courriel, GitHub, téléphone, numéro WhatsApp séparé).
9. **Sous-agents cachés pour le travail interne, modèle moins cher pour eux.** Brief dans `task`, seul `AGENTS.md` du workspace cible est injecté; résultats remontés par la chaîne d'annonce; sessions visibles réservées aux demandes explicites.
10. **Patron équipe officiel.** Coordinateur + spécialistes (`researcher`, `writer`, `reviewer`), `allowAgents` fermé chez les spécialistes, `delegationMode: "prefer"` chez le coordinateur, `systemAgent` = coordinateur. La FAQ le qualifie d'expérimental et gourmand en jetons.
11. **Patron lecteur de boîte courriel restreint.** Agent dédié, bac à sable sans workspace, profil minimal, pas de livraison automatique, test d'injection avant mise en service.
12. **Durcissement progressif.** Démarrer loopback + Tailscale Serve ou SSH; `security audit` régulier; passer `tools.exec.mode` à `ask` ou `auto` si des invites sont voulues; `non-main` sandbox pour les groupes; SecretRefs pour sortir les secrets du texte en clair; clé API pour la production.
13. **Config comme code validé.** JSON5 unique `openclaw.json`, schéma strict (clé inconnue = Gateway refuse de démarrer, code 78), hot reload, `$include` pour découper, `openclaw doctor --fix` comme outil de migration; dernière config valide conservée.

---

## 12. Points pertinents pour notre projet

Fondé sur `/home/claude/openclaw/recherche/CONTEXTE-PROJET.md`. Pour chaque élément : ce que la doc officielle dit, classé **Conforme** (prévu tel quel), **À adapter** (prévu avec une nuance), **Non prévu** (aucune fonction officielle), **Déconseillé** (la doc le présente comme à éviter). Aucune recommandation ici.

| Élément de l'architecture envisagée | Ce que dit la doc officielle | Statut |
|---|---|---|
| Serveur Linux toujours allumé, Gateway unique | Patron VPS documenté; une Gateway par hôte; systemd; sauvegardes état + workspace; specs minimales 1 vCPU/1 Go | Conforme |
| Un seul agent au départ (« CEO ») | Mode mono-agent `main` par défaut; multi-agent ajoutable plus tard via `agents.entries` et bindings | Conforme |
| Interface web privée + Telegram | Control UI/WebChat servis par le Gateway sur 18789 derrière loopback + Tailscale Serve ou SSH; Telegram bundlé, `allowFrom` numérique, `dmPolicy: allowlist` pour un bot à propriétaire unique | Conforme |
| Workspace = dépôt Git privé | Section officielle « Git backup (recommended, private) »; `.gitignore` de départ; rien de `~/.openclaw/` | Conforme |
| `AGENTS.md`, `SOUL.md`, `USER.md`, `MEMORY.md` à la racine | Fichiers bootstrap officiels, injectés chaque session (MEMORY.md en session principale seulement); limites 20 000 / 60 000 / 4 000 caractères | Conforme (surveiller les tailles) |
| `organisation.md`, `projets/`, `journal/decisions.md`, `PASSATION.md`, `commun/` | Pas des fichiers bootstrap : non injectés automatiquement; lisibles par les outils `read`/`memory_get`; indexables par `memory_search` seulement s'ils sont dans les racines par défaut (`memory/`, `MEMORY.md`, `USER.md`) ou ajoutés via `memory.search.extraPaths` | À adapter |
| `departements/` avec un `AGENTS.md` par équipe, activés un à la fois | Les sous-dossiers ne sont pas injectés; le hook `bootstrap-extra-files` peut injecter des `AGENTS.md` correspondant à des motifs (tous ceux qui correspondent, dans les budgets); la notion « activer un département à la fois » n'existe pas en tant que telle; le patron officiel pour plusieurs équipes est le préréglage équipe (agents séparés avec workspaces séparés) | À adapter / Non prévu tel quel |
| `skills/` dans le workspace (validateur, Gmail IMAP, GitHub) | `<workspace>/skills` est l'emplacement de plus haute précédence; `SKILL.md` avec `name` et `description`; un skill n'ajoute pas d'outil : la lecture IMAP existe comme plugin bundlé (agent lecteur restreint, pas skill), GitHub comme identité managée `tools.github` + `github_publish` | À adapter |
| Heartbeat 30 min pour clôture par inactivité | Le heartbeat classique est retiré; l'équivalent est un job « Proactive check » (défaut 30 min) avec scratch, `activeHours`, `idleOnly`, `NO_REPLY`; `session.reset: idle` existe par ailleurs pour couper la session | À adapter (terminologie et mécanisme) |
| Rituel du lundi, filet du soir (cron), relances de projets dormants | Automatisations `cron` avec `--tz`, sessions `main` ou `isolated`, livraison `owner` ou canal; standing orders dans `AGENTS.md` référencés par les jobs; décalage de 5 min sur les heures pleines sauf `--exact`; OU logique jour du mois / jour de semaine | Conforme |
| Mise à jour des fiches au fil de l'eau | Outils `write`/`edit` sur le workspace; flush mémoire avant compaction (note du jour seulement); Dreaming écrit dans `MEMORY.md` et pas ailleurs; aucune fonction native d'« index de fiches » | À adapter |
| Délégation à des sous-agents avec le brief du département | `sessions_spawn` avec `task`; les sous-agents reçoivent seulement `AGENTS.md` du workspace cible (pas SOUL/USER/MEMORY), prompt `minimal`; ni `message` ni `cron` ni `sessions_send`; profondeur 5, 5 enfants, 8 concurrents par défaut; modèle dédié possible | À adapter (le brief doit tenir dans `task` ou dans un fichier lu par le sous-agent) |
| Validateur indépendant | Même agent via sous-agent caché, ou autre agent via `agentId` + `subagents.allowAgents`; préréglage équipe inclut un `reviewer` | Conforme |
| Boucle d'évaluation (brief versionné, evaluations.md) | Pas de fonction native; ligne de statistiques d'annonce (durée, jetons, coût) et `sessions_history` fournissent la matière; `cleanup: keep` conserve les sessions enfants | Non prévu (faisable par fichiers) |
| Aucune action externe sans GO | Couches officielles : `tools.deny` (`message`, `exec`...), `tools.exec.mode: ask/auto`, approbations exec, `tools.agentToAgent`, `toolsBySender`; mais « Isolated automation ... turns are explicitly unattended » : un job ne peut pas attendre un GO dans le chat; les cartes d'approbation cron ne vont qu'aux clients d'approbation | À adapter (le GO ne peut passer que par la session interactive) |
| Aucune automatisation ajoutée à l'installation | L'installation crée un job « Proactive check » et le job Dreaming (`0 3 * * *`); la doc dit comment les désactiver (`openclaw automations list --all`, `dreaming.enabled: false`); l'agent peut aussi proposer de créer des jobs (promotion) | À adapter (il y a des automatisations par défaut) |
| Secrets hors dépôt | Conforme à « Do not commit secrets »; SecretRefs, `.env`, `~/.openclaw/`; `skills.entries.*.env` pour les secrets de skills | Conforme |
| Identités publiques séparées entre marques | FAQ : comptes dédiés pour le bot; multi-compte par canal avec bindings par compte; identité par agent (`IDENTITY.md`, `agents.entries.*.identity`); un seul agent = une seule identité | À adapter si une identité par marque est voulue |
| Fournisseur principal Anthropic par clé API | La doc préfère la clé API pour la production et l'automatisation partagée; avertit que le traitement des abonnements par Anthropic change; Opus 5.5 par défaut, cache de prompt `short` | Conforme |
| Plafond de dépense | Aucune clé de budget ou de plafond trouvée dans les pages lues; seulement coût estimé dans les annonces de sous-agents et panneau Usage | Non prévu (selon pages lues) |
| IA locale plus tard | Ollama, LM Studio, llama.cpp documentés pour l'inférence et les embeddings; FAQ : modèles plus petits plus sensibles au détournement d'instructions pour les agents outillés | Conforme |
| Paperclip (organigramme, tickets, budgets) | Absent de la documentation OpenClaw | Non prévu |
| PC Windows de Dominic (DaVinci) | Un node host headless existe pour Windows (`openclaw node run`), avec `system.run` sous allowlist; les nodes sont des périphériques du Gateway, pas des agents | Conforme (comme node) |
| Claude Desktop / projets Claude et ChatGPT existants | Import de mémoire officiel depuis Claude Code (auto-memory), Codex et Hermes vers `memory/imports/`; rien pour les projets Claude.ai ou ChatGPT | À adapter |

---

## 13. Non lu / incertain

### URL non accessibles
- https://docs.openclaw.ai/tools/github.md : erreur client (page inexistante; contenu trouvé sous `/gateway/config-tools/github-identity.md`).
- https://docs.openclaw.ai/install/tailscale.md : erreur client (contenu trouvé sous `/gateway/tailscale.md`).
- https://docs.openclaw.ai/automation/hooks/session-memory.md : erreur client (contenu trouvé dans `/automation/hooks/bundled-hooks.md`).
- https://github.com/openclaw/openclaw/issues?... : bloqué par robots; remplacé par l'API REST `search/issues` (résultats partiels, triés par réactions).
- API REST `repos/openclaw/openclaw` et `releases/latest` : accès non activé dans cette session (403); métadonnées prises sur la page GitHub publique.
- Non parcourus (hors périmètre ou temps) : pages par fournisseur autres qu'Anthropic et OpenAI, pages de plugins (Codex harness, Knowledge, 1Password), ACP agents, Code Mode, médias, protocole RPC, schémas de base de données, pages d'installation par hébergeur, `/concepts/memory-provenance`, `/reference/memory-config`, `/automation/hooks/writing-hooks`, `/gateway/config-hooks`, `/channels/telegram/messaging`, `/tools/skills-config`, `/gateway/health`, `/gateway/doctor`.

### Points incertains ou contradictoires
- Version courante : accueil des docs = v2026.10.10; page des versions = v2026.11.1 en tête; GitHub Releases = 2026.10.8 « Latest » et 2026.10.10-beta.4 en préversion. Les trois pages n'ont pas été capturées au même instant; la version stable réelle au 2026-11-05 est à confirmer.
- Intervalle du job proactif par défaut : 30 minutes (pages heartbeat et assistant personnel), « one hour for Anthropic OAuth/token auth » (page assistant personnel seulement).
- Statut du hook `session-memory` : la page bundled-hooks ne dit pas s'il est activé par défaut; la page mémoire dit que les variantes slugguées qu'il produit sont chargées automatiquement.
- `MEMORY.md` et Git : la commande d'exemple ne l'ajoute pas explicitement, sans l'exclure.
- `requireAgentId` (mentionné dans l'index sous-agents comme clé d'application) : non décrit dans la référence des outils.
- Risque de bannissement WhatsApp : non traité par la page du canal.
- Plafond de coût : absence constatée dans les pages lues, pas une certitude sur l'ensemble de la doc.
- Issues GitHub citées (#29387 fichiers bootstrap dans agentDir ignorés; #45608 flush avant reset; #67413 Dreaming par agent; #44925 complétion de sous-agent perdue; #111631 sous-agents sans profil d'outils du parent; #90299 équipes « lost active execution context »; #159596 pression mémoire du Gateway 2026.9.6) : titres et dates lus par API, contenu non lu; statut d'ouverture au 2026-11-05.

---

## 14. Sources (toutes consultées le 2026-11-05)

Documentation officielle (docs.openclaw.ai) :
- https://docs.openclaw.ai (index)
- https://docs.openclaw.ai/start/why-openclaw.md
- https://docs.openclaw.ai/start/getting-started.md
- https://docs.openclaw.ai/start/openclaw.md
- https://docs.openclaw.ai/start/docs-directory.md
- https://docs.openclaw.ai/concepts/features.md
- https://docs.openclaw.ai/concepts/architecture.md
- https://docs.openclaw.ai/concepts/agent.md
- https://docs.openclaw.ai/concepts/agent-loop.md
- https://docs.openclaw.ai/concepts/system-prompt.md
- https://docs.openclaw.ai/concepts/context-engine.md
- https://docs.openclaw.ai/concepts/soul.md
- https://docs.openclaw.ai/concepts/agent-workspace.md
- https://docs.openclaw.ai/concepts/session.md
- https://docs.openclaw.ai/concepts/multi-user.md
- https://docs.openclaw.ai/concepts/memory.md
- https://docs.openclaw.ai/concepts/memory-search.md
- https://docs.openclaw.ai/concepts/active-memory.md
- https://docs.openclaw.ai/concepts/dreaming.md
- https://docs.openclaw.ai/concepts/compaction.md
- https://docs.openclaw.ai/concepts/multi-agent.md
- https://docs.openclaw.ai/concepts/model-failover.md
- https://docs.openclaw.ai/concepts/managed-worktrees.md
- https://docs.openclaw.ai/tools.md
- https://docs.openclaw.ai/tools/skills.md
- https://docs.openclaw.ai/tools/subagents.md
- https://docs.openclaw.ai/tools/subagents/nesting.md
- https://docs.openclaw.ai/tools/subagents/tool-policy.md
- https://docs.openclaw.ai/tools/subagents/tool-reference.md
- https://docs.openclaw.ai/tools/subagents/operations.md
- https://docs.openclaw.ai/tools/subagents/announce.md
- https://docs.openclaw.ai/tools/exec.md
- https://docs.openclaw.ai/tools/exec-approvals.md
- https://docs.openclaw.ai/tools/permission-modes.md
- https://docs.openclaw.ai/tools/mcp.md
- https://docs.openclaw.ai/tools/browser.md
- https://docs.openclaw.ai/automation/cron-jobs.md
- https://docs.openclaw.ai/automation/cron-jobs/how-it-works.md
- https://docs.openclaw.ai/automation/cron-jobs/schedules.md
- https://docs.openclaw.ai/automation/cron-jobs/payloads.md
- https://docs.openclaw.ai/automation/cron-jobs/delivery.md
- https://docs.openclaw.ai/automation/cron-jobs/gmail.md
- https://docs.openclaw.ai/automation/imap.md
- https://docs.openclaw.ai/automation/hooks.md
- https://docs.openclaw.ai/automation/hooks/bundled-hooks.md
- https://docs.openclaw.ai/automation/standing-orders.md
- https://docs.openclaw.ai/gateway/configuration.md
- https://docs.openclaw.ai/gateway/configuration-examples.md
- https://docs.openclaw.ai/gateway/config-tools.md
- https://docs.openclaw.ai/gateway/config-tools/tool-policy.md
- https://docs.openclaw.ai/gateway/config-tools/github-identity.md
- https://docs.openclaw.ai/gateway/heartbeat.md
- https://docs.openclaw.ai/gateway/security.md
- https://docs.openclaw.ai/gateway/sandboxing.md
- https://docs.openclaw.ai/gateway/secrets.md
- https://docs.openclaw.ai/gateway/remote.md
- https://docs.openclaw.ai/gateway/tailscale.md
- https://docs.openclaw.ai/channels.md
- https://docs.openclaw.ai/channels/telegram.md
- https://docs.openclaw.ai/channels/telegram/setup.md
- https://docs.openclaw.ai/channels/telegram/access-control.md
- https://docs.openclaw.ai/channels/whatsapp.md
- https://docs.openclaw.ai/channels/pairing.md
- https://docs.openclaw.ai/channels/groups.md
- https://docs.openclaw.ai/channels/channel-routing.md
- https://docs.openclaw.ai/clawhub.md
- https://docs.openclaw.ai/providers/anthropic.md
- https://docs.openclaw.ai/providers/openai.md
- https://docs.openclaw.ai/providers/models.md
- https://docs.openclaw.ai/nodes.md
- https://docs.openclaw.ai/vps.md
- https://docs.openclaw.ai/install/docker.md
- https://docs.openclaw.ai/web/control-ui.md
- https://docs.openclaw.ai/cli.md
- https://docs.openclaw.ai/cli/agents.md
- https://docs.openclaw.ai/reference/AGENTS.default.md
- https://docs.openclaw.ai/reference/templates/AGENTS.md
- https://docs.openclaw.ai/reference/templates/SOUL.md
- https://docs.openclaw.ai/reference/templates/USER.md
- https://docs.openclaw.ai/reference/templates/IDENTITY.md
- https://docs.openclaw.ai/reference/templates/BOOTSTRAP.md
- https://docs.openclaw.ai/reference/templates/HEARTBEAT.md
- https://docs.openclaw.ai/releases.md
- https://docs.openclaw.ai/help/faq.md
- https://docs.openclaw.ai/help/faq/sessions-and-chats.md
- https://docs.openclaw.ai/help/faq/sandboxing-and-memory.md
- https://docs.openclaw.ai/help/faq/security-and-access-control.md
- https://docs.openclaw.ai/help/faq/where-things-live-on-disk.md
- https://docs.openclaw.ai/help/faq/remote-gateways-and-nodes.md
- https://docs.openclaw.ai/help/faq/skills-and-automation.md
- https://docs.openclaw.ai/help/faq-first-run.md
- https://docs.openclaw.ai/help/faq-first-run/quick-start.md
- https://docs.openclaw.ai/help/faq-first-run/providers-and-hosting.md
- https://docs.openclaw.ai/help/troubleshooting.md

GitHub :
- https://github.com/openclaw/openclaw (README, métadonnées)
- https://github.com/openclaw/openclaw/releases
- API REST GitHub `search/issues` (repo:openclaw/openclaw, is:open, requêtes « multi-agent », « subagent », « cron automation », « memory »)
