## 基本的な記法

### Hello world
下記にSolidityで記述された最も簡単なスマート・コントラクトのコードの例（HelloWorld)を示します。

``` plain
// Simple contract that returns constant string "Hello World"
contract HelloWorld {
    function get() constant returns (string retVal) {
        return "Hello World!!";
   }
}
```
このContractは、呼び出されたら固定の"Hello World!!"という文字列を返すというものです。

### Contract

上記のようにSolidityでは、`contract`命令で宣言されるContractが基本の構成要素であり、スマート・コントラクトの開発者は、このContractの処理の内容を内容を記述していくことになります。

ContractはJavaやPythonなどオブジェクト指向言語での「クラス」に似たものであり、それぞれのContractは、各自にクラス変数に相当するような、内部状態を保持するストレージ部分と、メソッドに相当するような関数を持ちます。

Solidityでは次の構文でContractを定義します。
```plain
contract Contract名 {
   //コントラクトの内容
}
```
ここでコントラクトの内容の部分に様々な処理を書き加えていくことになります。HelloWorldの例では`get()`関数が定義され、その中では文字列`Hello World`を返す処理が定義されていました。

### コメント
Solidityでは他のプログラミング言語と同様Solidityでも「コメント」を付加することが可能です。コメントを記述するには以下の２つの方法があります。

| 記法 | 概要 |
| -----   | -- |
| // コメント |単一行のコメント。行頭に「//」を付加することで、その行の行末までがコメントとみなされます。 |
| /\* コメント*/ |単一行のコメント。行頭に「//」を付加することで、その行の行末までがコメントとみなされます。 |

、行頭に`//`を付加することで、その行をコメントとすることができます。コメントはSolidityコンパイラにより無視され処理は行われません。
複数にまたがるのように行頭に


1つの`contract`命令
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