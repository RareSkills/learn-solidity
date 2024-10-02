# Variables de stockage ("Storage variables")

Jusqu'ici, toutes nos fonctions ont retourné des valeurs qui dépendaient uniquement des paramètres de la fonction. Elles ne dépendaient de rien d'autres que des entrées immédiates. C'est pourquoi elles sont dites fonctions **pure**. Elles n'ont pas connaissance du statut de la blockchain ou des transactions passées.

Ceci devient problématique si on veut faire un suivi de quelque chose, comme le montant dû à quelqu'un par exemple, ou combien de points on a dans un jeu.

Découvrons les **storage variable**

Elles ressemblent aux "variables de classe" dans d'autres langages, mais se comportent différemmment. Vous pouvez les considérer comme **des variables qui se comportent comme une base de données miniature**.

Regardons un exemple:

```solidity

contract ExampleContract {

    uint256 internal x;

    function setX(
        uint256 newValue
    ) 
        public {
            x = newValue;
    }
    
    function getX() 
        public 
        view 
        returns (uint256) {
            return x;
    }
}

```

Nous avons beaucoup à éclaircir ici !

**Les variables déclarées hors des fonctions sont des variables de stockage. Elles conservent leur valeur après la fin de la transaction**

Noter que **`getX()`** a le modifier (modificateur) **`view`** au lieu de **`pure`**. C'est parce qu'il voit(lit) le statut de la blockchain, c'est-à-dire ce qui est stocké dans la variable `x`. Si vous remplacez `view` par `pure` dans cet exemple, le code ne compilera pas. Vous pouvez aussi considérer **view** comme **read-only**(lecture seule). Notez également que la valeur retournée par `getX` a le même type que x, les deux étant des uint256.

Deuxièmement, notez que **`setX`** n'a pas de modifier view ou pure. Ceci est dû au fait qu'il s'agit d'une **state changing function**(fonction qui modifie l'état de la blockchain). Les fonctions qui modifient des variables de stockage, ou qui font d'autres changements permanents sur la blockchain ne peuvent avoir le modifier view ou pure, ceci parce qu'elles ne sont pas **read only** et ne peuvent donc être `view`, et encore moins **`pure`**.

Pour appuyer ce fait, retenez que le code suivant est invalide:

```solidity

contract ExampleContract {

    uint256 internal x;

    function setX(
        uint256 newValue
    ) 
        public {
            x = newValue;
    }
    
    // error: this function cannot be pure
    function getX() 
        public 
        pure 
        returns (uint256) {
            return x;
    }
}

```

Remarquez que la variable x elle-même a le modifier **`internal`**. Ceci indique que d'autres contrats intelligents ne peuvent accéder à sa valeur.

**Le fait qu'une variable soit "internal" ne signifie pas qu'elle est cachée. Elle est stockée sur la blockchain et n'importe qui peut parcourir la blockchain et obtenir sa valeur!**

C'est ici que les choses deviennent quelque peu confuses.

Le code suivant est valide, mais est considéré comme une mauvaise pratique.

```solidity

contract ExampleContract {

    uint256 x;

    function setX(
        uint256 newValue
    ) 
        public {
            x = newValue;
    }
    
    function getX() 
        public 
        view 
        returns (uint256) {
            return x;
    }
}

```

Ici, nous avons retiré le modifier "internal" de x, et la compilation réussit toujours. C'est vu comme une mauvaise pratique parce que vous n'êtes pas explicites quant-à vos intentions concernant la visibilité de x.

Le code suivant est également valide:

```solidity

contract ExampleContract {

    uint256 public x;

    function setX(
        uint256 newValue
    ) 
        public {
            x = newValue;
    }
    
    function getX() 
        public 
        view 
        returns (uint256) {
            return x;
    }
}

```

**Lorsqu'une variable est déclarée `public`, cela signifie que d'autres smarts contracts peuvent lire sa valeur mais pas la modifier.**

Ça prête à confusion parce que les fonctions publiques peuvent modifier des variables, mais les variables publiques ne peuvent être modifiées à moins qu'il y ait une fonction pour changer leur valeur.

**En résumé**

- Les variables de stockage (storage variables) se déclarent en dehors des fonctions
- Les fonctions "public" qui n'ont pas de modificateur "view" ou "pure" peuvent changer la valeur des variables de stockage
- Les fonctions "pure" ne peuvent accéder aux variables de stockage

****

**Exercices Pratiques**

[PublicVariable](https://github.com/RareSkills/Solidity-Exercises/tree/main/PublicVariable)

[BasicStorage](https://github.com/RareSkills/Solidity-Exercises/blob/main/BasicStorage/src/BasicStorage.sol)

[PublicFunction](https://github.com/RareSkills/Solidity-Exercises/tree/main/PublicFunction)

[PureVsView](https://github.com/RareSkills/Solidity-Exercises/tree/main/PureVsView)
