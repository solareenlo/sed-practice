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
