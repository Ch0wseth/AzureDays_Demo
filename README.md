# 🤖 Copilot Demo - Orange Boosted

> **Guide complet et reproductible** — Chaque démo est détaillée avec le modus operandi exact,
> ce qu'il faut taper, ce qu'il faut montrer, et le résultat attendu.

---

## 📋 Pré-requis

| Outil | Version min. | Installation |
|-------|-------------|-------------|
| Node.js | 20+ | https://nodejs.org |
| VS Code | 1.100+ | https://code.visualstudio.com |
| GitHub Copilot | Extension | Marketplace → `github.copilot` |
| GitHub Copilot Chat | Extension | Marketplace → `github.copilot-chat` |
| Playwright Test for VS Code | Extension | Marketplace → `ms-playwright.playwright` |
| Docker (optionnel) | Pour MCP awesome-copilot | https://docker.com |

> **Licence** : GitHub Copilot Individual, Business ou Enterprise requise.

---

## 🚀 Installation

```bash
git clone <url-du-repo> copilot-demo-orange
cd copilot-demo-orange
npm install
npx playwright install chromium
```

---

## ⚠️ Avant la démo : Désactiver le Caveman Mode

Le fichier `.github/instructions/caveman-mode.instructions.md` s'applique à **tous les fichiers** automatiquement.
Pour la démo, on veut le mode normal d'abord, puis activer caveman pour comparer.

**Action** : Renommer temporairement le fichier avant la démo :
```bash
mv .github/instructions/caveman-mode.instructions.md .github/instructions/caveman-mode.instructions.md.disabled
```

On le réactivera à l'étape 6.

---

## 📊 Suivi des Tokens (tout au long de la démo)

### Activer le suivi

Dans VS Code → `settings.json` :
```json
{
  "github.copilot.advanced.debug.showTokenCount": true,
  "chat.experimental.showTokenCount": true
}
```

Ou : Output Panel (`Ctrl+Shift+U`) → dropdown → `GitHub Copilot Chat`

### Dashboard en ligne

https://github.com/settings/copilot → section "Usage"

### Tableau de suivi à remplir pendant la démo

| # | Étape | Ce qu'on demande | Tokens In | Tokens Out | Observation |
|---|-------|-----------------|-----------|------------|-------------|
| 1 | Génération code | Nouveau service | ___ | ___ | |
| 2 | Génération tests | /tests sur taskService | ___ | ___ | |
| 3 | Génération docs | /doc sur une fonction | ___ | ___ | |
| 4 | Mode Ask | "Explique getTasks" | ___ | ___ | |
| 5 | Mode Edit | Ajout validation inline | ___ | ___ | |
| 6 | Mode Agent | Ajout catégories multi-fichiers | ___ | ___ | |
| 7 | Custom Prompt | /generate-route | ___ | ___ | |
| 8 | MCP Playwright | Navigation + screenshot | ___ | ___ | |
| 9 | Caveman Mode | Même question qu'étape 4 | ___ | ___ | **Comparer avec #4** |
| 10 | .copilotignore off | Supprimer .copilotignore + question | ___ | ___ | **Comparer avec #4** |

---

## 🎬 DÉMO 1 — Génération de Code

### Objectif
Montrer que Copilot génère du code JS complet à partir d'un commentaire.

### Modus Operandi

1. **Ouvrir** VS Code sur le projet
2. **Lancer** le serveur : `npm run dev` (terminal intégré)
3. **Créer** un nouveau fichier : `src/services/categoryService.js`
4. **Taper exactement** :

```javascript
/**
 * @fileoverview Category management service.
 * Provides CRUD operations for task categories.
 * @module services/categoryService
 */

```

5. **Attendre** — Copilot va proposer le code (ghost text gris)
6. **Accepter** avec `Tab`
7. **Montrer** : Copilot a généré un service CRUD complet avec JSDoc

### Ce qu'on montre à l'audience
- La vitesse de génération
- La qualité du code (JSDoc, validation, structure)
- La cohérence avec le reste du projet (même pattern que `taskService.js`)

### Résultat attendu
Un service avec `createCategory()`, `getCategories()`, `deleteCategory()` etc.

