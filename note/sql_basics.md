# SQL文の分類と基本

## SQL文の種類

### SASで利用する

### DML(Data Manipulation Language)

DMLは、データベース内のデータを操作するために使用されます。SELECT、INSERT、UPDATE、DELETEなどのコマンドがあります。

### DDL(Data Definition Language)

DDLは、データベースのスキーマを定義するために使用されます。DDLを使用することで、テーブル、列、インデックス、ビューなどを作成、変更、削除することができます。

### SASでは利用しない

<details><summary>DCL(Data Control Language)</summary>
DCLは、データベースのアクセスを制御するために使用されます。アクセス権を設定するGRANT、削除するREVOKEなどのコマンドがあります。
</details>

<details>
<summary>TCL(Transaction Control Language)</summary>
TCLは、トランザクションを管理するために使用されます。トランザクションでは複数の操作をまとめて実行することができます。COMMIT、ROLLBACKなどのコマンドがあります。 
</details>

この資料ではあまり触れませんが、データに関わる製品ではほぼデータベースが使われており、動作や制限の背景を理解するのには役に立つため、興味がある方は前で紹介しているようなデータベースからこれらも触ってみてください。

## 順序と記載ルール

各キーワードは後ほど紹介しますが、記載の順番と実行の順番は以下のように定められています。

**記載順序**

1. select 
2. from 
3. where
4. group by
5. having 
6. order by

**実行順序**

1. from
2. where
3. group by
4. having
5. select
6. order by

SASでは変数を複数指定する場合はスペースで区切りますが、SQLでは列を複数指定する場合にはカンマで区切ります。SQL文の末尾には一般に「;」が必要です。

**コメントアウト**

コメントアウトは1行と複数行の記法がありますが、1行のほうは製品で違うこともあり、SASと同じでもあるので複数行のほうがおすすめです。ただし、特定のプログラム言語下で使う場合はその言語の記法になると思いますので注意してください。

```sql
— 1行コメント
/* 
複数行コメント
*/
```
## 命名規則
製品によって異なるようで、詳細は利用する製品を確認してください。  
xptの命名規則であれば、以下の予約語をのぞき、ほぼ問題ないかと思います。

## 予約語

SQLの主な予約後を以下にあげておきます。なるべくSQL 99の語も避けておいたほうがいいと思います。製品によって独自のものもあるので、初めて扱う場合は注意しておくといいと思います。
<details>
<summary>一覧</summary>

