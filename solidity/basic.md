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
このContractは、呼び出されたら固定の"Hello World!!"という固定の文字列を返すというものです。

### Contract

上記のようにSolidityでは、`contract`命令で宣言されるContractが基本の構成要素であり、スマート・コントラクトの開発者は、このContractの処理の内容を記述していくことになります。

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
###### 「//」でのコメント
単一行のコメントを記述する際に用います。行頭に「//」を付加することで、その行の行末までがコメントとみなされます。

###### 「/\* ～ */」でのコメント
複数でまたがるコメントを記述する際に用います。 「/\*」と「*/」で囲まれたブロックがコメントとみなされます。

