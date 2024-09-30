# Opérations Arithmétiques

Les opérations arythmétiques en Solidity se comportent exactement de la même façon que dans les autres langages, nous n'allons donc pas tout réexpliquer ici.

Voici comment additionner des nombres

```solidity

uint256 sum = 10 + 5; // sum == 15

uint256 x = 1;
uint256 y = 2;

uint256 anotherSum = x + y; // anotherSum == 3
```

Les exposants sont pareils que dans les autres langages similaires au C.

```solidity

uint256 exp = 2 ** 3; // exp == 8
```

Il en est de même pour le modulo

```solidity

uint256 remainder = 10 % 4; // remainder == 2
```

Soustraire, multiplier, et diviser sont évidents, je ne peux me permettre d'insulter votre intelligence en vous les apprenant de nouveau.

**Solidity n'a pas de nombres réels (flottants)**

Si vous essayez de diviser 5 par 2, vous n'obtiendrez pas 2.5. Mais 2. Pour rappel, uint256 est un entier non-signé. Toutes les divisions que vous faites sont donc des divisions entières.

Et si vous voulez réellement savoir combien font 10% de 200? C'est assez raisonable pour le calcul d'intérêt par exemple.

```solidity

uint256 interest = 200 * 0.1; // échoue, 0.1 n'est pas valide
```

La solution ici est de convertir x * 0.1 en x * 1 / 10. C'est valide et produira le bon résultat.

```solidity

uint256 interest = 200 / 10;
```

Si votre intérêt est par exemple 7.5%, alors vous devrez procéder comme suit:

```solidity

uint256 interest = 200 * 75 / 1000;
```

Si vous souhaitez connaitre le pourcentage de population d'une ville par rapport à celle du pays, vous ne pouvez pas faire le calcul suivant:

```solidity

uint256 cityPopulation = 1000;
uint256 nationPopulation = 10000;

uint256 fractionOfPopulation = cityPopulation / nationPopulation;
// fractionOfPopulation est égal à zéro!
```

Ceci requiert une solution plus avancée que nous verrons plus tard.

**Note:** Pourquoi Solidity ne supporte-t-il pas les réels? Les réels ne sont pas toujours déterministes, et les blockchains doivent être déterministes sinon les noeuds ne pourront s'accorder sur le résultat des transactions. Par exemple, si vous divisez 2/3, certains ordinateurs retourneront 0.6666, et d'autres 0.66667. Ce désaccord pourrait causer le scindage du réseau blockchain! Solidity ne permet donc pas l'utilisation de réels.

**Solidity ne fait pas d'underflow ou d'overflow, il arrête l'éxécution**

Que se passe-t-il si vous essayez ceci ?

```solidity

function subtract(uint256 x, uint256 y) 
        public 
        pure 
        returns (uint256) {
            uint256 difference = x - y;
            return difference;
}
```

Qu'arrive-t-il si x est 2 et y est 5? Vous n'obtiendrez pas -3. En réalité, l'éxécution s'arrêtera avec un **revert**.

Solidity n'émet pas d'exceptions, mais vous pouvez considérer **revert** comme l'équivalent d'une exception non-capturée ou d'un crash dans d'autres langages.

Solidity permettait auparavant des overflows et underflows, mais ceci a conduit à tellement d'arrêt de fonctionnement ou de hacking de contrats intelligents que les protections contre l'overflow et l'underflow ont été ajoutées au langage. Cette fonctionnalité a été introduite après la version 0.8.0 de Solidity.

Vous avez probablement remarqué qu'un grand nombre de fichiers Solidity ont actuellement cette ligne:

```solidity
pragma solidity ^0.8.0;    
```

Ceci signifie que le code source est compilé avec une version de Solidity supérieure ou égale à 0.8.0. Si vous voyez une version précédente à celle-ci, vous ne pouvez pas aller du principe que la protection contre les overflows et underflows est intégrée.

**Si vous souhaitez permettre les overflows et underflows, vous devez utiliser un bloc "unchecked"**

Vous pouvez utiliser un bloc "unchecked" pour autoriser les overflows et underflows. Ceci n'est pas recommandé à moins d'avoir une bonne raison de le faire. Vous pouvez utiliser un block "unckecked" comme suit :

```solidity

uint256 x = 1;
uint256 y = 2;

unchecked {
    uint256 z = x - y; // z == 2**256 - 1
}
```

Noter que les instructions à l'intérieur du bloc "unchecked" ne causeront pas de **revert** même si elles vont hors des limites du type uint. C'est une fonctionnalité avancée à n'utiliser que si vous savez vraiment ce que vous faites.

**Exercices Pratiques**

[Addition](https://github.com/RareSkills/Solidity-Exercises/tree/main/Add)

[Division](https://github.com/RareSkills/Solidity-Exercises/tree/main/Divide)

[Exposant](https://github.com/RareSkills/Solidity-Exercises/tree/main/Exponent)

[Unchecked](https://github.com/RareSkills/Solidity-Exercises/tree/main/Unchecked)
