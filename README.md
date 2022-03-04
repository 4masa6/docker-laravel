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

- phpMyAdminの設定

```bash
cp .env.example .env
```
```plain:/.env
# 後述の/src/.envのMySQL情報と合わせる
PMA_ARBITRARY=1
PMA_HOST=
PMA_USER=
PMA_PASSWORD=
```

- phpMyAdminにアクセス `http://127.0.0.1:8086/`
- 上部「DB」タブより、下記SQLを実行
```sql
CREATE DATABASE <データベース名> DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci
```

- .envファイルを修正
```plain:/src/.env
DB_CONNECTION=mysql
DB_HOST=db # 必要であれば設定し変更
DB_PORT=3306
DB_DATABASE=<データベース名（snake_case）>
DB_USERNAME=root # 必要であれば設定し変更
DB_PASSWORD=password # 必要であれば設定し変更
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
