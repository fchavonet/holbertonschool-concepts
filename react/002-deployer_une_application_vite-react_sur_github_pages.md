# Déployer une application Vite/React sur GitHub Pages

L’objectif de ce guide est de publier une application React créée avec Vite sur GitHub Pages.

Nous allons configurer correctement le projet, générer une version optimisée de l’application, puis la publier directement depuis notre environnement de développement.

Cette méthode est simple à mettre en place et parfaitement adaptée pour des projets étudiants.

---

## Avant de commencer

Ce concept est conçu comme la suite directe du guide :

- [Monter un projet React proprement et rapidement](./001-monter_un_projet_react_proprement_et_rapidement.md)

Nous supposerons donc que vous disposez déjà :

- D’un projet React créé avec Vite.
- D’un dépôt GitHub associé.
- D’un environnement Node.js fonctionnel.
- D’un projet capable de générer un build sans erreur.

Si ce n’est pas encore le cas, nous vous recommandons de commencer par ce premier concept avant de poursuivre.

---

## Consulter la documentation officielle

Voici les pages de référence utilisées dans ce concept :

- [GitHub Pages documentation](https://pages.github.com/)
- [Deploying a static site with Vite](https://vite.dev/guide/static-deploy.html)
- [`gh-pages`](https://github.com/tschaub/gh-pages)

---

## Vérifier les prérequis

Vérifier que le projet fonctionne :

```
npm run dev
```

Vérifier que le build fonctionne :

```
npm run build
```

Si la commande `npm run build` échoue, il faut corriger les erreurs avant de poursuivre.

---

## Comprendre le principe du déploiement

Lorsque vous exécutez :

```
npm run build
```

Vite génère une version optimisée de votre application dans le dossier :

```
dist/
```

Ce dossier contient :

- Les fichiers HTML.
- Les fichiers CSS.
- Les fichiers JavaScript.
- Les images.
- Les autres ressources nécessaires au fonctionnement de l’application.

GitHub Pages hébergera directement le contenu de ce dossier.

---

## Configurer le chemin de base

Lorsque Vite génère un build de production, il doit savoir comment construire les chemins vers les fichiers CSS, JavaScript et images de votre application.

Par défaut, ces chemins sont générés en supposant que l'application est hébergée à la racine d'un domaine.

Or, GitHub Pages publie généralement les projets dans un sous-dossier, ce qui peut provoquer des erreurs de chargement après le déploiement.

Pour éviter ce problème, nous allons utiliser un chemin relatif.

Ouvrir le fichier :

```
vite.config.js
```

Puis modifier la configuration :

```
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  base: "./",
  plugins: [
    react(),
    tailwindcss()
  ],
  server: {
    host: "0.0.0.0",
    port: 3000
  }
});
```

La ligne :

```
base: "./",
```

indique à Vite de générer des chemins relatifs vers les ressources de l'application.

Par exemple :

```
./styles/index.css
./scripts/script.js
```

Cette approche présente plusieurs avantages :

- Elle fonctionne avec GitHub Pages.
- Elle ne dépend pas du nom du repository.
- Elle simplifie la configuration.
- Elle évite d'avoir à modifier le fichier si le projet est renommé.

Pour un projet étudiant ou une landing page statique, cette configuration est généralement suffisante.

> Attention : si votre serveur de développement est déjà lancé, pensez à le redémarrer après avoir modifié la configuration Vite.

Pour vérifier que tout fonctionne correctement :

```
npm run build
```

Puis :

```
npm run preview
```

Si l'application s'affiche correctement, vous pouvez passer à l'étape suivante.

---

## Installer `gh-pages`

Installer le package :

```
npm install gh-pages --save-dev
```

Ce package permettra de publier automatiquement le contenu du dossier `dist/` sur GitHub Pages.

---

## Ajouter le script de déploiement

Modifier la section `scripts` du fichier `package.json` :

```
"scripts": {
  "dev": "vite",
  "lint": "eslint .",
  "fix": "eslint . --fix",
  "build": "vite build",
  "preview": "vite preview",
  "deploy": "npm run build && gh-pages -d dist"
}
```

Nous ajoutons ici un nouveau script nommé `deploy`.

---

## Publier l'application

Lancer simplement :

```
npm run deploy
```

Après quelques secondes, vous devriez obtenir un message de confirmation.
<br>
Une nouvelle branche `gh-pages` devrait alors apparaître sur GitHub.

---

## Configurer GitHub Pages

1. Ouvrir votre repository GitHub puis accéder à :

Settings → Pages

![]()

2. Dans la section Build and deployment, sélectionner :

Deploy from a branch

![]()

3. Configurer ensuite les options suivantes :

Branch : `gh-pages`

Folder : `/(root)`

![]()

4. Cliquer sur Save pour enregistrer la configuration.

---

## Accéder à votre site

Après quelques minutes, GitHub affichera l’adresse publique de votre application.

Elle ressemblera généralement à :

[https://votre-pseudo.github.io/nom-du-repository/](#)

Par exemple :

[https://fchavonet.github.io/live_coding-react_and_daisy/](https://fchavonet.github.io/live_coding-react_and_daisy/)

Votre application React est désormais accessible sur Internet.

---

## Mettre à jour le site

Après chaque modification du projet :

```
git add .
git commit -m "Update project."
git push
```

Puis republier :

```
npm run deploy
```

La branche `gh-pages` sera mise à jour automatiquement.

---

## Comprendre ce qui se passe

Lors de l'exécution de :

```
npm run deploy
```

plusieurs opérations sont réalisées automatiquement :

1. Vite génère le build de production dans le dossier `dist/`.
2. Le package `gh-pages` crée ou met à jour la branche `gh-pages`.
3. Le contenu du dossier `dist/` est envoyé sur cette branche.
4. GitHub Pages publie automatiquement cette branche sur Internet.

L'ensemble du processus est donc automatisé par une seule commande.

---

## Problèmes fréquents

### La page est blanche

Vérifier que la propriété suivante est bien présente dans le fichier `vite.config.js` :

```
base: "./",
```

Puis reconstruire le projet :

```bash
npm run build
```

et republier :

```
npm run deploy
```

### Les images ne s'affichent pas

Vérifier les chemins utilisés dans l'application.

Pour les fichiers placés dans le dossier `public` :

```
<img src="/image.png" alt="Description" />
```

Les ressources placées dans `public/` sont automatiquement copiées dans le build final.

### Le site affiche une ancienne version

Vider le cache du navigateur `Cmd + Shift + R` (Mac) ou `Ctrl + Shift + R` (Windows)

Vous pouvez également attendre quelques minutes afin que GitHub Pages termine la propagation des modifications.

### La branche `gh-pages` n'apparaît pas

Vérifier que la commande `npm run deploy` s'est exécutée sans erreur.
<br>
Vous pouvez également vérifier que le package est bien installé :

```
npm list gh-pages
```

### La commande deploy n'existe pas

Vérifier que le script suivant est bien présent dans le fichier `package.json` :

```
"deploy": "npm run build && gh-pages -d dist"
```

Puis relancer :

```
npm install
```

---

## Conclusion

Vous savez maintenant :

- Générer un build de production avec Vite.
- Configurer correctement le chemin de base.
- Publier une application React sur GitHub Pages.
- Mettre à jour votre site après chaque modification.

Vous disposez désormais d’un workflow complet permettant de passer d’un dépôt GitHub vide à une application React publiée en ligne.