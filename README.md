# 02 GitFlow 風ブランチ操作

## 学習目標

この演習では、`main`、`develop`、`feature` ブランチを使った GitFlow 風の作業を、通常の `git` コマンドだけで体験します。

この演習の主目的は Python の実行ではなく、ブランチを作成し、変更を commit し、別のブランチへ merge する流れを理解することです。Python スクリプトは、研究活動に近い題材を使うための補助作業として扱います。

終了後、次のことを説明できるようになることを目標にします。

- `main`、`develop`、`feature` の役割を説明できる
- `git switch -c` で新しいブランチを作成できる
- feature ブランチの変更を develop に merge できる
- ブランチの履歴を簡単な図で理解できる

## 前提条件

- 1コマ目の Git / GitHub 基本操作を終えている
- 自分用リポジトリを clone している
- VS Code とターミナルを使える
- Python を実行できると望ましい

Python が実行できない場合でも、`analysis_plan.md` を編集して commit と merge の演習を完了できます。

Python の実行確認:

```bash
python --version
```

うまくいかない場合は、環境によって次を試します。

```bash
py --version
```

```bash
python3 --version
```

## このリポジトリで使うファイル

```text
analysis_plan.md
data/measurements.csv
scripts/summarize.py
```

## ブランチの考え方

```text
main
  |
  o----------------------o
   \
    develop
      |
      o-----------o
       \         /
        feature/add-summary
```

- `main`: 完成した安定版を置くブランチ
- `develop`: 授業中に作業をまとめるブランチ
- `feature/...`: 1つの変更を試すためのブランチ

## 手順

### 1. 現在のブランチを確認する

```bash
git status
```

確認ポイント:

- `On branch main` と表示されるか確認します。
- 変更中のファイルがある場合は、先に commit するか教員に相談します。

### 2. develop ブランチを作成する

```bash
git switch -c develop
```

確認ポイント:

- `Switched to a new branch 'develop'` と表示されるか確認します。

### 3. develop ブランチを GitHub に送る

```bash
git push -u origin develop
```

確認ポイント:

- GitHub の branch 一覧に `develop` が表示されるか確認します。

### 4. feature ブランチを作成する

```bash
git switch -c feature/add-summary
```

確認ポイント:

- `On branch feature/add-summary` と表示されるか確認します。

```bash
git status
```

### 5. 分析計画を編集する

VS Code で `analysis_plan.md` を開き、`今回追加する分析` の欄に次のような内容を追記します。

```text
- 測定値の件数、平均値、最小値、最大値を確認する。
```

### 6. Python スクリプトを実行する

この手順は補助作業です。Python が実行できる人は、分析用スクリプトを動かして結果を観察します。

```bash
python scripts/summarize.py
```

うまくいかない場合:

```bash
py scripts/summarize.py
```

```bash
python3 scripts/summarize.py
```

確認ポイント:

- 件数、平均値、最小値、最大値がターミナルに表示されるか確認します。

Python が実行できない場合:

VS Code で `analysis_plan.md` を開き、`今回追加する分析` の欄に次の1行を追記して先に進みます。

```text
- Python実行環境は未確認だが、今回はブランチ操作を優先する。
```

確認ポイント:

- Python の設定で止まらず、Git の branch、commit、merge の練習を続けます。
- この演習では、Python の成功よりも GitFlow 風のブランチ操作を優先します。

### 7. 変更を commit する

```bash
git status
```

```bash
git add analysis_plan.md
```

```bash
git commit -m "分析計画に要約方法を追加"
```

確認ポイント:

- `git log --oneline` で commit が増えているか確認します。

```bash
git log --oneline
```

### 8. develop に戻って merge する

```bash
git switch develop
```

```bash
git merge feature/add-summary
```

確認ポイント:

