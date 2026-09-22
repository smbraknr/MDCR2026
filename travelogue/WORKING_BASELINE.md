# Working Baseline

**Status:** Living document / update as work progresses  
**Last updated:** 2026-09-21  
**Branch:** `travelogue/album-2026`

この文書は、MDCR2026旅行記制作について、現在有効な方針・構成・スクリプト・実行履歴を一か所から把握し、今後の作業を再開するための基礎資料とする。

最終決定だけを記録する文書ではなく、制作方法や環境が変われば更新する。

文書の役割は以下のように分ける。

- `DECISIONS.md`: 確定した判断・方針
- `PROGRESS.md`: 現在の進捗
- `layout/style-guide.md`: 承認済みレイアウト基準
- `backlog/`: 未実施の改善候補
- **この文書**: 上記を横断した現在の作業基準・構成・実行実績

Codex関連の作業・検証はこの文書の対象外とする。

---

# 1. 方針・決定

## 1.1 写真処理

Google Photos上の数百枚の原本を、そのままChatGPTへ読ませる方式は採用しない。

採用した処理経路:

```text
Google Photos 原本
        ↓
Macへダウンロード
        ↓
originals/ に原本保存
        ↓
ChatGPT用Preview生成
        ↓
manifest.csv生成
        ↓
Contact Sheet生成
        ↓
generated/ のみGoogle Driveへ
        ↓
ChatGPTが写真選定・本文対応・レイアウト
        ↓
最終印刷時だけ原本へ戻る
```

原則:

- `originals/` は変更しない。
- ChatGPTには軽量Previewを渡す。
- 最終印刷時にはGoogle Photos上の高解像度原本へ差し替える。
- Gitには写真原本を保存しない。
- Contact Sheetで一次選定し、候補だけ個別Previewを見る。
- 写真ID `P0001` 形式で人間・ChatGPT間の参照を統一する。

## 1.2 Preview画像

試作時の基準:

```text
形式: JPEG
長辺: 1800px
JPEG Quality: 82
色空間: RGB / sRGB想定
```

EXIF Orientationは画像自体へ反映する。

Previewへコピーする主な情報:

- DateTimeOriginal
- CreateDate
- GPSLatitude
- GPSLongitude

写真検索・管理はEXIFだけに依存せず、`manifest.csv` を主要な索引として使う。

## 1.3 旅行記

- 1ページ A4縦
- 1見開き A3横相当
- 本文は原則として千香視点
- 写真集ではなく、後で「何があり、どこへ行き、何を感じたか」を思い出せる記録を目標とする
- 写真より重要な出来事・感情・会話は、対応写真がなくても本文として残す

## 1.4 レイアウト

8/2〜8/4の本文初稿を実際に流し込んだ結果:

```text
8/2 約621字
8/3 約858字
8/4 約1,184字
```

当初の3見開きでは8/3・8/4が過密になるため、以下の5見開きへ変更した。

```text
8/2 : 1見開き
8/3 : 2見開き
8/4 : 2見開き
合計 : 5見開き
```

5見開きレイアウトは人間が承認済み。

以後、8/2〜8/4のレイアウト構造は、新情報や前提変更がない限り再検討しない。

残りの日程は `layout/style-guide.md` を基準に制作する。

## 1.5 White Tara / Green Tara

8/4のPreview群から、White Tara / Green Taraと安全に同定できる作品写真は現時点で確認できていない。

したがって:

- P0235をWhite Tara / Green Taraとは呼ばない。
- P0235は仏教美術の一例としてのみ使用する。
- White Tara / Green Taraについては本文を中心に扱う。
- 明確な作品写真が後で見つかった場合のみ差し替える。

---

# 2. 構成

## 2.1 ローカル写真処理環境

基準ディレクトリ:

```text
~/Documents/Projects/smbraknr/MDCR2026-photos/
```

構成:

```text
MDCR2026-photos/
├── originals/
│   └── Google Photosから取得した原本
├── generated/
│   ├── previews/
│   │   ├── 2026-08-02/
│   │   ├── 2026-08-03/
│   │   └── 2026-08-04/
│   ├── contacts/
│   ├── photo_index.csv
│   └── manifest.csv
├── scripts/
│   └── photo_pipeline.py
└── .venv/
```

## 2.2 Google Drive

Google Driveには原本を置かず、`generated/` の内容をアップロードする。

