# 新規テーブル・列作成

## 新規テーブルの作成

### CREATE TABLE...AS

CREATE TABLE...AS構文により、既存のテーブルからデータを取得し新しいテーブルを作成することができます。SASでは利用するケースは非常に多いですが、一般的なSQLでは利用されることはそれほど多くはありません。

```sql
CREATE TABLE myclass AS
SELECT *
FROM sashelp.class
;
```

### 定義＋INSERT

SQLはデータ型の定義を厳しくしておくのが一般的で、定義だけをしてあとはシステムによりレコードが追加される、というようなケースが多くあります。先にデータのインポートが必要な場合には、利用している環境によりCSVからインポートしたり、データフレームを扱うライブラリなどからインポートすることになります。SASの場合はインポートなりcardsステートメントなどのほうが作成しやすいでしょう。ここではくわしく触れませんが、SQL文のみでサンプルデータを作成する例を以下にあげておきます。

```sql
/* テーブルの作成 */
CREATE TABLE sample_table (
   ID int PRIMARY KEY,
   Name varchar(50),
   Age int,
   Gender varchar(10)
);

/* データの挿入 */
INSERT INTO sample_table
VALUES (1, 'Alice', 25, 'Female'),
       (2, 'Bob', 30, 'Male'),
       (3, 'Charlie', 22, 'Male'),
       (4, 'Diana', 28, 'Female'),
       (5, 'Eva', 45, 'Female');
```

### CREATE TABLE...LIKE

CREATE TABLE...LIKE構文は、既存のテーブルの構造（列名、データ型、制約など）をコピーして新しいテーブルを作成するために使用されます。ただし、作成されるのは定義だけでレコードはコピーされません。

```sql
CREATE TABLE class LIKE sashelp.class ;

/* 同じ動作のSASコード例 */
data work.class ;
  set sashelp.class ;
  stop ;
run ;
```

## 新規列の作成

### 基本的な列の作成

SELECT文では演算子や関数を使用して新規の列を作成することができます。固定の値の列や中身が同じ列も作成可能です。`AS`句は省略可能ですがSASでは必須で、可読性の面からも書いておくことをおすすめします。

```sql
SELECT age+10 AS age2, "A" AS lit, age age2
/*label='age formatted' format=3.0*/ 
FROM sashelp.class
;
```

### CASE文を用いた列の作成

IF・ELSEのような条件判定により列を作成する場合、以下のようなCASE文を使用して新規の列を作成できます。SASではifc・ifn関数を使うことも可能ですが、SQLではSQLの書き方をしておくほうが一般には好まれます。

```sql
SELECT name, age,
CASE
  WHEN age < 15 THEN '<15'
  ELSE '>=15'
END AS agegroup
FROM sashelp.class
;
```