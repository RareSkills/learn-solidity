# Nested Mappings (mappages imbriqués)

Dans la plupart des langages, un "hashmap" peut contenir un autre hashmap, c'est également le cas pour Solidity. Par contre, vu que les mappages ne sont pas un type de retour valide, vous devez renseigner toutes les clés requises par le mappage.

Regardons un exemple.

```solidity

contract ExampleContract {

    mapping(uint256 => mapping(uint256 => uint256)) public nestedMap;

    function setNestedMap(
        uint256 key1, 
        uint256 key2, 
        uint256 finalValue
    ) 
        public {
            nestedMap[key1][key2] = finalValue;
    }

    function getNestedMap(
        uint256 key1, 
        uint256 key2
    ) 
        public 
        view 
        returns (uint256) {
            return nestedMap[key1][key2];
    }
}

```

L'imbrication de mappage est très courante dans les contrats intelligents, contrairement aux tableaux imbriqués. Par exemple, vous pouvez faire de la comptabilité comme suit:

```solidity

contract TrackDebt {

    mapping(address => mapping(address => uint256)) public amountOwed;

    function setAmountOwed(
        address borrower, 
        address lender, 
        uint256 amount
    ) 
        public {
            amountOwed[lender][borrower] = amount;
    }
}

```

Notez que l'ordre est important ici. Avec cette approche, un prêteur peut avoir plusieurs emprunteurs. Si vous avions défini `borrower` comme première clé, cela signifierait qu'un emprunteur pourrait avoir plusieurs créanciers.

Les mêmes restrictions qui s'appliquent aux simples mappages s'appliquent également aux mappages imbriqués. Vous ne pouvez itérer entre les clés, les déclarer dans une fonction ou les retourner comme résultat d'une fonction.

**Les mappages publics imbriqués ne fonctionnent pas**

Ceci est une autre particularité étrange de solidity. Solidity crée automatiquement des getters pour les variables lorsque vous les déclarez "public". Cependant, les fonctions getter publiques vous permettent de fournir les arguments nécessaires.

Oui. Vous avez bien lu.

La solution est de rendre les mappages imbriqués `private` et les envelopper dans une fonction publique qui obtient leur valeur. Il est temps de pratiquer tout ceci!

**Exercices Pratiques**

[NestedMapping](https://github.com/RareSkills/Solidity-Exercises/tree/main/NestedMapping)

[TripleNestedMapping](https://github.com/RareSkills/Solidity-Exercises/tree/main/TripleNestedMapping)
