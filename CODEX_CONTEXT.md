# keibi-app — Codex用プロジェクトコンテキスト

## 何を作っているか
警備員（服部さん）が夜勤中にiPhoneで使うチェックリストPWA。
単一ファイル `index.html` に CSS・HTML・JavaScript がすべて入っている。

## デプロイ
- GitHub Pages: `https://fire-fighter119.github.io/keibi-app/`
- ブランチ: ローカル `master` → リモート `main`
- プッシュコマンド: `git push origin master:main`

## ファイル構成（触るのはここだけ）
```
keibi-app/
├ index.html   ← 唯一の編集対象
├ sw.js        ← Service Worker（キャッシュ名変更のみ触る）
├ manifest.json
└ icon.png
```

---

## データ構造

### セクション（画面のカード1枚 = 1セクション）
```javascript
{
  id: 'patrol1',
  icon: '🔵',
  title: '1回目の巡回',
  time: '18:00',
  type: 'simple',        // 'simple' or 'pattern'
  expanded: false,
  subsections: [...],    // type:'simple' の場合
  // または
  holiday: { subsections: [...] },   // type:'pattern' の場合
  weekday: { subsections: [...] },
  selectedPattern: 'weekday',
}
```

### サブセクション
```javascript
{
  id: 'p1_bag',
  title: '〔持ち物〕',
  grid: true,            // trueならチップグリッド表示
  hasPatrolList: true,   // trueなら「巡回リスト」ボタン表示
  completeStamp: true,   // trueなら全完了時にスタンプ表示
  hint: '...',           // サブセクション下部のヒント文
  items: [...]
}
```

### アイテム（チェック項目）
```javascript
{
  id: 'ar1',
  text: '上番報告',
  icon: '📱',            // 絵文字 or 'phs' or 'u-lock'
  hint: '補足説明...',
  type: 'notice',        // 省略=通常 / 'notice'=オレンジ枠 / 'label'=見出し
  warn: true,            // オレンジ強調
  danger: true,          // 赤強調
  safe: true,            // 緑（対応不要バッジ）
  equipment: true,       // 青チップ（装備品）
  rare: true,            // 金チップ（条件付き持ち物）
  tag: '要注意',         // バッジ文字列
  lineMsg: '...',        // LINEで送る文章
  sharePhoto: true,      // 写真共有UI
  steps: [...],          // ステップ表示
  chips: [...],          // noticeアイテム内のチップ一覧
  checked: false,        // 状態（コードで自動管理）
}
```

### 巡回リスト（パターン別）
```javascript
DEFAULT_PATROL_LOCK    // 施錠巡回（1回目）
DEFAULT_PATROL_MAINTAIN // 維持巡回（2〜5回目・6回目休日）
DEFAULT_PATROL_UNLOCK  // 解錠巡回（6回目平日）

DEFAULT_PATROLS = { '施錠': LOCK, '維持': MAINTAIN, '解錠': UNLOCK }
```

---

## CSS変数（テーマカラー）
```css
--bg: #070B13          /* 背景 */
--surface: #142033     /* カード */
--surface2: #1F3150    /* セカンダリ */
--accent: #28B779      /* 緑 */
--accent2: #F4C76A     /* ゴールド */
--orange: #FFD166      /* 警告色 */
--red: #FF6B5E         /* 危険色 */
--text: #FFFFFF
--text2: #D9E6F7
```

---

## localStorage
- キー: `keibi_v5`
- 変更タイミング: セクション構造の大幅変更時のみ（通常は触らない）

---

## 重要ルール
1. `index.html` だけを編集する（他ファイルは基本触らない）
2. モバイルファースト（iPhone Safari で動作確認）
3. アイテムIDは既存のものを変えない（localStorage互換性のため）
4. 新規アイテムIDは既存と被らない一意のものを使う
5. ゲン（Claude Code）が設計・レビューを担当、ヒヨ（Codex）が実装を担当

---

## ゲン（Claude Code）との役割分担
| タスク | 担当 |
|---|---|
| 設計・構造変更の判断 | ゲン |
| テキスト内容・日本語 | ゲン |
| 新機能のアーキテクチャ | ゲン |
| CSS実装・ビジュアル調整 | ヒヨ（Codex）|
| アイテムデータの追加・編集 | ヒヨ（Codex）|
| UIコンポーネント実装 | ヒヨ（Codex）|
| バグ修正 | ヒヨ（Codex）→ ゲンがレビュー |
