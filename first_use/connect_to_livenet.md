### ライブ・ネットに接続してみる

2015年7月30日にFrontierがリリースされ、Ethereumは本番ネットワーク（ライブ・ネット）の運用が開始されました。このライブ・ネットで、今後 Ethereumのブロックチェーン等のインフラを利用した様々な分散アプリケーションが開発され、様々なサービス生まれていくことになります。

本書ではここまで、個人用のテスト用のネットワーク（テスト・ネット）に繋いで、Gethクライアントを実際に操作しながらEthereumの概観を説明してきました。この節では実際にライブ・ネットに接続していきます。

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
ライブ・ネットへ接続すると、Ethereumネットワーク内の他のノードと接続されます。Gethのコンソールを立上げ`net.peerCount`コマンドを実行すると、自分のノードが他のいくつのノードと接続されているかを表示することが出来ます。
```
> net.peerCount
25
```
また、実際に接続されているノードの情報は`admin.peers`のコマンドで確認することが出来ます。
```plain
> admin.peers
[{
  Caps: 'eth/60, eth/61',
  ID: '99017abe7031b48a855b8e79fecb6c927cda88229354f21184d343941ae78ee261d0ccb9f9999f620f96bd729b2cb7c4e8cdf3218d71b016fe531ff439b81dcc',
  LocalAddress: '160.16.80.199:34057',
  Name: 'Geth/v1.0.2/linux/go1.4.2',
  RemoteAddress: '192.169.7.150:30303'
}, 
（中略）
{
  Caps: 'eth/60, eth/61',
  ID: '6f8c6cc4a878ed88e03c7a0ee01386e095fbc1bfd48352c7bec05142cc60795317b5b8c5e9afc9863a414541df4d5b1627a86418177857166171fd45497c75ab',
  LocalAddress: '160.16.80.199:41617',
  Name: 'Geth/v1.0.2/linux/go1.4.2',
  RemoteAddress: '83.77.31.183:30303'
} ]
>
```