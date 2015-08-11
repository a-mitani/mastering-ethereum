## Gethのインストール

Ethereumを利用する場合、まずはEthereumのP2Pネットワークに参加する必要があります。ネットワークへの参加はEthereumクライアントをインストールし起動することで参加が可能になります。

現在Ethereumでは、Ethereumの仕様を実装した幾つかのEthereumクライアントが存在しますが[^1]、現在のところ推奨されているクライアントは「Geth」です。Gethはプログラミング言語[Go](http://golang.org/)により実装された、CUIクライアントであり、GethをインストールすることでEthereumネットワークにフル・ノードとして参加し、
* etherの採掘
* etherの送金
* スマート・コントラクトの生成
* トランザクションの生成
* ブロックチェーンの確認

といった動作が可能になります。

### Ubuntu、Debian、Mac OS X へのGethのインストール

Mac OS X、Ubuntu、Debian のOSを使用している場合、Gethのインストールは極めて容易です。
コンソールを立ち上げ、
```
$ bash <(curl -L https://install-geth.ethereum.org)
```
のコマンドを実行するだけで、最新の安定バージョンのGethがインストールされます。

このコマンドは、指定されたURL（https://install-geth.ethereum.org） からGethのインストール用bashスクリプトをダウンロードし、それ実行するものです。bashスクリプトには、実行環境（OS）の判定、依存パッケージ有無の判定とインストール、Gethのインストールまでを自動で行うスクリプトが記述されているため、ほぼ自動のインストールが可能になっています。

以下に、Ubuntu（14.04 LTS）での上記コマンドの実行結果を記載します。


```
$ bash <(curl -L https://install-geth.ethereum.org)

↓↓↓ 以下コマンドの実行結果↓↓↓
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 10797  100 10797    0     0   9073      0  0:00:01  0:00:01 --:--:--  9073

 ∷  WELCOME TO THE FRONTIER  ∷
==> Looking for geth
    Geth is missing 
==> Checking dependencies
    apt-get
apt 1.0.1ubuntu2 for amd64 compiled on Aug  1 2015 19:20:48
Supported modules:
*Ver: Standard .deb
（中略）
==> This script will install:
==> Ethereum:\n -> /usr/bin/geth\n

==> Before installing Geth (ethereum CLI) read this:

 -> Frontier is a live testnet, it is not the 'main release' of Ethereum, but rather an initial beta pr                                                         erelease;
 -> You'd be mad to use this for anything approaching important or valuable. Expect dragons;
 -> If you're in any doubt, stand back and enjoy the show. It's so unstable, even Chuck Norris would ru                                                         n away and hide from it;
 -> We fully expect instability and consensus flaws in the client, some of which may be exploitable;

==> I understand, I want to install Geth (ethereum CLI) (Y/n) 

```

Ethereumはまだテスト段階であり、非常に不安定である旨の警告が表示され、それを理解したうえでインストールを望むか、と質問されるので、
「Y」とコンソールに入力すると、以下のように、実行環境に合わせたインストールが開始されます。
また、sudoコマンドの実行のためにパスワードが求められる場合があるので、その場合はパスワードを入力します。

```
==> Installing ethereum
==> Installing common software properties
$ sudo apt-get install -q -y software-properties-common
Reading package lists...
Building dependency tree...
Reading state information...

（中略）

==> Verifying installation
 ✓  Found geth: /usr/bin/geth
Geth
Version: 1.0.1
Protocol Versions: [61 60]
Network Id: 1
Go Version: go1.4.2
OS: linux
GOPATH=
GOROOT=/usr/lib/go

==> Installation successful!
==> Next steps
==> Run `geth help` to get started.
```

「Install successful!」のように、インストールが成功した旨が表示されれば、インストール完了です。

実際にgethがインストールされたかを確認するために、
```
$ geth help
```
のコマンドを実行してみましょう。
インストールされたgethのヘルプ


[^1] C++で実装された[cpp-ethereum](https://github.com/ethereum/cpp-ethereum)、Pythonで実装された[pyethereum](https://github.com/ethereum/pyethereum)、Javaで実装された[ethereumj](http://ethereumj.io/)などが存在します。

