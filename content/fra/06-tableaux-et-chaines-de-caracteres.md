# Introduction aux tableaux et chaînes de caractères

Dans cette section, nous allons introduire les structures de données suivantes : tableau(array) et chaîne de caractères(string). Elles se comportent différemment des types de données en Solidity que nous avons vu précédemment. Nous en parlerons donc ici.

Syntaxe de déclaration de tableaux

Prenons l'exemple de cette fonction qui prend en paramètre un tableau et retourne un tableau. Il y a beaucoup d'éléments à éclaircir ici!

Pour commencer, ça devrait être clair que la syntaxe pour déclarer un tableau de nombres est uint256[]. Nous arriverons à `calldata` et `memory` dans un instant.

```solidity

contract ExampleContract {
    function useArrayForUint256(uint256[] calldata input) 
        public 
        pure 
        returns (uint256[] memory) {
            return input;
    }
}

```

Si vous voulez un tableau d'adresses ou de booléens, il faut procéder comme suit:

```solidity

contract ExampleContract {
    function booleanArrayExample(bool[] calldata input) 
        public 
        pure 
        returns (bool[] memory) {
            return input;
    }

    function addressArrayExample(address[] calldata input) 
        public 
        pure 
        returns (address[] memory) {
            return input;
    }
}

```

Que signifie `calldata` et `memory`? Sachez d'emblée que si vous ne les incluez pas, la compilation du code échouera. Voici deux exemples de codes qui ne compilent pas.

```solidity

contract BadContract1 {

    // argument is missing calldata
    function useArrayForUint256(uint256[] input) 
        public 
        pure 
        returns (uint256[] memory) {
            return input;
    }
}

```

```solidity

contract BadContract2 {

    // return type is missing memory
    function useArrayForUint256(uint256[] calldata input) 
        public 
        pure 
        returns (uint256[]) {
            return input;
    }
}

```

Que sont donc `calldata` et `memory`?

Si vous faites du C ou C++, ce concept vous est sûrement familier. La mémoire en Solidity est comme la pile en C, C++, ou Rust. Les tableaux peuvent être de taille illimitée, les stocker sur la pile d'éxécution (ne vous inquiétez pas si vous ne savez pas ce que c'est), pourrait occasionner une erreur de type *stackoverflow* (à ne pas confondre avec le célèbre forum!).

Calldata est propre à Solidity. Il représente les informations("transaction data") qui sont envoyées lorsque quelqu'un émet une transaction sur la blockchain.

Calldata signifie "Se référer aux données de la transaction Ethereum elle-même.". C'est un concept assez avancé, donc ne vous inquiétez pas si vous ne comprenez pas tout cela maintenant.

En cas de doute: les paramètres d'une fonction lorsqu'il s'agit des tableaux et chaînes de caractères doivent être "calldata", mais au niveau du type de retour, ils doivent être "memory".

Il y a quelques exceptions à utiliser `calldata` au niveau des paramètres d'une fonction, mais le type de retour pour un tableau doit toujours être `memory`, jamais "calldata", ou la compilation échouera. Pour éviter de vous bombarder d'informations, nous parlerons des exceptions à "calldata" plus tard.

Voici comment utiliser des tableaux de nombres avec Remix.

