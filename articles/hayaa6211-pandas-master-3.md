---
title: "Pandasをマスターしたい生活3日目"
emoji: "🗒️"
type: "idea"
topics:
  - "Python"
  - "pandas"
  - "備忘録"
published: true
published_at: "2022-07-08 15:56"
---

## これまで
https://zenn.dev/ha/articles/hayaa6211-pandas-master-1

https://zenn.dev/ha/articles/hayaa6211-pandas-master-2

## よく使えるやつ
```python
df.index                     #indexの表示
index = df.index.tolist()    #indexの中身をリストにする
type(index)
>>> List

df.columns                    #カラムの表示
columns = df.columns.tolist() #カラムの中身をリストにする
type(columns)
>>> List
```

### index,columns名の変更
```df.rename(index={"index名":"変更したいindex名"},columns={"columns名":"変更したいcolumns名"})```
```python
df = pd.DataFrame({"A":[1,2,3,4,5],"B":[10,9,8,7,6]})
df = df.rename(index={0:"zero"})
print(df.index.tolist())
>>> ["zero",1,2,3,4]
```

```python
df = df.reaname(columns={"A":"A_rename"})
print(df.columns.tolist())
>>> ["A_rename","B"]
```

### データの並び替え

```df.sort_values(by="並び変えたいcolumns",ascending=True,inplace=False)```
- by:columns名
- ascending:降順か昇順か(Trueで昇順)
- inplace:元のdfを置き換えるか(戻り値はnull)
```python
df_sorted = df.sort_values(by="B")
df_sorted["B"].tolist()
>>> [6,7,8,9,10]

df_sorted_ascending = df.sort_values(by="A_rename",ascending=False)
df_sorted_ascending["A_rename"].tolist()
>>>[5,4,3,2,1]

df_sorted_2 = df.sort_values(by=["A_rename","B",ascending=[False,True]]
```

### dfの演算
numpy配列みたいにそれぞれに演算を適用できる

```python
df["A*B"] = df["A_rename"] * df["B"]
df["A*B"].tolist()
>>> [6,14,24,36,50]
```

### 関数を用いた列の演算
- ```df.apply(関数)```

```python
import math
df["A_SQRT"] = df["A_rename"].apply(lambda x: math.sqrt(x))
```

### 列の削除
- ```df.drop(列名,axis=1)```
- ```df.drop([列名1,列名2],axis=1)```

### 列の結合
```pd.concat([df1,df2],ignore_index=False)```
- ignore_index:indexを振りなおすかどうか


### mergeによる結合
```pd.merge(df1,df2,how="結合方法",on="結合キー")```
- df1:左側のdf
- df2:右側のdf
- how:結合方法
    - inner:内部結合
    - left:左外部結合
    - right:右外部結合
    - outer:完全外部結合
- on:結合キー

### おわり
mergeよくわからん
