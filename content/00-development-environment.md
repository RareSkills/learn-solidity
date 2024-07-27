# Development Environment

To get started, we will learn solidity as a language. We won’t begin with deploying contracts on the blockchain, that will just make things more complicated.

Head over to [remix.ethereum.org](https://remix.ethereum.org/)

You are strongly encouraged to use Remix to follow along with the examples in this course.

Let’s create a hello world.
    
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
    
After you go to remix.ethereum.org, right click contracts and left click “New File”

![https://static.wixstatic.com/media/c0c19a_489ef2f484f548cf888c995d79cd54ac~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/new%20file.png](https://static.wixstatic.com/media/c0c19a_489ef2f484f548cf888c995d79cd54ac~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/new%20file.png)

This is a solidity file, so give the file a .sol extension. The name is not important

![https://static.wixstatic.com/media/c0c19a_3f7aed5e540941efbf85e0c8983090f1~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/save%20file.png](https://static.wixstatic.com/media/c0c19a_3f7aed5e540941efbf85e0c8983090f1~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/save%20file.png)

Copy the code from above, or better yet, type it out yourself.

![https://static.wixstatic.com/media/c0c19a_ec23c5ddf49743b69cb70f0310903faf~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/add%20code.png](https://static.wixstatic.com/media/c0c19a_ec23c5ddf49743b69cb70f0310903faf~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/add%20code.png)

To compile the code, hit *Command S* on mac (*CTRL S* on Windows). If you see a red bubble above the solidity symbol, you have a syntax error. if you see orange, you only have warnings which you can ignore for now.

Now deploy the functions. Click the Ethereum symbol on the left, then click deploy.

![https://static.wixstatic.com/media/c0c19a_7cb41170d78c4a34bef83c67ddc3be36~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/deploy.png](https://static.wixstatic.com/media/c0c19a_7cb41170d78c4a34bef83c67ddc3be36~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/deploy.png)

To test the functions, scroll down on the left menu, then click on them. They will return the values you expect them to.

![https://static.wixstatic.com/media/c0c19a_cfb667754c4a4398bbdf27c7c4914a76~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/test.png](https://static.wixstatic.com/media/c0c19a_cfb667754c4a4398bbdf27c7c4914a76~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/test.png)

What if we want to make changes? Delete the contract with by clicking the trash icon.

![https://static.wixstatic.com/media/c0c19a_0767e7d240244ca2b4e14a9ea8c0b669~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/trash.png](https://static.wixstatic.com/media/c0c19a_0767e7d240244ca2b4e14a9ea8c0b669~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/trash.png)

Now change the code, recompile with *command S*, then click deploy. Test the functions again.

![https://static.wixstatic.com/media/c0c19a_074c5f0e3de540a09e1d2d32e9672f03~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/recompile.png](https://static.wixstatic.com/media/c0c19a_074c5f0e3de540a09e1d2d32e9672f03~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/recompile.png)

If a function requires an argument, it will be supplied next to the button.

![https://static.wixstatic.com/media/c0c19a_4776ae8f1f634e8ebb309e62ef0d8ebb~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/argument.png](https://static.wixstatic.com/media/c0c19a_4776ae8f1f634e8ebb309e62ef0d8ebb~mv2.png/v1/fill/w_939,h_496,al_c,q_95,enc_auto/argument.png)

You are now ready to experiment with Solidity smart contracts!
