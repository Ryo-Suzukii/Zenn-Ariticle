---
title: "Pandasをマスターするまでやめない第4話"
emoji: "🗒️"
type: "idea"
topics:
  - "Python"
  - "pandas"
  - "備忘録"
published: true
published_at: "2022-07-08 15:56"
---

### これまで
https://zenn.dev/ha/articles/hayaa6211-pandas-master-1  

https://zenn.dev/ha/articles/hayaa6211-pandas-master-2  

https://zenn.dev/ha/articles/hayaa6211-pandas-master-3

### データクレンジングとは
破損したデータや不正確なデータ，無意味なデータを特定し最適な扱いをすることで予測する際や可視化する際に使いやすくすること．

### 重複行
重複している行があると特徴量に偏りが生まれるため消すことが必要．
```df.duplicated()```
```python
df.duplicated() -> pd.Series

"""
0    False
1    True
2    False
3    False
4    False
....
n    False
"""
```

#### 重複している行のカウント
```df.duplicated().value_counts()```  
```value_count()```はbool型のSeriesの中にあるTrueの数の合計

#### 重複している行の抽出
条件抽出とやり方は一緒

```python
df_duplicated = df[df.duplicated()]
```

#### 重複している行の削除
```df.drop_dupulicated(inplace=False)```

### 欠損値の処理
欠損値はnan(Not a Number)と呼ばれる.これがあると特徴量が足りず偏りが生まれる

#### 欠損値の判定
```df.isnull()```  
中身がbool型になり，nanならTrue値があればFalseとなる

#### nanのある列の判定
```df.isnull().any()```  
```any()```はTrueのある列をTrueとしてcolumnsをindexとしたSeriesを返す

```python
tf = df.isnull().any()
col_name = tf[tf].index.tolist()
```

#### 各カラムのnan個数をカウント
```df.isnull().sum()```
nanではない個数の場合は```df.count()```

#### 各行にnanがあるかどうか
```df.isnull().any(axis=1)```

#### nanがある行の抽出
```df_nan = df[df.isnull().any(axis=1)]```

#### nanのある行を削除
```df.dropna(inplace=False)

#### nanの補完
```df.fillna(object)```
```python
mean = df[columns].mean()
df[columns] = df[columns].fillna(mean) #平均で補完
df[columns] = df[columns].fillna(0)    #特定(0)の値で補完
```

#### nanの予測
```missingpy.MissForest().fit_transform()```
```python
from missingpy import MissForest
cols = df[df.isnull().any()].columns.tolist() #予測したいcolumns
df = df[cols]
res  = MissForest().fit_transform(df) -> np.array

df_inpute = pd.Dataframe(res,columns=[cols])
```

### エンコーディング
機械学習では文字列を数値に変換しなくてはいけない．
#### labelエンコーディング
```sklearn.preprocessing.LabelEncoder```

```python
from sklearn.preprocessing import LabelEncoding
le = LabelEncoding() #インスタンス生成

le.fit(df[column]) #予測したいカラムで学習

df[new_column] = le.transform(df[column]) #ラベル->ラベルID
```

#### one-hotエンコーディング
```pd.get_dummes(df)```

### おわり
ここら辺は無限にやり方あるし実際にkaggleとかでデータ加工の経験詰むのが一番よさそうあざした
