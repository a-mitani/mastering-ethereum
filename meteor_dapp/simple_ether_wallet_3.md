## 簡単なEtherのwalletを作る（３）

前節では「simple-ether-wallet」に送金を行う機能を追加しました。walletの画面上で送金を行うとネットワーク上に送金のトランザクションが送信され、それを採掘者が採掘することでアカウントの残高が変化しました。

ブロックチェーンを用いた送金では、あるトランザクションが組み込まれたブロックが採掘されることでそのトランザクションが「1回の承認（confirmation）を得た」と考えます。そしてそのブロックの後ろに続くブロックが採掘されればその数に応じて承認の数が増えて、そのトランザクションの信頼が高まります。Ethereumではおおよそ12回の承認を得ることでほぼ間違いなく信頼ができると言われています。

この節では下図の赤枠のようにwalletが送信したトランザクションの履歴とそれらのトランザクションが何回承認（confirmation）されたかを表示する機能を追加します。

<img src="00_img/send_view_transaction_list_red.png" width="500">

### Transactionsコレクションを定義
トランザクションの履歴を保管し管理するためにMeteorのCollectionオブジェクトを利用します。MeteorのCollectionオブジェクトはサーバサイドやブラウザのローカルストレージ上にデータを格納し永続的にデータを保持することも可能ですが、ここでは簡単のためにブラウザ上のメモリ上のみに履歴を保存することにします。（そのため、ブラウザのタブを閉じれば履歴はクリアされます。）トランザクション履歴のCollectionオブジェクトとして`Transactions`を定義するコードを`client/lib/init.js`内に追加します。

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
アプリが正しく動作していれば、このコードを追加したあと、ブラウザコンソール上で下記のようにコレクション内の全てのドキュメント（レコード）を取り出すコマンド`Transactions.find().fetch();`を実行してみると`[]`のように空の配列が返されるはずです。（Transactionsオブジェクトは定義したものの、まだデータは何も入れていないため空の配列が返されます。）

> ブラウザコンソール上

```
> Transactions.find().fetch();
  []
```

### トランザクション情報を登録
Transactionsコレクションが定義されたので、Etherの送金を実行した際にそのトランザクション情報をTransactionsコレクションに登録するようにします。

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

まずは、下記のようにSendビューのテンプレートにInclusionsタグ`{{> latestTransactionComponent}}`を追加します。

> client/templates/views/send.html

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
新しく、`client/templates/components/`以下に`latest_transaction_component.html`と`latest_transaction_component.js`のテンプレートとそのヘルパのファイルを下記のように追加します。ここでTransactionsコレクションに何も登録されていない場合は「No Transactions To Show」と表示し、一つ以上トランザクションが登録されていればTransactionsコレクションから`find`メソッドを用いて
* トランザクションの発生日時
* 送金元アドレス
* 送金先アドレス
* 送金額
を取り出し表示することを行っています。また現時点では承認回数をゼロで固定に表示しています。（後程、リアルタイムに承認回数を表示する機能を実装していきます。）

> client/templates/components/latest_transaction_component.html

```html
<template name="latestTransactionComponent">
  <div class="panel panel-default">
    <div class="panel-heading">
      <h4>Latest Transactions (Limit 5)</h4>
    </div>
    <table class="table table-striped" >
      <tbody>
        {{#each items}}
          {{> transactionItem}}
        {{else}}
          <tr>
           <td> No Transactions To Show</td>
          </tr>
        {{/each}}
      </tbody>
    </table>
  </div>
</template>

<template name="transactionItem">
  <tr>
    <td>
      <ul>
        <li><b>Datetime:</b> {{txDateTime}} </li>
        <li><b>From:</b> {{from}}</li>
        <li><b>To:</b> {{to}}</li>
      </ul>
    </td>
    <td>
      <h4>
        <b>{{amountInEther}}</b> ETHER
      </h4>
      <b>{{confirmationCount}}</b> confirmations
    </td>
  </tr>
</template>
```

> client/templates/components/latest_transaction_component.js

```javascript
//latestTransactionComponentテンプレートのヘルパー
Template.latestTransactionComponent.helpers({

  //Transactionsコレクションから最大5件のトランザクション情報を取得
  //timestamp属性について降順で取り出す。
  items: function(){
    selector = {};
    return Transactions.find(selector, {sort: {timestamp: -1}, limit: 5}).fetch();
  }
});


//transactionItemテンプレートのヘルパー
Template.transactionItem.helpers({

  //フォーマット化されたトランザクション時刻の取得
  txDateTime: function(){
    return unix2datetime(this.timestamp);
  },

  //送金額をEtherの単位で取得
  amountInEther: function(){
    var amountEth = web3.fromWei(this.amount, "ether");
    return parseFloat(amountEth).toFixed(3);
  },

  //承認回数の取得（現時点では固定でゼロを返す関数）
  confirmationCount: function(){
    var count = 0;
    return count;
  }
});
```

以上のコードを追加した後、画面から送金操作を行うとSendビューの下部に実際に送金した情報がリスト上に表示されるでしょう。


<div class="commit">
  <img src="../00_common_img/tags.png">
  <div class="message">
    <p><b><a href="https://github.com/a-mitani/simple-ether-wallet/releases/tag/step008" target="_blank">
      View this Commit On GitHub (Tag:"Step008")
    </a></b></p>
   </div>
</div>


### 承認回数をリアルタイム表示
最後に前節で一時的にゼロ固定にして表示した各トランザクションの承認回数をリアルタイムに表示する機能を付け加えてきましょう。

この機能は大まかに以下の方法で実現していきます。
* Transactionsコレクションの登録状況を常時ウォッチする機能を追加
  * トランザクションが新しく追加されたらそのトランザクションを常時ウォッチ
  * トランザクションが削除されたら、そのトランザクションの常時ウォッチを停止
* 個々のトランザクションを常時ウォッチし、


