# 05. Code source d'OpenClaw et conventions réelles

Date : 2026-11-05
Source principale : clone superficiel de `github.com/openclaw/openclaw`, branche `main`, commit `ba400bcb7cde1671fce060ff840161c3e478ac75` (daté du 2026-11-05 19:42 UTC). Version déclarée dans `package.json` : **2026.11.1**.
Méthode : lecture directe des schémas zod (`src/config/`), des gabarits (`docs/reference/templates/`), de la documentation embarquée (`docs/`), du CHANGELOG et du code des sous-agents, cron, skills et sécurité. Tous les chemins ci-dessous sont relatifs à la racine du dépôt; URL de base : `https://github.com/openclaw/openclaw/blob/main/`.
Avertissement : la documentation en ligne (docs.openclaw.ai) est générée depuis `docs/`; ce document cite donc la source de cette documentation, pas une copie tierce. Rien ici n'est une recommandation; la section 11 ne fait que relier le cahier aux mécanismes réels.

## 1. Schéma de configuration `openclaw.json`

Le schéma est en **zod** (pas de JSON Schema statique dans le dépôt; `$schema` est accepté comme clé optionnelle). Fichier racine : `src/config/zod-schema.root-shape.ts` (`OpenClawSchemaShape`). Les sous-schémas sont dans `src/config/zod-schema.*.ts`, les types dans `src/config/types.*.ts`.

Clés de premier niveau (ordre du fichier) : `$schema`, `meta`, `env`, `wizard`, `diagnostics`, `logging`, `update`, `telemetry`, `browser`, `ui`, `secrets`, `auth`, `accessGroups`, `acp`, `models`, `nodeHost`, `agents`, `worktreeRoot`, `repositoryBranchPrefix`, `worktreeAcceleration`, `worktreeMaxCount`, `tools`, `security`, `bindings`, `broadcast`, `attachments`, `messages`, `tts`, `commands`, `approvals`, `session`, `cron`, `transcripts`, `hooks`, `channels`, `discovery`, `talk`, `gateway`, `cloudWorkers`, `database`, `storage`, `desktop`, `memory`, `mcp`, `skills`, `plugins`, `surfaces`, `proxy`.

### 1.1 Agents (`src/config/zod-schema.agents.ts`, `zod-schema.agent-entry-base.ts`, `zod-schema.agent-defaults-base.ts`)

```ts
export const AgentsSchema = z.strictObject({
  ownership: z.literal("explicit").optional(),
  defaults: z.lazy(() => AgentDefaultsSchema).optional(),
  entries: z.record(AgentIdKeySchema, AgentEntryConfigSchema).optional(),
})
```

- `agents.entries` est un **objet indexé par identifiant** (regex `^[a-z0-9_][a-z0-9_-]{0,63}$`), pas un tableau. L'ancienne forme `agents.list` est migrée par `openclaw doctor --fix` (`docs/gateway/doctor/config-migrations.md`, ligne 732).
- Validation : `agents.entries` doit contenir au moins une entrée; au plus une entrée `default: true`; **dès qu'il y a plus d'un agent**, il faut `agents.ownership: "explicit"` (ou le marqueur hérité `default: true`), sinon erreur `multi-agent rosters require agents.ownership="explicit"`.
- Clés d'une entrée (`AgentEntryBaseSchema` étendu par `AgentEntrySchema`, `zod-schema.agent-runtime.ts` ligne 481) : `name`, `description`, `workspace`, `workspaceFileMode`, `cwd`, `agentDir`, `model`, `utilityModel`, `decisionModel`, `models`, `modelPolicy`, `thinkingDefault`, `verboseDefault`, `reasoningDefault`, `fastModeDefault`, `newSessionPermissionMode` (`read-only | guarded | workspace | full`), `contextInjection`, `bootstrapMaxChars`, `bootstrapTotalMaxChars`, `skills` (tableau de noms), `subagents`, `embeddedAgent`, `params`, `runtime` (`embedded` ou `acp`), `memory.search`, `humanDelay`, `typingMode`, `tts`, `skillsLimits`, `contextLimits`, `identity`, `groupChat`, `sandbox`, `tools`.
- `model` accepte une chaîne `provider/model` ou `{ primary, fallbacks[] }` (`zod-schema.agent-model.ts`).
- `agents.defaults` (`AgentDefaultsBaseSchema`) ajoute notamment : `workspace`, `skipBootstrap`, `skipOptionalBootstrapFiles` (valeurs permises : `SOUL.md`, `USER.md`, `HEARTBEAT.md`, `IDENTITY.md`), `userTimezone`, `startupContext`, `contextPruning`, `compaction` (dont `memoryFlush.enabled`), `elevatedDefault`, `timeoutSeconds` (défaut documenté : 48 h), `maxConcurrent`, `systemAgent.agentId`, `authInheritance.agentId`, `sessionStore.agentId`, `subagents`, `sandbox`.

### 1.2 Sous-agents (`zod-schema.agent-defaults-base.ts`, lignes 212 à 250)

```ts
subagents: z.strictObject({
  delegationMode: z.enum(["suggest", "prefer"]).optional(),
  allowAgents: z.array(z.string()).optional(),
  maxConcurrent: ...  // "default: 8"
  maxSpawnDepth: z.number().int().min(1).max(5)  // "Default: 5; 1 makes direct children leaves."
  maxChildrenPerAgent: z.number().int().min(1).max(20)  // "default: 5"
  archiveAfterMinutes, model, thinking, runTimeoutSeconds, announceTimeoutMs, requireAgentId
})
```

Au niveau d'une entrée d'agent, seules `delegationMode`, `allowAgents`, `delegateToolsTo`, `model`, `thinking`, `requireAgentId` existent; la profondeur et le parallélisme sont **globaux** (`agents.defaults.subagents`).

### 1.3 Bindings de canaux (`zod-schema.agents.ts`, lignes 120 à 168)

```ts
const RouteBindingSchema = z.strictObject({
  type: z.literal("route").optional(),
  agentId: z.string(),
  match: { channel, accountId?, peer?: { kind: "direct"|"group"|"channel", id }, guildId?, teamId?, roles? },
  session?: { dmScope?: "main"|"per-peer"|"per-channel-peer"|"per-account-channel-peer", groupScope?: "main"|"per-group" },
});
export const BindingsSchema = z.array(z.union([RouteBindingSchema, AcpBindingSchema])).optional();
```

`bindings` est une clé de **premier niveau** (pas sous `agents`). Un binding sans `accountId` ne couvre que le compte `default`; `accountId: "*"` couvre tous les comptes du canal.

### 1.4 Cron (`zod-schema.root-shape.ts`, lignes 418 à 476)

