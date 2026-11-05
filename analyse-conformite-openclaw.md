# Analyse de conformité OpenClaw du projet

Dernière mise à jour : 2026-11-05
Statut : version 1.0, **recommandations** (R) en attente de validation par Dominic; rien n'a été modifié dans les documents validés (architecture v1.4, principes v1.3, plan et cahier v1.1). Après validation, les corrections seront appliquées fichier par fichier (section 12).

Bases utilisées (dossier `recherche/`) : 01 documentation officielle (97 pages, 2026-11-05), 05 code source (dépôt au commit ba400bcb, version 2026.11.1), 02 cas d'usage publiés (31 cas), 03 retours d'expérience (19 vidéos, 34 sources écrites), 04 écosystème immédiat. Les références entre crochets renvoient à ces fichiers (ex. [01 §5.2], [02 idée 6], [03 leçon 4]).

## 1. Verdict global

**L'architecture est conforme à la façon OpenClaw et correspond à ce que font les utilisateurs qui tiennent dans la durée.** Un seul agent sur un serveur, interface privée et Telegram, workspace en dépôt Git privé, mémoire en fichiers Markdown, tâches planifiées pour les rituels, approbation humaine avant toute action externe : c'est le patron documenté par OpenClaw [01 §11] et celui des références les plus solides (VelvetShark 50 jours, Berman, Subramaniam, Sarver, Nervegna) [02 §4, 03 §4]. L'organigramme CEO + départements a un équivalent officiel : le préréglage « équipe » (coordinateur + chercheur + rédacteur + relecteur) [01 §5.1, 05 §2.9].

**Le cahier de construction, lui, doit être corrigé sur une douzaine de points techniques** : il a été écrit avec le vocabulaire de la documentation d'il y a quelques mois (heartbeat, HEARTBEAT.md, Node 22, `nodes.autoApprove`) et avec trois hypothèses fausses sur le fonctionnement interne : les sous-dossiers du workspace ne sont pas lus automatiquement, un sous-agent ne reçoit que `AGENTS.md`, et une tâche planifiée ne peut pas attendre un GO. Aucune de ces corrections ne change l'architecture; toutes changent la façon de la construire.

**Trois décisions nouvelles sont à prendre** (section 11) : le réveil d'inactivité, la consolidation nocturne de mémoire (Dreaming), et le fournisseur d'embeddings pour la recherche mémoire.

## 2. Grille de conformité

Verdicts : **Conforme** (tel quel), **À reformuler** (même intention, autre mécanisme), **À corriger** (faux ou inexistant), **À compléter** (prévu mais insuffisant), **À décider** (choix ouvert).

