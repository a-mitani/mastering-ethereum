# より便利なContract開発環境の活用

前節までで、gethを用いてコマンドライン上でSolidity言語によるContractの作成からコンパイル、そして実行までを行う手順を見てきました。

しかしこれらは見てきたように煩雑な操作が必要で、Solidity言語を用いてContractを実際に試行錯誤でコーディングしていくには不適です。

そのため、Contractのコーディングとコンパイル及び実行を助ける幾つかの開発環境が開発され始めています。この節では、これらの中の一つの「[browser-solidity](https://github.com/chriseth/browser-solidity)」の使い方を解説します。

[browser-solidity](https://github.com/chriseth/browser-solidity)は[chriseth](https://github.com/chriseth)により開発されているSolidity言語用Contract開発環境であり、Webブラウザ上で
* Contractのコーディング
* コンパイル
* 実行
    * ブロックチェーン上への登録
    * Contract上の関数の実行

が可能です。

## browser-solidityへアクセス
browser-solidity[はGithubから最新バージョンのzipファイル](https://github.com/chriseth/browser-solidity/archive/gh-pages.zip)をダウンロードし、それを解凍したフォルダ内のindex.htmlをブラウザで開くことで利用可能です[^1]。





