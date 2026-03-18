# 1. README.mdをindex.htmlにリネーム
mv README.md index.html

# 2. 本来のREADMEを作成（Markdown形式）
echo "# 数学問題データベース

AIを活用した数学問題管理・生成システム

## 機能
- 問題登録と管理
- ユーザー評価の集計
- AI試験生成
- 弱点分析

[こちらからアクセス](https://ryuya-urata.github.io/math-problems/)" > README.md

# 3. GitHubにプッシュ
git add .
git commit -m "Deploy app to GitHub Pages"
git push origin main
