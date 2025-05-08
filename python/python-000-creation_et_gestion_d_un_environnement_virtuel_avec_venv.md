<img height="50px" align="right" src="https://raw.githubusercontent.com/fchavonet/fchavonet/main/assets/images/logo-python.png" alt="Python logo">

# Python - Création et gestion d’un environnement virtuel avec `venv`

Quand on travaille sur plusieurs projets Python, un environnement virtuel permet d’isoler les dépendances de votre projet du reste de votre système. En créant un environnement dédié, vous vous assurez que les bibliothèques installées pour ce projet n’affectent pas les autres (et vice-versa). Python fournit pour cela le module `venv` qui permet de créer un environnement virtuel dans un dossier donné.

---

## Installation de `venv` sous Linux

```bash
sudo apt install python3-venv -y
```

> 📌 Si vous utilisez une sandbox Docker créée à partie du cours "[Configuration d’une sandbox Ubuntu avec Docker](https://github.com/fchavonet/holbertonschool-concepts/blob/main/miscellaneous/mac-001-configuration_d_une_sandbox_ubuntu_avec_docker.md)", `sudo` ne devra pas être utilisé dans la commande.

---

## Création du projet test

1. Ouvrez votre terminal.

2. Créez un nouveau répertoire pour le projet :

```bash
mkdir holbertonschool-test_github_api
```

3. Accédez au répertoire nouvellement créé :

```bash
cd holbertonschool-test_github_api
```

---

## Création de l’environnement virtuel

```bash
python3 -m venv venv
```

Cette commande crée un dossier `venv/` contenant une copie de l’interpréteur Python et de `pip`.
<br>
Le `-m` indique que l’on exécute le module nommé `venv` et pas un fichier script de base.


> 📌 Il est courant de nommer le dossier de l’environnement virtuel `.venv` (avec un point au début du nom). Cette convention a deux avantages : le dossier est caché par défaut dans l’explorateur de fichiers, et il est automatiquement ignoré par certains outils ou frameworks. Dans notre exemple, nous ne mettons pas le point pour vous permettre d'accéder facilement au dossier et de l'explorer si la curiosité vous en dit.

---

## Activation de l’environnement

**Linux / macOS :**

```bash
source venv/bin/activate
```

**Windows (PowerShell) :**


```powershell
.\venv\Scripts\Activate.ps1
```

Une fois activé, vous verrez le préfixe `(venv)` devant votre invite de commande : cela confirme que vous travaillez dans l’environnement isolé.

Après création de l’environnement, il est souvent utile de mettre à jour `pip` :

```bash
pip install --upgrade pip
```

> 📌 Ne versionnez pas le dossier d’environnement virtuel `venv` dans votre dépôt Git. Pensez à ajouter le nom de ce dossier (par exemple `venv/` ou `.venv/`) dans le fichier `.gitignore` de votre projet afin qu’il soit ignoré par le contrôle de version. Vous pouvez notamment utiliser le fichier [`.gitignore`](https://github.com/github/gitignore/blob/main/Python.gitignore) officiel pour Python proposé par GitHub (qui inclut déjà l’exclusion de `.venv`) comme point de départ.

---

## Installation d'un package

Dans notre exemple, nous allons tester la réponse de l'API GitHub, pour ce faire nous avons besoin du module `requests`:

```bash
pip install requests
```

Ici, le package `requests` sera disponible uniquement dans l'environnement `venv`.

---

## Test simple

1. Créez un fichier `test_github_api.py` à la racine du projet avec ce contenu :

```python
import requests

try:
    response = requests.get("https://api.github.com")

    if response.status_code == 200:
        print("✅ Succès : l'API GitHub a répondu correctement.")
    elif response.status_code == 403:
        print("⚠️ Erreur 403 : accès interdit. Vérifiez si vous êtes bloqué ou rate limité.")
    elif response.status_code == 404:
        print("❌ Erreur 404 : ressource introuvable.")
    elif response.status_code >= 500:
        print(f"🚨 Erreur serveur ({response.status_code}) : GitHub rencontre un problème.")
    else:
        print(f"ℹ️ Réponse inattendue : code {response.status_code}")

except requests.exceptions.RequestException as e:
    print(f"❌ Une erreur de connexion est survenue : {e}")
```

2. Exécutez-le :

```bash
python test_github_api.py
```

Vous devriez voir le message correspondant au statut de l'API GitHub dans votre Terminal.

---

## Congeler les dépendances

Après avoir installé les bibliothèques nécessaires dans votre environnement virtuel, il est recommandé de congeler les dépendances de votre projet. Cela signifie générer un fichier listant précisément les paquets installés et leurs versions, afin de pouvoir recréer le même environnement ailleurs. Par convention, ce fichier est nommé `requirements.txt`.

```bash
pip freeze > requirements.txt
```

---

## Désactivation de l’environnement

Quand vous avez fini de travailler sur votre projet :

```bash
deactivate
```

Le prompt revient à la normale, Python et pip pointent de nouveau vers votre installation globale.

---

## Recréer l’environnement ailleurs

Sur une autre machine (ou après avoir supprimé le dossier `venv` du répertoire de test) :

```bash
python3 -m venv venv
```

```bash
source venv/bin/activate
```

> 📌 Ou `.\venv\Scripts\Activate.ps1` sous Windows.

```bash
pip install -r requirements.txt
```

Cela installera l’ensemble des packages listés dans le fichier `requirements.txt` (dans notre exemple, le package `requests`).

---

## Conclusion

L’utilisation de `venv` vous permet :

- D’isoler vos projets.
- D’éviter les conflits de versions.
- De garantir que chaque collaborateur ou serveur reproduise exactement le même environnement.

> 📌 Utilisez toujours un environnement virtuel pour vos projets Python, même pour les plus petits scripts. Cela garantit un environnement propre, et vous évite des conflits avec d’autres projets ou bibliothèques installées globalement.

C’est une étape essentielle pour un développement Python professionnel et fiable.
