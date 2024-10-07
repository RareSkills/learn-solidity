# Require

Il n'y a plus qu'un mot-clé essentiel de Solidity, et nous serons prêts à créer notre propre token ERC20.

Bien que nous puissions utiliser une structure If pour vérifier que les paramètres d'une fonction sont valides, ou que le bon msg.sender appelle la fonction, le moyen élégant de procéder est d'utiliser l'instruction "require". L'instruction "require" oblige la transaction à faire un "revert" si une condition n'est pas validée.

```solidity

contract ExampleContract {
    function mustNotBeFive(
        uint256 x
    ) 
        public 
        pure 
        returns (uint256) {
            require(x != 5, "five is not valid");
            return x * 2;
    }
}

```

Essayez le code suivant sur Remix.

Notez que ne pas mettre de message d'erreur est valide, mais considéré comme une mauvaise pratique, parce que ça rend la compréhension de l'erreur plus compliquée.

```solidity

contract ExampleContract {
    function mustNotBeFive(
        uint256 x
    ) 
        public 
        pure 
        returns (uint256) {
            // valid, but bad practice
            require(x != 5);
            return x * 2;
    }
}
```

Vous pouvez utiliser ce procédé afin de vous assurer que msg.sender est celui qu'il devrait être. Les problèmes suivants vous permettront de mettre tout ceci en pratique.

**Exercices Pratiques**

[NotEnough](https://github.com/RareSkills/Solidity-Exercises/tree/main/NotEnough)

[Owner](https://github.com/RareSkills/Solidity-Exercises/tree/main/Owner)
