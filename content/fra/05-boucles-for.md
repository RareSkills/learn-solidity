# Boucles For

Tout comme les structures If, il n'y a rien de surprenant en ce qui concerne les boucles `for`. Voici le code pour additionner tous les nombres de 0 à 99 :

```solidity

contract ExampleContract {
    function addNumbers() 
        public 
        pure 
        returns (uint256) {
            uint256 sum = 0;
            for (uint256 i = 0; i < 100; i++) {
                sum = sum + i;
            }
            return sum;
    }
}

```

Solidity supporte aussi l'opérateur `+=` si vous préférez procéder comme tel.

```solidity

contract ExampleContract {
    function addNumbers() 
        public 
        pure 
        returns (uint256) {
            uint256 sum = 0;
            for (uint256 i = 0; i < 100; i++) {
                sum += i;
            }
            return sum;
    }
}

```

Solidity a également les boucles "while" et "do while" mais elles sont si peu utilisées que ça ne vaut pas la peine d'en parler maintenant.

Un cas d'utilisation naturel pour les boucles "for" est l'itération d'un tableau. mais nous n'avons pas déjà parlé des tableaux, nous y reviendront donc plus tard.

Comme avec d'autres langages, vous pouvez faire un retour dans une boucle for avant la fin d'une fonction. Ce code va itérer de 2 au nombre x jusqu'à ce qu'il trouve un facteur premier.

```solidity

contract ExampleContract {
    function findPrimeFactor(uint256 x) 
        public 
        pure 
        returns (uint256) {
            // start at 2, 1 is not a prime factor
            // use <= because x might be prime
            for (uint256 i = 2; i <= x; i++) {
                if (x % i == 0) {
                    return i;
                }
            }
    }
}

```

**Exercices Pratiques**

[IsPrime](https://github.com/RareSkills/Solidity-Exercises/tree/main/IsPrime)

[Fibonacci](https://github.com/RareSkills/Solidity-Exercises/tree/main/Fibonacci)
