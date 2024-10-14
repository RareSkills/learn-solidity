# Constructeur (constructor)

Plus tôt dans notre exemple ERC20, nous avons fait quelque chose d'assez bizarre, nous avons initialisé notre variable "banker" directement à l'intérieur de notre contrat.

```solidity

contract ERC20 {
    address public banker = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;

    mapping(address => uint256) public balances;

    function setSomeonesBalance(
        address owner, 
        uint256 amount
    ) 
        public {
            if (msg.sender == banker) {
                balances[owner] = amount;
            }
            // do nothing
    }

    function transfer(
        address receiver, 
        uint256 amount
    ) 
        public {
            balances[msg.sender] -= amount;
            balances[receiver] += amount;
    }
}

```

C'est okay, mais que fait-on si quelqu'un souhaite déployer le contrat et se définir lui-même comme "banker" (banquier)?

Les contrats intelligents ont une fonction spéciale qui est appelée au moment du déploiement, le constructeur. C'est assez similaire à d'autres langages de programmation orienté-objet. Voici à quoi ça ressemble:

```solidity

contract ExampleContract {

    address public banker;

    constructor() {
        deployer = msg.sender;
    }
}
```

Notez que c'est "constructor()" et non "function constructor()" et nous ne spécifions pas **public** parce que les constructeurs ne peuvent être modifiés avec des éléments tels "pure", "view", "public", et bien d'autres.

Si vous souhaitez que le "banker" soit défini par celui qui déploie le contrat, vous pouvez le passer en argument au constructeur.

```solidity

contract ExampleContract {

    address public banker;

    constructor(address _banker) {
        banker = _banker;
    }
}
```

Aussi, vous verrez beaucoup le schéma variable = _variable au niveau des constructeurs. Solidity n'exige pas que vous procédiez ainsi, mais c'est considéré comme une convention.

Lors du déploiement sur Remix de contrats qui ont des paramètres au niveau du constructeur, vous devez renseigner les paramètres au niveau de la case qui apparait près du bouton "deploy".

![https://static.wixstatic.com/media/61a666_a136b53ad685419582210f02b620e762~mv2.png/v1/crop/x_0,y_0,w_2000,h_973/fill/w_939,h_457,al_c,q_95,enc_auto/Constructor.png](https://static.wixstatic.com/media/61a666_a136b53ad685419582210f02b620e762~mv2.png/v1/crop/x_0,y_0,w_2000,h_973/fill/w_939,h_457,al_c,q_95,enc_auto/Constructor.png)

**Contrairement aux autres fonctions, calldata ne peut être utilisé pour les tableaux et chaînes de caractères, vous devez utiliser memory**

Aussi, pour des raisons dont nous ne pouvons parler maintenant, calldata ne peut être utilisé avec les paramètres d'une fonction. Je sais, cela peut sembler bizarre et arbitraire comme restriction, mais ça fera sens plus tard, après que vous ayez compris comment Ethereum fonctionne sous le capot.

Voici comment définir une chaîne de caractères au moment du déploiement:

```solidity

contract ExampleContract {
    string public name;

    // if you use calldata, it won't compile
    constructor(string memory _name) {
        name = _name;
    }
}
```

Vous pourriez être tenté de juste utiliser "memory" partout et ne pas vous soucier d'employer calldata. Mais ça vaut la peine de retenir ceci pour le moment, vu que calldata resulte en des frais moins élévés (C'est-à-dire moins de frais de gaz pour l'utilisateur).

Aussi, au cas où vous vous posiez la question, **les constructeurs ne peuvent retourner de valeur.**

**Exercices Pratiques**

[Deployer](https://github.com/RareSkills/Solidity-Exercises/tree/main/Deployer)
