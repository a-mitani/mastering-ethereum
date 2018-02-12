## Contractを作成する

Ethereumは、分散アプリケーション・プラットフォームです。Ethereumにおいて分散アプリケーションは、単一のContract、または複数のContractが連携して実現されるものとなっています。この章では最も単純なContractを作成し、それを動作させる手順を追うことで、Contractとは何か、どのようにContractを作成しデプロイするのか、そしてどのようにContractを利用するのかを見ていきます。

### Contractとは
先に述べたように、Ethereumには２つのタイプのアカウントが存在します。

一つはEOA（Externally Owned Account）です。我々ユーザーがそのアカウントを作成し、任意のタイミングでそのアカウントを通じてEthereumネットワークとのやり取りを行うものです。

そしてもう一つがContractです。JavaやPythonなどオブジェクト指向言語になじみがある人であればContractは、オブジェクト指向言語での「クラス」に似たものと考えるとイメージがつかみやすいかもしれません。それぞれのContractは、各自にクラス変数に相当するような、内部状態を保持するストレージ部分と、メソッドに相当するような、実行コードである「コントラクト・コード」を持っています。

「etherを送金してみる」節<!--[REF]-->で見たように、EOAが、他のEOAのアドレスを宛先としたトランザクションを生成することで、他のEOAに対してetherを送金することができました。同様にEOAがContractのアドレスを宛先とするトランザクションを生成することも可能です。この場合、EOAを宛先とした場合と同様にContractへのetherの送金も可能ですが、同時にコントラクト・コードの実行を指示することが可能です。

コントラクト・コードに様々な動作をプログラムすることで、そのContractにより名前管理やトークン管理のようなアプリケーションが実現できます。コントラクト・コードの実行は採掘者によって行われ、実行結果は公開元帳であるブロックチェーンに書き込まれていき、特定の中央機関なくアプリケーションが動作していきます。

### Solidity
コントラクト・コードは、Ethereumネットワーク上では「Ethereum Virtual Machine Code」または略して「EVM Code」と呼ばれる、バイトコードの形式で記述され処理されます。
このようなバイトコードの形式は低水準の機械言語であって、人間にとっては可読性が悪く、また開発の生産性も悪いものとなっています。

そこでEthereumでは、可読性と生産性が高く、コントラクト・コードを記述することに特化した高水準言語と、それを EVM Code に翻訳するためのコンパイラが幾つか開発されています。

その代表的なものとして「Solidity」が挙げられます。Solidityは Java Script に似た構文をもつ言語です。

ここでは、ContractをこのSolidityを使って開発していくものとして、まずは、Solidityのコンパイラである「solc」を準備しましょう。

