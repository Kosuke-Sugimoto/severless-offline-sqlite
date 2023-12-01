## 概要
serverless-offlineとsqlite3を用いてCRUD命令を処理する簡単なAPIを作成する

## 実行環境整備に関するメモ
```bash
npm install –g serverless 
npx sls create --template aws-python3 --name [任意のディレクトリ名]
cd [任意のディレクトリ名]
npm install –-save-dev serverless-offline 
```

## 運用におけるメモ
- sessionはそれぞれのエントリ毎に消去される方が都合がよい  
⇒ 概要  
1つのデータベースに複数のユーザーがアクセスすることを考えた際に、エントリ毎に消去されない（globalでセッションを持つ）と最大session数を上回る恐れがある  
対して、エントリ毎に消去を行うとglobalで保持していた場合に比べて多くのユーザーに対応することができる  
⇒ 対応方法  
デコレータ（抽象クラスみたいな）にて、外側でsessionを定義する記述をし、内側でsessionを処理関数に引き渡すような記述をすれば解決できる（詳しくは実装を参照）
- global 宣言はなるべく使わないように
- user-idはこちらが指定するものではなく、発行するもの（尚且つ一意でなくてはならないため、uuid等を使うのが吉）

## 確認されたエラーに関するメモ
次のような文言が確認され、  
&emsp;[504] Lambda Timeout  
尚且つ、サーバー側のログに "Python" と出力された場合には、  
Pythonランタイムの未インストールが原因の可能性が高い！  
Server側のログに出力された "Python" の表記は、Windows で Python ランタイムがインストールされていない際に python コンソールを立ち上げようとすると生じる事例であると考えられる