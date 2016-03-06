## 単純なネットワークモニターを作ってみる

MeteorによるDapp開発の第一歩として、Ethereumのノードに接続しEthereumのネットワークの状態をモニターする単純なアプリケーション（simple-eth-monitor）を作るところから始めましょう。本節で説明するアプリケーションのソースコードはGitHub上に公開しています。

### simple-eth-monitor
ここでは、ブラウザ上から特定のEthereumノードに接続し、下図の画面のような情報を表示する単純なモニターの役割を果たすアプリケーション（simple-eth-monitor）を作成します。
また、XXXとXXXの項目については、ユーザーがブラウザ上で再読み込み等の特別な操作をすることなくEthereumネットワーク上での最新の情報が自動的に更新されていくリアクティブな動きをします。

<img src="00_img/simple-eth-monnitor-final.png" width="500">

ここで下準備として、今回作るモニターが接続する先のEthereumノードをアプリケーションからの接続を受けるように下記のコマンドでgethを起動しておきます。
``` bash
$ geth --networkid "1201" --datadir "/home/mitani/eth_testnet_1201" --logfile "/home/mitani/eth_testnet_1201/geth_01.log" --mine --unlock 0xa7595f153f9ead98dc3ad08abfc5314f596f97e7 --rpc --rpcaddr "160.16.80.199" --rpcport "8545" --rpccorsdomain "*"  --olympic 2>> /home/mitani/eth_testnet_1201/^Cth_e01.log

```


