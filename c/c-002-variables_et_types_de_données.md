<img height="50px" align="right" src="https://raw.githubusercontent.com/fchavonet/fchavonet/main/assets/images/logo-c.png" alt="C logo">

# C - Variables et types de données

En langage C, les variables permettent de stocker et manipuler des données en mémoire. Avant toute utilisation, chaque variable doit être déclarée avec un type qui précise la nature des données qu’elle contiendra : un nombre entier, un caractère, une valeur flottante...

---

## Qu’est-ce qu’une variable ?

Une variable est une zone mémoire nommée, réservée pour contenir une valeur temporaire.
En C, chaque variable doit être déclarée explicitement avec un type.

**Exemple :**

```c
int age = 40;
char lettre = 'A';
float temperature = 37.5;
```

---

## Déclaration et initialisation

Déclaration : on indique le type et le nom de la variable.
<br>
Initialisation : on lui donne une valeur dès sa création.

```c
int a;        // Déclaration.
a = 42;       // Affectation.
```

```c
int a = 42;   // Déclaration + initialisation.
```

---

## Principaux types de données en C

| **Type** | **Description**        | **Taille** | **Plage de valeurs approximative** | **Exemple**                |
|----------|------------------------|------------|------------------------------------|----------------------------|
| `char`   | Caractère ASCII        | 1 octet    | -128 à 127                         | `char c = 'A';`            |
| `short`  | Nombre entier court    | 2 octets   | -32768 à 32767                     | `short s = -42;`           |
| `int`    | Nombre entier standard | 4 octets   | -2147483648 à 2147483647           | `int n = 42;`              |
| `long`   | Nombre entier long     | 8 octets   | ≈ -9×10¹⁸ à +9×10¹⁸                | `long l = 1000000000;`     |
| `float`  | Nombre réel simple     | 4 octets   | 1.17549 × 10⁻³⁸ à 3.40282 × 10³⁸   | `float f = 3.14;`          |
| `double` | Nombre réel double     | 8 octets   | 2.22507 × 10⁻³⁰⁸ à 1.79769 × 10³⁰⁸ | `double d = 3.1415926535;` |

> 📌 La taille peut varier selon l'architecture (32 bits, 64 bits...).

---

## Constantes (`const`)

Une constante est une variable dont la valeur ne peut pas changer après initialisation.

```c
const float PI = 3.14159;
// PI = 3.2; ⚠️ Erreur : modification interdite !
```

---

## Types modifiés (`signed`, `unsigned`)

En C, on peut modifier la taille et/ou le signe d’un entier avec certains mot-clés.

```c
signed int a;
unsigned int b;
signed char c;
unsigned char d;
```

| **Modificateur** | **Effet**                                                                                | 
|------------------|------------------------------------------------------------------------------------------|
| `signed`         | Permet de stocker des valeurs positives et négatives (c’est le comportement par défaut). |
| `unsigned`       | Ne stocke que des valeurs positives (double la plage positive)  .                        |

> 📌 Écrire `int` est équivalent à écrire `signed int`.

---

## Format d’affichage (`printf`)

Chaque type a son spécificateur de format pour être affiché avec `printf` :

| **Type** | **Format** | 
|----------|------------|
| `char`   | `%c`       |
| `int`    | `%d`       |
| `float`  | `%f`       |
| `double` | `%lf`      |

**Exemple :**

```c
#include <stdio.h>

int main(void)
{
	int a = 42;
	float b = 3.14;

	printf("a = %d\n", a);
	printf("b = %.2f\n", b);

	return (0);
}
```

---

## Portée des variables

La portée d'une variable détermine où dans le code on peut accéder à cette variable. En C, la portée est liée à l’endroit où la variable est déclarée.

### Portée locale (à un bloc)

Une variable déclarée dans une fonction ou dans une paire d’accolades `{}` n’existe que dans ce bloc.
On dit qu’elle a une portée locale.

```c
#include <stdio.h>

int main(void)
{
	int x = 10; // Portée : uniquement dans main().

	if (x > 5)
	{
		int y = 20; // Portée : uniquement dans ce if.
		printf("x = %d, y = %d\n", x, y);
	}

	// printf("y = %d\n", y); // ⚠️ Erreur : y n'existe plus ici.

	return (0);
}
```

> 📌 Une variable définie dans un bloc disparaît une fois ce bloc terminé.

### Portée globale

Une variable déclarée en dehors de toute fonction a une portée globale : elle est accessible dans tout le fichier `.c` à partir du point où elle est déclarée.

```c
#include <stdio.h>

int compteur = 0; // Portée : globale.

void afficher_compteur(void)
{
	printf("compteur = %d\n", compteur);
}

int main(void)
{
	compteur = 5;
	afficher_compteur(); // Affiche 5.
	return (0);
}
```

---

## Bonnes pratiques

- Toujours initialiser vos variables.
- Nommez-les de façon claire (`nombre_utilisateur`, pas juste `n`).
- Utilisez `float` pour économiser de la mémoire, `double` pour plus de précision.
- Faites attention aux formats `%d`, `%f`, etc. dans `printf` et `scanf`.
- Évitez d’utiliser des variables non initialisées (valeurs indéfinies).

---

## Erreurs classiques

```c
int x;
printf("%d\n", x); // ⚠️ Non initialisé → valeur aléatoire.

unsigned char n = 255;
n = n + 1; // ⚠️ Overflow : n devient 0.

int age;
scanf("%f", &age); // ⚠️ Mauvais format ! "%f" attend un float.
```

> 📌 Quand un entier `unsigned` dépasse sa valeur maximale, il revient à 0 (comportement cyclique).

---

## Exemple complet

```c
#include <stdio.h>

int score_global = 100;

int main(void)
{
	char lettre = 'Z';
	int points = 42;
	float moyenne = 15.75;
	const double pi = 3.14159;

	printf("Lettre : %c\n", lettre);
	printf("Points : %d\n", points);
	printf("Moyenne : %.2f\n", moyenne);
	printf("Pi : %.5lf\n", pi);
	printf("Score global : %d\n", score_global);

	return (0);
}
```

---

## Conclusion

Les variables sont les éléments de base sur lesquels repose tout programme en C. Comprendre comment les déclarer, les initialiser, et surtout comment fonctionne leur portée est essentiel pour écrire du code fiable et efficace. À travers elles, vous commencez à interagir avec la mémoire, à structurer vos données et à donner vie à votre logique métier. Maîtriser les types permet d’écrire du code plus clair, plus précis et plus adapté à vos besoins. Une bonne gestion des variables est aussi indispensable pour aller plus loin, notamment lorsqu’il s’agira de manipuler des tableaux, d’échanger des données entre fonctions ou encore de gérer dynamiquement la mémoire. En apprenant à bien nommer, bien typer et bien positionner vos variables, vous posez les fondations solides d’un code propre, lisible et maintenable.