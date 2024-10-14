# Tokens ERC20

Nous sommes désormais prêts à créer un token ERC20!

Les tokens ERC20 ont généralement un **nom(name)** et un **symbole(symbol)**. Par exemple, ApeCoin a pour nom  "ApeCoin" mais pour symbole "APE." Le nom du token ne change pratiquement pas, nous allons donc le définir dans le constructeur et ne pas créer de fonction pour le modifier plus tard. Nous rendrons ces variables "public" pour que quinconque puisse avoir accès au nom et symbole du contrat.

```solidity

contract ERC20 {
    string public name;
    string public symbol;

    constructor(string memory _name, string memory _symbol) {
        name = _name;
        symbol = _symbol;
    }
}
```

Ensuite, nous devrons stocker les soldes de tout un chacun.

```solidity

contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;

    constructor(string memory _name, string memory _symbol) {
        name = _name;
        symbol = _symbol;
    }
}
```

Nous disons "balanceOf" parce que cela fait partie de la spécification [ERC20](https://eips.ethereum.org/EIPS/eip-20). La spécification ERC20 indique que des personnes peuvent appeler la fonction "balanceOf" sur votre contrat, fournir une adresse, et obtenir le nombre de tokens que cette adresse possède.

Tout le monde a un solde nul pour le moment, il nous faut un moyen de donner vie à de nouveaux tokens. Nous permettrons donc à une adresse spéciale, celle de la personne qui déploie le contrat, de créer des tokens à volonté.

```solidity

contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;
    address public owner;

    constructor(string memory _name, string memory _symbol) {
        name = _name;
        symbol = _symbol;

        owner = msg.sender;
    }

    function mint(address to, uint256 amount) public {
        require(msg.sender == owner, "only owner can create tokens");
        balanceOf[owner] += amount;
    }
}
```

C'est une pratique standard que la fonction mint() prenne *to* et *amount* comme paramètres. Cela permet au déployeur du contrat de minter(créer) des tokens pour d'autres adresses. Pour faire simple, la fonction mint() n'autorisera pour le moment au déployeur qu'à minter des tokens pour lui-même.

Pour faire un suivi du nombre de tokens en circulation, la specification ERC20 nécessite une variable "public"  **totalSupply** qui nous indique combien de tokens ont été créés.

```solidity

contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;
    address public owner;

    uint256 public totalSupply;

    constructor(string memory _name, string memory _symbol) {
        name = _name;
        symbol = _symbol;

        owner = msg.sender;
    }

    function mint(address to, uint256 amount) public {
        require(msg.sender == owner, "only owner can create tokens");
        totalSupply += amount;
        balanceOf[owner] += amount;

    }

    function transfer(address to, uint256 amount) public {
        require(balanceOf[msg.sender] >= amount, "you aint rich enough");
        require(to != address(0), "cannot send to address(0)");
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
    }
}
```

Si vous avez déjà eu à utiliser des tokens ERC20 dans votre wallet(portefeuille), vous avez certainement rencontré des cas où vous possédez des fractions d'un token. Comment est-ce possible vu que les "integers"(entiers) n'ont pas de décimales?

Le plus grand nombre qu'un uint256 peut contenir est :

115792089237316195423570985008687907853269984665640564039457584007913129639935

Réduisons un peu ce nombre pour le rendre plus clair.

100000000000000000000000000000000000000000000000000000000000000000000000000000

Pour pouvoir utiliser les "décimales(decimals)", les 18 zéros à droite sont considérés comme la partie fractionnelle du token.

100000000000000000000000000000000000000000000000000000000000.000000000000000000

Alors, si notre ERC20 a 18 décimales, nous pouvons avoir au maximum:

10000000000000000000000000000000000000000000000000000000000

pièces, les zéros à droite représentant la partie décimale. Il s'agit de 10 nonilliard pièces, ou pour ceux non habitués à des nombres aussi inutilement grands, c'est 10 billiard x 1 billiard x 1 billiard x 1 billion.

10 nonilliard devrait être suffisant pour la plupart des applications, voire pour les pays qui vont en hyper-inflation.

Les "units"(dénominations) de la monnaie sont toujours des entiers, mais de très faible valeur.

18 décimales est assez courant, mais certains tokens utilisent 6 décimales.

Le nombre de décimales d'un token ne devrait pas changer, ainsi la fonction "decimals()" retourne juste le nombre de décimales que le token a.

```solidity

contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;
    address public owner;
    uint8 public decimals;

    uint256 public totalSupply;

    constructor(string memory _name, string memory _symbol, uint8 decimals) {
        name = _name;
        symbol = _symbol;
        decimals = 18;

        owner = msg.sender;
    }

    function mint(address to, uint256 amount) public {
        require(msg.sender == owner, "only owner can create tokens");
        totalSupply += amount;
        balanceOf[owner] += amount;
    }
}
```

Si vous avez remarqué, je vous ai un peu pris de court plus haut. Le type du nombre est uint8, au lieu de uint256. uint8 peut uniquement contenir des nombres allant jusqu'à 255. Cependant, un uint256 a 77 zéros (si ça vous dit de compter le nombre de 0 du nombre plus haut, vous pouvez le vérifier). Ainsi, il n'est pas possible d'avoir plus de 77 décimales si vous souhaitez avoir un token entier. Alors, le standard nous dicte d'utiliser un uint8 vu que le nombre de décimales ne peut jamais être plus grand.

**Transfer**

Ajoutons de nouveau notre fonction `transfer`.

```solidity

contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;
    address public owner;
    uint8 public decimals;

    uint256 public totalSupply;

    constructor(string memory _name, string memory _symbol) {
        name = _name;
        symbol = _symbol;
        decimals = 18;

        owner = msg.sender;
    }

    function mint(address to, uint256 amount) public {
        require(msg.sender == owner, "only owner can create tokens");
        totalSupply += amount;
        balanceOf[owner] += amount;
    }

    function transfer(address to, uint256 amount) public {
        require(balanceOf[msg.sender] >= amount, "you aint rich enough");
        require(to != address(0), "cannot send to address(0)");
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
    }
}
```

Aha, nous avons glissé une ligne de code supplémentaire : **require(to != address(0), "cannot send to address(0))**

Pourquoi cet ajout? Bien, personne n'est "propriétaire" de l'adresse zéro, les tokens qui y sont envoyés ne peuvent donc être dépensés. Par convention, envoyer un token à l'adresse zéro devrait réduire la **totalSupply (quantité de tokens en circulation)** donc nous préférons avoir une fonction spécialement pour ça.

Maintenant introduisons le concept d'**allocation**.

**Allowance (Allocation)**

Une allocation autorise une adresse à dépenser les tokens de quelqu'un d'autre, jusqu'à une certaine quantité définie par celui-ci.

Pourquoi autoriseriez-vous quelqu'un à dépenser vos tokens? C'est une très longue histoire, mais en résumé, demandez-vous comment vous "sauriez" que quelqu'un vous a transféré des tokens. Une fonction est juste éxécutée et un mappage change de valeur. Vous n'avez pas vraiment "reçu" de tokens, ils ont juste été associés à votre adresse.

Et là, comme entité extérieure à la blockchain, vous pouvez l'inspecter pour suivre les évènements qui vous rendent plus riches.

Cependant, les contrats intelligents ne peuvent le faire.

Le schéma défini pour permettre aux contrats intelligents d'être récipiendaires de transferts, est d'allouer au contrat une certaine allocation, puis dire au contrat de faire un retrait de tokens depuis votre compte.

Quand vous souhaitez transférer des tokens à un contrat intelligent, la méthode usuelle est de commencer par approuver au smart contract le retrait d'une quantité de tokens depuis votre compte. Ensuite, vous devez demander au contrat de retirer la quantité approuvée de tokens de votre compte. C'est un schéma récurrent utilisé dans les contrats intelligents afin de permettre le transfert de tokens aux contrats.

Ajoutons à présent une variable pour suivre les allocations, et le moyen d'en attribuer une à un autre utilisateur.

```solidity

contract ERC20 {
    string public name;
    string public symbol;
    
    mapping(address => uint256) public balanceOf;
    address public owner;
    uint8 public decimals;

    uint256 public totalSupply;

    // owner -> spender -> allowance
    // this enables an owner to give allowance to multiple addresses
    mapping(address => mapping(address => uint256)) public allowance; 
    // just added address public owner;
    

    constructor(string memory _name, string memory _symbol) {
        name = _name;
        symbol = _symbol;
        decimals = 18;

        owner = msg.sender;
    }

    function mint(address to, uint256 amount) public {
        require(msg.sender == owner, "only owner can create tokens");
        totalSupply += amount;
        balanceOf[owner] += amount;
    }

    function transfer(address to, uint256 amount) public {
        require(balanceOf[msg.sender] >= amount, "you aint rich enough");
        require(to != address(0), "cannot send to address(0)");
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
    }

    // just added
    function approve(address spender, uint256 amount) public {
        allowance[msg.sender][spender] = amount;
    }
}
```

A la ligne allowance[msg.sender][spender] = amount;, "spender" fait référence à l'adresse du compte qui reçoit l'allocation du "msg.sender". Le "msg.sender" donne la permission au "spender" de dépenser une quantité de tokens depuis son compte.

Donc, "msg.sender" est le propriétaire des tokens et "spender" est celui qui a été approuvé par le propriétaire à dépenser une quantité de token à sa place.

Ah, mais nous n'avons pas de moyen de dépenser l'allocation qui a été faite, elle reste juste là! Voilà à quoi sert "transferFrom".

**transferFrom**

```solidity

contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;
    address public owner;
    uint8 public decimals;
    uint256 public totalSupply;

    // owner -> spender -> allowance
        // this enables an owner to give allowance to multiple addresses
    mapping(address => mapping(address => uint256)) public allowance;

    constructor(string memory _name, string memory _symbol) {
        name = _name;
        symbol = _symbol;
        decimals = 18;

        owner = msg.sender;
    }

    function mint(address to, uint256 amount) public {
        require(msg.sender == owner, "only owner can create tokens");
        totalSupply += amount;
        balanceOf[owner] += amount;
    }

    function transfer(address to, uint256 amount) public {
        require(balanceOf[msg.sender] >= amount, "you aint rich enough");
        require(to != address(0), "cannot send to address(0)");
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
    }

    function approve(address spender, uint256 amount) public {
        allowance[msg.sender][spender] = amount;
    }

    // just added
    function transferFrom(address from, address to, uint256 amount) public {
        require(balanceOf[from] >= amount, "not enough money");

        if (msg.sender != from) {
            require(allowance[from][msg.sender] >= amount, "not enough allowance");

            allowance[from][msg.sender] -= amount;
        }

        balanceOf[from] -= amount;
        balanceOf[to] += amount;
    }
}
```

Déballons ce qui a été fait ici.

Tout d'abord, il est possible pour le propriétaire des tokens d'appeler transferFrom. Dans ce cas, l'allocation n'est pas nécessaire, nous ne vérifions donc pas le mappage "allowance", et mettons à jour les soldes comme il se doit.

Autrement, nous vérifions que le "spender" a reçu suffisamment d'allocation, puis soustrayons le montant qu'il dépense. Si nous n'avions pas soustrait ses dépenses, il aurait eu une capacité de dépense illimitée.

Il y a une dernière correction à faire. Si nous lisons la spécification originale de [EIP 20](https://eips.ethereum.org/EIPS/eip-20) elle précise que "approve", "transfer", et "transferFrom" doivent retourner "true" en cas de succès. Ajoutons cela à notre code.

```solidity

contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;
    address public owner;
    uint8 public decimals;

    uint256 public totalSupply;

    // owner -> spender -> allowance
    // this enables an owner to give allowance to multiple addresses
    mapping(address => mapping(address => uint256))
        public allowance;

    constructor(
        string memory _name, 
        string memory _symbol
    ) {
        name = _name;
        symbol = _symbol;
        decimals = 18;

        owner = msg.sender;
    }

    function mint(
        address to, 
        uint256 amount
    ) 
        public {
            require(msg.sender == owner, 
                "only owner can create tokens");
            totalSupply += amount;
            balanceOf[owner] += amount;
    }

    function transfer(
        address to, 
        uint256 amount
    ) 
        public 
        returns (bool) {
            require(balanceOf[msg.sender] >= amount, 
                "you aint rich enough");
            require(to != address(0), 
                "cannot send to address(0)");
            balanceOf[msg.sender] -= amount;
            balanceOf[to] += amount;

            return true;
    }

    function approve(
        address spender, 
        uint256 amount
    ) 
        public 
        returns (bool) {
            allowance[msg.sender][spender] = amount;

            return true;
    }

    function transferFrom(
        address from, 
        address to, 
        uint256 amount
    ) 
        public 
        returns (bool) {
            require(balanceOf[from] >= amount, 
                "not enough money");
            require(to != address(0), 
                "cannot send to address(0)");

            if (msg.sender != from) {
                require(allowance[from][msg.sender] >= amount, 
                    "not enough allowance");

                allowance[from][msg.sender] -= amount;
            }

            balanceOf[from] -= amount;
            balanceOf[to] += amount;

            return true;
    }
}

```

Au risque de vous envoyer trop d'informations d'un coup, nous pouvons encore rendre ce code plus propre. Notez que **transferFrom** et **transfer** ont un bout de code en commun. Que faire par rapport à cela? Nous pouvons réécrire le code de mise à jour de soldes dans une fonction séparée, mais en s'assurant que cette fonction n'est pas publique sinon quelqu'un pourrait dérober des tokens!

```solidity

contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;
    address public owner;
    uint8 public decimals;

    uint256 public totalSupply;

    // owner -> spender -> allowance
    // this enables an owner to give allowance to multiple addresses
    mapping(address => mapping(address => uint256))
        public allowance;

    constructor(
        string memory _name, 
        string memory _symbol
    ) {
        name = _name;
        symbol = _symbol;
        decimals = 18;

        owner = msg.sender;
    }

    function mint(
        address to, 
        uint256 amount
    ) 
        public {
            require(msg.sender == owner, 
                "only owner can create tokens");
            totalSupply += amount;
            balanceOf[owner] += amount;
    }

    function transfer(
        address to, 
        uint256 amount
    ) 
        public 
        returns (bool) {
            return helperTransfer(msg.sender, to, amount);
    }

    function approve(
        address spender, 
        uint256 amount
    ) 
        public 
        returns (bool) {
            allowance[msg.sender][spender] = amount;

            return true;
    }

    function transferFrom(
        address from, 
        address to, 
        uint256 amount
    ) 
        public 
        returns (bool) {
            if (msg.sender != from) {
                require(allowance[from][msg.sender] >= amount, 
                    "not enough allowance");

                allowance[from][msg.sender] -= amount;
            }

            return helperTransfer(from, to, amount);
    }

    function helperTransfer(
        address from, 
        address to, 
        uint256 amount
    ) 
        internal 
        returns (bool) {
            require(balanceOf[from] >= amount, 
                "not enough money");
            require(to != address(0), 
                "cannot send to address(0)");
            balanceOf[from] -= amount;
            balanceOf[to] += amount;

            return true;
    }
}

```

Beaucoup plus propre!

Exercices Pratiques

- Modifier le code plus haut afin qu'il n'autorise qu'un maximum de 1 million de tokens à entrer en circulation, même si le "owner"(propriétaire) essaie d'en minter plus.
