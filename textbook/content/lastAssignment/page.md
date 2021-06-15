---
title: "ページを完成させる"
index: 530
label: "Chapter 5"
---

### 作るもの

![page](./images/page.png)

今まで作成したコンポーネントを使ってページを完成させていきます。

### 手順

1. まず、各コンポーネントで作成した.scss ファイルを.css ファイルに書き出す処理（コンパイル）を行います。VSCode の画面上部にある Terminal > New Terminal をクリックし、Terminal を表示してください。（コマンドプロンプトや bash 等を使っていただいても結構です。）
2. Terminal に`yarn build-sass`と入力しエンターキーを押すと、コンパイルが開始します。完了すると src/page ディレクトリに style.css というファイルが生成されます。このファイルに今まで作成してきた全ての scss ファイルの内容が css の記法に変換されて書き出されています。試しにファイルを開いて、今までに作成したコンポーネントのクラスの名前を検索(Ctrl or Command + f)してみると分かりやすいかもしれません。
3. 同じ src/page ディレクトリに index.html ファイルを作成します。先ほど生成した style.css を読み込むのはもちろん、必要な meta タグや bootstrap の CDN 読み込みの記述を忘れずにしてください。以下に例を示します。

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>サイト制作課題</title>

    <link rel="preconnect" href="https://fonts.gstatic.com" />
    <link
      href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700&family=Noto+Serif+JP:wght@400;500;700&family=Roboto:wght@400;500;700&display=swap"
      rel="stylesheet"
    />
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css"
      integrity="sha384-TX8t27EcRE3e/ihU7zmQxVncDAy5uIKz4rEkgIXeMed4M0jlfIDPvg6uqKI2xXr2"
      crossorigin="anonymous"
    />
    <link
      rel="stylesheet"
      href="https://use.fontawesome.com/releases/v5.6.3/css/all.css"
    />

    <link rel="stylesheet" href="./style.css" />
  </head>
  <body>

    ...

    <script
      src="https://code.jquery.com/jquery-3.5.1.slim.min.js"
      integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj"
      crossorigin="anonymous"
    ></script>
    <script
      src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js"
      integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN"
      crossorigin="anonymous"
    ></script>
    <script
      src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.min.js"
      integrity="sha384-w1Q4orYjBQndcko6MimVbzY0tgp4pWB4lZ7lr30WKz0vr/aWKhXdBNmNb5D92v7s"
      crossorigin="anonymous"
    ></script>
  </body>
</html>

```

4. 今まで Organisms として作成した部品を body の中にコピー＆ペーストして並べていきます。順番が前後しないよう気をつけてください。
5. index.html を保存し、chrome で開いて表示がお手本サイトと違っていないか確認しましょう。検証ツールを活用して、画面幅によってレイアウトが崩れていないかもよく見直しましょう。
6. 見た目に問題があったら該当するパーツに戻って修正し、1. 2.の手順を踏んで style.css を更新してもう一度確認しましょう。修正ができたら修正したファイルだけをコミットしておくと、何の修正をしたのか分かりやすいとともに、やはり間違っていたとき元に戻すことがしやすいのでしておきましょう。
   > たまに style.css がブラウザにキャッシュ（読み込み時間を短縮するために一度ダウンロードしたファイルを一時的に保存する機能）されてしまって修正した内容が反映されないことがあります。その場合はブラウザのキャッシュを削除して対応してください。
7. 問題を解決したら他の部品のときと同じようにプルリクエストを出してください。

### 補足　使用した技術

style.css を書き出す工程で見慣れない操作をしたと思うのですが、あれが何なのか気になる方へ向けて簡単に説明させていただきます。
まず、実行したコマンド`yarn build-sass`は、package.json 内 scripts に定義されているもので、`gulp`コマンドを実行します。コマンドは yarn で統一した方が分かりやすいと考えてこれにしただけですので直接 gulp と打ち込んでも結果は変わりません。

#### gulp

gulp はあらかじめ定義しておいた一連の処理をまとめて実行する、タスクランナーと呼ばれるツールの一つです。他に似たものでは webpack があり、こちらはサーバーを立てて開発中にファイルの更新をブラウザに常に反映させるような運用に向いています。今まで使ってきた storybook にも内部で使われています。

gulp のタスクは gulpfile.js で定義することができます。今回は sass のコンパイルを実行するとともに、画像への相対パスの変換やファイルの結合、アグリファイ（コードから改行や不要な空白を取り除き全て 1 行にまとめる。データが削減され若干読み込み速度が改善する。）をしています。試しに gulpfile.js の 14 行目を`//`でコメントアウトして`yarn build-sass`を実行すると style.css の中身がアグリファイされる前のものになるのが確認できると思います。

webpack の方が高機能ですが、比較的手軽に導入できるので、ちょっとした変換くらいだったらこっちを使うといいかもしれません。
