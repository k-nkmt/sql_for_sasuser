# その他基本操作

SASから利用することはほとんどないと思いますが、以下その他の基本操作を簡単に紹介します。

## データ操作関連

### INSERT

INSERT文は、データベースのテーブルに新しい行を追加するために使用されます。SELECT文で選択したものを追加、VALUESでは直接記載した値を追加ができます。

```sql
CREATE TABLE work.class LIKE sashelp.class ;

INSERT INTO work.class
SELECT * FROM sashelp.class
;

INSERT INTO work.class
VALUES ("Mary", "F", 15, 66.5, 112.0),
       ("William", "M", 15, 66.5, 112.0)
;
```

### UPDATE

UPDATE文はテーブルの既存のデータを変更するために使用されます。WHEREで対象を限定していなければ、列すべてが更新されます。

```sql
UPDATE　work.class
SET weight = weight+10 
;
UPDATE　work.class
SET age = 15 
WHERE name = 'Alfred'
;
```

### DELETE

DELETE文は、テーブルから特定の行を削除するために使用されます。WHEREで指定しなければすべてのレコードが削除されますが、その場合は以降で紹介するテーブルごとの削除も検討してください。

```
DELETE 
FROM work.class
WHERE Age = 15
;
```

## データ定義関連

### テーブル定義

各列の定義する形でテーブルを作成することができます。一般に文字型はvarchar, 数値型はintやfloatなどを指定します。また列に主キーやNot NULLなどの制約を設定することができます。

DESCRIBE句を使用すると指定したテーブルの定義を出力できます。

```sql
CREATE TABLE work.myclass (
   ID int PRIMARY KEY,
   Name varchar(50),
   Age int,
   Gender varchar(10)
);

DESCRIBE TABLE work.myclass ;
```

### DROP TABLE

DROP TABLE文は、テーブルを削除するために使用されます。

```sql
DROP TABLE work.myclass ;
```

### ALTER TABLE

ALTER TABLE文は、既存のデータベースのテーブルを変更するために使用されます。すでにレコードを含む状態で列を追加した場合、追加した列の値はnullになります。
列の定義変更はMODIFY句やALTER COLUMNとする場合など製品によって違うようなので、再作成も検討しながら注意して扱ってください。またレコードを含む状態では型の変換はできないものと考えておいたほうがいいと思います。

```sql
ALTER TABLE work.myclass
ADD weight2 num
;
ALTER TABLE work.myclass
MODIFY name char(60)
;
```

### インデックス

CREATE INDEX からまたはテーブル定義としてALTER TABLEからインデックスを設定することができます。 削除するときはCREATE句やADD句をDROPにします。

SASではデータステップやproc datasetsでもインデックスの設定は可能です。インデックスが設定されている場合、ソートがされていない状態でもBYグループ処理が可能です。

```
CREATE INDEX my_index ON work.class(age) ;
ALTER TABLE work.class ADD INDEX my_index(age) ;

data work.class(index=(age)) ;
  set sashelp.class ;
run ;
```

### ビュー

ビューはCREATE文でTABLEの代わりにVIEWを指定すれば作成することができます

```

CREATE VIEW work.class AS
SELECT age, sex,　Height, Weight
FROM sashelp.class
;

data work.class / view=work.class ;
   set sashelp.class ;
   drop name ;
run ;
    
```

## トランザクション

コードの詳細には触れませんが、pythonのコードの例だけ紹介しておきます。
複数の操作を一つのトランザクションとしてまとめることで、途中で何らかのエラーが発生した場合には全ての操作を取り消すことができます。また、トランザクションをまとめることで、複数のユーザーが同時に操作を行った場合でも、データの整合性を保つことができます。

```python
import sqlite3

# データベースに接続（メモリ上のデータベースを使用）
conn = sqlite3.connect(':memory:')
# SQLiteの自動トランザクションを無効にする
conn.isolation_level = None
# カーソルオブジェクトを作成
cursor = conn.cursor()

# 新しいテーブルを作成
cursor.execute('''
    CREATE TABLE accounts (
        id INTEGER PRIMARY KEY,
        name TEXT,
        balance REAL CHECK (balance >= 0)
    )
''')

# 初期データを挿入
cursor.execute('''
    INSERT INTO accounts (name, balance)
    VALUES ('Alice', 1000), ('Bob', 1500), ('Charlie', 300)
''')

# トランザクションを開始
conn.execute("BEGIN TRANSACTION;")

try:
    # 残高に一定の金額を足す
    cursor.execute("UPDATE accounts SET balance = balance + 500")
    # 残高から一定の金額を引く 制約条件により失敗する
    cursor.execute("UPDATE accounts SET balance = balance - 1000")
    # トランザクションをコミット
    conn.commit()
    print("Transaction committed successfully.")
except Exception as e:
    # エラーが発生した場合、トランザクションをロールバック
    conn.rollback()
    print("Transaction rolled back due to error:", e)

# 全てのレコードを取得
cursor.execute("SELECT * FROM accounts")
records = cursor.fetchall()

# 結果を表示
for record in records:
    print(record)

# カーソルとデータベース接続を閉じる
cursor.close()
conn.close()
```