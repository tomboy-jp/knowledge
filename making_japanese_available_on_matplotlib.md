# Making Japanese available on matplotlib
matplotlibを日本語対応させます。  
<br >

## はじめに  
matplotlibのデフォルトフォントが日本語に対応していないため(Macだけの問題かも？)、  
グラフに日本語を書こうとすると「□□□」みたいな感じに文字化けしてしまう。  
そいつを解決するまでのまとめ。
<br >

## 経緯  
[ここ](https://qiita.com/knknkn1162/items/be87cba14e38e2c0f656)と[ここ](http://kaisk.hatenadiary.com/entry/2015/02/15/215831)と[ここ](https://gcbgarden.com/2017/05/04/matplotlib-japanese/)を試した。  

だが、結局上手くいかず、あれこれ模索をすることに。

結論から書くと、ボトルネックは「~/.matplotlib/fontList.json」というファイルだった。  
名前で詐欺ってるがどうやらこやつもキャッシュのようで毎回読み込まれている模様。  
(その証拠に問題解決後、新しく作られたファイルをgrepしてやったらちゃっかり「IPAexGothic」の行が追加されていた)  
<br >
参照先の手順を実行した上でこやつを消したら解決した。  
<br >

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
<br >

## 手順  
1. fontのダウンロード  
[ここ](https://ipafont.ipa.go.jp/node26#jp)から「IPAexゴシック」をダウンロード。  


2. zipを解凍すると「ipaexg.ttf」というフォントファイルが出現するので、  
 「../python[$バージョン]/site-packages/matplotlib/mpl-data/fonts/ttf」にコピーする。  
 ※「site-packages」より上のパスは使用環境によって異なるため、要確認。    


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
<br >

## 留意点  
ただ一つ留意しなければならないことが。  
matplotlibが更新されるときに「../python[$バージョン]/site-packages/matplotlib/」以下が書き換わり、  
以前の状態に戻ってしまう可能性がある。  
そのときは 「ipaexg.ttf」を再配置すれば直る(はず)。
