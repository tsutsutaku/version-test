# version-test（release-please 試用）

`main` へ push すると [release-please](https://github.com/googleapis/release-please) が Release PR を作成・更新します。マージすると GitHub Release と `package.json` / `CHANGELOG.md` のバージョンが揃います。

## 手順

1. GitHub で空のリポジトリを作成する（README は付けなくてよい）。
2. このディレクトリでリモートを追加して `main` を push する。

   ```bash
   git remote add origin https://github.com/<あなたのユーザー>/<リポジトリ名>.git
   git branch -M main
   git push -u origin main
   ```

3. リポジトリの **Settings → Actions → General** で「Allow GitHub Actions to create and approve pull requests」を有効にする（release-please が PR を作るため）。

4. 以降、`feat:` / `fix:` など [Conventional Commits](https://www.conventionalcommits.org/) で `main` に積むと、Release PR が更新される。内容がよければその PR をマージしてリリースする。

## ローカルでバージョン表示（設定画面のイメージ）

`package.json` の `version` を読むだけの例です。

```bash
npm run version:show
```

## develop ブランチを試す場合

`develop` にだけマージしても Actions は動きません。`develop` → `main` のマージ（または `main` への直接 push）で release-please が動きます。
