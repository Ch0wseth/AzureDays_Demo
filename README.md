# 🤖 Copilot Demo - Orange Boosted

> **Guide reproductible** pour démontrer GitHub Copilot dans VS Code avec l'interface Orange Boosted.
> Chaque participant peut suivre ce README pas à pas pour reproduire la démo.

---

## 📋 Pré-requis

| Outil | Version min. | Installation |
|-------|-------------|-------------|
| Node.js | 20+ | https://nodejs.org |
| VS Code | 1.100+ | https://code.visualstudio.com |
| GitHub Copilot | Extension VS Code | Marketplace → `github.copilot` |
| GitHub Copilot Chat | Extension VS Code | Marketplace → `github.copilot-chat` |
| Docker (optionnel) | Pour MCP awesome-copilot | https://docker.com |

> **Licence Copilot** : Copilot Individual, Business ou Enterprise requise.

---

## 🚀 Installation & Lancement

```bash
# 1. Cloner le projet
git clone <url-du-repo> copilot-demo-orange
cd copilot-demo-orange

# 2. Installer les dépendances
npm install

# 3. Installer Playwright (navigateurs pour tests E2E)
npx playwright install chromium

# 4. Lancer le serveur de dev
npm run dev
# → http://localhost:3000

# 5. Ouvrir dans VS Code
code .
```

---

## 📊 Suivi de Consommation des Tokens

> **IMPORTANT** : Tout au long de la démo, gardez un œil sur la consommation de tokens.

### Comment suivre les tokens dans VS Code

1. **Panneau Copilot Chat** → icône `⚙️` en bas → "Show Token Usage" (si disponible)
2. **Output Panel** → sélectionner `GitHub Copilot Chat` dans le dropdown
3. **Settings** : `"github.copilot.advanced.debug.showTokenCount": true`
4. **Dashboard** : https://github.com/settings/copilot → Usage

### Tableau de suivi démo

Utilisez ce tableau pour noter la consommation à chaque étape :

| # | Étape Démo | Tokens Entrée | Tokens Sortie | Notes |
|---|-----------|---------------|---------------|-------|
| 1 | Génération code | ___ | ___ | |
| 2 | Génération tests | ___ | ___ | |
| 3 | Génération docs | ___ | ___ | |
| 4 | Mode Ask | ___ | ___ | |
| 5 | Mode Edit | ___ | ___ | |
| 6 | Mode Agent | ___ | ___ | |
| 7 | Custom Prompt | ___ | ___ | |
| 8 | MCP Playwright | ___ | ___ | |
| 9 | Avec .copilotignore | ___ | ___ | Comparer avant/après |
| 10 | Mode Caveman | ___ | ___ | Réduction attendue |

---

## 📁 Structure du Projet

```
copilot-demo-orange/
├── .github/
│   ├── copilot-instructions.md       # ⚡ Instructions custom Copilot
│   ├── agents/                       # 🤖 Custom Agents
│   │   └── caveman-mode.agent.md     # 🦴 Agent low-token (awesome-copilot)
│   ├── instructions/                 # 📏 Instructions par fichier
│   │   └── caveman-mode.instructions.md  # 🦴 Instructions terse
│   └── prompts/                      # 📝 Prompt files réutilisables
│       ├── generate-route.prompt.md
│       ├── generate-tests.prompt.md
│       └── generate-boosted-component.prompt.md
├── .vscode/
│   └── mcp.json                      # 🔌 MCP: Playwright + Awesome Copilot
├── .copilotignore                    # ⚡ Optimisation tokens
├── src/
│   ├── app.js                        # Express app principale
│   ├── routes/                       # API REST endpoints
│   ├── services/                     # Logique métier (CRUD tasks)
│   └── utils/                        # Utilitaires (validation, sanitisation)
├── public/                           # Frontend Orange Boosted 5.3
│   ├── index.html                    # UI dark mode
│   ├── css/app.css
│   └── js/app.js
├── tests/
│   ├── taskService.test.js           # Tests unitaires Jest
│   ├── validators.test.js
│   └── e2e/taskManager.spec.js       # Tests Playwright E2E
├── playwright.config.js
└── package.json
```

---

## 🎯 Scénarios de Démo

### Démo 1 — Génération de Code, Tests & Documentation

**Objectif** : Montrer Copilot qui génère du code JS, des tests et de la documentation.

#### 1.1 Générer un service (code)

1. Ouvrir `src/services/` dans VS Code
2. Créer un nouveau fichier `userService.js`
3. Taper le commentaire :
   ```javascript
   /**
    * @fileoverview User management service with CRUD operations.
    * Uses in-memory store, similar to taskService.
    */
   ```
4. **Observer** Copilot qui propose le code complet du service

> 📊 **Tokens** : Noter la consommation dans le tableau

#### 1.2 Générer des tests

