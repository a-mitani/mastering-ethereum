# より便利なContract開発環境の活用

前節までで、gethを用いてコマンドライン上でSolidity言語によるContractの作成からコンパイル、そして実行までを行う手順を見てきました。

しかしこれらは見てきたように煩雑な操作が必要で、Solidity言語を用いてContractを実際に試行錯誤でコーディングしていくには不適です。

そのため、Contractのコーディングとコンパイル及び実行を助ける幾つかの開発環境が開発され始めています。この節では、これらの中の一つの「[browser-solidity](https://github.com/chriseth/browser-solidity)」の使い方を解説します。

[browser-solidity](https://github.com/chriseth/browser-solidity)はSolidity言語の開発者の一人である[chriseth](https://github.com/chriseth)により開発されているSolidity言語用Contract開発環境（IDE）であり、Webブラウザ上で
* Contractのコーディング
* コンパイル
* 実行
    * ブロックチェーン上への登録
    * Contract上の関数の実行

が可能です。

## browser-solidityのダウンロードと起動
browser-solidity[はGithubから最新バージョンのzipファイル](https://github.com/chriseth/browser-solidity/archive/gh-pages.zip)をダウンロードし、それを解凍したフォルダ内のindex.htmlをブラウザで開くことで利用可能です[^1]。

実際にindex.htmlをブラウザで開いた画面を下図に示します。画面は大きく左右２つに分かれています。左側でSolidity言語でContractを記述し、右側にはそのContractの情報が表示されたり、さらにContractの実行等を行うことが可能です。

![Browser Solidity画面](00_images/browser_solidity_initial_screen.png)

実際に「Contractを作成してみる」<!--[REF]-->節で作成した以下のContractを画面左側に入力してみましょう。


#### 脚注
[^1] gitに慣れている方はもちろんcloneしてもいいですし、browser-solidityのサイトも公開されているのでそこにアクセスして利用することも可能です。