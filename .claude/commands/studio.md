---
description: "Claude Code Studio — 空の実装Ready環境を起動する。「/studio」で汎用起動、「/studio cover」で用途別起動"
---

# /studio — Claude Code Studio

空の実装Ready環境を起動するSkillです。

**`/studio` は「既存アプリを開くコマンド」ではなく、「空の制作環境を起動するコマンド」です。**

---

## コマンド体系

```
/studio              → 汎用Studio（空のReady画面）を起動
/studio cover        → Kindle Cover Studio（表紙制作アプリ）を起動
/studio book         → Kindle Book Studio（本制作ダッシュボード）を起動
```

---

## /studio（引数なし）— 汎用Studio

### 何が起きるか

1. 空のVite+Reactプロジェクト（`studio/`）の開発サーバーを起動
2. プレビューに「Claude Code Studio — Ready」画面が表示される
3. **特定アプリ名や特定用途のUIは一切表示されない**
4. ここから何でも作り始められる状態になる

### 初期画面

```
┌──────────────────────────────────┐
│                                    │
│            ● (green)               │
│                                    │
│      Claude Code Studio            │
│           READY                    │
│                                    │
│     ───────────────               │
│                                    │
│  ここからチャットで指示したUIを     │
│  実装し、プレビューで確認できます    │
│                                    │
│  作成されたUIは、見た目だけでなく   │
│  操作可能な状態で実装します         │
│                                    │
│  ┌─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─┐     │
│  │ Empty Canvas              │     │
│  │ Waiting for instruction   │     │
│  └─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─┘     │
│                                    │
└──────────────────────────────────┘
```

### Ready宣言

起動後、以下を表示:

```
「🟢 Claude Code Studio — Ready

 プレビュー: localhost:5174
 実装反映:   ✅ HMR（Vite + React）

 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

 空の制作環境が起動しました。

 ■ チャットで指示したUIがプレビューに即反映されます
 ■ 作成されるUIは見た目だけでなく、実際に操作できます
 ■ 画像挿入UIを作れば、実際に画像をアップロードできます
 ■ ソースコード変更 → HMR → 即プレビュー更新

 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

 何を作りますか？」
```

### その後の動作

ユーザーがチャットで指示を出したら:
1. `studio/src/App.tsx` を修正
2. HMRで即反映
3. プレビューのスクリーンショットで確認
4. UIは**必ず操作可能な状態**で実装する

**例:**
- 「A/Bの選択ボタンを作って」→ クリック・状態変化・画面反映まで実装
- 「画像アップロードUIを作って」→ ファイル選択・読み込み・画面表示まで実装
- 「フォームを作って」→ 入力・state管理・値の反映まで実装

---

## /studio cover — Kindle Cover Studio

### 何が起きるか

1. `kindle-cover-master/` の開発サーバーを起動（port 5173）
2. Kindle表紙制作アプリが表示される
3. 既存のフォーム・テンプレート・プレビュー・書き出し機能が全て利用可能

### Ready宣言

```
「🟢 Kindle Cover Studio — Ready

 プレビュー: localhost:5173
 実装反映:   ✅ HMR

 利用可能なSkill:
 /book-cover-analyze  → 表紙AI分析
 /book-cover-evaluate → 100点評価
 /book-cover-title    → タイトル最適化

 何を作りますか？ / 何を変更しますか？」
```

---

## /studio book — Kindle Book Studio

### 何が起きるか

1. `book-project-*/publish/` のHTTPサーバーを起動（port 8234）
2. 出版ダッシュボード + 本文プレビューが表示される
3. **注意: 閲覧専用（HMR非対応）**

### Ready宣言

```
「🟢 Kindle Book Studio — Ready

 ダッシュボード: localhost:8234
 実装反映:       ⚠️ 閲覧専用（静的HTML）

 利用可能なSkill:
 /book-seed → /book-outline → /book-write → /book-edit → /book-meta → /book-publish

 ※ コンテンツ変更はSkill経由で行い、ダッシュボードで確認する構成です。」
```

---

## サーバー設定

| Studio | launch.json名 | ポート | HMR | プロジェクト |
|--------|-------------|--------|-----|-----------|
| 汎用 | `studio` | 5174 | ✅ | `studio/` |
| Cover | `kindle-cover` | 5173 | ✅ | `kindle-cover-master/` |
| Book | `book-dashboard` | 8234 | ❌ | `book-project-*/publish/` |

---

## 実行手順（共通）

### Step 1: 引数判定

```
$ARGUMENTS が空           → 汎用Studio（studio/）を起動
$ARGUMENTS が "cover"     → kindle-cover を起動
$ARGUMENTS が "book"      → book-dashboard を起動
$ARGUMENTS がそれ以外     → 「未登録のStudioです。新規作成しますか？」
```

### Step 2: サーバー起動

1. launch.json の対応設定でプレビューサーバーを起動
2. すでに起動中なら再利用
3. 起動失敗時はエラー表示

### Step 3: Ready検証

1. プレビューのスクリーンショットを取得
2. 画面が正常に表示されているか確認
3. HMR対応の場合、ファイル変更が反映されるか確認

### Step 4: Ready宣言

検証OKなら、対応するReady宣言を表示。

---

## Ready状態の定義（全Studio共通）

`/studio` 実行後は、以下が成立する:

```
✅ プレビュー画面が立ち上がっている
✅ Claude Code からのコード変更がプレビューに即反映される（HMR対応の場合）
✅ プレビュー画面側をユーザーが実際に操作できる
✅ UIは見た目だけでなく、クリック・入力・選択が実際に動作する
✅ 画像挿入UIは実際にファイルを読み込み画面に反映できる
✅ チャット指示・Skill実行を、プレビューを見ながら進められる
```

## 開発ルール（全Studio共通）

1. **UIは「動くもの」を作る** — 見た目だけは禁止
2. **実装は必ずプレビューで確認する** — スクリーンショット取得
3. **エラーは即対応** — HMRエラー・型エラーは放置しない
4. **state管理を必ず伴う** — ボタン→onClick、入力→onChange、画像→FileReader

---

## 引数

$ARGUMENTS

```
/studio              → 汎用Studio（空のReady画面）
/studio cover        → Kindle Cover Studio
/studio book         → Kindle Book Studio
```
