# Structures de données et optimisation algorithmique

(D'eventuelles prises de notes ou ressources seront ajoutées à l'issue de la peerclass)

## Sommaire

* I - Complexité algorithmique
    * Definition
    * Notation O
    * Analyse de complexité
* II - Structures de données
    * Liste (List)
    * Pile (Stack)
    * File (Queue)
    * Arbre (Tree)
    * Pour aller plus loin
* III - Optimisation algorithmique
    * Erreurs fréquentes
    * Optimisation d'une fonction récursive
* IV - Introduction à la théorie des graphes
    * Implémentations
    * Notions importantes
    * Pour aller plus loin

## I - Complexité algorithmique

### Definition

```
La complexité algorithmique peut être de temps ou d'espace par rapport à une (ou plusieurs) entrée de taille N.
La complexité de temps d'un algorithme représente le nombre d'opérations effectuées en fonction de la taille des entrées ou son ordre de grandeur.
La complexité de mémoire d'un algorithme représente la quantitée en mémoire allouée en fonction de la taille des entrées ou son ordre de grandeur.

La complexité d'un algorithme est notée T(N) avec N taille de l'entrée.
On utilise généralement la notation O pour l'évaluer, étant la notation la plus représentative pour évaluer les performances d'un algorithme sur le papier.
```

### Notation O

- Definition
```
La notation O représente l'ordre de grandeur de la complexité (en temps ou en mémoire) d'un algorithme dans le pire des cas.
On ne conserve que le plus "gros" facteur afin d'avoir les informations minimales pour l'évaluation.

Examples :
T(N) = 8N^2 + 15N + 84 => O(N^2)
T(N) = 158000000 => O(1)
T(N) = 8log(N) * N + N => O(Nlog(N))
```

### Analyse de complexité

- Methodologie de base pour la complexité de temps
```
Une affectation, comparaison ou autre opération de base équivaut à une opération.
Un appel à une fonction équivaut au nombre d'opérations de celle ci.
Le nombre d'opérations d'une boucle équivaut a son nombre d'itérations multiplié au nombre d'opérations de chaque opération qui y est contenu.
```

- Examples
```
void functionA(int *arr, int index) {
    arr[index] = 1;
}

# Attribue 1 à une valeur d'un tableau d'int dont l'addresse est passée en parametre, complexité en temps O(1) et en mémoire O(1)
```

```
void functionB(int *arr, int size) {
    for (int i = 0; i < size; i++) {
        arr[i] = 1;
    }
}

# Attribue 1 à toutes les valeurs d'un tableau d'int dont l'addresse est passée en parametre, complexité en temps O(N) et en mémoire O(1)
```

```
int *functionC(int v, int size) {
    int *arr = (int *)malloc(sizeof(int) * size);
    for (int i = 0; i < size; i++) {
        arr[i] = v;
    }
    return arr;
}

# Crée un tableau d'int et attribue la valeur v dans toutes ses cases, complexité en temps O(N) et en mémoire O(N)
```

```
int **functionD(int size) {
    int **arr = (int **)malloc(sizeof(int*) * size);
    for (int i = 0; i < size; i++) {
        arr[i] = (int *)malloc(sizeof(int) * size);
        for (int j = 0; j < size; j++)
            arr[i][j] = i * j;
    }
    return arr;
}

# Crée une matrice d'int de taille N x N et attribue dans chaque case le produit de ses coordonnées, complexité en temps O(N^2) et en mémoire O(N^2)
```

### Sources supplémentaires

- https://openclassrooms.com/fr/courses/1467201-algorithmique-pour-lapprenti-programmeur/1467358-la-notion-de-complexite
- https://info.blaisepascal.fr/nsi-complexite-dun-algorithme
- https://www.youtube.com/watch?v=v4cd1O4zkGw&ab_channel=HackerRank

## II - Structures de données

### Liste (List)

- Example en C
```
typedef struct          list_node_s {
    int                 content;
    struct list_node_s  *next;
}                       list_node_t;

typedef struct      list_s {
    list_node_t     *begin;
    list_node_t     *end;
    size_t          size;
}                   list_t;

list_node_t  *createNode(int content) {
    list_node_t  *l = (list_node_t *)malloc(sizeof(list_node_t));
    l->content = content;
    l->next = NULL;
    return l;
}

list_t  *createList() {
    list_t *l = (list_t *)malloc(sizeof(list_t));
    l->begin = NULL;
    l->end = NULL;
    l->size = 0;
    return l;
}

void listPushFront(list_t **alst, int v) {
    list_node_t *new = createNode(v);
    list_t *l = *alst;

    new->next = l->begin;
    l->begin = new;
    if (l->size == 0)
        l->end = l->begin;
    l->size++;
}

void listPushBack(list_t **alst, int v) {
    list_node_t *new = createNode(v);
    list_t *l = *alst;

    if (l->end)
        l->end->next = new;
    else
        l->begin = new;
    l->end = new;
    l->size++;
}

int listGetValueAtIndex(list_t **alst, int index) {
    list_t *l = *alst;
    list_node_t *bg = l->begin;

    if (l->size <= index)
        return (0);
    while (index > 0) {
        bg = bg->next;
        index--;
    }
    return bg->content;
}
```

