<img height="50px" align="right" src="https://raw.githubusercontent.com/fchavonet/fchavonet/main/assets/images/logo-c.png" alt="C logo">

# C - Introduction à `argc` et `argv`

Lorsque vous exécutez un programme en C depuis la ligne de commande, il est possible de lui passer des arguments.
Ces arguments peuvent être utilisés par le programme pour modifier son comportement en fonction des besoins de l'utilisateur.
Les paramètres `argc` et `argv` de la fonction `main` permettent de gérer ces arguments.

---

## Qu'est-ce que `argc` et `argv` ?

- **`argc`** : c'est un entier qui représente le nombre d'arguments passés au programme, y compris le nom du programme lui-même.
- **`argv`** : c'est un tableau de chaînes de caractères (`char *argv[]`), où chaque élément est un argument passé au programme.

**En résumé :**

- `argc` (argument count) est le nombre total d'arguments.
- `argv` (argument vector) est un tableau contenant les arguments sous forme de chaînes de caractères.

---

## Déclaration de la fonction `main` avec `argc` et `argv`

Pour utiliser `argc` et `argv`, la fonction `main` doit être déclarée comme suit :

```c
int main(int argc, char *argv[])
{     
	/* Votre code ici... */
}
```

> 📌 `char *argv[]` est équivalent à `char **argv`.

---

## Comment fonctionnent `argc` et `argv` ?

Imaginons que vous exécutez votre programme comme ceci :

```bash
./mon_programme argument1 argument2 argument3
```

- **`argc`** vaudra `4`.
- **`argv`** sera un tableau contenant :
	- `argv[0]` : `"./mon_programme"`
    - `argv[1]` : `"argument1"`
    - `argv[2]` : `"argument2"`
    - `argv[3]` : `"argument3"`

> 📌 `argc` compte le nom du programme lui-même (`argv[0]`) dans le total, d'où le chiffre 4.

---

## Exemple pratique

Voici un programme qui affiche tous les arguments passés en ligne de commande :

```c
#include <stdio.h>

int main(int argc, char *argv[])
{
	printf("Nombre d'arguments : %d\n", argc);

	for (int i = 0; i < argc; i++)
	{
		printf("Argument %d : %s\n", i, argv[i]);
	}

	return (0);
}
```

**Explication :**

- On affiche le nombre total d'arguments (`argc`).
- On parcourt le tableau `argv` pour afficher chaque argument.

---

## Utilisation des arguments dans le programme

Les arguments dans `argv` sont des chaînes de caractères (`char *`). Si vous avez besoin d'un entier ou d'un flottant, vous devez les convertir.

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
	if (argc != 3)
	{
		printf("Usage : %s nombre1 nombre2\n", argv[0]);
		return (1);
	}

	int nombre1 = atoi(argv[1]);
	int nombre2 = atoi(argv[2]);
	int somme = nombre1 + nombre2;

	printf("La somme de %d et %d est %d\n", nombre1, nombre2, somme);

	return (0);
}
```

**Explication :**

- On vérifie que l'utilisateur a passé exactement 2 arguments (en plus du nom du programme).
- `atoi` (ASCII to Integer) convertit une chaîne de caractères en entier.
- On calcule la somme des deux nombres et on l'affiche.

---

## Gestion des erreurs

Il est important de gérer les cas où l'utilisateur n'entre pas les arguments attendus.

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
	if (argc != 2)
	{
		fprintf(stderr, "Erreur : Veuillez fournir un seul argument.\n");
		return (1);
	}

	printf("Vous avez entré : %s\n", argv[1]);

	return (0);
}
```

**Explication :**

- On utilise `fprintf` avec `stderr` pour afficher les messages d'erreur.
- On retourne `1` pour indiquer que le programme s'est terminé avec une erreur.

---

## Parcourir tous les arguments

Si vous ne savez pas combien d'arguments seront passés, vous pouvez les parcourir tous.

```c
#include <stdio.h>

int main(int argc, char *argv[])
{
	printf("Liste des arguments :\n");

	for (int i = 1; i < argc; i++)
	{
		printf("- %s\n", argv[i]);
	}

	return (0);
}
```

---

## Utilisation avancée : Analyse des options

Les programmes utilisent souvent des options pour modifier leur comportement (`-h`, `--version`, etc.).

