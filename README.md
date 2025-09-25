#!/bin/bash

# Script de génération et déploiement pour la Boîte à Outils Linux Debian
# Usage: ./deploy.sh [options]

set -e

# Colors
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m' # No Color

# Default values
REPO_NAME="linux-debian-toolbox"
GITHUB_USER=""
AUTO_PUSH=false
CREATE_EXAMPLES=false

# Functions
print_usage() {
    echo "Usage: $0 [OPTIONS]"
    echo ""
    echo "Options:"
    echo "  -u, --user USERNAME     GitHub username"
    echo "  -r, --repo REPO_NAME   Repository name (default: linux-debian-toolbox)"
    echo "  -p, --push             Auto push to GitHub"
    echo "  -e, --examples         Create example command files"
    echo "  -h, --help             Show this help message"
}

log_info() {
    echo -e "${BLUE}[INFO]${NC} $1"
}

log_success() {
    echo -e "${GREEN}[SUCCESS]${NC} $1"
}

log_warning() {
    echo -e "${YELLOW}[WARNING]${NC} $1"
}

log_error() {
    echo -e "${RED}[ERROR]${NC} $1"
}

# Parse arguments
while [[ $# -gt 0 ]]; do
    case $1 in
        -u|--user)
            GITHUB_USER="$2"
            shift 2
            ;;
        -r|--repo)
            REPO_NAME="$2"
            shift 2
            ;;
        -p|--push)
            AUTO_PUSH=true
            shift
            ;;
        -e|--examples)
            CREATE_EXAMPLES=true
            shift
            ;;
        -h|--help)
            print_usage
            exit 0
            ;;
        *)
            log_error "Unknown option: $1"
            print_usage
            exit 1
            ;;
    esac
done

# Main setup
setup_project() {
    log_info "Setting up Linux Debian Toolbox project..."
    
    # Create directory structure
    mkdir -p docs
    
    # Create _config.yml
    log_info "Creating Jekyll configuration..."
    cat > _config.yml << EOF
title: "Boîte à Outils Linux Debian"
description: "Guide interactif des commandes Linux Debian"
url: "https://${GITHUB_USER}.github.io"
baseurl: "/${REPO_NAME}"

markdown: kramdown
highlighter: rouge

exclude:
  - README.md
  - .gitignore
  - deploy.sh
  - "*.sh"

include:
  - docs

plugins:
  - jekyll-sitemap
  - jekyll-feed
EOF

    # Create .gitignore
    log_info "Creating .gitignore..."
    cat > .gitignore << EOF
# OS
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/
*.swp
*.swo

# Logs
*.log

# Temporary files
*.tmp
*~

# Node modules (if used)
node_modules/
EOF

    log_success "Project structure created!"
}

