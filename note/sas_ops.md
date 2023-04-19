# SAS特有の操作と有利なケース

## SAS特有の操作

### マクロ変数の作成

SAS独自の機能としてINTO句を使用して、マクロ変数を作成することができます。対象のレコードが複数ある場合は最初のレコードだけが対象になります。

```
proc sql noprint ;
  SELECT name, age
  INTO:name1:, :age1
  FROM sashelp.class(obs=1)
  ;
quit ;
```

### 連番の設定

monotonic関数を使用すると、_n_と同じような値を設定することができます。

```
proc sql ;
  SELECT *, monotonic() as _n_
  FROM sashelp.class
  ;
quit ;
```

### 再マージ

単一のSELECT文で個別のレコードと集計したレコードを同時に扱うようなことはSQLでは基本的にできませんが、製品によっては可能です。ORACLEなどではOVER句により、SASでは再マージという仕様・オプションにより可能です。

```
proc sql ;
  SELECT *, count(*) as total
  FROM sashelp.class
  ;
quit ;
```

以下の条件のいずれかに該当すると再マージが行われます。

- SELECT 句が、集計関数を含む列を参照しており、かつGROUP BY句に記述されていない他の1 つ以上の列を参照している
- ORDER BY 句が、SELECT句によって参照されていない列を参照している

再マージを行うとデータのソート順が変わることがあるため、ソート順が重要な場合はORDER BY句でソート順を指定しておくことをおすすめします。またSQLの標準的な機能ではないため、機能を無効にしたい場合にはグローバルオプションでSQLREMERGEをNOSQLREMERGEにする、またはproc sqlのオプションでデフォルトのREMERGEをNOREMERGEにすることで再マージを無効にすることができます。

## SQLが有利なケース

### 区切り文字によるマクロ変数とループ

対象のレコードが複数ある場合、SEPARATED BY を指定することで区切り文字を指定して複数のレコードの値をマクロ変数に格納できます。特に値の規則が明確で区切り文字が入らないような場合では、countw関数を使用して値の数をカウントしてループ処理をすることができます。
この方法だと&を複数用いる間接参照が不要で、マクロ変数がひとつなので値の確認が容易です。

```
proc sql noprint ;
  SELECT name
  INTO:name separated by " "
  FROM sashelp.class
  ;
quit ;

%macro sample ;
  %do i = 1 %to %sysfunc(countw(&names.)) ;
    %let name = %scan(&names., &i.) ;
    %put &name. ;
  %end ; 
%mend ;

data _null_ ;
  set sashelp.class end = EOF ;
  call symputx(cats("name_", _n_), name) ;
  if EOF = 1 then call symputx("obs", _n_) ;
run ;

%macro sample ;
  %do i = 1 %to &obs. ;
    %put &&name_&i. ;
  %end ; 
%mend ;
```

### 0レコードの場合もあるレコード数のカウント

SQLではCOUNTで対象が0レコードの場合も、通常と同じような記載で0レコードと取得できます。
SASの場合0レコードの場合はset以降が動作しないため、setの前に記載する必要があります。またWHERE文を使いたい場合、WHERE文の適用済みのデータセットを作成するか、ATTRN関数でNLOBSFを指定してレコード数を取得する必要があります。

```
data dummy ；
  stop ;
run ;

proc sql noprint ;
  SELECT count(*)
  INTO:obs
  FROM work.dummy
  ;
quit ;

%put &obs. ;

data _null_ ;
  call symputx("obs",　obs) ;
  set work.dummy(nobs = obs) ;
  stop ；
run ;

%put &obs. ;
```

### 一意のレコードのカウント

一意のレコードのカウントをする場合、SASでは重複を削除する処理が必要になります。定型的な処理なので複数の処理をマクロでまとめていることも多く、共通した手順というのはあまりありません。違う組織の人に抽出条件を確認してもらう際などに、以下のようにDISTINCTを利用すると一意の値のカウントをするという処理を共有しやすくなります。

```
proc sql ;
  SELECT AESOCCD, AEPTCD, count(DISTINCT USUBJID)
  FROM adam.ADAE
  WHERE FASFL = "Y"
  GROUP BY AESOCCD, AEPTCD
  ;
quit ;
```

### 範囲結合

上限・下限の範囲を指定して結合する、というような動作をわかりやすく書くことができます。

```sql
SELECT a.name, b.name AS name2 
FROM sashelp.class a
LEFT JOIN work.code b
ON b.lower < a.weight <= b.upper 
;
```