```c
#include <stdio.h>
#include <string.h>

int main(int argc, char *argv[])
{
	int afficherAide = 0;

	for (int i = 1; i < argc; i++)
	{
		if (strcmp(argv[i], "--help") == 0 || strcmp(argv[i], "-h") == 0)
		{
			afficherAide = 1;
		}
	}

	if (afficherAide)
	{
		printf("Usage : %s [options]\n", argv[0]);
		printf("Options :\n");
		printf("  -h, --help    Afficher cette aide\n");
		return (0);
	}

	printf("Le programme s'exécute normalement.\n");

	return (0);
}
```

**Explication :**

- On parcourt les arguments pour vérifier si `-h` ou `--help` est présent.
- Si c'est le cas, on affiche l'aide et on termine le programme.

---

## Exemple : calculatrice simple

Créons une calculatrice qui prend une opération en argument.

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
	if (argc != 4)
	{
		fprintf(stderr, "Usage : %s nombre1 opérateur nombre2\n", argv[0]);
		return (1);
	}

	double nombre1 = atof(argv[1]);
	char operateur = argv[2][0];
	double nombre2 = atof(argv[3]);
	double resultat;

	switch (operateur)
	{
		case '+':
			resultat = nombre1 + nombre2;
			break;
		case '-':
			resultat = nombre1 - nombre2;
			break;
		case 'x':
		case '*':
			resultat = nombre1 * nombre2;
			break;
		case '/':
			if (nombre2 == 0)
			{
				fprintf(stderr, "Erreur : Division par zéro.\n");
				return (1);
			}
			resultat = nombre1 / nombre2;
			break;
		default:
			fprintf(stderr, "Erreur : Opérateur non reconnu.\n");
			return (1);
	}

	printf("Résultat : %f\n", resultat);

	return (0);
}
```

> 📌 Attention, si vous testez le programme aves la multiplication, il est conseillé d’entourer `*` de guillemets pour éviter les soucis dans un shell.

**Explication :**

- On attend trois arguments : un nombre, un opérateur, un autre nombre.
- `atof` convertit une chaîne en `double`.
- On utilise un `switch` pour déterminer l'opération à effectuer.
- On gère la division par zéro.

---

## Notes importantes

- Indexation de `argv` :
    - `argv[0]` est toujours le nom du programme.
    - Les arguments commencent à `argv[1]`.
- Vérification des arguments :
    - Toujours vérifier que le nombre d'arguments est correct avant de les utiliser.
    - Fournir des messages d'erreur clairs à l'utilisateur.
- Conversion des types :
    - Les arguments sont des chaînes de caractères.
    - Utilisez `atoi`, `atol`, `atof` ou `strtol`, `strtod` pour les convertir.

---

## Fonctions de conversion

- **`atoi(char *str)`** : convertit une chaîne en `int`.
- **`atol(char *str)`** : convertit une chaîne en `long`.
- **`atof(char *str)`** : convertit une chaîne en `double`.
- **`strtol(char *str, char **endptr, int base)`** : convertit une chaîne en `long` avec plus de contrôle.
- **`strtod(char *str, char **endptr)`** : convertit une chaîne en `double` avec plus de contrôle.

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
	if (argc != 2)
	{
		fprintf(stderr, "Usage : %s nombre\n", argv[0]);
		return (1);
	}

	char *endptr;
	long nombre = strtol(argv[1], &endptr, 10);

	if (*endptr != '\0')
	{
		fprintf(stderr, "Erreur : '%s' n'est pas un nombre valide.\n", argv[1]);
		return (1);
	}

	printf("Vous avez entré le nombre : %ld\n", nombre);

	return (0);
}
```

**Explication :**

- `strtol` permet de détecter si la conversion s'est bien passée en vérifiant si `*endptr` est `'\0'`.

---

## Les limitations de `argc` et `argv`

- Taille des arguments : les arguments de la ligne de commande ont une taille maximale qui dépend du système d'exploitation.
- Sécurité : soyez vigilant avec les entrées de l'utilisateur. Ne faites pas confiance aux arguments sans les valider.

---

## Conclusion

L'utilisation de `argc` et `argv` est essentielle pour créer des programmes interactifs en C qui peuvent accepter des entrées de l'utilisateur via la ligne de commande. En comprenant comment ils fonctionnent, vous pouvez :

- Personnaliser le comportement de vos programmes.
- Traiter des données d'entrée sans avoir besoin de recompiler le code.
- Créer des outils utiles qui peuvent être utilisés dans des scripts ou d'autres programmes.

**Conseils pour maîtriser `argc` et `argv` :**

- Pratiquez : écrivez des programmes qui utilisent différents types d'arguments.
- Validez les entrées : toujours vérifier et valider les arguments pour éviter les erreurs ou les failles de sécurité.
- Lisez la documentation : les fonctions comme `strtol` offrent des options avancées pour gérer les conversions.