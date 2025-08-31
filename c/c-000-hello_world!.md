# C - Hello World!

Bienvenue dans ce premier cours de programmation en langage C !

Nous allons créer ensemble notre tout premier programme. Vous apprendrez à afficher du texte à l'écran avec une fonction simple et comprendrez les bases du fonctionnement d'un programme en C.

Pour suivre ce cours, il est important d’avoir quelques notions sur l'utilisation de Linux et du terminal (naviguer dans les répertoires et exécuter des commandes simples) ainsi que de savoir manipuler un éditeur de texte sous terminal tel que `Emacs`, `Nano` ou `vim`, pour écrire et modifier du code source.

---

## Qu'est-ce qu'un programme "Hello World!" ?

C'est souvent le premier programme écrit par un débutant. Il permet d'afficher le message "Hello World!" à l'écran. C’est une manière simple de s’assurer que tout est correctement configuré pour écrire, compiler et exécuter du code en C.

---

## Structure d’un programme C

Un programme C est constitué de plusieurs éléments clés :
- Bibliothèques : permettent d'utiliser des fonctions déjà écrites, comme `printf` pour afficher du texte.
- La fonction principale : `main()`, point de départ de tout programme C.
- Instructions : lignes de code qui réalisent des actions spécifiques.

---

## Notre premier programme C

```c
#include <stdio.h>

int main(void)
{
  printf("Hello World!\n");

  return (0);
}
```

**Explication du programme :**

- `#include <stdio.h>` : cette ligne inclut la bibliothèque standard d'entrée/sortie (`stdio.h`). Elle permet d'utiliser la fonction `printf`.
- `int main(void)` : la fonction main est le point d'entrée du programme. C’est ici que l'exécution commence.
- `printf("Hello World!\n");` : cette ligne affiche le texte "Hello, World!" suivi d’un retour à la ligne (`\n`).
- `return (0);` : le programme retourne la valeur `0`, ce qui signifie qu'il s'est terminé correctement.

---

## Comment exécuter ce programme

1. Écrire le code :

Utilisez un éditeur de texte pour écrire le programme et enregistrez le fichier avec une extension `.c` (par exemple `main.c`).

2. Compiler le programme :

Utilisez un compilateur C, tel que `gcc`, pour compiler votre code. Si vous êtes sous Linux, vous pouvez l'installer en tapant la commande suivante :

```bash
sudo apt install gcc -y
```

Une fois le programme installé, utilisez la ligne de commande suivante :

```bash
gcc *.c -o hello_world
```

Cela génère un fichier exécutable nommé `hello_world`.

> Consultez le cours [Programme compilé vs programme interprété](./c-001-programme_compile_vs_programme_interprete.md) pour en apprendre plus sur cette commande.

3. Exécuter le programme :

Tapez la commande suivante pour exécuter le programme :

```bash
./hello_world
```

Vous verrez le message Hello World! s'afficher dans votre Terminal.

---

## Comprendre les erreurs possibles

### Bibliothèque manquante

Si vous oubliez d’inclure `#include <stdio.h>`, vous obtiendrez une erreur du type :

```bash
undefined reference to 'printf'
```

### Oubli de point-virgule

Chaque instruction en C doit se terminer par un point-virgule (`;`). Si vous oubliez le point-virgule après `printf`, le compilateur affichera une erreur de syntaxe.

### Nom de fichier incorrect

Assurez-vous que le fichier source a bien l'extension `.c`.

---

## Modifications et améliorations

Vous pouvez personnaliser le message affiché.
Par exemple, pour afficher "Bonjour, tout le monde !", modifiez la ligne `printf` :

```c
printf("Bonjour, tout le monde !\n");
```

---

## Conseils pratiques

- Indentation et lisibilité : indentez correctement votre code (avec la tabulation) pour le rendre plus lisible.
- Commentaires : utilisez des commentaires pour expliquer votre code.
- Pratique : essayez de modifier le programme pour mieux comprendre son fonctionnement.

---

## Conclusion

Vous venez de créer votre premier programme en C ! 

Cette étape, bien que simple, constitue une base essentielle pour la suite de votre apprentissage.

N’hésitez pas à approfondir vos connaissances en vous posant des questions comme :

- À quoi sert le mot `void` ?
- Pourquoi la phrase à afficher est entourée de guillemets (`"`) ?
- Pourquoi utilisons-nous `return (0);` à la fin du programme ?

Nous aborderons ces notions dans les cours suivants, mais rappelez-vous qu’un bon développeur est avant tout curieux et cherche à comprendre chaque élément d'un code.