1. Ouvrir Copilot Chat (`Ctrl+Shift+I`)
2. Taper :
   ```
   /tests #file:src/services/taskService.js
   ```
3. **Ou** utiliser le prompt file : taper `/` puis sélectionner `generate-tests`

#### 1.3 Générer de la documentation

1. Sélectionner une fonction dans `taskService.js`
2. Copilot Chat → Taper :
   ```
   /doc Génère la documentation JSDoc complète pour cette fonction
   ```

---

### Démo 2 — Modes Copilot (Ask, Edit, Agent)

#### 2.1 Mode Ask (Ctrl+Shift+I)

Poser des questions sur le code :
```
Explique-moi comment fonctionne le filtrage dans getTasks()
Quels sont les risques de sécurité dans ce code ?
Comment cette app utilise Orange Boosted ?
```

#### 2.2 Mode Edit (Ctrl+I dans l'éditeur)

Sélectionner du code et demander des modifications inline :
```
Ajoute la validation de la longueur de la description (max 500 chars)
Refactore cette fonction pour utiliser une Map au lieu d'un Array
```

#### 2.3 Mode Agent (le plus puissant)

Dans Copilot Chat, sélectionner le mode **Agent** puis demander :
```
Ajoute un système de catégories aux tâches :
- Nouveau champ "category" dans le service
- Endpoint API pour lister les catégories
- Filtre par catégorie dans le frontend
- Tests unitaires pour la nouvelle fonctionnalité
```

> ⚡ **L'Agent** va créer/modifier plusieurs fichiers automatiquement avec un plan.

---

### Démo 3 — Custom Agents, Prompts, Instructions & MCP

#### 3.1 Instructions personnalisées

Ouvrir `.github/copilot-instructions.md` et montrer comment elles affectent les réponses :
- Copilot sait qu'on utilise Orange Boosted
- Il génère du code en ES Modules
- Il met les textes UI en français

#### 3.2 Prompt Files (réutilisables)

Taper `/` dans Copilot Chat pour voir les prompts disponibles :
- `generate-route` — Nouveau endpoint Express
- `generate-tests` — Suite de tests pour un service
- `generate-boosted-component` — Composant UI Orange

#### 3.3 MCP Playwright Server

Le serveur MCP est configuré dans `.vscode/mcp.json`. En mode Agent :

```
Navigue vers http://localhost:3000 et prends un screenshot
Clique sur "Nouvelle Tâche", remplis le formulaire et vérifie que la tâche apparaît
Lance les tests E2E et montre-moi les résultats
```

> Le MCP Playwright permet à Copilot de **contrôler un vrai navigateur** !

#### 3.4 Awesome Copilot (repo communautaire GitHub)

