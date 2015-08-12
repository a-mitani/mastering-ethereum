## アカウントの管理

### Gethの起動
Gethのインストールが完了したら早速Gethを起動しますが、事前に任意の場所にGethのデータ用のディレクトリを準備します。
ここでは、ログイン・ユーザー（ここではtest_u）のhomeディレクトリ直下に作成します。

```
$ mkdir /home/test_u/eth_data
```
データ用ディレクトリを作成したら、以下のコマンドでGethを起動します。
```
$ geth --networkid "10" --datadir "/home/test_u/eth_data" --logfile "/home/test_u/geth_01.log" console 2>> /home/test_u/geth_e01.log
```
ここで、各オプションの意味は以下のとおりです。
* --networkid ：
* 

Ethereumでトランザクションの処理を行うためにはトランザクション手数料を採掘者に支払う必要があります。
