SQLite
---

SQLite の COUNT は毎回フルスキャンする。ので、どうしても大きいテーブルをカウントしたい場合は、別テーブルに COUNT の数自体を持つ。

- http://stackoverflow.com/questions/8988915/sqlite-count-slow-on-big-tables
