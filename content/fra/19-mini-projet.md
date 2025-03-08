# Mini projet d'échange de tokens

**C'est un défi**

Croyez-le ou non, vous avez maintenant suffisamment de connaissances préalables pour construire un contrat intelligent très simple d'échange de tokens ! Voici votre mission.

Construisez deux contrats ERC20 : **RareCoin** et **SkillsCoin** (vous pouvez changer les noms si vous le souhaitez). N'importe qui peut créer des SkillsCoin, mais la seule façon d'obtenir des RareCoin est d'envoyer des SkillsCoin au contrat RareCoin. Vous devrez supprimer la restriction qui empêche la création de SkillsCoin par d'autres que le propriétaire.

Voici le procédé:

- **mint()** mintez des SkillsCoin pour vous-même
- **SkillsCoin.approve(address rareCoinAddress, uint256 yourBalanceOfSkillsCoin)** Permettez à RareCoin d'utiliser vos tokens.
- **RareCoin.trade()** Cela fera en sorte que RareCoin appelle **SkillsCoin.transferFrom(address you, address RareCoin, uint256 yourBalanceOfSkillsCoin)** Rappelez-vous, RareCoin peut connaitre sa propre adresse avec address(this)
- **RareCoin.balanceOf(address you)** devrait retourner le nombre de tokens que vous avez initalement minté pour SkillsCoin.

Souvenez-vous que les **tokens (ou contrats) ERC20** **peuvent posséder d'autres tokens ERC20**. Ainsi, lorsque vous appelez RareCoin.trade(), cela devrait appeler SkillsCoin.transferFrom et transférer vos SkillsCoin vers lui-même, c'est-à-dire. address(this).

Si vous avez l'adresse de SkillsCoin stockée, cela devrait ressembler à ceci:

```solidity

function trade(uint256 amount) 
    public {
        // some code
        // you can pass the address of the deployed SkillsCoin contract as a parameter 
        // to the constructor of the RareCoin contract as 'source'
        (bool ok, bytes memory result) = source.call(
            abi.encodeWithSignature(
                "transferFrom(address,address,uint256)", 
                msg.sender, 
                address(this), 
                amount
            )
        );
        // this will fail if there is insufficient approval or balance
        require(ok, "call failed");
        // more code
}

```

Déployez ces contrats dans Remix et vérifiez qu'ils fonctionnent.

Si vous êtes nouveau sur Solidity, réservez quelques jours pour cela. Beaucoup d'ingénieurs sont déconcertés par le fait que les soldes sont stockés dans les contrats intelligents, et non dans les portefeuilles, donc cela prend un peu de temps pour s'y habituer. De plus, croyez-moi, vous serez confus par les appels croisés de contrats.
