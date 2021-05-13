# Gethをインストールする

Ethereumを利用する場合、まずはEthereumのP2Pネットワークに参加する必要があります。ネットワークへの参加はEthereumクライアントをインストールし起動することで参加が可能になります。

Ethereumでは、Ethereumの仕様を実装した幾つかのEthereumクライアントが存在しますが、現在のところ推奨されているクライアントは「Geth」です。Gethはプログラミング言語[Go](http://golang.org/)により実装されたCUIクライアントであり、GethをインストールすることでEthereumネットワークにフル・ノードとして参加し、

* etherの採掘
* etherの送金
* スマート・コントラクトの生成
* トランザクションの生成
* ブロックチェーンの確認

といった動作が可能になります。

本節では、Gethのインストール手順を解説します。

## UbuntuへのGethのインストール

Ubuntu OSを使用している場合、下記の一連のコマンドを実行するとでGethがインストールされます  。

```text
$ sudo add-apt-repository -y ppa:ethereum/ethereum
$ sudo apt-get update
$ sudo apt-get install ethereum
```

また、sudoコマンドの実行のためにパスワードが求められる場合があるので、その場合は適宜パスワードを入力します。

コマンド実行が完了した後、実際にgethがインストールされたかを確認するために、

```text
$ geth --help
```

のコマンドを実行してみましょう。gethコマンドのオプション情報が表示されれば、正しくインストールされています。

### Gethのアップデート

Ethereumの開発は現在Proof of Concept の第9フェーズであり、正式リリースではありません。そのため、クライアント・ソフトにおいても頻繁にアップデートが行われております。  
Gethをアップデートする際には`apt-get`コマンドにより、以下の手順で行います。

```text
$ sudo apt-get update
$ sudo apt-get upgrade
```

## Mac OS へのGethのインストール

Homebrewを用いたインストールとソースからインストールする方法があります。

### Homebrew を用いたGethのインストール

Homebrewをインストールがされてあれば、次のコマンドだけでインストールできます。

```text
$ brew tap ethereum/ethereum
$ brew install ethereum
```

### ソースからGethをビルドする

go-ethereumのレポジトリをクローン

```text
$ git clone https://github.com/ethereum/go-ethereum
```

そして、次のコマンドを実行することでGethをビルドできます。\(ビルドにはGoが必要になります\)

```text
$ cd go-ethereum
$ make geth
```

## Windows へのGethのインストール

Windows環境へのインストールはUnix系統のOSへのインストールと異なり、若干手順が煩雑です。Windows環境へのインストールは[こちら](https://github.com/ethereum/go-ethereum/wiki/Installation-instructions-for-Windows)に詳しく記載されているので、参考ください。

## 脚注

