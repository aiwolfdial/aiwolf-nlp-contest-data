# aiwolf-nlp-contest-data

人狼知能コンテスト（自然言語部門）の大会データ。公開してよい情報だけを置く（連絡先は置かない）。

```
contests/<大会 id>/contest.yaml   大会の定義（人が書く。大会中はほぼ変わらない）
contests/<大会 id>/results.yaml   結果（スクリプトが生成する）
roles/<人数>.yaml        村の人数ごとの役職構成
```

大会 id はウェブサイトの `content/menu/<大会>` を小文字にして `_en` を除いたもの（例: `inlg_2026`、`aiwolfdial2026_springjp`）。
ウェブサイト（aiwolf-nlp）はこのリポジトリを `external/aiwolf-nlp-contest-data` に submodule として取り込み、`contests/` と `roles/` を `data/` にマウントして、
`site.Data.contests.<id>.contest` / `.results` として参照する。

## contest.yaml に何を書くか

項目の一覧と「どの道具が使うか」は [contests/_example/contest.yaml](contests/_example/contest.yaml) のコメントに書いてある（これが仕様書）。
新しい大会は直近の同種の大会をコピーして直す。項目を足すときは、まず使う側（サイトのショートコード、ops）を作り、同時に `_example` に行を足す。
使う道具が無い項目は書かない、が散らかさない唯一の規則。

| 使う側 | 読む項目 |
|---|---|
| サイト（ショートコード・一覧の仕切り・結果ページ） | name / name_en, form_url, venue, sponsors, rules, tracks, rounds, texts, results.yaml |
| ops form（フォーム生成） | language, tracks, rounds |
| ops sync（登録の取り込み・通知） | name, owners, tracks, rounds → teams.yaml を書く |
| ops server（サーバ設定） | language, rules, tracks, rounds.public_name, roles/, teams.yaml |
| ops results（結果の集計） | name / name_en, tracks, rounds.public_name → results.yaml を書く |

`teams.yaml` と `results.yaml` は道具が生成する（手で書くのは `contest.yaml` だけ）。
