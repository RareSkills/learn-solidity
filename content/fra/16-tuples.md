# Tuples

Nous allons prendre une petite tangente pour étudier le type de données "tuple", car il s'agit d'un prérequis pour les prochaines sessions.

Si vous avez utilisé des tuples dans un langage tel Python ou Rust, rien de surprenant ici. C'est un tableau de taille fixe, mais les types de données qu'il contient peuvent être différents.

Voici un exemple de fonction qui retourne un tuple:

```solidity

contract ExampleContract {

    function getTopLeaderboardScore()
        public 
        pure 
        returns (address, uint256) {
            return (
                0xd8da6bf26964af9d7eed9e03e53415d37aa96045, 
                100
            );
    }
}

```

Noter que la déclaration de tuples est implicite. Le mot-clé "tuple" n'apparait jamais en Solidity.

Les tuples peuvent aussi être "unpacked"(déballés) afin d'obtenir les variables qu'ils contiennent, un peu comme dans l'exemple suivant:

```solidity

contract ExampleContract {

    function getTopLeaderboardScore()
        public 
        pure 
        returns (address, uint256) {
            return (
                0xd8da6bf26964af9d7eed9e03e53415d37aa96045, 
                100
            );
    }

    function highestScoreIsOver9000()
        public 
        pure 
        returns (bool) {
            (address leader, uint256 score) = 
                getTopLeaderboardScore();

            if (score > 9000) {
                return true;
            }
        
            return false;  
    }
}

```

Comme avec d'autres langages, les tuples ne doivent pas nécessairement être de taille 2. Elle peut être 3, 4 ou plus.

**Exercices Pratiques**

[Tupledore](https://github.com/RareSkills/Solidity-Exercises/tree/main/Tupledore)
