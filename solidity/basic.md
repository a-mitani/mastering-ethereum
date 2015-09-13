## 基本的な記法
### Contract

スマート・コントラクトを記述するための言語であるSolidityでは、`contract`命令で宣言されるコントラクトが基本であり、そのcontractの内容を記述していくことになります。1つの`contract`命令
The contract is the basic structure of Solidity. It is a prototype of an object which lives on the blockchain. A contract may be instantiated into a contract-account (or 'object', or sometimes just 'account') at which point it gets a uniquely identifying address with which is may be called. The address here is similar to a reference or pointer in C-like languages, or just a plain old object in Javascript. Like plain objects in many object-oriented languages, contracts can never run themselves - they may only be called, or, put another way, they can only react to the receipt of a message; they can never be proactive.

We denote a contract with the contract keyword and a name, followed by its definition enclosed in braces ({ & }). Here is the most basic contract:

contract MostBasicContract {
}
It doesn't do anything. To make it do something when it receives a message, we can introduce a function. The function you've already seen is the so-called default function. Why this is default will be properly explained later, but for now, suffice it to say that it is simply the function that gets called when this contract (well, an instantiation of it, anyway) receives a message from the Ethereum Environment.

The syntax of a function is similar to Javascript: it begins with the function keyword, then has a parenthesised parameter list, and finally has a braced expression block.

contract MostBasicContract {
    function() {
        // executed when a message is received.
    }
}
This expression block is very similar to other C-like languages including Javascript. Within our expression block, we are able to do arbitrary computation; for instance, we might try to determine what the sum of 1 and 1 is and place it into a variable called two:

contract Simple {
    function() {
        var two = 1 + 1;
    }
}
Unlike in Javascript, all variables must be declared prior to use and typed. However, for convenience, var is provided as a way to automatically determine the type through the expression it is initialised to. In this case, the type is uint, but more on the types later.