Clés : `cron.enabled`, `defaultTools`, `skipMissedJobs`, `triggers.enabled`, `webhookToken`, `webhookSsrfPolicy`, `sessionRetention` (défaut `"24h"`), `delivery { channel, to, threadId, accountId }`, `failureAlert { enabled, after, cooldownMs, includeSkipped, mode: "announce"|"webhook", channel, to, threadId, accountId }`, `agents.<agentId> { delivery, failureAlert }`, `creators.<profileId> { delivery, failureAlert }`.
Les **tâches elles-mêmes ne sont pas dans openclaw.json** : elles sont persistées dans la base SQLite partagée et gérées par `openclaw automations ...` (voir section 5).

### 1.5 Heartbeat : clé retirée

Le test `src/config/zod-schema.agent-defaults.test.ts` (ligne 104) est explicite :

```ts
it("rejects retired heartbeat config at both runtime agent scopes", () => {
  const heartbeat = { every: "30m", target: "owner" };
  expect(AgentDefaultsSchema.safeParse({ heartbeat }).success).toBe(false);
  expect(AgentEntrySchema.safeParse({ id: "ops", heartbeat }).success).toBe(false);
});
```

`docs/gateway/heartbeat.md` (titre « Heartbeat migration ») : « Heartbeat monitoring now uses ordinary automation jobs. Doctor converts existing monitors into editable jobs ». La note de version 2026.9.7 (`docs/releases/2026.9.7.md`, ligne 1295) mentionne la migration des réglages `heartbeat` retirés. La clé `agents.defaults.heartbeat` **n'existe plus**; l'équivalent est un job d'automatisation avec `--every`, `--idle-only`, `--active-hours-*`, `--skip-if-scratch-empty` et un « scratch » (bloc-notes du job) à la place de `HEARTBEAT.md`.

### 1.6 Sandbox (`zod-schema.agent-runtime.ts`, lignes 382 à 410; `zod-schema.sandbox.ts`)

`agents.defaults.sandbox` ou `agents.entries.*.sandbox` : `mode: "off"|"non-main"|"all"`, `backend`, `workspaceAccess: "none"|"ro"|"rw"`, `sessionToolsVisibility`, `scope: "session"|"agent"|"shared"`, `workspaceRoot`, `docker {...}`, `ssh {...}`, `browser {...}`, `prune {...}`. Par défaut la sandbox est **désactivée** (`docs/gateway/sandboxing.md` : « Sandboxing is off by default »).

### 1.7 Nodes

Il n'existe pas de clé `nodes.autoApprove`. Les clés réelles sont dans `gateway.nodes` (`zod-schema.gateway.ts`, lignes 433 à 470) : `gateway.nodes.pairing.autoApproveLocal` (booléen), `gateway.nodes.pairing.autoApproveCidrs` (tableau), `gateway.nodes.pairing.sshVerify`, `gateway.nodes.commands.allow` / `deny`, `gateway.nodes.allowSkills`, `gateway.nodes.pluginTools.enabled`, `gateway.nodes.browser.mode`. La clé racine `nodeHost` configure, elle, la machine qui **est** un node (`autoUpdate`, `agentRuns`, `workerRuns`, `browserProxy`, `mcp`, `skills`).

### 1.8 Mémoire (`zod-schema.root-support.ts` ligne 110; `zod-schema.memory-search.ts`)

`memory.citations: "auto"|"on"|"off"`, `memory.search { enabled, rememberAcrossConversations, sources: ["memory","sessions"], extraPaths, multimodal, provider, remote, fallback, model, inputFormat, outputDimensionality, local { modelPath }, store { fts.tokenizer: "unicode61"|"trigram" }, query, cache }`. Le fournisseur d'embeddings par défaut est OpenAI (`docs/concepts/memory.md` : « OpenClaw uses OpenAI embeddings by default »); alternatives documentées : Gemini, Voyage, Mistral, Bedrock, DeepInfra, GGUF local (`memory.search.local.modelPath`), Ollama, LM Studio, endpoint compatible OpenAI.
Le « dreaming » n'est pas sous `memory` mais sous le plugin : `plugins.entries.memory-core.config.dreaming.enabled` (défaut `true`), `frequency` (défaut `"0 3 * * *"`), `model` (`docs/concepts/dreaming.md`, section « Key defaults »). Quand il est actif, `memory-core` crée et gère lui-même **un job cron** pour la passe de dreaming.

### 1.9 Gateway (`zod-schema.gateway.ts`)

`gateway.port` (défaut 18789, `src/gateway/server-start.ts` ligne 20), `gateway.mode: "local"|"remote"`, `gateway.bind: "auto"|"lan"|"loopback"|"custom"|"tailnet"` (défaut `loopback` sur un hôte ordinaire; `auto` = `0.0.0.0` dans un conteneur détecté), `customBindHost`, `publicOrigin`, `controlUi { enabled, basePath, root, ... }`, `auth { mode: "none"|"token"|"password"|"trusted-proxy", token, password, allowTailscale, identityScopes, rateLimit, trustedProxy }`, `roles`, `trustedProxies`, `tailscale { mode: "off"|"serve"|"funnel" }`, `remote`, `reload { mode: "off"|"hybrid" }`, `tls`, `http`, `push`, `nodes`.
`docs/gateway/config-gateway.md` ligne 149 : « Auth: required by default. Non-loopback binds require gateway auth. Onboarding wizard generates a token by default. »

## 2. Gabarits officiels du workspace

