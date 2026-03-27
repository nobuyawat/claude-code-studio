# Claude Code Studio

> **チャットでUIを作り、目の前で動かす。**
> API不要のAI開発環境。

---

## これは何か

`/studio` を実行すると、空のReady画面が立ち上がる。

そこからチャットで指示したUIだけが順番に実装され、**しかも実際に操作できる**。

- ボタンをクリックできる
- テキストを入力できる
- 画像をアップロードできる
- 状態が変わる。即座に反映される

**Google AI Studioのような「作りながら確認する体験」を、Claude Codeで再現する。**

---

## 特徴

| 機能 | 説明 |
|------|------|
| 🟢 Empty Canvas | 特定アプリに依存しない空の開発環境からスタート |
| ⚡ HMR | コード変更がリロードなしで即反映 |
| 🖱️ 実動作UI | クリック・入力・状態変化が実際に動く |
| 🖼️ 画像アップロード | ファイル選択→読み込み→画面表示まで完全動作 |
| 🤖 APIなし | Claude Code自身がAIエンジン。追加コストゼロ |
| 🧩 Skill連携 | /studio上で他のSkillを並行実行可能 |

---

## セットアップ

### 1. Skillファイルを配置

```bash
cp studio.md ~/.claude/commands/studio.md
```

### 2. Studioプロジェクトを作成

```bash
npm create vite@latest studio -- --template react-ts
cd studio && npm install
```

### 3. launch.jsonを設定

`.claude/launch.json` に追加:

```json
{
  "name": "studio",
  "runtimeExecutable": "npm",
  "runtimeArgs": ["run", "dev"],
  "port": 5174,
  "cwd": "studio"
}
```

### 4. 起動

```
/studio
```

🟢 **Ready** と表示されたら準備完了。

---

## 使い方

```
/studio を実行
  ↓
空のReady画面が立ち上がる
  ↓
「ボタンを作って」→ ボタンが現れる（クリックできる）
  ↓
「入力欄を追加して」→ 入力できる（リアルタイム反映）
  ↓
「画像アップロードを追加して」→ 実際にアップロードできる
```

### 用途別Studio

```
/studio              → 汎用（空のCanvas）
/studio cover        → Kindle Cover Studio（表紙制作）
/studio book         → Kindle Book Studio（本制作）
```

---

## 設計思想

### 「OS」と「アプリ」の分離

```
/studio        = 開発環境（OS）
/studio cover  = アプリ
/studio book   = アプリ
```

OSは1つ。アプリは無限に追加できる。

### APIが不要な理由

Claude Code自身がAIエンジン。API課金なし。キー管理なし。

---

## 動作環境

- Claude Code
- Node.js 18+

---

## ライセンス

MIT

---

**KMN Works** — AI × 個人開発 | [X](https://x.com/KmnWorks)