create_example_files() {
    if [[ "$CREATE_EXAMPLES" == true ]]; then
        log_info "Creating example command files..."
        
        # Create debutant.txt
        cat > docs/debutant.txt << EOF
nom: ls
description: Liste les fichiers et dossiers du répertoire courant
commande: ls
commande: ls -la
commande: ls -lh
exemple: ls -la
exemple: ls -lh /home
exemple: ls --color=auto
tags: navigation, fichiers, essentiel

nom: cd
description: Change de répertoire
commande: cd
commande: cd /path
commande: cd ..
exemple: cd /home/user
exemple: cd ..
exemple: cd ~
tags: navigation, essentiel

nom: pwd
description: Affiche le chemin du répertoire courant
commande: pwd
exemple: pwd
tags: navigation, localisation

nom: mkdir
description: Crée un ou plusieurs répertoires
commande: mkdir
commande: mkdir -p
exemple: mkdir nouveau_dossier
exemple: mkdir -p dossier/sous-dossier
tags: création, dossiers
EOF

        # Create fichiers.txt
        cat > docs/fichiers.txt << EOF
nom: cp
description: Copie fichiers et répertoires
commande: cp
commande: cp -r
commande: cp -a
exemple: cp fichier.txt copie.txt
exemple: cp -r dossier/ dossier_copie/
exemple: cp -a source/ destination/
tags: copie, fichiers

nom: mv
description: Déplace ou renomme fichiers et répertoires
commande: mv
exemple: mv ancien.txt nouveau.txt
exemple: mv fichier.txt /autre/dossier/
tags: déplacement, renommage

nom: rm
description: Supprime fichiers et répertoires
commande: rm
commande: rm -r
commande: rm -f
exemple: rm fichier.txt
exemple: rm -rf dossier/
exemple: rm -i *.log
tags: suppression, attention

nom: find
description: Recherche fichiers et répertoires
commande: find
exemple: find /home -name "*.txt"
exemple: find . -type f -size +100M
tags: recherche, filtres
EOF

        # Create processus.txt
        cat > docs/processus.txt << EOF
nom: ps
description: Affiche les processus en cours d'exécution
commande: ps aux
commande: ps -ef
exemple: ps aux
exemple: ps -ef | grep apache
tags: processus, monitoring

nom: top
description: Affichage dynamique des processus
commande: top
exemple: top
exemple: top -u username
tags: temps-réel, performance

nom: htop
description: Version améliorée de top avec interface colorée
commande: htop
exemple: htop
exemple: htop -u root
tags: interactif, coloré

nom: kill
description: Termine un processus par son PID
commande: kill
commande: kill -9
exemple: kill 1234
exemple: killall firefox
tags: terminer, processus
EOF

        # Create paquets.txt
        cat > docs/paquets.txt << EOF
nom: apt update
description: Met à jour la liste des paquets disponibles
commande: apt update
exemple: sudo apt update
tags: paquets, mise-à-jour

nom: apt upgrade
description: Met à jour tous les paquets installés
commande: apt upgrade
exemple: sudo apt upgrade
exemple: sudo apt full-upgrade
tags: paquets, mise-à-jour

nom: apt install
description: Installe un ou plusieurs paquets
commande: apt install
exemple: sudo apt install vim
exemple: sudo apt install -y htop curl git
tags: paquets, installation

nom: apt search
description: Recherche des paquets
commande: apt search
exemple: apt search text-editor
exemple: apt search "web server"
tags: paquets, recherche
EOF

        log_success "Example files created in docs/ folder!"
    fi
}

