# レコードの要約

SQLでは要約関数により、列から単一の値を計算することができます。SQLの主要な要約関数には以下のような関数があります。

- COUNT
- SUM
- AVG
- MIN
- MAX

SASではそれに加えデータステップ関数の多くが利用可能で、FCMPで独自に定義した関数のも一部条件を除き使用可能です。ただし端数処理がデータステップと違うという場合もあるので、厳密な値が必要な場合は平均や分散などの算出は避けておくことをおすすめします。

列名は自動で設定されるので確認だけであればそのままでも問題ないですが、`AS`句を使用して列名を指定しておくと、その後データセットとして利用する場合などで扱いやすくなります。

```sql
SELECT count(*)
FROM sashelp.iris
;
SELECT count(*) AS obs
FROM sashelp.iris
;
```

**COUNT＋DISTINCT**
COUNT関数の中でDISTINCT句を使用することで、1文でユニークなレコードの数を数えることができます。

```sql
SELECT count(DISTINCT species)
FROM sashelp.iris
;
```

## GROUP BY

GROUP BY句は、指定された列の値に基づいてレコードをグループ化し、要約関数を適用するために使用されます。

```sql
SELECT species, count(*)
FROM sashelp.iris
GROUP BY species
;
```

## HAVING

HAVING句はGROUP BY句でグループ化されたレコードに対して条件を適用するために使用されます。HAVING句では、以下のように要約関数を含む条件を指定できます。

```sql
SELECT Species, count(*)
FROM sashelp.iris
GROUP BY Species
HAVING count(*) > 40
;
```