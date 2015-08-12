## アカウントの管理

### Gethの起動
Gethのインストールが完了したら早速Gethを起動しますが、事前に任意の場所にGethのデータ用のディレクトリを準備します。
ここでは、ログイン・ユーザー（今回の例ではtest_u）のhomeディレクトリ直下に作成します。

```
$ mkdir /home/test_u/eth_data
```
データ用ディレクトリを作成したら、以下のコマンドでGethを起動します。
```
$ geth --networkid "10" --datadir "/home/test_u/eth_data" --logfile "/home/test_u/geth_01.log" console 2>> /home/test_u/geth_e01.log
```
ここで各オプションの意味は以下のとおりです。
* `--networkid "10"` ：`--networkid`オプションで任意の正の整数のIDを指定することで、個人用テストネットを立ち上げることが可能です（ここでは10を指定）。Frontier（PoC-9）から、Ethereumの本番ネットワークの稼働が開始されましたが、個人的な試験などを行うにはテストネットを立ち上げると非常に便利です。自分だけが参加者のネットワークなので、採掘なども非常に短時間で可能です。
* `--datadir "/home/test_u/eth_data"`：データ用ディレクトリとして、事前に作成しておいたディレクトリのパスを指定しています。データ用ディレクトリにブロックチェーン情報やノード情報など各種データが保存されます。
* `--logfile "/home/test_u/geth_01.log"`：ログファイルの指定。
* `console`：Gethには採掘やトランザクションの生成をインタラクティブに行うためのコンソールが用意されています。`console`サブ・コマンドを指定することで、Gethの起動時に同時にコンソール立ち上げることが可能です。なお、`console`サブ・コマンドを付加せず起動したGethプロセスに対して後からコンソールを起動する事も可能です（下記TIP参照）。

上記コマンドを実行すると、下記の実行結果のように、幾つかの情報の表示の後に「>」のプロンプトが表示され、コンソールが起動されます。次の節ではこのコンソール上で、Ethereumのアカウント（EOA）を生成します。

```
instance: Geth/v1.0.1/linux/go1.4.2
 datadir: /home/test_u/eth_testnet_10
coinbase: null
at block: 0 (1970-01-01 09:00:00)
 modules: admin:1.0 db:1.0 debug:1.0 eth:1.0 miner:1.0 net:1.0 personal:1.0 shh:1.0 txpool:1.0 web3:1.0
>

```


###### ■■ TIP ■■
今後Gethを使用していくなかで、採掘等のためにGethをバックグラウンドで常時起動しておき、必要に応じてそのGethプロセスに対してインタラクティブなコンソールで操作をしたいといった場合が発生します。その際は、下記のように`attach`サブ・コマンドを利用することで、既に起動されたGethプロセスのコンソールを起動することが可能です。

```
$ # gethプロセスをconsoleサブ・コマンドを付加せず、バックグラウンドで起動します。
$ # この場合、起動時にはコンソールは立ち上がりません。
$ geth --networkid "10" --datadir "/home/mitani/eth_testnet_10" --logfile "/home/mitani/eth_testnet_10/geth_01.log" 2>> /home/miti/eth_testnet_10/geth_e01.log &
$
$ # attachサブ・コマンドを用いて先に立ち上げたプロセスのコンソールを立ち上げます。
$ # ここで、ipc:以降に先に立ち上げたgethプロセスのデータ用ディレクトリ以下のgeth.ipcファイル（実際はソケット）のパスを指定します。
$ geth attach ipc:/home/mitani/eth_testnet_10/geth.ipc

```


### アカウントの作成
Gethのコンソール上で新規のアカウントを作成します。Ethereumには２種類のアカウントが存在します。一つはEOA（Externally Owned Account)、もう一つはContractです。

EOAは私たちユーザーによりコントロールされるアカウントであり、我々ユーザーによる任意のタイミングで、EOAがトランザクションを発生させ、他のアカウントへのetherの送金、コントラクト・コードの実行などを行います。また、etherの採掘もこのEOAアカウントにより行われます。

まずはEOAを新規作成してみましょう。
前節の手順に従い、Gethが起動されGethのコンソールが表示された状態を前提とします。

まず、このノードに登録されたアカウントを表示させてみましょう。`personal.listAccounts`コマンドは

```
> personal.listAccounts
[ ]
```

プロンプトに従い

多く発生する。バックグラウンドで起動差せておき、

の発生を"/home/mitani/eth_testnet_10"Geth起動時に

Ethereumでトランザクションの処理を行うためにはトランザクション手数料を採掘者に支払う必要があります。
