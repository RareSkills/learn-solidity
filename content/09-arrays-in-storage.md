# Arrays in Storage

You may have noticed in our section about arrays we curiously left off

- writing to indexes in the array
- appending to an array
- popping from an array

This is because you very rarely do that to arrays that are supplied as function arguments.

However, when arrays are in storage, this operation is more common.

Here’s some example code

```solidity

contract ExampleContract {

    uint256[] public myArray;

    function setMyArray(
        uint256[] calldata newArray
    ) 
        public {
            myArray = newArray;
    }

    function addToArray(
        uint256 newItem
    ) 
        public {
            myArray.push(newItem);
    }

    function removeFromArray() 
        public {
            myArray.pop();
    }

    function getLength() 
        public 
        view 
        returns (uint256) {
            return myArray.length;
    }

    function getEntireArray() 
        public 
        view 
        returns (uint256[] memory) {
            return myArray;
    }
}

```

I recommend you copy and paste this code into [remix](https://www.notion.so/Storage-arrays-2ad2de7a813b45ec9ca83932dbbfa9f4?pvs=21) so you can gain an intuition for what is happening.

Call setArray with **[1,2,3,4,5,6]**

Now call **getLength()** it returns 6, which is the length of the array.

Now call **addToArray** with argument 10. Call **getLength()** again. Now it returns 7.

Call **removeFromArray()** followed by **getLength()**. It now returns 6 as you expect.

Because myArray is public, Remix shows it as visible as a function. But it will not return the entire array. It will ask for an index and return the value at that index. The myArray function behaves like this

It is worth noting that because myArray is public, Remix shows it as visible as a function. This means that the compiler will automatically generate a function called myArray() that can be called to read the values stored in myArray.

![https://static.wixstatic.com/media/61a666_7e7dc8b8051d40f0b27eef324637a121~mv2.png/v1/fill/w_620,h_749,al_c,q_95,enc_auto/https___s3-us-west-2_amazonaws_com_secure_notion-static_com_d1f0cfa4-c946-44de-b3a9-a67fe0.png](https://static.wixstatic.com/media/61a666_7e7dc8b8051d40f0b27eef324637a121~mv2.png/v1/fill/w_620,h_749,al_c,q_95,enc_auto/https___s3-us-west-2_amazonaws_com_secure_notion-static_com_d1f0cfa4-c946-44de-b3a9-a67fe0.png)

But it will not return the entire array. It will ask for an index and return the value at that index. The myArray function behaves like this

```solidity

function myArray(uint256 index) 
        public 
        view 
        returns (uint256) {
            return myArray[index];
}
```

However, the function **getEntireArray()** returns the entire array.

Note that **pop()** does not return the value.

**Removing an item**

Solidity does not have a way to remove an item in the middle of a list and reduce the length by one. The following code is valid, but it does not change the length of the list.

```solidity

contract ExampleContract {

    uint256[] public myArray;

    function removeAt(uint256 index) public {
        delete myArray[index];
        // sets the value at index to be zero

        // the following code is equivalent
        // myArray[index] = 0;

        // myArray.length does not change in either circumstance
    }
}
```

If you want to remove an item and also reduce the length, you must do a “pop and swap”.

It removes the element at the index argument and swaps it with the last element in the array

```solidity

contract ExampleContract {

    uint256[] public myArray;

    function popAndSwap(uint256 index) public {
        uint256 valueAtTheEnd = myArray[myArray.length - 1];
        myArray.pop(); // reduces the length;
        myArray[index] = valueAtTheEnd;
    }
}
```

Solidity cannot delete from the middle of the list and preserve the array’s original order.

**Strings**

Strings behave similarly to arrays, except when they are public they return the entire string, because strings cannot be indexed (confusing, isn’t it?). There is no pop or length operation for strings.

```solidity

contract ExampleContract {

    string public name;

    function setName(string calldata newName) 
            public {
        name = newName;
    }
}
```

**Practice Problems**

[ListOfNumbers](https://github.com/RareSkills/Solidity-Exercises/tree/main/ListOfNumbers)

[InsertInArray](https://github.com/RareSkills/Solidity-Exercises/tree/main/InsertInArray)

[Stack](https://github.com/RareSkills/Solidity-Exercises/tree/main/Stack)
