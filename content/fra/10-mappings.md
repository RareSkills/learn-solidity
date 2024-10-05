# Mappings

`Mapping`, `Hashmap`, tableaux associatifs, `maps`, peu importe le nom que vous lui donnez, Solidity l'a.

Nous l'appelerons "mapping" parce que c'est le mot-clé utilisé par Solidity. Regardons un exemple:

```solidity

contract ExampleContract {

    mapping(uint256 => uint256) public myMapping;

    function setMapping(uint256 key, uint256 value) 
        public {
            myMapping[key] = value;
    }

    function getValue(uint256 key) 
        public 
        view 
        returns (uint256) {
            return myMapping[key];
    }
}

```

Ce code fait exactement ce que vous pensez. Puisque myMapping est "public", Solidity l'associe à une fonction `getter` nous permettant d'accéder aux valeurs contenues dans le mapping. Par contre, si vous souhaitez accéder au mapping en passant par une fonction, vous pouvez suivre la logique de **getValue**.

Voici un point surprenant :

**Si vous accédez à un mapping avec une clé qui n'a pas déjà été définie, il n'y aura PAS de revert. Le mapping retournera juste la "valeur par défaut" du type de données que vous essayez d'obtenir**

Dans l'exemple suivant, le mapping retournera *false* si vous l'interrogez avec un nombre qui n'a pas déjà été défini.

```solidity

contract ExampleContract {
    // returns false by default
    mapping(uint256 => bool) public mapToBool;

    // returns 0 by default
    mapping(uint256 => uint256) public mapToUint; 

    // returns 0x0000000000000000000000000000000000000000 by default
    mapping(uint256 => address) public mapToAddress;

}
```

Je vous encourage à coller ce code sur remix, puis mettre des nombres comme clés pour voir les valeurs par défaut qui seront retournées pour chaque mapping.

![https://static.wixstatic.com/media/61a666_499ad0d908c24c7d8cb892b11941611e~mv2.png/v1/crop/x_2,y_0,w_1998,h_972/fill/w_939,h_457,al_c,q_95,enc_auto/Mappings.png](https://static.wixstatic.com/media/61a666_499ad0d908c24c7d8cb892b11941611e~mv2.png/v1/crop/x_2,y_0,w_1998,h_972/fill/w_939,h_457,al_c,q_95,enc_auto/Mappings.png)

De la même manière, les tokens ERC20 utilisent les mappings pour stocker combien de tokens quelqu'un a! Ils lient une adresse au nombre de tokens que possède le détenteur de cette addresse.

```solidity

contract ERC20Token {

    mapping(address => uint256) public balances;

    function setSomeonesBalance(address owner, uint256 amount) 
        public {
            balances[owner] = amount;
    }

    function transferTokensBetweenAddresses(
            address sender, 
            address receiver, 
            uint256 amount) 
        public {
            balances[sender] -= amount;   // deduct/debit the sender's balance
            balances[receiver] += amount; // credit the reciever's balance
    }
}

```

Cette implémentation a une faille puisque quiconque peut appeler les fonctions publiques et envoyer des tokens entre les addresses autant qu'il le souhaite, mais nous règlerons celà plus tard.

Contrairement à ce qu'on pourrait penser, **les tokens ERC20 ne sont pas stockés dans des portefeuilles crypto, ils sont simplement un [uint256](https://www.rareskills.io/post/uint-max-value-solidity) associé à votre addresse dans un contrat intelligent**. "Les tokens ERC20" sont juste des contrats intelligents.

Voici le contrat intelligent de l'USDC, un token ERC20: [https://etherscan.io/token/0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48](https://etherscan.io/token/0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48)

Et voici le token ApeCoin, le token de l'écosystème Bored Ape Yatch Club:

[https://etherscan.io/token/0x4d224452801aced8b2f0aebe155379bb5d594381](https://etherscan.io/token/0x4d224452801aced8b2f0aebe155379bb5d594381)

**Surprise 1: Les mappings peuvent être uniquement déclarés comme storage (variable de stockage), vous ne pouvez pas les déclarer à l'intérieur d'une fonction**

Ceci peut sembler très bizarre comme restriction, mais est lié au fonctionnement de la machine virtuelle Ethereum. Les blockchains en général n'aiment pas les "hashmaps" ceci dû à leur temps d'éxécution difficile à prédire. Le code suivant est invalide.

```solidity

contract BrokenContract {

    function wontWork() 
        public 
        view {
            mapping(uint256 => uint256) someMap; 
            // This won't compile, mappings must be state variables
    }
}

```

**Surprise 2: On ne peut itérer dans un mapping**

Il n'existe pas de moyen d'itérer entre les clés d'un mapping. Chaque clé est techniquement correcte, elle renvoie juste 0 par défaut.

```solidity

contract BrokenContract {
    mapping(uint256 => uint256) public someMap;

    function wontWork() 
        public 
        view {
            for (uint256 key in someMap) {  
            // Corrected to valid Solidity syntax, though the logic still 
            // won't compile in Solidity
                // do something
            }
    }
}

```

**Surprise 3: On ne peut retourner un mapping**

Le code suivant est invalide. les "maps" ne sont pas un type de retour valide pour les fonctions en solidity.

```solidity

contract BrokenContract {
    mapping(uint256 => uint256) public someMap;

    function wontWork() 
        public 
        view 
        returns (mapping(uint256 => uint256)) {
            return someMap; // This will not compile, as mappings cannot be returned from public functions
    }
}

```

**Exercices Pratiques**

[SpecialNumbers](https://github.com/RareSkills/Solidity-Exercises/tree/main/SpecialNumbers)