### Solidityコンパイラ（solc）の導入
まずは、Gethにsolcが導入されているかを確認するために、Gethのコンソール上で、Gethにリンクされているコンパイラのリストを表示する`eth.getCompilers()`コマンドを実行してみましょう。
solc等のコンパイラがリンクされていない場合は、以下のような結果になります。（Solcがリンクされていると結果に`['Solidity' ]`と表示されます。）
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
また、以下のコマンドを実行し、その結果に表示されるsolcへのパスをメモしておいてください。Gethとsolcをリンクさせるために後ほど利用します。
```bash
$ which solc
```
##### Mac OS Xへのインストール
以下のコマンドを実行し、cpp-ethereumをインストールしてください[^1] 。Mac OS Xへのインストールには、[Homebrew](http://brew.sh/) が事前にインストールされている必要があります。
```bash
$ brew install cpp-ethereum
$ brew linkapps cpp-ethereum
$ brew install solidity
```
以下のコマンドでsolcのバージョン情報が表示されれば問題なくインストールされています。
```bash
$ solc --version
```
また、以下のコマンドを実行し、その結果に表示されるsolcへのパスをメモしておいてください。Gethとsolcをリンクさせるために後ほど利用します。
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
確認として、Gethにリンクされているコンパイラのリストを表示してみましょう。下記のようにSolidityが表示されれば、正しくリンクされています。
```bash
> eth.getCompilers()
['Solidity' ]
```

### Contractの作成から利用までの流れ
コンパイラの導入が完了し、最初のContractを作成する準備が整いました。Contractを作成し、それにアクセスして利用するまでの流れは次のようになります。
1. Contractの作成
    * Solidity言語でコントラクト・コードをプログラミングする。そしてプログラムしたコントラクト・コードを、solcを使ってコードをコンパイルする。
2. Contractのブロックチェーンへの登録
    * コンパイル結果を用いて、Contract を生成するトランザクションをEthereumネットワークに送信する。そのトランザクションを受信した採掘者は、トランザクションをブロックチェーンに登録する（＝Contractをブロックチェーンに登録する）。この時Contractのアドレスが発行される。
2. Contractへアクセス
    * Contractを利用するユーザーは、Contract作成者からContractへのアクセス情報の取得する。その情報をもとにContractへアクセスし、コントラクト・コードの実行等を行う。

以上の手順を、最も単純なContractを用いて実際に行って行きましょう。

####Contractの作成
#####最も単純なContract（OneNumRegister）
<!--最初のContractとして1つの文字列を登録・管理するContractを作成してみましょう。ある利用者が"Hello World! "という文字列を登録すると、他の利用者が登録情報を参照した時に、"Hello World!"が表示され、また別の利用者がその文字列を"Good morning Japan!"と更新すれば、他の利用者が登録情報を参照した時に"Hello World! "でなく、"Good moring Japan!"と表示されるものです。
-->
最初のContractとして１つの整数値を登録・管理するContractを作成してみましょう。ある利用者が「3」を登録すると、他の利用者が登録情報を参照した時には「3」が表示され、また別の利用者がその整数値を「10」と更新すれば、他の利用者が登録情報を参照した時には新しい登録内容の「10」が表示されるものです[^3]。

非常にシンプルな機能ですが、このようなものも以前までは何らかの登録情報を管理する中央機関やシステムが必要でした。従来ではデータの整合性を保つために中央管理が必要だったアプリケーションをEthreumはP2Pのシステムで行う事を可能にするのです。

#####Contractのコード
さて、上記のようなContract（SingleNumRegisterと名付けます）は、Solidity言語を使って記述すると以下のコードになります。
```javascript
contract SingleNumRegister {
    uint storedData;
    function set(uint x) {
        storedData = x;
    }
    function get() constant returns (uint retVal) {
        return storedData;
    }
}
```
Solidityの言語仕様の詳細は後の「コントラクト・プログラミング言語：Solidity」の章<!--[REF]-->で解説します。そのため、今ここで、このコードの意味を全て把握する必要はありません。しかしコードを眺めると大きく以下の特徴があることが見て取れると思います。

* Contractの名前はcontractの宣言で規定されること。
* storedDataというuint型の変数が定義されており、この変数に登録数値が格納されること。
* setとgetの２つの関数が定義されていること。
    * setという名前の関数は、引き渡されたパラメータの内容で、storedData変数が更新すること。
    * getという名前の関数は、登録されているstoredData変数の内容を返却すること。

#####solcによるコンパイル
Gethコンソール上で以下のコマンドを実行し上記のソースコードをsolcでコンパイルします。source変数に入れる文字列は上記のソースコードから改行を抜いた文字列を代入します[^4]。

```
> var source = "contract SingleNumRegister { uint storedData; function set(uint x) { storedData = x; } function get() constant returns (uint retVal) { return storedData; }}"
> var sourceCompiled = eth.compile.solidity(source)//ソースファイルをコンパイル
```
これで、sourceCompiledという変数にコンパイル済みのContract情報が格納されました。

#### Contractのブロックチェーンへの登録
コントラクト・コードのコンパイルは完了しましたが、コンパイルしたコードはContract作成者のノード上にあるだけで、まだEthereumネットワーク内の誰もこのContractにアクセスできません。このコンパイル済みコードをEthereumネットワークに送信し、採掘者によってブロックチェーンに登録してもらって初めて他のユーザーがこのContractにアクセス出来るようになります。

EOAからトランザクションを生成し送信することで、作成したContractをEthereumネットワークに送信できます。先ほどのコンパイルのコマンドに続いて、以下のコマンドを実行します。

```javascript
> var contractAbiDefinition = sourceCompiled.SingleNumRegister.info.abiDefinition
> var sourceCompiledContract = eth.contract(contractAbiDefinition)
> var contract = sourceCompiledContract.new({from:eth.accounts[0], data: sourceCompiled.SingleNumRegister.code})
```

詳細は「コントラクト・プログラミング言語：Solidity」の章<!-- [REF] -->で解説しますが、上記のコマンドの1行目でContractのオブジェクトを生成し、2行目で、そのオブジェクト情報を含んだトランザクションをEthereumネットワークに送信しています。

採掘者はこのトランザクションを受信し、このContractを登録したブロックの採掘を行います。その際にContractのアドレスが付加されます。アドレスは、上記でトランザクションを送信した際の戻り値を格納した変数`contract`に格納されます。

採掘者が採掘を終える前の`contract`の内容を表示してみると、
```
> contract
{
  address: undefined,
  transactionHash: '0xeb76caefdfe5a9aa10b11743d317cf15f881d3b2e52ba3251dcf8e0718ed5b33'
}
```
のように、contractのアドレスが未定になっています。ここで、transactionHashは今回のトランザクションのIDです。しばらくして採掘が成功すると、
```
> contract
{
  address: '0x8ea277dfe4195daf7b8c101d79da35d1eb4c4aeb',
  transactionHash: '0xeb76caefdfe5a9aa10b11743d317cf15f881d3b2e52ba3251dcf8e0718ed5b33',
  allEvents: function (),
  get: function (),
  set: function ()
}
```
のように、contractのアドレス（`'0x8ea277dfe4195daf7b8c101d79da35d1eb4c4aeb'`）が付加されています。これで、作成したContractがEthereumネットワーク上にアドレス`'0x8ea277dfe4195daf7b8c101d79da35d1eb4c4aeb'`のContractとして登録されたことになります。

### Contractにアクセス
登録されたContractにアクセスしてみましょう。今回作成したContractは
1つの整数値を任意のユーザーが登録・更新できるものでした。作成者であるあなたは、このContractを他のユーザーにも利用してもらいたいと考えたとき、どのようにすればよいのでしょうか。

他のユーザーに自身の作成したContractを利用してもらうためには、以下の2種類の情報を他のユーザーに伝える必要があります。

###### Contractのアドレス：
ContractにアクセスするためにそのContractのアドレスが必要となります。今回のContractでは'0x8ea277dfe4195daf7b8c101d79da35d1eb4c4aeb'が付加されていました。
######ContractのABI (Application Binary Interface) ：
ABIとはContractの取り扱い説明書のようなものです。例えば、このContractがどのような名前の関数が定義されているか、それぞれの関数にアクセスするために、どのような型のパラメータを渡す必要があるか、関数の実行結果はどのような型のデータが返るか、などの情報が含まれたものです。今回のContractでは、Contract登録時に`contractAbiDefinition`の変数に格納した情報です。実際にcontractAbiDefinitionの内容を表示してみると以下のとおりです[^5]。Contract内に定義された関数の引数や戻り値などの情報が記述されているのが見て取れます。
```
> contractAbiDefinition
[{
  constant: false,
  inputs: [{name: 'x', type: 'uint256'}],
  name: 'set',
  outputs: [ ],
  type: 'function'
}, {
  constant: true,
  inputs: [ ],
  name: 'get',
  outputs: [{name: 'retVal', type: 'uint256'} ],
  type: 'function'
} ]

```

この、ContractのアドレスとABIの２つの情報があれば、他のユーザーは、あなたの作ったContractにアクセスができます。Contractへアクセスするオブジェクトは以下の書式で生成できます。
```
eth.contract(ABI_DEF).at(ADDRESS);
```
ここで、`ABI_DEF`、`ADDRESS`を今回のContractのものに置きかえ、変数`cnt`に代入します。ABIは改行を取り除いたものを入れます。

```
var cnt = eth.contract([{ constant: false, inputs: [{ name: 'x', type: 'uint256' } ], name: 'set', outputs: [ ], type: 'function' }, { constant: true, inputs: [ ], name: 'get', outputs: [{ name: 'retVal', type: 'uint256' } ], type: 'function' } ]).at('0x8ea277dfe4195daf7b8c101d79da35d1eb4c4aeb');
```
このオブジェクト`cnt`を用いてContractにアクセスをします。Contractの状態を変更する場合、つまり今回のContractでset関数でContractに登録された整数値を変更する場合は、トランザクションを生成することでアクセスします。このトランザクションは採掘者によりブロックチェーンに登録されることで、トランザクションの発生と、それによるContractの状態の変化についてEthereumネットワーク内で合意形成されることになります。

Contractの登録値を「6」に変更するトランザクションは以下のコマンドで送信できます。ここで`set`はコントラクト・コード内で定義した登録値を更新する関数の関数名です。このコマンドを実行した際の戻り値はトランザクションIDです。
```
> cnt.set.sendTransaction(6,{from:eth.accounts[0]})
'0x979c4e413a647673632d74a6c8b7f5b25a3260f3fefa4abea2dc265d61215939'
```
このトランザクションとその結果の状態が採掘者によりブロックチェーンに登録されると、Contractの登録整数値は「6」に更新されます。
<!-- [TODO]採掘されたかはトランザクションの情報のブロックのところ見ればよい旨記述する。 -->

では、このContractに登録されている最新の登録整数値を参照する場合はどうすればよいでしょうか？Contractの状態を参照する際は、状態を変更しそれをEthereumネットワーク内で同意を形成する必要はないため、トランザクションを送信する必要はありません。下記のように参照の関数（今回は`get`）を単純に呼び出すことで参照が可能です。
```
> cnt.get()
'6'
```

ここまで、Ethereumで行う「採掘」「送金」「Contractの生成と利用」についての大まかな手順を見てきました。これまでは自分個人だけが参加するテスト・ネットでこれらの機能を確認してきました。次の節では、多数の参加者が参加している、本番のEthereumネットワーク（ライブ・ネット）に接続して行きます。


#### 脚注
[^1]: cpp-ethereumはC++で実装されたEthereumのフル・クライアントであり、その一部にsolcが含まれています。

[^2]: コマンドは「テスト・ネットに接続してみる」節<!--[REF]-->を参照してください。

[^3]: あまり役に立たないコントラクトですが・・・

[^4]: javascriptの文字列変数に格納するための制限に起因するものです。

[^5]: 表示の都合上、一部整形しています。