```text
generated/
├── manifest.csv
├── photo_index.csv
├── contacts/
└── previews/
```

8/2〜8/4検証で使用したフォルダ:

```text
https://drive.google.com/drive/folders/1e2R-wWd6zuout0gQuR3yO-aFgP4P8RL5
```

## 2.3 Git

Repository:

```text
smbraknr/MDCR2026
```

作業ブランチ:

```text
travelogue/album-2026
```

主要構成:

```text
travelogue/
├── README.md
├── WORKING_BASELINE.md
├── DECISIONS.md
├── PROGRESS.md
├── SOURCES.md
├── timeline.md
├── days/
├── photos/
│   └── selection.md
├── captions/
│   └── captions.md
├── layout/
│   ├── page-plan.md
│   ├── trial-wireframes-2026-08-02_04.md
│   ├── trial-render-evaluation-2026-08-02_04.md
│   └── style-guide.md
└── backlog/
    └── PHOTO_PIPELINE_IMPROVEMENTS.md
```

## 2.4 Backlog

現在有効な写真パイプライン改善候補:

1. Previewに500〜600KB程度のファイルサイズ上限を設ける。
2. Driveアップロード後にmanifestとPreviewのIDを自動照合する。
3. `.DS_Store`、`._*` 等をアップロード対象から除外する。
4. 本番写真選定開始後は既存写真IDを固定する。

詳細は `backlog/PHOTO_PIPELINE_IMPROVEMENTS.md` を参照する。

---

# 3. スクリプト

主要スクリプト:

```text
~/Documents/Projects/smbraknr/MDCR2026-photos/scripts/photo_pipeline.py
```

サブコマンド方式で運用する。

## 3.1 ディレクトリ作成

```bash
python scripts/photo_pipeline.py init
```

生成対象:

```text
originals/
generated/
generated/previews/
generated/contacts/
scripts/
```

## 3.2 Preview生成

```bash
python scripts/photo_pipeline.py build-previews
```

処理:

```text
originals/走査
    ↓
ExifToolでメタデータ取得
    ↓
DateTimeOriginal
    ↓ なければ
CreateDate
    ↓ なければ
FileMTime
    ↓
撮影日時順ソート
    ↓
P0001... を採番
    ↓
Orientation反映
    ↓
長辺1800pxへ縮小
    ↓
JPEG quality 82
    ↓
必要EXIFをコピー
    ↓
日付別previewsへ保存
    ↓
photo_index.csv生成
```

対応形式:

- jpg / jpeg
- png
- heic / heif
- tif / tiff

HEIC読込には `pillow-heif`、EXIF処理には `exiftool` を使用する。

## 3.3 manifest生成

```bash
python scripts/photo_pipeline.py build-manifest
```

`photo_index.csv` から `generated/manifest.csv` を生成する。

主要列:

```text
id
capture_date
capture_datetime
date_source
original_relpath
preview_relpath
latitude
longitude
original_width
original_height
preview_width
preview_height
original_size_bytes
preview_size_bytes
```

## 3.4 Contact Sheet生成

```bash
python scripts/photo_pipeline.py build-contacts
```

デフォルト:

- 5列
- 1枚20写真
- 写真ID `Pxxxx` と撮影時刻を焼き込む

## 3.5 一括処理

```bash
python scripts/photo_pipeline.py all
```

Preview、manifest、Contact Sheetまで一括生成する。

---

# 4. 実行履歴

## 4.1 写真環境構築

8/2〜8/4の写真をGoogle Photosから取得し、ローカルで写真パイプラインを実行した。

`generated/` をGoogle Driveへアップロードし、ChatGPTからDriveへアクセスできることを確認した。

## 4.2 manifest / Preview整合確認

対象総数:

```text
241枚
```

日別:

```text
8/2 : 29枚   P0001〜P0029
8/3 : 149枚  P0030〜P0178
8/4 : 63枚   P0179〜P0241
```

確認結果:

```text
ID欠番             なし
ID重複             なし
Previewパス重複    なし
DateTimeOriginal   全241件取得
```

Contact Sheet:

```text
8/2 : 2枚
8/3 : 8枚
8/4 : 4枚
合計 : 14枚
```

## 4.3 Drive同期確認

初回確認時、P0024.jpgとP0026.jpgがDrive一覧に一時的に現れなかった。

再確認後は正しい8/2フォルダに反映され、最終的に29/29枚が一致した。

