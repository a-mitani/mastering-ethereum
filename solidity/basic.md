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
このContractは、`get()`関数が呼び出されたら固定の"Hello World!!"という固定の文字列を返すというものです。

### Contract

上記のようにSolidityにおいて`contract`句で宣言されるContractが基本の構成要素であり、スマート・コントラクトは、この`contract`句に処理を記述していくことで実装されます。

Solidityでは次の構文でContractを定義します。
```plain
contract Contract名 {
   //スマート・コントラクトで行う処理をここに記述
}
```
ここで「・・・・」の部分にContractの具体的な内容が記述されます。ContractはJavaやPythonなどオブジェクト指向言語での「クラス」に似たものであり、クラス変数に相当するような内部状態を保持するストレージ部分やメソッドに相当するような関数、その中で有効なローカル変数などを持ちます。

HelloWorldの例では`get()`関数が定義され、その中では文字列`Hello World`を返す処理が定義されていました。

なお、1つのソースファイル上に複数のContractを定義することも可能です。

### 小文字・大文字の区別
Solidityでは、Contract名や関数名、変数名などは大文字と小文字が区別されます。例えば上述のContractのHelloWorldは「helloworld」の名前で呼ぶことはできません。「HelloWorld」と「helloworld」は別のものと解釈されます。

### 文（Statement）最後にはセミコロンをつける
Solidityで記述されたコードは一般的に１つ以上の文（Statement）から構成されます。例えば上記のコードの例では、`return "Hello World!!";`は1つの文であり、最後にセミコロンが付加されています。

### コメント
Solidityでは他のプログラミング言語と同様Solidityでも「コメント」を付加することが可能です。コメントを記述するには以下の２つの方法があります。

###### 「//」でのコメント
単一行のコメントを記述する際に用います。「//」を付加することで、その後の部分がコメントとみなされます。

###### 「/\* ～ */」でのコメント
複数でまたがるコメントを記述する際に用います。 「/\*」と「*/」で囲まれたブロックがコメントとみなされます。

<!-- [TODO] ///のNATSPECについて記述 -->

<!-- [TODO] 他のソースファイルからソースを呼びだすことができること -->

<!-- [TODO] 継承について書く -->
