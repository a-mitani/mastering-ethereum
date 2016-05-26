## 簡単なEtherのwalletを作る（３）

前節では「simple-ether-wallet」に送金を行う機能を追加しました。walletの画面上で送金を行うとネットワーク上に送金のトランザクションが送信され、それを採掘者が採掘することでアカウントの残高が変化しました。

ブロックチェーンを用いた送金では、あるトランザクションが組み込まれたブロックが採掘されることでそのトランザクションが「1回の承認（confirmation）を得た」と考えます。そしてそのブロックの後ろに続くブロックが採掘されればその数に応じて承認の数が増えて、そのトランザクションの信頼が高まります。Ethereumではおおよそ12回の承認を得ることでほぼ間違いなく信頼ができると言われています。

この節ではwalletが送信したトランザクションの履歴を表示し、それぞれのトランザクションが何回承認（confirmation）されたかを下図のように表示する機能を追加します。

### Transactionsコレクションを定義
トランザクションの履歴を保管し管理するためにMeteorのCollectionオブジェクトを利用します。MeteorのCollectionオブジェクトはサーバサイドやブラウザのローカルストレージ上にデータを格納し永続的にデータを保持することも可能ですが、ここでは簡単のためにはブラウザ上のメモリ上のみに履歴を保存することにします。（そのため、ブラウザのタブを閉じれば履歴はクリアされます。）トランザクション履歴のCollectionオブジェクトとして`Transactions`を定義するコードを`client/lib/init.js`内に追加します。

> client/lib/init.js

```javascript
（前略）
//Session変数の初期化
initSessionVars();

//Transactionsコレクションの初期化
Transactions = new Mongo.Collection('transactions', {connection: null});

//オブザーバの起動
observeNode();
```
アプリが正しく動作していれば、このコードを追加したあと、ブラウザコンソール上で下記のようにコレクション内の全てのドキュメント（レコード）を取り出すコマンド`Transactions.find().fetch();`を実行してみると`[]`のように空の配列が返されるはずです。

> ブラウザコンソール上

```
> Transactions.find().fetch();
  []
```

### トランザクション情報を登録
Transactionsコレクションが定義されたので、Etherの送金を実行した際にそのトランザクション情報がTransactionsコレクションに登録されるようにします。

トランザクションの送信を行う`web3.eth.sendTransaction`関数のコールバック関数内で`alert("Ether Transfer Succeeded");`としていた部分の代わりに次のようなTransactionsコレクションへのドキュメント追加を行う処理を追加し下記のようにします。

> client/templates/components/send_ether_component.js

```javascript
（中略）
    //非同期関数「web3.eth.sendTransaction」を呼ぶことでノードにトランザクションを送信
    web3.eth.sendTransaction({
      from: fundInfo.fAddr,
      to: fundInfo.tAddr,
      value: fundInfo.amount
    }, function(error, txHash){ //戻り値としてトランザクションハッシュ値が返る
      console.log("Transaction Hash:", txHash, error);
      if(!error) {
        //発行したトランザクション情報をTransactionsコレクションに挿入
        Transactions.upsert(txHash, {$set: {
          amount: Session.get("sendEther.fundInfo").amount,
          from: Session.get("sendEther.fundInfo").fAddr,
          to: Session.get("sendEther.fundInfo").tAddr,
          timestamp: getCurrentUnixTime(),
          transactionHash: txHash,
          fee: estimatedFeeInWei().toString(10),
        }});
      } else {
        alert("Ether Transfer Failed");
      }
    });
    $('#sendConfirmModal').modal('hide');
}});

```

また、上記で追加した部分に現在のUNIX時刻を求める新しい関数を利用しているのでその定義を下記のように追記します。

> client/lib/modules/time_utils.js

```javascript
（前略：既存コード）
//現在のUNIX時刻を取得（単位：秒）
getCurrentUnixTime = function(){
  var date = new Date() ;
  var unixTimeSecond = Math.floor( date.getTime() / 1000 ) ;
  return unixTimeSecond;
};
```

以上の修正を行った上で、アプリ上で送金の操作を行い、ブラウザコンソール上で再度、` Transactions.find().fetch();`コマンドを実行してみてください。送金を行った情報がドキュメントとして保存されたのが見て取れるはずです。

### トランザクション情報を表示
次にTransactionsコレクションの情報を画面上に表示する機能をつけ加えていきます。Sendビューに`latestTransactionComponent`テンプレートを付け加え、このテンプレート内でTransactionsコレクションを表示する機能を実装していきます。

まずは、下記のようにSendビューのテンプレートに`latestTransactionComponent`テンプレートを挿入するInclusionsタグを追加します。

```html
<template name="send">
  <div class="row-fluid">
    <div class="col-md-8 col-md-offset-2">
      {{> accountStatusComponent}}
      {{> sendEtherComponent}}
      {{> latestTransactionComponent}}
    </div>
  </div>
</template>
```