# Ethereumとは何か？

[Ethereum](https://www.ethereum.org/)（イーサリアム）は分散アプリケーションのためのプラットフォームであり、2013年12月以降からオープンソースプロジェクトとして開発が進められているものです。2016年3月14日に最初の安定版（Homestead）リリースがされました。

インターネットの登場以来、私たちは、メール、SNS、電子決済、クラウド・ファンディングなど数えきれないほどのWebサービスを利用して日々を生活しています。これらのサービスのほぼ全てにおいて、その運営に何らかの中央管理システム（組織）の存在が必須でした。

例えばSNSでは、個人がアップロードしたデータをFacebookやTwitterといった企業が中央で一元管理することでサービスが成り立っています。またクラウド・ファインディングでは、[Kickstarter](https://www.kickstarter.com/)のような企業が、資金調達の仲立ちをし、その中で集まった資金についての管理（資金調達希望者が提示した最低金額以上の資金が集まれば資金調達希望者に渡し、そうでなければ渡さない等）を行うことで、サービスが成り立っています。 また、インターネットの基盤であるドメイン名も、[ICANN](https://www.icann.org/)を中心とした管理組織によって管理がなされています。

このような中央管理システムの存在するサービスは以下の点で欠点があります。

* 可用性： 中央管理システム（組織）が存在する以上、その中央組織が何らかの理由で潰れればサービスは存続できません。また、組織が存続している場合でも[実際にEvernoteでもあったように](https://blog.evernote.com/blog/2010/08/09/july1/)、障害によるデータ消失の危険が避けられません。
* プライバシー：例えばSNSなど、個人の生活（プライバシー）のデータを私的企業が一手に握ることは、データ漏洩の危険性、企業によるデータの不正利用の危険性を考えると好ましい物ではありません。
* 検閲：中央の組織により管理されたサービスは、そのサービス提供者による独自の検閲が少なからず入ります。検閲するかしないかは、「サービス提供者（中央管理者）」の手に握られ、たとえそれが公序良俗に「反しない」ものであっても検閲の対象になる可能性があります。

Ethereumは、「ブロックチェーン」と呼ばれる技術をベースに、なんら特別な管理者のいないP2Pシステム上で様々なサービスを実現するための基盤を提供するものです。つまり、FacebookやTwitter、KickstarterやICANNといったような、中央で管理する機関（私的企業）の存在を必要とせずに同様のサービスを実現する基盤をEthereumは提供します。

Ethereumがどのようなものかを詳しく見ていくために、まずはベースとなる「ブロックチェーン」技術の革新性について見ていきます。