ローカル生成障害ではなく、Google Drive側のアップロードまたは一覧反映途中だった可能性が高いと判断した。

## 4.4 Preview容量確認

241枚について:

```text
平均      約439KB
中央値    約421KB
500KB超   60枚
750KB超   10枚
1MB超     2枚
```

1MB超:

```text
P0110 約1.11MB
P0111 約1.15MB
```

この結果から、Preview容量制御をbacklogへ登録した。

## 4.5 ChatGPT画像認識試験

Contact Sheetと個別PreviewをGoogle Drive経由で取得し、画像内容を認識できることを確認した。

確認例:

- P0004
- P0029
- P0034
- P0178
- P0179
- P0241

Contact Sheet上のIDと個別Previewの内容が一致することも確認した。

したがって、以下の処理経路が成立している。

```text
manifest
↓
Contact Sheet
↓
候補ID選定
↓
個別Preview確認
```

## 4.6 写真選定

8/2〜8/4について全Contact Sheetを確認し、一次選定を実施した。

記録先:

```text
travelogue/photos/selection.md
```

本文との対応、主役・準主役・小写真・補助候補等を整理済み。

## 4.7 本文初稿

以下へ旅行記本文初稿を作成した。

```text
travelogue/days/2026-08-02.md
travelogue/days/2026-08-03.md
travelogue/days/2026-08-04.md
```

実文字量:

```text
8/2 約621字
8/3 約858字
8/4 約1,184字
```

## 4.8 ページ構成

当初は3見開き案だった。

本文を実際に入れて評価した結果、8/3・8/4が過密と判断し、以下へ変更した。

```text
見開き1  8/2      日本を発ち、モンゴルへ
見開き2  8/3前半  街へ出る
見開き3  8/3後半  博物館と、乗れなかったバス
見開き4  8/4前半  自然史博物館へ
見開き5  8/4後半  心に残ったもの、語り合ったこと
```

## 4.9 キャプション

選定写真のキャプション案を以下へ記録した。

```text
travelogue/captions/captions.md
```

## 4.10 A3レイアウト試作

8/2〜8/4について5見開きのA3横レイアウトを生成し、ページを画像化して目視確認した。

確認済み:

```text
日本語文字化け          なし
写真縦横比崩れ          なし
本文・写真の重なり      なし
重大なキャプション欠け  なし
中央折りの重大問題      なし
```

人間がこのレイアウトを承認した。

評価記録:

```text
travelogue/layout/trial-render-evaluation-2026-08-02_04.md
```

## 4.11 スタイル固定

承認された試作を基準として以下を作成した。

```text
travelogue/layout/style-guide.md
```

今後の基本:

- A3横見開き
- 見開きごとに視覚上の主役写真を原則1枚
- 本文9〜11pt相当
- キャプション7〜8.5pt相当
- 文字を小さく押し込むより、必要なら見開きを増やす
- 写真説明より、記憶・感情・会話を優先する

## 4.12 情報入り旅行記サンプル

単なるワイヤーフレームではなく、旅行記本文・写真・キャプション・補足情報を誌面へ実際に埋め込んだ8/2〜8/4のサンプルを生成した。

生成物名:

```text
MDCR2026_travelogue_fulltext_sample_2026-08-02_04.pdf
MDCR2026_travelogue_fulltext_sample_2026-08-02_04.html
```

これらは現時点では制作セッションで生成したサンプルであり、Git管理ファイルそのものではない。

情報の取捨選択、文章修正、レイアウト微調整は別工程で行う。

---

# 5. 現在地点

```text
写真処理環境の検証       完了
Drive → ChatGPT連携       完了
写真一次選定             8/2〜8/4完了
本文初稿                 8/2〜8/4完了
A3ページ構成             8/2〜8/4承認済み
レイアウト方式           承認・固定済み
情報入り旅行記サンプル   8/2〜8/4生成済み
```

8/2〜8/4で制作パイプラインの一巡が成立した。

以後は、原則として同じ流れを8/5以降へ適用する。

```text
旅行メモ
↓
日別記録
↓
本文初稿
↓
写真一次選定
↓
本文と写真の対応
↓
ページ割り
↓
承認済みStyle Guideによるレイアウト
↓
人間による内容・写真・レイアウトの最終判断
↓
高解像度原本へ差し替え
↓
校正・最終PDF
```

この文書は、制作方法・構成・実績に変更があった場合に更新する。
