# Les tableaux imbriqués (nested arrays)

Les tableaux imbriqués sont rarement utilisés en pratique, mais nous les incluons ici afin d'avoir un contenu complet.

Les tableaux imbriqués, comme leur nom l'indique, font référence aux tableaux contenus dans un autre tableau.

Dans cet exemple, la fonction reçoit en paramètre une grille rectangulaire.

```solidity

contract ExampleContract {

    function containsAThree(uint256[][] calldata nestedArray) 
        public 
        pure 
        returns (bool) {
            for (uint256 i = 0; i < nestedArray.length; i++) {
                for (uint256 j = 0; j < nestedArray[i].length; j++) {
                    if (nestedArray[i][j] == 3) {
                        return true;
                    }
                }
            }
            return false;
    }
}

```

Voici le déroulé sur Remix.

![https://static.wixstatic.com/media/c0c19a_b9ca99cdc4384a34b4ad9c56ae28e8b9~mv2.png/v1/fill/w_939,h_494,al_c,q_95,enc_auto/nested%20arrays.png](https://static.wixstatic.com/media/c0c19a_b9ca99cdc4384a34b4ad9c56ae28e8b9~mv2.png/v1/fill/w_939,h_494,al_c,q_95,enc_auto/nested%20arrays.png)

Vous pouvez aussi obtenir un tableau 1D (une dimension) d'un tableau 2D (deux dimensions).

```solidity

contract ExampleContract {

    // [[1,2],[3,4],[5,6]] becomes [1,2]
    function getRow(uint256[][] calldata nestedArray) 
        public 
        pure 
        returns(uint256[] memory) {
            return nestedArray[0];
    }
}

```

Pour déclarer des tableaux de taille fixe, utilisez la syntaxe suivante:

```solidity

contract ExampleContract {

    // ACCEPTED: [[1,2],[3,4],[5,6]]
    // REJECTED: [[1,2,3],[4,5,6]]
    function fixedSize(uint256[2][3] calldata nestedArray) 
        public 
        pure 
        returns (uint256) {
            return 0; // just for the sake of compilation
    }
}

```

Ce qui peut sembler confus est que lorsque vous accédez à un élément précis dans un tableau, l'ordre peut sembler inverse à celui d'autres langages, mais c'est plutôt logique quand on y pense.

```solidity

contract ExampleContract {

    // ACCEPTED: [[1,2],[3,4],[5,6]] -> returns 6
    function getLast(uint256[2][3] calldata nestedArray) 
        public 
        pure 
        returns (uint256) {
            return nestedArray[2][1];

            // nestedArray[2] -> [5,6] then get the 1st index item -> 6
    }
}

```

Tout comme avec les tableaux 1D, si vous accédez à un index hors-limite, la transaction échouera.

Notez que les tableaux imbriqués sont extrêmement peu utilisés en pratique. S'il vous prend l'envie de sauter cette section, sentez-vous libre de le faire.

**Exercices Pratiques**

[NestedArray](https://github.com/RareSkills/Solidity-Exercises/tree/main/NestedArray)

[TicTacToe](https://github.com/RareSkills/Solidity-Exercises/tree/main/TicTacToe)