- `Fast-forward` または merge 完了の表示が出るか確認します。
- `Fast-forward` と表示されてもエラーではありません。`develop` が feature ブランチの変更をそのまま前に進められる状態だった、という意味です。
- `analysis_plan.md` に feature ブランチの変更が入っているか確認します。

### 9. merge 後の履歴を確認する

```bash
git log --oneline --graph --all
```

確認ポイント:

- `feature/add-summary` で作成した commit が履歴に表示されるか確認します。
- `develop` が、その commit を含んでいるか確認します。
- 表示を終了できない場合は、`q` キーを押します。

### 10. develop を GitHub に push する

```bash
git push
```

確認ポイント:

- GitHub の `develop` ブランチで `analysis_plan.md` の変更を確認します。

## 補足: feature ブランチを GitHub に push する

3コマ目の Pull Request 演習では、feature ブランチを GitHub に push してから Pull Request を作成します。

今回の演習でも、feature ブランチを GitHub に送る場合は次のように実行できます。

```bash
git push -u origin feature/add-summary
```

確認ポイント:

- GitHub の branch 一覧に `feature/add-summary` が表示されます。
- `-u` は、次回以降そのブランチで `git push` を短く実行できるようにする指定です。

## 発展: merge commit を明示的に作る

この授業では通常の `git merge feature/add-summary` を使います。実行結果として `Fast-forward` になる場合がありますが、それは自然な動作です。

履歴に「feature ブランチを merge した」という commit を明示的に残したい場合は、発展として次のコマンドがあります。

```bash
git merge --no-ff feature/add-summary -m "Merge feature/add-summary into develop"
```

注意:

- 初学者演習では必須ではありません。
- `--no-ff` は fast-forward できる場合でも merge commit を作る指定です。
- 授業中は、教員の指示がある場合だけ使ってください。

## 期待される結果

- `develop` ブランチが作成されている
- `feature/add-summary` ブランチで作業した commit がある
- feature ブランチの変更が `develop` に merge されている
- `git log --oneline --graph --all` で履歴を確認できる
- GitHub 上でも `develop` ブランチの内容を確認できる

## よくあるエラー

### `fatal: a branch named 'develop' already exists`

原因:

すでに `develop` ブランチがあります。

対応:

```bash
git switch develop
```

### `error: Your local changes to the following files would be overwritten`

原因:

commit していない変更がある状態でブランチを切り替えようとしています。

対応:

```bash
git status
```

変更を確認し、必要なら commit します。不要な変更を消す操作は教員に確認してから行ってください。

### Python が実行できない

確認すること:

```bash
python --version
```

```bash
py --version
```

```bash
python3 --version
```

どれも使えない場合は、Python のインストール状態を教員に確認してください。

この演習では、Python が実行できなくても先に進めます。`analysis_plan.md` に次の行を追記して commit と merge の練習を続けます。

```text
- Python実行環境は未確認だが、今回はブランチ操作を優先する。
```

## 提出物

- GitHub 上の `develop` ブランチ URL
- `feature/add-summary` で作成した commit の URL
- `analysis_plan.md` の変更内容
- 授業中の作業記録

## 振り返り質問

- `main` と `develop` はどのように使い分けるとよいですか。
- feature ブランチを使う利点は何ですか。
- merge すると、どのブランチに変更が取り込まれますか。
- `Fast-forward` と表示された場合、それは何を意味しますか。
- Python が実行できない場合でも、この演習で達成すべき Git の操作は何ですか。

## 提出前チェックリスト

- [ ] `develop` ブランチを作成した
- [ ] `feature/add-summary` ブランチを作成した
- [ ] `analysis_plan.md` を編集した
- [ ] Python スクリプトを実行した、または Python が未確認であることを `analysis_plan.md` に追記した
- [ ] feature ブランチで commit した
- [ ] `develop` に merge した
- [ ] `git log --oneline --graph --all` で履歴を確認した
- [ ] `develop` を GitHub に push した
- [ ] 作業記録を書いた
