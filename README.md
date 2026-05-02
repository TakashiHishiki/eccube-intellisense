# EC-CUBE IntelliSense

EC-CUBE 4.x 開発のためのコード補完・IntelliSense を提供する Visual Studio Code 拡張機能です。PHP エンティティ、リポジトリ、サービス、Twig テンプレート、YAML 設定、プラグイン開発ワークフローをサポートします。

[![Visual Studio Marketplace](https://img.shields.io/visual-studio-marketplace/v/colscenery.eccube-intellisense?label=VS%20Marketplace&logo=visual-studio-code)](https://marketplace.visualstudio.com/items?itemName=colscenery.eccube-intellisense)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

🔗 **[Visual Studio Code 拡張機能マーケットプレイスで見る](https://marketplace.visualstudio.com/items?itemName=colscenery.eccube-intellisense)**

---

## 機能

### PHP 補完

- **エンティティ補完** — `Product`、`Customer`、`Order`、`OrderItem`、`Cart`、`Category` などの EC-CUBE コアエンティティをプロパティ・メソッドごとに補完します。
- **リポジトリ補完** — `$this->entityManager->getRepository(...)` 使用時にリポジトリメソッドを提案します。`src/Eccube/Entity`、`app/Customize/Entity`、`app/Plugin/*/Entity` から自動的にエンティティクラスを一覧します。
- **サービス補完** — `CartService`、`PurchaseFlow`、`MailService`、`EccubeConfig` などの組み込みサービスに対するタイプヒントと補完を提供します。
- **イベント名補完** — `getSubscribedEvents()` 内で利用可能な EC-CUBE イベント名をすべて提供します（テンプレートイベント `Shopping/index.twig`、コントローライベント `eccube.event.controller.*`、管理画面イベントなど）。
- **ルート補完** — `path()`、`url()`、`redirectToRoute()` 呼び出し内のルート名を補完します。`routes.yaml` および PHP の `#[Route]` アトリビュートや `@Route` アノテーションをスキャンして取得します。
- **EccubeConfig キー補完** — `$this->eccubeConfig[...]` アクセス時に一般的な設定キーを提案します。

### Twig 補完

- **EC-CUBE 関数** — `asset()`、`url()`、`path()`、`eccube_block_logo()`、`eccube_block_category()`、`eccube_block_cart()` などの組み込み Twig 関数を補完します。
- **EC-CUBE フィルター** — `|price`、`|ellipsis`、`|no_image_product`、`|trans` などの EC-CUBE 固有フィルターを提案します。
- **テンプレートパス補完** — `extends` タグや `include` タグでの EC-CUBE 標準テンプレート構造に基づくパス補完。
- **ルート補完** — Twig 内の `path()`、`url()` 呼び出しでルート名を補完します（ワークスペースの動的スキャン含む）。
- **Twig タグ** — `set`、`if`、`for`、`block`、`extends`、`include` などの Twig タグをキーワード補完します。

### YAML 補完（services.yaml）

- **タグ補完** — `eccube.event.subscriber`、`eccube.twig.extension`、`eccube.purchase.flow.cart`、`eccube.purchase.flow.order` などの EC-CUBE サービスタグを提案します。
- **サービスクラス補完** — DI 設定用に一般的な EC-CUBE サービスクラスを一覧します。
- **PurchaseFlow 優先度ヒント** — 各プロセッサーステージ（バリデート、在庫、価格、割引、配送、支払い、税、合計）の推奨優先度範囲を表示します。

---

## スニペット

### PHP スニペット

| プレフィックス | 説明 |
|---|---|
| `eccube-entity` | Doctrine ORM アノテーション付き EC-CUBE エンティティクラス |
| `eccube-repository` | `ServiceEntityRepository` を継承した EC-CUBE リポジトリ |
| `eccube-subscriber` | `getSubscribedEvents()` 付き EventSubscriber |
| `eccube-formtype` | FormType クラス |
| `eccube-controller` | `@Route` アノテーション付きコントローラー |
| `eccube-pluginmanager` | 全ライフサイクルメソッド付き Plugin PluginManager |
| `eccube-processor` | `ItemHolderPreprocessor` を実装した PurchaseFlow プロセッサー |
| `eccube-getrepo` | `$this->entityManager->getRepository(Entity::class)` |
| `eccube-config` | `$this->eccubeConfig['key']` |
| `eccube-qb` | Doctrine QueryBuilder テンプレート |

### Twig スニペット

| プレフィックス | 説明 |
|---|---|
| `eccube-extends` | `{% block main %}` 付き `default_frame.html.twig` の継承 |
| `eccube-price` | 価格フィルター: `{{ value\|price }}` |
| `eccube-path` | `{{ path('route_name', {}) }}` |
| `eccube-asset` | `{{ asset('path') }}` |
| `eccube-for-pagination` | ページネーション用 for ループ（空状態あり） |
| `eccube-product-image` | `no_image_product` フィルター付き商品画像タグ |
| `eccube-form` | `form_start`、`form_row`、`form_rest`、`form_end` の完全なフォームレンダリング |
| `eccube-csrf` | CSRF トークン hidden input |
| `eccube-block-*` | EC-CUBE ブロックヘルパー（ロゴ、カテゴリ、カート） |
| `eccube-pagination` | KnpPaginator ページネーションリンク |

### YAML スニペット

| プレフィックス | 説明 |
|---|---|
| `eccube-yaml-subscriber` | EventSubscriber サービス登録 |
| `eccube-yaml-twig-ext` | Twig 拡張サービス登録 |
| `eccube-yaml-purchaseflow-cart` | 優先度付き CartFlow プロセッサー登録 |
| `eccube-yaml-purchaseflow-order` | 優先度付き OrderFlow プロセッサー登録 |
| `eccube-yaml-formtype-ext` | フォームタイプ拡張登録 |

---

## ボイラープレートジェネレーター

コマンドパレット（`Ctrl+Shift+P`）から **EC-CUBE: Generate Boilerplate** コマンドを実行すると、以下のファイルをすばやく生成できます。

- エンティティクラス
- リポジトリクラス
- EventSubscriber
- FormType
- コントローラー
- Plugin PluginManager
- PurchaseFlow プロセッサー

---

## 要件

- Visual Studio Code 1.80.0 以上
- ワークスペースに EC-CUBE 4.x プロジェクトがあること（`src/Eccube/`、`app/Customize/`、`app/Plugin/` ディレクトリを自動検出します）

## 対応 EC-CUBE バージョン

- EC-CUBE 4.2
- EC-CUBE 4.3

---

## 拡張機能の設定

| 設定 | デフォルト | 説明 |
|---|---|---|
| `eccubeIntellisense.enable` | `true` | 拡張機能の有効/無効 |
| `eccubeIntellisense.eccubeVersion` | `"auto"` | EC-CUBE バージョン（`4.2`、`4.3`、または `auto`） |
| `eccubeIntellisense.enablePhpCompletion` | `true` | PHP 補完の有効/無効 |
| `eccubeIntellisense.enableTwigCompletion` | `true` | Twig 補完の有効/無効 |
| `eccubeIntellisense.enableYamlCompletion` | `true` | YAML 補完の有効/無効 |
| `eccubeIntellisense.scanOnStartup` | `true` | 起動時にワークスペースをスキャンして動的補完を有効にする |

---

## ライセンス

MIT

## コントリビュート

Issues やプルリクエストは歓迎します。  
https://github.com/TakashiHishiki/eccube-intellisense
