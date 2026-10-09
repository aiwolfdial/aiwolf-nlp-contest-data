# aiwolf-nlp-contests

人狼知能コンテスト（自然言語部門）の大会データ。公開してよい情報だけを置く（連絡先は置かない）。

```
<大会 id>/contest.yaml   大会の定義（人が書く。大会中はほぼ変わらない）
<大会 id>/results.yaml   結果（スクリプトが生成する）
roles/<人数>.yaml        村の人数ごとの役職構成
```

大会 id はウェブサイトの `content/menu/<大会>` を小文字にして `_en` を除いたもの（例: `inlg_2026`、`aiwolfdial2026_springjp`）。
ウェブサイト（aiwolf-nlp）はこのリポジトリを `data/contests/` に submodule として取り込み、
`site.Data.contests.<id>.contest` / `.results` として参照する。
