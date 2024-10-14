# Structure de contrôle If

Les structures de contrôle If (If Statement) se comportement exactement comme avec d'autres langages.

```solidity

contract ExampleIfStatement {
    function isAMultipleOfTen(uint256 x) 
        public 
        pure 
        returns (bool) {
            if (x % 10 == 0) {
                return true;
            } else {
                return false;
            }
    }
}

```

L'argument à l'intérieur des parenthèses d'un bloc "If" doit être un booléen. Notre code plus haut est équivalent à celui ci-dessous.

```solidity

contract ExampleIfStatement {
    function isAMultipleOfTen(uint256 x) 
        public 
        pure 
        returns (bool) {
            bool isMul = x % 10 == 0;
            if (isMul) {
                return true;
            } else {
                return false;
            }
    }
}

```

Contrairement aux langages dynamiques tels Python ou JavaScript, vous ne pouvez pas procéder comme suit:

```solidity

function isNotZero(uint256 x) 
    public 
    pure 
    returns (bool) {
        if (x) {
            return true;
        } else {
            return false;
        }
}

```

Solidity supporte aussi le bloc "else if", nous supposons que vous savez déjà comment tout ceci fonctionne.

Solidity n'a pas de structures de type **switch** comme c'est le cas pour Java et C.

**Exercices Pratiques**

[IfStatement](https://github.com/RareSkills/Solidity-Exercises/tree/main/IfStatement)
