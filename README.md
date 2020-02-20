# 4d-tips-on-after-on-after-edit
少しの猶予（1.5秒程度）を置いてからOn After Editイベントを受け取る例題

[ダウンロード](https://github.com/4D-JP/4d-tips-on-after-on-after-edit/releases/tag/1.0)

### 概要

v14以降，テキスト入力エリアのイベント制御が新しくなったことにより，``On After Edit``イベントは日本語入力メソッドで文字列を変換している最中にも発生するようになりました。**ライブ検索**が実装できるようになったのは良いのですが，イベントの間隔が短すぎるため，かえって処理が重くなってしまうことがあります。この例題は，**ウィジェット**の仕組みを活用し，``On After Edit``イベントの発生タイミングをカスタマイズする，というものです。具体的には，タイプ入力中はイベントが発生せず，一定時間（デフォルトは``100``Ticks=1秒ちょっと。変更可），休止した後にイベントを発生させる，というものです。

### スクリーンショット

<img width="455" src="https://user-images.githubusercontent.com/10509075/74890156-9106e680-53c6-11ea-8a18-c6defe94fe31.png" />
