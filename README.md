# TimefreeDownloader
radiko.jpのタイムフリーをダウンロードするkivy(python GUI)アプリ。

プレミア会員限定のエリアフリー機能に**対応**しました。

**個人での視聴目的以外では使用しないでください。またDLした番組を動画サイトなどで公開しないでください。**

![](https://github.com/mj127/TimefreeDownloader/blob/master/image.png)

## Description
事前登録しておいた番組を選択してDLボタンを押すだけで簡単にタイムフリーをダウンロードできます。  
kivyを用いたpythonアプリですが、音声ファイルのDLはaria2cを用いることで**高速化**しています。  
DL後のファイルを結合するのにffmpegを用いています。  

## Requirements
- ffmpeg
- aria2c

以下は python
- BeautifulSoup
- kivy
- japanize-kivy

## Usage

プレミアム会員機能を利用するには、TFDL.py 内の下記にメールアドレスとパスワードを入力します。

```
premium_email    = ''
premiun_password = ''
```

 TFDL.py を起動後、DLしたい番組のスイッチをonにしてDLボタンをクリックするとDLが始まります。  
曜日のラベルをクリックすれば、その曜日の全スイッチをon/offできます。  
DLされた番組は```~/Radio```に保存されます。 この出力先は変数```output_file```によって変更も可能です。

上記の方法で番組をDLできるのは(その番組の放送終了後) ~ (次週放送日の前日24時)です。  
ここで言う放送日とはradikoの番組表での放送日に対応します。深夜番組は注意してください。  
例) [1/2(火)午前1時]放送の番組は、番組表では[1/1(月)25時]放送なので(次週放送日の前日24時)は[1/7(日)24時]です。  

(次週放送日の前日24時)以降にDLする場合は"a week ago"のスイッチをonにしてからDLしてください。  

## Custom
ダウンロードする番組は、DownLoadList.py を編集することで自由に設定が可能です。  
編集を保存後、TFDL.py を再度 起動することで反映されます。

対象の番組についての下記を、該当の曜日へ追加します。

```
"ID"  : ["ラベル",[["放送局ID","番組名,"保存名",曜日]]
```

"ID" は内部で使用します。任意です。番組ごとに被らないようにしてください。  
"ラベル" は GUI のスイッチ横に表示されるラベルです。  
"放送局ID"はradiko上での放送局IDです。"番組名"はradikoの番組表にある通りに入力してください。  
(コピペする事をオススメします。改行は'¥n'を代用してください。)  
"保存名"はDLしたファイルの保存名です。実際の出力は保存名の後に"年_月_日.aac"が追加されます。    
曜日は帯番組でないときは0にしてください。帯の場合は月曜なら1, 火曜なら2, ... ,日曜なら7と設定して、曜日を指定してください。

一つの番組のタイムフリーが複数に分割されている場合や、放送中の一部のみをダウンロードする場合は、下記を追加します。

```
"ID"  : ["ラベル",[["放送局ID","番組名,"保存名",曜日,"開始時刻","終了時刻"]]
```

タイムフリーでの "開始時刻" から "終了時刻" がダウンロードの対象となります。  
時刻は HHMM としてください。


## Technical Custom
TFDL.py ファイルの変更で、DLが高速化する場合があります。  
- thread の値(デフォルト値4)を変更する。
- aria2c のオプションの変更する。cmd_aria2c内のオプション-s, -j, -xの値を変更します。値を大きくしすぎると一部が欠陥することもあるので注意してください。

ffmpegのオプションと出力ファイル名を変更することで、aac以外の出力や年月日表記の変更も可能です。

各種APIを用いてカスタマイズすることで、DLしたファイルを自動でクラウドにアップロードすることも可能です。


## Troubleshooting
- "ERROR: cannot find the program on the schedule"は、番組を番組表から見つけられないエラーです。番組名、放送局IDを確認してください。
- "ERROR: Radiko.jp says 'illegal parameter'"は、放送前の番組をDLを試みた場合のエラーです。一週間前の番組をDLする場合は、"a week ago"のスイッチをonにしてからDLしてください。
- "ERROR: Radiko.jp says 'forbidden'"は、地域判定によるエラーです。起動時にコンソールに出力される地域を確認してください。異なった地域判定の場合は radiko.jp へ問合せることで修正が可能な場合があります。