| # | Élément prévu (document) | Réalité OpenClaw et terrain | Verdict | Voir |
|---|---|---|---|---|
| G-1 | Serveur Linux, Gateway unique, service, loopback + Tailscale ou SSH (archi §5, cahier §2 et §8) | Patron officiel VPS; `gateway.bind: "loopback"` par défaut, `auth.mode: "token"`, `gateway.tailscale.mode: "serve"`; minimum documenté 1 vCPU / 1 Go, recommandé 2 Go+ [01 §10.7]. 4 Go confortable [03 leçon 11] | Conforme | |
| G-2 | Node.js 22+ (cahier §3.1) | `package.json` exige `>=24.16 <25` ou `>=26.1`; Node 26 recommandé; 22 non supporté [05 §10] | À corriger | R-1 |
| G-3 | Un seul agent `ceo` (archi, cahier §4) | `agents.entries.ceo` (objet indexé, pas de tableau); `ownership: "explicit"` exigé seulement à partir du deuxième agent [05 §1.1] | Conforme | |
| G-4 | Workspace = clone du dépôt `ceo`; fichiers AGENTS/SOUL/IDENTITY/USER/MEMORY | Fichiers de bootstrap officiels, injectés dans cet ordre; limites 20 000 caractères par fichier, 60 000 au total, USER.md plafonné à 4 000; MEMORY.md en session principale seulement; `agents.defaults.skipBootstrap: true` pour un workspace géré soi-même; dépôt Git privé recommandé par la documentation [01 §3, 05 §2.1] | Conforme, à compléter (tailles, skipBootstrap) | R-2 |
| G-5 | `organisation.md`, `projets/`, `departements/`, `commun/`, `journal/`, `PASSATION.md` | Fichiers ordinaires sans statut pour OpenClaw : jamais injectés; lus par les outils de fichiers quand le CEO le décide; indexables par `memory_search` seulement via `memory.search.extraPaths` [01 §3.2, §4.3] | À reformuler | R-3 |
| G-6 | Un `AGENTS.md` par département, « activé » à la demande (cahier §5.7) | Un seul `AGENTS.md` injecté par workspace; un sous-agent ne reçoit que celui du workspace cible et le texte de `task`; un `AGENTS.md` de département n'agit que (a) passé intégralement dans `task`, (b) porté par un agent distinct avec son propre workspace, ou (c) injecté par le hook `bootstrap-extra-files` (tous ceux qui correspondent au motif, dans les budgets) [01 §3.2, §5.2; 05 §4; 02 idée 6; 03 leçon 3] | À reformuler | R-4 |
| G-7 | Heartbeat 30 min pour la clôture par inactivité (archi v1.3, cahier §4) | La clé `heartbeat` et `HEARTBEAT.md` sont retirés (2026.9.7); équivalent : un job d'automatisation `--every 30m --idle-only --active-hours ... --no-deliver` avec « scratch »; l'installation crée elle-même un job « Proactive check » 30 min [05 §1.5, 01 §6.2]; coût 0,47 à 0,75 $ par réveil sur un gros modèle [03 §6] | À reformuler et à décider | R-5, Q-A |
| G-8 | Deux crons : lundi 07:00 et 22:00 (cahier §7) | `openclaw automations add --cron "0 7 * * 1" --tz America/Toronto --session isolated --agent ceo`; livraison Telegram par `cron.agents.ceo.delivery` ou `--announce`; `NO_REPLY` pour le silence; décalage jusqu'à 5 min sur les heures pleines sauf `--exact`; alertes d'échec natives après 2 échecs, auto-désactivation après 10 [01 §6.1, 05 §5] | Conforme, à compléter | R-6 |
| G-9 | « Aucune automatisation ajoutée à l'installation » (D-010, cahier §0.4 et §7) | Trois jobs existent par défaut ou sont suggérés : « Proactive check » (créé à la configuration), Dreaming (`0 3 * * *`, actif par défaut, crée son propre job cron), maintenance mémoire (suggérée par le gabarit) [05 §1.8, 01 §4.5, §6.2] | À compléter et à décider | R-7, Q-B |
| G-10 | « Rien d'envoyé, publié, dépensé sans GO » (P-20, AGENTS.md du CEO) | Dans la session interactive : faisable par la règle écrite plus `tools.deny`, `tools.exec.mode`. Dans un run planifié : impossible d'attendre un GO (« explicitly unattended »); les cartes d'approbation ne vont qu'aux clients d'approbation, pas aux canaux [01 §6.1, §10.3]. Incident documenté : règle donnée seulement en chat, perdue à la compaction, courriels supprimés [02 anti-patron 1] | À compléter | R-8 |
| G-11 | Sandbox « configuration par défaut » (cahier §3.7) | Sandbox désactivée par défaut; la limitation des commandes passe par `tools.exec.mode` (`deny / allowlist / ask / auto / full`) et par `tools.profile` [05 §1.6, §8] | À corriger | R-9 |
| G-12 | `nodes.autoApprove: false` (cahier §3.8) | Clé inexistante; réel : `gateway.nodes.pairing.autoApproveLocal: false`, `autoApproveCidrs: []` [05 §1.7] | À corriger | R-10 |
| G-13 | Modèle Anthropic par clé API; classe supérieure pour le CEO, intermédiaire pour les sous-agents; plafond dans la console (archi §5, cahier §3.3) | Route `anthropic/*` recommandée par la documentation pour un serveur; `agents.defaults.model.primary` + `fallbacks`; `agents.defaults.subagents.model`; `utilityModel` pour les tâches internes; aucun plafond natif [01 §7, 05 §7]. Terrain : modèle intermédiaire par défaut, économique pour les réveils, supérieur à la demande; plafond chez le fournisseur = seul disjoncteur [03 conseils 1 et 2] | Conforme, à compléter (routage) | R-11 |
| G-14 | Telegram limité à Dominic, aucun groupe (cahier §3.4) | `dmPolicy: "allowlist"` + `allowFrom` avec identifiant **numérique**; `groupPolicy: "allowlist"` sans `groups` = tous les groupes bloqués; `commands.ownerAllowFrom` pour les commandes réservées [05 §6.1] | Conforme, à préciser | R-12 |
| G-15 | Gmail : skill IMAP lecture seule (cahier §5.9) | Pas de skill de lecture à la demande dans le dépôt; plugin IMAP bundlé = **déclencheur** à l'arrivée de courriels avec `allowedSenders` obligatoire et patron « agent lecteur isolé »; pour la recherche à la demande : skill `gog` (Google Workspace, auteur d'OpenClaw) ou skill IMAP tiers lu avant installation [01 §10.4, 04 §A, 05 §11] | À reformuler | R-13 |
| G-16 | GitHub : skill `github` (cahier §5.9) | Identité GitHub managée `tools.github` (PAT ou OAuth, jeton jamais dans la config) et outil `github_publish` qui « never merges on its own »; `gh` CLI possible via `exec`; skill `github` de steipete [01 §10.4, 04 §A] | À reformuler | R-14 |
| G-17 | Validateur comme skill (cahier §5.9) | Un skill ne crée pas d'isolation; le validateur indépendant est un sous-agent `context: "isolated"` ou un agent `reviewer` du préréglage équipe; le gabarit coordinateur exige des « artefacts vérifiables » [01 §5.2, 05 §11, 02 idée 8] | À reformuler | R-15 |
| G-18 | Mémoire : recherche et Dreaming « selon la documentation » (cahier §4) | Embeddings OpenAI par défaut (deuxième fournisseur et clé); alternatives Ollama, GGUF local, ou `none` (mots-clés seulement) [01 §4.3]; Dreaming écrit dans MEMORY.md la nuit; mémoire empoisonnée par des modèles bon marché documentée [02 anti-patron 3] | À décider | Q-B, Q-C |
| G-19 | Fiches de projet (gabarit cahier §5.6) | Les cas aboutis ajoutent : propriétaire, `blocked_by`, prochaines actions, catégorie « en panne », et une sortie standard « décisions en attente » dans le bilan [02 idées 3, 25] | À compléter | R-16 |
| G-20 | Boucle d'évaluation des sous-agents (P-32) | Pas de fonction native; les stats d'annonce (durée, jetons, coût) et `sessions_history` fournissent la matière; `cleanup: "keep"` conserve la session enfant; « règle des 3 » et vérification de livraison chez les utilisateurs [01 §5.2, 02 idées 7, 9] | Conforme (par fichiers), à compléter | R-17 |
| G-21 | Mises à jour d'OpenClaw « suivies » (cahier §8) | 2 à 3 versions par semaine; première cause de panne silencieuse des crons; utilisateurs expérimentés : épingler, sauvegarder, tester les crons après chaque mise à jour [01 §1, 03 leçon 6, conseils 11 et 12] | À compléter | R-18 |
| G-22 | Sauvegardes (cahier §2) | `openclaw backup --to`, état et workspace sauvegardés séparément; `.gitignore` officiel; scan des secrets avant commit chez les utilisateurs [01 §3.4, 04 §F, 03 conseil 10] | Conforme, à compléter | R-19 |
| G-23 | Identités publiques séparées par marque (P-3) | Un agent = une identité; multi-compte par canal avec bindings par compte; comptes dédiés pour le bot recommandés [01 §12] | Conforme pour le jour 1 (le CEO n'écrit vers aucun public) | |
| G-24 | PC Windows comme node plus tard (P-28) | Node host headless Windows existe; `system.run` sous liste blanche; les nodes sont des périphériques du Gateway [01 §2.6] | Conforme | |
| G-25 | Paperclip plus tard (D-014) | Absent de la documentation OpenClaw; adaptateur « Coming soon » dans l'interface, ruptures bimensuelles, inutile sous 3 à 5 agents selon toutes les sources [04 §C] | Conforme (retardé) | |
| G-26 | Tableau de bord : page `organigramme.html` générée, option communautaire (archi §5) | La moitié des tableaux de bord communautaires a cessé d'évoluer; la Control UI officielle suffit pour un agent [04 §B] | Conforme | |
| G-27 | Import des projets Claude et ChatGPT | Import officiel de mémoire depuis Claude Code, Codex et Hermes seulement; rien pour les projets Claude.ai ou ChatGPT : l'import reste manuel (phase 6, étape 2) [01 §4.7] | Conforme (manuel) | |

## 3. Corrections à apporter aux documents (R-1 à R-19)

Chaque point : ce qu'il faut changer, pourquoi, où.

- **R-1 Node 24 ou 26.** Cahier §3.1 : « Node.js 24.16+ ou 26.1+ (26 recommandé) ». [05 §10]
- **R-2 Tailles et bootstrap.** Cahier §5 : `AGENTS.md` du CEO sous 20 000 caractères (le texte actuel fait environ 6 000, marge correcte), `USER.md` sous 4 000 (le contenu prévu dépasse si on y met les canaux et les horaires : déplacer vers `commun/identite.md` ce qui n'est pas une directive), `MEMORY.md` court (moins de 100 lignes, patron des cas aboutis). Ajouter `agents.defaults.skipBootstrap: true` puisque le workspace arrive déjà rempli; supprimer `BOOTSTRAP.md` s'il est généré. Adopter le format officiel de `USER.md` (une directive par ligne, impératif, métadonnée de date et statut). [01 §3.3, §3.5; 05 §2.1; 02 idée 1]
- **R-3 Indexer les dossiers du dépôt.** Cahier §4 : `memory.search.extraPaths` doit inclure `projets/`, `commun/`, `departements/`, `journal/`, `organisation.md`, `PASSATION.md`, sinon le CEO ne les retrouve que s'il sait quel fichier ouvrir. Dans `AGENTS.md` du CEO, écrire explicitement : « L'état des projets est dans projets/<nom>.md; ouvre la fiche avant de répondre sur un projet; utilise memory_search pour retrouver un projet par son sujet. » [01 §4.3]
- **R-4 Le brief de département passe dans la tâche.** Cahier §5.7 et §5.1 (section Délégation) : quand le CEO délègue, il **lit** `departements/<nom>/AGENTS.md` et le **colle intégralement** dans le paramètre `task` de `sessions_spawn`, avec : la règle d'autonomie (P-20), le livrable attendu, le format de retour vérifiable (fichiers écrits, sources, « source indisponible » plutôt qu'invention), le délai (`runTimeoutSeconds`). Gabarit de brief à cinq champs : question, contexte, livrable, échéance, budget [02 idée 5]. Option à garder pour plus tard : quand une équipe devient permanente, la créer comme agent distinct (`openclaw agents add <dept> --role ...`) avec son workspace, câblée par `subagents.allowAgents` et `delegationMode: "prefer"`, comme le préréglage équipe [05 §2.9]. [01 §5.2, 03 leçon 3]
- **R-5 Réveil d'inactivité : vocabulaire et décision.** Archi §5 et §6, cahier §4 : remplacer « heartbeat » par « tâche d'automatisation »; voir Q-A pour le choix du mécanisme et du modèle. [05 §1.5, 01 §6.2]
- **R-6 Crons : écriture exacte et alertes.** Cahier §7 : donner les commandes réelles (`openclaw automations add --name "Bilan du lundi" --cron "0 7 * * 1" --tz America/Toronto --exact --session isolated --agent ceo --message "..." --announce --channel telegram --to <id>`), configurer `cron.agents.ceo.delivery` et `failureAlert` (natif : annonce après 2 échecs), et ajouter un **cron de contrôle** hebdomadaire qui vérifie que les autres ont tourné (`openclaw automations runs`) [02 idée 12]. `--session isolated` pour les jobs, `thinking` bas, timeout d'au moins 180 s [03 conseil 12]. [01 §6.1]
- **R-7 Inventorier les automatisations par défaut.** Cahier §7 : à l'installation, lister `openclaw automations list --all`, désactiver ou régler le « Proactive check », décider du Dreaming (Q-B), et consigner tout dans `reglages.md` (R-20). La règle D-010 devient : « aucune automatisation **au-delà de celles listées dans reglages.md** ». [05 §1.8, 01 §6.2]
- **R-8 Le GO imposé par la configuration, pas seulement par la phrase.** Cahier §4 et §8 : pour l'agent `ceo`, `tools.deny: ["message"]` tant qu'aucun envoi n'est voulu (la livraison des crons passe par `delivery`, pas par l'outil `message`), `tools.exec.mode: "allowlist"` (ou `ask` en session interactive) avec une liste blanche limitée à `git`, `gh` lecture, outils de fichiers; `tools.agentToAgent.enabled: false` tant qu'il n'y a qu'un agent; interdiction écrite au CEO de modifier `openclaw.json` ou ses propres règles sans GO [02 idée 28]. Les jobs planifiés reçoivent `--tools` restreint (pas d'`exec`, pas de `message`). La phrase reste dans `AGENTS.md` (jamais seulement en conversation) [02 idée 2]. [01 §10.2, §10.3]
- **R-9 Sandbox.** Cahier §3.7 : écrire « sandbox désactivée (défaut); la limitation passe par `tools.exec.mode` et `tools.profile` »; sandbox Docker `non-main` envisagée seulement si un lecteur de courriel isolé est mis en place (R-13). [05 §1.6, 01 §10.4]
- **R-10 Nodes.** Cahier §3.8 : `gateway.nodes.pairing.autoApproveLocal: false`, `autoApproveCidrs: []`. [05 §1.7]
- **R-11 Routage de modèles.** Cahier §3.3 et §4 : `agents.entries.ceo.model.primary` = modèle Anthropic de classe intermédiaire (Sonnet) par défaut, `fallbacks` vers la classe supérieure; CEO autorisé à monter en classe supérieure (`/model` ou `thinking`) pour les synthèses à enjeu; `agents.defaults.subagents.model` = intermédiaire; `utilityModel` = économique (Haiku); jobs planifiés `--model` économique sauf le bilan du lundi (intermédiaire). Interdire aux modèles économiques d'écrire dans `MEMORY.md` (mémoire empoisonnée) [02 anti-patron 3]. Plafond dans la console Anthropic **avant** le premier message, plus `ANTHROPIC_ADMIN_KEY` pour voir les coûts réels dans l'onglet Usage. Alerte explicite quand le plafond est atteint (sinon l'agent se tait) [02 idée 13]. [01 §7, 03 §6]
- **R-12 Telegram.** Cahier §3.4 : `channels.telegram.dmPolicy: "allowlist"`, `allowFrom: ["<id numérique de Dominic>"]` (obtenu par `/whoami@<bot>`), `groupPolicy: "allowlist"` sans `groups`, `commands.ownerAllowFrom: ["telegram:<id>"]`. [05 §6.1]
- **R-13 Gmail.** Cahier §5.9 et plan étape 7 : deux voies, à choisir à l'activation de l'équipe Services : (a) lecture à la demande par un skill de source connue (`gog` de l'auteur d'OpenClaw, portées OAuth limitées à la lecture) ou un skill IMAP lu ligne à ligne avant installation; (b) plugin IMAP bundlé comme déclencheur avec `allowedSenders` (liste de clients) et agent lecteur isolé (profil `minimal`, sans outils, résume sans citer). La règle « résumer plutôt que citer » entre dans `AGENTS.md`. Rien n'est installé au jour 1. [01 §10.4, 04 §A, 03 leçon 14]
- **R-14 GitHub.** Cahier §3.6 et §5.9 : utiliser l'identité GitHub managée (`tools.github`, PAT à portée minimale, jeton hors config) et `github_publish` (commits et PR, jamais de fusion), plutôt qu'un skill maison; `gh` en lecture pour les autres dépôts. [01 §10.4]
- **R-15 Validateur.** Cahier §5.9 : le validateur est un **sous-agent** lancé par le CEO avec `context: "isolated"`, modèle intermédiaire, brief fixe (« tu ne vois que le livrable et les règles jointes; tu n'inventes aucune consigne; verdict GO / NO GO avec BLOQUANT / IMPORTANT / MINEUR »), pas un skill. Le `SKILL.md` prévu devient le texte du brief, stocké dans `departements/validateur/brief.md`. [01 §5.2, 02 idée 8]
- **R-16 Fiches de projet.** Cahier §5.6 : ajouter `Bloqué par`, `Propriétaire` (Dominic ou CEO ou équipe), `Prochaines actions` (liste courte), statut `en panne` (bloqué sans action possible) ; le bilan du lundi sort toujours les sections « en panne » et « décisions en attente de Dominic ». [02 idées 3, 25]
- **R-17 Boucle d'évaluation.** Cahier §5.1 : ajouter la « règle des 3 » (une correction répétée trois fois devient une leçon dans `lecons/` ou une proposition de modification du brief, soumise à Dominic) et la vérification de livraison (« fait » seulement quand Dominic a reçu le résultat; preuves : fichiers, URL, sources). Les sous-agents sont lancés avec `cleanup: "keep"` pendant la phase d'évaluation pour pouvoir relire leur session. [02 idées 7, 9; 03 leçon 7]
- **R-18 Mises à jour.** Cahier §8 et `PASSATION.md` : désactiver la mise à jour automatique, `update.channel: "extended-stable"` (canal LTS) ou version épinglée, sauvegarde avant, **test de fumée après** (Telegram répond, `openclaw automations list`, un run forcé du filet du soir, fournisseur de modèle), lecture du changelog avant toute mise à jour; mise à jour mensuelle planifiée par Dominic, pas par le CEO. [03 conseils 11 et 12, 01 §1]
- **R-19 Sauvegardes et secrets.** Cahier §2 et §8 : `openclaw backup --to <destination>` hebdomadaire; `.gitignore` officiel plus `memory/imports/` si sensible; scan des secrets (gitleaks ou `openclaw secrets audit --check`) avant chaque push du dépôt `ceo`; SecretRefs pour la clé API, le jeton Telegram et le mot de passe Gmail. [01 §3.4, §10.5; 04 §F]
- **R-20 Fichier `reglages.md`** (demande de Dominic du 2026-11-05) : dans le dépôt `ceo`, un fichier qui liste chaque réveil (nom, fréquence, heures actives, modèle, actif ou non, ce qu'il fait, dernière exécution vérifiée), y compris ceux créés par OpenClaw. Dominic le modifie à la main ou dit au CEO « passe le filet à 21 h »; le CEO applique par `openclaw automations edit` après GO et met le fichier à jour. Le cron de contrôle (R-6) compare le fichier à `openclaw automations list --all` et signale tout écart. Mécanisme réel de modification : CLI `openclaw automations`, panneau Automations de la Control UI, ou outil `automations` de l'agent [01 §6.1].

## 4. Ce que la « façon OpenClaw » apporte et qu'on adopte

- **Standing orders** : le « quoi » autorisé vit dans `AGENTS.md` (ou `standing-orders.md` référencé), le « quand » dans les automatisations; les prompts de jobs renvoient à l'ordre permanent au lieu de le répéter. Nos règles du CEO sont déjà construites ainsi; nommer la section « Ordres permanents » et y faire pointer les deux crons. [01 §6.3]
- **Séparation stricte des fichiers** : `AGENTS.md` règles, `SOUL.md` court et tranchant (pas de politique de sécurité dedans), `USER.md` directives, `MEMORY.md` curé et court, `memory/` journal brut. Notre `SOUL.md` prévu est conforme; notre `AGENTS.md` contient la frontière d'autonomie, ce qui est le bon endroit. [01 §11.2]
- **Promotion de mémoire au rituel du lundi** : le CEO relit `memory/` de la semaine et propose ce qui monte dans `MEMORY.md`; audit de 10 minutes par Dominic (supprimer le bruit, corriger, archiver). Remplace ou encadre le Dreaming (Q-B). [02 idée 1, 03 conseil 9]
- **Journal des décisions append-only avec options rejetées** : déjà notre format d'audit; l'appliquer tel quel dans `journal/decisions.md` du CEO. [02 idée 4]
- **Préréglage équipe** comme forme future d'un département permanent (agent distinct, `allowAgents`, `reviewer`). [05 §2.9]
- **Lecteur de courriel isolé** comme patron officiel pour Gmail (R-13).
- **Config comme code** : `openclaw.json` strict (clé inconnue = refus de démarrer), `openclaw doctor --fix` pour les migrations, `security audit` régulier. À inscrire dans `PASSATION.md` du CEO. [01 §11.13]

## 5. Comparaison avec ce que d'autres ont déjà fait

| Cas | Ce qu'il fait | Ce qu'on fait de pareil | Différence | Idée à prendre |
|---|---|---|---|---|
| ucsandman / OpenClaw-Setup [02 cas 2] | Agent principal « point de contact unique » + spécialistes; dossiers `memory/people`, `projects`, `decisions`; brief de délégation à 5 champs; index mémoire court (~2 k jetons) | Même structure (CEO, `projets/`, `journal/decisions.md`) | Nous ajoutons `commun/` et `departements/` | Brief à 5 champs (R-4); index court chargé à chaque session plutôt que tout `MEMORY.md` |
| Tommaso Nervegna, organigramme « COO » [02 cas 6] | Hub unique qui délègue à 7 agents de département; un seul Telegram; bilan du vendredi; fichier de veille partagé en lecture seule; humain requis pour publier | Même vision (D-013), mêmes garde-fous | Il a 7 agents permanents après des mois; nous commençons à un | Fichier de veille quotidien en lecture seule comme seul canal entre équipes [02 idée 15]; « un agent, un travail » au départ |
| Rahul Subramaniam (Trilogy) [02 cas 8] | Agent parent avec 5 sous-agents décrits dans son `AGENTS.md`; dépôt Git + Stow, « temps de récupération 10 minutes »; cron préféré au heartbeat; compaction hebdomadaire de la mémoire | Dépôt Git comme vérité, cron pour les rituels | Il décrit ses sous-agents dans l'`AGENTS.md` du parent | Décrire les départements (une ligne chacun, rôle et brief) dans l'`AGENTS.md` du CEO, pas seulement dans `organisation.md` |
| Ryan Sarver, « chef de cabinet » [02 cas 4] | Mémoire Markdown en Git; cron « kaizen » du vendredi (veille sur l'écosystème OpenClaw + revue conjointe) | Rituel du lundi | Il ajoute une boucle d'amélioration continue du système lui-même | Cron mensuel « veille OpenClaw » (changelog, failles) préparé pour la mise à jour planifiée (R-18) |
| VelvetShark, 50 jours [03 V1, E27] | VPS, Telegram, Discord par workflow, sauvegarde Git quotidienne avec scan des secrets, brouillons seulement, « plan puis approbation » | Identique sur l'essentiel | Discord par salon pour cloisonner | Si Telegram devient encombré : un sujet (topic) de forum Telegram par domaine, chaque sujet ayant sa session [01 §8] |
| Matthew Berman [03 V16] | Git horaire; approbation Telegram avant toute écriture externe; aucun droit d'envoi | Identique | | Confirme R-8 |
| kindomLee [02 cas 3] | « Règle des 3 »; vérification de livraison; cron de détection de contradictions mémoire | Boucle d'évaluation (P-32) | | R-17; cron hebdo « contradictions entre mémoire récente et longue » [02 idée 10] |
| NetworkChuck (Paperclip) [03 V3] | Organigramme Paperclip avec Claude Code en CEO | Vision D-013 | Il utilise Paperclip, pas OpenClaw, pour l'organigramme | Confirme D-014 : organigramme en fichiers d'abord, Paperclip quand il y aura plusieurs agents |

## 6. Idées à mettre sur la table (I-1 à I-12)

Classées : **adopter** (je recommande), **discuter** (choix de Dominic), **écarter** (avec raison).

| # | Idée | Source | Avis | Ce que ça change |
|---|---|---|---|---|
| I-1 | Cron de contrôle qui vérifie que les autres crons ont tourné et alerte sinon | [02 idée 12], échec silencieux de 7 jours | Adopter | Une tâche de plus (hebdo, modèle économique); entre dans R-6 |
| I-2 | Alerte quand le plafond API est atteint | [02 idée 13] | Adopter | Alerte dans la console Anthropic + le cron de contrôle détecte un agent muet |
| I-3 | Sortie standard « décisions en attente de Dominic » et « projets en panne » dans chaque bilan | [02 idées 3, 25] | Adopter | R-16 |
| I-4 | Règle des 3 et vérification de livraison | [02 idées 7, 9] | Adopter | R-17 |
| I-5 | Interdire au CEO de modifier sa configuration et ses règles sans GO | [02 idée 28] | Adopter | R-8 |
| I-6 | Fichier de veille quotidien en lecture seule comme canal entre équipes (plutôt que des appels entre agents) | [02 idée 15] | Discuter (sans objet au jour 1) | Prépare la façon dont les équipes se parleront |
| I-7 | Sujets de forum Telegram par domaine (une session par sujet) | [01 §8, 03 V1] | Discuter | Garde un seul bot mais évite le mélange dans une seule conversation; alternative à « plusieurs agents » |
| I-8 | Cron hebdo de détection de contradictions mémoire | [02 idée 10] | Discuter | Une tâche de plus; utile dès que `MEMORY.md` grandit |
| I-9 | Journal de performance par contenu publié (vues, accroche, format) et mémoire des sujets déjà couverts | [02 idées 17, 18] | Discuter (équipe Éditorial) | Alimente plus tard la Machine à idées |
| I-10 | Cron mensuel « veille OpenClaw » avant la mise à jour planifiée | [02 cas 4] | Adopter | R-18 |
| I-11 | Compte Google dédié à l'agent plutôt que le Gmail personnel | [02 idée 21, 03 conseil 19] | Discuter | Dominic a accepté de connecter ses comptes; la lecture seule limite le risque; un compte dédié reste la pratique recommandée pour toute écriture future |
| I-12 | Plusieurs utilisateurs avancés finissent par reconstruire une version réduite (Hermes, Claude Code) : la valeur vient des patrons (fichiers, cron, canal), pas du logiciel | [03 leçon 17] | À garder en tête | Notre dépôt `ceo` est portable : si OpenClaw décevait, les fichiers survivent |

Écartées : skills ClawHub de longue traîne (YouTube, réseaux sociaux, Patreon : non maintenus ou absents, risque malveillant) [04 §A]; tableaux de bord « bureau virtuel » (la moitié abandonnés) [04 §B]; WhatsApp avec numéro personnel [03 leçon 9]; Paperclip maintenant [04 §C].

## 7. Réveils et coûts

### 7.1 Les trois réveils

| Réveil | Mécanisme réel | Modèle | Coût estimé (H) | Proposition |
|---|---|---|---|---|
| Bilan du lundi | `cron "0 7 * * 1" --tz America/Toronto --exact --session isolated` | intermédiaire | quelques dizaines de cents à 1 $ par semaine | Garder |
| Filet du soir + relances | `cron "0 22 * * *"` idem | économique (lecture de fiches, écriture de clôtures) ; intermédiaire si une clôture doit résumer une longue conversation | 0,05 à 0,30 $ par jour | Garder |
| Clôture par inactivité | job `--every 30m --idle-only --active-hours-start 08:00 --active-hours-end 23:00 --skip-if-scratch-empty --no-deliver` | économique | de 0 (saute si rien) à environ 0,05 $ par réveil utile; 0,47 à 0,75 $ si on le laisse sur un gros modèle | Voir Q-A |
| Contrôle des crons (nouveau, I-1) | `cron` hebdo, `--tools ""` ou commande | économique ou sans modèle | négligeable | Ajouter |
| Proactive check (créé par OpenClaw) | job par défaut | défaut de l'agent | 0,47 $ et plus par réveil s'il reste sur un gros modèle | Désactiver (remplacé par les trois ci-dessus) |
| Dreaming (créé par OpenClaw) | `0 3 * * *` | défaut de l'agent sauf `dreaming.model` | un passage par nuit | Voir Q-B |

### 7.2 Coût mensuel révisé (H)

Avec le routage de R-11 et sans réveil d'inactivité sur gros modèle : **30 à 130 $ par mois** pour un usage quotidien actif, d'après les témoignages après optimisation [03 §6]; l'estimation initiale de 20 à 80 $ est remplacée par cette fourchette. Un plafond de 150 $ dans la console, revu après un mois, est cohérent avec ces chiffres. Les dérives rapportées (plus de 1 000 $) viennent toutes d'un gros modèle utilisé pour tout, de réveils trop fréquents ou de boucles non détectées : R-6, R-11 et I-1 couvrent les trois.

## 8. Risques nouveaux et parades

| Risque (terrain) | Parade |
|---|---|
| Mise à jour qui casse les crons en silence | R-18 : canal `extended-stable` ou version épinglée, test de fumée, mise à jour mensuelle par Dominic |
| Cron qui échoue en silence | `failureAlert` natif + cron de contrôle (I-1) |
| Agent muet au plafond API | Alerte console + cron de contrôle (I-2) |
| Faux succès (« fait » sans preuve) | Artefacts vérifiables exigés dans chaque brief; vérification de livraison (R-17) |
| Mémoire empoisonnée | Modèles économiques jamais autorisés à écrire `MEMORY.md`; promotion au rituel du lundi relue par Dominic (R-11, §4) |
| Compaction qui perd l'état d'une tâche | Écriture au fil de l'eau (déjà prévue), flush avant compaction (actif par défaut), nouvelle session au-delà de 50 % de contexte [03 conseil 8] |
| Agent qui modifie sa propre configuration | Interdiction écrite + `tools.exec` en liste blanche (R-8) |
| Injection par courriel | Lecteur isolé, résumé sans citation, liste d'expéditeurs (R-13) |
| Fichiers de bootstrap tronqués en silence | Tailles surveillées, `openclaw doctor` et `/context list` (R-2) |

## 9. L'écosystème pour la vision finale (D-013)

- **Organigramme et équipes** : la trajectoire reste « départements en fichiers → agents distincts par le préréglage équipe → Paperclip quand plusieurs agents et budgets ». L'import d'organisation de Paperclip accepte du Markdown; `organisation.md` est à écrire dans cet esprit (rôle, rend compte à, description de poste) [04 §C].
- **Visualisation** : Control UI officielle au jour 1; `organigramme.html` généré; tableau de bord communautaire encore actif (mudrii, Autensa) seulement si un suivi des coûts par projet devient nécessaire [04 §B].
- **Skills** : sources connues seulement (auteur d'OpenClaw : `gog`, `github`, `obsidian`), Skill Vetter comme procédure du validateur avant toute installation; aucun skill de longue traîne [04 §A].
- **IA locale** : Ollama pour les embeddings et les tâches de tri quand le PC est allumé, ou un petit modèle sur le serveur; après mesure, comme prévu (P-26) [04 §D].
- **Codex / ChatGPT** : fournisseur de secours possible par abonnement; n'aide en rien à migrer les projets ChatGPT [04 §E].

## 10. Ce qui est confirmé sans changement

Serveur virtuel et Gateway unique; un seul agent au départ; interface privée et Telegram; dépôt Git privé comme workspace; fichiers de bootstrap; rituel du lundi et filet du soir en cron; relance des projets dormants (tâche du filet); clôture au fil de l'eau; délégation par sous-agents avec modèle moins cher; validateur indépendant (comme sous-agent); boucle d'évaluation par fichiers; secrets hors dépôt; identités publiques séparées; Paperclip plus tard; PC comme node plus tard; import manuel des projets actuels; développement avec le CEO et production en place.

## 11. Décisions à prendre par Dominic

- **Q-A Réveil d'inactivité.** (1) Le supprimer : écriture au fil de l'eau + filet du soir suffisent; coût nul; risque : une interruption en milieu d'après-midi n'est clôturée qu'à 22 h. (2) Le garder en job `--every 30m --idle-only`, heures actives 8 h à 23 h, modèle économique, `skip-if-scratch-empty` : clôture dans la demi-heure, coût de quelques cents par jour. (3) Toutes les 2 heures, même réglage. **Recommandation : (2)**, parce que la clôture rapide est au cœur de ce que Dominic attend (perte du fil) et que le coût devient négligeable avec le bon modèle.
- **Q-B Dreaming.** (1) Désactiver et confier la promotion de mémoire au rituel du lundi, relue par Dominic. (2) Garder, sur modèle intermédiaire, avec relecture de `DREAMS.md` le lundi. **Recommandation : (1) pendant le premier mois**, puis réévaluer : la mémoire empoisonnée est un risque documenté, et le rituel du lundi existe déjà.
- **Q-C Embeddings de la recherche mémoire.** (1) OpenAI (défaut; exige une clé OpenAI payante : deuxième fournisseur). (2) Ollama ou GGUF local sur le serveur (gratuit, un peu de mémoire vive). (3) `none` : mots-clés seulement. **Recommandation : (2)**, conforme à « un fournisseur principal »; si le serveur est trop petit, (3) au départ.
- **Q-D Gmail.** Lecture à la demande (skill de source connue) ou déclencheur isolé (plugin IMAP), à trancher à l'activation de l'équipe Services, pas maintenant.

## 12. Modifications à appliquer après validation

| Fichier | Changements |
|---|---|
| `architecture-cible.md` | §5 (composants) : vocabulaire « tâches d'automatisation », routage de modèles, indexation des dossiers, validateur comme sous-agent, Gmail en deux voies; §6 : réveils selon Q-A, `reglages.md`; §8 : coûts 30 à 130 $ |
| `principes-de-conception.md` | P-9 (réveils bornés, `reglages.md`), P-18 (« aucune automatisation au-delà de `reglages.md` »), P-20 (GO imposé par la configuration), P-26 (routage de modèles, embeddings) |
| `cahier-de-construction.md` | R-1 à R-20 : sections 0.4, 2, 3, 4, 5.1, 5.4, 5.6, 5.7, 5.9, 7, 8, 10, 11 |
| `plan-de-transition.md` | Étape 1 : désactivation des jobs par défaut, test de fumée; étape 2 : `reglages.md`, `extraPaths`; étape 3 : T-7 « un cron a échoué, l'alerte arrive »; étape 7 : Gmail en deux voies |
| `decisions.md` | D-018 : résultats de l'analyse et réponses à Q-A, Q-B, Q-C |
| `questions-ouvertes.md` | Q-A à Q-D ajoutées; Q-061 close (Paperclip confirmé plus tard) |
