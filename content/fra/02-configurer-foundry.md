# Mettre en place votre environnement

Si vous n'avez pas déjà installé cURL sur votre système, veuillez visiter le lien suivant pour le faire:

[cURL setup](https://help.ubidots.com/en/articles/2165289-learn-how-to-install-run-curl-on-windows-macosx-linux)

```solidity

# installer foundry
curl -L https://foundry.paradigm.xyz | bash

# cloner les exercices pratiques
git clone https://github.com/rareSkills/solidity-exercises.git

# Ouvrir le premier exercice
cd solidity-exercises/EverythingWorks

# tester votre installation
forge test
```

Vous devriez obtenir un résultat semblable à ceci:

```solidity

[⠆] Compiling...
[⠘] Compiling 1 files with 0.8.19
[⠃] Solc 0.8.19 finished in 457.65ms
Compiler run successful

Running 1 test for test/Everythingworks.t.sol:EverythingWorksTest
[PASS] testIncrement() (gas: 302)
Test result: ok. 1 passed; 0 failed; finished in 3.47ms
```

**Que sont exactement "forge" et "foundry"?**

Ils peuvent être assimilés à gulp ou webpack pour JavaScript ou maven pour Java ou tox pour Python. Foundry est une boite à outils (Framework) de développement pour rendre plus facile les tests, développements et déploiements. C'est sans aucun doute la plus connue en 2023, et absolument nécessaire à connaitre en tant que développeur Solidity.

Une chose vraiment géniale à propos de Foundry est que vous pouvez écrire vos tests unitaires en Solidity, ce qui rend la réalisation des tests plus facile. Les outils précédents utilisaient JavaScript, ce qui contraignait au changement de contexte entre langages et rendaient la conversion de types ou "casting" assez délicate.

**VS Code Extension**

Si vous n'avez pas déjà installé l'extension suivante, vous devriez le faire.
![Extension Solidity pour VS Code](https://github.com/user-attachments/assets/31d6b1bc-4fcd-4205-b722-ccce31551772)
