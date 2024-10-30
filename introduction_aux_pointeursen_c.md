# Introduction aux Pointeurs en C

Les pointeurs sont une notion fondamentale en langage C, ce petit guide a pour but de vous expliquer en détail ce que sont les pointeurs, comment les utiliser et pourquoi ils sont utiles, le tout avec des exemples clairs pour en faciliter la compréhension.

---
## Qu'est-ce qu'un pointeur ?

Un pointeur est une variable qui contient l'adresse mémoire d'une autre variable.
Au lieu de stocker une valeur directe (comme un entier ou un caractère), un pointeur stocke l'emplacement en mémoire où cette valeur est située.

**Illustration :**

Imaginez la mémoire comme une grande armoire avec des tiroirs numérotés (les adresses mémoire). Une variable normale stocke une valeur dans un tiroir spécifique. Un pointeur, lui, ne contient pas directement la valeur, mais le numéro du tiroir où est rangée cette valeur.

---
## Pourquoi utiliser des pointeurs ?

- **Manipulation efficace de données** : les pointeurs permettent de manipuler directement la mémoire, ce qui peut améliorer les performances de votre programme.
- **Passage par référence** : ils permettent de modifier la valeur d'une variable à l'intérieur d'une fonction, ce qui n'est pas possible en passant simplement la variable par valeur.
- **Allocation dynamique de mémoire** : indispensables pour allouer de la mémoire à l'exécution avec `malloc`, `calloc`, etc. (que vous découvrirez bientôt).
- **Structures de données complexes** : utiles pour créer des structures comme les listes chaînées, les arbres, les graphes, etc.

---
#regular
## Comment déclarer un pointeur ?

Pour déclarer un pointeur, vous utilisez l'opérateur `*` lors de la déclaration de la variable.

```c
int *pointeur_entier;
char *pointeur_char;
float *pointeur_float;
```

- `int *pointeur_entier;` déclare un pointeur vers un entier.
- `char *pointeur_char;` déclare un pointeur vers un caractère.
- `float *pointeur_float;` déclare un pointeur vers un flottant.

---
#regular
## Obtenir l'adresse d'une variable

Pour obtenir l'adresse mémoire d'une variable, on utilise l'opérateur `&`.

```c
int nombre = 42;
int *pointeur = &nombre;
```

- `&nombre` renvoie l'adresse mémoire de `nombre`.
- `pointeur` contient maintenant l'adresse de `nombre`.

---
#regular
## Accéder à la valeur pointée (déréférencement)

Pour accéder ou modifier la valeur à laquelle un pointeur fait référence, on utilise l'opérateur `*` (déréférencement).

```c
// Affiche la valeur de "nombre" (42).
printf("%d\n", *pointeur);

// Modifie la valeur de "nombre" à 100.
*pointeur = 100; 
```

---
#regular
## Exemple complet

```c
#include <stdio.h>

int main(void) {
    int nombre = 42;
    int *pointeur = &nombre;

	// Affiche 42.
    printf("Valeur de nombre: %d\n", nombre);
    
    // Affiche l'adresse mémoire de "nombre".
    printf("Adresse de nombre: %p\n", &nombre);
    
    // Même adresse que "&nombre".
    printf("Valeur du pointeur: %p\n", pointeur);
    
    // Affiche 42.
    printf("Valeur pointée par le pointeur: %d\n", *pointeur); 

	// Modifie la valeur de "nombre" à 100.
    *pointeur = 100; 
    
    // Affiche 100.
    printf("Nouvelle valeur de nombre: %d\n", nombre);

    return (0);
}
```

**Explication :**

- `pointeur` pointe vers `nombre`.
- En modifiant `*pointeur`, on modifie directement `nombre`.

---
#regular
## Pointeurs et tableaux

En C, le nom d'un tableau est un pointeur vers son premier élément.

