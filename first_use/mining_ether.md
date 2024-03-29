# etherを採掘する

前節までで、Ethereumの代表的なクライアントであるGethをインストールし、テストネットへの接続、対話型のコンソールの立ち上げ方法を解説しました。Ethereumは内部通貨etherが規定されており、Ethereumでトランザクションを発生させるためにはetherの手数料が必要になります。この節ではetherを採掘する手順を見ていきます。

## アカウントの作成

Gethのコンソール上で新規のアカウントを作成します。Ethereumには２種類のアカウントが存在します。一つはEOA（Externally Owned Account\)、もう一つはContractです。

EOAは私たちユーザーによりコントロールされるアカウントであり、我々ユーザーによる任意のタイミングで、EOAがトランザクションを発生させ、他のアカウントへのetherの送金、コントラクト・コードの実行などを行います。また、etherの採掘もこのEOAアカウントにより行われます。

一方Contractは一種の自動エージェントであり、EOAにより発生したトランザクションをトリガにContractアカウントが内部に持つコントラクト・コードが実行されます。それによりContractのフィールドのデータも更新されます。オブジェクト指向言語に馴染みのある人には、「Contractは一種のクラスのようなもので、EOAによりContractのメソッドが呼ばれ、そのメソッドが実行されるとContractの持つクラス変数が書き換えられる」というアナロジーでの説明が分かりやすいかもしれません。

ここでは、EOAを新規に作成していきます。 前節の手順に従い、Gethが起動されコンソールが表示された状態を前提とします。

まず、このノードに登録されたアカウント（EOA）を表示させてみましょう。`eth.accounts`コマンドはこのノード内で作成されたEOAのリストを表示するものです。現時点ではアカウントを作成していないため、下記のように実行しても空のリストが表示されるのみです。

```text
> eth.accounts
[ ]
```

EOAの作成は`personal.newAccount("passwd")`コマンドで行います。ここでpasswdの部分は作成するEOAのパスワードです。実行時には適宜書き換えて実行してください。実行すると、作成されたEOAの20バイトのアドレスが表示されます。また、`personal.listAccounts`コマンドの実行結果にも作成したアカウントのアドレスが表示されるようになります。

```text
> personal.newAccount("hogehoge01")
'0x24afe6c0c64821349bc1bfa73110512b33fa18e1'

>eth.accounts
['0x24afe6c0c64821349bc1bfa73110512b33fa18e1']
```

ここで、もう一つアカウントを作成しましょう（後で使用します）。

```text
> personal.newAccount("hogehoge02")
'0x59c444d6c4f4187d1dd1875ad74a558a2a3e20b6'

> eth.accounts
['0x24afe6c0c64821349bc1bfa73110512b33fa18e1', '0x59c444d6c4f4187d1dd1875ad74a558a2a3e20b6' ]
```

【注意】パスワードを忘れると復元する手段は**ありません**。絶対にパスワードは忘れないようにしてください。また、上記の例では簡易なパスワードを用いましたが、実際には、セキュリティの観点から半角英数記号を含む長い複雑なパスワードを設定するようにしてください。

#### etherbase

ここで、`eth.coinbase`コマンドを実行してみます。すると下記のとおり実行結果には先ほど作成した2つのEOAのうちの一つが表示されます。このコマンドはetherbase（coinbaseとも呼ばれます）を表示するコマンドで、etherbaseとは、各ノードで採掘を行う際にその報酬を紐づけるEOAのアドレスを示します。

```text
> eth.coinbase
'0x24afe6c0c64821349bc1bfa73110512b33fa18e1'
```

etherbaseはデフォルトではプライマリーのアカウント（`eth.accounts[0]`コマンドを実行して表示されるアドレスのEOA）が設定されますが、下記のように`miner.setEtherbase(new_adress)`コマンドで変更することも可能です。

```text
> miner.setEtherbase(eth.accounts[1])
> eth.coinbase
'0x59c444d6c4f4187d1dd1875ad74a558a2a3e20b6'
```

## etherの採掘

作成したEOAのアドレスがetherbaseとしてセットされていれば、etherの採掘が可能です。 etherの採掘は`miner.start(thread_num)`コマンドで開始します。ここでthread\_num は採掘を何本のスレッドで同時実行するかを指定するパラメータです。指定しない場合は動作環境でのCPUコア数に設定されます。ここではthread\_numは指定せず、以下のコマンドで採掘を開始します。

```text
> miner.start()
null
```

また、採掘を停止したい場合は `miner.stop()`コマンドを実行すれば停止できます。

```text
> miner.stop()
true
```

### 採掘状況の確認

採掘を開始するとブロックが次々と生み出されていきます。ブロックチェーンに何番目のブロックまで連なっているのか（＝ブロック高）を確認するには、`eth.blockNumber`コマンドを用います。 採掘開始後しばらく すると、

```text
> eth.blockNumber
145
```

といったように、ブロックが採掘されていることが確認できます。今回の結果の場合、145個のブロックが採掘されています。（今は自分だけが参加しているテストネットで採掘を行っているので、この145個のブロックは全て自分が採掘したことになります。）

**■TIP■**

なかなか採掘に成功しない場合、実際にGethで採掘処理が行われているのか不安になることが多くあります。処理が行われているかを確認する方法として`eth.mining`コマンドで採掘中か否かを表示する方法があります。採掘中であればコマンドの実行結果として`true`が返り、そうでなければ`false`が返却されます。もう一つの方法は、`eth.hashrate`コマンドで、現在の採掘処理のハッシュ・レートを確認することです。ハッシュ・レートがゼロよりも大きければ、採掘処理が行われていると考えてよいでしょう。

