# EC-CUBE IntelliSense

EC-CUBE 4.x 開発向けの Visual Studio Code 拡張機能です。PHP エンティティ・リポジトリ・サービス・Twig テンプレート・YAML 設定・プラグイン開発に対して、コード補完と IntelliSense を提供します。

[![Visual Studio Marketplace](https://img.shields.io/visual-studio-marketplace/v/colscenery.eccube-intellisense?label=VS%20Marketplace)](https://marketplace.visualstudio.com/items?itemName=colscenery.eccube-intellisense)

**[拡張機能マーケットプレイスで入手する](https://marketplace.visualstudio.com/items?itemName=colscenery.eccube-intellisense)**

---

## 機能

### PHP 補完

**エンティティ補完**

`$entity->` と入力すると、EC-CUBE コアエンティティ（`Product`・`Customer`・`Order`・`OrderItem`・`Cart`・`Category` など）のプロパティとメソッドを補完します。

**リポジトリ補完**

`$this->entityManager->getRepository(...)` 使用時にリポジトリメソッドを補完します。`src/Eccube/Entity`・`app/Customize/Entity`・`app/Plugin/*/Entity` 以下のエンティティクラスを自動的に一覧表示します。

**サービス補完**

`CartService`・`PurchaseFlow`・`MailService`・`EccubeConfig` などの組み込みサービスに対して型ヒントと補完を提供します。

**イベント名補完**

`getSubscribedEvents()` 内で利用可能な EC-CUBE イベント名をすべて補完します。テンプレートイベント（`Shopping/index.twig`）・コントローライベント（`eccube.event.controller.*`）・管理画面イベントに対応しています。

**ルート名補完**

`path()`・`url()`・`redirectToRoute()` の引数にルート名を補完します。`routes.yaml` と PHP の `#[Route]` 属性・`@Route` アノテーションをスキャンしてワークスペースのルートを検出します。

**EccubeConfig キー補完**

`$this->eccubeConfig[...]` でよく使われる設定キーを補完します。

---

### Twig 補完

**EC-CUBE 関数**

`asset()`・`url()`・`path()`・`eccube_block_logo()`・`eccube_block_category()`・`eccube_block_cart()` などの組み込み Twig 関数を補完します。

**EC-CUBE フィルタ**

`|price`・`|ellipsis`・`|no_image_product`・`|trans` など EC-CUBE 固有のフィルタを補完します。

**テンプレートパス補完**

EC-CUBE 標準のテンプレート構造に基づいて、`extends` と `include` タグのテンプレートパスを補完します。

**ルート名補完**

`path()` と `url()` の Twig 呼び出し内でルート名を補完します。ワークスペースから動的にスキャンしたルートにも対応しています。

**Twig タグ補完**

`set`・`if`・`for`・`block`・`extends`・`include` などの Twig タグキーワードを補完します。

---

### YAML 補完（`services.yaml`）

**タグ補完**

`eccube.event.subscriber`・`eccube.twig.extension`・`eccube.purchase.flow.cart`・`eccube.purchase.flow.order` などの EC-CUBE サービスタグを補完します。

**サービスクラス補完**

DI 設定用の EC-CUBE 共通サービスクラスを一覧表示します。

**PurchaseFlow 優先度ヒント**

各プロセッサステージ（validate・stock・price・discount・delivery・payment・tax・total）の推奨優先度範囲を表示します。

---

### スニペット

#### PHP スニペット

| プレフィックス | 説明 |
|---|---|
| `eccube-entity` | Doctrine ORM アノテーション付き EC-CUBE エンティティクラス |
| `eccube-repository` | `ServiceEntityRepository` を継承した EC-CUBE リポジトリ |
| `eccube-subscriber` | `getSubscribedEvents()` 付き EventSubscriber |
| `eccube-formtype` | FormType クラス |
| `eccube-controller` | `@Route` アノテーション付きコントローラ |
| `eccube-pluginmanager` | 全ライフサイクルメソッド付き Plugin PluginManager |
| `eccube-processor` | `ItemHolderPreprocessor` を実装した PurchaseFlow プロセッサ |
| `eccube-getrepo` | `$this->entityManager->getRepository(Entity::class)` |
| `eccube-config` | `$this->eccubeConfig['key']` |
| `eccube-qb` | Doctrine QueryBuilder テンプレート |

#### Twig スニペット

| プレフィックス | 説明 |
|---|---|
| `eccube-extends` | `default_frame.html.twig` を継承した `{% block main %}` |
| `eccube-price` | 価格フィルタ: `{{ value\|price }}` |
| `eccube-path` | `{{ path('route_name', {}) }}` |
| `eccube-asset` | `{{ asset('path') }}` |
| `eccube-for-pagination` | 空状態付きページネーションの for ループ |
| `eccube-product-image` | `no_image_product` フィルタ付き商品画像タグ |
| `eccube-form` | `form_start`・`form_row`・`form_rest`・`form_end` を含むフォーム全体 |
| `eccube-csrf` | CSRF トークン hidden input |
| `eccube-block-*` | EC-CUBE ブロックヘルパー（logo・category・cart） |
| `eccube-pagination` | KnpPaginator ページネーションリンク |

#### YAML スニペット

| プレフィックス | 説明 |
|---|---|
| `eccube-yaml-subscriber` | EventSubscriber サービス登録 |
| `eccube-yaml-twig-ext` | Twig 拡張サービス登録 |
| `eccube-yaml-purchaseflow-cart` | CartFlow プロセッサの優先度付き登録 |
| `eccube-yaml-purchaseflow-order` | OrderFlow プロセッサの優先度付き登録 |
| `eccube-yaml-formtype-ext` | Form Type Extension 登録 |

---

### ボイラープレートジェネレーター

コマンドパレット（`Ctrl+Shift+P` / `Cmd+Shift+P`）から `EC-CUBE: Generate Boilerplate` を実行すると、以下のボイラープレートを生成できます。

- Entity クラス
- Repository クラス
- EventSubscriber
- FormType
- Controller
- Plugin PluginManager
- PurchaseFlow プロセッサ

---

## 動作要件

- Visual Studio Code 1.80.0 以上
- ワークスペースに EC-CUBE 4.x プロジェクトが存在すること（`src/Eccube/`・`app/Customize/`・`app/Plugin/` ディレクトリを自動検出します）

## EC-CUBE バージョン対応

- EC-CUBE 4.2
- EC-CUBE 4.3

---

## インストール

**マーケットプレイスからインストール（推奨）:**

[拡張機能マーケットプレイスで入手する](https://marketplace.visualstudio.com/items?itemName=colscenery.eccube-intellisense)

**VS Code Quick Open からインストール:**

`Ctrl+P` を押して以下のコマンドを貼り付けて実行します。

```
ext install colscenery.eccube-intellisense
```

---

## 設定

| 設定項目 | デフォルト | 説明 |
|---|---|---|
| `eccubeIntellisense.enable` | `true` | 拡張機能の有効/無効 |
| `eccubeIntellisense.eccubeVersion` | `"auto"` | EC-CUBE バージョン（`4.2` / `4.3` / `auto`） |
| `eccubeIntellisense.enablePhpCompletion` | `true` | PHP 補完の有効/無効 |
| `eccubeIntellisense.enableTwigCompletion` | `true` | Twig 補完の有効/無効 |
| `eccubeIntellisense.enableYamlCompletion` | `true` | YAML 補完の有効/無効 |
| `eccubeIntellisense.scanOnStartup` | `true` | 起動時のワークスペーススキャンの有効/無効 |

---

## おすすめの組み合わせ

- **Intelephense** — PHP 言語サポートと静的解析
- **Twig Language 2** — Twig シンタックスハイライト

---

## ライセンス

MIT

---

## コントリビュート

Issue・プルリクエストはリポジトリまでお願いします。

https://github.com/TakashiHishiki/eccube-intellisense