```c
int tableau[5] = {1, 2, 3, 4, 5};
int *pointeur = tableau;

printf("%d\n", tableau[0]); // 1
printf("%d\n", *pointeur);  // 1

// Accéder aux autres éléments.
printf("%d\n", *(pointeur + 1)); // 2
printf("%d\n", pointeur[2]); // 3
```

**Note :** `pointeur[2]` est équivalent à `*(pointeur + 2)`.

---
#regular
## Passage de pointeurs aux fonctions

Les pointeurs permettent de modifier une variable dans une fonction.

```c
#include <stdio.h>

void incrementer(int *nombre) {
    (*nombre)++;
}

int main(void) {
    int valeur = 10;
    incrementer(&valeur);
    
    printf("Valeur : %d\n", valeur); // 11
    
    return (0);
}
```

**Explication :**

- On passe l'adresse de `valeur` à la fonction `incrementer`.
- La fonction modifie la valeur à cette adresse.

---
#advanced
## Allocation dynamique de mémoire

Les pointeurs sont essentiels pour gérer la mémoire dynamique.

```c
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    int *tableau;
    int taille;

    printf("Entrez la taille du tableau : ");
    scanf("%d", &taille);

    tableau = (int *)malloc(taille * sizeof(int));
    if (tableau == NULL) {
        printf("Allocation échouée.\n");
        return 1;
    }

    for (int i = 0; i < taille; i++) {
        tableau[i] = i * 2;
    }

    for (int i = 0; i < taille; i++) {
        printf("tableau[%d] = %d\n", i, tableau[i]);
    }

    free(tableau);
    return (0);
}
```

**Explication :**

- `malloc` alloue de la mémoire pour le tableau.
- Toujours vérifier si l'allocation a réussi.
- Ne pas oublier de libérer la mémoire avec `free`.

---
#advanced
## Arithmétique des pointeurs

Vous pouvez effectuer des opérations sur les pointeurs pour naviguer en mémoire.

```c
int tableau[5] = {10, 20, 30, 40, 50};
int *pointeur = tableau;

printf("%d\n", *pointeur); // 10

pointeur++;
printf("%d\n", *pointeur); // 20

pointeur += 2;
printf("%d\n", *pointeur); // 40
```

**Note :** lorsqu'on incrémente un pointeur vers un type `int`, il avance de la taille d'un `int` en mémoire.

---
#regular
## Pointeurs NULL

Un pointeur peut ne pointer vers aucune adresse valide. On l'initialise alors à `NULL`.

```c
int *pointeur = NULL;

if (pointeur == NULL) {
    printf("Le pointeur ne pointe vers rien.\n");
}
```

**Important :** toujours vérifier qu'un pointeur n'est pas `NULL` avant de le déréférencer.

---
#regular #advanced
## Erreurs courantes

- **Déréférencement d'un pointeur non initialisé :**

```c
int *pointeur;
*pointeur = 10; // Erreur ! "pointeur" n'a pas d'adresse valide...
```

- **Oubli de `&` lors de l'affectation d'une adresse :**

```c
int nombre = 5;
int *pointeur = nombre; // Erreur ! Doit être "int *pointeur = &nombre;".
```

- **Ne pas libérer la mémoire allouée dynamiquement :**  
	Toujours utiliser `free` pour éviter les fuites de mémoire.

---

## Conseils pour bien utiliser les pointeurs

- **Initialiser les pointeurs :** toujours initialiser les pointeurs à une adresse valide ou à `NULL`.
- **Vérifier les allocations :** après un `malloc`, vérifier que le pointeur n'est pas `NULL`.
- **Utiliser les pointeurs avec précaution :** une mauvaise manipulation peut entraîner des erreurs difficiles à déboguer.
- **Commenter votre code :** les pointeurs peuvent rendre le code moins lisible, des commentaires aident à comprendre.

---

## Conclusion

Les pointeurs sont un outil puissant qui, une fois maîtrisé, vous permettra de créer des programmes efficaces et complexes. Ils demandent du temps et de la pratique pour être bien compris, mais sont indispensables pour tout programmeur C ;) !