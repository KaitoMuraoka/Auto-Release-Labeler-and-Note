# Auto Release Labeler and Note

## 概要
このワークフローは、リリースプロセスを自動化する GitHub Actions です。
特定のブランチに対する Pull Request(PR) にラベルを自動付与し、マージ時にリリースタグとリリースノートを自動生成します。
これにより、リリース作業の効率化と一貫性の向上が期待できます。

リリースタグは、ラベルが付与されたPRと同じタイトルが付与されます。
例えば、ラベルが付与されたPRのタイトルが `1.1.0` の場合、`v1.1.0` というタグが作成されます。

リリースノートでは、マージされるまでに作られた PR と貢献者の名前が過剰式で作成されます。


## サンプル
以下のコードはサンプルコードです。
こちらは、GitHub flow を想定して作られています。
`release/O.O.O` というブランチを作成し、かつ `main` ブランチに向けた PR を作成すると、`release` というタグが自動で付与されます。

これにより、このブランチがリリース用のPRであることがわかります。

PRのタイトルにはバージョン名を入力します。例えば、`3.1.1` とか `4.1.3` とか

`release` タグが付与された PR をマージするとタグとリリースノートが自動で作成されます。
PR のタイトルが `3.1.1` の場合、タグとリリースノートには、`v3.1.1` がとなります。

```yml
name: Release Management

on:
  pull_request:
    types:
      - opened
      - synchronize
      - closed
    branches:
      - main

permissions:
  contents: write
  pull-requests: write

jobs:
  release-management:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4
    
      - name: Auto Release Labeler and Note
        uses: KaitoMuraoka/Auto-Release-Labeler-and-Note@0.1.0
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          release_label: 'release'
          release_tag: ${{ github.event.pull_request.title }}
```