---
title: "URB Block Lab と最近のXの投稿まとめ（UIAPduino・WebHID）"
emoji: "🧩"
type: "tech"
topics: ["uiapduino", "ruby", "webhid", "coderdojo", "電子工作"]
published: false
---

X（[@momoonga](https://x.com/momoonga)）に投稿した内容を、2026年9月20日から10月3日までの5件についてまとめました。5件のうち3件が URB Block の話なので、先に URB Block が何なのかを書いておきます。

## URB Block とは

![URB Block Lab のバナー。ブロックでつくる Ruby × 電子工作](/images/x-posts-summary-2026-10/urb-block-banner.png)

[URB Block Lab](https://tarosay.github.io/uiap-hid-web/uiapruby-block.html) は、**ブロックを組むと Ruby が出てきて、それが基板の中に入る**ページです。UIAPduino（HID ProMicro CH32V003）向けに、子ども向けの入口として作りました。

インストールもアカウントも要りません。Chrome か Edge で開くだけです。ブラウザと基板は WebHID API で、ドライバなしに直接やり取りします。

![URB Block Lab の画面。LED を 500 ミリ秒ごとに点けたり消したりするブロック](/images/x-posts-summary-2026-10/urb-block-lab.png)
*ギャラリーの「1秒ごとに光る」を開いたところ*

### ブロックが Ruby になる

上の画面のブロックからは、次の Ruby が出てきます。「Ruby」タブで見られます。

```ruby
pin2 = GPIO.new(2, GPIO::OUT)

loop do
  pin2.on
  wait_ms 500
  pin2.off
  wait_ms 500
end
```

この Ruby は [UIAPruby](https://tarosay.github.io/uiap-hid-web/uiapruby.html) のものです。UIAPruby は [Wakayama.rb](https://wakayamarb.org) コミュニティが開発した、Ruby の構文で UIAPduino を制御できる組み込み Ruby 環境です。

Ruby タブは読み取り専用です。書き換えたくなったら「UIAPruby で編集する ↗」を押すと、Ruby を手で書くページ（URB EE Lab）へ渡せます。

### プログラムは基板の中で動く

Scratch や Smalruby との最大の違いは、**プログラムが PC ではなく基板の中で動く**ことです。

書き込んでしまえば USB を抜いてよく、電源さえあればそのまま動き続けます。micro:bit の「ダウンロード」や、Arduino の「書き込み」と同じ考え方です。だからボタンの名前も「動かす」ではなく **「基板に書き込む」** にしてあります。

書き込みまでの流れは次のとおりです。

1. ブロックから Ruby を作る
2. ブラウザの中で Ruby を解析し、独自の軽量バイトコード（URB1 形式）にコンパイルする
3. WebHID で基板の I2C EEPROM に書き込む
4. 基板の上の TinyVM がバイトコードを実行する

### 必要なもの

| 必要なもの | 内容 |
|---|---|
| 基板 | UIAPduino |
| EEPROM | CAT24M01WI（128KB）または 24FC256（32KB）。ピン 3（SDA）とピン 4（SCL）につなぎます |
| ブラウザ | Chrome か Edge |

ファームウェアは、ページの「⚙ 準備」からブラウザで書き込みます。基板 1 台につき一度だけです。

### 使えるブロック

ブロックは「基本」「入出力」「通信」「I2C」「モーター」「NeoPixel」「繰り返し」「論理」「計算」「変数」「関数」に分かれています。特徴的なものを挙げます。

- **8×8 の NeoPixel に絵を出す** — マスを押して（押したままなぞって）絵を描けます。色は 8 つ使えます
- **記録** — 電源を切っても残る変数です。「記録〔スコア〕を 1 増やす」のように使います
- **配列** — 番号は 0 からで、番号には変数も式も使えます
- **繰り返し** — 「◯ 回繰り返す」のほか、条件の間繰り返す、番号を変えながら繰り返す、など 5 つあります
- **関数** — よく使う手順に名前をつけて、置いた場所で呼べます
- **I2C** — I2C の部品のレジスタから 1 バイト読み書きできます
- **サーボ** — 角度は数を打つほかに、変数でも渡せます

作品は `.rb` で保存できます。出てくるのはそのまま動く Ruby で、末尾のコメントにブロックが畳んで入っているので、開くとブロックの位置まで元通りになります。

### URB ギャラリー

[URB ギャラリー](https://tarosay.github.io/urb-gallery/) は、UIAPduino で作ったプログラムを並べる「みんなの作品」のページです。パネルを選ぶと URB Block Lab が開いて、その作品のブロックが出てきます。開いた先は新しい作品なので、いま開いている作品を上書きすることはありません。

![8×8 の NeoPixel に出したくだもののドット絵](/images/x-posts-summary-2026-10/kudamono.jpg =300x)
*くだものドット絵*

![距離センサを載せた車](/images/x-posts-summary-2026-10/urb-rover.jpg =400x)
*見て走る（距離センサで見ながら走り回る）*

ここから、新しい順にまとめます。

## 24FC256の配線をシンプルに（10月3日）

24FC256の配線をシンプルにやり直しました。向きを逆にして、SDAとSCLの側を合わせています。

https://x.com/momoonga/status/2106106667188543494

## URB BlockがI²C EEPROM「24FC256」に対応（10月2日）

URB BlockがI²C EEPROM「24FC256」に対応しました。

32KBのEEPROMが約90円で、ブレッドボードで組めます。CoderDojoでもURB Blockをぐっと使いやすくなりました。安く手軽に、UIAPrubyのブロックプログラミングを実機で楽しめます。

https://x.com/momoonga/status/2105917207826047349

これまで URB Block Lab は CAT24M01WI（128KB）を前提にしていて、EEPROM に置く変数の場所も CAT24M01WI の大きさに合わせて決めていました。そのままでは 24FC256（32KB）に収まらないので、次のように変えました。

- **変数の番地の割り当てを、24FC256 用に新しくした。** CAT24M01WI は今までどおりです
- **どの石かは、基板が自分で見分ける。** 選ぶところはありません。プログラムを始めるたびに判定します
- **配列に入る数は 5,410 個まで。** 超えると「配列が大きすぎて、基板に入りません」と出て、プログラムは始まりません
- **配列の番号が個数の外なら、その場で止まる。** どの配列の何番かを知らせます

24FC256 で使うには、基板のファームウェアを書き込み直す必要があります。古いファームウェアのままの基板には、プログラムが始まるときにページが「基板のファームウェアが古い版です」と知らせます。「⚙ 準備」から書き込み直せば出なくなります。

## Xcratch + UIAPduino / Scratch UIAPduino v0.3.0（9月27日）

[Xcratch + UIAPduino / Scratch UIAPduino](https://tarosay.github.io/scratch3-uiapduino/)（デスクトップ版）の v0.3.0 を公開しました。

- 「UIAPduino Remap3」を追加
- PWMが5本から8本に増え、サーボやLEDをより多く制御できる
- ブロックは従来どおり
- Xcratch・Scratchの両方で使える

https://x.com/momoonga/status/2103940720679879060

## WebHIDの接続トラブルを調べる「HID 接続チェック」（9月21日）

WebHIDの接続トラブルを調べるためのページ「[HID 接続チェック](https://tarosay.github.io/uiap-hid-web/hid-test.html)」を作りました。次のようなときに使えます。

- URB Block LabでUIAPduinoに書き込めない
- Android版ChromeでUSB機器が出てこない
- PCでWebHID接続できない

https://x.com/momoonga/status/2101968664983724081

きっかけは、講習で URB Block Lab を使ってもらったときのことです。ファームウェアの書き込みは全員できたのに、「基板に書き込む」でデバイスを選ぶダイアログが出ない PC がありました。

基板がつながらないときに、どこで止まっているかを切り分けるための診断ページです。スケッチは要りません。「チェックする」を 1 回押すと、次の 3 つを続けて確かめて、✔／！／✖ で結果を出します。

1. ブラウザが対応しているか
2. 機器を選べるか
3. 機器を開けるか

つまずいたときだけ、その段のくわしい説明が自動で開きます。開いてすぐ閉じるだけで、基板には何も書き込みません。URB Block Lab の「⚙ 準備」の一番下からも開けます。

## CoderDojoでURB Blockを初出し（9月20日）

CoderDojoでURB Blockを初めて出しました。8×8 NeoPixelを使って、子どもたちがコマ絵をじっくり作っていました。

その様子を見て早速改造し、絵の移動・回転・反転に対応しました。作品はギャラリーにも載せています。

https://x.com/momoonga/status/2101329983914578215

子どもたちに使ってもらって分かったことと、そこから直したことは次のとおりです。

| 子どもたちの様子 | 直したこと |
|---|---|
| 色の見本が小さく、ブロック本体の「いろ」を押しに行っていた | マスの右に大きな色の見本を 2 列 × 4 段で並べた |
| みんな、最初に白で塗りつぶしていた | 真っ白から描きはじめるようにした |
| 黒を「黒くない」と言われた（LED では黒は消灯になる） | 黒を ✕ の見た目の「けす」にした |
| 右にずらした絵を 1 枚ずつ描いている子がいた | 絵を動かすボタンを足した |

絵を動かすボタンは、十字で上下左右に 1 マスずつずらす、`↻` で右に 90 度まわす、`⇋` `⇅` で左右・上下に裏返す、の 3 種類です。「この絵からはじめる」を押すと、いま見えている絵を元にした新しいブロックができます。パラパラ漫画は、ブロックを複製して「待つ」を挟みながら並べ、1 コマずつずらして作ります。

この改造ではファームウェアは変えていません。ページが絵を作って、今までどおり基板へ送るだけです。

## リンク

- [URB Block Lab](https://tarosay.github.io/uiap-hid-web/uiapruby-block.html)
- [URB ギャラリー](https://tarosay.github.io/urb-gallery/)
- [UIAPduino WebHID Lab](https://tarosay.github.io/uiap-hid-web/)（ページ集のトップ）
- [uiap-hid-web（GitHub）](https://github.com/tarosay/uiap-hid-web)
- [urb-gallery（GitHub）](https://github.com/tarosay/urb-gallery)
