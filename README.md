# ClassLog のインストールと更新（講師向け）

ClassLog は授業の出欠・宿題・テスト・演習を記録し、報告書や座席表を作る Mac 用アプリです。
このページは、インストールから更新を受け取るまでの手順です。

## ダウンロード

- 最新版: [Releases ページ](https://github.com/sky-wing1/classlog-releases/releases/latest) の `ClassLog_x.y.z_universal.dmg`
- 現在の版（v0.4.0）: [ClassLog_0.4.0_universal.dmg](https://github.com/sky-wing1/classlog-releases/releases/download/v0.4.0/ClassLog_0.4.0_universal.dmg)

Apple Silicon と Intel のどちらの Mac も同じ DMG で動きます。

## 必要なもの

- macOS（Apple Silicon、Intel のどちらも可）
- 上の DMG
- PDF 出力・印刷を使う場合: Python 3 と PDF 用ライブラリ（下記「PDF 出力の準備」）

出欠入力、CSV 出力、画面上の統計は Python なしで使えます。

## インストール

1. DMG をダブルクリックして開きます。
2. 中の `ClassLog` を `アプリケーション` フォルダへドラッグします。
3. `アプリケーション` から `ClassLog` を起動します。

配布している DMG とアプリは Apple の署名と公証を受けているので、通常は警告なく開けます。

### 「開けません」と表示されたとき

署名のない検証用ビルドを受け取った場合や、macOS の判定で止まった場合は、次の操作で一度だけ許可します。

- macOS 15 以降: 一度ダブルクリックして警告を閉じたあと、`システム設定` › `プライバシーとセキュリティ` を開き、下の方にある「"ClassLog" はブロックされました」の横の `このまま開く` を押します。
- macOS 14 以前: `アプリケーション` で `ClassLog` を右クリック（control+クリック）して `開く` を選び、確認ダイアログでも `開く` を押します。

「壊れているため開けません」と表示された場合は、配布担当者に連絡してください。

### 初回起動後の設定

起動直後はクラスがありません。次の順で設定します。

1. ホーム画面右上の `年度設定` で年度、前期・後期の開始日、休講期間を入れます。
2. `＋ クラス追加` でクラス名、学年、学期、曜日、時刻を入れます。
3. クラス画面で名簿を取り込み、`設定` で宿題・テスト・演習の項目と満点を確認します。

## PDF 出力の準備

週報と座席表の PDF は `PyMuPDF`、家庭宛報告書は `WeasyPrint` という Python ライブラリを使います。
ターミナルで次を実行すると確認できます。エラーが出なければ準備済みです。

```bash
python3 -c "import fitz; print('PyMuPDF', fitz.__doc__)"
```

```bash
python3 -c "import weasyprint; print('WeasyPrint', weasyprint.__version__)"
```

入っていない場合の導入は、Mac 側の設定に左右されるため配布担当者と一緒に行ってください。
Homebrew の Python では `pip install` がシステム保護（externally-managed-environment）で拒否されることがあり、WeasyPrint は Pango などのライブラリも必要です。
参考として、Homebrew を使う場合の例は次のとおりです。

```bash
brew install python pango && python3 -m pip install --break-system-packages PyMuPDF==1.28.0 WeasyPrint
```

家庭宛報告書のまとめ印刷は鉄TeX の `tetsuconvert` も使います。
PDF 出力でエラーが出たときは、表示されたメッセージをそのまま配布担当者へ送ってください。アプリは「どの Python を探して何が足りなかったか」を出します。

## データの保存場所とバックアップ

記録は次のフォルダに保存されます。Google やサーバーには送られません。

```
~/Library/Application Support/com.classlog.app/
```

バックアップは ClassLog を終了してから、このフォルダ全体をコピーします。
フォルダ名が `.app` で終わるため、Finder ではアプリのように見えて開きにくいことがあります。ターミナルで次を実行すると、デスクトップに日付付きのコピーができます。

```bash
cp -R "$HOME/Library/Application Support/com.classlog.app" "$HOME/Desktop/ClassLog-backup-$(date +%Y%m%d)"
```

Finder から開く場合は `移動` › `フォルダへ移動` に上のパスを貼ります。
生徒情報を含むので、共有フォルダやメール添付には入れないでください。

## 更新

ClassLog は起動時に新しい版があるかを確認し、あれば画面右下に「アップデートがあります」と表示します。

1. 入力中の内容を保存します。
2. `今すぐ更新` を押します。ダウンロードが終わるとアプリが自動で再起動します。
3. 再起動後、ホーム画面の左上に新しいバージョン番号（`v0.4.1` など）が表示されます。

`後で` を押すと、次回起動時にまた表示されます。
手動で確認するときは、ホーム画面のバージョン番号の横にある `更新を確認` を押します。

更新してもデータの保存場所は変わらず、記録はそのまま残ります。
「アップデートに失敗しました」と出たときは、上の Releases ページから新しい DMG を取得し、`アプリケーション` の `ClassLog` を置き換えてください。データはそのまま使えます。

## 困ったとき

配布担当者に次の 4 点を伝えてください。

- ClassLog のバージョン（ホーム画面左上の `v` で始まる番号）
- macOS のバージョン（Apple メニュー › `この Mac について`）
- 何をしたときに起きたか
- 表示されたメッセージ（スクリーンショットがあれば）

このリポジトリには配布物と案内だけを置いています。ソースコードは含みません。
