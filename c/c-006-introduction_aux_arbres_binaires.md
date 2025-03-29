<img height="50px" align="right" src="https://raw.githubusercontent.com/fchavonet/fchavonet/main/assets/images/logo-c.png" alt="C logo">

# C - Introduction aux arbres binaires

Les arbres binaires sont une structure de données fondamentale qui organise l’information de manière hiérarchique. Comme les listes chaînées, ils utilisent des nœuds reliés entre eux, mais chaque nœud peut avoir jusqu’à deux enfants : un enfant gauche et un enfant droit. Cette caractéristique permet d’accélérer des opérations telles que la recherche et le tri, et de représenter des données de façon plus naturelle pour certains problèmes informatiques.

---

## Qu'est-ce qu'un arbre binaire ?

Un arbre binaire se compose de nœuds reliés hiérarchiquement :

- Racine (root) : nœud au sommet de l’arbre.
- Nœuds enfants (left / right) : chaque nœud peut avoir deux enfants au maximum.
- Feuilles (leaves) : nœuds sans enfant.

```scss
        (A)
       /   \
     (B)   (C)
    /  \   /  \
  (D) (E) (F) (G)
```

- (A) est la racine.
- (B) et (C) sont les enfants de (A).
- (D), (E), (F), (G) sont des feuilles.

---

## Terminologie importante

- Profondeur d’un nœud : nombre de *niveaux* entre la racine et ce nœud.
- Hauteur d’un arbre : profondeur maximale parmi tous les nœuds (distance la plus longue de la racine à une feuille).
- Taille d’un arbre : nombre total de nœuds dans l’arbre.
- Arbre parfait : tous les niveaux sont entièrement remplis.
- Arbre complet : tous les niveaux sauf le dernier sont remplis, et le dernier niveau est occupé le plus à gauche possible.

---

## Différence entre arbre binaire simple et arbre binaire de recherche

### Arbre binaire simple

Aucune organisation particulière n’est imposée :

```scss
        (10)
       /    \
    (50)     (30)
```

Ici, 50 est à gauche, 30 est à droite, mais ce n’est pas un BST, car il n’y a pas de règle d’ordre stricte.

### Arbre binaire de recherche (BST:  Binary Search Tree)

Respecte une contrainte d’ordre pour faciliter la recherche :

- Les valeurs du sous-arbre gauche sont strictement inférieures à la valeur du nœud courant.
- Les valeurs du sous-arbre droit sont strictement supérieures à la valeur du nœud courant.

```scss
         (10)
       /      \
    (5)       (20)
   /   \     /    \
 (2)   (7) (15)  (25)
```

Cela permet une recherche rapide (`O(log n)` en général, si l’arbre est équilibré).

---

## Structure d’un nœud d’arbre binaire en C

En C, on peut représenter un arbre binaire avec une struct contenant :

```c
typedef struct binary_tree_s
{
    int n;
    struct binary_tree_s *parent;
    struct binary_tree_s *left;
    struct binary_tree_s *right;
} binary_tree_t;
```

- `n` : la donnée (ici, un entier).
- `parent` : un pointeur vers le nœud parent (pratique pour certaines opérations).
- `left` et `right` : pointeurs vers les enfants gauche et droit.

---

## Création d’un nœud

Pour créer un nœud dans un arbre binaire, on utilise malloc et on initialise ses champs :

```c
binary_tree_t *binary_tree_node(binary_tree_t *parent, int value)
{
    binary_tree_t *new_node = malloc(sizeof(binary_tree_t));

    if (new_node == NULL)
    {
        perror("Erreur d'allocation de mémoire.");
        exit(EXIT_FAILURE);
    }

    new_node->n = value;
    new_node->parent = parent;
    new_node->left = NULL;
    new_node->right = NULL;

    return (new_node);
}
```

**Explication :**

- `malloc` alloue la mémoire pour le nouveau nœud.
- `new_node->n` stocke la valeur value.
- `parent` permet de relier ce nœud à son parent (ou NULL si c’est la racine).
- Les pointeurs `left` et `right` sont mis à `NULL` puisqu’ils ne sont pas encore définis.

---

## Les différents parcours (traversées) d’un arbre binaire

Pour *visiter* tous les nœuds d’un arbre et traiter leurs valeurs, on utilise les traversées (ou parcours) :

### Parcours pré-ordre (racine → gauche → droite)

```c
void binary_tree_preorder(const binary_tree_t *tree, void (*func)(int))
{
	if (tree == NULL || func == NULL)
		return;

	func(tree->n);
	binary_tree_preorder(tree->left, func);
	binary_tree_preorder(tree->right, func);
}
```

### Parcours en ordre (gauche → racine → droite)

```c
void binary_tree_inorder(const binary_tree_t *tree, void (*func)(int))
{
	if (tree == NULL || func == NULL)
		return;

	binary_tree_inorder(tree->left, func);
	func(tree->n);
	binary_tree_inorder(tree->right, func);
}
```

### Parcours post-ordre (gauche → droite → racine)

```c
void binary_tree_postorder(const binary_tree_t *tree, void (*func)(int))
{
	if (tree == NULL || func == NULL)
		return;

	binary_tree_postorder(tree->left, func);
	binary_tree_postorder(tree->right, func);
	func(tree->n);
}
```

---

## Fonctions supplémentaires

### Taille d’un arbre

Pour connaître le nombre total de nœuds :

