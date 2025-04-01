# Concepts Avancés du C

## Types de données spécifiques

### `unsigned char`
Variable de type caractère non signé pouvant stocker des valeurs comprises entre 0 et 255. Particulièrement adapté pour la manipulation de données binaires brutes et le traitement d'informations liées aux couleurs.

### `size_t`
Type de données spécialement conçu pour représenter des tailles et des longueurs. Couramment utilisé avec les fonctions `strlen()` et `sizeof()` pour garantir une compatibilité optimale.

## Évolution et compatibilité du langage

Le C a été développé avec une forte attention à la rétrocompatibilité, mais des évolutions existent entre les différentes versions standardisées. La documentation officielle est maintenue par l'ISO et son accès complet est généralement payant, à l'exception des versions préliminaires qui sont disponibles gratuitement.

## Déclaration et organisation des types

### `typedef`
Mécanisme permettant de créer des synonymes de types existants, facilitant ainsi la lecture du code et sa maintenance.

```c
typedef struct {
    int x;
    int y;
} Point;
```

### Structure classique
Méthode standard pour définir un nouveau type composé:

```c
struct nom {
    int x;
    int y;
};
```

Lors de la phase de compilation, les identificateurs de variables sont éliminés, le compilateur ne conservant que les décalages mémoire (offsets) et les informations de type associées.

## Optimisation de la mémoire

Les compilateurs alignent automatiquement les structures sur des frontières correspondant à des puissances de 2 pour optimiser les performances d'accès mémoire. Cette optimisation peut parfois entraîner une consommation mémoire plus importante si elle n'est pas correctement gérée.

## Étape de prétraitement

Les directives commençant par le symbole `#` sont traitées avant la compilation proprement dite.

Exemples courants:
```c
#define PI 3.14159
#include <stdio.h>
```

## Exemple de manipulation de structures

```c
#include <math.h>
#include <stdio.h>

struct point {
    int x;
    int y;
};

double point_dist_from_point(struct point self, struct point other) {
    return sqrt(pow(self.x - other.x, 2) + pow(self.y - other.y, 2));
}

double point_dist_from_origin(struct point self) {
    struct point origin = {0, 0};
    return point_dist_from_point(self, origin);
}

void point_print(struct point self) {
    printf("Point(x=%d, y=%d)\n", self.x, self.y);
}

int main() {
    struct point p1 = {10, 11};
    point_print(p1);
    return 0;
}
```

## Bibliothèque standard et gestion de fichiers

L'en-tête `stdint.h` permet de définir des types à taille fixe comme `int32_t` et `uint8_t`, assurant ainsi une portabilité accrue entre différents systèmes d'exploitation.

### Manipulation de fichiers sous Linux

Sous Linux, tout élément du système est représenté comme un fichier, y compris les périphériques matériels.

Exemple d'ouverture et fermeture de fichier:
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *file = fopen("test.txt", "r");
    if (!file) {
        perror("Erreur pendant l'ouverture du fichier");
        return EXIT_FAILURE;
    }
    fclose(file);
    return EXIT_SUCCESS;
}
```

## Outils de développement C

* **readelf**: Analyse en détail les en-têtes de format ELF d'un fichier binaire.
* **strace**: Visualise l'ensemble des appels système réalisés par un programme.
* **gdb**: Outil complet de débogage pour programmes GNU.
* **make**: Automatise la chaîne de compilation et de construction des projets.
* **objdump**: Désassemble les fichiers binaires pour analyse.
* **gcc**: Compilateur standard du projet GNU.
* **nm**: Affiche la table des symboles des fichiers exécutables.
* **valgrind**: Détecte les problèmes de fuite mémoire et autres erreurs.

## Gestion avancée de la mémoire

### Différences fondamentales entre pile et tas

La **pile (stack)** est réservée au stockage temporaire des variables locales et à la gestion de l'exécution des fonctions, tandis que le **tas (heap)** est dédié aux allocations dynamiques effectuées via `malloc()` et libérées par `free()`.

### Appels système de gestion mémoire

* **brk()**: Permet d'ajuster la limite supérieure du segment de données (heap).
* **malloc()/free()**: Fonctions d'allocation et libération dynamique de mémoire.

Exemple d'utilisation:
```c
#include <stdlib.h>
#include <stdio.h>

int main() {
    int *ptr = malloc(sizeof(int) * 10);
    if (!ptr) {
        perror("Échec d'allocation mémoire");
        return EXIT_FAILURE;
    }
    free(ptr);
    return EXIT_SUCCESS;
}
```

## Formatage de sortie

* **%.15f**: Affiche un nombre à virgule flottante avec précision de 15 décimales.
* **%02d**: Formate un entier sur 2 positions avec remplissage par des zéros (exemple: 5 devient 05).
