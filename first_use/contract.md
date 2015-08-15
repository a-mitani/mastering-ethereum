## コントラクトを作成してみる

Ethereumは、分散アプリケーション・プラットフォームです。Ethereumにおいて分散アプリケーションは、単一のContract、または複数のContractが連携して実現されるものとなっています。この章では、最も単純なContractを作成し、それを動作させる手順を追うことで、Contractとは何か、どのようにContractを作成しデプロイするのかを見ていきます。

### Contractとは
先に述べたように、Ethereumには２つのタイプのアカウントが存在します。

一つはEOA（Externally Owned Account）です。我々ユーザーがそのアカウントを作成し、任意のタイミングでそのアカウントを通じてEthereumネットワークとのやり取りを行うものです。

そしてもう一つがContractです。JavaやPythonなどオブジェクト指向言語になじみがある人であればContractは、オブジェクト指向言語での「クラス」に似たものと考えるとイメージがつかみやすいかもしれません。それぞれのContractは、各自に内部状態(内部データベース）とメソッド（実行コード）を持っています。

「etherを送金してみる」節<!-- [REF] -->で見たように、EOAが、他のEOAを宛先としたトランザクションを生成することで、他のEOAに対してetherを送金することができました。同様にEOAがContractを宛先とするトランザクションを生成することも可能です。この場合、EOAを宛先とした場合と同様、Contractにetherが送金されますが、それと同時にContractのコードが実行されます。

Contractのコードに様々な動作をプログラムすることで、そのContractが名前管理やトークン管理のようなアプリケーションが実現できます。Contractの実行は採掘者によって行われ、実行結果は公開元帳であるブロックチェーンに書き込まれていき、特定の中央機関なくアプリケーションが動作していきます。

### Solidity
Contractのコードは、Ethereumネットワーク上では「Ethereum Virtual Machine Code」または略して「EVM Code」と呼ばれる、バイト・コードの形式で記述され処理されます。
このようなバイト・コードの形式は低水準の機械言語であって、人間にとっては可読性が悪く、また開発の生産性も悪いものとなっています。

幸いにもEthereumには、より可読性と生産性のたかい、また、Contractのコードを記述するために特化した高水準言語と、それを EVM Code に翻訳するためのコンパイラが、幾つか開発されています。

その代表的なものとして「Solidity」が挙げられます。Solidityは Java Script に似た構文をもつ言語です。

ここでは、ContractをこのSolidityを使って開発していくものとして、まずは、Solidityのコンパイラである「solc」を準備しましょう。

### Solidityコンパイラ（solc）の導入
まずは、Gethにsolcが導入されているかを確認するために、Gethのコンソール上で、Gethにリンクされているコンパイラのリストを表示する`eth.getCompilers()`コマンドを実行してみましょう。
solc等のコンパイラがリンクされていない場合は、以下のような結果になります。（Solcがリンクされていると結果に、`['Solidity' ]`と表示されます。）
```javascript
> eth.getCompilers()
['' ]
```
#### solcのインストール
Gethにsolcが導入されていないことが分かれば、まずはシステムへsolcのインストールを行います。solcのインストールは、Gethのコンソールから抜けてそれぞれのプラットフォーム（OS）のコンソール上で行います。

##### Ubuntuへのインストール
以下のコマンドを実行してください。

```bash
$ sudo add-apt-repository ppa:ethereum/ethereum
$ sudo apt-get update
$ sudo apt-get install solc
```
以下のコマンドでsolcのバージョン情報が表示されれば問題なくインストールされています。
```bash
$ solc --version
```
また、以下のコマンドを実行し、その結果に表示されるsolcへのパスをメモしておいてください。Gethとsolcをリンクさせるために、後ほど利用します。
```bash
$ which solc
```
##### Mac OS Xへのインストール
以下のコマンドを実行し、cpp-ethereumをインストールしてください[^1] 。Mac OS Xへのインストールには、[Homebrew](http://brew.sh/) が事前にインストールされている必要があります。
```bash
$ brew install cpp-ethereum
$ brew linkapps cpp-ethereum
```
以下のコマンドでsolcのバージョン情報が表示されれば問題なくインストールされています。
```bash
$ solc --version
```
また、以下のコマンドを実行し、その結果に表示されるsolcへのパスをメモしておいてください。Gethとsolcをリンクさせるために、後ほど利用します。
```bash
$ which solc
```
##### Windowsへのインストール
[このページ](https://github.com/ethereum/cpp-ethereum/wiki/Installing-clients)の手順を参考にcpp-ethereumをインストールしてください。
<!-- [TODO]もうすこし詳しく記述 -->

#### Gethへsolcをリンクする
solcのシステムのインストールが完了したら、次にGethとsolcのリンクを行います。これにより、Gethから直接solcによるコンパイルを行うことが可能になります。

Gethのコンソールを開き[^2]、以下のコマンドを実行します。ここでパラメーターの「path_to_your_solc」の部分は各人のsolcインストール時の`which`コマンドの実行結果に書き換えてください。

```bash
> admin.setSolc("path_to_your_solc")
```

以下にUbuntu環境での実行結果を表示します。
```bash
> admin.setSolc("/usr/bin/solc")
'solc v0.1.1
Solidity Compiler: /usr/bin/solc
'
```
確認として、Gethにリンクされているコンパイラのリストを表示してみましょう。下記ののようにSolidityが表示されれば、正しくリンクされています。
```bash
> eth.getCompilers()
['Solidity' ]
```

### Contractの作成から利用までの流れ
コンパイラの導入が完了し、最初のContractを作成する準備が整いました。Contractを作成し、それを利用する流れは次のようになります。
1. Contractの作成
    1. Contractコードのプログラミングをする。
    2. solcを使ってコードをコンパイルする。
2. Contractの登録 
    3. コンパイル結果を含んだContract生成トランザクションを、Ethereumネットワークに送信する。
    4. 採掘者がトランザクションをブロックチェーンに登録する（＝Contractをブロックチェーンに登録する）。この時、Contractのアドレスが発行される。
2. Contractの利用
    1. Contractのアドレスを宛先としたトランザクションを生成する。
    2. 採掘者はトランザクションをブロックチェーンに登録する。この時、ContractCodeを実行し、その結果の状態もブロックチェーンに登録する。

以上の手順を、最も単純なContractを用いて実際に行って行きましょう。

####Contractの作成
#####最も単純なContract（OneStringRegister）
最初のContractとして1つの文字列を登録・管理するContractを作成してみましょう。ある利用者が"Hello World! "という文字列を登録すると、他の利用者が登録情報を参照した時に、"Hello World!"が表示され、また別の利用者がその文字列を"Good morning Japan!"と更新すれば、他の利用者が登録情報を参照した時に"Hello World! "でなく、"Good moring Japan!"と表示されるものです。

非常にシンプルなものですが、このような機能もこれまでは何らかの中央で登録情報を管理する機関やシステムが必要でした。Ethereumはこのような本来中央管理が必要だったアプリケーションをP2Pのシステムで行う事を可能にするのです。

#####Contractのコード
さて、上記のようなContract（名前をOneStringRegisterと名付けます）は、Solidity言語を使って記述すると、以下のコードになります。
```javascript
contract OneStringRegister {
    string registeredString;
    function set(string x) {
        registeredString = x;
    }
    function get() constant returns (string retVal) {
        return registeredString;
    }
}
```
Solidityの言語仕様の詳細は後の「コントラクト・プログラミング言語：Solidity」の章で解説します。そのため、このコードの意味を全て把握する必要はありませんが、コードを眺めると大きく以下の特徴があることが見て取れると思います。

* Contractの名前は「OneStringRegister」であること。
* registeredStringという文字列型の変数が定義されており、この変数に登録文字列が格納されること。
* setとgetの２つの関数が定義されていること。
    * setという名前の関数は、引き渡されたパラメータの内容で、registeredString変数が更新すること。
    * getという名前の関数は、登録されているregisteredString変数の内容を返却すること。

#####solcによるコンパイル
Gethコンソール上で以下のコマンドを実行し上記のソースコードをsolcでコンパイルします。source変数に入れる文字列は上記のソースコードから改行を抜いた文字列を代入します[^3]。

```
> var source = "contract OneStringRegister { string registeredString; function set(string x) { registeredString = x; } function get() constant returns (string retVal) { return registeredString; } }"
> var sourceCompiled = eth.compile.solidity(source) //ソースファイルをコンパイル
```
これで、sourceCompiledという変数にコンパイルされたContract情報が格納されました。

####Contractの登録 
Contractコードをコンパイルが完了しましたが、これをEthereumネットワークに送信しブロックチェーンに登録されてはじめて他のユーザーがこのContractにアクセスすることができるようになります。

EOAからトランザクションを生成し送信することで、このContractをEthereumネットワークに送信できます。先ほどのコンパイルのコマンドに続いて、以下のコマンドを実行します。

```javascript
> var sourceCompiledContract = eth.contract(sourceCompiled.OneStringRegister.info.abiDefinition) //Contractオブジェクトの生成
> var contract = sourceCompiledContract.new({from:eth.accounts[0], data: sourceCompiled.OneStringRegister.code})
```
詳細は「コントラクト・プログラミング言語：Solidity」の章<!-- [REF] -->で解説しますが、上記のコマンドの1行目でContractのオブジェクトを生成し、2行目で、そのオブジェクト情報を含んだトランザクションをEthereumネットワークに送信しています。

採掘者はこのトランザクションを受信し、このContractを登録したブロックの採掘を行います。その際にこのContractのアドレスが付加されます。アドレスは、上記でトランザクションを送信した際の戻り値を格納した変数`cntract`に格納されています。

採掘者が採掘を終える前の`contract`の内容を表示してみると、
```
> contract
{
  address: undefined,
  transactionHash: '0xc393f9fa1a95bc9e6a5e7344e88d7fc8d13b3672433273fee71f3e6c633b3c9b'
}

```
のように、contractのアドレスが未定になっています。ここで、transactionHashは今回のトランザクションのIDです。しばらくして採掘が成功すると、
```

```

には、


一定時間が経過し採掘が完了した状態で

利用したContractGethコンソール上で以下のコマンドを実行し上記のソースコードをsolcでコンパイルします。source変数に入れる文字列は上記のソースコードから改行を抜いた文字列を代入します[^3]。

が、コンパイルのためには、ソースコードから改行を抜く必要[^3]があるため、以下のように整形します。
```
contract OneStringRegister { string registeredString; function set(string x) { registeredString = x; } function get() constant returns (string retVal) { return registeredString; } }```



#### 脚注
[^1] cpp-ethereumはC++で実装されたEthereumのフル・クライアントであり、その一部にsolcが含まれています。

[^2] コマンドは[テスト・ネットへ接続してみる]章を参照してください。

[^3]javascriptの文字列変数に格納するための制限に起因するものです。