```c
size_t binary_tree_size(const binary_tree_t *tree)
{
	if (tree == NULL)
		return (0);

	return (1 + binary_tree_size(tree->left) +
		binary_tree_size(tree->right));
}
```

2. Hauteur d’un arbre
Pour déterminer la profondeur maximale :

```c
size_t binary_tree_height(const binary_tree_t *tree)
{
	size_t left_height;
	size_t right_height;

	if (tree == NULL || (tree->left == NULL && tree->right == NULL))
		return (0);

	left_height = binary_tree_height(tree->left);
	right_height = binary_tree_height(tree->right);

	if (left_height > right_height)
		return (1 + left_height);
	else
		return (1 + right_height);
}
```

---

## Exemple complet

Ci-dessous, un petit programme complet illustrant la création d’un arbre, son parcours en pré-ordre, et l’utilisation des fonctions binary_tree_size et binary_tree_height.

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct binary_tree_s
{
	int n;
	struct binary_tree_s *parent;
	struct binary_tree_s *left;
	struct binary_tree_s *right;
} binary_tree_t;

binary_tree_t *binary_tree_node(binary_tree_t *parent, int value);
void binary_tree_preorder(const binary_tree_t *tree, void (*func)(int));
size_t binary_tree_size(const binary_tree_t *tree);
size_t binary_tree_height(const binary_tree_t *tree);

int main(void)
{
	binary_tree_t *root;

	root = binary_tree_node(NULL, 10); /* Racine */
	root->left = binary_tree_node(root, 5);  /* Enfant gauche */
	root->right = binary_tree_node(root, 20); /* Enfant droit */

	root->left->left = binary_tree_node(root->left, 2);
	root->left->right = binary_tree_node(root->left, 7);

	printf("Parcours pré-ordre : ");
	binary_tree_preorder(root, &printf);

	printf("\nTaille de l'arbre : %lu\n", binary_tree_size(root));
	printf("Hauteur de l'arbre : %lu\n", binary_tree_height(root));

	return (0);
}

binary_tree_t *binary_tree_node(binary_tree_t *parent, int value)
{
	binary_tree_t *new_node;

	new_node = malloc(sizeof(binary_tree_t));
	if (new_node == NULL)
	{
		perror("Erreur d'allocation de mémoire");
		return (NULL);
	}
	new_node->n = value;
	new_node->parent = parent;
	new_node->left = NULL;
	new_node->right = NULL;

	return (new_node);
}

void binary_tree_preorder(const binary_tree_t *tree, void (*func)(int))
{
	if (tree == NULL || func == NULL)
		return;

	func(tree->n);
	binary_tree_preorder(tree->left, func);
	binary_tree_preorder(tree->right, func);
}

size_t binary_tree_size(const binary_tree_t *tree)
{
	if (tree == NULL)
		return (0);

	return (1 + binary_tree_size(tree->left) +
		binary_tree_size(tree->right));
}

size_t binary_tree_height(const binary_tree_t *tree)
{
	size_t left_height;
	size_t right_height;

	if (tree == NULL || (tree->left == NULL && tree->right == NULL))
		return (0);

	left_height = binary_tree_height(tree->left);
	right_height = binary_tree_height(tree->right);

	if (left_height > right_height)
		return (1 + left_height);
	else
		return (1 + right_height);
}
```

**Explication :**

- Création de la racine (10) et de deux enfants (5) et (20).
- Ajout de deux nœuds (2) et (7) comme enfants du nœud 5.
- Parcours pré-ordre pour afficher les valeurs.
- Affichage de la taille (nombre de nœuds) et de la hauteur de l’arbre.

---

# Applications courantes

- Recherche rapide : un BST (Binary Search Tree) peut offrir une recherche très performante (en `O(log n)` dans le meilleur des cas).
- Tri : le tri par tas (heap sort) repose sur une structure de type arbre binaire (un tas binaire).
- Hiérarchisation de données : représentation de systèmes de fichiers (dossiers et sous-dossiers), bases de données arborescentes, etc.
- Algorithmes : de nombreux algorithmes avancés s’appuient sur des arbres binaires (arbres rouges-noirs, AVL, etc.) pour assurer l’équilibrage automatique des données.

---

# Conseils pour bien utiliser les arbres binaires

- Gestion de la mémoire : n’oubliez jamais de libérer les nœuds alloués lorsque vous avez fini de les utiliser.
- Équilibrage : si un arbre binaire n’est pas équilibré, la performance peut se dégrader (jusqu’à `O(n)` pour certaines opérations). Les arbres auto-équilibrés (AVL, rouges-noirs) sont souvent préférés pour les applications sensibles aux performances.
- Pointeurs : maîtrisez bien la manipulation des pointeurs (enfants et parent) pour éviter les accès invalides.

---

# Conclusion

Les arbres binaires sont une structure de données centrale en programmation :

- Ils facilitent la recherche et l’organisation hiérarchique des données.
- Ils forment la base de nombreuses structures avancées (BST, arbres équilibrés, tas…).
- Ils sont polyvalents et se retrouvent dans de multiples domaines (systèmes de fichiers, bases de données, algorithmes de tri, etc.).

En prenant le temps de comprendre les concepts de nœuds, de traversées et de propriétés (taille, hauteur), vous serez en mesure d’utiliser efficacement les arbres binaires dans vos projets C. N’hésitez pas à expérimenter, créer des fonctions d’insertion, de suppression, et à explorer les différentes formes d’arbres pour enrichir votre maîtrise de cette structure essentielle !