# msg.sender et address(this)

Vous vous rappelez notre exemple précédent d'un token ERC20 invalide?

Le voici de nouveau:

```solidity

contract ERC20Token {

    mapping(address => uint256) public balances;

    function setSomeonesBalance(
        address owner, 
        uint256 amount
    ) 
        public {
            balances[owner] = amount;
    }

    function transferTokensBetweenAddresses(
        address sender, 
        address receiver, 
        uint256 amount
    ) 
        public {
            balances[sender] -= amount;   // deduct/debit the sender's balance
            balances[receiver] += amount; // credit the receiver's balance
    }
}

```

Ici, la faille est que nous ignorons qui appelle les fonctions.

Par chance, Solidity a un méchanisme permettant de savoir qui appelle un contrat intelligent: **msg.sender**. 

msg.sender retourne l'adresse de celui qui appelle une fonction d'un contrat intelligent.

Essayez le code suivant sur Remix:

```solidity

contract ExampleContract {
    function whoami()
        public
        view 
        returns (address) {
            address sender = msg.sender;
            return sender;
    }
}
```

Il retournera l'adresse de test que vous utilisez sur Remix.

Maintenant, changez l'adresse de test en sélectionnant la liste déroulante "ACCOUNT". Puis réessayez la fonction. L'adresse retournée sera différente.

![https://static.wixstatic.com/media/61a666_6911916215e746e4be2a25bdbcf2e9ac~mv2.png/v1/fill/w_939,h_610,al_c,q_95,enc_auto/msgsender.png](https://static.wixstatic.com/media/61a666_6911916215e746e4be2a25bdbcf2e9ac~mv2.png/v1/fill/w_939,h_610,al_c,q_95,enc_auto/msgsender.png)

En combinant msg.sender avec une structure If, vous pouvez donner à une adresse certains privilèges.

Supposons que nous voulons que l'adresse par défaut de Remix soit l'adresse spéciale.

```solidity

contract ERC20Token {
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

    function transferTokensBetweenAddresses(
        address sender, 
        address receiver, 
        uint256 amount
    ) 
        public {
            if (msg.sender == banker) {
                balances[sender] -= amount;   // deduct/debit the sender's balance
                balances[receiver] += amount; // credit the reciever's balance
            }
            // do nothing
    }
}

```

Le code suivant permet aux personnes de voir leur solde (puisque `balances` est une variable `public`), mais uniquement le `banker` peut les modifier.

En étant un peu plus malins, nous pouvons permettre à quiconque de transférer ses fonds à quelqu'un d'autre sans avoir à passer par le banquier("banker"). Prenons l'exemple suivant:

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

La fonction **transfer** peut être appelée par n'importe qui. Cependant, elle peut uniquement débiter le solde du **msg.sender**. Comme exercice pour le lecteur, je vous encourage à réfléchir et trouver pourquoi il est impossible de voler le solde de quelqu'un d'autre en utilisant **transfer**.

Une question naturelle à se poser, que se passe-t-il si quelqu'un essaie d'envoyer un montant supérieur à son solde? Si vous utilisez Solidity 0.8.0 ou plus, rien ne se passe. La transaction échoue vu que vous ne pouvez faire des soustractions sur des entiers non-signés et obtenir un résultat négatif.

**tx.origin**

Il existe un autre mécanisme permettant de connaitre celui qui interagit avec un contrat intelligent, **tx.origin**. Bien qu'il se comporte de façon similaire à msg.sender, vous ne devriez pas l'utiliser. Pour éviter de vous bombarder avec trop d'informations, nous n'expliquerons pas tout de suite les failles de sécurité autour de **tx.origin**. Mais ce qu'il faut retenir est ceci, ne pas utiliser tx.origin mis à part dans des conditions bien spécifiques.

**address(this)**

Un contrat intelligent peut connaitre sa propre adresse avec le code suivant:

```solidity

contract ExampleContract {

    function whoami()
        public
        view 
        returns (address) {
            return address(this);
    }
}
```

Essayez-le sur Remix et voyez que les addresses correspondent.

![https://static.wixstatic.com/media/61a666_055fd70f34e24119a3e16f51820845f3~mv2.png/v1/fill/w_939,h_275,al_c,q_95,enc_auto/addressmsg.png](https://static.wixstatic.com/media/61a666_055fd70f34e24119a3e16f51820845f3~mv2.png/v1/fill/w_939,h_275,al_c,q_95,enc_auto/addressmsg.png)

**Exercices Pratiques**

[WhoCalledMe](https://github.com/RareSkills/Solidity-Exercises/tree/main/WhoCalledMe)
