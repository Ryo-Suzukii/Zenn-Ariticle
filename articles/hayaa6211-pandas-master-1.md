---
title: "Pandasをマスターしたい備忘録"
emoji: "🗒️"
type: "idea"
topics:
  - "Python"
  - "pandas"
  - "備忘録"
published: true
published_at: "2022-06-02 15:56"
---

# 初めに
pandasって使いこなせたらめっちゃかっこいいですよねわかる．ってことで講義で教授からもらった資料や個人的に大事だなって思ったところをまとめておきたいと思います．  
ほかにも「これ大事だろ」ってことだったり「これちがくね？」って点があったら教えてください

# 目次

# pandasとは
- データ操作を効率的に行うPythonライブラリ．
- pandasでできること
    - データの読み込み，整形，加工，結合，保存
    - データの検索，表計算，集計
    - データの可視化

# pandasのデータ型
- pandasには大きく分けて二つのデータ型がある．
    - DataFrame
        - 表形式データで`値`,`インデックス`,`カラム`からなる．Excelみたいな感じ
    - Series
        - 1次元データで`値`,`インデックス`からなる．listみたいな感じ

# pandasのインポート
pandasは慣習的に`pd`として読み込む

```python
import pandas as pd
```

# ファイルからデータの読み込み
## csvファイルの読み込み
`pd.read_csv("csvファイルパス",encoding="文字コード")`

```python
df = pd.read_csv("csvファイルパス":str,encoding="文字コード":str)
type(df)
#>DataFrame
```
- csvファイルパスはそのまま
- encodingのデフォルトは`utf_8`(国際標準)．日本語ファイルや国勢調査などのオフィシャルのデータによくある．

## Excelファイルの読み込み
`pd.read_excel("Excelファイルのパス":str,"シートの名前":str)`

```python
df = pd.read_excel("Excelファイルのパス","シートの名前")
```

シートの名前はファイルの中に1シートしかないときいらない．

## webサイトの票データの取得
`pd.read_html(url:str,encoding:str)`

```python
df = pd.read_html("url",encoding)
```


# DataFrameの表示
DataFrameの表示には`display()`が有効．`print()`でもいいけど見やすさがレベチ．

## DataFrameの上位表示
DataFrameは基本たくさんのデータがあるので全部表示させると画面がごちゃごちゃする．  
中身がどんな内容であるか確認したいときは`head()`を使うといい

```python
type(df) == DataFrame #True

display(df.head())
#冒頭5行の表示

display(df.head(3))
#冒頭3行の表示
```

# ファイルへ出力
## DataFrameをcsvファイルとして出力
`dataFrame.to_csv("csvファイルパス",index=False,encoding="utf_8")`

```python
df.to_csv("csv.csv")
```
- `index`:indexを出力するときはTrue,いらなければFalse

## DataFrameをExcelファイルとして出力
Excelファイルとして出力する際は外部ライブラリ`openpyxl`のインストールが必要．
`DataFrame.to_excel("Excelファイルパス",sheet_name="シート名",index,encoding)`

```python
DataFrame.to_excel("Excelファイルパス",sheet_name="シート名",index=False,encoding="utf_8")
```

- 既存のExcelファイルにシートを追加して出力
```python
import openpyxl

with pd.ExcelWriter("Excelファイルパス",engine="openpyxl",mode="a") as writer:
    DataFrame.to_excel(writer,sheet_name="シート名",index=False,encoding="utf_8")
```

# データ構造の確認
## 行数と列数の確認
`DataFrame.shape`

```python
display(df.head(3))
```

| 都道府県コード | 都道府県名 | 元号 | 和暦（年）| 西暦（年）| 人口（総数）| 人口（男） | 人口（女）
|---------------| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| 0	| 1 | 北海道 | 大正 | 9 | 1920 | 2359183 | 1244322 | 1114861 |
| 1 | 2 | 青森県 | 大正 | 9 | 1920 | 756454  | 381293  |	375161  |
| 2 | 3	| 岩手県	 | 大正 | 9 | 1920 | 845540 | 421069  |	424471 |

```python
df.shape
#>(939,8)
```

#データの抽出
## 列の抽出
### 一列抽出
`データフレーム["列名"]`->Series型

```python
df_col = df["人口（総数）"]
df_col
```
|   |        |
|---|--------|
| 0 | 232453 |
| 1 | 75343  |
| 2 | 83123  |
| ...|...    |

### 複数列抽出
`データフレーム[["列名1","列名2",...]]->DataFrame`

```python
df_cols = df[["人口（総数）", "人口（男）", "人口（女）"]]
df_cols
```

| | 人口（総数）| 人口（男）| 人口（女） | 
|-|------------|----------|-----------|
|0|   232453   | 1244322  |  1114861  |
|1|   756454   | 381293   |  375161   |
|2|   845540   |  421069  |  424471   |
|.| ...        | ...      | ...       |

## 条件に合ったデータの抽出
pandasではDataFrameに演算子が使用できる．

```python
TF = df["西暦（年）"] == 2000
TF
```
| | |
|-|-|
|0|False|
|1|False|
|2|False|
|.|...|

さらにTrueのみの抽出ができる

```python
df_TF = df[TF]
df_TF
```

| | 都道府県コード | 都道府県名 | 元号 | 和暦（年）| 西暦（年）| 人口（総数）| 人口（男） | 人口（女）
|-|---------------| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
|751|1|北海道|12|2000| 123213 | 12321 | 12321 | 53241 |

これよりこんな書き方で書ける

```python
df_TrueFalse = df[df["西暦（年）] == 2000]
```

## 論理演算子を用いた複数条件の抽出

```python
FT2 = df[(df["西暦（年）"] == 2000) & (df["都道府県名"] == "奈良県")]
```

## 特定の要素の選択，取得，変更

`loc`や`iloc`が使える

### loc
- 単独の要素を選択
    - `DataFrame.loc["行名","列名"]`
- 複数の要素を選択
    - `DataFrame.loc[["行名1","行名2",...],["列名1","列名2",...]]`
- スライス表記
    - `Dataframe.loc['行名1' : '行名2', '列名1':'列名2']`

### iloc
- 単独の要素を選択
    - `DataFrame.iloc[行番号，列番号]`
- 複数の要素を選択
    - `DataFrame.iloc[[行番号,行番号2,...],[列番号1,列番号2,...]]`
- スライス表記
    - `DataFrame.iloc[行番号(開始):行番号(終了),列番号(開始):列番号(終了)]`
    - 終了の数値は-1まで出力されることに注意
    - 10:20を表示したければ10:21
