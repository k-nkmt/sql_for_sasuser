# SQL for SASユーザー

## 作成の目的
SAS言語のユーザーはまずデータステップの学習が優先で、SQLの利用に慣れていない方もそれなりにいるように感じていましたが、昨今の生成系AIの発達と学習データに多く含まれる内容は高精度な出力が得られることにより、SQLの重要性は非常に高くなると考えています。

基本的なハンドリングのコードを生成する際には、自然言語→SQL→目的の言語、というように間にSQLを挟むことで、目的の言語に詳しくない場合でも高い精度でコードの生成が可能になります。

以下の内容をしっかり覚えていく、というよりは概念の理解や用語を覚えることに重点を置くといいと思います。覚えていない場合には生成系AIに質問しつつ、共通の用語を用いて指示をすることで、生成系AIへの指示を短く的確に行うことができます。

またある程度SQLを触っている方でも、SQLは製品により独自の仕様があるので、SAS独自の拡張的な仕様や対応していない仕様について、SASと他言語で動かす場合の違いや注意などの整理に役立ててもらえたら幸いです。

## 内容
- [SQLとデータベース](note/sql_db.md)
- [SQL文の分類と基本](note/sql_basics.md)
- [SQLとSASの関係](note/sql_sas.md)
- [データの抽出](note/data_extract.md)
- [レコードの要約](note/summarization.md)
- [新規テーブル・列作成](note/new_tables_columns.md)
- [データの結合方法(横)](note/merges.md)
- [サブクエリ](note/subqueries.md)
- [データの結合方法(縦)](note/unions.md)
- [その他基本操作](note/basic_ops.md)
- [SAS特有の操作と有利なケース](note/sas_ops.md)
- [R・Pythonでハンドリングのための利用](note/r_python_handling.md)
- [練習問題](note/exercise.ipynb)
- [練習問題(回答)](note/exercise_answer.ipynb)


## 参考資料・注意等
SAS関連は主には[proc sqlのリファレンス](https://www.sas.com/offices/asiapacific/japan/service/help/pdf/v94/sqlproc.pdf) を参考にしています。  
実行・確認はSAS Ondemandをjupyter notebookで動作させています。  
文章の構成、各機能等の概要的な部分(1,2行程度)およびコードの例にはClaudeおよびchatGPT(GPT-4)による出力を利用しています。