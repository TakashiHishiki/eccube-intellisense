# EC-CUBE IntelliSense

EC-CUBE 4.x 開発向けの Visual Studio Code 拡張機能です。  
Twig テンプレート・PHP エンティティ・YAML サービス設定に対して、コード補完・引数ヒント・ホバードキュメント・定義ジャンプを提供します。

[![Visual Studio Marketplace](https://img.shields.io/visual-studio-marketplace/v/colscenery.eccube-intellisense?label=VS%20Marketplace)](https://marketplace.visualstudio.com/items?itemName=colscenery.eccube-intellisense)

**[Visual Studio Marketplace で入手する](https://marketplace.visualstudio.com/items?itemName=colscenery.eccube-intellisense)**

---

## 機能

### PHP

**コード補完**

- `$entity->` と入力すると、型推論に基づいてエンティティのメソッド・プロパティ候補を表示します。
- 各候補にはメソッドシグネチャ（引数名・型・戻り値）と説明が表示されます。
- 型推論は `@var` アノテーション・コンストラクタ引数・`new ClassName()`・`foreach` ループに対応しています。

**引数ヒント（Signature Help）**

- `$entity->methodName(` と入力するとポップアップが表示され、現在カーソルがある引数の名前・型・説明が強調表示されます。

**ホバードキュメント**

- `$entity` 変数や `->methodName` の上にマウスを置くと、型情報・メソッドシグネチャ・引数の説明・戻り値の型がポップアップ表示されます。

**その他の PHP 補完**

- `->getRepository(` — EC-CUBE エンティティの `::class` 候補を表示します。
- `getSubscribedEvents()` 内のイベント定数 — EC-CUBE イベント定数の候補を表示します。
- `path()`・`url()`・`redirectToRoute()` 内のルート名 — ワークスペースのルート名を補完します。
- `@var`・`use`・型ヒント位置での FQCN — EC-CUBE エンティティ・サービスの完全修飾クラス名を補完します。
- `eccubeConfig[` — よく使われる設定キーとその説明を補完します。

---

### Twig

**ドットアクセス補完**

- `{{ product.` や `{% if order.` のようにドットを入力すると、エンティティのメソッド・プロパティ候補が表示されます。
- 各候補には引数名・型・戻り値・説明が表示されます。
- `app/Customize/Entity` に配置したカスタムエンティティのメソッドも補完対象になります。

**エンティティ名補完**

- `{{ Cu` のように入力すると `CustomizeProduct` などのエンティティクラス名が候補として表示されます。

**引数ヒント（Signature Help）**

- Twig 関数（例：`asset(`、`path(`）を入力すると引数ヒントが表示されます。
- エンティティメソッドの呼び出し（例：`product.setName(`）でも引数ヒントが表示されます。

**ホバードキュメント**

- Twig 関数・フィルタ・タグ・変数・エンティティメソッドの上にマウスを置くと詳細な説明がポップアップ表示されます。

**定義ジャンプ（Go to Definition）**

- Twig ファイル内でエンティティのメソッド名を `Ctrl+クリック`（Mac: `Cmd+クリック`）すると、そのメソッドが定義されている PHP ファイルが別タブで開きます。
- 例：`{{ product.getName }}` の `getName` を Ctrl+クリック → `src/Eccube/Entity/Product.php` の `getName` メソッド定義行へジャンプします。
- `app/Customize` および `app/Plugin` 以下のカスタムエンティティにも対応しています。

**その他の Twig 補完**

- `{% for item in ... %}` で定義されたループ変数の型推論と補完。
- `|` 入力後にフィルタ候補を表示。
- `path(` / `url(` 内でルート名を補完。
- `extends` / `include` 内でテンプレートパスを補完。
- `{% %}` 内で Twig タグキーワードを補完。

**右クリックメニュー**

- Twig / PHP エディタ上で右クリックすると、`EC-CUBE: IntelliSense` サブメニューが表示されます。
- `EC-CUBE: Rescan Entities` で `app/Customize/Entity` を再スキャンできます。
- 補完が更新されない場合は、`EC-CUBE: Reload Window` でウィンドウ再読み込みを実行できます。

---

### YAML（`services.yaml`）

- `name:` の後に EC-CUBE サービスタグ名を補完します（例：`eccube.event_subscriber`）。
- サービスクラス名（4スペースインデント）の補完。
- `priority:` の後に PurchaseFlow プロセッサの推奨優先度範囲を補完します。

---

### スニペット

コマンドパレット（`Ctrl+Shift+P` / `Cmd+Shift+P`）から `EC-CUBE: Generate Boilerplate` を実行すると、以下のボイラープレートを生成できます。

- **Entity Class** — EC-CUBE エンティティクラスのテンプレート
- **Repository Class** — リポジトリクラスのテンプレート
- **EventSubscriber** — イベントサブスクライバのテンプレート
- **FormType** — フォームタイプのテンプレート
- **Controller** — コントローラのテンプレート
- **Plugin PluginManager** — プラグインマネージャのテンプレート
- **PurchaseFlow Processor** — PurchaseFlow プロセッサのテンプレート

---

## 型推論

以下のパターンから変数の型を推論します。

- `@var EntityName $variable` — PHPDoc アノテーション
- `function foo(EntityName $variable)` — 関数・コンストラクタの引数型宣言
- `$variable = new EntityName()` — インスタンス生成
- `foreach ($collection as $item)` — foreach ループ変数
- `$result = $variable->getRelation()` — メソッド戻り値チェーン

---

## 動作要件

- Visual Studio Code `1.118.0` 以上
- EC-CUBE `4.2` または `4.3`

---

## インストール

**マーケットプレイスからインストール（推奨）:**

[Visual Studio Marketplace で入手する](https://marketplace.visualstudio.com/items?itemName=colscenery.eccube-intellisense)

**VSIX からインストール:**

1. リリースページから `.vsix` ファイルをダウンロードします。
2. VS Code の拡張機能パネルを開きます（`Ctrl+Shift+X` / `Cmd+Shift+X`）。
3. 右上の `...` メニューから「VSIX からインストール...」を選択します。
4. ダウンロードした `.vsix` ファイルを選択します。

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

## リリースノート

### 0.0.5

- カスタムエンティティ（`app/Customize/Entity`）のメソッド情報を Twig 補完に反映する処理を改善。
- インデントされた `class` 宣言を持つカスタムエンティティを正しく検出できるよう修正。
- Twig 内でエンティティクラス名（例：`CustomizeProduct`）を直接補完できる機能を追加。
- ホバー・引数ヒント・定義ジャンプにもカスタムエンティティ情報を反映。
- 右クリックに `EC-CUBE: IntelliSense` サブメニューを追加。
- サブメニューから `EC-CUBE: Rescan Entities` と `EC-CUBE: Reload Window` を実行可能に変更。

### 0.0.4

- **新機能**: Twig の定義ジャンプ — `{{ product.getName }}` などのメソッド名を Ctrl+クリックすると、対応する PHP エンティティファイルのメソッド定義行が別タブで開きます。ワークスペース内のカスタムエンティティにも対応。
- **改善**: PHP ホバードキュメント — `$variable` やメソッド名の上にマウスを置くと、型情報・引数の説明・戻り値の型が表示されるようになりました。
- **改善**: コード補完候補のドキュメントに引数の詳細説明（名前・型・説明）を表示。
- **改善**: PHP・Twig の引数ヒントポップアップに引数ごとの説明を表示。
- **改善**: Twig ホバードキュメントにエンティティメソッドの引数詳細説明を表示。

### 0.0.3

- Twig の foreach ループ型推論とドットアクセス補完。
- PHP エンティティ補完・引数ヒント・イベント/ルート/FQCN 補完。
- YAML サービスタグ・優先度補完。
- ボイラープレート生成コマンド。

---

## ライセンス

MIT

---

## リポジトリ

[https://github.com/TakashiHishiki/eccube-intellisense](https://github.com/TakashiHishiki/eccube-intellisense)
