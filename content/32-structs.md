# Structs

Structs in Solidity behave similar to C. They group different variables within a single composite data type, which can be extremely useful in organizing data and creating more complex data structures.

Here’s how you declare a struct in Solidity.

```solidity

contract StructsExample {

    struct Foo {
        uint256 a;
        uint256 b;
    }
        
    Foo public myFoo;
}
```

**myFoo** is a public variable of struct Foo, it stores both **uint256 a** and **uint256 b**. As you can see if we deploy it in remix, myFoo returns:

![https://static.wixstatic.com/media/c0c19a_1bbba8b3b1784a26a19574db16a82851~mv2.png/v1/fill/w_560,h_542,al_c,q_95,enc_auto/myfoo.png](https://static.wixstatic.com/media/c0c19a_1bbba8b3b1784a26a19574db16a82851~mv2.png/v1/fill/w_560,h_542,al_c,q_95,enc_auto/myfoo.png)

To pass a struct in Remix to a function that takes a struct as an argument (we’ll talk more about this in a moment), encode it as follows:

![https://static.wixstatic.com/media/c0c19a_72d068d1006e4b898e7dc38ebd704984~mv2.png/v1/fill/w_554,h_412,al_c,q_95,enc_auto/Screenshot_2023-05-15_at_3_46_21_PM.png](https://static.wixstatic.com/media/c0c19a_72d068d1006e4b898e7dc38ebd704984~mv2.png/v1/fill/w_554,h_412,al_c,q_95,enc_auto/Screenshot_2023-05-15_at_3_46_21_PM.png)

The function in question takes the example Foo above, which consists of two uint256 variables. It might be a bit confusing to format it like an array, but that’s how it works.

To create a new instance of Foo in Solidity, simply wrap the values in struct Foo.

- Foo( a , b )

To access or assign each individual variable in the struct **myFoo** , use the dot notation.

- myFoo.a
- myFoo.b

Why do we use structs? Let’s say we have a deposit contract that keeps track of the depositor’s **name** and **balance**.

```solidity

contract DepositOnly {
    mapping(address => string) public name;
    mapping(address => uint256) public balance;

    function deposit(
        string memory _name
    ) 
        external 
        payable {
            balance[msg.sender] += msg.value;
            name[msg.sender] = _name;
    }
}

```

In the contract above, the depositor’s name and balance is stored into two separate **mapping** data structure.

The address variable in the mapping is repeated twice for both the name and balance of the same msg.sender, and hence it is not efficient.

So here’s where structs come in handy, we can register both the name and balance under a struct variable, and store that variable in one key value pair mapping like this.

```solidity

contract DepositOnly {
    struct Person {
        string name;
        uint256 balance;
    }
    mapping(address => Person) public depositor;

    function deposit(
        string memory _name
    ) 
        external 
        payable {
            depositor[msg.sender] = Person(_name, msg.value);
    }
}

```

See how useful it is? It makes your code cleaner and more efficient.

**How to use Structs**

Simple right? Here’s the demonstration.

```solidity

contract StructsExample {

    struct Foo {
        uint256 a;
        uint256 b;
    }
    
    Foo public myFoo;

    function assignMyFoo(
        uint256 _a, 
        uint256 _b
    ) 
        public {
            myFoo = Foo(_a, _b);
    }

    function assignA(
        uint256 _a
    ) 
        public {
            myFoo.a = _a;
    }

    function accessA() 
        public 
        view 
        returns(uint256) {
            return myFoo.a;
    }
    
}

```

If you want to pass struct Foo as an argument or as a return value, here are some rules you have to follow:

- Structs passed as arguments must be declared as a memory and
- Structs as return types must also be declared as a memory. Here’s how it looks.

```solidity

contract StructsExample {

    struct Foo {
        uint256 a;
        uint256 b;
    }

    Foo myFoo;

    function passStructAsArgument(
        Foo memory foo
    ) 
        public {
            myFoo = foo;
    }

    function returnAStruct() 
        public 
        view 
        returns (Foo memory) {
            return myFoo;
    }
}

```

It’s important to note that structs in Solidity **cannot** contain a member of its own type. For example, this is not allowed

```solidity

struct Foo {
    Foo innerFoo; // disallowed
}
```

**Arrays and mappings**

Structs can be used as the value type in arrays and mappings. For example, you could create a dynamic array of the Foo instance like this:

```solidity

contract StructsExample {

    struct Foo {
        uint256 a;
        uint256 b;
    }

    Foo[] public arrayFoo;

}
```

**arrayFoo** is an array composed of Foo instances.

To demonstrate:

```solidity

contract StructsExample {

    struct Foo {
        uint256 a;
        uint256 b;
    }

    Foo[] public arrayFoo;

    function addFooToArray(
        uint256 _a, 
        uint256 _b
    ) 
        public {
            arrayFoo.push(Foo(_a, _b));
    }

    function readFooFromArray(
        uint256 _index
    ) 
        public 
        view 
        returns(Foo memory){
            return arrayFoo[_index];
    }

    function readFooA(
        uint256 _index
    ) 
        public 
        view 
        returns(uint256){
            return arrayFoo[_index].a;
    }

    function modifyFooA(
        uint256 _index, 
        uint256 _a
    ) 
        public {
            arrayFoo[_index].a = _a;
    }

    function setFooAtIndex(
        uint256 _index, 
        uint256 _a, 
        uint256 _b
    ) 
        public {
            arrayFoo[_index] = Foo(_a, _b);
    }
}

```

You could also create a mapping where the keys are addresses and the values are Foo instances:

```solidity

contract StructsExample {
    struct Foo {
        uint256 a;
        uint256 b;
    }

    mapping(address => Foo) public mappingFoo;

    function insertFoo(
        uint256 _a, 
        uint256 _b
    ) 
        public {
            mappingFoo[msg.sender] = Foo(_a, _b);
    }
}

```

Up to this point, it should be obvious what’s going on here. We have a mapping of address ⇒ struct Foo; mappingFoo.

To assign a Foo instance to an address mapping, here’s how we do it

```solidity

function insertFoo(
    uint256 _a, 
    uint256 _b
) 
    public {
        mappingFoo[msg.sender] = Foo(_a, _b);
}

```

and to modify it

```solidity

function modifyFoo(uint256 _a) public {
    mappingFoo[msg.sender].a = _a;
}
```

**Real Life Example**

A more practical use case would be in a ticket system. We have a BuyTickets contract that sells one ticket at a price of 0.01 ether. An address can’t purchase more than 10 Tickets and we have a function that displays an address’ ticket information.

```solidity

contract BuyTickets {

    uint256 public constant TICKET_PRICE = 0.01 ether;

    struct Ticket {
        string name;
        uint256 numberOfTickets;
    }

    mapping(address => Ticket) public tickets;

    function buyTicket(
        string memory _name, 
        uint256 _numberOfTickets
    ) 
        external 
        payable {
            require(msg.value == _numberOfTickets * TICKET_PRICE, "Wrong Value");
            require(_numberOfTickets <= 10, "Maximum Limit Exceeded");
            require(tickets[msg.sender].numberOfTickets + _numberOfTickets <= 10, "Maximum Limit Reached");

            tickets[msg.sender].name = _name;
            tickets[msg.sender].numberOfTickets += _numberOfTickets;
    }

    function displayTicket(
        address _ticketHolder
    ) 
        external 
        view 
        returns (Ticket memory) {
            return(tickets[_ticketHolder]);
    }
}

```

We could use NFTs for tickets, but if users aren’t going to transfer tickets to each other, then that would be overkill.

**Exercises**

[StudentDB](https://github.com/RareSkills/Solidity-Exercises/tree/ddb9541ee6cad402e498bde51f06308350bfca7d/StudentDB)
