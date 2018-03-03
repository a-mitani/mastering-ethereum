## スマートコントラクトを作成し実行する

Ethereumは分散アプリケーション・プラットフォームです。Ethereumにおいて分散アプリケーションは、単一のスマートコントラクト、または複数のスマートコントラクトが連携して実現されるものとなっています。この章では最も単純なスマートコントラクトを作成し、それを動作させる手順を追うことで、スマートコントラクトとは何か、どのようにスマートコントラクトを作成しデプロイするのか、そしてどのようにスマートコントラクトを利用するのかを見ていきます。

### スマートコントラクトとは
先に述べたように、Ethereumには２つのタイプのアカウント、つまりEOA（Externally Owned Account）とContractが存在します。Ethereum上でスマートコントラクトの実態はContractアカウントです。

JavaやPythonなどオブジェクト指向言語になじみがある人であればContractアカウントは、オブジェクト指向言語での「クラス」に似たものと考えるとイメージがつかみやすいかもしれません。それぞれのContractは、各自にクラス変数に相当するような、内部状態を保持するストレージ部分と、メソッドに相当するような、実行コードである「コントラクト・コード」を持っています。

[「etherを送金する」](https://book.ethereum-jp.net/first_use/sending_ether.html)節で見たように、EOAが、他のEOAのアドレスを宛先としたトランザクションを生成することで、他のEOAに対してetherを送金することができました。同様にEOAがContractアカウントのアドレスを宛先とするトランザクションを生成することも可能です。この場合、EOAを宛先とした場合と同様にContractアカウントへのetherの送金も可能ですが、同時にコントラクト・コードの実行を指示することが可能です。

コントラクト・コードに任意の動作をプログラムすることで、独自通貨の発行や投票システムなどの様々なアプリケーションが実現できます。コントラクト・コードの実行は採掘者によって行われ、実行結果は公開元帳であるブロックチェーンに書き込まれていき、特定の中央機関なくアプリケーションが動作していきます。

### Solidity
コントラクト・コードは、Ethereumネットワーク上では「Ethereum Virtual Machine Code」または略して「EVM Code」と呼ばれる、バイトコードの形式で記述され処理されます。
このようなバイトコードの形式は低水準の機械言語であって、人間にとっては可読性が悪く、また開発の生産性も悪いものとなっています。

そこでEthereumでは、コントラクト・コードを記述することに特化した可読性の高い高水準言語と、それを EVM Code に翻訳するためのコンパイラが幾つか開発されています。

その代表的なものとして「Solidity」が挙げられます。Solidityは Java Script に似た構文をもつ言語です。

ここでは、ContractをこのSolidityを使って開発していくものとして、まずは、Solidityのコンパイラである「solc」を準備しましょう。

### Solidityコンパイラ（solc）のインストール
まず、システムへsolcのインストールを行います。solcのインストールは、Gethのコンソールから抜けてそれぞれのプラットフォーム（OS）のコンソール上で行います。

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
brew update
brew upgrade
brew tap ethereum/ethereum
brew install solidity
brew linkapps solidity
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
[このページ](http://solidity.readthedocs.io/en/latest/installing-solidity.html)の手順を参考にインストールしてください。
<!-- [TODO]もうすこし詳しく記述 -->


### スマートコントラクトの作成から実行までの流れ
コンパイラsolcのインストールが完了し、最初のスマートコントラクトを作成する準備が整いました。スマートコントラクトを作成し、それにアクセスし実行するまでの流れは次のようになります。
1. コントラクト・コードの作成
    * Solidity言語でスマートコントラクトの内容を記述したコントラクト・コードをプログラミングする。
    
２. コントラクト・コードのコンパイル
    * コントラクト・コードを、solcを使ってコンパイルする。
    
３. 「Contract」アカウントを生成
    * コンパイル済みのコードをトランザクションに付加してネットワークに送信する。そのトランザクションを受信した採掘者は、トランザクションをブロックチェーンに登録する（＝コントラクトをブロックチェーンに登録する）。これにより「Contract」アカウントが生成されそのアドレスが発行される。
    
４. スマートコントラクトへのアクセスと実行
    * スマートコントラクトを実行したいユーザーはContractアカウントへトランザクションを発行することによりスマートコントラクトを実行する。

以上の手順を、最も単純なContractを用いて実際に行って行きましょう。

####コントラクト・コードの作成
#####最も単純なスマートコントラクト（OneNumRegister）
<!--最初のContractとして1つの文字列を登録・管理するContractを作成してみましょう。ある利用者が"Hello World! "という文字列を登録すると、他の利用者が登録情報を参照した時に、"Hello World!"が表示され、また別の利用者がその文字列を"Good morning Japan!"と更新すれば、他の利用者が登録情報を参照した時に"Hello World! "でなく、"Good moring Japan!"と表示されるものです。
-->
最も簡単なスマートコントラクトとして、複数のユーザーで１つの整数値を登録・更新するスマートコントラクトを作成してみましょう。例えば、ある利用者が「3」を登録すると他の利用者が登録情報を参照した時には「3」が表示され、また別の利用者がその整数値を「10」と更新すれば、他の利用者が登録情報を参照した時には新しい登録内容の「10」が表示されるものです[^3]。

非常にシンプルな機能ですが、この登録内容とその更新の整合性を担保するために、通常は何らかの管理のための中央機関やシステムが必要です。しかしEthreumはブロックチェーンの特性を利用することで中央機関の存在しないP2Pのシステムでこの整合性を保つことが可能になるのです。

#####コントラクト・コード
さて、上記のようなContract（SingleNumRegisterと名付けます）は、Solidity言語を使って記述すると以下のコードになります。
```javascript
pragma solidity ^0.4.0;
contract SingleNumRegister {
    uint storedData;
    function set(uint x) public{
        storedData = x;
    }
    function get() public constant returns (uint retVal){
        return storedData;
    }
}
```
Solidityの言語仕様の詳細は後の「コントラクト・プログラミング言語：Solidity」の章<!--[REF]-->で解説します。そのため、今ここで、このコードの意味を全て把握する必要はありません。しかしコードを眺めると大きく以下の特徴があることが見て取れると思います。

* スマートコントラクトの名前は`contract`の宣言で規定されること。
* `storedData`という`uint`型の変数が定義されており、この変数に登録数値が格納されること。
* `set`と`get`の２つの関数が定義されていること。
    * setという名前の関数は、引き渡されたパラメータの内容で、storedData変数が更新すること。
    * getという名前の関数は、登録されているstoredData変数の内容を返却すること。

このコードを記載したファイルを適当なディレクトリ上で「SingleNumRegister.sol」の名前で保存します。

####コントラクト・コードのコンパイル
上記のソースコードを先にインストールしたsolcコマンドを用いて下記のようにコンパイルします。実行結果としてプロンプト上に`Binary`と`Contract JSON ABI`が表示されます [^4] 。

```
$ solc --abi --bin SingleNumRegister.sol　//ソースコードをコンパイル

======= SingleNumRegister.sol:SingleNumRegister =======
Binary:
6060604052341561000f57600080fd5b60d38061001d6000396000f3006060604052600436106049576000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806360fe47b114604e5780636d4ce63c14606e575b600080fd5b3415605857600080fd5b606c60048080359060200190919050506094565b005b3415607857600080fd5b607e609e565b6040518082815260200191505060405180910390f35b8060008190555050565b600080549050905600a165627a7a72305820bdd0549ef41e9c70cc944a6c19e54da47ecda63a1c5edfc7024125b5c49b4acb0029
Contract JSON ABI
[{"constant":false,"inputs":[{"name":"x","type":"uint256"}],"name":"set","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"get","outputs":[{"name":"retVal","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"}]


```

#### 「Contract」アカウントを生成
コントラクト・コードのコンパイルは完了しましたが、コンパイルしたコードはContract作成者のノード上にあるだけで、まだEthereumネットワーク内の誰もこのContractにアクセスできません。このコンパイル済みコードをEthereumネットワークに送信し、採掘者によってブロックチェーンに登録してもらって初めて他のユーザーがこのContractにアクセス出来るようになります。

EOAからトランザクションを生成し送信することで、作成したContractをEthereumネットワークに送信できます。gethを起動し、geth上のプロンプトでまず、先ほどのコンパイル結果を適当な変数に格納します。

```javascript

> var bin = "0x6060604052341561000f57600080fd5b60d38061001d6000396000f3006060604052600436106049576000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806360fe47b114604e5780636d4ce63c14606e575b600080fd5b3415605857600080fd5b606c60048080359060200190919050506094565b005b3415607857600080fd5b607e609e565b6040518082815260200191505060405180910390f35b8060008190555050565b600080549050905600a165627a7a72305820bdd0549ef41e9c70cc944a6c19e54da47ecda63a1c5edfc7024125b5c49b4acb0029"
> var abi = [{"constant":false,"inputs":[{"name":"x","type":"uint256"}],"name":"set","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"get","outputs":[{"name":"retVal","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"}]
```

さらに続けて、これらの変数を用いて下記のコマンドを実行します。
```javascript

> var contract = eth.contract(abi)
> var myContract = contract.new({ from: eth.accounts[0], data: bin})

```

詳細は「コントラクト・プログラミング言語：Solidity」の章<!-- [REF] -->で解説しますが、上記のコマンドの1行目でContractのオブジェクトを生成し、2行目で、そのオブジェクト情報を含んだトランザクションをEthereumネットワークに送信しています。そして、ここで`myContract`が「Contract」アカウントを示すオブジェクトになります。

採掘者はこのトランザクションを受信し、このContractを登録したブロックの採掘を行います。その際にContractのアドレスが付加されます。

採掘者が採掘を終える前の`myContract`の内容を表示してみると、
```
> myContract
{
  abi: [{
      constant: false,
      inputs: [{...}],
      name: "set",
      outputs: [],
      payable: false,
      stateMutability: "nonpayable",
      type: "function"
  }, {
      constant: true,
      inputs: [],
      name: "get",
      outputs: [{...}],
      payable: false,
      stateMutability: "view",
      type: "function"
  }],
  address: undefined,
  transactionHash: "0x902709c68fa91ddd19559301b97adb17ed602deb9f8f3a44f48efc2d667fb2aa",
  allEvents: function(),
  get: function(),
  set: function()
}
```
のように、myContractのアドレスが未定になっています。ここで、transactionHashは今回のトランザクションのIDです。しばらくして採掘が成功すると、
```
> myContract
{
  abi: [{
      constant: false,
      inputs: [{...}],
      name: "set",
      outputs: [],
      payable: false,
      stateMutability: "nonpayable",
      type: "function"
  }, {
      constant: true,
      inputs: [],
      name: "get",
      outputs: [{...}],
      payable: false,
      stateMutability: "view",
      type: "function"
  }],
  address: "0x7a28373a596a5e0dc7074aeec2c02e4f2413cf34",
  transactionHash: "0x902709c68fa91ddd19559301b97adb17ed602deb9f8f3a44f48efc2d667fb2aa",
  allEvents: function(),
  get: function(),
  set: function()
}
```
のように、contractのアドレス（`'0x7a28373a596a5e0dc7074aeec2c02e4f2413cf34'`）が付加されています。これで、作成したスマートコントラクトがEthereumネットワーク上にアドレス`'0x7a28373a596a5e0dc7074aeec2c02e4f2413cf34'`のContractアカウントとして登録されたことになります。

### 「Contract」アカウントへのアクセス
登録されたContractアカウントの情報を参照したり、トランザクションを発行することで、今回作成したスマートコントラクトを実行することが可能になります。今回のスマートコントラクト
スマートコントラクト1つの整数値を任意のユーザーが登録・更新できるものでした。作成者であるあなたは、このスマートコントラクトを他のユーザーにも利用してもらいたいと考えたとき、どのようにすればよいのでしょうか。

他のユーザーに自身の作成したスマートコントラクトを利用してもらうためには、以下の2種類の情報を他のユーザーに伝える必要があります。

###### Contractのアドレス：
スマートコントラクトにアクセスするために、その実態であるContractアカウントのアドレスが必要となります。今回のContractでは`'0x7a28373a596a5e0dc7074aeec2c02e4f2413cf34'`が付加されていました。
######ContractのABI (Application Binary Interface) ：
ABIとはContractの取り扱い説明書のようなものです。例えば、このContractがどのような名前の関数が定義されているか、それぞれの関数にアクセスするために、どのような型のパラメータを渡す必要があるか、関数の実行結果はどのような型のデータが返るか、などの情報が含まれたものです。今回のスマートコントラクトでは、Contractアカウント生成時に`myContract`の変数に格納された情報の中の`abi`属性です。実際に`myContract.abi`の内容を表示してみると以下のとおりです[^5]。Contract内に定義された関数の引数や戻り値などの情報が記述されているのが見て取れます。
```
> myContract.abi
[{
      constant: false,
      inputs: [{...}],
      name: "set",
      outputs: [],
      payable: false,
      stateMutability: "nonpayable",
      type: "function"
  }, {
      constant: true,
      inputs: [],
      name: "get",
      outputs: [{...}],
      payable: false,
      stateMutability: "view",
      type: "function"
  }]

```

この、ContractアカウントのアドレスとABIの２つの情報があれば、他のユーザーは、あなたの作ったスマートコントラクトにアクセスができます。Contractアカウントへアクセスするオブジェクトは以下の書式で生成できます。

```
eth.contract(ABI_DEF).at(ADDRESS);
```
ここで、`ABI_DEF`、`ADDRESS`を今回のContractのものに置きかえます。今回はそれぞれ変数`myContract.address`、`myContract.abi`に既に格納されているためそれを利用します。

```
vvar cnt = eth.contract(myContract.abi).at(myContract.address);

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

[^3]: あまり実用的ではないコントラクトですが・・・

[^5]: 表示の都合上、一部整形しています。



<br/>
-----
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />

