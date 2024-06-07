Yiiフレームワークを使用してToDoリストアプリケーションを作成するための詳細な手順を説明します。以下の手順に従ってください。

### 1. Yii2プロジェクトの作成

まず、Composerを使用してYii2プロジェクトを作成します。

```bash
composer create-project --prefer-dist yiisoft/yii2-app-basic todolist
cd todolist
```

### 2. データベースの設定

`config/db.php`ファイルを開き、データベース接続情報を設定します。

```php
return [
    'class' => 'yii\db\Connection',
    'dsn' => 'mysql:host=localhost;dbname=todolist',
    'username' => 'root',
    'password' => '',
    'charset' => 'utf8',
];
```

### 3. マイグレーションファイルの作成

`migrations`ディレクトリを手動で作成し、マイグレーションファイルを生成します。

```bash
mkdir migrations
php yii migrate/create create_todo_table
```

生成されたマイグレーションファイルを開き、以下のコードを追加します。

```php
<?php

use yii\db\Migration;

/**
 * Handles the creation of table `{{%todo}}`.
 */
class m{timestamp}_create_todo_table extends Migration
{
    /**
     * {@inheritdoc}
     */
    public function safeUp()
    {
        $this->createTable('{{%todo}}', [
            'id' => $this->primaryKey(),
            'title' => $this->string()->notNull(),
            'description' => $this->text(),
            'status' => $this->boolean()->defaultValue(false),
            'created_at' => $this->integer()->notNull(),
            'updated_at' => $this->integer()->notNull(),
        ]);
    }

    /**
     * {@inheritdoc}
     */
    public function safeDown()
    {
        $this->dropTable('{{%todo}}');
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

ブラウザで`http://localhost/todolist/web/index.php?r=gii`にアクセスし、Giiを使用して`Todo`モデルを生成します。

### 6. CRUDの作成

Giiを使用してCRUD操作を行うためのコードを生成します。

ブラウザで`http://localhost/todolist/web/index.php?r=gii`に再度アクセスし、CRUD Generatorを使用して`Todo`モデルのCRUD操作を生成します。

### 7. ルーティングの設定

`config/web.php`ファイルでルーティングを設定します。

```php
'components' => [
    'urlManager' => [
        'enablePrettyUrl' => true,
        'showScriptName' => false,
        'rules' => [
            'todo' => 'todo/index',
            'todo/create' => 'todo/create',
            'todo/update/<id:\d+>' => 'todo/update',
            'todo/delete/<id:\d+>' => 'todo/delete',
            'todo/view/<id:\d+>' => 'todo/view',
        ],
    ],
],
```

### 8. ビューのカスタマイズ

生成されたビューをカスタマイズして、ToDoリストの外観や動作を調整します。`views/todo`フォルダ内のファイルを編集して、必要なUIやデザインを追加します。

### 9. アプリケーションのテスト

ブラウザでアプリケーションを開き、動作を確認します。

```
http://localhost/todolist/web/index.php?r=todo
```

### まとめ

これで、Yiiフレームワークを使用して基本的なToDoリストアプリケーションを作成する手順が完了です。追加の機能やカスタマイズを行って、さらに充実したアプリケーションに仕上げることができます。