> 📊 **Noter les tokens** dans le tableau (ligne 1)

---

## 🎬 DÉMO 2 — Génération de Tests

### Objectif
Montrer la génération automatique de tests unitaires.

### Modus Operandi

1. **Ouvrir** Copilot Chat : `Ctrl+Shift+I`
2. **Taper** :

```
/tests #file:src/services/taskService.js
```

3. **Montrer** le résultat : suite de tests Jest complète
4. **Alternative** — Utiliser le prompt file :
   - Taper `/` dans le chat
   - Sélectionner `generate-tests`
   - Indiquer le fichier cible

### Ce qu'on montre à l'audience
- La commande `/tests` intégrée
- L'utilisation de `#file:` pour cibler un fichier précis (= moins de tokens)
- Les tests couvrent les cas positifs ET négatifs

### Résultat attendu
~15-20 tests couvrant `createTask`, `getTasks`, `updateTask`, `deleteTask`, `getTaskStats`

> 📊 **Noter les tokens** (ligne 2)

---

## 🎬 DÉMO 3 — Génération de Documentation

### Objectif
Montrer la génération de documentation JSDoc.

### Modus Operandi

1. **Ouvrir** `src/utils/validators.js`
2. **Sélectionner** la fonction `sanitizeInput` (lignes 19-27)
3. **Ouvrir** Copilot Chat : `Ctrl+Shift+I`
4. **Taper** :

```
/doc Génère la documentation complète avec exemples d'utilisation
```

5. **Montrer** : JSDoc enrichi avec `@example`, `@param`, `@returns`

### Alternative inline

1. Placer le curseur au-dessus d'une fonction sans JSDoc
2. Taper `/**` puis Entrée
3. Copilot complète automatiquement le bloc JSDoc

### Résultat attendu
```javascript
/**
 * Sanitizes a string to prevent XSS attacks.
 * @param {string} input - Raw input string from user.
 * @returns {string} Sanitized string safe for HTML rendering.
 * @example
 * sanitizeInput('<script>alert("xss")</script>')
 * // Returns: '&lt;script&gt;alert(&quot;xss&quot;)&lt;/script&gt;'
 */
```

> 📊 **Noter les tokens** (ligne 3)

---

## 🎬 DÉMO 4 — Modes Copilot (Ask, Edit, Agent)

### 4a. Mode Ask

**Objectif** : Poser des questions, comprendre du code.

**Modus Operandi** :
1. `Ctrl+Shift+I` pour ouvrir Copilot Chat
2. **Taper** :

```
Explique-moi comment fonctionne le filtrage dans la fonction getTasks() et quels sont les risques de performance
```

3. **Montrer** : Copilot explique le code, identifie les O(n) multiples

**Ce qu'on tape** : Juste la question ci-dessus.
**Résultat attendu** : Explication détaillée + suggestion d'optimisation.

> 📊 **Noter les tokens** (ligne 4) — ON COMPARERA AVEC LE CAVEMAN MODE À L'ÉTAPE 6

---

### 4b. Mode Edit (Inline)

**Objectif** : Modifier du code sans quitter l'éditeur.

**Modus Operandi** :
1. **Ouvrir** `src/services/taskService.js`
2. **Sélectionner** la fonction `createTask` (lignes 20-40)
3. **Appuyer** sur `Ctrl+I` (Edit inline)
4. **Taper** :

```
Ajoute la validation de la description : max 500 caractères, et log un warning si elle dépasse 200
```

5. **Montrer** le diff inline (vert/rouge)
6. **Accepter** ou **Rejeter**

**Ce qu'on tape** : La commande ci-dessus dans la zone d'edit inline.
**Résultat attendu** : Ajout de validation dans le corps de la fonction, pas de modification du reste.

> 📊 **Noter les tokens** (ligne 5)

---

### 4c. Mode Agent (multi-fichiers)

**Objectif** : Montrer la puissance du mode Agent qui modifie plusieurs fichiers.

**Modus Operandi** :
1. **Ouvrir** Copilot Chat
2. **Sélectionner le mode** : cliquer sur le dropdown en haut du chat → choisir **"Agent"**
3. **Taper** :