- Complexité en temps des opérations
```
Accès : O(N)
Recherche : O(N)
Insertion : O(1)
Suppression : O(1)
```

### Pile (Stack)

- Caracteristiques et mnémonique
```
Structure de données LIFO (Last In First Out), le dernier entré est le premier sorti.
On peut s'imaginer une stack comme étant une pile d'assietes l'opération Push ajoute une assiete au sommet, tandis que l'opération Pop la récupère et la retire.

On représente ici la stack sous forme de liste, afin d'en réutiliser les opérations qui sont déjà en temps O(1).
Le premier élement de la liste représente le sommet de la pile.
```

- Example en C
```
typedef list_t stack_t;

stack_t *createStack() {
    return ((stack_t *)createList());
}

void stackPush(stack_t **astk, int v) {
    listPushFront((list_t **)astk, v);
}

int stackPop(stack_t **astk) {
    stack_t *s = *astk;

    if (!s->size)
        return 0;
    s->size--;
    int ret = s->begin->content;
    if (!s->size) {
        s->begin = NULL;
        s->end = NULL;
    }
    else {
        list_node_t *old = s->begin;
        s->begin = s->begin->next;
        free(old);
    }
    return ret;
}

int stackTop(stack_t **astk) {
    stack_t *s = *astk;

    if (!s->size)
        return 0;
    return s->begin->content;
}
```

- Complexité en temps des opérations
```
Top : O(1)
Push : O(1)
Pop : O(1)
```

### File (Queue)

- Caracteristiques et mnémonique
```
Structure de données FIFO (First In First Out), le premier entré est le premier sorti.
On peut s'imaginer une queue comme étant une file d'attente pour un magasin ou une attraction, les premières personnes entrées (Enqueue) sont les premières à accéder au lieu (Dequeue), tandis que les derniers arrivés attendent leur tour.

On représente ici la queue sous forme de liste, afin d'en réutiliser les opérations qui sont déjà en temps O(1).
Le premier élement de la liste représente le sommet de la file, le dernier élement la queue de la file.
```

- Example en C
```
typedef list_t queue_t;

queue_t *createQueue() {
    return ((queue_t *)createList());
}

void queueEnqueue(queue_t **aque, int v) {
    listPushBack((list_t **)aque, v);
}

int queueTop(queue_t **aque) {
    queue_t *q = *aque;

    if (!q->size)
        return 0;
    return q->begin->content;
}

int queueTail(queue_t **aque) {
    queue_t *q = *aque;

    if (!q->size)
        return 0;
    return q->end->content;
}

int queueDequeue(queue_t **aque) {
    return (stackPop((stack_t **)aque));
}
```

- Complexité en temps des opérations
```
Top : O(1)
Tail : O(1)
Enqueue : O(1)
Dequeue : O(1)
```

### Arbre (Tree)

- Caracteristiques et mnémonique
```
Structure de données sous forme d'arboréscence pouvant s'apparenter à un arbre généalogique/
Généralement représenté par une racine, un certain nombre de noeuds et un certain nombres de feuilles, suivant des relations de parenté.

Il existe une multitude d'implémentations d'arbres possible, la plus part ayant pour but d'amortir la complexité en temps des opérations sur la structure, comme par exemple les arbres rouges et noirs ou les arbres binaires de recherche (voir section "Pour aller plus loin" pour quelques ressources la dessus), un arbre n'est pas forcément binaire et peut avoir un nombre quelquonque de fils ou de parents, et est un sous-groupe des graphes en général.

Il est généralement conseiller d'utiliser une liste lorsqu'il y a des ajouts et des suppressions répétées (par exemple pour les instances des projectiles dans un jeu de tir), il est, au contraire, préférable d'utiliser un arbre binaire lorsque les ajouts et suppressions sont plus rares, mais les accès bien plus fréquents (par exemple pour une application de dictionnaire).
```

- Complexité en temps des opérations
```
Accès : O(log(N)) ~ O(N)
Recherche : O(log(N)) ~ O(N)
Insertion : O(1) ~ O(N)
Suppression : O(1) ~ O(N)

(La complexité dans les pires cas dépend de l'implémentation de l'arbre choisie et du type d'arbre en question, la liste ou l'arbre binaire en faisant partie)
```

### Pour aller plus loin

