# サブクエリ

サブクエリはSQL文の中に入れ子になった別のSQL文です。メインのSQL文に対してテーブルや値の指定のかわりにSQL文を使用することができます。

```sql
SELECT *, (SELECT max(age) FROM sashelp.class ) as maxage 
FROM (SELECT DISTINCT sex
      FROM sashelp.class
      )
;
```

使える箇所が多く便利な反面、可読性が下がりやすいため、指示の意図にそった単純なケースに限定して利用することをおすすめします。

## サブクエリを使用した抽出

サブクエリを使用して条件となる値を取得し、それによりレコードを抽出することができます。比較的よく使われる例では、他のテーブルしか持っていない列の条件で抽出し、そのIDをin演算子で利用すると、別にテーブル作成して結合させることをしなくても抽出が可能です。

```sql
SELECT *
FROM sashelp.class
WHERE age > (SELECT AVG(Age) FROM sashelp.class)
;

SELECT *
FROM sashelp.class
WHERE name in (SELECT name 
               FROM work.user 
               WHERE county ="US"
              )
;
```

proc sqlでは利用できないですが、WHERE句の複数指定とサブクエリでグループごとの最大値などを取得することも可能です。

```sql
proc fedsql ;
  SELECT * FROM work.class 
  WHERE (sex, weight) IN (SELECT sex, MAX(weight) 
                          FROM work.class 
                          GROUP BY sex
                          )
  ;
quit ;
```

### サブクエリに関する演算子(exists,any,all)

サブクエリに関するWHERE句で利用できる演算子に`exists`, `any`, `all`があります。サブクエリで複数のレコードを抽出し判定を行います。existsは抽出されたレコードが存在する、any,allでは複数抽出されたレコードがいずれかまたはいずれも条件を満たす場合、元のレコードが抽出の対象となります。
比較的動作が分かりづらく、結合や通常の演算子によるサブクエリのほうが好まれるので、利用されないことが多いです。

```sql
/*性別ごとに年齢が最も大きいレコード*/
SELECT *
FROM sashelp.class a
WHERE exists (SELECT *
              FROM sashelp.class b 
              WHERE a.sex = b.sex
              GROUP BY Sex
              having a.age = max(age)
)
/* weightが100超のレコードよりageが小さいレコード */
SELECT *
FROM sashelp.class a
WHERE age < all (SELECT age
              FROM sashelp.class b 
              WHERE weight > 100
)
;
```