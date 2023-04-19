# R・Pythonでハンドリングのための利用

他のプログラミング言語からもデータベース・SQLは利用できます。以下ではSQLiteについてRとPythonでデータフレームをデータベースに格納し、再度データフレームに戻す例を紹介します。

データフレームを直接操作できるsqldfやpandasqlなどのライブラリもありますが、メンテナンスの状態が不明(最後のアップデートが数年以上前)で、環境によって追加できないこともあると思うので、SQLiteのライブラリのみを以下では使用します。SQLiteは通常のファイルとしてだけでなく、一時ファイルやメモリで読み書きをする機能があり、以下ではメモリを使用します。

## R

```r
library(RSQLite)

# データをロード
data(iris)

# メモリ上のSQLiteデータベースに接続
con <- dbConnect(SQLite(), ":memory:")

# データフレームをデータベースに書き込む
dbWriteTable(con, "iris", iris)

# SQLクエリを実行して結果をデータフレームに読み込む
sql_query <- "SELECT * FROM iris ;"
filtered_data <- dbGetQuery(con, sql_query)

# 結果を表示
print(filtered_data)

# データベース接続を閉じる
dbDisconnect(con)
```

## Python

```python
import sqlite3
import pandas as pd
from sklearn import datasets

# データをロード
iris = datasets.load_iris()
iris_df = pd.DataFrame(iris.data, columns=iris.feature_names)

# メモリ上のSQLiteデータベースに接続
conn = sqlite3.connect(':memory:')

# データフレームをデータベースに書き込む
iris_df.to_sql('iris', conn, if_exists='replace', index=False)

# SQLクエリを実行して結果をデータフレームに読み込む
sql_query = "SELECT * FROM iris ;"
filtered_data = pd.read_sql_query(sql_query, conn)

# 結果を表示
print(filtered_data)

# データベース接続を閉じる
conn.close()
```