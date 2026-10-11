# aiwolf-nlp-contest-data

人狼知能コンテスト（自然言語部門）の大会データ。公開してよい情報だけを置く（連絡先は置かない）。

```
contests/<大会 id>/contest.yaml   大会の定義（大会定義シートから生成する。手で編集しない）
contests/<大会 id>/results.yaml   結果（スクリプトが生成する）
contests/<大会 id>/teams.yaml     公開用のチーム一覧（ops sync が生成する）
catalog/tracks.yaml, rules.yaml   大会横断のトラック一覧とルールセット（シートの「トラック」「ルール」タブから生成）
roles/<人数>.yaml        村の人数ごとの役職構成
schema/fields.yaml       大会定義の項目（シートの行の元。ラベル・型・必須・説明）
sheet.yaml               大会定義シートの id
tools/sheet_import.py    シート → contest.yaml / catalog の変換（GitHub Actions とローカルの両方が使う）
```

大会 id はウェブサイトの `content/menu/<大会>` を小文字にして `_en` を除いたもの（例: `inlg_2026`、`aiwolfdial2026_springjp`）。
ウェブサイト（aiwolf-nlp）はこのリポジトリを `external/aiwolf-nlp-contest-data` に submodule として取り込み、`contests/` と `roles/` を `data/` にマウントして、
`site.Data.contests.<id>.contest` / `.results` として参照する。

## 大会定義の更新

大会定義シート（`sheet.yaml` の URL）を直し、シートのメニュー「大会データ → 大会データを更新」を押す。
`.github/workflows/import-sheet.yml` が `tools/sheet_import.py` でシートを読み、変わった大会の `contest.yaml` を書いて push する。
シートは非公開でよい。ボタンを押すとシートの中身が圧縮されて Actions に渡されるので、Actions はシートを読みに行かない。

項目を足すときは、まず使う側（サイトのショートコード、ops）を作り、`schema/fields.yaml` に行を足してから organizer-tool の `ops contest layout` でシートの形を作り直す。
使う道具が無い項目は書かない、が散らかさない唯一の規則。

| 使う側 | 読む項目 |
|---|---|
| サイト（ショートコード・一覧の仕切り・結果ページ） | name / name_en, form_url, venue, sponsors, rules, tracks, rounds, texts, results.yaml |
| ops form（フォーム生成） | language, tracks, rounds |
| ops sync（登録の取り込み・通知） | name, owners, tracks, rounds → teams.yaml を書く |
| ops server（サーバ設定） | language, rules, tracks, rounds.public_name, roles/, teams.yaml |
| ops results（結果の集計） | name / name_en, tracks, rounds.public_name → results.yaml を書く |

`teams.yaml` と `results.yaml` は道具が生成する（手で書くのは `contest.yaml` だけ）。