Dossier : `docs/reference/templates/` (ce sont les gabarits documentés; les fichiers créés à l'onboarding sont dérivés des mêmes textes). Fichiers présents : `AGENTS.md`, `AGENTS.dev.md`, `SOUL.md`, `SOUL.dev.md`, `USER.md`, `USER.dev.md`, `IDENTITY.md`, `IDENTITY.dev.md`, `BOOTSTRAP.md`, `BOOT.md`, `HEARTBEAT.md` (page de retrait), `TOOLS.md` (page de retrait), `roles/`.

### 2.1 Ordre d'injection, fichiers requis, troncature

`src/agents/workspace-bootstrap-policy.ts` :

```ts
export const WORKSPACE_BOOTSTRAP_FILENAMES = [
  "AGENTS.md", "SOUL.md", "IDENTITY.md", "USER.md", "BOOTSTRAP.md", "MEMORY.md",
] as const;  // "Canonical bootstrap filenames in prompt order."
export const GENERATED_WORKSPACE_BOOTSTRAP_FILENAMES = ["AGENTS.md","SOUL.md","IDENTITY.md","USER.md"];
```

- Fichiers générés dans un workspace neuf : `AGENTS.md`, `SOUL.md`, `IDENTITY.md`, `USER.md`, plus `BOOTSTRAP.md` (rituel de première conversation, à supprimer ensuite).
- Limites (`src/agents/embedded-agent-helpers/bootstrap-limits.ts` et `bootstrap.ts`) : `DEFAULT_BOOTSTRAP_MAX_CHARS = 20_000` par fichier, `DEFAULT_BOOTSTRAP_TOTAL_MAX_CHARS = 60_000` au total, `USER_BOOTSTRAP_MAX_CHARS = 4_000` (plafond fixe de USER.md, non relevable par `bootstrapMaxChars`). Les fichiers trop longs sont tronqués dans le prompt, le fichier sur disque reste intact. Un fichier requis manquant produit un marqueur « missing file »; `USER.md` et `MEMORY.md` absents sont simplement omis (`docs/concepts/agent-workspace.md`, note sous « Workspace file map »).
- `MEMORY.md` : chargé **dans la session principale seulement** (gabarit AGENTS.md : « Load only in the main session »). `memory/YYYY-MM-DD.md` : notes du jour et de la veille chargées sur `/new` ou `/reset`; indexées pour `memory_search`, non injectées à chaque tour.
- Emplacement par défaut : `~/.openclaw/workspace`; override `agents.defaults.workspace` puis `agents.entries.*.workspace`; variables `OPENCLAW_WORKSPACE_DIR`, `OPENCLAW_STATE_DIR`, `OPENCLAW_PROFILE`. Dans un roster multi-agents, une entrée sans `workspace` reçoit `<agents.defaults.workspace>/<agentId>` ou `<state-dir>/workspace-<agentId>`.
- `agents.defaults.skipBootstrap: true` désactive la création des fichiers de bootstrap si l'on gère soi-même le workspace.

### 2.2 AGENTS.md (gabarit, 7 278 octets)

Sections : « Red Lines » (pas de partage de données privées, confirmer les actions destructrices, inspecter avant de modifier crontab/systemd, préférer `trash` à `rm`); « External vs Internal » (« Ask first: public or outbound actions the user did not request »); « Existing Solutions Preflight »; « First Run » (suivre `BOOTSTRAP.md` puis le supprimer); « Session Startup » (utiliser le contexte injecté, relire les fichiers seulement si nécessaire); « Session Ownership and Follow-through »; « Memory » (notes quotidiennes `memory/`, `USER.md` = directives sur l'utilisateur, `MEMORY.md` = faits et décisions durables, « Write It Down », maintenance par automatisation planifiée); « Group Chats »; « Tools » (notes locales, remplace l'ancien TOOLS.md); « Automations - Be Proactive » (« Keep checklists and check timing in each automation's scratch »; commandes `openclaw automations list --all`, `openclaw automations scratch <jobId> --set "..."`); « Make It Yours ».

Extrait (section Memory) :

```md
- **Daily notes:** `memory/YYYY-MM-DD.md` holds raw logs; create `memory/` if needed.
- **User model:** `USER.md` holds stable preferences and profile facts as active directives.
- **Long-term:** `MEMORY.md` holds durable non-profile facts and decisions.
```

### 2.3 SOUL.md (1 660 octets)

« Core Truths » (aider sans flatter, avoir des opinions, chercher avant de demander, « Check before public or outbound actions nobody asked for », « Remember you're a guest »), « Boundaries », « Vibe », « Continuity » (« These files _are_ your memory ... If you change this file, tell the user »).

### 2.4 USER.md (1 277 octets)

Une directive par entrée, précédée d'un commentaire de métadonnées :

```md
<!-- observed: YYYY-MM-DD | status: active -->
- Prefer concise progress updates during implementation work.
```

Règles : impératifs (`Always`, `Never`, `Prefer`), statut `active` ou `superseded`, remplacer en place, « It loads every session with a separate 4,000-character budget ».

### 2.5 IDENTITY.md (1 575 octets)

Champs `- Name:`, `- Creature:`, `- Vibe:`, `- Emoji:`, `- Avatar:` parsés comme lignes `- Label: value`; les textes de remplissage entre parenthèses sont ignorés. `openclaw agents set-identity` copie ce fichier vers la configuration de l'agent; les mises à jour d'identité depuis la Gateway peuvent réécrire `Name`, `Theme`, `Emoji`, `Avatar`.

### 2.6 BOOTSTRAP.md (7 612 octets)

« Birth Sequence » en quatre temps : demander un nom (ne pas en proposer), choisir une « vibe » et un emoji, proposer un avatar (`image_generate` si disponible, sinon sauter), écrire les fichiers d'identité. « The user's request always comes first » : si le premier message demande un vrai travail, le faire d'abord. Le fichier n'est créé que dans un workspace **neuf**.

### 2.7 HEARTBEAT.md et TOOLS.md : retirés

`docs/reference/templates/HEARTBEAT.md` : « OpenClaw no longer creates `HEARTBEAT.md` in new workspaces or reads it at runtime. Monitoring instructions now live in ordinary automation job scratch in the shared state database. » `docs/reference/templates/TOOLS.md` : « Local tool and environment notes now live in the `## Tools` section of `AGENTS.md`. » `openclaw doctor --fix` archive ces fichiers et importe leur contenu.

### 2.8 BOOT.md (833 octets)

Liste de démarrage exécutée une fois par workspace à chaque démarrage de la Gateway, seulement si le hook `boot-md` est activé (`openclaw hooks enable boot-md`). Désactivé par défaut.

### 2.9 Gabarits de rôles (`docs/reference/templates/roles/`)

`presets/team.json` définit un coordinateur et trois spécialistes (`researcher`, `writer`, `reviewer`). Chaque rôle a un `CLAW.md` (en-tête YAML : `agent.id`, `name`, `description`, `identity`, `workspace.bootstrapFiles`) et un `workspace/AGENTS.md`. Le `coordinator/workspace/AGENTS.md` (« Chief of staff operating program ») prescrit : découvrir les spécialistes par `agents_list`, déléguer par `sessions_spawn` (tâche bornée) ou `sessions_send` (suivi), un propriétaire par artefact, « Never let two specialists loop on each other », exiger des artefacts vérifiables, « Approval gates: Never send messages outside the assigned team workflow, publish, purchase, delete, or change production without the human's approval ». Création : `openclaw agents team create` ou `openclaw agents add <id> --role <role>` (`docs/concepts/multi-agent.md`, section « Team preset »). Le câblage de délégation est en configuration : `agents.entries.coordinator.subagents.allowAgents: [...]`, `delegationMode: "prefer"`; les spécialistes ont `allowAgents: []`.

## 3. Format SKILL.md, emplacements, installation

Source : `docs/tools/skills.md`, `docs/tools/creating-skills.md`, `src/skills/`.

### 3.1 En-tête

Minimum : `name` et `description` dans le frontmatter YAML. « OpenClaw follows the AgentSkills spec » (agentskills.io). Clés optionnelles : `homepage`, `user-invocable` (défaut `true`, expose un slash command), `disable-model-invocation` (défaut `false`), `command-dispatch: tool`, `command-tool`, `command-arg-mode`. Bloc `metadata.openclaw` (JSON5 dans le YAML) pour le filtrage : `always`, `emoji`, `os`, `requires.bins`, `requires.anyBins`, `requires.env`, `requires.config`, `primaryEnv`, `install[]`, `skillKey`. L'ancien bloc `metadata.clawdbot` n'est plus lu. `{baseDir}` dans le corps désigne le dossier du skill.

```md
---
name: image-lab
description: Generate or edit images via a provider-backed image workflow
metadata: { "openclaw": { "requires": { "bins": ["uv"], "env": ["GEMINI_API_KEY"] } } }
---
```

### 3.2 Emplacements et priorité (du plus prioritaire au moins)

1. `<workspace>/skills` 2. `<workspace>/.agents/skills` 3. `~/.agents/skills` 4. `<state-dir>/skills` (géré, partagé par tous les agents) 5. `<state-dir>/agents/<agentId>/agent/workshop-skills` (Skill Workshop) 6. skills embarqués (et « Custodian ») 7. `skills.load.extraDirs` et skills de plugins. Découverte jusqu'à 6 niveaux de sous-dossiers; le nom vient du champ `name` (sinon du nom du dossier).

### 3.3 Visibilité par agent

`agents.defaults.skills` = liste de base; `agents.entries.*.skills` **remplace** entièrement cette liste (pas de fusion); `[]` = aucun skill. Omettre la clé = tous les skills.

### 3.4 Configuration et installation

`skills.entries.<name> { enabled, apiKey, env, config }`, `skills.allowBundled`, `skills.load { extraDirs, allowSymlinkTargets, watch }`, `skills.install { preferBrew, nodeManager, allowUploadedArchives }`, `skills.limits { maxCandidatesPerRoot, maxSkillsLoadedPerSource, maxSkillsInPrompt, maxSkillsPromptChars, maxSkillFileBytes }`.
Installation : `openclaw skills install @owner/<slug>` (ClawHub, dans `<workspace>/skills`), `--global` (dans `~/.openclaw/skills`), `git:owner/repo@ref`, `skills-sh:owner/repo/slug`, `./path --as <name>`; `openclaw skills update --all`; `openclaw skills verify`; `openclaw skills check` pour diagnostiquer inventaire / prêt / visible.

## 4. Sous-agents et routage multi-agents

Sources : `docs/tools/subagents.md` et sous-pages (`tool-reference.md`, `nesting.md`, `tool-policy.md`, `announce.md`, `operations.md`), `docs/concepts/multi-agent.md`, `docs/gateway/config-tools/sessions-and-subagents.md`.

- Un sous-agent est une exécution en arrière-plan dans sa propre session `agent:<agentId>:subagent:<uuid>`; il **annonce** son résultat au demandeur (chaîne d'annonce niveau par niveau; le principal reçoit l'annonce finale et répond à l'humain).
- Outil `sessions_spawn` : paramètres `task` (requis), `taskName`, `label`, `agentId` (autre agent si permis par `subagents.allowAgents`), `cwd`, `projectId`, `projectGitUrl`, `runtime: "subagent"|"acp"`, `model`, `runTimeoutSeconds` (0 = sans limite), `thinking`, `thread`, `mode: "run"|"session"`, `cleanup: "delete"|"keep"`, `expectsCompletionMessage`, `completionTarget: "parent"`, `sandbox: "inherit"|"require"`, `context: "isolated"|"fork"` (défaut `isolated` hors fil de discussion), `blind`, `visible`, `placement`, `group`, `worktree`, `worktreeName`, `worktreeBaseRef`. Il **n'accepte aucun paramètre de livraison vers un canal**.
- Modèle du sous-agent : hérite du modèle actif de l'appelant sauf `agents.defaults.subagents.model` ou `agents.entries.*.subagents.model`; `sessions_spawn.model` l'emporte. Un spawn vers un autre agent utilise le modèle configuré de cet agent.
- Disponibilité : `sessions_spawn`, `sessions_yield`, `subagents` sont dans les profils d'outils `coding` et `messaging`, pas dans `minimal`; `full` autorise tout.
- Outils toujours retirés aux sous-agents cachés : `gateway`, `agents_list`, `session_status`, `progress_card`, `cron`, `message`, `sessions_send`, `conversations_*`. À la profondeur maximale, en plus : `subagents`, `sessions_list`, `sessions_history`, `sessions_spawn`.
- Limites : `maxSpawnDepth` défaut 5 (1 = enfants feuilles), `maxChildrenPerAgent` défaut 5, `maxConcurrent` défaut 8 par session appelante, `runTimeoutSeconds` défaut 0.
- `sessions_send` : envoie un message à une autre session ou à un autre agent (`{ "agentId": "ops", "message": "..." }`); accès inter-agents activé par défaut (`tools.agentToAgent.enabled`), restreint par `tools.agentToAgent.allow` et `agents.entries.<id>.tools.agentToAgent.send`; visibilité des sessions par `tools.sessions.visibility: "all"|"tree"|"agent"|"self"`.
- Isolation multi-agents (`docs/concepts/multi-agent.md`) : chaque agent a son workspace, son `agentDir` (`~/.openclaw/agents/<agentId>/agent`) avec `openclaw-agent.sqlite` (profils d'authentification, sessions, transcriptions, état d'index mémoire). « Never reuse `agentDir` across agents ». Partagé : la base d'état `~/.openclaw/state/openclaw.sqlite`, les skills gérés `~/.openclaw/skills`, la configuration. Le workspace est « the default cwd, not a hard sandbox ».
- Routage : les bindings sont évalués par spécificité (pair exact, pair parent, joker de pair, guilde et rôles, guilde, équipe, compte, canal, propriétaire de repli); à égalité, le premier en ordre de configuration. Un seul agent configuré : il résout implicitement; identifiant par défaut `main`, session principale `agent:main:main`.

## 5. Cron (« automations ») et heartbeat

Sources : `docs/automation/cron-jobs.md` et sous-pages, `src/cron/types.ts`, `src/config/zod-schema.root-shape.ts`.

- Le vocabulaire courant est **automations** (`openclaw automations add|edit|list|runs|remove|scratch|run`); `openclaw cron` est l'ancien nom. Les jobs vivent dans la base SQLite partagée, s'exécutent dans le processus Gateway (« The Gateway must be running for schedules to fire »).
- Types de planification : `at` (ISO 8601 ou relatif `20m`), `every` (`10m`, `1h`, `1d`), `cron` (5 ou 6 champs, option `--tz <IANA>`), `on-exit`, `stream`. « Cron expressions without `--tz` use the Gateway host timezone »; les timestamps sans fuseau sont en UTC. Les expressions à la minute 0 avec heure joker sont étalées jusqu'à 5 minutes sauf `--exact`.
- Cible de session (`src/cron/types.ts` ligne 51) : `type CronSessionTarget = "main" | "isolated" | "current" | \`session:${string}\``. Le job porte un champ `agentId?: string` (`src/cron/types.ts`, lignes 285 et 424); CLI : `--agent <id>` et `--clear-agent`.
- Charge utile `agentTurn` : `--message`, `--model`, `--fallbacks`, `--thinking`, `--light-context` (saute l'injection des fichiers de bootstrap), `--tools`, `--skip-if-scratch-empty`, `--include-reasoning`, `--timeout-seconds`; charges `command` et `script` aussi possibles. Budget par défaut : 60 min (agent isolé), 10 min (command), 5 min (script).
- Politiques de surveillance (remplacement du heartbeat) : `activeHours { start, end, timezone: "user"|"local"|IANA }`, `idleOnly: true`, `payload.skipIfScratchEmpty: true`. Exemple officiel (`docs/automation/cron-jobs/schedules.md`) :

```bash
openclaw automations add --name "Project monitor" --every 30m --session isolated \
  --message "Check the job scratch ... otherwise reply NO_REPLY." \
  --active-hours-start 08:00 --active-hours-end 22:00 --active-hours-timezone user \
  --idle-only --skip-if-scratch-empty --no-deliver
```

- Livraison : modes `announce` (envoi puis enregistrement dans la conversation de destination), `webhook`, `none`. Destinations par défaut en cascade : route propre du job > `cron.creators.<profileId>.delivery` > `cron.agents.<agentId>.delivery` > `cron.delivery` > conversation créatrice. Exemple Telegram : `cron: { delivery: { channel: "telegram", to: "-100123456789", threadId: 1 } }`. `NO_REPLY` comme réponse entière = silence.
- Portée par agent et issue GitHub n° 16053 (« Cron jobs should be agent-scoped in multi-agent setups », fermée comme obsolète sans correctif lié) : la demande (champ `agentId` sur le job, outil `cron` restreint à l'agent appelant) correspond à l'état actuel du code : `agentId` sur le job, `--agent` en CLI, `cron.agents.<agentId>` pour les destinations, et jobs créés par un agent plafonnés aux outils de sa session créatrice (`docs/automation/cron-jobs/payloads.md`). Le contenu de l'issue a été lu via la page GitHub; sa date exacte n'était pas visible.
- Heartbeat : voir 1.5. `agents.defaults.userTimezone` existe pour les fenêtres `timezone: "user"`.

## 6. Canaux

### 6.1 Telegram (`docs/channels/telegram.md`, `telegram/setup.md`, `telegram/access-control.md`, `src/config/types.telegram.ts`)

- Jeton : `channels.telegram.botToken` ou `tokenFile`; env `TELEGRAM_BOT_TOKEN` (compte `default` seulement); CLI `openclaw channels add --channel telegram --token <token>`.
- Accès DM : `dmPolicy: "pairing"` (défaut; approbation par `openclaw pairing approve telegram <CODE>`, code valable 1 h) | `"allowlist"` (exige `allowFrom` non vide, **identifiants numériques** Telegram) | `"open"` (exige `allowFrom: ["*"]`) | `"disabled"`. La documentation écrit : « One-owner bots should use `allowlist` with numeric user IDs ».
- Groupes : `groupPolicy: "open"|"allowlist"(défaut)|"disabled"`, `groupAllowFrom`, `groups.<chatId> { requireMention, topics, ... }`, `groups."*"`. Sans entrée `groups`, tous les groupes sont bloqués en mode `allowlist`.
- Commandes : `commands.native: "auto"` enregistre le menu via `setMyCommands`; `commands.nativeSkills`, `customCommands`; `commands.ownerAllowFrom` (clé racine `commands`) désigne le propriétaire pour les commandes réservées et les approbations d'exécution.
- Outils par DM : `channels.telegram.direct.<chatId>.tools`, `toolsBySender`.
- Multi-agents : chaque compte Telegram doit avoir un propriétaire résoluble; binding minimal `{ agentId: "main", match: { channel: "telegram", accountId: "default" } }` dans `bindings[]` (un seul agent résout implicitement).

### 6.2 Interface web (Control UI)

`gateway.controlUi.enabled`, `gateway.bind` (défaut `loopback`), `gateway.port` (18789), `gateway.auth.mode` (`token` généré par l'onboarding; `none` réservé au loopback et non proposé), `gateway.tailscale.mode: "serve"` (tailnet seulement, bind loopback) ou `"funnel"` (public, auth obligatoire), `gateway.auth.allowTailscale`. Tunnel SSH documenté dans `src/gateway/connection-details.ts` : `ssh -N -L 18789:127.0.0.1:18789 user@gateway-host`. Les fichiers du workspace sont éditables dans **Settings → Agents → Files** (limite 2 MiB par fichier).

### 6.3 WhatsApp (`docs/channels/whatsapp.md`)

Plugin externe `@openclaw/whatsapp` (installé à la demande par `openclaw channels add --channel whatsapp`), WhatsApp Web via Baileys, connexion par **QR uniquement** (`openclaw channels login --channel whatsapp`), `dmPolicy: "pairing"` par défaut, `allowFrom: ["+15551234567"]`, `groupPolicy`, `groupAllowFrom`. Demandes d'accès DM : 1 h de validité, 3 en attente maximum par compte.

## 7. Fournisseurs de modèles

Sources : `docs/providers/` (90 pages, une par fournisseur), `docs/concepts/models.md`, `docs/gateway/authentication.md`.

- Référence de modèle : `provider/model` (ex. `anthropic/claude-opus-5`, `claude-cli/claude-haiku-5-5`, `ollama/<model>`, `ollama-cloud/<model>`, `openai/...`, `openrouter/...`). Alias (`haiku`, etc.) et `agents.defaults.models` pour les réglages par modèle.
- Ordre de sélection : `agents.defaults.model.primary` puis `model.fallbacks` dans l'ordre; rotation des profils d'auth à l'intérieur d'un fournisseur avant de passer au fallback. `agents.defaults.modelPolicy.allow` (jokers `provider/*`) limite les modèles; `agents.entries.*.model` et `modelPolicy` remplacent par agent. `utilityModel` (tâches internes courtes), `imageModel`, `pdfModel`, `mediaModels`, `decisionModel`.
- Anthropic : deux routes, **clé API** (`anthropic/*`, « preferable for shared automation or predictable production spend ») ou **Claude CLI** (`claude-cli/*`, réutilise la connexion Claude Code de l'hôte). Configuration par clé : `env.vars.ANTHROPIC_API_KEY` ou `~/.openclaw/.env`, puis `agents.defaults.model.primary: "anthropic/..."`; `openclaw onboard --anthropic-api-key "$ANTHROPIC_API_KEY"`. Un « setup-token » (`sk-ant-oat01-`) reste une voie supportée (`openclaw models auth login --provider anthropic --method setup-token`). `ANTHROPIC_ADMIN_KEY` donne 30 jours de coûts réels dans l'onglet Usage.
- OpenAI : trois méthodes (`docs/providers/openai/authentication.md`) : **Codex login** (OAuth navigateur ou code d'appareil, endpoint `chatgpt.com/backend-api/codex/responses`, consomme l'allocation Codex), **Sign in with ChatGPT (bêta)**, **clé API Platform**.
- Local : Ollama via l'API native `/api/chat` (`models.providers.ollama.baseUrl: "http://host:11434"`, jamais `/v1`), LM Studio, vLLM, SGLang, llama.cpp, etc. Ollama peut aussi servir les embeddings mémoire.
- Routage par tâche : par agent (`agents.entries.*.model`), par sous-agent (`subagents.model`), par job cron (`--model`, `--fallbacks`), par session (`/model`), par tâche utilitaire (`utilityModel`), par fournisseur/modèle pour les outils (`tools.byProvider["provider/model"]`).

## 8. Sécurité

Sources : `docs/tools/permission-modes.md`, `docs/tools/exec-approvals.md`, `docs/gateway/sandboxing.md`, `docs/gateway/security/prompt-injection.md`, `docs/gateway/secrets.md`, `docs/concepts/agent-workspace.md`, `docs/logging.md`, `src/config/zod-schema.agent-runtime.ts`.

- Exécution de commandes : `tools.exec.mode` ∈ `deny | allowlist | ask | auto | full` (résolu en couple `security`/`ask`); `tools.exec.host: "auto"|"sandbox"|"gateway"|"node"`; `tools.exec.safeBins`, `strictInlineEval` (défaut `false`), `reviewer { model, thinking }` pour le mode `auto`. Les approbations sont locales à l'hôte d'exécution (`openclaw approvals get`, `openclaw exec-policy show`). Profils d'outils : `tools.profile: "minimal"|"coding"|"messaging"|"full"`, puis `allow` / `alsoAllow` / `deny`; `tools.elevated` a une porte globale et une porte par agent.
- Sandbox : désactivée par défaut; `agents.*.sandbox.mode`, backends Docker (défaut local), Podman, SSH, OpenShell, Crabbox; « This is not a perfect security boundary ».
- Injection de prompt : le contenu externe (web, courriel, fichiers, hooks) est enveloppé par des marqueurs `<<<EXTERNAL_UNTRUSTED_CONTENT ...>>>` et débarrassé des jetons spéciaux de gabarits de chat; indicateurs de contournement à laisser éteints (`hooks.gmail.allowUnsafeExternalContent`, champ cron `allowUnsafeExternalContent`). La documentation recommande un « reader agent » sans outils pour résumer le contenu non fiable, et insiste : « Model choice matters ».
- Secrets : plaintext possible, **SecretRefs** opt-in (`{ source: "env"|"file"|"exec"|"store", provider, id }`); `openclaw secrets audit --check`. Emplacements hors workspace : `~/.openclaw/openclaw.json`, `~/.openclaw/.env`, `~/.openclaw/credentials/`, `~/.openclaw/agents/<agentId>/agent/openclaw-agent.sqlite` (auth, sessions, transcriptions), `~/.openclaw/state/openclaw.sqlite` (état partagé, jobs), `~/.openclaw/skills/`. La documentation dit de ne **pas** les committer avec le workspace et de mettre le workspace dans un dépôt Git **privé** (section « Git backup (recommended, private) »); les nouveaux workspaces sont `git init` automatiquement si git est présent.
- Journaux : `/tmp/openclaw/openclaw-YYYY-MM-DD.log` (JSON lines, date en fuseau de l'hôte), rotation en `.1.log`; `openclaw logs --follow`; audit de sécurité par `openclaw security audit` (`docs/gateway/security/audit-checks.md`).
- Pairing des nodes : `gateway.nodes.pairing.autoApproveLocal` / `autoApproveCidrs` (voir 1.7).

## 9. Versions

- Version courante au commit lu : **2026.11.1** (`package.json`), publiée en novembre 2026 comme correctif (« This patch release adds no new features »; 83 PR, 67 contributeurs).
- Numérotation : `AAAA.M.N` (année, mois, numéro dans le mois). Rythme observé dans `CHANGELOG/` : octobre 2026 a eu dix versions (2026.10.1 à 2026.10.10); septembre neuf; la 2026.10.5 regroupe « 2,250 pull requests, 411 direct commits, and 637 contributors ». Index : `docs/releases/index.md`; notes complètes : `CHANGELOG/<version>.md`; canaux de mise à jour `update.channel: "stable"|"extended-stable"|"beta"|"dev"`.
- Changements récents touchant les points ci-dessus :
  - 2026.9.7 : migration des réglages `heartbeat`, `llm`, `embeddedPi`, `embeddedHarness`, `sandbox.perSession` retirés (PR 156859); heartbeat devenu job d'automatisation; `HEARTBEAT.md` et `TOOLS.md` retirés des gabarits.
  - 2026.8.1 : `modelPolicy.allow` explicite remplace les restrictions implicites par carte de modèles.
  - `agents.list` (tableau) remplacé par `agents.entries` (objet) avec `agents.ownership: "explicit"` en multi-agents; `openclaw doctor --fix` migre.
  - Pré-juillet 2026 : `metadata.clawdbot` dans SKILL.md n'est plus lu; fichiers sidecar `openclaw-workspace-state.json` remplacés par SQLite; la fenêtre de migration exige de passer par 2026.9.7 pour les installations antérieures à juillet.
  - 2026.10.x : série de correctifs de sécurité (10.6, 10.8, 10.9), « Chat Details » et « Advisor » (10.10).
  - Éléments dépréciés ou retirés encore cités dans de vieux tutoriels : `agents.defaults.heartbeat`, `HEARTBEAT.md`, `TOOLS.md`, `agents.list`, `auth-profiles.json` (importé en SQLite), réglages Telegram `timeoutSeconds`, `mediaGroupFlushMs`, `retry`.

## 10. Prérequis système

- Node.js : `package.json` → `"engines": { "node": ">=24.16.0 <25 || >=26.1.0" }`. `docs/install/node.md` : Node 26 recommandé (26.12.1+), 24.22.1+ accepté; Node 22, 23, 25 **non supportés**; exigence d'une SQLite liée « WAL-reset-safe ».
- OS : macOS, Linux, Windows (Windows Hub natif, PowerShell, ou Gateway sous WSL2). Installateur : `curl -fsSL https://openclaw.ai/install.sh | bash`, ou `npm install -g openclaw@latest --allow-scripts=openclaw`; `pnpm` seulement pour compiler depuis les sources; `openclaw onboard --install-daemon` installe le service (systemd/launchd; Scheduled Tasks sur Windows).
- Mémoire : `docs/install/hetzner.md` et `docs/install/docker.md` : « at least 6 GB RAM for a local source image build »; les images préconstruites évitent cette exigence. Aucun minimum chiffré n'est documenté pour l'exécution seule (voir section 12).
- Docker : `Dockerfile` et `docker-compose.yml` à la racine (image `${OPENCLAW_IMAGE:-openclaw:local}`); Docker est aussi le backend de sandbox par défaut quand la sandbox est activée; sans sandbox, Docker n'est pas requis.
- Fichiers : configuration `~/.openclaw/openclaw.json` (JSON5), état `~/.openclaw/`, variables `OPENCLAW_CONFIG_PATH`, `OPENCLAW_STATE_DIR`, `OPENCLAW_GATEWAY_PORT`, `OPENCLAW_PROFILE`.

## 11. Conséquences factuelles pour notre cahier de construction

Correspondances entre le cahier (`cahier-de-construction.md` v1.1) et les mécanismes réels, sans recommandation.

| Point du cahier | État dans le code et les gabarits |
|---|---|
| Section 3.1 « Node.js 22+ » | Faux aujourd'hui : Node 22 est non supporté; il faut 24.16+ (24.x) ou 26.1+. |
| Section 3.2 `openclaw onboard --install-daemon` | Existe (`docs/cli/onboard.md`). |
| Section 3.3 Anthropic par clé API | Correspond à la route `anthropic/*` avec `ANTHROPIC_API_KEY` (env ou `~/.openclaw/.env`) et `agents.defaults.model.primary`. Le plafond mensuel n'a pas d'équivalent dans OpenClaw (côté console Anthropic); `ANTHROPIC_ADMIN_KEY` permet d'afficher les coûts réels. |
| Section 3.4 Telegram restreint à Dominic, aucun groupe | `channels.telegram.dmPolicy: "allowlist"` + `allowFrom: ["<id numérique>"]`, `groupPolicy: "allowlist"` sans entrée `groups` = tous les groupes bloqués. `commands.ownerAllowFrom: ["telegram:<id>"]` pour les commandes réservées. |
| Section 3.5 workspace = clone de `NetBLB/ceo` | Correspond à `agents.entries.ceo.workspace` (ou `agents.defaults.workspace`); la documentation recommande elle-même un dépôt Git privé pour le workspace. `agents.defaults.skipBootstrap: true` évite que l'onboarding régénère les fichiers. `BOOTSTRAP.md` n'est créé que dans un workspace neuf; un clone déjà peuplé n'en recevra pas. |
| Section 3.7 « Sandbox : configuration par défaut » | La sandbox par défaut est **off**; la limitation des commandes passe par `tools.exec.mode` (`allowlist`, `ask` ou `auto`) et les approbations, pas par la sandbox. |
| Section 3.8 `nodes.autoApprove: false` | Clé inexistante; équivalents : `gateway.nodes.pairing.autoApproveLocal: false` et `autoApproveCidrs` vide. |
| Section 4 « Un seul agent `agents.entries.ceo` » | Correspond; l'identifiant `ceo` est valide. Avec une seule entrée, `ownership: "explicit"` n'est pas exigé; il le devient au deuxième agent. |
| Section 4 « modèle des sous-agents classe intermédiaire, profondeur 1 » | `agents.defaults.subagents.model` (ou `agents.entries.ceo.subagents.model`) et `agents.defaults.subagents.maxSpawnDepth: 1`. `maxChildrenPerAgent` (défaut 5) et `maxConcurrent` (défaut 8) existent aussi. |
| Section 4 « Heartbeat activé, 30 minutes, prompt de clôture » | **Pas de clé heartbeat.** L'équivalent réel est un job : `openclaw automations add --every 30m --session isolated --idle-only --message "<prompt>" --no-deliver` (éventuellement `--active-hours-*`); le prompt vit dans le job, pas dans `HEARTBEAT.md`. Le cahier compte donc trois jobs, pas deux plus un heartbeat. |
| Section 4 Cron 1 et 2 (lundi 07:00, 22:00 America/Toronto, Telegram) | `openclaw automations add --cron "0 7 * * 1" --tz America/Toronto --session isolated --agent ceo --message "..."` avec livraison `announce` vers `channel: telegram`, `to: <id Dominic>`, ou `cron.agents.ceo.delivery` comme défaut. Le silence conditionnel se fait par `NO_REPLY`. `--light-context` existe si les fichiers de bootstrap ne doivent pas être injectés. |
| Section 4 Mémoire : recherche et dreaming | `memory.search.provider` (OpenAI par défaut, Ollama ou GGUF local possibles); dreaming sous `plugins.entries.memory-core.config.dreaming` (actif par défaut, `0 3 * * *`, et il crée **lui-même un job cron**, ce qui touche la règle « aucune automatisation ajoutée » de la section 0.4). |
| Section 4 « sauvegarde de mémoire avant compaction » | `agents.defaults.compaction.memoryFlush.enabled` (actif par défaut). |
| Section 5 structure du dépôt (`AGENTS.md`, `SOUL.md`, `IDENTITY.md`, `USER.md`, `MEMORY.md`) | Les cinq fichiers correspondent aux fichiers de bootstrap réels et sont injectés dans cet ordre, avec `USER.md` plafonné à 4 000 caractères et `MEMORY.md` chargé seulement en session principale. `PASSATION.md`, `organisation.md`, `projets/`, `departements/`, `commun/`, `journal/`, `outils/` n'ont **aucun statut particulier** pour OpenClaw : ce sont des fichiers ordinaires lus par les outils de fichiers ou par `memory_search` (si `memory.search.extraPaths` les inclut), jamais injectés automatiquement. |
| Section 5.1 AGENTS.md « adapter au gabarit officiel » | Le gabarit officiel attend des sections `## Tools` (notes locales, ex-TOOLS.md), `## Memory` et `## Automations`; les notes de bas de page `memory/YYYY-MM-DD.md` sont le journal quotidien attendu. |
| Section 5.7 `departements/<nom>/AGENTS.md` | Pas d'équivalent : un seul `AGENTS.md` par workspace est injecté. Un AGENTS.md de département ne devient actif que si un agent distinct (`agents.entries.<dept>` avec son propre workspace, ou gabarit de rôle `--role`) le porte, ou si le CEO le lit lui-même et le passe dans `task` de `sessions_spawn`. |
| Section 5.9 skills `validateur`, `gmail-lecture`, `github` | Format réel : dossier `skills/<nom>/SKILL.md` avec `name` et `description`. Le « validateur indépendant » n'est pas un skill dans le modèle d'OpenClaw mais plutôt un sous-agent (`sessions_spawn` en `context: "isolated"`) ou un agent `reviewer` du preset d'équipe. Pour Gmail, OpenClaw a un plugin IMAP bundle (`docs/automation/imap.md`) qui **déclenche** des sessions à l'arrivée de courriels (pas de lecture à la demande) et dont la documentation prescrit un agent lecteur isolé; un skill IMAP de lecture à la demande n'est pas fourni par le dépôt. GitHub : `gateway.github` et les worktrees gérés existent; le `gh` CLI dans l'environnement est une voie ordinaire via `exec`. |
| Section 7 « aucune autre tâche planifiée » | Trois mécanismes en créent sans action explicite : dreaming (memory-core), le job de maintenance mémoire suggéré par le gabarit AGENTS.md (non créé automatiquement), et la migration doctor d'anciens heartbeats (sans objet ici). |
| Section 8 « interface inaccessible depuis l'IP publique » | `gateway.bind: "loopback"` (défaut) + `gateway.auth.mode: "token"`; Tailscale par `gateway.tailscale.mode: "serve"`; tunnel SSH documenté. |
| Validateur, boucle d'évaluation, `evaluations.md` | Pas d'équivalent natif; le preset d'équipe fournit un rôle `reviewer` et le gabarit coordinateur exige des « verifiable artifacts », ce qui est le mécanisme le plus proche. |
| Serveur 4 Go de RAM | Aucune exigence chiffrée pour l'exécution dans la documentation; 6 Go cités seulement pour construire l'image Docker depuis les sources. |

## 12. Non lu ou incertain

- Je n'ai pas exécuté OpenClaw; les valeurs par défaut citées viennent des commentaires du schéma et de la documentation embarquée, pas d'un test.
- La date et les commentaires détaillés de l'issue 16053 proviennent d'une lecture de la page GitHub par outil (API GitHub inaccessible depuis cette session); la date de création n'était pas visible.
- Je n'ai pas lu le code de `src/cron/` au delà des types, ni `src/skills/` au delà de la configuration; les limites de skills (`maxSkillsInPrompt`, etc.) n'ont pas de défaut trouvé dans `src/skills/config/`.
- Les fichiers réellement copiés par `openclaw onboard` viennent d'un paquet de gabarits (« packaged templates ») que je n'ai pas localisé dans le dépôt; j'ai supposé qu'ils correspondent à `docs/reference/templates/` et à `docs/reference/AGENTS.default.md`.
- Mémoire vive minimale pour un VPS exécutant seulement la Gateway : non documentée; `docs/platforms/linux.md` parle de pression mémoire et d'OOM sans chiffre lu.
- Le plugin IMAP (`docs/automation/imap.md`) a été lu en tête seulement; son mode exact (IDLE, filtrage d'expéditeurs, lecture seule) mérite une lecture complète avant l'étape 7 du plan.
- `gateway.github` et `repositoryIntegration` (schéma lu, documentation non lue) : portée réelle pour les commits d'un agent non vérifiée.
- Le comportement précis de `agents.defaults.skipBootstrap` sur un workspace cloné déjà complet (création ou non de `BOOTSTRAP.md`) n'a pas été vérifié dans le code.

## 13. Sources

Toutes au commit `ba400bcb` de `https://github.com/openclaw/openclaw` :

- `package.json` (version, engines)
- `src/config/zod-schema.root-shape.ts`, `zod-schema.agents.ts`, `zod-schema.agent-entry-base.ts`, `zod-schema.agent-defaults-base.ts`, `zod-schema.agent-defaults.ts`, `zod-schema.agent-defaults.test.ts`, `zod-schema.agent-runtime.ts`, `zod-schema.agent-model.ts`, `zod-schema.gateway.ts`, `zod-schema.root-support.ts`, `zod-schema.memory-search.ts`
- `src/agents/workspace-bootstrap-policy.ts`, `src/agents/embedded-agent-helpers/bootstrap.ts`, `bootstrap-limits.ts`, `src/memory/root-memory-files.ts`
- `src/cron/types.ts`; `src/gateway/server-start.ts`, `connection-details.ts`
- `docs/reference/templates/{AGENTS,SOUL,USER,IDENTITY,BOOTSTRAP,BOOT,HEARTBEAT,TOOLS}.md`, `docs/reference/templates/roles/**`, `docs/reference/AGENTS.default.md`
- `docs/concepts/agent-workspace.md`, `multi-agent.md`, `memory.md`, `dreaming.md`, `models.md`, `storage-locations.md`
- `docs/tools/skills.md`, `creating-skills.md`, `subagents.md`, `subagents/{tool-reference,nesting,tool-policy}.md`, `exec-approvals.md`, `permission-modes.md`
- `docs/automation/cron-jobs.md`, `cron-jobs/{schedules,how-it-works,delivery,payloads,managing-jobs}.md`, `docs/automation/imap.md`
- `docs/channels/telegram.md`, `telegram/{setup,access-control,messaging}.md`, `docs/channels/whatsapp.md`
- `docs/providers/anthropic.md`, `openai/authentication.md`, `ollama.md`; `docs/gateway/authentication.md`, `config-gateway.md`, `heartbeat.md`, `sandboxing.md`, `secrets.md`, `logging.md`, `security/prompt-injection.md`, `config-tools/sessions-and-subagents.md`
- `docs/install/index.md`, `node.md`, `hetzner.md`, `docker.md`; `docs/cli/onboard.md`
- `CHANGELOG.md`, `CHANGELOG/{2026.11.1,2026.10.10,2026.10.5,Unreleased}.md`, `docs/releases/index.md`, `docs/releases/2026.9.7.md`
- Issue GitHub : `https://github.com/openclaw/openclaw/issues/16053` (lue via la page web)
