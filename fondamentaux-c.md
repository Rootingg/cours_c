# Fondamentaux du langage C

## Concepts essentiels

### Sections mémoire

**Segment de texte**
Contient les instructions du programme qui doivent être exécutées. Ce segment stocke le code nécessaire à l'utilisation de la mémoire RAM par l'application.

**mmap (Memory Mapping)**
Technologie permettant d'associer des zones mémoire à des fichiers sur le disque dur. Particulièrement efficace pour charger rapidement des données ou partager de la mémoire entre différents processus.

### Composants système

**Noyau (Kernel)**
Élément central du système d'exploitation qui assure la gestion des ressources matérielles et facilite les communications entre les composants logiciels et physiques.

**MMU (Memory Management Unit)**
Composant matériel essentiel qui transforme les adresses virtuelles en adresses physiques et assure la protection des espaces mémoire.

## Traduction du code

### Processus de compilation

La transformation du code C en langage assembleur s'effectue pendant la phase de compilation. Durant cette étape, le code source est converti en instructions machine directement exécutables par le processeur.

### Manipulation des chaînes en mémoire

Pendant l'exécution d'un programme, les chaînes de caractères sont stockées dans la mémoire et leur position initiale peut être utilisée.

* **argv** : Référence l'adresse initiale du tableau contenant les arguments fournis au programme.
* **argc** : Indique le nombre d'arguments transmis via la ligne de commande.
* **RSI** : Registre spécifique contenant l'adresse de départ de la liste de chaînes de caractères.

### Structure d'un programme C

```c
int main(int argc, char **argv) {
  return 0;
}
```

L'instruction `return 0;` déclenche un appel système `exit` qui signale la fin du programme.

## Déclarations et appels système

### Prototypes de fonctions

Un prototype permet de spécifier une fonction avant son implémentation.

Exemples de prototypes pour des fonctions système courantes:
```c
int write(int, char*, int);
int exit(int);
```

### Variables d'environnement

La variable `environ` offre un accès aux paramètres d'environnement d'un programme en C.

## Outils de diagnostic

**strace**
Utilitaire qui affiche tous les appels système effectués par un programme, facilitant la compréhension de son interaction avec le noyau.

## Manipulation des chaînes en C

Pour déterminer la longueur d'une chaîne de caractères, on recherche généralement le caractère nul `\0` (valeur 0 en mémoire) qui marque sa fin.

En C, les paramètres sont transmis aux fonctions par valeur (par copie), ce qui signifie que les variables originales restent intactes sauf indication contraire.

## Documentation

* **man 1 exit** : Documentation relative aux commandes utilisateurs.
* **man 2** : Documentation concernant les appels système du noyau.
* **man 3** : Documentation des fonctions de la bibliothèque standard C.

La fonction `exit` est définie dans l'en-tête `stdlib.h`.

## Format de fichiers ELF

Le format ELF (Executable and Linkable Format) est le standard utilisé par Linux pour les fichiers exécutables, les bibliothèques partagées et les objets.

## Gestion des erreurs

**SIGSEGV**
Signal envoyé quand un programme tente d'accéder à une région mémoire non autorisée, provoquant son arrêt immédiat.

## Gestion mémoire

**free -h**
Commande Linux qui montre l'état actuel de la mémoire RAM et de l'espace d'échange.

**Valeur immédiate**
Constante directement intégrée dans l'instruction machine, utilisée dans les opérations du processeur.

**Swap (mémoire d'échange)**
Zone du disque dur utilisée comme extension de la RAM lorsque celle-ci est saturée. Principalement utilisée pour les processus inactifs, car les programmes en cours d'exécution nécessitent un accès rapide à la mémoire principale.

## Protection mémoire

**Canary**
Technique de sécurité contre les débordements de tampon. Si la valeur du canary est modifiée, le programme s'arrête immédiatement pour éviter une exploitation malveillante.

## Registres et exécution

**RIP (Registre d'instruction)**
Registre qui pointe vers l'instruction actuellement exécutée. Sa valeur est mise à jour dynamiquement durant l'exécution du programme.

**Instructions assembleur**
Une instruction en langage assembleur peut occuper plusieurs octets en mémoire, selon son code d'opération et ses paramètres.
