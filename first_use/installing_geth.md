## Gethのインストール

Ethereumを利用する場合、まずはEthereumのP2Pネットワークに参加する必要があります。ネットワークへの参加はEthereumクライアントをインストールし起動することで参加が可能になります。

現在Ethereumでは、Ethereumの仕様を実装した幾つかのEthereumクライアントが存在しますが[^1]、現在のところ推奨されているクライアントは「Geth」です。Gethはプログラミング言語[Go](http://golang.org/)により実装された、CUIクライアントであり、GethをインストールすることでEthereumネットワークにフル・ノードとして参加し、
* etherの採掘
* etherの送金
* スマート・コントラクトの生成
* トランザクションの生成
* ブロックチェーンの確認

といった動作が可能になります。

この章では、Linux（Ubuntu）、Windows、Mac、それぞれでのGethのインストール方法を解説します。

[^1] C++で実装された[cpp-ethereum](https://github.com/ethereum/cpp-ethereum)、Pythonで実装された[pyethereum](https://github.com/ethereum/pyethereum)、Javaで実装された[ethereumj](http://ethereumj.io/)などが存在します。

### Linux（Ubuntu）


