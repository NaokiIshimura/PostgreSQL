# PostgreSQL

---

# クライアント

## DB接続

```
$ psql -U ユーザ名 -d データベース名
$ psql -h ホスト名 -U ユーザ名 -d データベース名
```

---

# postgresユーザ

PostgreSQLをインストールするとシステムに追加されるpostgresユーザの操作

```
# postgresqlをインストールするとシステムに追加される
# postgresユーザのパスワードを変更する
$ sudo passwd postgres

# postgresユーザに切り替え
$ su - postgres

# DBユーザを追加
-bash-4.2$ createuser -P ユーザー名

# スーパーユーザを追加
-bash-4.2$ createuser --superuser -P ユーザー名

# Postgresqlに接続する
-bash-4.2$ psql
```

---

# DB

```
# データベースの作成
postgres=# create database データベース名;

# データベースの作成
postgres=# create database データベース名 owner ユーザー名;

# データベース一覧の確認
postgres=# \l

# テーブル一覧
postgres=# \d

# PostgreSQLから抜ける
postgres=# \q

# ユーザ一覧
postgres=# \du

# ユーザ一覧
postgres=# SELECT * FROM pg_shadow;
```

---

# インデックス

## インデックス作成

```
CREATE INDEX インデックス名 ON テーブル名 ( カラム名 );
CREATE INDEX インデックス名 ON テーブル名 ( カラム名１, カラム名２ );
```

## インデックス削除

```
DROP INDEX インデックス名;
```

## インデックス一覧

```
\di
```

## インデックス詳細

```
\d インデックス名
```

---

# その他

## カラム名取得

```
SELECT
    column_name
FROM
    information_schema.columns
WHERE
    table_name = 'テーブル名'
ORDER BY
    ordinal_position;
```

---

# 導入

## インストール

```
# インストール
$ sudo yum install -y postgresql-server

# バージョン
$ psql --version
```

## 準備

```
# 初期化
$ sudo service postgresql initdb

# サービス起動
$ service postgresql start

# 自動起動を設定
$ sudo systemctl enable postgresql

# 自動起動を停止
$ sudo systemctl disable postgresql

# 状態確認
$ sudo systemctl status postgresql
```

## 設定

```
# 認証設定を変更
$ sudo vi /var/lib/pgsql/data/pg_hba.conf

local   all             postgres                                peer
↓
local   all             postgres                                md5

$ sudo service postgresql stop
$ sudo service postgresql start
```

---

# Rails

```
$ sudo yum install -y postgresql-devel

$ vi Gemfile

gem 'pg'

$ bundle install
```
