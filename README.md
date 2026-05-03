# EC-CUBE IntelliSense

Visual Studio Code上で**EC-CUBE 4.x**の開発をサポートするコード補完・IntelliSense・シグネチャヒント拡張機能です。

[拡張機能マーケットプレイス](https://marketplace.visualstudio.com/items?itemName=colscenery.eccube-intellisense)

## 機能

### Twig — forループの型推論とドットアクセス補完

テンプレート内のすべての`{% for %}`ループを解析し、ループ変数の型を推論します。変数の後に`.`を入力すると、そのメソッドやプロパティがすぐに表示されます。

```twig
{% for Product in pagination %}
  {# 「Product.」と入力 → getId(), getName(), getProductTag(), ... #}

  {% for Tag in Product.ProductTag %}
    {# 「Tag」はProductTagエンティティとして解決
       「Tag.」と入力 → shows getTag(), getProduct(), getId(), ... #}

    {{ Tag.Tag }}
    {# 「Tag.Tag」はMaster\Tagエンティティとして解決
       任意の識別子にホバーすると型とドキュメントが表示 #}
  {% endfor %}
{% endfor %}
```

チェーンドットアクセスは任意の深さまでサポートしています。

### Twig — 引数ヒント（シグネチャヘルプ）

関数やメソッド名の後に`(`を入力すると、パラメータ名と型を含むシグネチャオーバーレイが表示されます。

例:

- `path(` → `path(route_name, parameters)`
- `Product.setName(` → `Product::setName(string $name): self`

### PHP — 型推論による補完

`$var->`の補完は、複数の宣言パターンからエンティティ型を解決します。

```php
/** @var Product $product */            // PHPDocアノテーション
$product = new Product();               // new式
function edit(Product $product) { ... } // パラメータ型宣言
foreach ($products as $product) { ... } // foreach変数
$product = $orderItem->getProduct();    // メソッド戻り値型のチェーン
```

上記のいずれかの後に`$product->`と入力すると、Productエンティティのすべてのメソッドとプロパティが完全なシグネチャとともに表示されます。

### PHP — 引数ヒント（シグネチャヘルプ）

`$entity->methodName`の後に`(`を入力すると、完全なシグネチャが表示されます。

```
Order::setShippingDate(\DateTime|null $shippingDate): self
                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

### ボイラープレートジェネレーター

コマンドパレット（`Ctrl+Shift+P`）を開き、**EC-CUBE: Generate Boilerplate**を実行すると、任意のクラステンプレートをインタラクティブに挿入できます。

---

## PHPスニペット

| プレフィックス | 説明 |
|---|---|
| `eccube-entity` | Doctrine ORMアノテーション付きエンティティクラス |
| `eccube-repository` | `ServiceEntityRepository`を継承したリポジトリ |
| `eccube-subscriber` | `getSubscribedEvents()`付きEventSubscriber |
| `eccube-formtype` | FormTypeクラス |
| `eccube-controller` | `#[Route]`属性付きコントローラー |
| `eccube-pluginmanager` | 全ライフサイクルメソッド付きPluginManager |
| `eccube-processor` | PurchaseFlowプロセッサー |
| `eccube-getrepo` | `$this->entityManager->getRepository(Entity::class)` |
| `eccube-config` | `$this->eccubeConfig['key']` |
| `eccube-qb` | Doctrine QueryBuilderテンプレート |

## Twigスニペット

| プレフィックス | 説明 |
|---|---|
| `eccube-extends` | `{% block main %}`付き`default_frame.html.twig`の継承 |
| `eccube-price` | `{{ value\|price }}` |
| `eccube-path` | `{{ path('route_name', {}) }}` |
| `eccube-asset` | `{{ asset('path') }}` |
| `eccube-form` | フルフォームレンダ―ブロック |
| `eccube-csrf` | CSRFトークン入力 + `form_widget(form._token)` |
| `eccube-raw` | `{{ variable\|raw }}` |
| `eccube-pagination` | KnpPaginatorページネーションリンク |

## YAMLスニペット

| プレフィックス | 説明 |
|---|---|
| `eccube-yaml-subscriber` | EventSubscriberサービス登録 |
| `eccube-yaml-twig-ext` | Twig拡張サービス登録 |
| `eccube-yaml-purchaseflow-cart` | 優先度付きCartFlowプロセッサー |
| `eccube-yaml-purchaseflow-order` | 優先度付きOrderFlowプロセッサー |

---

## サポートされているエンティティ

`Product` · `ProductClass` · `ProductImage` · `ProductTag` · `Tag` · `ProductCategory` · `Customer` · `CustomerAddress` · `Order` · `OrderItem` · `Shipping` · `Cart` · `CartItem` · `Category` · `News` · `BaseInfo` · `Page`

---

## 必要条件

- Visual Studio Code 1.80.0以降
- EC-CUBE 4.2または4.3プロジェクト

---

## 拡張機能の設定

| 設定 | デフォルト | 説明 |
|---|---|---|
| `eccubeIntellisense.enable` | `true` | 拡張機能の有効/無効 |
| `eccubeIntellisense.eccubeVersion` | `"auto"` | EC-CUBEバージョン: `4.2`、`4.3`、または`auto` |
| `eccubeIntellisense.enablePhpCompletion` | `true` | PHP補完を有効化 |
| `eccubeIntellisense.enableTwigCompletion` | `true` | Twig補完を有効化 |
| `eccubeIntellisense.enableYamlCompletion` | `true` | YAML補完を有効化 |
| `eccubeIntellisense.scanOnStartup` | `true` | 起動時にワークスペースをスキャン |

---

## 推奨拡張機能

- **Intelephense** — PHP言語サポートと静的解析
- **Twig Language 2** — Twigシンタックスハイライト

---

## ライセンス

MIT