```
Ajoute un système de catégories aux tâches :
- Nouveau champ "category" (string) dans le task service
- Les catégories possibles : "bug", "feature", "documentation", "test"
- Validation à la création
- Filtre par catégorie dans GET /api/tasks
- Mise à jour du frontend : ajout d'un select dans le formulaire et dans les filtres
- Tests unitaires pour la nouvelle fonctionnalité
```

4. **Observer** :
   - Copilot affiche un **plan** avec les fichiers à modifier
   - Il édite `taskService.js`, `tasks.js` (route), `index.html`, `app.js` (frontend)
   - Il crée ou met à jour les tests
5. **Reviewer** chaque modification avant d'accepter

**Ce qu'on tape** : Le prompt ci-dessus.
**Résultat attendu** : 4-6 fichiers modifiés avec un plan cohérent.

> 📊 **Noter les tokens** (ligne 6) — Le mode Agent consomme plus car il planifie

---

## 🎬 DÉMO 5 — Custom Agents, Prompts, Instructions & MCP

### 5a. Instructions personnalisées

**Objectif** : Montrer que Copilot respecte les conventions du projet.

**Modus Operandi** :
1. **Ouvrir** `.github/copilot-instructions.md` → montrer le contenu à l'audience
2. **Aller** dans Copilot Chat
3. **Taper** :

```
Crée un nouveau endpoint GET /api/health/details qui retourne l'uptime et la mémoire utilisée
```

4. **Montrer** que Copilot :
   - Utilise ES Modules (pas `require`)
   - Ajoute du JSDoc
   - Met les textes en français si applicable
   - Suit le pattern du projet

**Fichier à montrer** : `.github/copilot-instructions.md`

---

### 5b. Prompt Files réutilisables

**Objectif** : Montrer les prompts pré-définis pour éviter de retaper.

**Modus Operandi** :
1. **Ouvrir** Copilot Chat
2. **Taper** `/` → la liste des prompts apparaît :
   - `generate-route` — Créer un endpoint Express
   - `generate-tests` — Générer des tests
   - `generate-boosted-component` — Créer un composant UI Orange
3. **Sélectionner** `generate-route`
4. **Compléter** avec : `pour gérer des utilisateurs avec email et nom`

**Fichiers à montrer** : `.github/prompts/*.prompt.md`

**Ce qu'on tape** : `/generate-route pour gérer des utilisateurs avec email et nom`
**Résultat attendu** : Un fichier route complet avec le bon pattern.

> 📊 **Noter les tokens** (ligne 7) — Moins de tokens en entrée car le prompt est pré-défini

---

### 5c. MCP Playwright Server

**Objectif** : Montrer que Copilot peut contrôler un navigateur.

**Pré-requis** : Le serveur tourne (`npm run dev`) + Playwright installé.

**Modus Operandi** :
1. **S'assurer** d'être en mode **Agent** dans Copilot Chat
2. **Vérifier** que le MCP est actif : icône 🔌 dans le chat ou panneau MCP
3. **Taper** :

```
Utilise Playwright pour naviguer vers http://localhost:3000, prendre un screenshot, puis cliquer sur "Nouvelle Tâche", remplir le formulaire avec le titre "Tâche créée par MCP" et la priorité haute, et vérifier qu'elle apparaît dans la liste.
```

4. **Observer** : Copilot ouvre un navigateur, interagit avec la page
5. **Montrer** le screenshot généré

**Configuration MCP** (`.vscode/mcp.json`) :
```json
{
  "mcp": {
    "servers": {
      "playwright": {
        "command": "npx",
        "args": ["@playwright/mcp@latest"]
      }
    }
  }
}
```

**Ce qu'on tape** : Le prompt ci-dessus.
**Résultat attendu** : Copilot navigue, remplit le form, vérifie le résultat → screenshots.

> 📊 **Noter les tokens** (ligne 8)

---

### 5d. MCP Awesome Copilot

**Objectif** : Montrer le MCP communautaire pour découvrir/installer des agents.

**Pré-requis** : Docker doit tourner.

**Modus Operandi** :
1. **S'assurer** que Docker est lancé
2. Le MCP est configuré dans `.vscode/mcp.json` :

