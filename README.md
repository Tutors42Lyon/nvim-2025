🛠️ Atelier Pratique : Ta première config Neovim
================================================

Yo ! Bienvenue à cet atelier. 

On va faire ensemble les trois étapes qui vont te transformer en utilisateur Neovim correct :
1. Installer Kickstart (la base solide)
2. Vérifier que tout fonctionne (`checkhealth`)
3. Apprendre à organiser ta config comme un pro (le split de plugins)

---

## Étape 1 : Avant de commencer

Vérifie que tu as le kit complet. C'est pas compliqué :

- **Terminal** : Pas Windows Terminal. Du bash ou zsh classique (sur Linux tu l'as déjà)
- **Une belle police** : JetBrainsMono Nerd Font (ou similaire). Sans ça, les icônes vont être bizarres
- **Neovim** : Version 0.9 minimum. Fais `nvim --version` pour vérifier
- **Les outils** : git, ripgrep (pour chercher vite), et un compilateur C (gcc ou clang)

---

## Étape 2 : Installation

### On nettoie avant

Si t'as déjà Neovim d'installé avec une config, fais une sauvegarde (juste en cas) :

```bash
mv ~/.config/nvim ~/.config/nvim.bak
mv ~/.local/share/nvim ~/.local/share/nvim.bak
```

### Go !

Récupère Kickstart :

```bash
git clone https://github.com/nvim-lua/kickstart.nvim.git "${XDG_CONFIG_HOME:-$HOME/.config}"/nvim
```

### Ton premier lancement (important !)

```bash
nvim
```

**NE TOUCHE À RIEN.** Sérieusement.

Lazy.nvim va cloner tous les plugins. Ça va prendre 1-2 minutes. Laisse faire, va prendre un café.

Une fois que c'est fini, tape `:q` et relance `nvim`. Là ça va être mieux.

---

## Étape 3 : Vérification rapide

Fais un check de santé pour voir si tout va bien :

```vim
:checkhealth
```

Navigue avec les flèches (ou j/k si tu veux être cool).

Des **ERROR** en rouge ? Pas grave, sauf si c'est sur git ou nodejs. Les erreurs de clipboard / perl / ruby on s'en fout.

Tape `:q` pour sortir.

---

## Étape 4 : La Mission (le vrai truc important)

### Pourquoi on fait ça ?

Kickstart commence avec 800 lignes dans un seul fichier `init.lua`. C'est le chaos. On va l'organiser correctement.

L'idée : Un plugin = Un fichier. Boom. Propre.

### Le plan

1. On va sortir **Telescope** de `init.lua`
2. On le met dans son propre fichier : `lua/custom/plugins/telescope.lua`
3. On relance. Et ça marche. Magie.

### Comment faire

**Étape 1 :** Ouvre `init.lua`

**Étape 2 :** Cherche Telescope (tape `/telescope` + Enter)

**Étape 3 :** Tu vois un bloc qui commence par `{ 'nvim-telescope/telescope.nvim',`

**Étape 4 :** Place-toi sur l'accolade du début (`{`) et tape `Va}` pour tout sélectionner (cool hein ?)

**Étape 5 :** Tape `d` pour couper. Voilà, c'est parti.

**Étape 6 :** S'il y a une virgule à la fin, vire-la aussi (Vim te le montrera).

**Étape 7 :** `:w` pour sauvegarder.

### Créer le nouveau fichier

**Étape 8 :** Crée le dossier et le fichier : `lua/custom/plugins/telescope.lua`

(Ou fais `:e lua/custom/plugins/telescope.lua` directement dans Neovim)

**Étape 9 :** Colle ce que tu as copié, mais entoure-le avec `return { ... }` :

```lua
return {
  'nvim-telescope/telescope.nvim',
  event = 'VimEnter',
  branch = '0.1.x',
  dependencies = { 'nvim-lua/plenary.nvim' },
  config = function()
    require('telescope').setup {}
  end,
}
```

**Étape 10 :** `:w` pour sauvegarder

**Étape 11 :** `:q` pour quitter

**Étape 12 :** `nvim` pour relancer

### Et voilà !

Si tu tapes `<Space>sf` et que la recherche de fichiers marche... t'as gagné ! 🎉

Tu viens de faire du vrai refactoring. C'est ça qu'un vrai dev fait.

---

## 🎁 Bonus : T'en veux plus ?

Si tu as fini avant tout le monde, voilà des trucs sympas à try.

### Bonus 1 : Split d'autres plugins

Maintenant que t'as compris le truc, répète avec :
- **neo-tree** (l'explorateur de fichiers)
- **lspconfig** (l'autocomplétion)
- **gitsigns** (les modifications Git en live)

Même processus. Tu vas être rapide.

### Bonus 2 : Un nouveau thème

Crée `lua/custom/plugins/theme.lua` et teste un truc qui te plaît :

| Thème | Commande |
|-------|----------|
| **Tokyonight** (sombre et cool) | `vim.cmd.colorscheme 'tokyonight-night'` |
| **Catppuccin** (couleurs chaudes) | `vim.cmd.colorscheme 'catppuccin-mocha'` |
| **Nightfox** (épuré) | `vim.cmd.colorscheme 'nordfox'` |
| **Gruvbox** (warm vibes) | `vim.cmd.colorscheme 'gruvbox'` |

Essaie plusieurs et vois ce qui te plaît.

### Bonus 3 : Les superpowers de Telescope

Tu sais déjà chercher des fichiers. Mais y'a bien d'autres trucs :

| Raccourci | Ce que ça fait |
|-----------|---|
| `<Space>sf` | Chercher un fichier (tu connaissais) |
| `<Space>sg` | Chercher du texte PARTOUT dans le projet (grep) |
| `<Space>sh` | Chercher de l'aide dans la doc |
| `<Space>sw` | Chercher le mot sur lequel t'es |

Essaie `<Space>sg` sur un mot du code. C'est fou.

### Bonus 4 : Tes propres raccourcis

Crée `lua/custom/keymaps.lua` et mets ce que TU veux :

```lua
local keymap = vim.keymap.set

-- Sauvegarder plus rapidement
keymap('n', '<C-s>', ':w<CR>', { noremap = true })

-- Split vertical (pratique pour faire du pair coding)
keymap('n', '<M-v>', ':vsplit<CR>', { noremap = true })

-- Split horizontal
keymap('n', '<M-h>', ':split<CR>', { noremap = true })
```

Puis dans `init.lua` ajoute juste : `require('custom.keymaps')`

### Bonus 5 : Autopairs (parenthèses auto)

Crée `lua/custom/plugins/autopairs.lua` :

```lua
return {
  'windwp/nvim-autopairs',
  event = 'InsertEnter',
  config = function()
    require('nvim-autopairs').setup()
  end,
}
```

Tape `(` et c'est automatiquement `)`. Pareil pour `[`, `{`, etc. Pratique.

### 🏆 Le Challenge pour les warriors

Si t'es vraiment motivé(e) :
- Splitte TOUS les plugins (pas juste Telescope)
- Crée `lua/custom/config/` avec options.lua, keymaps.lua, etc.
- Rends `init.lua` hyper simple (juste des require)

Si tu finis ça, tu seras vraiment à l'aise avec ta config. T'auras compris comment Neovim marche.

## Aide-Mémoire : Cheat Sheet

Un petit recap des trucs à savoir. Rien de fou.

| Touche | Action |
|--------|--------|
| `i` | Passer en mode Insert (Écrire) |
| `Echap` | Revenir en mode Normal |
| `:w` | Sauvegarder (Write) |
| `:q` | Quitter (Quit) |
| `:e fichier` | Ouvrir un fichier |
| `<Space>sf` | Chercher un fichier (Telescope) |
| `<Space>sg` | Grep - Chercher du texte |
| `/texte` | Chercher du texte dans le fichier |

---

## Voilà, c'est bon !

T'as fait du bon boulot aujourd'hui.

Tu sais maintenant:
- ✅ Comment installer Neovim proprement
- ✅ Vérifier que tout fonctionne (checkhealth)
- ✅ Organiser ta config sans ça devienne un fouillis

D'ici, tu peux:
- Explorer which-key pour voir TOUS tes raccourcis
- Ajouter d'autres plugins sans te perdre dans 800 lignes
- Customiser à ta guise

**Amuse-toi bien ! 🚀**
