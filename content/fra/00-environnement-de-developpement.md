# Environnement de Développement

Pour commencer, nous allons apprendre solidity comme langage. Nous n'allons pas tout de suite déployer des contrats intelligents sur la blockchain, ça rendrait juste les choses plus compliquées.

Rendez vous sur [remix.ethereum.org](https://remix.ethereum.org/)

Nous vous recommandons vivement d'utiliser Remix pour pratiquer les exemples de ce cours.

Créons un "Hello World".
    
```solidity
contract ExampleContract {

    function helloWorld() 
        public 
        pure 
        returns (uint256) {
            return 100;
    }

    function haloDunia() 
        public 
        pure 
        returns (bool) {
            return true;
    }
}
```
    
Une fois sur remix.ethereum.org, faites un clic droit sur le dossier "contracts" et choisissez Nouveau Fichier ("New File")

![Choisir "new file" sur Remix](https://static.wixstatic.com/media/c0c19a_489ef2f484f548cf888c995d79cd54ac~mv2.png)

C'est un fichier solidity qu'il faut créer, donnez-lui donc l'extension .sol. Le nom du fichier n'est pas important.

![https://static.wixstatic.com/media/c0c19a_3f7aed5e540941efbf85e0c8983090f1~mv2.png](https://static.wixstatic.com/media/c0c19a_3f7aed5e540941efbf85e0c8983090f1~mv2.png)

Copiez et collez le code ci-dessus, ou mieux encore, saisissez-le vous-même.

![Code Solidity sur l'éditeur Remix](https://static.wixstatic.com/media/c0c19a_ec23c5ddf49743b69cb70f0310903faf~mv2.png)

Pour compiler le code, maintenez *Command S* sur mac (*CTRL S* sur Windows). Si vous remarquez une bulle rouge sur le symbole de Solidity, votre code doit avoir un problème de syntaxe. Si la bulle est orange, il s'agit juste d'avertissements que vous pouvez ignorer pour le moment.

Maintenant déployez votre code. Cliquez sur le symbole Ethereum à gauche, puis sur Déployer ("Deploy").

![Cliquer sur le symbole Ethereum](https://static.wixstatic.com/media/c0c19a_7cb41170d78c4a34bef83c67ddc3be36~mv2.png)

Pour tester les fonctions, faites défiler le menu à gauche, puis cliquer sur elles. Elles retourneront les valeurs attendues.
![https://static.wixstatic.com/media/c0c19a_cfb667754c4a4398bbdf27c7c4914a76~mv2.png](https://static.wixstatic.com/media/c0c19a_cfb667754c4a4398bbdf27c7c4914a76~mv2.png)

Et si on veut apporter des modifications ? Supprimez le contrat en cliquant sur l'icone de Corbeille.

![Supprimer un contrat intelligent sur Remix](https://static.wixstatic.com/media/c0c19a_0767e7d240244ca2b4e14a9ea8c0b669~mv2.png)

Maintenant modifiez le code, recompilez avec *command S* ou *CTRL S*, puis choisissez Déployer ("Deploy"). Testez de nouveau les fonctions.

![https://static.wixstatic.com/media/c0c19a_074c5f0e3de540a09e1d2d32e9672f03~mv2.png](https://static.wixstatic.com/media/c0c19a_074c5f0e3de540a09e1d2d32e9672f03~mv2.png)

Si une fonction a besoin d'un argument, il sera disponible près du bouton.

![Fonction retournant une valeur](https://static.wixstatic.com/media/c0c19a_4776ae8f1f634e8ebb309e62ef0d8ebb~mv2.png)

Vous êtes désormais prêts à expérimenter avec les contrats intelligents en Solidity!
