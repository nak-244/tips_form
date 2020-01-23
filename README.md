# GASとHTMLで入力フォームを作成し、スプレッドシートに記録する方法
G Suiteを利用されている方であれば、アンケート収集や申し込み受付などの場面でGoogleフォームを使うことができます。Googleフォームは、プログラミング不要で簡単にフォームを作成することができますので、Web初心者の方が作る場合や、パッと作ってしまいたい時には重宝しますね。しかし、簡単に作ることができるということは、裏返せば自由度が低いということであるため、デザインをこだわったり、動作をカスタマイズしたかったりする場合には十分に要件が満たせないこともあります。

今回は、Googleフォームでは表現できないようなフォームを作成するために、GASとHTMLを利用してオリジナルのフォームを作成し、入力内容をスプレッドシートに記録するための方法について紹介していきます。

方法1 doPostによる方法（同期処理）

方法2 google.script.runによる方法（非同期処理）

どちらの方法でも、ユーザーから見ればそれほど大きな違いがないため、あまり詳しくない方であれば、わかりやすい方法を採用すれば良いかと思われます。今回は、方法１の「doPost」による方法を説明していきます。

## doPostによる方法
### フォーム画面
https://script.google.com/macros/s/AKfycbwvNsTpF5-yq9IH6_kFutV_JjUAU1-uojNZghC-CfW4x9lov389/exec
シンプルに、テキストボックス、ラジオボタン、チェックボックスの３つの質問を用意しています。

### スプレッドシート
https://docs.google.com/spreadsheets/d/10iP5jXSHMMU3RBv-Snl8whxksgaRx4zP1SC2oXD6GtU/edit#gid=0
フォームで入力された３つの質問に加え、入力された時間が記入されるようになっています。

## 解説
スプレッドシートをコピーいただければ、「ツール」→「スクリプトエディタ」からスクリプトエディタを開くことができます。以下、スクリプトエディタの各ファイルごとに中身を説明していきます。

### index.html
~~~html
<!DOCTYPE html>
<html>

<head>
<base target="_top">
</head>

<body>
<h1>doPostによる方法</h1>
<form method="post" action="https://script.google.com/macros/s/AKfycbwvNsTpF5-yq9IH6_kFutV_JjUAU1-uojNZghC-CfW4x9lov389/exec">
氏名：<input type="text" name="name"><br>
性別：
<input type="radio" name="gender" value="男性">男性
<input type="radio" name="gender" value="女性">女性<br>
好きな動物：
<input type="checkbox" name="animal" value="犬">いぬ
<input type="checkbox" name="animal" value="猫">ねこ
<input type="checkbox" name="animal" value="うさぎ">うさぎ<br>
<input type="submit" value="送信する">
</form>
</body>

</html>
~~~
index.htmlでは、ユーザーがブラウザで表示し、入力する部分が表現されています。

`<input>`タグがいくつか設置されており、それぞれの質問を受け付けるボックスやボタンとなっています。また、name属性には各質問ごとに名称が付与されており、スプレッドシートに書き込む際に利用します。

`<input type="submit">`がクリックされると、`<form>`の中の情報が送信されます。

また、`<form>`タグには以下のようにaction属性が付与されています。

`<form method="post" action="https://script.google.com/macros/s/AKfycbwvNsTpF5-yq9IH6_kFutV_JjUAU1-uojNZghC-CfW4x9lov389/exec">`

action属性として設定されているURLの文字列は、このWEBアプリケーションのURLそのままです。上記スプレッドシートをコピーして利用されている方であれば、スクリプトエディタから「公開」→「Webアプリケーションとして導入」→「導入」で表示されるURLをコピーしてこの部分に置換してください。
