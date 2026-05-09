# MatchMaker 3v3

3対3形式のスポーツ(バスケットボール、バレーボール、フットサル等)で、参加者の組み合わせを公平に自動生成するシンプルな Web アプリです。

ブラウザで `index.html` を開くだけで動作し、サーバーもインストール作業も不要です。

![App](https://img.shields.io/badge/platform-Web-blue) ![License](https://img.shields.io/badge/license-MIT-green) ![Deps](https://img.shields.io/badge/dependencies-none-brightgreen)

---

## 特徴

- **単一HTMLファイル** — 依存ライブラリなし、`index.html` だけで完結
- **オフライン動作** — ネットワーク不要、すべてクライアントサイドで処理
- **localStorage 保存** — リロードしても状態が保持される
- **公平な組み合わせ** — 過去のチームメイト/対戦履歴を考慮し、偏りが最小になる組み合わせを自動探索
- **試合数の自動均等化** — 出場機会が少ないプレイヤーが優先的に選ばれる
- **複数コート対応** — 1〜20コートまで設定可能(出場人数に応じて自動調整)
- **モバイルファースト** — スマートフォンでの利用を最適化、iOSはホーム画面追加にも対応
- **ダークテーマ** — 体育館や屋外でも見やすい配色

---

## 使い方

### セットアップ

このリポジトリをクローン、もしくは `index.html` をダウンロードします。

```bash
git clone https://github.com/Masami-Koike/matchmaker3v3.git
cd matchmaker3v3
```

ブラウザで `index.html` を開くだけで起動します。

```bash
# macOS
open index.html

# Windows
start index.html

# Linux
xdg-open index.html
```

### GitHub Pages で公開する場合

リポジトリの **Settings → Pages → Source** で `main` ブランチのルートを指定すれば、そのまま公開できます。

### 基本フロー

1. **プレイヤータブ** — 参加者の名前を追加。出場しないメンバーはチェックを外す。
2. **組み合わせタブ** — コート数を設定し、「組み合わせを作成」をタップ。
3. **履歴タブ** — 過去のラウンド一覧を確認できる。

メニュー(右上 `⋯`)から、試合数のリセットや全データのリセットも可能です。

---

## アルゴリズム

各ラウンドの組み合わせ生成は次のように動作します。

1. アクティブなプレイヤーのうち、**試合数が少ない順** に出場者を選出(同数はランダム)。
2. 過去ラウンドから「ペアごとのチームメイト回数」と「対戦回数」を集計。
3. 6人を A 3人 / B 3人 に分ける全 20 通りを総当たりし、

   ```
   スコア = Σ(チームメイト履歴) × 3 + Σ(対戦履歴) × 2
   ```

   を最小化する分割を選択。
4. 複数コートの場合は、出場者のシャッフルを 80 回試行し、合計スコア最小の割当を採用。
5. 出場者の累計試合数を +1 してラウンドを保存。

「同じ味方が固定化する」のを最も嫌うようにチームメイト重複の重みを大きくしています。

詳細は [docs/SPEC.md](docs/SPEC.md) を参照してください。

---

## 技術スタック

- HTML5 / CSS3 / Vanilla JavaScript(ES2015+)
- 依存ライブラリ・ビルドツール **なし**
- ストレージ: `localStorage`(キー: `matchmaker3v3_v1`)

---

## ファイル構成

```
.
├── index.html       # アプリ本体(HTML / CSS / JS を内包)
├── docs/
│   └── SPEC.md      # 詳細仕様書
├── LICENSE
└── README.md
```

---

## 動作環境

- Chrome / Safari / Edge / Firefox の最新版
- スマートフォン(iOS Safari、Android Chrome)で動作確認

---

## ライセンス

[MIT License](LICENSE)
