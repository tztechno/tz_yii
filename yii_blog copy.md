
---

cf. https://app.wftutorials.com/tutorial/144

---

Yiiフレームワークを使用してブログアプリケーションを作成する手順を説明します。以下の手順に従って、基本的なブログ機能を持つアプリケーションを構築します。

### 1. Yii2プロジェクトの作成

まず、Composerを使用してYii2プロジェクトを作成します。

```bash
composer create-project --prefer-dist yiisoft/yii2-app-basic blog
cd blog
```

### 2. データベースの設定

`config/db.php`ファイルを開き、データベース接続情報を設定します。

```php
return [
    'class' => 'yii\db\Connection',
    'dsn' => 'mysql:host=localhost;dbname=blog',
    'username' => 'root',
    'password' => '',
    'charset' => 'utf8',
];
```

### 3. マイグレーションファイルの作成

`migrations`ディレクトリを手動で作成し、マイグレーションファイルを生成します。

```bash
mkdir migrations
php yii migrate/create create_post_table
```
$ migrationフォルダの中は空、TODO作成時の同一問題、ここで中断

生成されたマイグレーションファイルを開き、以下のコードを追加します。

```php
<?php

use yii\db\Migration;

/**
 * Handles the creation of table `{{%post}}`.
 */
class m{timestamp}_create_post_table extends Migration
{
    /**
     * {@inheritdoc}
     */
    public function safeUp()
    {
        $this->createTable('{{%post}}', [
            'id' => $this->primaryKey(),
            'title' => $this->string()->notNull(),
            'content' => $this->text()->notNull(),
            'status' => $this->boolean()->defaultValue(true),
            'created_at' => $this->integer()->notNull(),
            'updated_at' => $this->integer()->notNull(),
        ]);
    }

    /**
     * {@inheritdoc}
     */
    public function safeDown()
    {
        $this->dropTable('{{%post}}');
    }
}
```

### 4. マイグレーションの実行

以下のコマンドを実行して、データベースにテーブルを作成します。

```bash
php yii migrate
```

### 5. モデルの作成

Giiを使用してモデルを生成します。まず、Giiモジュールを有効にします。`config/web.php`ファイルを編集して、Giiモジュールを追加します。

```php
if (YII_ENV_DEV) {
    // configuration adjustments for 'dev' environment
    $config['bootstrap'][] = 'gii';
    $config['modules']['gii'] = [
        'class' => 'yii\gii\Module',
    ];
}
```

ブラウザで`http://localhost/blog/web/index.php?r=gii`にアクセスし、Giiを使用して`Post`モデルを生成します。

### 6. CRUDの作成

Giiを使用してCRUD操作を行うためのコードを生成します。

ブラウザで`http://localhost/blog/web/index.php?r=gii`に再度アクセスし、CRUD Generatorを使用して`Post`モデルのCRUD操作を生成します。

### 7. ルーティングの設定

`config/web.php`ファイルでルーティングを設定します。

```php
'components' => [
    'urlManager' => [
        'enablePrettyUrl' => true,
        'showScriptName' => false,
        'rules' => [
            'post' => 'post/index',
            'post/create' => 'post/create',
            'post/update/<id:\d+>' => 'post/update',
            'post/delete/<id:\d+>' => 'post/delete',
            'post/view/<id:\d+>' => 'post/view',
        ],
    ],
],
```

### 8. ビューのカスタマイズ

生成されたビューをカスタマイズして、ブログの外観や動作を調整します。`views/post`フォルダ内のファイルを編集して、必要なUIやデザインを追加します。

### 9. ウィジェットの追加

ブログの機能を拡張するために、コメント機能やカテゴリ、タグなどのウィジェットを追加できます。

### 10. アプリケーションのテスト

ブラウザでアプリケーションを開き、動作を確認します。

```
http://localhost/blog/web/index.php?r=post
```

### まとめ

これで、Yiiフレームワークを使用して基本的なブログアプリケーションを作成する手順が完了です。以下の追加機能を実装することで、さらに充実したブログアプリケーションにすることができます。

- **コメント機能**: ブログポストにコメントを追加できるようにする。
- **ユーザー管理**: 複数のユーザーがブログに投稿できるようにする。
- **カテゴリーとタグ**: 投稿をカテゴリーやタグで整理できるようにする。
- **検索機能**: ブログ内の投稿を検索できるようにする。

これらの機能を追加することで、より使い勝手の良いブログアプリケーションを構築できます。