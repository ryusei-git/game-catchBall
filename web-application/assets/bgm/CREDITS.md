# BGM クレジット / ライセンス

このフォルダの音楽はすべて **CC0 1.0（パブリックドメイン）** です。
クレジット表記・使用許諾・使用料はいずれも**不要**で、商用利用・改変・再配布が自由にできます。
Google Play などへ配信する場合も、音楽まわりで追加の手続きは発生しません。

> CC0 は表記義務がありませんが、作者への敬意として下記を残しています。
> ゲーム内に表示する義務はありません。

---

## 収録曲

### `home.*` — ホーム / ステージ選択 / きせかえ画面

| | |
|---|---|
| 曲名 | Childhood Flavors（"Ice Cream Truck Theme"） |
| 作者 | Cleyton Kauffman |
| 配布元 | https://opengameart.org/content/ice-cream-truck-theme |
| 作者リンク | https://soundcloud.com/cleytonkauffman |
| ライセンス | CC0 1.0 — https://creativecommons.org/publicdomain/zero/1.0/ |
| 長さ / テンポ | 約 91秒 / 約 123 BPM |
| 選定理由 | アイスクリーム屋さんをイメージした、まるくて楽しい曲。配布元でも「childlike」「cute」「joyful」とされており、パステル＋切り紙のホーム画面に合う。ループ前提で作られている。 |

### `play.*` — ゲームプレイ中

| | |
|---|---|
| 曲名 | Flowerbed Fields（Loop） |
| 作者 | Zane Little Music |
| 配布元 | https://opengameart.org/content/flowerbed-fields-loop |
| 作者リンク | https://ko-fi.com/zanelittle/ |
| ライセンス | CC0 1.0 — https://creativecommons.org/publicdomain/zero/1.0/ |
| 長さ / テンポ | 約 106秒 / 約 144 BPM |
| 選定理由 | 「お花畑」をイメージしたかわいいチップチューン。ホーム曲より速く前向きで、キャッチ中の高揚感が出る。音数が少なめなので、キャッチ音・ミス音（WebAudio の SFX）が埋もれない。 |

---

## 加工内容

配布元のオリジナルに対して、以下だけを行っています（CC0 のため改変は自由）。

* **ラウドネス統一**: 2曲とも **-20 LUFS / True Peak -1.5 dBFS** に揃えました。
  * 画面を移動しても音量が飛ばないようにするためです。
  * ゲーム側でさらに音量を絞っています（ホーム `0.60` / プレイ中 `0.50`）。SFX を埋もれさせないための値で、`catch-game.html` の `Bgm` モジュール内 `VOL` で調整できます。
  * 音量変更は**リニア（全体を一律に増減）**で行っているため、曲の頭とお尻のつながりは元のまま保たれています。
* **形式変換**: `.ogg`（Vorbis 約96kbps）と `.mp3`（96kbps）の2種類を用意。
  * カット・切り詰め・フェード追加は**していません**（ループの継ぎ目を壊さないため）。

---

## ファイル構成と、なぜ2形式あるか

```
assets/bgm/
├── home.ogg  /  home.mp3
└── play.ogg  /  play.mp3
```

* **`.ogg` が本命**です。Chrome / Firefox / Android では `.ogg` が使われ、**継ぎ目なくループ**します。
* **`.mp3` は iOS Safari 用の保険**です。iOS は Ogg Vorbis を再生できないため。
  * MP3 は仕様上、曲の前後にわずかな無音が入るため、ループの継ぎ目に一瞬すきまが出ることがあります（90〜106秒に1回なので、実用上ほとんど気になりません）。
* 再生時にどちらか一方しか読み込まれないので、**通信量は 1曲あたり 0.9〜1.3MB 程度**です。

---

## 差し替えるときは

1. 同じ名前（`home` / `play`）で `.ogg` と `.mp3` を置き換えるだけで動きます。
2. **ループ前提の曲を選んでください。** 曲の終わりがフェードアウトや無音で終わっていると、ループのたびに音が途切れます。
   * 見分け方: 曲の頭 0.6秒と、お尻 0.6秒の音量がだいたい同じなら OK です。
3. ラウドネスを揃える場合のコマンド例（`ffmpeg`）:

   ```bash
   # 1回目: 測定
   ffmpeg -i in.ogg -af loudnorm=I=-20:TP=-1.5:LRA=11:print_format=json -f null -

   # 2回目: 上で出た measured_* を渡して変換（linear=true でループの継ぎ目を保つ）
   ffmpeg -i in.ogg -af loudnorm=I=-20:TP=-1.5:LRA=11:measured_I=...:measured_TP=...:measured_LRA=...:measured_thresh=...:linear=true \
     -c:a libvorbis -q:a 2 home.ogg
   ffmpeg -i in.ogg -af loudnorm=...同上... -c:a libmp3lame -b:a 96k home.mp3
   ```

### 同じ CC0 で、雰囲気の近い候補（今回は見送ったもの）

| 曲 | 見送った理由 |
|---|---|
| [Ukulele Forest](https://opengameart.org/content/ukulele-forest-beginning-loop-and-end) | ウクレレの手触りは世界観に非常に合うが、ループ部分が**8.4秒**しかなく、繰り返しが早く耳につく |
| [Apple Cider](https://opengameart.org/content/apple-cider) | ほっこりした名曲だが、**終わりがフェードアウト**でループに向かない（BGM以外の用途向き） |
| [Feel Good Island](https://opengameart.org/content/feel-good-island) | マリンバ主体で子供向けにぴったりだが、こちらも**終わりが無音**でループ不可 |
| [Happy Clappy Loop](https://opengameart.org/content/happy-clappy-loop) | ピアノで上品。ループも綺麗だが **17.5秒**と短め |
