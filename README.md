# 警備チェックアプリ — デプロイ手順

## GitHub Pages でデプロイ（無料・5分）

### 1. GitHubリポジトリ作成
1. https://github.com/new を開く
2. Repository name: `keibi-app`
3. Public を選択
4. Create repository

### 2. ファイルをアップロード
```
Add file → Upload files
```
以下の3ファイルをドラッグ＆ドロップ：
- `index.html`
- `manifest.json`
- `sw.js`
- `icon.png`（make-icon.htmlで生成したもの）

### 3. GitHub Pages 有効化
1. Settings → Pages
2. Source: Deploy from a branch
3. Branch: main / root
4. Save

### 4. URLが発行される
```
https://fire-fighter119.github.io/keibi-app/
```
（5分ほどで有効になる）

### 5. iPhoneにインストール
1. Safari でURLを開く
2. 画面下の共有ボタン（□↑）をタップ
3. 「ホーム画面に追加」
4. 追加 → 完了

---

## アイコン作成
`make-icon.html` をブラウザで開くと自動でダウンロードされる。
ダウンロードされた `icon.png` をフォルダに入れてアップロード。
