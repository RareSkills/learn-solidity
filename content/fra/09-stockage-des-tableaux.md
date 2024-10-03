# Stockage des tableaux

Vous avez certainement remarqué que dans notre section sur les tableaux, nous n'avons pas vu comment :

- Modifier la valeur contenue dans un index du tableau
- Ajouter des éléments à un tableau
- Supprimer des éléments d'un tableau

Ceci est dû au fait que ces opérations sont peu utilisées sur les tableaux passés en paramètre.

Par contre, lorsq'un tableau est déclaré comme variable de stockage, cette opération est assez commune.

Voici quelques exemples de code :

```solidity

contract ExampleContract {

    uint256[] public myArray;

    function setMyArray(
        uint256[] calldata newArray
    ) 
        public {
            myArray = newArray;
    }

    function addToArray(
        uint256 newItem
    ) 
        public {
            myArray.push(newItem);
    }

    function removeFromArray() 
        public {
            myArray.pop();
    }

    function getLength() 
        public 
        view 
        returns (uint256) {
            return myArray.length;
    }

    function getEntireArray() 
        public 
        view 
        returns (uint256[] memory) {
            return myArray;
    }
}

```

Je vous suggère de copier et coller ce code sur [Remix](https://www.notion.so/Storage-arrays-2ad2de7a813b45ec9ca83932dbbfa9f4?pvs=21) afin d'avoir une idée concrète de ce qui se passe.

Éxécutez setArray avec **[1,2,3,4,5,6]**

Maintenant éxécuter **getLength()**, elle retourne 6, qui est la taille du tableau.

Ensuite appelez **addToArray** avec 10 comme paramètre. Puis **getLength()** une fois de plus. Maintenant elle retourne 7.

Enfin **removeFromArray()** suivi par **getLength()**. Elle retourne cette fois 6 comme vous vous y attendiez.

Parce que `myArray` est `public`, Remix le fait apparaitre avec la même visibilité qu'une fonction. Mais il ne retournera pas le tableau entier. Il demande plutôt un index et retourne la valeur contenue à cet index.

Ceci signifie que le compilateur générera automatiquement une fonction nommée myArray() qui peut être appelée pour stocker les valeurs contenues dans myArray.

![https://static.wixstatic.com/media/61a666_7e7dc8b8051d40f0b27eef324637a121~mv2.png/v1/fill/w_620,h_749,al_c,q_95,enc_auto/https___s3-us-west-2_amazonaws_com_secure_notion-static_com_d1f0cfa4-c946-44de-b3a9-a67fe0.png](https://static.wixstatic.com/media/61a666_7e7dc8b8051d40f0b27eef324637a121~mv2.png/v1/fill/w_620,h_749,al_c,q_95,enc_auto/https___s3-us-west-2_amazonaws_com_secure_notion-static_com_d1f0cfa4-c946-44de-b3a9-a67fe0.png)

La fonction myArray() se comporte comme suit:

```solidity

function myArray(uint256 index) 
        public 
        view 
        returns (uint256) {
            return myArray[index];
}
```

Par contre, la fonction **getEntireArray()** retourne le tableau entier.

Noter que **pop()** ne retourne pas la valeur supprimée.

**Supprimer un élément**

Solidity n'a pas de moyen de retirer un élément au milieu d'une liste et réduire la taille de celle-ci de 1. Le code suivant est valide, mais n'affecte pas la taille de la liste.

```solidity

contract ExampleContract {

    uint256[] public myArray;

    function removeAt(uint256 index) public {
        delete myArray[index];
        // sets the value at index to be zero

        // the following code is equivalent
        // myArray[index] = 0;

        // myArray.length does not change in either circumstance
    }
}
```

Si vous souhaitez supprimer un élément et réduire la taille de la liste, vous devez faire un "pop and swap".

Il supprime l'élément à la position indiquée par le paramètre index et le remplace avec le dernier élément du tableau.

```solidity

contract ExampleContract {

    uint256[] public myArray;

    function popAndSwap(uint256 index) public {
        uint256 valueAtTheEnd = myArray[myArray.length - 1];
        myArray.pop(); // reduces the length;
        myArray[index] = valueAtTheEnd;
    }
}
```

Solidity ne peut effacer un élément en milieu de liste et conserver l'ordre original des éléments du tableau.

**Chaînes de caractère (strings)**

Les chaînes de caractère se comportent pareillement aux tableaux, à l'exception du fait que lorsqu'elles sont `public`, elles retournent la chaîne entière, car les "strings" ne peuvent être indexées (ça prête à confusion, n'est-ce pas ?). Les opérations "pop" ou "length" n'existent pas pour les "strings".

```solidity

contract ExampleContract {

    string public name;

    function setName(string calldata newName) 
            public {
        name = newName;
    }
}
```

**Exercices Pratiques**

[ListOfNumbers](https://github.com/RareSkills/Solidity-Exercises/tree/main/ListOfNumbers)

[InsertInArray](https://github.com/RareSkills/Solidity-Exercises/tree/main/InsertInArray)

[Stack](https://github.com/RareSkills/Solidity-Exercises/tree/main/Stack)
