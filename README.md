# pc-recommend-for-beginners
PC初心者におすすめを紹介する。

# 使った技術スタック
Node.js
Reveal.js
VSCode
ChatGPT
Windows 11 Home


# 初期ディレクトリ構成（提案）
```bash
.
├─ docs/                  # Markdown, 画像, 図表
│  └─ 01_project_overview.md  ← 先ほどの Canvas 版をここへ
├─ slides/                # Reveal.js ソース (Markdown)
├─ video/                 # 解説動画素材（収録後に配置）
├─ .github/
│  ├─ workflows/          # GitHub Actions
│  └─ ISSUE_TEMPLATE/     # テンプレート（後述）
└─ README.md              # リポジトリ概要
```

# Readveal.jsスライド＆GitHub Pages自動公開
1. Reveal.jsのセットアップ
```bash
cd slides
npm init -y
npm install reveal.js
echo '# YourSlide.md' > index.md
```

2. GitHub Actions ワークフロー（.github/workflows/deploy.yml）
```yaml
name: Deploy Slides to Pages
on:
  push:
    branches: [ main ]
    paths: [ "slides/**" ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: |
          cd slides
          npm ci
          npx reveal-md index.md --static _site
      - name: Deploy to Pages
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: slides/_site
```
GitHub Pages の「Source」を GitHub Actions に設定しておく

3. 初回 Push → 自動デプロイ
数十秒後、https://<ユーザ名>.github.io/pc-recommend-for-beginners/ でスライドが閲覧可能に。


## タスク管理のベストプラクティス
