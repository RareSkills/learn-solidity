# Types de données de taille fixe : Solidity est un langage typé.

Contrairement à JavaScript ou Python où vous pouvez affecter un booléen, une chaine de caractères, ou un nombre, à une variable, chaque variable ne peut avoir qu'un seul type, et elle doit être explicitement déclarée en tant que tel.

Ceci s'applique également aux fonctions. Vous devez explicitement spécifier le type des paramètres et de ce que la fonction retourne.

Découvrons ensemble les types les plus communs:

- L'entier non signé (unsigned integer), ou **uint256**
- Le booléen (boolean) ou **bool**
- Le type adresse ou **address**, qui stocke les addresses de wallet(portefeuille) Ethereum ou les addresses de contrats intelligents.

Solidity a également des tableaux(array), chaînes de caractères(string), structures(structs), et d'autres types, mais ils nécessitent un traitement différent, nous en parlerons donc plus tard.

Jetons un coup d'oeil à trois différentes fonctions qui retournent chacun de ces types.

```solidity

contract ExampleContract {
    function getANumber()
        public
        pure
        returns (uint256) {
            uint256 x = 1;
            return x;
    }

    function getABoolean()
        public
        pure
        returns (bool) {
            bool y = true;
            return y;
    }

    function getAnAddress()
        public
        pure
        returns (address) {
            // Vitalik Buterin's address
            address z = 0xd8da6bf26964af9d7eed9e03e53415d37aa96045;
            return z;
    }

    function getAnotherAddress()
        public
        pure
        returns (address) {
            // address of the USDC stablecoin
            address z2 = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48;
            return z2;
    }
}

```

Dans ces exemples, nous avons affecté une valeur à une variable et l'avons retournée. Nous pouvons bien-sûr retourner directement la valeur comme suit:

```solidity

function getAddress() 
        public 
        pure 
        returns (address) {
    return 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48;
}
```

Il est très important que la signature de la fonction corresponde au type retourné. Le code suivant produira une erreur:

```solidity

function getAddressFail()
    public
    pure
    returns (bool) {
        return 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48;
}
```

**Address**

Une adresse est représentée comme une chaîne hexadécimale (hex string) contenant 40 caractères, et commençant toujours par 0x. Une chaîne hexadécimale valide est constituée des caractères [0-9] et/ou [a-f].

**Attention**: Soyez vigilant lorsque vous entrez des adresses manuellement. Solidity convertira 0x1 en une adresse de valeur  0x0000000000000000000000000000000000000001. Si vous avez une adresse de moins de 40 caractères, il ajoutera des 0 pour atteindre la taille requise.

Si vous créez une adresse de plus de 40 caractères, la compilation échouera.

Notez bien que 0x n'est pas inclu dans les 40 caractères.

**uint256**

Penchons-nous sur le cas de **uint256**. Que celà représente-t-il exactement?

Le *u* signifie "unsigned" (non signé). Il ne peut donc contenir des nombres négatifs. Le 256 indque qu'il peut stocker des nombres d'une largeur maximale de **256** bits, ou 2^256-1.

Déterminons avec Python, le plus grand nombre stockable dans un uint256.

```solidity

>>> pow(2,256) - 1
115792089237316195423570985008687907853269984665640564039457584007913129639935
```

Ce nombre est extrêment large, assez grand pour quasiment tout ce que vous aurez besoin de faire sur la blockchain.

Ce code compile avec succès en Solidity

```solidity

function getBiggestNumber()
    public
    pure
    returns (uint256) {
        return 115792089237316195423570985008687907853269984665640564039457584007913129639935;
}

```

Mais si vous augmentez ce nombre, la compilation échouera.

Comme vous pouvez le deviner, un **uint128** stocke des nombres positifs allant jusqu'à 2^128 - 1.

La plupart du temps, vous devriez utiliser uniquement uint256. Les fois où il faut utiliser un type plus petit comme uint64 ou uint128 est un sujet plus avancé. Pour le moment, restons-en à uint256.

Le type booléen (Boolean)

Ici il n'y a rien de bien nouveau, c'est pareil que dans les autres langages. Une variable de type booléen peut contenir true(vrai) ou false(faux). Voilà tout.
