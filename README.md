## 概要

Docker環境構築
PHP8・Laravel8・Nginx・MySQL

## 手順
### プロジェクトの立ち上げ

```bash
# プロジェクトをダウンロード
git pull https://github.com/4masa6/docker-laravel.git

cd docker-laravel

# ビルド
docker-compose build

# コンテナ起動
docker-compose up -d

# コンテナに入る
docker-compose exec php /bin/bash

# Laravelプロジェクト作成
composer create-project --prefer-dist laravel/laravel src "8.*"

# .envファイルを作成
cd src
cp .env.example .env
php artisan key:generate
```

URL
`http://127.0.0.1:8000/`

### DBとの接続

- phpMyAdminに接続 `http://127.0.0.1:8086/`
- 上部「DB」タブより、下記SQLを実行
```sql
CREATE DATABASE <データベース名> DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci
```

- .envファイルを修正
```plain:.env
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=<データベース名>
DB_USERNAME=root
DB_PASSWORD=password
```

- phpコンテナに入り、マイグレーションを実行
```bash
docker-compose exec php /bin/bash
cd src
php artisan migrate
```

### src/.gitignore 編集
下記を追記
```
/public/css/app.css
/public/js/app.js
```

### LaravelBreezeのインストール

```bash
docker-compose exec php /bin/bash
cd src
composer require laravel/breeze "1.*" --dev
php artisan breeze:install
npm install && npm run dev
```
