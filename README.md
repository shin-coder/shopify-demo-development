# Shopify Demo Development

Shopifyのカスタムテーマ開発プロジェクト

## 🚀技術スタック

- **[Shopify](https://www.shopify.com/)** - ECプラットフォーム
- **[Tailwind CSS](https://tailwindcss.com/)** - ユーティリティファーストCSSフレームワーク
- **[Alpine.js](https://alpinejs.dev/)** - 軽量JavaScriptフレームワーク

## ⚙️セットアップ

### 前提条件

- Node.js (npm)
- Shopify CLI
- Shopifyストアへのアクセス権限

### プロジェクトの立ち上げ

このプロジェクトは、[Elizabeth_Clean](https://github.com/polidario/Elizabeth_Clean)スケルトンテーマをベースにしています。

新しくプロジェクトを作成する場合:

```sh
shopify theme init [ NAME OF YOUR THEME ] --clone-url https://github.com/polidario/Elizabeth_Clean
```

### インストール

```sh
npm install
```

### 開発サーバーの起動

```sh
npm run dev
```

このコマンドは以下を同時に実行します:
- Tailwind CSSのwatchモード（`src/tailwind.css` → `assets/application.css`）
- Shopifyの開発サーバー（ローカルプレビュー）

開発用のプレビューURLがターミナルに表示されます。

## 💻利用可能なコマンド

| コマンド | 説明 |
|---------|------|
| `npm run dev` | 開発サーバーを起動（Tailwind watch + Shopify dev） |
| `npm run build` | TailwindのCSSをビルド（本番用） |
| `npm run tailwind:watch` | Tailwind CSSのみをwatchモードで実行 |
| `npm run shopify:dev` | Shopifyの開発サーバーのみを起動 |

## 🔥デプロイ

テーマファイルをShopifyストアにアップロードする:

```sh
shopify theme push
```

特定のテーマにプッシュする場合:

```sh
shopify theme push --theme [THEME_ID]
```

## 🌏多言語対応（Locales）

### ロケールファイルの設定

翻訳テキストは `locales/` ディレクトリのJSONファイルで管理します。

**ファイル構成例:**
```
locales/
├── en.default.json  # 英語（デフォルト・必須）
└── ja.json          # 日本語（必要な場合のみ追加）
```

**JSONの構造例** (`locales/en.default.json`):
```json
{
  "general": {
    "404": {
      "title": "Not found",
      "subtext_html": "The page you were looking for does not exist"
    }
  }
}
```

日本語対応が必要な場合は、同じ構造で `locales/ja.json` を作成します。

### Liquidテンプレートでの使用方法

ロケールキーを参照するには `t` フィルターを使用します:

```liquid
{{ 'general.404.title' | t }}
```

**変数を含む翻訳:**
```json
{
  "general": {
    "greeting": "Hello, {{ name }}!"
  }
}
```

```liquid
{{ 'general.greeting' | t: name: customer.name }}
```

**複数形対応:**
```json
{
  "cart": {
    "item_count": {
      "one": "{{ count }} item",
      "other": "{{ count }} items"
    }
  }
}
```

```liquid
{{ 'cart.item_count' | t: count: cart.item_count }}
```

## 📁テンプレートファイル（JSON）でのページ作成

Shopify 2.0では、JSONテンプレートを使用してページを構築します。

### JSONテンプレートの基本構造

`templates/` フォルダにJSONファイルを作成します:

```json
{
  "name": "Page Name",
  "sections": {
    "main": {
      "type": "about-section"
    },
    "testimonials": {
      "type": "testimonials-section"
    }
  },
  "order": ["main", "testimonials"]
}
```

**各プロパティの説明:**
- `name`: テンプレートの表示名（Shopify管理画面で表示される）
- `sections`: 使用するセクションを定義
- `type`: `sections/` フォルダ内のファイル名（拡張子なし）
  - 例: `"type": "about-section"` → `sections/about-section.liquid`
- `order`: セクションの表示順序を配列で指定

### 使い方

1. `sections/` フォルダにセクションファイル（`.liquid`）を作成
2. `templates/` にJSONファイルを作成して、`type` でセクションを指定
3. Shopify管理画面でページ作成時にこのテンプレートを選択

## リソース

- **SVGアイコン**: [Heroicons](https://heroicons.com/)を使用
- **多言語対応**: [Shopify Theme Translation](https://shopify.dev/docs/themes/architecture/locales)

## プロジェクト構成

```
.
├── assets/           # CSS, JS, 画像などの静的ファイル
├── config/           # テーマ設定ファイル
├── layout/           # レイアウトテンプレート
├── locales/          # 多言語対応ファイル
├── sections/         # セクション（再利用可能なコンポーネント）
├── snippets/         # スニペット（小さな再利用可能なコード）
├── src/              # ソースファイル（Tailwind CSS等）
└── templates/        # ページテンプレート
```

## 注意事項

- 開発時は必ず `npm run dev` を使用してください（Tailwind CSSの自動コンパイルのため）
- 本番環境へのプッシュ前に `npm run build` を実行することを推奨します
