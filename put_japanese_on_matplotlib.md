# put japanese on matplotlib  
matplotlibを日本語対応させます。  

## 前提
matplotlibのデフォルトフォントが日本語に対応していないため、  
グラフに日本語を書こうとすると「□□□」みたいな感じに文字化けしてしまう。

## 試したこと  
[ここ](https://qiita.com/knknkn1162/items/be87cba14e38e2c0f656)と[ここ](http://kaisk.hatenadiary.com/entry/2015/02/15/215831)と[ここ](https://gcbgarden.com/2017/05/04/matplotlib-japanese/)を参考にするも一向に□□□状態から抜け出せず。

結論から先に書くと、これらの手順に加えて、  
~/.matplotlib/fontList.json
を消したら上手くいった。  

## 環境  
```
$ sw_vers  
ProductName:    Mac OS X  
ProductVersion: 10.13.3  
BuildVersion:   17D102  

$ python -V  
Python 3.6.3 :: Anaconda custom (64-bit)

$ brew -v
Homebrew 1.5.8
Homebrew/homebrew-core (git revision 695f341; last commit 2018-03-04)

$ pyenv -v
pyenv 1.2.0  
```
## 手順  
1. fontのダウンロード  
[ここ](https://ipafont.ipa.go.jp/node26#jp)から「IPAexゴシック」をダウンロード。  

2. zipを解凍すると「ipaexg.ttf」というファイルが出現するので、  
 「../python[$バージョン]/site-packages//matplotlib/mpl-data/fonts/ttf」(「site-packages」より上のパスは環境によって異なる)にコピー。  

3. ターミナルを立ち上げ、matplotlibの使用ディレクトリへ移動。  
```
$ cd ~/.matplotlib  
```
4. matplotlibrcを作成しつつ、使用フォント指定の文字列を書き込む。  
```
$ echo 'font.family  : IPAexGothic' > matplotlibrc
```
5. 邪魔なキャッシュを削除(fontList.jsonを含めるのがポイント)  
```
rm -f fontList.json fontList.cache fontList.py3k.cache
```
6. 動作確認
```
$ python  

>>> import matplotlib.pyplot as plt
>>> plt.title('ぐりとぐらふ')
>>> plt.show()  
```

グラフタイトルがきちんと表示されれば完了です。  
お疲れ様でした。
