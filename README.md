# sed-practice

## 初めてのsed
```bash
cat names.txt
> 1 taguchi
> 2 koji
> 3 taro
> 4 hanako
> 5 yasuda

# 3行目を削除して標準出力する
sed -e '3d' names.txt
# 3dのところが1つしかない場合は-eを省略できる
sed '3d' names.txt
> 1 taguchi
> 2 koji
> 4 hanako
> 5 yasuda

# sedして上書きしたい場合
sed -i '3d' names.txt

# 上記をバックアップを取りながらしたい場合
sed -i.bak '3d' names.txt
cat names.txt
> 1 taguchi
> 2 koji
> 4 hanako
> 5 yasuda

ls
> names.txt names.txt.bak
cat names.txt.bak
> 1 taguchi
> 2 koji
> 3 taro
> 4 hanako
> 5 yasuda
mv names.txt.bak names.txt
ls
> names.txt

# ファイルを読み込んでsedする
vim ex1.sed
3d # と書き込む
:wp
sed -f ex1.sed names.txt
> 1 taguchi
> 2 koji
> 4 hanako
> 5 yasuda

# パイプとsedを用いてディレクトリ情報の初めの3行を削除して標準出力
ls -la | sed '1,3d'
> ディレクトリ情報の初めの3行が削除された形で標準出力される

# さらにリダイレクト(>)も使って, 上記をファイルに出力する
ls -la | sed '1,3d' > output.txt
```

## パターンスペースについて
sedはパターンスペースを用いて操作を行っている.  
どういうことかというと,  
`sed '3d' names.txt'`  
は, 3がaddress, dが削除commandの意味で,  
1. fileから1行目を読み込んでパターンスペースに格納  
2. addressにマッチする？ → commandを実行  
3. パターンスペースを表示  
という流れになっている.

## アドレスを使いこなす
```bash
# 3行目以外を削除
sed '3!d' names.txt
> 3 taro

# 1行目と3行目を削除
sed '1d;3d' names.txt
> 2 koji
> 4 hanako
> 5 yasuda

# 1行目から3行目までを削除
sed '1,3d' names.txt
> 4 hanako
> 5 yasuda

# 1行目を削除してから2行飛ばしで削除.
# つまり, 奇数行目を削除.
sed '1~2d' names.txt
> 2 koji
> 4 hanako

# 最後の行を削除
sed '$d' names.txt
> 1 taguchi
> 2 koji
> 3 taro
> 4 hanako

# 3行目から最後の行までを削除
# $が最後の意味
sed '3,$d' names.txt
> 1 taguchi
> 2 koji

# iで終わる行は削除
sed '/i$/d' names.txt
> 3 taro
> 4 hanako
> 5 yasuda

# addressは省略もできるのでaddressを書かないと全削除
sed 'd' names.txt
```

## p/a/iコマンドを使う
```bash
# 3行目を2回表示する
sed '3p' names.txt
> 1 taguchi
> 2 koji
> 3 taro
> 3 taro
> 4 hanako
> 5 yasuda

# 3行目だけを表示する
sed -n '3p' names.txt
> 3 taro

# 3行目で処理を終わる
sed '3q' names.txt
> 1 taguchi
> 2 koji
> 3 taro

# 1行目の前に文字列をinsertする
sed '1i\--- start ---' names.txt
> --- start ---
> 1 taguchi
> 2 koji
> 3 taro
> 3 taro
> 4 hanako
> 5 yasuda

# 最終行にも文字列を挿入する
sed -e '1i\--- start ---' -e '$a\--- end ---' names.txt
> --- start ---
> 1 taguchi
> 2 koji
> 3 taro
> 3 taro
> 4 hanako
> 5 yasuda
> --- end ---

# 最終行に空の文字列を挿入する
sed '/^$/i' names.txt
> 1 taguchi
> 2 koji
> 3 taro
> 3 taro
> 4 hanako
> 5 yasuda
>
```