![Tableaux de nombres](https://github.com/user-attachments/assets/a399da3e-cf19-494e-ad4e-6d5dc568722b)

**Les index de tableaux commencent à 0 comme avec tous les autres langages**

Aucune surprise à ce niveau.

```solidity

contract ExampleContract {
    function returnFirstElement(uint256[] calldata myArray) 
        public 
        pure 
        returns (uint256) {
            uint256 first = myArray[0];
            return first;
    }
}

```

Remarquez que le type de retour est uint256, vu que nous retournons un nombre, pas un tableau.

Notez que si le tableau(array) est vide, la transaction échouera.

**Pour obtenir la taille d'un tableau, utiliser `.length`**

C'est pareil qu'en JavaScript.

```solidity

contract ExampleContract {
    function returnFirstElement(uint256[] calldata myArray) 
        public 
        pure 
        returns (uint256) {
            uint256 len = myArray.length;
            return len;
    }
}

```

Voici également comment parcourir un tableau.

```solidity

contract ExampleContract {
    function productOfarray(uint256[] calldata myArray) 
        public 
        pure 
        returns (uint256) {
            uint256 product = 1;
            for (uint256 i = 0; i < myArray.length; i++) {
                product *= myArray[i];
            }
            return product;
    }
}

```

**Les tableaux peuvent être déclarés pour avoir une taille fixe**

Dans les exemples précédents, les crochets `[]` sont vides durant la déclaration. Si vous voulez qu'un tableau ait une taille fixe, vous pouvez indiquer sa taille à l'intérieur des crochets.

```solidity

contract ExampleContract {
    function productOfarray(uint256[5] calldata myArray) 
        public 
        pure 
        returns (uint256) {
            uint256 last = myArray[4];
            return last;
    }
}

```

Dans le cas où la fonction reçoit en paramètre un tableau de taille différente de 5, il y'aura un **revert** (crash).

**Chaînes de caractères**

Les chaînes de caractères(strings) ont un comportement similaire à celui des tableaux. En réalité, ce sont des tableaux sous le capot(mais avec quelques différences). Voici une fonction qui retourne la chaîne que vous lui passez en paramètres.

```solidity

contract ExampleContract {
    function echo(string calldata input) 
        public 
        pure 
        returns (string memory) {
            return input;
    }
}

```

Et voici finalement un *hello world*.

```solidity

contract ExampleContract {
    function helloWorld() 
        public 
        pure 
        returns (string memory) {
            return "Hello, world!";
    }
}

```

**Concaténer des "strings"**

C'est assez drôle de constater que solidity ne permettait pas la concatenation de chaîne de caractères avant Février 2022 lorsque Solidity 0.8.12 est sorti. Si vous souhaitez concaténer des chaînes en Solidity, rassurez vous que le `pragma` en début de fichier est au moins à la version 0.8.12

```solidity

pragma solidity ^0.8.12;
contract ExampleContract {
    function useArrays(string calldata user) 
        public 
        pure 
        returns(string memory) {
            return string.concat("hello ", user);
    }
}

```

Il y a une raison pour laquelle le support de la concaténation a été ajouté aussi tard, les contrats intelligents traitent habituellement les nombres, pas les chaînes de caractères.

**Les chaînes ne peuvent être indexées**

Dans les langages comme JavaScript ou Python, vous pouvez indexer une chaîne tout comme avec un tableau et obtenir un caractère en retour. Ceci n'est pas possible avec Solidity. Le code suivant ne compile pas:

```solidity

pragma solidity ^0.8.12;
contract BadContract {
    function useArrays(string calldata input) 
        public 
        pure 
        returns(string memory) {
            return input[0]; // erreur
    }
}

```

**`length` ne fonctionne pas sur les chaînes de caractères**

Solidity ne permet pas d'obtenir la taille d'une chaîne. Ceci est dû aux caractères unicode qui peuvent rendre la taille ambigue, et Solidity représente les chaînes de caractères comme un tableau de bytes, et non comme une suite de caractères.

```solidity

pragma solidity ^0.8.12;
contract StringContract {
    function useArrays(string calldata input) 
        public 
        pure 
        returns(uint256) {
            return input.length; // does not compile
    }
}

```

**Ce que nous n'avons pas vu**

- Les tableaux en Solidity ont des opérations telles "pop()", qui ont des effets secondaires plus avancés, nous en parlerons donc plus tard.
- La syntaxe pour déclarer les tableaux et chaînes de caractères à l'intérieur d'une fonction, contrairement aux paramètres ou au niveau de la valeur retournée, est différente.

**Exercices Pratiques**

[FizzBuzz](https://github.com/RareSkills/Solidity-Exercises/tree/main/FizzBuzz)

[SumArray](https://github.com/RareSkills/Solidity-Exercises/tree/main/SumArray)

[FilterOddNumbers](https://github.com/RareSkills/Solidity-Exercises/tree/main/FilterOddNumbers)

[IsSorted](https://github.com/RareSkills/Solidity-Exercises/tree/main/IsSorted)

[Mean](https://github.com/RareSkills/Solidity-Exercises/tree/main/Mean)
