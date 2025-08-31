# C - Programme compilé vs programme interprété

Dans ce cours, nous allons voir la différence entre un programme compilé et un programme interprété. Cette distinction est essentielle pour comprendre comment votre code en C est exécuté et pourquoi la compilation est une étape importante.

---

## Programme compilé

Un programme compilé est un programme dont le code source est traduit en un fichier exécutable par un compilateur. Ce fichier contient des instructions que l'ordinateur peut comprendre directement. Une fois le programme compilé, il peut être exécuté sans avoir besoin du code source ni du compilateur.

**Exemples de langages compilés :**

- C
- C++
- Go
- Rust

---

## Programme interprété

Un programme interprété est un programme qui est lu et exécuté ligne par ligne par un interpréteur (qui doit être installé sur la machine qui va recevoir le code) au moment de l’exécution. Contrairement à la compilation, il n’y a pas de fichier exécutable préalablement créé. Le code source doit être disponible à chaque exécution (tout comme l'interpréteur).

**Exemples de langages interprétés :**

- JavaScript
- PHP
- Python
- Ruby

---

## Différences principales

| **Aspect**               | **Programme compilé**                          | **Programme interprété**                  |
|--------------------------|------------------------------------------------|-------------------------------------------|
| **Traduction du code :** | Compilation en un fichier exécutable.          | Exécution directe par un interpréteur.    |
| **Performance :**        | Plus rapide (optimisé lors de la compilation). | Moins rapide (traduit à l’exécution).     |
| **Portabilité :**        | Dépend de la machine cible.                    | Plus portable (nécessite l’interpréteur). |
| **Exécution :**          | Le fichier binaire est autonome.               | Le code source est nécessaire.            |

---

## Les 4 étapes de la compilation en C

La compilation en C est un processus complexe qui se décompose en quatre étapes :

1. Préprocesseur (preprocessing).
2. Compilation (compiling).
3. Assemblage (assembling).
4. Édition de liens (linking).

Voici un schéma simplifié :

```bash
Source C (.c)   -->  Préprocesseur      -->  Fichier prétraité (.i)  
                -->  Compilation        -->  Fichier assembleur (.s)  
                -->  Assemblage         -->  Fichier objet final (.o)  
                -->  Edition de liens   -->  Exécutable (.out)
```

### Préprocesseur (preprocessing)

Le préprocesseur traite les directives spéciales comme `#include` ou `#define`.

- Il remplace les macros (`#define`) par leurs valeurs.
- Il inclut les fichiers d’en-tête nécessaires (`#include`).

### Compilation (compiling)

Le compilateur traduit le fichier prétraité (`.i`) en code assembleur (fichier `.s`).
Ce code est encore lisible par un humain, mais il est proche du langage machine.

### Assemblage (assembling)

L’assembleur convertit le code assembleur (`.s`) en code binaire (fichier objet `.o`).

- Ce fichier objet est un fichier machine qui contient les instructions traduites sous forme binaire.
- Cependant, ce fichier n’est pas encore exécutable : il manque les liens avec les bibliothèques.

### Édition de liens (linking)

Le fichier objet est lié aux bibliothèques nécessaires (comme la bibliothèque standard `libc`).

- Cette étape produit un fichier exécutable (`a.out` ou avec un nom défini).
- Si des fonctions externes sont utilisées (comme `printf`), le linker les associe à la bonne bibliothèque.

---

## Exemple de compilation complète

```bash
gcc *.c -o hello_world
```

**Détail de la commande :**

- `gcc`: nom du compilateur (GNU Compiler Collection). Il sert à transformer votre code source en un exécutable.
- `*.c` : on sélectionne tous les fichiers du dossier courant ayant l’extension `.c` (le code source en C).
- `-o hello_world` : permet de définir le nom de l’exécutable généré (ici `hello_world`). Si cette option n’est pas utilisée, le fichier exécutable prendra le nom par défaut `a.out`.

**Étapes réalisées par la commande `gcc` :**

1. Le préprocesseur traite les directives.
2. Le compilateur traduit en assembleur, puis en fichier objet.
3. L’assembleur crée le fichier objet.
4. Le linker crée l’exécutable final (`hello_world`).

---

## Pourquoi ces étapes sont-elles importantes ?

- Optimisation : le compilateur peut optimiser votre code en supprimant ou améliorant certaines instructions.
- Flexibilité : le processus en plusieurs étapes permet de déboguer et d'analyser chaque étape si nécessaire.
- Portabilité : le fichier source peut être compilé sur différentes architectures (grâce au compilateur).

---

## Conclusion

Vous savez désormais ce qui différencie un programme compilé d’un programme interprété et les étapes de compilation en C. Cette compréhension vous aidera à déboguer et optimiser vos programmes tout au long de votre apprentissage.