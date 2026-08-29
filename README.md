# wdyb — what do you build?

GitHub の Pull Request の差分を **hunk 単位で 1 つずつ** 提示し、「これは何をやっていると思いますか？」と尋ねる Claude Code Skill です。

全部の hunk に自分の言葉で解釈を書き終えたあとで、はじめて Claude が答え合わせをします。読み違えていたところを根拠つきで指摘し、結果を `wdyb-<PR番号>.md` に保存します。

コードレビューの前の下読み、既存コードのキャッチアップ、コードリーディングの練習用です。

## 特徴

- **答えを先に教えない** — 収集フェーズでは相槌も評価もヒントも返しません。自力で読む時間を確保します
- **hunk 単位** — ファイル単位より細かい粒度で、どこを読み違えたかがはっきりします
- **根拠つきの指摘** — リポジトリの実コードを読んだうえで、ファイル名と行番号を挙げて指摘します
- **記録が残る** — `wdyb-123.md` として手元に残り、あとから傾向を振り返れます

## インストール

Claude Code で以下を実行します。

```
/plugin marketplace add kaleidot725/wdyb
/plugin install wdyb@wdyb
```

## 使い方

```
/wdyb:wdyb 123
```

引数には以下を渡せます。

| 引数 | 例 |
| --- | --- |
| PR 番号 | `/wdyb:wdyb 123` |
| PR の URL | `/wdyb:wdyb https://github.com/owner/repo/pull/123` |
| なし | `/wdyb:wdyb` — 現在のブランチに紐づく PR を探します |

対話中は次の入力が使えます。

| 入力 | 挙動 |
| --- | --- |
| `skip` | わからない hunk を飛ばす（未回答として記録） |
| `back` | 1 つ前の hunk に戻る |
| `stop` | 収集を打ち切り、そこまでの分で答え合わせに進む |

## 必要なもの

- [Claude Code](https://claude.com/claude-code)
- [GitHub CLI (`gh`)](https://cli.github.com/) — 認証済みであること（`gh auth login`）

## 出力例

`wdyb-123.md`

```markdown
# wdyb #123 — Add retry to API client

- 結果: 正しい 8 / おしい 3 / 誤り 2 / 未回答 1（全 14 hunk）

## 総評

エラーハンドリングの追加は正確に捉えられていますが、非同期処理の
実行順序の変化を見落とす傾向があります。

---

## 2. src/api/client.ts `@@ -40,3 +48,9 @@` — ⚠️ おしい

**あなたの解釈**

> リトライを追加している

**実際の内容**

リトライに加えて、指数バックオフの待機を挟んでいます。...
```

## ライセンス

MIT
