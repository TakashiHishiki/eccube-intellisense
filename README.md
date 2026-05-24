# EC-CUBE IntelliSense

EC-CUBE 4.x 開発向けの Visual Studio Code 拡張機能です。  
Twig テンプレート・PHP エンティティ・PHP カスタマイズクラス・YAML サービス設定に対して、コード補完・引数ヒント・ホバードキュメント・定義ジャンプを提供します。

**[Visual Studio Code 拡張機能マーケットプレイスでインストール](https://marketplace.visualstudio.com/items?itemName=colscenery.eccube-intellisense)**

---

## インストール

VSCode のクイックオープン（`Ctrl+P`）を開き、以下を貼り付けて Enter を押してください。

```
ext install colscenery.eccube-intellisense
```

または拡張機能マーケットプレイスで `EC-CUBE IntelliSense` を検索してインストールできます。

---

## 機能

### PHP

**コード補完**

- `$entity->` と入力すると、型推論に基づいてエンティティのメソッド・プロパティ候補を表示します。
- `/app/Customize/` 内のカスタムクラス（Repository・Entity・Service など）のメソッド・プロパティ補完に対応しています。
- 各候補にはメソッドシグネチャ（引数名・型・戻り値）と説明が表示されます。
- 型推論は `@var` アノテーション・コンストラクタ引数・`new ClassName()`・`foreach` ループに対応しています。

**引数ヒント（Signature Help）**

- `$entity->methodName(` と入力するとポップアップが表示され、現在カーソルがある引数の名前・型・説明が強調表示されます。

**ホバードキュメント**

- `$entity` 変数や `->methodName` の上にマウスを置くと、型情報・メソッドシグネチャ・引数の説明・戻り値の型がポップアップ表示されます。

**その他のPHP補完**

- `->getRepository(` — EC-CUBE エンティティの `::class` 候補を表示します。
- `/app/Customize/Repository/` — カスタムリポジトリメソッドの補完を提供します。
- `/app/Customize/Entity/` — カスタムエンティティメソッド・プロパティの補完を提供します。
- `/app/Customize/Service/` — カスタムサービスメソッドの補完を提供します。
- `getSubscribedEvents()` 内のイベント定数 — EC-CUBE イベント定数の候補を表示します。
- `path()`・`url()`・`redirectToRoute()` 内のルート名 — ワークスペースのルート名を補完します。
- `@var`・`use`・型ヒント位置での FQCN — EC-CUBE エンティティ・サービス・カスタムクラスの完全修飾クラス名を補完します。
- `eccubeConfig[` — よく使われる設定キーとその説明を補完します。

---

### Twig

**ドットアクセス補完**

- `{{ product.` や `{% if order.` のようにドットを入力すると、エンティティのメソッド・プロパティ候補が表示されます。
- `/app/Customize/` 内のカスタムクラスメソッド・プロパティの補完にも対応しています。
- 各候補には引数名・型・戻り値・説明が表示されます。

**引数ヒント（Signature Help）**

- Twig 関数（例：`asset(`、`path(`）を入力すると引数ヒントが表示されます。
- エンティティメソッドの呼び出し（例：`product.setName(`）でも引数ヒントが表示されます。

**ホバードキュメント**

- Twig 関数・フィルタ・タグ・変数・エンティティメソッドの上にマウスを置くと詳細な説明がポップアップ表示されます。

**定義ジャンプ（Go to Definition）**

- Twig ファイル内でエンティティのメソッド名を `Ctrl+クリック`（Mac: `Cmd+クリック`）すると、そのメソッドが定義されている PHP ファイルが別タブで開きます。
- 例：`{{ product.getName }}` の `getName` を Ctrl+クリック → `src/Eccube/Entity/Product.php` の `getName` メソッド定義行へジャンプします。
- `/app/Customize/` 内のカスタムクラスメソッドの定義ジャンプにも対応しています。

**その他のTwig補完**

- `{% for item in ... %}` で定義されたループ変数の型推論と補完。
- `|` 入力後にフィルタ候補を表示。
- `path(` / `url(` 内でルート名を補完。
- `extends` / `include` 内でテンプレートパスを補完。
- `{% %}` 内で Twig タグキーワードを補完。

**右クリックメニュー**

- Twig / PHP エディタ上で右クリックすると、`EC-CUBE: IntelliSense` サブメニューが表示されます。
- サブメニュー内の `EC-CUBE: Rescan Customize` で `app/Customize/` 配下のカスタムクラスを再スキャンできます。
- 補完が更新されない場合は、`EC-CUBE: Reload Window` でウィンドウ再読み込みを実行できます。

---

### YAML（`services.yaml`）

- `name:` の後に EC-CUBE サービスタグ名を補完します（例：`eccube.event_subscriber`）。
- サービスクラス名（4スペースインデント）の補完。
- `priority:` の後に PurchaseFlow プロセッサの推奨優先度範囲を補完します。

---

### スニペット

コマンドパレット（`Ctrl+Shift+P` / `Cmd+Shift+P`）から `EC-CUBE: Generate Boilerplate` を実行すると、以下のボイラープレートを生成できます。

#### PHP スニペット

| プレフィックス | 説明 |
|---|---|
| `eccube-entity` | EC-CUBE Entity クラス（Doctrine ORM アノテーション付き） |
| `eccube-repository` | `ServiceEntityRepository` を継承した Repository クラス |
| `eccube-subscriber` | `getSubscribedEvents()` 付き EventSubscriber |
| `eccube-formtype` | FormType クラス |
| `eccube-controller` | `@Route` アノテーション付き Controller |
| `eccube-pluginmanager` | 全ライフサイクルメソッド付き Plugin PluginManager |
| `eccube-processor` | `ItemHolderPreprocessor` を実装した PurchaseFlow Processor |
| `eccube-getrepo` | `$this->entityManager->getRepository(Entity::class)` |
| `eccube-config` | `$this->eccubeConfig['key']` |
| `eccube-qb` | Doctrine QueryBuilder テンプレート |

#### Twig スニペット

| プレフィックス | 説明 |
|---|---|
| `eccube-extends` | `default_frame.html.twig` を継承した `{% block main %}` |
| `eccube-price` | 価格フィルタ：`{{ value\|price }}` |
| `eccube-path` | `{{ path('route_name', {}) }}` |
| `eccube-asset` | `{{ asset('path') }}` |
| `eccube-for-pagination` | 空状態付きのページネーション foreach ループ |
| `eccube-product-image` | `no_image_product` フィルタ付き商品画像タグ |
| `eccube-form` | `form_start`・`form_row`・`form_rest`・`form_end` を含む完全なフォーム |
| `eccube-csrf` | CSRF トークン hidden input |
| `eccube-block-*` | EC-CUBE ブロックヘルパー（logo・category・cart） |
| `eccube-pagination` | KnpPaginator ページネーションリンク |

#### YAML スニペット

| プレフィックス | 説明 |
|---|---|
| `eccube-yaml-subscriber` | EventSubscriber サービス登録 |
| `eccube-yaml-twig-ext` | Twig Extension サービス登録 |
| `eccube-yaml-purchaseflow-cart` | 優先度付き CartFlow プロセッサ登録 |
| `eccube-yaml-purchaseflow-order` | 優先度付き OrderFlow プロセッサ登録 |
| `eccube-yaml-formtype-ext` | Form Type Extension 登録 |

---

## 型推論

以下のパターンから変数の型を推論します。

- `@var EntityName $variable` — PHPDoc アノテーション
- `@var CustomClassName $variable` — カスタムクラス型推論（PHPDoc）
- `function foo(EntityName $variable)` — 関数・コンストラクタの引数型宣言
- `$variable = new EntityName()` — インスタンス生成
- `$variable = new CustomClassName()` — カスタムクラスのインスタンス生成
- `foreach ($collection as $item)` — foreach ループ変数
- `$result = $variable->getRelation()` — メソッド戻り値チェーン

---

## 動作要件

- Visual Studio Code `1.118.0` 以上
- EC-CUBE `4.2` または `4.3`

---

## 設定

| 設定項目 | デフォルト | 説明 |
|---|---|---|
| `eccubeIntellisense.enable` | `true` | 拡張機能の有効/無効 |
| `eccubeIntellisense.eccubeVersion` | `"auto"` | EC-CUBE バージョン（`"4.2"` / `"4.3"` / `"auto"`） |
| `eccubeIntellisense.enablePhpCompletion` | `true` | PHP IntelliSense の有効/無効 |
| `eccubeIntellisense.enableTwigCompletion` | `true` | Twig IntelliSense の有効/無効 |
| `eccubeIntellisense.enableYamlCompletion` | `true` | YAML IntelliSense の有効/無効 |
| `eccubeIntellisense.scanOnStartup` | `true` | 起動時のワークスペーススキャンの有効/無効 |

---

## おすすめの組み合わせ

以下の拡張機能と併用すると、より快適に開発できます。

- **[Intelephense](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client)** — PHP 静的解析・言語サポート
- **[Twig Language 2](https://marketplace.visualstudio.com/items?itemName=mblode.twig-language-2)** — Twig シンタックスハイライト

---

## リリースノート

### 0.0.7

- **変更**: `EC-CUBE: Rescan Entities` コマンドを `EC-CUBE: Rescan Customize` に改名。
- **変更**: 再スキャンコマンドのスキャン対象を `app/Customize/` のみに限定。`src/Eccube/` やプラグインディレクトリを除外することで、カスタマイズクラスの変更だけを素早く反映できるようになりました。

### 0.0.6

- **新機能**: `/app/Customize/` 内のカスタムクラス（Repository・Entity・Service など）のメソッド・プロパティ補完機能を追加。
- PHP での `$variable->` ドット補完がカスタムクラスをサポート。
- Twig での `{{ variable.` ドット補完がカスタムクラスをサポート。
- カスタムクラスメソッドの定義ジャンプ（Go to Definition）に対応。
- 型推論にカスタムクラス型推論を追加。
- ワークスペーススキャンで `/app/Customize/` 配下のすべてのクラスを自動検出。

### 0.0.5

- カスタムエンティティ（`app/Customize/Entity`）のメソッド情報を Twig 補完に反映する処理を改善。
- 右クリックに `EC-CUBE: IntelliSense` サブメニューを追加。
- サブメニューから `EC-CUBE: Rescan Customize` と `EC-CUBE: Reload Window` を実行可能に変更。

### 0.0.4

- **新機能**: Twig の定義ジャンプ — `{{ product.getName }}` などのメソッド名を Ctrl+クリックすると、対応する PHP エンティティファイルのメソッド定義行が別タブで開きます。
- **改善**: PHP ホバードキュメント — 型情報・引数の説明・戻り値の型を表示。
- **改善**: コード補完候補のドキュメントに引数の詳細説明を表示。
- **改善**: PHP・Twig の引数ヒントポップアップに引数ごとの説明を表示。

### 0.0.3

- Twig の foreach ループ型推論とドットアクセス補完。
- PHP エンティティ補完・引数ヒント・イベント/ルート/FQCN 補完。
- YAML サービスタグ・優先度補完。
- ボイラープレート生成コマンド。

---

## ライセンス

MIT

---

## コントリビューション

Issue や Pull Request は歓迎します。  
[https://github.com/TakashiHishiki/eccube-intellisense](https://github.com/TakashiHishiki/eccube-intellisense)