```text
> miner.start()
null
> eth.mining
true
> eth.hashrate  //採掘処理実行時
445445
> miner.stop()
true
> eth.mining
false
> eth.hashrate  //採掘処理が行われていない場合、ハッシュ・レートは0となる。
0
```

### 採掘したブロックの内容を調べる

`eth.getBlock(number)`コマンドは、numberに任意のブロック高を指定すると、そのブロック高のブロック情報を表示することができます。 以下に、ブロック高が100と101のブロックの情報を表示してみます。

```text
> eth.getBlock(100)
{
  difficulty: '137447',
  extraData: '0x476574682f76312e302e312f6c696e75782f676f312e342e32',
  gasLimit: 3141592,
  gasUsed: 0,
  hash: '0x4d3063b91cbaa12bf2de81014c1319febc9f197c93f81b0746afaffaa9496620',
  logsBloom: '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000',
  miner: '0x24afe6c0c64821349bc1bfa73110512b33fa18e1',
  nonce: '0x28fda83cb19ed497',
  number: 100,
  parentHash: '0x5885cdec1d1410580eaaf1fb7ef9db245a735822d48e816c73d926b7c9872f15',
  sha3Uncles: '0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347',
  size: 536,
  stateRoot: '0xacf2c3dfc512373ae6d9693207b3ac43fd4811791fec994c2eecd8fdd3333699',
  timestamp: 1439451765,
  totalDifficulty: '13551548',
  transactions: [ ],
  transactionsRoot: '0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421',
  uncles: [ ]
}
> eth.getBlock(101)
{
  difficulty: '137514',
  extraData: '0x476574682f76312e302e312f6c696e75782f676f312e342e32',
  gasLimit: 3141592,
  gasUsed: 0,
  hash: '0xca9b241dabe753ed83d6242f226c0ad6b559c722edf5d24baff126670f70a30c',
  logsBloom: '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000',
  miner: '0x24afe6c0c64821349bc1bfa73110512b33fa18e1',
  nonce: '0x06024dfb81cc05ef',
  number: 101,
  parentHash: '0x4d3063b91cbaa12bf2de81014c1319febc9f197c93f81b0746afaffaa9496620',
  sha3Uncles: '0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347',
  size: 536,
  stateRoot: '0xbd43f9a44f2064c564060e585a23f7183036d83b411e14ad6b346de9d8dead02',
  timestamp: 1439451766,
  totalDifficulty: '13689062',
  transactions: [ ],
  transactionsRoot: '0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421',
  uncles: [ ]
}
```

各項目については、「Ethereumの内部」章で詳しく説明しますが、ここでは、`miner`、`number`、`hash`、`parentHash`、`transactions`の項目に着目します。

`miner`の項目はそのブロックを採掘したEOAのアドレスを示しています。今回は採掘者は自分一人の環境のテスト・ネットなので、採掘者は自分のEOAのアドレス（eth.coinbaseで得られるアドレス）が採掘者として記録されているはずです。

`number`はそのブロックのブロック高を示しています。（今回はブロック高を指定して情報を表示しているので、指定したブロック高と同一の値が表示されているはずです。

`hash`はそのブロックのブロック・ヘッダ・ハッシュを表示しています。ブロック・ヘッダ情報をSHA-3アルゴリズム適用して得られた32-byteのハッシュ値です。この`hash`はブロックを指し示すユニークなIDとして利用されます。ここで、`hash`は、当該ブロックのデータ構造の中に含まれないことに注意してください。`hash`を知る必要がある場合には各自がブロック・ヘッダのデータを元に計算することになります。

`parentHash`は、親ブロックのブロック・ヘッダ・ハッシュを示しています。parentHashはブロック・ヘッダのデータ構造の中に含まれており、つまりは、子ブロックから親ブロックを参照していることになります。このような参照の連鎖が連なることでブロックのチェーン「ブロックチェーン」が形成されていることになります（下図参照）。

![](../.gitbook/assets/blockchain_succession.png)

### 報酬の確認

採掘者が、自身の持つ計算資源を費やして採掘を行うインセンティブは、Ethereumの内部通貨であるetherを報酬として得られることにあります。実際に採掘をしたことによりetherが得られているかを確認してみましょう。

各アカウントのetherの持ち高を参照するには`eth.getBalance(address)`コマンドを用います。持ち高を確認したいアカウントのアドレスをaddressパラメータに引き渡すことで確認が可能です。先の手順で、２つのアカウントを作成していました。それぞれのetherの持ち高を確認してみましょう。

```text
> eth.accounts //作成したアカウントのアドレスを再確認
['0x24afe6c0c64821349bc1bfa73110512b33fa18e1', '0x59c444d6c4f4187d1dd1875ad74a558a2a3e20b6' ]
> 
> eth.coinbase == eth.accounts[0] //etherbaseは0番目のアカウントに紐づいている。
true
> eth.getBalance(eth.accounts[0])
'51500000000000000000'
> eth.getBalance(eth.accounts[1])
'0'
```

先に解説したとおり、coinbaseに紐づいたアカウントに採掘の報酬が与えられているのがわかります。`eth.getBalance(address)`は「wei」の単位 で持ち高が表示されます。以下の変換用のコマンドを使うことでetherの単位で表示することも可能です。

```text
> web3.fromWei(eth.getBalance(eth.accounts[0]),"ether")
'515'
```

## 脚注

