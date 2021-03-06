# 4d-tips-on-after-on-after-edit
少しの猶予（1.5秒程度）を置いてからOn After Editイベントを受け取る例題

[ダウンロード](https://github.com/4D-JP/4d-tips-on-after-on-after-edit/releases/tag/1.0)

### 概要

v14以降，テキスト入力エリアのイベント制御が新しくなったことにより，``On After Edit``イベントは日本語入力メソッドで文字列を変換している最中にも発生するようになりました。**ライブ検索**が実装できるようになったのは良いのですが，イベントの間隔が短すぎるため，かえって処理が重くなってしまうことがあります。この例題は，**ウィジェット**の仕組みを活用し，``On After Edit``イベントの発生タイミングをカスタマイズする，というものです。具体的には，タイプ入力中はイベントが発生せず，一定時間（デフォルトは``100``Ticks=1秒ちょっと。変更可），休止した後にイベントを発生させる，というものです。

### スクリーンショット

<img width="455" src="https://user-images.githubusercontent.com/10509075/74890156-9106e680-53c6-11ea-8a18-c6defe94fe31.png" />

### 仕組み

ウィジェット側では，``On After Edit``のイベント処理で``SET TIMER(100)``を実行しています。これは，``100``Tick後にイベントを発生させるコードですが，そのイベントが発生する前に再度，実行された場合，保留中のイベントは取り消され，その時から``100``Tick後にイベントが再設定されます。``SET TIMER`` ``POST OUTSIDE CALL`` ``CALL SUBFORM CONTAINER``などのコマンドは，イベントをキューに追加するわけではないためです。また，イベントの無限ループを防止するため，連鎖的なイベント処理は中断され，次回のフォームイベント（何でも良い）に持ち越されます。最終的に``On Timer``イベントが発生した時には，``SET TIMER(0)``でタイマーを中止し，``CALL FORM``で親フォームをコールしています。

v12からv15までは，``CALL SUBFORM CONTAINER``で親フォームをコールすることができました。（コールするといっても，イベントを「生成」するわけではなく，実際には，処理中のフォームイベントを転送するだけでした。前述したように，フォームイベントは「キュー」に追加されるわけではないので，特に``On Timer``などのイベントで``CALL SUBFORM CONTAINER``を使用した場合，イベントは次回のフォームイベント（何でも良い）に持ち越されてしまいます。v16以降，``CALL FORM``でこれを避けることができます。``CALL FORM``（および``CALL WORKER``）の呼び出しは「キュー」に追加され，順番に処理されるからです。

### おまけ

v18では，ウィジェットも``On Resize``イベントが受け取れるようになりました。そのことも，この例題で確認することができます。
