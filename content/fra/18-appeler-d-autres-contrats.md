# Appeler d'autres contrats

Tout ce que nous avons fait jusqu'à présent consite à appeler des contrats intelligents. Cependant, il est également possible, et même souhaitable, que les contrats puissent communiquer entre eux.

Prenons un exemple minimal pour illustrer cela.

```solidity

contract ExampleContract {
    function askTheMeaningOfLife(address source)
        public 
        returns (uint256) {
            (bool ok, bytes memory result) = source.call(
                abi.encodeWithSignature("meaningOfLifeAndAllExistence()")
            );
            require(ok, "call failed");

            return abi.decode(result, (uint256));
    }
}

contract AnotherContract {
    function meaningOfLifeAndAllExistence()
        public 
        pure 
        returns (uint256) {
            return 42;
    }
}

```

Voici comment vous pouvez le voir en action

![https://static.wixstatic.com/media/61a666_ad5ca9f4df6844fc8f3c4d7463fdd53e~mv2.png/v1/fill/w_939,h_610,al_c,q_95,enc_auto/Calling%20other%20contracts1.png](https://static.wixstatic.com/media/61a666_ad5ca9f4df6844fc8f3c4d7463fdd53e~mv2.png/v1/fill/w_939,h_610,al_c,q_95,enc_auto/Calling%20other%20contracts1.png)

Puisque nous avons déjà passé en revue les **tuples, encodage abi**, et **bytes memory**, la seule chose surprenante ici est **call** et le fait que **askTheMeaningOfLife()** n'est pas une fonction **view**.

Pourquoi askTheMeaningOfLife() n'est-elle pas une fonction `view`? Si vous essayez de le compiler avec le **modifier** ``view`, la compilation échouera.

Les fonction `view` sont en **lecture** seule. Lorsque vous appelez une fonction d'un contrat intelligent arbitraire, vous ne pouvez pas savoir si elle est en lecture seule ou non. Par conséquent, Solidity ne vous permet pas de spécifier une fonction comme `view` si elle appelle un autre contrat intelligent.

De plus, bien que nous voyons que **meaningOfLifeAndAllExistence** dans **AnotherContract** retourne un uint256, nous ne pouvons pas le savoir en général. Il pourrait tout aussi bien retourner une chaine de caractères (`string`).

**Les fonctions retournent toujours des octets encodés en ABI**. Comment Remix sait-il formater les chaînes en texte et les nombres en nombres? En coulisses, il effectue l'opération **abi.decode** que nous utilisons ici.

Que représente la partie **bool ok** du tuple? Les appels de fonctions à d'autres contrats intelligents peuvent échouer, par exemple si la fonction émet un `revert`. Pour savoir si l'appel externe a échoué, un booléen est retourné. Dans cette implémentation, la fonction appelante, askTheMeaningOfLifeAndAllExistence, émet également un `revert`, mais ce n'est nécessairement pas une exigence générale.

Voici un point intéressant. Que se passe-t-il si vous appelez un contrat intelligent inexistant ?

Essayez askTheMeaningOfLife(address source) avec 0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db comme argument.

Ne soyez pas paresseux, essayez le en utilisant le code ci-dessus!

Cela entraîne un `revert`, mais ce n'est pas parce que l'adresse n'est pas présente, c'est parce que vous avez essayé de décoder des données vides. Si nous commentons la partie de décodage, alors la fonction ne revient plus lors de l'appel d'une adresse inexistante.

Lorsque vous ouvrez le menu déroulant de la transaction dans Remix, vous voyez l'explication du `revert` ici.

![https://static.wixstatic.com/media/61a666_07808a80849d4e6c8938a4459a811a87~mv2.png/v1/fill/w_952,h_69,al_c,q_95,enc_auto/Calling%20other%20contracts2.png](https://static.wixstatic.com/media/61a666_07808a80849d4e6c8938a4459a811a87~mv2.png/v1/fill/w_952,h_69,al_c,q_95,enc_auto/Calling%20other%20contracts2.png)

```solidity

contract ExampleContract {
    function askTheMeaningOfLife(address source)
        public 
        returns (uint256) {
            (bool ok, bytes memory result) = source.call(
                abi.encodeWithSignature("meaningOfLifeAndAllExistence()")
            );
            require(ok, "call failed");

            //return abi.decode(result, (uint256));
            return 0;
    }
}

```

Et si l'autre contrat prend des arguments ? Voici le code pour le faire.

```solidity

contract ExampleContract {
    function callAdd(address source, uint256 x, uint256 y)
        public 
        returns (uint256) {
            (bool ok, bytes memory result) = source.call(
                abi.encodeWithSignature("add(uint256,uint256)", x, y)
            );
            require(ok, "call failed");

            uint256 sum = abi.decode(result, (uint256));
            return sum;
    }
}

contract Calc {
    function add(uint256 x, uint256 y)
        public 
        returns (uint256) {
            return x + y;
    }
}

```

Attention à ne pas avoir d'espaces dans "add(uint256,uint256)"

Prêt à mettre ceci en pratique ?

**Exercices Pratiques**

[CrossContract](https://github.com/RareSkills/Solidity-Exercises)
