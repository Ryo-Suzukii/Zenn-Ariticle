---
title: "Pandasをマスターしたい備忘録.2"
emoji: "🗒️"
type: "idea"
topics:
  - "Python"
  - "pandas"
  - "備忘録"
published: true
published_at: "2022-06-27 15:56"
---

### 前回の記事
https://zenn.dev/ha/articles/hayaa6211-pandas-master-1

### 基本統計量の確認
- 各列の基本統計量を計算したい
```python
df.describe()
```

||||
|:-:|:-:|:--|
|count |標本数　　　　　   |行数              | 　　　　　　　　　　　　　　　　　　　　　　　　　　　 |
|mean  |平均値             |算術平均                                                   |
|std   |標準偏差           |データのばらつき                                           |
|min   |最小値             |最も小さい値                                               |
|25%   |第1四分位数        |データを降順に並び替えたとき、後半から4分の1番目にあたる値 |
|50%   |第2四分位数(中央値)|データを降順に並び替えたとき、後半から4分の2番目にあたる値 |  
|75%   |第3四分位数        |データを降順に並び替えたとき、後半から4分の3番目にあたる値 |  
|max   |最大値　　　　     |最も大きい値                                               |
||||

### 個別の統計量の計算

||||
|:-:|:--|:--|
|要素の個数 |データフレーム名.count() | |
|算術平均|データフレーム名.mean()| |
|標準偏差|データフレーム名.std()|n-1で割った不偏標準偏差 |
|最小値|データフレーム名.min()| |
|最大値|データフレーム名.max()| |
|中央値|データフレーム名.median()| |
|最頻値|データフレーム名.mode()|最も出現回数の多い値 |
|ユニークな値の個数|データフレーム名.nuique()|重複を除いた値の個数 |
|特定の列のユニーク値の出現頻度|データフレーム名["列名"].value_counts()|Series型メソッド|
||||

```python
df.nunique() -> pd.Series
# 各列のユニークな数

df["特定カラム"].value_count() -> pd.Series
# 特定列のユニークな値の出現回数
```

### groupby
```python
df = pd.DataFrame({
    'city': ['osaka', 'osaka', 'osaka', 'osaka', 'tokyo', 'tokyo', 'tokyo'],
    'food': ['apple', 'orange', 'banana', 'banana', 'apple', 'apple', 'banana'],
    'price': [100, 200, 250, 300, 150, 200, 400],
    'quantity': [1, 2, 3, 4, 5, 6, 7]
})
df
```

| | city | food | price | quantity |
|---|------|------|-------|----------|
| 0 | osaka | apple | 100 | 1 |
| 1 | osaka | orange | 200 | 2 |
| 2 | osaka | banana | 250 | 3 |
| 3 | osaka | banana | 300 | 4 |
| 4 | tokyo | apple | 150 | 5 |
| 5 | tokyo | apple | 200 | 6 |
| 6 | tokyo | banana | 400 | 7 |

```python
object = df.groupby("グループ化したい列").関数
df.groupby("city").mean()
```

| city | price | quantity |
|------|-------|----------|
| osaka | 212.5 | 2.5 |
| tokyo | 250.0 | 6.0 |

#### indexの表示
これでindex表示してくれる
```python
df.groupby(['city', 'food'], as_index=False).mean()
```

### メソッドの適用
- count
    - 標本数`(欠損値を含まない)`
- size
    - 標本数`(欠損値を含む)`
- sum
    - 合計
- mean
    - 平均
- var
    - 分散
- std
    - 標準偏差
- max
    - 最大値
- min
    - 最小値
### aggオブジェクト
```python
df.groupby("カラム").agg(処理方法)
```
- aggはリストとして入れられるからそれぞれ表示できたりもする
- さらにlambdaを代入できる
```python
df.groupby("column").agg(lambda x: x*1.1)
# それぞれ1.1倍になる
```

### ピボットテーブル
- ピボットテーブルとはユニーク値ごとにグループ化して統計量の算出をするために用いられる．

```python
df.pivot_table(index = "まとめたい単位",
                values = "まとめたい列",
                aggfunc = "集計方法"
                )

df.pivot_table(index = "集計単位",
                columns = "集計単位2",
                values = "集計する列名",
                aggfunc = "関数",
                margins = "合計列を追加するかどうか",
                margins_name = "合計列の名前"
                )
```

### 終わり
ピボットテーブルは正直たくさんやって感覚でなれる必要があると思う．そもそもいつ使うのかいまいちわかってないけどできてて損はないからマスターしたい  
乙
