## 単純なネットワークモニターを作ってみる

MeteorによるDapp開発の第一歩として、Ethereumのノードに接続しEthereumのネットワークの状態をモニターする単純なアプリケーション（simple-eth-monitor）を作るところから始めましょう。本節で説明するアプリケーションのソースコードはGitHub上に公開しています。

### simple-eth-monitor
ここでは、ブラウザ上から特定のEthereumノードに接続し、下図の画面のような情報を表示する単純なモニターの役割を果たすアプリケーション（simple-eth-monitor）を作成します。
また、XXXとXXXの項目については、ユーザーがブラウザ上で再読み込み等の特別な操作をすることなくEthereumネットワーク上での最新の情報が自動的に更新されていくリアクティブな動きをします。

<img src="00_img/simple-eth-monnitor-final.png" width="500">

ここで下準備として、今回作るモニターが接続する先のEthereumノードをアプリケーションからの接続を受けるように下記のコマンドでgethを起動しておきます。ここではネットワークIDが10のテストネットに接続しています。本格的にDappを公開するまではテストネットにて動作を確認するほうが良いでしょう。

``` bash
$ geth --networkid "10" --datadir "/home/test_u/eth_data" --logfile "/home/test_u/eth_data/geth_01.log" --mine --unlock 0xa7653f153f9ead98dc3be08abfc5314f596f97c6 --rpc --rpcaddr "192.168.5.6" --rpcport "8545" --rpccorsdomain "*" --olympic console 2>> /home/test_u/eth_testnet_1201/geth_e01.log
```

ここで、幾つか新しいコマンドオプションが出てきました。browser solidityとノードの接続には、gethのRPC（Remote Procedure Call）のAPI機能を利用するのでその設定をコマンドオプションで行っています。
* `--rpc`：gethのRPCサーバとしてのAPIを有効化します。
* `--rpcaddr "192.168.5.6"`:読者の環境に合わせてgethノードのIPアドレスを指定します。browser solidityとgethを同じPC上で利用するなら "127.0.0.1"か"localhost"を指定します。
* `--rpcport "8545"`： RCP APIのポート番号を指定します。（特に問題なければデフォルトの8545を指定すればよいです。）
* `--rpccorsdomain "*"`： クロスドメインアクセスを許可するドメイン。ここでは任意のドメインを許可しています。

また、以下のオプションも加えています。
* `--mine`：gethの起動と同時に採掘を開始するオプション
* `--unlock 0xa7653f153f9ead98dc3be08abfc5314f596f97c6"`: 指定されたアドレスのアカウントのロックを解除します。読者の環境に合わせて、coinbaseのアドレスを指定してください。（起動時にパスワードが求められます。）

