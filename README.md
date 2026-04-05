# version-test（release-please 試用）

## ブランチの流れ（feature → develop → main）

| ブランチ | 役割 |
|----------|------|
| `feature/*` | 作業用。`develop` から作成し、PR で `develop` に戻す。 |
| `develop` | 統合ブランチ。日々のマージ先。 |
| `main` | リリース用。本番相当の履歴。`develop` からマージしてリリースする。 |

```text
feature/foo ──PR──► develop ──PR（リリースタイミング）──► main
```

- **日々の開発**: `develop` をチェックアウトし、`feature/件名` を切って作業 → PR のマージ先は **`develop`**。
- **リリース**: `develop` がまとまったら **`develop` → `main`** の PR を作り、レビュー後にマージする。

## release-please（バージョン・CHANGELOG・GitHub Release）

[release-please](https://github.com/googleapis/release-please) は **`main` への push** だけがトリガーです（`develop` への push では動きません）。

- `develop` → `main` のマージが `main` に入ったタイミングで、Release PR の作成・更新が走ります。
- マージ先が `main` になるコミットは、可能なら [Conventional Commits](https://www.conventionalcommits.org/)（`feat:` / `fix:` など）にすると、バージョンと CHANGELOG が意図どおり付きます。
  - `develop` 上の feature PR を **Squash merge** する場合は、**squash 後の 1 行目**を `feat:` / `fix:` 形式にするとよいです。
  - `develop` → `main` を **Squash** する場合は、その squash コミットのメッセージを Conventional Commits にすると、リリースノートに反映されやすいです。

`package.json` の `version` と `CHANGELOG.md` は、Release PR をマージしたときに揃います。設定画面などでは `package.json` を読むだけでよいです（`npm run version:show` がサンプル）。

## 初回セットアップ（クローンした人向け）

1. リポジトリの **Settings → Actions → General** で「Allow GitHub Actions to create and approve pull requests」を有効にする（release-please が PR を作るため）。GitHub CLI なら次でも可です。

   ```bash
   gh api -X PUT "repos/<owner>/<repo>/actions/permissions/workflow" --input - <<'EOF'
   {"default_workflow_permissions":"write","can_approve_pull_request_reviews":true}
   EOF
   ```

2. 日々は `develop` を基準にする。

   ```bash
   git fetch origin
   git checkout develop
   git pull origin develop
   ```

3. 機能ブランチの例。

   ```bash
   git checkout -b feature/my-change develop
   # ... 作業・コミット ...
   git push -u origin feature/my-change
   # GitHub で develop 向け PR を作成
   ```

## ローカルでバージョン表示（設定画面のイメージ）

```bash
npm run version:show
```