```json
"awesome-copilot": {
  "type": "stdio",
  "command": "docker",
  "args": ["run", "-i", "--rm", "ghcr.io/microsoft/mcp-dotnet-samples/awesome-copilot:latest"]
}
```

3. **En mode Agent**, taper :

```
Utilise awesome-copilot pour chercher des instructions pour Node.js et Express
```

4. **Montrer** : les résultats et la possibilité d'installer directement

**Alternative sans Docker** — Aller sur https://awesome-copilot.github.com et montrer le catalogue.

---

## 🎬 DÉMO 6 — Optimisation des Tokens (Caveman Mode)

### 6a. Démo comparative — Avec vs Sans Caveman

**Objectif** : Prouver visuellement la réduction de tokens.

**Modus Operandi** :

**Étape 1 — Mode normal (déjà fait à l'étape 4a)** :
- On a déjà demandé "Explique getTasks()" → tokens notés dans le tableau (ligne 4)

**Étape 2 — Activer Caveman Mode** :
```bash
mv .github/instructions/caveman-mode.instructions.md.disabled .github/instructions/caveman-mode.instructions.md
```

**Étape 3 — Même question** :
1. **Ouvrir** Copilot Chat (nouvelle conversation pour reset le contexte)
2. **Sélectionner** l'agent **"Caveman Mode"** dans le dropdown (ou simplement garder l'instruction qui s'applique globalement)
3. **Taper exactement la même chose** :

```
Explique-moi comment fonctionne le filtrage dans la fonction getTasks() et quels sont les risques de performance
```

4. **Comparer** :
   - Réponse normale : paragraphes, détails, suggestions → beaucoup de tokens out
   - Réponse caveman : bullets, 3-6 mots par phrase, direct → **50-70% moins de tokens out**

> 📊 **Noter les tokens** (ligne 9) et **comparer avec ligne 4**

**Ce qu'on montre à l'audience** :
- Le nombre de tokens OUT a drastiquement baissé
- La qualité de l'information est préservée
- Le code généré reste identique (seul le chat est terse)

---

### 6b. L'impact du `.copilotignore`

**Objectif** : Montrer que le contexte envoyé impacte les tokens IN.

**Modus Operandi** :

**Étape 1 — Supprimer temporairement** :
```bash
mv .copilotignore .copilotignore.backup
```

**Étape 2 — Poser une question** :
```
Explique-moi comment fonctionne le filtrage dans la fonction getTasks()
```

> 📊 **Noter les tokens** (ligne 10)

**Étape 3 — Remettre le fichier** :
```bash
mv .copilotignore.backup .copilotignore
```

**Étape 4 — Montrer la différence** :
- Sans `.copilotignore` : Copilot indexe plus de fichiers → tokens IN plus élevés
- Avec : seuls les fichiers pertinents sont dans le contexte

---

### 6c. Technique `#file:` pour cibler le contexte

**Modus Operandi** :

**Sans ciblage** :
```
Comment fonctionne la création de tâches ?
```
→ Copilot scanne tout le projet pour trouver le contexte pertinent

**Avec ciblage** :
```
#file:src/services/taskService.js Comment fonctionne la création de tâches ?
```
→ Copilot utilise uniquement ce fichier → beaucoup moins de tokens IN

---

### 6d. Tableau récapitulatif des techniques d'optimisation

| Technique | Impact Tokens IN | Impact Tokens OUT | Comment activer |
|-----------|:---:|:---:|---------|
| `.copilotignore` | **-30 à -50%** | — | Créer le fichier à la racine |
| `#file:` ciblé | **-60 à -80%** | — | Préfixer le prompt avec `#file:chemin` |
| Prompt Files | **-20 à -30%** | — | Mettre dans `.github/prompts/` |
| Caveman Mode (agent) | — | **-50 à -70%** | Sélectionner l'agent "Caveman Mode" |
| Caveman Mode (instruction) | — | **-50 à -70%** | Fichier dans `.github/instructions/` |
| Fonctions courtes (<30 lignes) | **-15 à -25%** | — | Bonne pratique de code |
| JSDoc précis | — | **+qualité** = moins de re-prompts | Documenter les fonctions |
| Modèle léger (`gpt-4o-mini`) | **-50% coût** | — | Configurer dans prompt file `model:` |

---

## 📁 Structure complète du projet

```
copilot-demo-orange/
├── .github/
│   ├── copilot-instructions.md          # Instructions globales projet
│   ├── agents/
│   │   └── caveman-mode.agent.md        # 🦴 Agent Caveman (awesome-copilot)
│   ├── instructions/
│   │   └── caveman-mode.instructions.md # 🦴 Instruction globale terse
│   └── prompts/
│       ├── generate-route.prompt.md     # Prompt: nouveau endpoint
│       ├── generate-tests.prompt.md     # Prompt: tests unitaires
│       └── generate-boosted-component.prompt.md  # Prompt: composant UI
├── .vscode/
│   ├── mcp.json                         # MCP: Playwright + Awesome Copilot
│   └── extensions.json                  # Extensions recommandées
├── .copilotignore                       # Exclure fichiers du contexte
├── src/
│   ├── app.js                           # Express (port 3000)
│   ├── routes/
│   │   ├── tasks.js                     # CRUD API /api/tasks
│   │   └── analytics.js                 # GET /api/analytics/stats
│   ├── services/
│   │   └── taskService.js               # Logique métier
│   └── utils/
│       └── validators.js                # Email, sanitize, pagination
├── public/
│   ├── index.html                       # UI Orange Boosted dark mode
│   ├── css/app.css                      # Styles custom
│   └── js/app.js                        # Frontend vanilla JS
├── tests/
│   ├── taskService.test.js              # 18 tests unitaires
│   ├── validators.test.js               # 11 tests unitaires
│   └── e2e/
│       └── taskManager.spec.js          # 7 tests Playwright
├── playwright.config.js
├── jest.config.js
└── package.json
```

---

## 🧪 Commandes

```bash
npm run dev        # Serveur hot-reload → http://localhost:3000
npm test           # 29 tests unitaires Jest
npm run test:e2e   # Tests E2E Playwright (serveur doit tourner)
npm run lint       # ESLint
```

---

## 🔗 Ressources

| Ressource | Lien |
|-----------|------|
| Awesome Copilot (repo) | https://github.com/github/awesome-copilot |
| Awesome Copilot (site) | https://awesome-copilot.github.com |
| Caveman Mode - Agent | https://github.com/github/awesome-copilot/blob/main/agents/caveman-mode.agent.md |
| Caveman Mode - Instructions | https://github.com/github/awesome-copilot/blob/main/instructions/caveman-mode.instructions.md |
| Orange Boosted 5.3 | https://boosted.orange.com/ |
| Copilot Docs | https://docs.github.com/copilot |
| Playwright MCP | https://www.npmjs.com/package/@playwright/mcp |
| Custom Instructions | https://docs.github.com/copilot/customizing-copilot |

---

## ✅ Checklist Jour-J

```
AVANT LA DÉMO :
[ ] VS Code ouvert avec extensions Copilot + Playwright installées
[ ] npm install fait
[ ] npm run dev lancé → http://localhost:3000 accessible
[ ] Docker lancé (si démo awesome-copilot MCP)
[ ] npx playwright install chromium fait
[ ] caveman-mode.instructions.md renommé en .disabled
[ ] Settings tokens activés (showTokenCount)
[ ] Tableau de suivi tokens imprimé ou ouvert
[ ] Nouvelle fenêtre Copilot Chat ouverte (contexte propre)

PENDANT LA DÉMO :
[ ] À chaque étape, noter tokens IN + OUT dans le tableau
[ ] Démo 1-3 : Génération (code, tests, docs)
[ ] Démo 4 : Modes (Ask, Edit, Agent)
[ ] Démo 5 : Personnalisation (instructions, prompts, MCP)
[ ] Démo 6 : Optimisation (caveman, .copilotignore, #file)

APRÈS LA DÉMO :
[ ] Remettre caveman-mode.instructions.md (enlever .disabled)
[ ] Remettre .copilotignore si supprimé
[ ] Montrer le tableau de tokens → conclusion sur l'optimisation
```
