# データの抽出

## SELECT, FROM

SQLでデータを抽出するには、SELECT句を使用します。
SELECTでは抽出したい列の名前を指定します。`*` を使うと全ての変数を指定することができます。一般には既存のテーブルから抽出するので、FROM句に抽出したいテーブルの名前を指定します。また`DISTINCT`句を使うと重複したレコードを除外できます。`proc sort ... nodupkey`と同じような動作です。

```sql
SELECT name, age 
FROM sashelp.class
;
SELECT * 
FROM sashelp.class
;
SELECT DISTINCT age 
FROM sashelp.class
;
```

## WHERE

特定の条件を満たすレコードを抽出する場合WHERE句を使用します。一般的な比較演算子やin演算子、andやor条件の他に、以下を使用することができます。

- `BETWEEN-AND`：値がある範囲(境界を含む)に含まれるかどうかを検証します。
- `CONTAINS`：指定された文字列が値に含まれるかどうかを検証します。
- `EXISTS`：サブクエリによって取得された値が存在するかどうかを検証します。
- `IS NULL` または `IS MISSING`：欠損値を検証します。
- `LIKE`：値が指定されたパターンに一致するかどうかを検証します。`%`は任意の数の文字、`_`は任意の1文字としてパターンを指定することができます。

```sql
SELECT * 
FROM sashelp.class
/* 実行する場合は1つ以外コメントアウト */
WHERE age between 10 and 12
WHERE name contains'AA'
WHERE weight is missing
WHERE name like '_h%n'
;
```

## ORDER BY

レコードの並び替えは、ORDER BY句を使用します。複数の変数を指定する場合はカンマで区切ります。順序はデフォルトは昇順で省略可能です。降順にする場合は該当の変数のあとに`DESC`と記載します。

```sql
SELECT * 
FROM sashelp.class
ORDER BY age ASC, weight DESC
;
```

## proc sqlで使えない句

**LIMIT, OFFSET**
LIMITは出力するレコード数、OFFSETはスキップするレコード数を指定することができます。SASではproc fedsql では利用可能ですが、proc sqlで動かす場合はデータセットオプションのobs, firstobsを使用することで同じ動作が可能です。ただし似たように見えますが若干仕様は異なるので注意してください。以下は同じ動作のコードです。

```
proc fedsql ;
  SELECT *
  FROM work.myclass
  LIMIT 10 OFFSET 5
  ;
quit ;

proc sql ;
  SELECT *
  FROM work.myclass(obs=15 firstobs=6)
  ;
quit ;
```