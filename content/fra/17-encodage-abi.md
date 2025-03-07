# L'encodage de l'Interface Binaire d'Application (ABI encoding)

Nous allons devoir aller sur ce qui semble une autre déviation aléatoire avant de pouvoir introduire notre prochaine information.

Mais j'aimerais que vous compreniez ce que sont les éléments suivants:

- abi.encode

- abi.decode

- abi.encodeWithSignature

Pour les illustrer, créons un autre contrat intelligent, ouvrons le menu déroulant "debug", et obtenons une certaine information.

![https://static.wixstatic.com/media/61a666_57ac790548bd46f293823e0574f3e152~mv2.png/v1/fill/w_939,h_447,al_c,q_95,enc_auto/ABI%20Encoding1.png](https://static.wixstatic.com/media/61a666_57ac790548bd46f293823e0574f3e152~mv2.png/v1/fill/w_939,h_447,al_c,q_95,enc_auto/ABI%20Encoding1.png)

```solidity

contract ExampleContract {

    function meaningOfLifeAndAllExistence()
        public 
        pure 
        returns (uint256) {
            return 42;
    }
}

```

Quand nous copions ceci, nous obtenons :

**0x92d62db5**

Qu'est-ce exactement ? Ceci est la **signature de la fonction** "meaningOfLifeAndAllExistence()". Nous verrons plus tard comment elle est dérivée.

Chaque fois que vous "appelez" un contrat intelligent, vous êtes en réalité en train d'envoyer une transaction Ethereum avec quelques données qui y sont attachées afin que le contrat intelligent sache quelle fonction exécuter.

Examinons l'information sous un autre angle.

![https://static.wixstatic.com/media/61a666_38d5431035ee47bca02b56df9b3f92f7~mv2.png/v1/crop/x_2,y_0,w_1998,h_882/fill/w_939,h_414,al_c,q_95,enc_auto/ABI%20Encoding2.png](https://static.wixstatic.com/media/61a666_38d5431035ee47bca02b56df9b3f92f7~mv2.png/v1/crop/x_2,y_0,w_1998,h_882/fill/w_939,h_414,al_c,q_95,enc_auto/ABI%20Encoding2.png)

```solidity

contract ExampleContract {

    function meaningOfLifeAndAllExistence()
        public 
        pure 
        returns (bytes memory) {
            return msg.data;
    }
}

```

Nous avons changé le type de retour pour "bytes memory" (ne vous en faites pas si nous ne l'avons pas déjà vu avant), et retourné une variable appelée msg.data (ici aussi, ne vous en faites pas si nous ne l'avons pas déjà vu).

Le point important à noter est que nous obtenons une séquence d'octets identique!

Alors qu'est-ce qui se passe?

Quand vous appelez une fonction dans un contrat intelligent, vous ne faites en réalité pas un "appel de fonction" à proprement parler, vous envoyez des données au contrat avec des informations sur la fonction à exécuter.

Ça a du sens, non? Lorsque vous ouvrez votre portefeuille web et échangez des jetons ERC20, il n'y a aucun moyen pour vous "d'appeler une fonction" sur un contrat ERC20 à distance. Les appels de fonction se produisent uniquement à l'intérieur d'un même contexte d'exécution. Décrire les transactions comme des fonctions, est cependant pratique. Mais nous devons regarder derrière le rideau pour voir exactement ce qui se passe afin de réellement comprendre Solidity.

Lorsque vous "appelez un smart contract", vous lui envoyez des données contenant des instructions sur la manière dont il doit s'exécuter.

Il y a de nombreux formats d'encodage de données: JSON, XML, Protobufs, etc. **Solidity et Ethereum utilisent l'encodage ABI**.

Nous n'entrerons pas dans les spécifications de l'ABI ici. Mais ce que vous devez savoir, c'est que cela ressemble toujours à une séquence d'octets.

Les fonctions sont identifiées par une séquence de 4 octets. Notre séquence d'octets originale **(0x92d62db5)** contenait 4 octets: 92, d6, 2d, b5.

Rappelez-vous, un octet est composé de 8 bits, et 8 bits peuvent représenter une valeur allant jusqu'à 255 (2^8 - 1). Un octet, representé en hexadécimal, peut aller de 0x00 à 0xff. Convertissez 0xff en décimal, cela devrait clarifier les choses.

Lorsqu'une fonction ne prend aucun argument, l'envoi de 4 octets représentant la fonction indique au smart contract d'exécuter cette fonction.

Mais à quoi ressemblerait les données si la fonction prenait un argument?

![https://static.wixstatic.com/media/61a666_2afb4e91b2f0423ba0d719a3d8b382ec~mv2.png/v1/fill/w_939,h_209,al_c,q_95,enc_auto/ABI%20Encoding3.png](https://static.wixstatic.com/media/61a666_2afb4e91b2f0423ba0d719a3d8b382ec~mv2.png/v1/fill/w_939,h_209,al_c,q_95,enc_auto/ABI%20Encoding3.png)

```solidity

contract ExampleContract {
    function takeOneArg(uint256 x)
        public 
        pure 
        returns (bytes memory) {
            // we won't do anything with x
            return msg.data;
    }
}

```

Nous obtenons

0xf8689fd30000000000000000000000000000000000000000000000000000000000000007

en retour. La portion f8689fd3 signifie appeler la fonction "takeOneArg" et le 7 avec beaucoup de zéros en tête signifie passer le nombre 7.

Ce serait très compliqué si nous devions faire cela à la main.

Heureusement, nous n'avons pas à le faire.

Regardez ceci.

![https://static.wixstatic.com/media/61a666_687ca50bc74449e28295e2641620d6c1~mv2.png/v1/fill/w_939,h_256,al_c,q_95,enc_auto/ABI%20Encoding4.png](https://static.wixstatic.com/media/61a666_687ca50bc74449e28295e2641620d6c1~mv2.png/v1/fill/w_939,h_256,al_c,q_95,enc_auto/ABI%20Encoding4.png)

```solidity

contract ExampleContract {

    function getEncoding(uint x)
        public 
        pure 
        returns (bytes memory) {
            return abi.encodeWithSignature("takeOneArg()", x);
    }
    
    function takeOneArg(uint256 x)
        public 
        pure 
        returns (bytes memory) {
            return msg.data;
    }
}

```

Nous n'avons pas besoin de nous préoccuper de la spécification de l'encodage ABI pour l'instant, concentrons-nous simplement sur son utilisation.

Considérons l'exemple suivant.

![https://static.wixstatic.com/media/61a666_ef35fbdaa086479aa76b7ce06edceb08~mv2.png/v1/fill/w_939,h_297,al_c,q_95,enc_auto/ABI%20Encoding5.png](https://static.wixstatic.com/media/61a666_ef35fbdaa086479aa76b7ce06edceb08~mv2.png/v1/fill/w_939,h_297,al_c,q_95,enc_auto/ABI%20Encoding5.png)

```solidity

contract ExampleContract {

    function encodingXY(uint x, uint256 y)
        public 
        pure 
        returns (bytes memory) {
            return abi.encode(x,y);
    }
    
    function getATuple(bytes memory encoding)
        public 
        pure 
        returns (uint256, uint256) {
            (uint256 x, uint256 y) = abi.decode(encoding, 
                (uint256, uint256));
            return(x,y);
    }
}

```

Notez que nous utilisons "abi.encode" et "abi.decode". La partie "withSignature" intervient lorsqu'il y a des fonctions, mais ce n'est pas le cas ici.

Dans cet exemple, les variables x et y sont encodées en ABI sous forme de:

0x0000000000000000000000000000000000000000000000000000000000000005000000000000000000000000000000000000000000000000000000000000000f

Les nombres décimaux ont été convertis en hexadécimal, ce qui explique pourquoi "5" est toujours "5" mais 15 est devenu "f".

Si nous savons à l'avance qu'il s'agit d'une paire de uint256, nous pouvons la "décoder" en utilisant la fonction capturée ci-dessus.

Le tuple qui apparait comme deuxième argument dans abi.decode représente les instructions sur la façon de décoder les données. Si vous fournissez les mauvais types de données ou une longueur de tuple incorrecte, vous obtiendrez soit un résultat erroné, ou le code sera annulé (revert).

**Exercices Pratiques**

[Encode](https://github.com/RareSkills/Solidity-Exercises/tree/main/Encoder)

[Decoder](https://github.com/RareSkills/Solidity-Exercises/tree/main/Decoder)
