### ライブ・ネットに接続してみる

ここまでは、自分だけが参加する試験用のネットワーク（テスト・ネット）に接続してEthereumの動きをみてきました。ここでは、不特定多数のノードが参加する本番のネットワーク（ライブ・ネット）に接続していきます。

### ライブ・ネットへの接続
ライブ・ネットに接続する際のデータ用ディレクトリを事前に用意しておきます。
```plain
$ mkdir /home/test_u/livenet_data
```
以下のコマンドを実行してライブ・ネットに接続します。
```bash
$ geth --datadir "/home/test_u/livenet_data" --logfile "/home/test_u/livenet_data/geth_01.log" 2>> /home/test_u/livenet_data/e01.log &
```
テスト・ネットに接続した際のコマンドとの違いは、`--networkid "10"` と `--olympic` のオプションを付加していないことになります。

### 接続状況を確認する
ライブ・ネットへ接続すると、Ethereumネットワーク内の他のノードと接続されます。Gethのコンソールを立上げ
```
```