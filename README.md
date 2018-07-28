# PostgreSQL

---

# クライアント

## DB接続

```
$ psql -U ユーザ名 -d データベース名
$ psql -h ホスト名 -U ユーザ名 -d データベース名
```

## URL

```
# SSL
postgres://<username>:<password>@<host>:<port>/<database>

# SSL Disable
postgres://<username>:<password>@<host>:<port>/<database>?sslmode=disable
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

```
production:
  <<: *default
  adapter: postgresql
  encoding: utf8
  database: postgres
  username: postgres
  password: test
  host: localhost
  port: 5432
```

---

# Docker

## コンテナ作成

https://hub.docker.com/_/postgres/

```
# sample
$ docker run --name some-app --link some-postgres:postgres -d application-that-uses-postgres
```

```
# postgres コンテナ作成
$ docker run --name postgres -e POSTGRES_PASSWORD=test -p 5432:5432 postgres

# コンテナへログイン
$ docker exec -ti postgres bash

# URL
postgres://postgres:test@xxx.xxx.xxx.xxx:5432/postgres?sslmode=disable
```

```
# config/database.yml
production:
  <<: *default
  adapter: postgresql
  encoding: utf8
  database: postgres
  username: postgres
  password: test
  host: localhost
  port: 5432
```

## データベース作成

http://www.kakiro-web.com/postgresql/postgresql-create-user.html

```
# データベース接続
$ psql -h localhost -U postgres 

# データベース作成
postgres=# create database test_database01;

# テーブル一覧
postgres=# \d
```

## select専用ユーザ作成

```
# データベース接続
$ psql -h localhost -U postgres -d test_database01

# ユーザ作成
postgres=# CREATE ROLE user01 LOGIN PASSWORD 'test';

# select権限の追加
postgres=# GRANT select ON users TO user01;
postgres=# GRANT select ON schema_migrations TO user01;

# データベース接続ユーザの一覧
postgres=# SELECT * FROM pg_authid;
postgres=# \du

# テーブル一覧
postgres=# \dp
```

## GRANT ALLユーザ作成

https://blog.longkey1.net/2013/02/13/how-to-grant-access-to-all-tables-of-a-database-in-postgres/

```
# スキーマを確認
postgres=# \d
                  List of relations
 Schema |         Name         |   Type   |  Owner
--------+----------------------+----------+----------
 public | ar_internal_metadata | table    | postgres
 public | schema_migrations    | table    | postgres
 public | users                | table    | postgres
 public | users_id_seq         | sequence | postgres
 
 # ユーザ追加
postgres=# CREATE ROLE user01 LOGIN PASSWORD 'test';

# 権限を追加
postgres=# GRANT ALL ON ALL TABLES IN SCHEMA public TO user01;
postgres=# GRANT ALL ON ALL SEQUENCES IN SCHEMA public TO user01;
postgres=# GRANT ALL ON ALL FUNCTIONS IN SCHEMA public TO user01;
```