| キーワード | SQL 92(proc sql) | SQL 99 |
| --- | --- | --- |
| ABSOLUTE | ○ | ○ |
| ACTION | ○ | ○ |
| ADD | ○ | ○ |
| ADMIN |  | ○ |
| AFTER |  | ○ |
| AGGREGATE |  | ○ |
| ALIAS |  | ○ |
| ALL | ○ | ○ |
| ALLOCATE | ○ | ○ |
| ALTER | ○ | ○ |
| AND | ○ | ○ |
| ANY | ○ | ○ |
| ARE | ○ | ○ |
| ARRAY |  | ○ |
| AS | ○ | ○ |
| ASC | ○ | ○ |
| ASSERTION | ○ | ○ |
| AT | ○ | ○ |
| AUTHORIZATION | ○ | ○ |
| BEFORE |  | ○ |
| BEGIN | ○ | ○ |
| BINARY |  | ○ |
| BIT | ○ | ○ |
| BLOB |  | ○ |
| BOOLEAN |  | ○ |
| BOTH | ○ | ○ |
| BREADTH |  | ○ |
| BY | ○ | ○ |
| CALL |  | ○ |
| CASCADE | ○ | ○ |
| CASCADED | ○ | ○ |
| CASE | ○ | ○ |
| CAST | ○ | ○ |
| CATALOG | ○ | ○ |
| CHAR | ○ | ○ |
| CHARACTER | ○ | ○ |
| CHECK | ○ | ○ |
| CLASS |  | ○ |
| CLOB |  | ○ |
| CLOSE | ○ | ○ |
| COLLATE | ○ | ○ |
| COLLATION | ○ | ○ |
| COLUMN | ○ | ○ |
| COMMIT | ○ | ○ |
| COMPLETION |  | ○ |
| CONNECT | ○ | ○ |
| CONNECTION | ○ | ○ |
| CONSTRAINT | ○ | ○ |
| CONSTRAINTS | ○ | ○ |
| CONSTRUCTOR |  | ○ |
| CONTINUE | ○ | ○ |
| CORRESPONDING | ○ | ○ |
| CREATE | ○ | ○ |
| CROSS | ○ | ○ |
| CUBE |  | ○ |
| CURRENT | ○ | ○ |
| CURRENT_DATE | ○ | ○ |
| CURRENT_PATH |  | ○ |
| CURRENT_ROLE |  | ○ |
| CURRENT_TIME | ○ | ○ |
| CURRENT_TIMESTAMP | ○ | ○ |
| CURRENT_USER | ○ | ○ |
| CURSOR | ○ | ○ |
| CYCLE |  | ○ |
| DATA |  | ○ |
| DATE | ○ | ○ |
| DAY | ○ | ○ |
| DEALLOCATE | ○ | ○ |
| DEC | ○ | ○ |
| DECIMAL | ○ | ○ |
| DECLARE | ○ | ○ |
| DEFAULT | ○ | ○ |
| DEFERRABLE | ○ | ○ |
| DEFERRED | ○ | ○ |
| DELETE | ○ | ○ |
| DEPTH |  | ○ |
| DEREF |  | ○ |
| DESC | ○ | ○ |
| DESCRIBE | ○ | ○ |
| DESCRIPTOR | ○ | ○ |
| DESTROY |  | ○ |
| DESTRUCTOR |  | ○ |
| DETERMINISTIC |  | ○ |
| DIAGNOSTICS | ○ | ○ |
| DICTIONARY |  | ○ |
| DISCONNECT | ○ | ○ |
| DISTINCT | ○ | ○ |
| DOMAIN | ○ | ○ |
| DOUBLE | ○ | ○ |
| DROP | ○ | ○ |
| DYNAMIC |  | ○ |
| DYNAMIC_FUNCTION_CODE |  | ○ |
| EACH |  | ○ |
| ELSE | ○ | ○ |
| END | ○ | ○ |
| END-EXEC | ○ | ○ |
| EQUALS |  | ○ |
| ESCAPE | ○ | ○ |
| EVERY |  | ○ |
| EXCEPT | ○ | ○ |
| EXCEPTION | ○ | ○ |
| EXEC | ○ | ○ |
| EXECUTE | ○ | ○ |
| EXTERNAL | ○ | ○ |
| FALSE | ○ | ○ |
| FETCH | ○ | ○ |
| FIRST | ○ | ○ |
| FLOAT | ○ | ○ |
| FOR | ○ | ○ |
| FOREIGN | ○ | ○ |
| FOUND | ○ | ○ |
| FREE |  | ○ |
| FROM | ○ | ○ |
| FULL | ○ | ○ |
| FUNCTION |  | ○ |
| GENERAL |  | ○ |
| GET | ○ | ○ |
| GLOBAL | ○ | ○ |
| GO | ○ | ○ |
| GOTO | ○ | ○ |
| GRANT | ○ | ○ |
| GROUP | ○ | ○ |
| GROUPING |  | ○ |
| HAVING | ○ | ○ |
| HOST |  | ○ |
| HOUR | ○ | ○ |
| IDENTITY | ○ | ○ |
| IGNORE |  | ○ |
| IMMEDIATE | ○ | ○ |
| IN | ○ | ○ |
| INDICATOR | ○ | ○ |
| INITIALIZE |  | ○ |
| INITIALLY | ○ | ○ |
| INNER | ○ | ○ |
| INOUT |  | ○ |
| INPUT | ○ | ○ |
| INSERT | ○ | ○ |
| INT | ○ | ○ |
| INTEGER | ○ | ○ |
| INTERSECT | ○ | ○ |
| INTERVAL | ○ | ○ |
| INTO | ○ | ○ |
| IS | ○ | ○ |
| ISOLATION | ○ | ○ |
| ITERATE |  | ○ |
| JOIN | ○ | ○ |
| KEY | ○ | ○ |
| LANGUAGE | ○ | ○ |
| LARGE |  | ○ |
| LAST | ○ | ○ |
| LATERAL |  | ○ |
| LEADING | ○ | ○ |
| LEFT | ○ | ○ |
| LESS |  | ○ |
| LEVEL | ○ | ○ |
| LIKE | ○ | ○ |
| LIMIT |  | ○ |
| LOCAL | ○ | ○ |
| LOCALTIME |  | ○ |
| LOCALTIMESTAMP |  | ○ |
| LOCATOR |  | ○ |
| MAP |  | ○ |
| MATCH | ○ | ○ |
| MINUTE | ○ | ○ |
| MODIFIES |  | ○ |
| MODIFY |  | ○ |
| MODULE | ○ | ○ |
| MONTH | ○ | ○ |
| NAMES | ○ | ○ |
| NATIONAL | ○ | ○ |
| NATURAL | ○ | ○ |
| NCHAR | ○ | ○ |
| NCLOB |  | ○ |
| NEW |  | ○ |
| NEXT | ○ | ○ |
| NO | ○ | ○ |
| NONE |  | ○ |
| NOT | ○ | ○ |
| NULL | ○ | ○ |
| NUMERIC | ○ | ○ |
| OBJECT |  | ○ |
| OF | ○ | ○ |
| OFF |  | ○ |
| OLD |  | ○ |
| ON | ○ | ○ |
| ONLY | ○ | ○ |
| OPEN | ○ | ○ |
| OPERATION |  | ○ |
| OPTION | ○ | ○ |
| OR | ○ | ○ |
| ORDER | ○ | ○ |
| ORDINALITY |  | ○ |
| OUT |  | ○ |
| OUTER | ○ | ○ |
| OUTPUT | ○ | ○ |
| PAD | ○ | ○ |
| PARAMETER |  | ○ |
| PARAMETERS |  | ○ |
| PARTIAL | ○ | ○ |
| PATH |  | ○ |
| POSTFIX |  | ○ |
| PRECISION | ○ | ○ |
| PREFIX |  | ○ |
| PREORDER |  | ○ |
| PREPARE | ○ | ○ |
| PRESERVE | ○ | ○ |
| PRIMARY | ○ | ○ |
| PRIOR | ○ | ○ |
| PRIVILEGES | ○ | ○ |
| PROCEDURE | ○ | ○ |
| PUBLIC | ○ | ○ |
| READ | ○ | ○ |
| READS |  | ○ |
| REAL | ○ | ○ |
| RECURSIVE |  | ○ |
| REF |  | ○ |
| REFERENCES | ○ | ○ |
| REFERENCING |  | ○ |
| RELATIVE | ○ | ○ |
| RESTRICT | ○ | ○ |
| RESULT |  | ○ |
| RETURN |  | ○ |
| RETURNED_LENGTH | ○ | ○ |
| RETURNS |  | ○ |
| REVOKE | ○ | ○ |
| RIGHT | ○ | ○ |
| ROLE |  | ○ |
| ROLLBACK | ○ | ○ |
| ROLLUP |  | ○ |
| ROUTINE |  | ○ |
| ROW |  | ○ |
| ROWS | ○ | ○ |
| SAVEPOINT |  | ○ |
| SCHEMA | ○ | ○ |
| SCOPE |  | ○ |
| SCROLL | ○ | ○ |
| SEARCH |  | ○ |
| SECOND | ○ | ○ |
| SECTION | ○ | ○ |
| SELECT | ○ | ○ |
| SEQUENCE |  | ○ |
| SESSION | ○ | ○ |
| SESSION_USER | ○ | ○ |
| SET | ○ | ○ |
| SETS |  | ○ |
| SIZE | ○ | ○ |
| SMALLINT | ○ | ○ |
| SOME | ○ | ○ |
| SPACE | ○ | ○ |
| SPECIFIC |  | ○ |
| SPECIFICTYPE |  | ○ |
| SQL | ○ | ○ |
| SQLEXCEPTION |  | ○ |
| SQLSTATE | ○ | ○ |
| SQLWARNING |  | ○ |
| START |  | ○ |
| STATE |  | ○ |
| STATEMENT |  | ○ |
| STATIC |  | ○ |
| STRUCTURE |  | ○ |
| SYSTEM_USER | ○ | ○ |
| TABLE | ○ | ○ |
| TEMPORARY | ○ | ○ |
| TERMINATE |  | ○ |
| THAN |  | ○ |
| THEN | ○ | ○ |
| TIME | ○ | ○ |
| TIMESTAMP | ○ | ○ |
| TIMEZONE_HOUR | ○ | ○ |
| TIMEZONE_MINUTE | ○ | ○ |
| TO | ○ | ○ |
| TRAILING | ○ | ○ |
| TRANSACTION | ○ | ○ |
| TRANSLATION | ○ | ○ |
| TREAT |  | ○ |
| TRIGGER |  | ○ |
| TRUE | ○ | ○ |
| UNDER |  | ○ |
| UNION | ○ | ○ |
| UNIQUE | ○ | ○ |
| UNKNOWN | ○ | ○ |
| UNNEST |  | ○ |
| UPDATE | ○ | ○ |
| USAGE | ○ | ○ |
| USER | ○ | ○ |
| USING | ○ | ○ |
| VALUE | ○ | ○ |
| VALUES | ○ | ○ |
| VARCHAR | ○ | ○ |
| VARIABLE |  | ○ |
| VARYING | ○ | ○ |
| VIEW | ○ | ○ |
| WHEN | ○ | ○ |
| WHENEVER | ○ | ○ |
| WHERE | ○ | ○ |
| WITH | ○ | ○ |
| WITHOUT |  | ○ |
| WORK | ○ | ○ |
| WRITE | ○ | ○ |
| YEAR | ○ | ○ |
| ZONE | ○ | ○ |

</details>