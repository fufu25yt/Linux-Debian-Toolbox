# 🐧 Boîte à Outils Linux Debian

[![GitHub Pages](https://img.shields.io/badge/GitHub-Pages-blue)](https://fufu25yt.github.io/Linux-Debian-Toolbox.github.io)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

**Linux Debian Toolbox** est un site qui recense et explique les principales commandes à utiliser sur Debian. Idéal pour les utilisateurs débutants ou confirmés souhaitant retrouver rapidement des commandes utiles pour administrer, configurer ou dépanner leur système Debian.

## 🌟 Fonctionnalités

- ✅ Interface moderne avec animations et transitions
- 🔍 Recherche en temps réel dans toutes les commandes
- 🏷️ Filtrage par tags et catégories
- 🌙 Mode sombre/clair avec sauvegarde des préférences
- 📱 Design responsive pour mobile et desktop
- 📄 Export PDF et impression
- 📋 Copie en un clic des commandes
- 🔄 Mise à jour automatique lors de l'ajout de fichiers
- 📚 Organisation par catégories (débutant, avancé, etc.)

## 🚀 Accès direct

Visitez la boîte à outils : [https://fufu25yt.github.io/Linux-Debian-Toolbox.github.io](https://fufu25yt.github.io/Linux-Debian-Toolbox.github.io)

## 📁 Structure du projet

```
Linux-Debian-Toolbox.github.io/
├── index.html              # Application principale
├── docs/                   # Dossier des commandes
│   ├── debutant.txt       # Commandes pour débutants
│   ├── fichiers.txt       # Gestion des fichiers
│   ├── processus.txt      # Gestion des processus
│   ├── reseau.txt         # Commandes réseau
│   ├── paquets.txt        # Gestion des paquets
│   └── ...               # Autres catégories
├── _config.yml            # Configuration Jekyll
├── README.md              # Documentation
└── deploy.sh              # Script de déploiement
```

## 📝 Format des fichiers de commandes

Créez des fichiers `.txt` dans le dossier `docs/` avec ce format :

```
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
```

## 🛠️ Installation et déploiement

### Méthode rapide avec le script

```bash
# Cloner le repository
git clone https://github.com/fufu25yt/Linux-Debian-Toolbox.github.io.git
cd Linux-Debian-Toolbox.github.io

# Rendre le script exécutable
chmod +x deploy.sh

# Déployer avec exemples
./deploy.sh -u fufu25yt -r Linux-Debian-Toolbox.github.io -e -p
```

### Méthode manuelle

1. **Créer le repository GitHub** :
   ```bash
   git init
   git add .
   git commit -m "Initial commit: Linux Debian Toolbox"
   git branch -M main
   git remote add origin https://github.com/fufu25yt/Linux-Debian-Toolbox.github.io.git
   git push -u origin main
   ```

2. **Activer GitHub Pages** :
   - Aller dans Settings → Pages
   - Source: Deploy from a branch
   - Branch: main / (root)

3. **Attendre le déploiement** (quelques minutes)

## ➕ Ajouter de nouvelles commandes

1. **Créer un fichier** dans `docs/` (ex: `docs/ma_categorie.txt`)
2. **Suivre le format** spécifié ci-dessus
3. **Commit et push** :
   ```bash
   git add docs/ma_categorie.txt
   git commit -m "Add ma_categorie commands"
   git push
   ```
4. **Le site se met à jour automatiquement** ! 🎉

## 🎨 Personnalisation

### Modifier les couleurs
Éditez les variables CSS dans `index.html` :

```css
:root {
    --primary-color: #2563eb;     /* Bleu principal */
    --secondary-color: #1e40af;   /* Bleu secondaire */
    --accent-color: #3b82f6;      /* Bleu accent */
    /* ... autres variables */
}
```

### Ajouter des icônes de catégories
Modifiez l'objet `categoryIcons` dans le JavaScript :

```javascript
const categoryIcons = {
    'ma_categorie': 'fas fa-my-icon',
    // ...
};
```

### Utiliser un fichier JSON global
Créez `docs/commands.json` avec la structure complète (voir exemple fourni).

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
3. Vérifiez l'URL : `https://username.github.io/repo-name`

### Les commandes ne s'affichent pas
1. Vérifiez le format des fichiers `.txt`
2. Regardez la console du navigateur (F12)
3. Vérifiez que les fichiers sont dans `docs/`

### Problèmes de CORS
- GitHub Pages résout automatiquement les problèmes CORS
- Si vous testez en local, utilisez un serveur web simple

## 🤝 Contribution

1. **Fork** le projet
2. **Créer une branche** : `git checkout -b feature/AmazingFeature`
3. **Commit** : `git commit -m 'Add AmazingFeature'`
4. **Push** : `git push origin feature/AmazingFeature`
5. **Pull Request**

## 📄 Licence

Ce projet est sous licence MIT. Voir `LICENSE` pour plus de détails.

## 🙏 Remerciements

- [Font Awesome](https://fontawesome.com/) pour les icônes
- [GitHub Pages](https://pages.github.com/) pour l'hébergement
- La communauté Linux Debian

---

Fait avec ❤️ pour la communauté Linux