- HashMap (et comparaisons avec les arbres binaires de recherche)
```
https://www.youtube.com/watch?v=sfWyugl4JWA&ab_channel=CSDojo
https://afteracademy.com/blog/binary-search-tree-vs-hash-table
https://cs.stackexchange.com/questions/270/hash-tables-versus-binary-trees
```

- BST (Binary Search Tree)
```
https://www.tutorialspoint.com/data_structures_algorithms/binary_search_tree.htm
```

- Red and Black trees
```
https://www.cs.auckland.ac.nz/software/AlgAnim/red_black.html
https://www.youtube.com/watch?v=A3JZinzkMpk&ab_channel=MichaelSambol
```

### Sources supplémentaires

- https://www.bigocheatsheet.com/

## III - Optimisation algorithmique

### Erreurs fréquentes

- Appel à une fonction dans une boucle while
```
// Exemple de boucle à ne pas reproduire
while (strlen(s) != 0) {
  ...
}

// La plupart des interpreteurs/compileurs peuvent gérer ce cas implicitement, cependant si ça n'est pas le cas, chaque itération sur la condition de la // boucle aura la complexité des fonctions qui y sont appelées, ralentissant inutilement le programme.
```

- Rappel de la même fonction lorsqu'elle retourne le même resultat
```
size_t len = strlen(s)
if (strlen(s) < 5)
  ...

// La longeur de la chaine s est déjà stockée dans la variable len et n'a pas été changée avant la condition, on parcourt la chaine une fois de plus pour rien ici.
```

### Optimisation d'une fonction récursive

- Problèmes récurents dans les fonctions récursives
```
=> Les fonctions récursives génèrent une nouvelle stack frame (contenant chaque variable) à chaque appel dans la pile, la pile disposant d'une mémoire assez limitée et n'ayant pas le controle sur le débordement de celle-ci, le débordement de pile (stack overflow) peut survenir même en ayant un nombre d'appels qui peut sembler raisonnable.
=> Les implémentations récursives sont généralement plus lentes que celles itératives, la raison principale étant l'allocation de la stack frame.

Cependant, il y a plusieurs moyens d'y remedier.
```

- Tail Call Optimisation (TCO)
```
/* Cette optimisation n'est pas présente pour tout les languages/compilateurs, il est necessaire d'utiliser le flag -Ofast pour s'en servir en C par exemple.
L'optimisation principale apportée par la TCO est qu'au lieu d'allouer une nouvelle stack frame par appel, on réutilise directement celle présente en remontant dedans à chaque appel, étant donné que la récursion est terminale et que toutes les variables n'ont besoin que d'une instance. */

// Exemple :

// Factorielle sans TCO :
ssize_t factorialA(int n) {
  if (n <= 1)
    return 1;
  return n * factorialA(n - 1);
}

// On concerve ici la variable n à l'exterieur du nouvel appel à factorialA, empechant la réutilisation de la stack frame et donc l'utilisation de la TCO.

// Factorielle avec TCO, acc doit être initialisé à 1 :
ssize_t factorialB(int n, ssize_t acc) {
  if (n <= 1)
    return acc;
  return factorialB(n - 1, acc * n);
}

// Le retour recursif de factorialB est ici terminal, il n'y a aucune information supplémentaire après l'appel recursif, la TCO est donc applicable et la stack frame réutilisable.
```

- Programmation Dynamique (DP)

```
/* La programmation dynamique apporte un aspect plus reflechi sur l'optimisation d'une fonction récursive, en utilisant notamment la récurence avec des equations d'hérédité afin de la convertir en fonction itérative plus légère */

// Exemple (avec la factorielle) :

ssize_t factorialC(int n) {
  ssize_t ret = 1;

  for (int i = 2; i <= n; i++)
    ret = ret * i;

  return ret;
}

// Equations :
// Initialisation : fact(N) = 1 pour tout N <= 1
// Hérédité : fact(N) = fact(N - 1) * N pour tout N > 1
```

- Emulation de la récursivité en utilisant une stack
```
/* On peut également utiliser une stack implémentée soi même, permetant par exemple de l'allouer sur le tas ou de ne push que les variables necessaires au prochain appel, améliorant grandement les performances de celle-ci, a noter qu'il s'agit ici de mettre en place une methode itérative simulant le fonctionnement de la méthode récursive. */

// Exemple (Parcours d'arbre infixe avec une stack) :

void btreeInfixTraversal(btree_node_t *root, void (*f)(btree_node_t*)) {
  stack_t *s = createStack();

  btree_node_t *cur = root;

  while(s->size || cur) {
    if (cur) {
      stackPush(&s, cur);
      cur = cur->left;
    }
    else {
      cur = stackPop(&s);
      (*f)(cur);
      cur = cur->right;
    }
  }
}

// (Si vous reutilisez ce programme, il vous faudra remplacer l'int dans l'implémentation de la stack précédement fournie par un void* (afin de prendre tout type de pointeur), il vous faudra aussi implémenter le type btree_node_t qui contient un btree_node_t left et right
// On crée ici une stack qui va contenir tout les noeuds adjaçants non utilisés dans l'instant, puis on va appeller notre fonction successivement sur chaque triplet left-mid-right en partant du fond de l'arbre et en dépilant les noeuds au fur et à mesure.
```

