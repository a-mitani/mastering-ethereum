## 単純なネットワークモニターを作ってみる

### simple-eth-monitor
MeteorによるDapp開発の第一歩として、Ethereumのノードに接続しEthereumのネットワークの状態をモニターする単純なアプリケーション「simple-eth-monitor」を作るところから始めましょう。本節で説明するアプリケーションのソースコードはGitHub上に公開しています。

今回作成するsimple-eth-monitorはブラウザ上から特定のEthereumノードに接続し、下図の画面のような情報を表示する単純なモニターの役割を果たすものです。また、XXXとXXXの項目については、ユーザーがブラウザ上で再読み込み等の特別な操作をすることなくEthereumネットワーク上での最新の情報が自動的に更新されていくリアクティブな動きをします。

<img src="00_img/simple-eth-monnitor-final.png" width="500">

### gethの起動（RPCの有効化）
まず下準備として、simple-eth-monitorからの接続を受けるように下記のコマンドでgethを起動しておきます。ここではネットワークIDが10のテストネットに接続しています。本格的にDappを公開するまではテストネットにて動作を確認するほうが良いでしょう。

``` bash
$ geth --networkid "10" --datadir "/home/test_u/eth_data" --logfile "/home/test_u/eth_data/geth_01.log" --mine --unlock 0xa7653f153f9ead98dc3be08abfc5314f596f97c6 --rpc --rpcaddr "192.168.5.6" --rpcport "8545" --rpccorsdomain "*" --olympic console 2>> /home/test_u/eth_testnet_1201/geth_e01.log
```

上記コマンドは幾つか新しいコマンドオプションを追加しています。今回作成するDappとノードの連携はGethのRPC（Remote Procedure Call）のAPI機能を利用するのでその設定をコマンドオプションで行っています。
* `--rpc`：gethのRPCサーバとしてのAPIを有効化します。
* `--rpcaddr "192.168.5.6"`:読者の環境に合わせてgethノードのIPアドレスを指定します。browser solidityとgethを同じPC上で利用するなら "127.0.0.1"か"localhost"を指定します。
* `--rpcport "8545"`： RCP APIのポート番号を指定します。（特に問題なければデフォルトの8545を指定すればよいです。）
* `--rpccorsdomain "*"`： クロスドメインアクセスを許可するドメイン。ここでは任意のドメインを許可しています。

また、以下のオプションも加えています。
* `--mine`：gethの起動と同時に採掘を開始するオプション
* `--unlock 0xa7653f153f9ead98dc3be08abfc5314f596f97c6"`: 指定されたアドレスのアカウントのロックを解除します。読者の環境に合わせて、coinbaseのアドレスを指定してください。（起動時にパスワードが求められます。）

###### ■■ TIP ■■
gethのconsoleを利用せずバックグラウンドでgethを起動しておくと便利です。その場合は
1. 上記コマンドの`console`のオプションを外して実行
2. プロンプト上でパスワードを聞かれたら、入力。
3. ［Ctrl］＋［Z］キーを押下しプロセスを一時停止
4. `bg`コマンドを実行し、プロセスをバックグラウンド実行に移行

の手順で行えばよいでしょう。

### Meteorプロジェクト作成と整備
Meteorをインストールした環境[^1]で適当なディレクトリに、今回作成するsimple-eth-monitorのMeteorプロジェクトを作成します。
``` bash
$ cd ~/eth-meteor-proj # 任意のディレクトリに移動
$ meteor create simple-eth-monitor # 新しいMeteorプロジェクトを作成
```
「Meteorを使ってみる」節と同様に、この初期状態のWebアプリで念のためアクセス可能かを確認してみます。上記コマンドを実行して新しく作成されたを`simple-eth-monitor`ディレクトリに移動して
``` bash
$ meteor
```
コマンドを実行します。実行するとしばらくしてコンソールに`=> App running at: http://localhost:3000/`と表示されるのでWebブラウザでアドレスに「http://localhost:3000/ 」と入力しアクセスします。すると下図のような（単純な）Webアプリケーションが表示されます。

<img src="00_img/myfirstapp.png" width="500">

起動して画面が表示されることが確認できたら、Meteorプロジェクトのフォルダ構成を整備します。

`meteor create`を実行して作成された`simple-eth-monitor`ディレクトリ直下の`main.html`、`main.js`、`main.css`を削除します。また新しく`client`ディレクトリを作成し、そのディレクトリ下に`main.html`、`main.js`ファイルを作成します。ここで`main.html`ファイルには下記のコードを記述、`main.js`は空のままにしておきます。


> main.js

``` html
<head>
  <title>Simple Ethereum Status Monitor</title>
</head>
<body>
  <h1> Hello, world!!</h1>
</body>
```
>**Tag**  Commit step_001 ⇒ View on GitHub

> **Note** 
> Meteorにはプロジェクト内のディレクトリ名には下記のルールがあります。
> * `server` ディレクトリ以下のファイルは、サーバサイドのみで実行されます。
> * `client` ディレクトリ以下のファイルはクライアントサイド（ex.ブラウザ上）のみで実行されます。
> * プロジェクト直下のファイル、および、上記以外のディレクトリ以下のファイルはサーバサイドとクライアントサイドの両方で実行されます。
>
>  また、静的なコンテンツ、例えば画像データやフォントデータ等は`public`ディレクトリに配置されるのが慣習です。

### パッケージの追加
Meteorには標準の機能以外の拡張機能をパッケージとしてインストールすることで様々な機能が追加可能です。ここではsimple-eth-monitorで必要なパッケージ
* **twbs:bootstrap**: CSSフレームワーク「bootstrap」のパッケージ。
* **ethereum:web3**：EthreumノードとRPC接続するためのライブラリが含まれるパッケージ。
* **ethereum:accounts**：ethereum:web3パッケージのラッパーパッケージで、Ethereumのアカウント関連の情報をmeteor上でリアクティブに取得可能にするパッケージ。
* **ethereum:blocks**：ethereum:web3パッケージのラッパーパッケージで、Ethereumのブロックチェーン関連の情報をmeteor上でリアクティブに取得可能にするパッケージ。



 *  
###脚注
[^1] gethが起動しているサーバと同じ環境でも構いませんし、別サーバでも構いません。ここではgethが起動しているサーバと同じサーバ上で作っていく前提で解説していきます。