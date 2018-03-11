# put japanese on matplotlib  
matplotlibを日本語対応させます。  

<br>
## 前提
matplotlibのデフォルトフォントが日本語に対応していないため(Macだけの問題?)、  
グラフに日本語を書こうとすると「□□□」みたいな感じに文字化けしてしまう。

<br>
## 試したこと  
[ここ](https://qiita.com/knknkn1162/items/be87cba14e38e2c0f656)と[ここ](http://kaisk.hatenadiary.com/entry/2015/02/15/215831)と[ここ](https://gcbgarden.com/2017/05/04/matplotlib-japanese/)を試した。  

が、結局上手くいかず色々模索することに。

結論から書くと、参照先の手順に加えて、  
~/.matplotlib/fontList.json
を消したら上手くいった。  

<br>
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

<br>
## 手順  
1. fontのダウンロード  
[ここ](https://ipafont.ipa.go.jp/node26#jp)から「IPAexゴシック」をダウンロード。  


2. zipを解凍すると「ipaexg.ttf」というファイルが出現するので、  
 「../python[$バージョン]/site-packages/matplotlib/mpl-data/fonts/ttf」(「site-packages」より上のパスは環境によって異なる)にコピーする。  


3. ターミナルを立ち上げ、matplotlibの使用ディレクトリへ移動。  
```
$ cd ~/.matplotlib  
```

4. matplotlibrc(matplotlibを起動時に読み込むファイル)を作成しつつ、使用フォント指定の文字列を書き込む。  
```
$ echo 'font.family  : IPAexGothic' > matplotlibrc
```

5. 邪魔なキャッシュ共を削除(fontList.jsonを含めるのがポイント)  
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


グラフタイトルがきちんと表示されていればひとまず完了です。  
お疲れ様でした。

<br>
## 留意点  
matplotlibが更新されるときに「../python[$バージョン]/site-packages/matplotlib/」以下が書き換わり、
以前の状態に戻ってしまう可能性がある。
そのときは 「ipaexg.ttf」を再配置すれば直る(はず)。
