### ライブ・ネットに接続してみる

ここまでは、自分だけが参加する試験用のネットワーク（テスト・ネット）に接続してEthereumの動きをみてきました。ここでは、不特定多数のノードが参加する本番のネットワーク（ライブ・ネット）に接続していきます。

###ライブネットへの接続
以下のコマンドを実行してライブ・ネットに接続します。
```plain
$ geth --datadir "/home/mitani/eth_livenet" --logfile "/home/mitani/eth_livenet/geth_01.log" 2>> /home/mitani/eth_livenet/e01.log &
```
テスト・ネットに接続した際のコマンドとの違いは、