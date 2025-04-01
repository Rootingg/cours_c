# Architecture et Composants du C
**Date: 03/03/2025**

## Éléments fondamentaux pour l'exécution du C

Le langage C s'appuie sur deux composants essentiels:

### Processeur (CPU)
Le processeur traite les données de façon efficace grâce à:

* **L'ALU (Unité Arithmétique et Logique)**: Exécute diverses opérations mathématiques, notamment:
  * AND
  * OR
  * NOT
  * SHL (Shift Left)
  * SHR (Shift Right)
  * Addition
  * *et autres*

* **Les registres**: Espaces de stockage temporaire permettant de conserver des valeurs pour les utiliser ensuite dans les calculs de l'ALU.

### Mémoire vive (RAM)
Cette mémoire volatile stocke les données et contient principalement:

* **La stack (pile)**: Structure organisée contenant la séquence des processus à exécuter
* **Le tas (heap)**: Zone mémoire moins structurée, utilisée comme tampon, caractérisée par son organisation moins rigoureuse
* **Variables**: Différents types de données stockées sous forme binaire exclusivement, nécessitant une conversion préalable

## Interface utilisateur

**Terminal**
* Interface permettant aux utilisateurs d'interagir facilement avec la machine
* Plusieurs terminaux peuvent être connectés simultanément
* Outil principalement utilisé pour le débogage, mais non essentiel au fonctionnement
* Aujourd'hui, les terminaux sont virtualisés et communiquent avec le noyau du système

**Protocoles de communication**
* Affichage: généralement en format ASCII
* Entrées clavier: protocole distinct

## Organisation de la mémoire

### Types de données fondamentales

#### Caractères
Plusieurs encodages permettent de représenter les caractères en binaire:

* **ASCII**: Format historique encodant les caractères sur 1 octet (8 bits), mais n'utilisant initialement que 4 bits. Extension ultérieure avec les codes ASCII étendus.
  * Plages importantes:
    * 0x30 à 0x39: Chiffres 0 à 9
    * 0x41 à 0x5A: Lettres majuscules A à Z
    * 0x61 à 0x7A: Lettres minuscules a à z
  * Caractères de contrôle (comme CR - Carriage Return 0xD): Permettent d'effectuer des actions spécifiques

* **UTF-8**: Format optimisé reconnaissable par ses caractéristiques:
  * Premier octet commençant toujours par 11
  * Octets suivants commençant par 10
  * Format le plus répandu sur le web actuellement

* **UTF-32**: Utilise 4 octets par caractère, permettant de représenter un nombre beaucoup plus important de symboles

#### Nombres
* Un nombre occupe basiquement 1 octet
* Sur les processeurs 64 bits, un nombre peut atteindre 8 octets
* Le stockage binaire natif ne permet que les nombres positifs, d'où l'utilisation de techniques spécifiques:
  * **Nombres positifs**: Valeurs de 0 à 18446744073709551615 (2^64)
  * **Nombres négatifs**:
    * Solution du bit de signe: problématique à cause de l'impossibilité de réaliser certaines opérations et de la double représentation du zéro
    * Solution retenue: complément à 2 pour les nombres négatifs
  * **Nombres à virgule**: Représentés avec 1 bit de signe, 52 bits pour la mantisse et 11 bits pour l'exposant (en valeur biaisée)

#### Instructions
Identifiées par des codes spécifiques qui correspondent à des fonctions prédéfinies, utilisant une table de correspondance.

## Organisation de la mémoire: Heap et Stack

Ces deux structures contiennent les instructions à exécuter, organisées en pile. Elles sont dynamiques et gérées par le programme via le CPU.

| Heap | Stack |
|------|-------|
| Située en fin de RAM et remontant | Croît du haut vers le bas |
| S'agrandit en déplaçant la limite (BRK) | |
| Contient des objets à durée de vie longue | Stocke des objets temporaires à traitement rapide |
| Durée de vie des objets gérée indépendamment | Durée de vie liée à celle des fonctions |

La stack est particulièrement importante, comme l'illustre l'exécution de la fonction de Fibonacci.

## Fonctionnement du processeur

### Registres
Cases mémoire de la taille du système (64 bits sur un ordinateur 64 bits) utilisables par les instructions.

| Acronyme | Nom | Fonction |
|----------|-----|----------|
| IP | Instruction Pointer | Contient l'adresse de l'instruction courante, s'incrémente automatiquement |
| SP | Stack Pointer | Indique la taille de la pile |
| RDI{X} | | Registre utilisé pour les paramètres de fonctions |
| RAX | | Registre utilisé pour les valeurs de retour |
| R{X} | | Format générique |

### Instructions
Pour faire exécuter des opérations au processeur, on utilise des identifiants correspondant à des instructions prédéfinies.

| Acronyme | Nom | Fonction | Arguments |
|----------|-----|----------|-----------|
| ADD | Addition | Additionne deux valeurs | Registre contenant la valeur à ajouter |
| POP | | Extrait une valeur de la stack vers un registre | Registre de destination |
| SYSCALL | | Effectue un appel système | |
| MOV | | Déplace une valeur dans un registre | Valeur immédiate, registre destination |
| PUSH | | Place la valeur d'un registre dans la stack | Registre |
| MOVRR | Registre vers Registre | Transfère entre registres | Registre source, registre destination |
| SUB | Soustraction | Soustrait une valeur | Registre contenant la valeur à soustraire |
| RET | Restore IP | Affecte une nouvelle valeur à IP | Nouvelle valeur IP |
| CMP | Comparaison | Compare un registre à une valeur | Registre et valeur à comparer |
| LEAVE | | Restaure l'état précédent de la stack | |
| JE | Jump Equal | Effectue un saut conditionnel | |

## Communication avec le système d'exploitation

Pour terminer un programme, il faut signaler au noyau une demande d'arrêt via les appels système (syscalls).

### Syscalls
Ces appels possèdent des identifiants spécifiques:

| Nom | Fonction | R1/ID | R2 | R3 | R4 |
|-----|----------|-------|----|----|-----|
| exit | Termine le processus | 1 | Code d'erreur (0: OK, 1: Erreur) | X | |
| write | Affiche du texte | 2 | Type de message (1: standard, 2: erreur) | Adresse mémoire du premier caractère | Nombre de caractères à afficher |
| read | | | | | |

Le document inclut également un exemple de code assembleur implémentant la fonction Fibonacci.
