## 基本的な記法

### Hello world
下記にSolidityで記述された最も単純なスマート・コントラクトのコードの例（HelloWorld)を示します。

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
ここで「コントラクトの内容」の部分はContractの具体的な内容が記述されます。HelloWorldの例では`get()`関数が定義され、その中では文字列`Hello World`を返す処理が定義されていました。

1つのソースファイル上に複数のContractを定義することも可能です。

### 小文字・大文字の区別
Solidityでは、Contract名や関数名、変数名などは大文字と小文字が区別されます。例えば上記の例のコントラクトHelloWorldは「helloworld」の名前で呼ぶことはできません。「HelloWorld」と「helloworld」は別のものと解釈されます。

### 文（Statement）最後にはセミコロンをつける
Solidityで記述されたコードは一般的に１つ以上の文（Statement）から構成されます。例えば上記のコードの例では、`return "Hello World!!";`は1つの文であり、Solidityでは文の最後にセミコロンを付ける必要があります。

### コメント
Solidityでは他のプログラミング言語と同様Solidityでも「コメント」を付加することが可能です。コメントを記述するには以下の２つの方法があります。

###### 「//」でのコメント
単一行のコメントを記述する際に用います。「//」を付加することで、その後の部分がコメントとみなされます。

###### 「/\* ～ */」でのコメント
複数でまたがるコメントを記述する際に用います。 「/\*」と「*/」で囲まれたブロックがコメントとみなされます。

<!-- [TODO] ///のNATSPECについて記述 -->

