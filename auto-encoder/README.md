
# Arbre d'Expression Mathématique

Ce projet contient un ensemble de fonctions pour manipuler des expressions mathématiques, les transformer en arbres binaires, les afficher et les reformer en expressions algébriques. Le projet utilise le module `ast` de Python pour parser les expressions et les convertir en une structure arborescente. 

## Prérequis


## Fonctionnalités

- **Convertir une expression en un arbre binaire.**
- **Visualiser l'arbre représentant l'expression.**
- **Reformer une expression algébrique à partir d'un arbre.**

---

## Structure du projet

Le projet contient les fonctions suivantes :

### 1. Classe `TreeNode`

```python
class TreeNode:
    def __init__(self, value, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right
```

- **But** : Cette classe représente les nœuds de l’arbre binaire.
- **Attributs** :
  - `value` : La valeur contenue dans le nœud. Elle peut être un opérateur (`+`, `*`, `-`, `/`) ou une constante/variable (`x`, `3`, etc.).
  - `left` : Le sous-arbre gauche, représentant la partie gauche d'une opération.
  - `right` : Le sous-arbre droit, représentant la partie droite d'une opération.

---

### 2. Fonction `expression_to_tree`

```python
def expression_to_tree(expr):
    parsed_expr = ast.parse(expr, mode='eval')
    return ast_to_tree(parsed_expr.body)
```

- **But** : Convertir une expression mathématique sous forme de chaîne de caractères en un arbre binaire de type `TreeNode`.
- **Input** : 
  - `expr` : Chaîne de caractères contenant une expression algébrique (par exemple, `"x * (x + 2) - 3 / x"`).
- **Output** : Un objet `TreeNode` représentant l'arbre correspondant à l'expression.
- **Use** : Cette fonction utilise `ast.parse` pour transformer l'expression en un arbre de syntaxe abstraite (AST), puis appelle `ast_to_tree` pour la convertir en un arbre binaire personnalisé.

---

### 3. Fonction `ast_to_tree`

```python
def ast_to_tree(node):
    if isinstance(node, ast.BinOp):
        op = {
            ast.Add: '+',
            ast.Sub: '-',
            ast.Mult: '*',
            ast.Div: '/'
        }[type(node.op)]
        return TreeNode(op, ast_to_tree(node.left), ast_to_tree(node.right))
    elif isinstance(node, ast.Constant):
        return TreeNode(node.value)
    elif isinstance(node, ast.Name):
        return TreeNode(node.id)
```

- **But** : Convertir chaque nœud de l'AST (généré par `ast.parse`) en un arbre binaire `TreeNode`.
- **Input** : Un nœud AST, qui peut être une opération binaire, une constante, ou une variable.
- **Output** : Un objet `TreeNode` représentant un nœud de l'arbre binaire.

---

### 4. Fonction `print_tree`

```python
def print_tree(node, level=0, indent="    "):
    if node is not None:
        print_tree(node.left, level + 1, indent)
        print(indent * level + '->', node.value)
        print_tree(node.right, level + 1, indent)
```

- **But** : Afficher l'arbre binaire de manière hiérarchique.
- **Input** :
  - `node` : Le nœud racine de l'arbre ou sous-arbre à afficher.
  - `level` : Le niveau d'indentation actuel (utile pour représenter la profondeur dans l'arbre).
  - `indent` : La chaîne d'espaces utilisée pour indenter l'arbre visuellement.
- **Output** : Affiche l'arbre dans le terminal de manière lisible.
- **Use** : Parcours infixe (gauche → nœud courant → droite) pour afficher l'arbre.

---

### 5. Fonction `tree_to_expression`

```python
def tree_to_expression(node):
    if node.left is None and node.right is None:
        return str(node.value)
    
    left_expr = tree_to_expression(node.left)
    right_expr = tree_to_expression(node.right)
    
    return f'({left_expr} {node.value} {right_expr})'
```

- **But** : Reformer une expression mathématique à partir d'un arbre binaire.
- **Input** : Le nœud racine de l'arbre ou sous-arbre.
- **Output** : Une chaîne de caractères représentant l'expression mathématique originale.
- **Use** : Traverse l'arbre de manière infixe, ajoutant des parenthèses autour des sous-expressions pour respecter l'ordre des opérations.

---

## Exemple d'utilisation

Voici un exemple de bout en bout pour illustrer l'utilisation des fonctions dans ce projet :

```python
# Expression à analyser
expression = "x * (x + 2) - 3 / x"

# Étape 1 : Convertir l'expression en arbre binaire
tree = expression_to_tree(expression)

# Étape 2 : Afficher l'arbre généré
print("Arbre généré :")
print_tree(tree)

# Étape 3 : Reconstruire l'expression à partir de l'arbre
reconstructed_expr = tree_to_expression(tree)
print("\nExpression reconstruite :", reconstructed_expr)
```

### Exemple de sortie :

```
Arbre généré :
        -> x
    -> /
        -> 3
-> -
        -> 2
    -> +
        -> x
-> *
    -> x

Expression reconstruite : ((x * (x + 2)) - (3 / x))
```

---

## Remarques :

1. **Parenthèses** : L'expression reformée utilise des parenthèses pour s'assurer que la hiérarchie des opérations est respectée (par exemple, multiplication avant addition).

2. link to notebook if github wasn't able to open (Tree-auto-encoder.ipynb) file.

**GoogleColab_link**: https://colab.research.google.com/drive/1lBWXZ8vG3CrtJdWELqyqMH91hRWXH9St?usp=sharing
