# actions-diff-test

cargo workspace 環境下で特定の workspace 内に変更差分がある時に限り、github actions workflow において任意の step を踏めるか検証したもの。

## workspace

`space_a` `space_b` の 2 つ。

## workflow

差分検出対象は `space_a` 配下の全ファイル

```yaml
on:
  push:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - uses: technote-space/get-diff-action@v5
        id: space_a_diff
        with:
          PATTERNS: |
            space_a/**/*

      - name: space_a に差分があった場合に実行する step
        if: steps.space_a_diff.outputs.diff
        run: echo '${{ steps.space_a_diff.outputs.diff }}'

      - name: space_a に差分がなかった場合に実行する step
        if: ${{ steps.space_a_diff.outputs.diff == false }}
        run: echo space_aに差分なし
```