- Memoization
```
/* Parmi les methodes d'optimisation, la memoization est un peu differente etant donné qu'elle consiste à mettre dans le cache (ou dans un dictionnaire) les resultats connus des executions précédentes d'une fonction afin de s'en resservir lors des executions suivantes, permetant de limiter la quantité de calcul de manière drastique dans certains cas d'utilisation. */

// Exemple en python avec fibonacci :

def fiboA(n, dico=dict()):
    if n not in dico:
        if n <= 1:
            dico[n] = n
        else:
            v1 = fiboA(n - 1, dico)
            v2 = fiboA(n - 2, dico)
            dico[n] = v1 + v2
    return dico[n]

```

### Sources supplémentaires

- https://medium.com/@williambdale/recursion-the-pros-and-cons-76d32d75973a
- http://wiki.c2.com/?TailCallOptimization


## IV - Introduction à la théorie des graphes

### Implémentations

```
Il y a 3 manières principales d'implémenter un graphe.
Avec |V| le nombre de points et |E| le nombre d'arrêtes :

- La matrice d'adjacence

=> Matrice de taille |V| * |V| ou chaque case à pour valeur 1 si les deux points sont reliés par une arrête, 0 sinon.
=> Complexité de mémoire O(|V|^2)
=> Complexité de temps :
- Ajouter/Retirer un point : O(|V|^2)
- Ajouter/Retirer une arrête : O(1)
- Verifier l'adjacence entre 2 points : O(1)

> Lent à l'ajout et à la suppression de points, rapide pour les arrêtes
> Test de voisinage très rapide

- La matrice d'incidence

=> Matrice de taille |V| * |E| ou chaque case à pour valeur 1 si le point de V (représenté sur un axe) est impliqué dans l'arrête de E (représentée sur l'autre axe)
=> Complexité de mémoire O(|V|*|E|)
=> Complexité de temps :
- Ajouter/Retirer un point : O(|V|*|E|)
- Ajouter/Retirer une arrête : O(|V|*|E|)
- Verifier l'adjacence entre 2 points : O(|E|)

> Lent à l'ajout et à la suppression (Necessité de copier la matrice afin de la réallouer)

- La liste d'adjacence

=> Liste des points de V contenant chacuns une sous-liste des points (ou de leur référence) avec lesquels ils sont adjacents.
=> Complexité de mémoire O(|V|+|E|)
=> Complexité de temps :
- Ajouter un point/une arrête : O(1)
- Retirer un point : O(|E|)
- Retirer une arrête : O(|V|)
- Verifier l'adjacence entre 2 points : O(|V|)

> Lent à la suppression (par necessité de parcourir toute la liste pour trouver l'element)
> Léger en mémoire, rapide à l'ajout
```

### Notions importantes

- Vocabulaire de base
```
- Arrête : Connection entre deux points d'un graphe
- Arc : Connection entre deux points d'un graphe unidirectionel (dans le cas d'un graphe orienté)
- Boucle : Arrête ou arc qui relie un point à lui même
- Points ajacents/voisins : Points reliés par une arrête ou un arc
- Degrée d'un point : Nombre d'arrêtes connectées à ce point, en entrée comme en sortie
- Chemin : Série de points et d'arrêtes/arcs représentant un parcours dans le graphe, sans que le même point y soit présent plusieurs fois
- Cycle : Chemin partant d'un point et y revenant
```

- Types de graphes
```
- Graphe orienté : Graphe pour lequel les points sont reliés par des arcs unidirectionels, ne permetant le passage que d'un côté
- Graphe pondéré : Graphe pour lequel chaque arrête ou arc à un poids
- Graphe simple : Graphe sans boucle ni arrête multiples (=> Pas 2 arrêtes entre les deux mêmes points)
- Graphe connexe : Graphe d'un seul tenant, sans sommets isolés
```

### Pour aller plus loin

- Plus court chemin
```
https://www.youtube.com/watch?v=rI-Rc7eF4iw&ab_channel=glassus (Dijkstra)
```

- Arbre de poids couvrant minimum
```
https://www.youtube.com/watch?v=71UQH7Pr9kU&ab_channel=MichaelSambol (Kruskal, utilisant la structure de donnée Union find)
https://www.youtube.com/watch?v=I0uiQyAs5G4&ab_channel=%C3%80lad%C3%A9couvertedesgraphes (Prim)
```