create_readme() {
    log_info "Creating README.md..."
    
    cat > README.md << EOF
# 🐧 Boîte à Outils Linux Debian

[![GitHub Pages](https://img.shields.io/badge/GitHub-Pages-blue)](https://${GITHUB_USER}.github.io/${REPO_NAME})
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

Une boîte à outils interactive et moderne pour les commandes Linux Debian, hébergée sur GitHub Pages.

## 🌟 Fonctionnalités

- ✅ **Interface moderne** avec animations et transitions
- 🔍 **Recherche en temps réel** dans toutes les commandes
- 🏷️ **Filtrage par tags** et catégories
- 🌙 **Mode sombre/clair** avec sauvegarde des préférences
- 📱 **Design responsive** pour mobile et desktop
- 📄 **Export PDF** et impression
- 📋 **Copie en un clic** des commandes
- 🔄 **Mise à jour automatique** lors de l'ajout de fichiers
- 📚 **Organisation par catégories** (débutant, avancé, etc.)

## 🚀 Accès direct

Visitez la boîte à outils : [https://${GITHUB_USER}.github.io/${REPO_NAME}](https://${GITHUB_USER}.github.io/${REPO_NAME})

## 📁 Structure du projet

\`\`\`
${REPO_NAME}/
├── index.html              # Application principale
├── docs/                   # Dossier des commandes
│   ├── commands.json      # Configuration JSON (optionnel)
│   ├── debutant.txt       # Commandes pour débutants
│   ├── fichiers.txt       # Gestion des fichiers
│   ├── processus.txt      # Gestion des processus
│   ├── reseau.txt         # Commandes réseau
│   ├── paquets.txt        # Gestion des paquets
│   └── ...               # Autres catégories
├── _config.yml            # Configuration Jekyll
├── README.md             # Documentation
└── deploy.sh            # Script de déploiement
\`\`\`

## 📝 Format des fichiers de commandes

Créez des fichiers \`.txt\` dans le dossier \`docs/\` avec ce format :

\`\`\`
nom: ls
description: Liste les fichiers et dossiers du répertoire courant
commande: ls
commande: ls -la
commande: ls -lh
exemple: ls -la
exemple: ls -lh /home
exemple: ls --color=auto
tags: navigation, fichiers, essentiel

nom: cd
description: Change de répertoire
commande: cd
exemple: cd /home/user
exemple: cd ..
tags: navigation, essentiel
\`\`\`

## 🛠️ Installation et déploiement

### Méthode rapide avec le script

\`\`\`bash
# Cloner le repository
git clone https://github.com/${GITHUB_USER}/${REPO_NAME}.git
cd ${REPO_NAME}

# Rendre le script exécutable
chmod +x deploy.sh

# Déployer avec exemples
./deploy.sh -u ${GITHUB_USER} -r ${REPO_NAME} -e -p
\`\`\`

### Méthode manuelle

1. **Créer le repository GitHub** :
   \`\`\`bash
   git init
   git add .
   git commit -m "Initial commit: Linux Debian Toolbox"
   git branch -M main
   git remote add origin https://github.com/${GITHUB_USER}/${REPO_NAME}.git
   git push -u origin main
   \`\`\`

2. **Activer GitHub Pages** :
   - Aller dans Settings → Pages
   - Source: Deploy from a branch
   - Branch: main / (root)

3. **Attendre le déploiement** (quelques minutes)

## ➕ Ajouter de nouvelles commandes

1. **Créer un fichier** dans \`docs/\` (ex: \`docs/ma_categorie.txt\`)
2. **Suivre le format** spécifié ci-dessus
3. **Commit et push** :
   \`\`\`bash
   git add docs/ma_categorie.txt
   git commit -m "Add ma_categorie commands"
   git push
   \`\`\`
4. **Le site se met à jour automatiquement** ! 🎉

## 🎨 Personnalisation

### Modifier les couleurs
Éditez les variables CSS dans \`index.html\` :
\`\`\`css
:root {
    --primary-color: #2563eb;     /* Bleu principal */
    --secondary-color: #1e40af;   /* Bleu secondaire */
    --accent-color: #3b82f6;      /* Bleu accent */
    /* ... autres variables */
}
\`\`\`

### Ajouter des icônes de catégories
Modifiez l'objet \`categoryIcons\` dans le JavaScript :
\`\`\`javascript
const categoryIcons = {
    'ma_categorie': 'fas fa-my-icon',
    // ...
};
\`\`\`

### Utiliser un fichier JSON global
Créez \`docs/commands.json\` avec la structure complète (voir exemple fourni).

## 📱 Fonctionnalités mobiles

- Interface tactile optimisée
- Sidebar coulissante
- Recherche adaptée mobile
- Boutons d'action accessibles

## 🖨️ Impression et export

- **Ctrl+P** ou bouton d'impression
- Style optimisé pour l'impression
- Export PDF via le navigateur
- Mise en page automatique

## 🔧 Dépannage

### Le site ne se charge pas
1. Vérifiez que GitHub Pages est activé
2. Attendez quelques minutes après le push
3. Vérifiez l'URL : \`https://username.github.io/repo-name\`

### Les commandes ne s'affichent pas
1. Vérifiez le format des fichiers \`.txt\`
2. Regardez la console du navigateur (F12)
3. Vérifiez que les fichiers sont dans \`docs/\`

### Problèmes de CORS
- GitHub Pages résout automatiquement les problèmes CORS
- Si vous testez en local, utilisez un serveur web simple

## 🤝 Contribution

1. **Fork** le projet
2. **Créer une branche** : \`git checkout -b feature/AmazingFeature\`
3. **Commit** : \`git commit -m 'Add AmazingFeature'\`
4. **Push** : \`git push origin feature/AmazingFeature\`
5. **Pull Request**

## 📄 Licence

Ce projet est sous licence MIT. Voir \`LICENSE\` pour plus de détails.

## 🙏 Remerciements

- [Font Awesome](https://fontawesome.com/) pour les icônes
- [GitHub Pages](https://pages.github.com/) pour l'hébergement
- La communauté Linux Debian

---

Fait avec ❤️ pour la communauté Linux
EOF

    log_success "README.md created!"
}

setup_git() {
    if [[ -n "$GITHUB_USER" ]]; then
        log_info "Setting up Git repository..."
        
        if [[ ! -d ".git" ]]; then
            git init
            log_info "Git repository initialized"
        fi
        
        git add .
        git commit -m "Initial commit: Linux Debian Toolbox

- Interactive web interface for Linux Debian commands
- Automatic loading from docs/ folder
- Search, filter, and categorization features
- Dark/light mode with persistence
- Mobile-responsive design
- Print/PDF export functionality" || log_warning "Nothing to commit or commit failed"
        
        if [[ "$AUTO_PUSH" == true ]]; then
            log_info "Setting up remote and pushing to GitHub..."
            git branch -M main
            git remote remove origin 2>/dev/null || true
            git remote add origin "https://github.com/${GITHUB_USER}/${REPO_NAME}.git"
            
            echo ""
            log_warning "You'll need to authenticate with GitHub to push."
            log_info "Make sure the repository https://github.com/${GITHUB_USER}/${REPO_NAME} exists!"
            echo ""
            
            read -p "Press Enter to continue with git push, or Ctrl+C to cancel..."
            
            if git push -u origin main; then
                log_success "Successfully pushed to GitHub!"
                log_info "Enable GitHub Pages:"
                log_info "1. Go to https://github.com/${GITHUB_USER}/${REPO_NAME}/settings/pages"
                log_info "2. Set Source to 'Deploy from a branch'"
                log_info "3. Select branch 'main' and folder '/ (root)'"
                log_info "4. Your site will be available at: https://${GITHUB_USER}.github.io/${REPO_NAME}"
            else
                log_error "Failed to push to GitHub. Please check your authentication and repository settings."
            fi
        else
            log_info "Git setup complete. To push to GitHub:"
            echo "  git remote add origin https://github.com/${GITHUB_USER}/${REPO_NAME}.git"
            echo "  git push -u origin main"
        fi
    else
        log_warning "GitHub username not provided. Skipping Git setup."
        log_info "To set up Git later, run:"
        echo "  git init"
        echo "  git add ."
        echo "  git commit -m 'Initial commit'"
        echo "  git remote add origin https://github.com/YOUR_USERNAME/${REPO_NAME}.git"
        echo "  git push -u origin main"
    fi
}

create_license() {
    log_info "Creating MIT License..."
    cat > LICENSE << EOF
MIT License

Copyright (c) $(date +%Y) ${GITHUB_USER:-"Your Name"}

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
EOF
    log_success "License created!"
}

# Main execution
main() {
    echo -e "${BLUE}"
    echo "╔══════════════════════════════════════════════════════════════╗"
    echo "║                Linux Debian Toolbox Setup                   ║"
    echo "║              GitHub Pages Deployment Script                 ║"
    echo "╚══════════════════════════════════════════════════════════════╝"
    echo -e "${NC}"
    
    setup_project
    create_example_files
    create_readme
    create_license
    setup_git
    
    echo ""
    log_success "🎉 Linux Debian Toolbox setup complete!"
    echo ""
    if [[ -n "$GITHUB_USER" ]]; then
        log_info "Next steps:"
        echo "1. 🌐 Enable GitHub Pages in repository settings"
        echo "2. 📝 Add more commands to docs/ folder"
        echo "3. 🎨 Customize colors and styling"
        echo "4. 📱 Test on different devices"
        echo ""
        log_info "Your site will be available at:"
        echo "   🔗 https://${GITHUB_USER}.github.io/${REPO_NAME}"
    else
        log_info "To complete setup:"
        echo "1. 📁 Create GitHub repository: ${REPO_NAME}"
        echo "2. 🚀 Push code to GitHub"
        echo "3. 🌐 Enable GitHub Pages"
        echo "4. 📝 Add your commands to docs/ folder"
    fi
    echo ""
}

# Run main function
main "$@"
