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

Gethのコンソールを開き[^2]、


#### 脚注
[^1] cpp-ethereumはC++で実装されたEthereumのフル・クライアントであり、その一部にsolcが含まれています。

[^2] 