Le repo [github/awesome-copilot](https://github.com/github/awesome-copilot) est une collection communautaire de centaines de :
- **Agents** — Personas IA spécialisés (`.github/agents/*.agent.md`)
- **Instructions** — Standards de code par techno (`.github/instructions/*.instructions.md`)
- **Prompts** — Tâches réutilisables (`.github/prompts/*.prompt.md`)

##### Installation dans le projet (3 méthodes)

**Méthode 1 — MCP Server** (recherche + installation automatique via Docker) :

```json
// Déjà configuré dans .vscode/mcp.json
"awesome-copilot": {
  "type": "stdio",
  "command": "docker",
  "args": ["run", "-i", "--rm", "ghcr.io/microsoft/mcp-dotnet-samples/awesome-copilot:latest"]
}
```

En mode Agent, demander : `Cherche et installe l'instruction "nodejs" depuis awesome-copilot`

**Méthode 2 — Badges "Install in VS Code"** (1-clic) :

Aller sur https://github.com/github/awesome-copilot et cliquer les badges ![Install](https://img.shields.io/badge/VS_Code-Install-0098FF?style=flat-square) à côté de chaque ressource.

**Méthode 3 — Copie manuelle** (déjà fait pour cette démo) :

Les fichiers caveman mode sont copiés dans :
- `.github/agents/caveman-mode.agent.md`
- `.github/instructions/caveman-mode.instructions.md`

##### Ressources intégrées dans cette démo

| Fichier | Source | Usage |
|---------|--------|-------|
| `caveman-mode.agent.md` | [agents/caveman-mode](https://github.com/github/awesome-copilot/blob/main/agents/caveman-mode.agent.md) | Optimisation tokens |
| `caveman-mode.instructions.md` | [instructions/caveman-mode](https://github.com/github/awesome-copilot/blob/main/instructions/caveman-mode.instructions.md) | S'applique à tous les fichiers |

---

### Démo 4 — Optimisation des Tokens (Mode "Caveman")

**Objectif** : Montrer comment réduire drastiquement la consommation de tokens.

#### 4.1 Le `.copilotignore`

```gitignore
# Fichiers exclus du contexte Copilot → moins de tokens envoyés
node_modules/
dist/
coverage/
*.lock
*.log
.git/
```

**Démo** : Supprimer temporairement le fichier, poser une question, noter les tokens.
Puis remettre le fichier et reposer la même question → **observer la différence**.

#### 4.2 Références ciblées avec `#file`

Au lieu de laisser Copilot scanner tout le projet :
```
#file:src/services/taskService.js Ajoute une méthode getTaskById
```

→ Copilot n'envoie que ce fichier en contexte = **beaucoup moins de tokens**

#### 4.3 Mode "Caveman" (depuis [github/awesome-copilot](https://github.com/github/awesome-copilot))

Le **Caveman Mode** est un agent + instruction directement issus du repo communautaire `github/awesome-copilot`.
Il est inclus dans ce projet dans :
- `.github/agents/caveman-mode.agent.md` — Agent custom sélectionnable dans Copilot Chat
- `.github/instructions/caveman-mode.instructions.md` — Instruction applicable à tous les fichiers

**Principe** : Réponses ultra-concises, style "homme des cavernes" :
- "Me fix code" au lieu de "I will fix the code for you"
- Phrases de 3-6 mots max
- Cible **50-70% moins de tokens** en sortie
- Code toujours propre, seules les réponses chat sont terse
- Pas de fillers : "Great question", "Here's what I did"...

**Démo comparative** :
1. **Sans** caveman : sélectionner mode "Agent" par défaut → demander "Explique la fonction createTask"
2. **Avec** caveman : sélectionner l'agent "Caveman Mode" → même question → **observer la réduction**

> 💡 **Installation directe** : [![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_Agent-0098FF?logo=visualstudiocode)](https://aka.ms/awesome-copilot/install/agent?url=vscode%3Achat-agent%2Finstall%3Furl%3Dhttps%3A%2F%2Fraw.githubusercontent.com%2Fgithub%2Fawesome-copilot%2Fmain%2Fagents%2Fcaveman-mode.agent.md)
> 
> Ou via le MCP awesome-copilot : demander à l'agent de chercher et installer "caveman"

#### 4.4 Prompt Files pour réutilisation

Les prompts dans `.github/prompts/` évitent de retaper les mêmes instructions :
- Moins de tokens en entrée à chaque utilisation
- Contexte pré-défini = réponses plus précises du premier coup
- Moins d'allers-retours = moins de tokens consommés au total

#### 4.5 Bonnes pratiques token-efficaces

| Technique | Impact Tokens | Exemple |
|-----------|:---:|---------|
| `.copilotignore` | -30-50% contexte | Exclure `node_modules`, `dist` |
| `#file:` ciblé | -60-80% contexte | Ne référencer que les fichiers utiles |
| Mode Caveman | -50-70% sortie | Réponses minimales |
| Prompt Files | -20-30% entrée | Éviter de retaper les instructions |
| Fonctions courtes | -15-25% | Fichiers < 100 lignes |
| JSDoc précis | +qualité | Meilleur contexte, moins de re-prompts |

---

## 🎨 Interface Orange Boosted

L'application utilise [Orange Boosted 5.3](https://boosted.orange.com/) :
- **Dark mode** activé (`data-bs-theme="dark"`)
- **CDN** : pas de build CSS nécessaire
- **Accessible** : WCAG AA (contraste, labels, ARIA)
- **Composants** : Navbar, Cards, Modal, Accordion, Badges, Forms

---

## 🧪 Commandes disponibles

```bash
npm run dev        # Serveur avec hot-reload (--watch)
npm test           # Tests unitaires Jest (29 tests)
npm run test:e2e   # Tests E2E Playwright
npm run lint       # ESLint
```

---

## 🔗 Ressources

| Ressource | Lien |
|-----------|------|
| Awesome Copilot (repo) | https://github.com/github/awesome-copilot |
| Awesome Copilot (site) | https://awesome-copilot.github.com |
| Caveman Mode Agent | https://github.com/github/awesome-copilot/blob/main/agents/caveman-mode.agent.md |
| Orange Boosted | https://boosted.orange.com/ |
| Copilot Docs | https://docs.github.com/copilot |
| Playwright MCP | https://www.npmjs.com/package/@playwright/mcp |
| Copilot Custom Instructions | https://docs.github.com/copilot/customizing-copilot |

---

## 📝 Checklist Démo

- [ ] VS Code ouvert avec extensions Copilot installées
- [ ] `npm install` + `npm run dev` lancé
- [ ] Dashboard tokens ouvert (Output panel ou settings debug)
- [ ] Tableau de suivi tokens prêt (imprimer ou ouvrir dans un onglet)
- [ ] Docker lancé (si démo MCP awesome-copilot)
- [ ] Mode caveman : instructions prêtes à copier/retirer
- [ ] Navigateur ouvert sur http://localhost:3000
