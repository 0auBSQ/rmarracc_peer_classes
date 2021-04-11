# Challenge 1

## Enonce

Entrée :
- Un fichier redirigé sur l'entrée standard qui contient la définition d'un arbre
```
42
a
b
c
...
70
a-b
a-c
...
```

```
Avec :
Ligne 1 : Nombre N de noeuds
Ligne 2 - (N + 1) : Les noms de chaque noeud suivi d'un saut de ligne
Ligne (N + 2) : Nombre M de liens
Lignes (N + 3) - (M + 4) : Lien entre deux noeuds, représentant leur noms separés par -
```
- Sortie
```
0
1
1
...
```

```
Avec :
Ligne 1 - N : Profondeur de chaque noeud (affichés dans l'ordre donné en entrée)
```

Contraintes spécifiques :
- Complexité linéaire au maximum (O(N))
- Pas de parcours d'arbre récussif (doit être fait à partir d'une methode itérative)

Language libre, vous pouvez pull request vos solutions dans le dossier `Challenges` avec pour nom `challenge1_pseudo.[extention]`
