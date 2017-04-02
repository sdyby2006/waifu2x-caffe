﻿waifu2x-caffe (for Windows)
----------

 制作者 : lltcggie

本ソフトは、画像変換ソフトウェア「[waifu2x](https://github.com/nagadomi/waifu2x)」の変換機能のみを、
[Caffe](http://caffe.berkeleyvision.org/)を用いて書き直し、Windows向けにビルドしたソフトです。
CPUで変換することも出来ますが、CUDA(あるいはcuDNN)を使うとCPUより高速に変換することが出来ます。

GUI supports English and Japanese and Simplified Chinese and Traditional Chinese and Korean and Turkish and French.

ソフトのダウンロードは[こちらのreleasesページ](https://github.com/lltcggie/waifu2x-caffe/releases)の「Downloads」の項で出来ます。


 要求環境
----------

このソフトを動作させるには最低でも以下の環境が必要です。

 * OS : Windows Vista以降 64bit (32bit用exeはありません)
 * メモリ : 空きメモリ1GB以上 (ただし、変換する画像サイズによる)
 * GPU : Compute Capability 2.0 以上のNVIDIA製GPU(CPUで変換する場合は不要)
 * Visual C++ 2013 再頒布可能パッケージがインストールされていること(必須)
    - 上記パッケージは[こちら](https://www.microsoft.com/ja-jp/download/details.aspx?id=40784)
    - `ダウンロード` ボタンを押した後、`vcredist_x64.exe`を選択し、ダウンロード・インストールを行って下さい。
    - 見つからない場合は、「Visual C++ 2013 再頒布可能パッケージ」で検索してみて下さい。

cuDNNで変換する場合はさらに

 * GPU : Compute Capability 3.0 以上のNVIDIA製GPU

自分のGPUのCompute Capabilityが知りたい場合は[こちらのページ](https://developer.nvidia.com/cuda-gpus)で調べて下さい。


 cuDNNについて
--------

cuDNNはNVIDIA製GPUでのみつかえる高速な機械学習向けのライブラリです。
cuDNNを使わなくてもCUDAで変換出来ますが、cuDNNを使うと以下のような利点があります。

 * 使用するGPUの種類によっては画像をより高速に変換することが出来る
 * VRAMの使用量を減らすことが出来る(最低でもCUDAの半分未満。分割サイズが大きくなるほど差が開いていく)

このような利点があるcuDNNですが、ライセンスの関係上動作に必要なファイルを配布することが出来ません。  
なので、cuDNNを使いたい人は[こちらのページ](https://developer.nvidia.com/cuDNN)でWindows向けバイナリ(v5.1 RC以降)をダウンロードし、
「cudnn64_5.dll」をwaifu2x-caffeのフォルダに入れて下さい。  
なお、ソフトを起動している最中にdllを入れた場合はソフトを起動しなおしてください。  
(cuDNNをダウンロードするにはNVIDIA Developerへの登録とCUDA Registered Developersへの登録が必要です。
 CUDA Registered Developersはおそらく(簡単な)審査があるっぽいので登録してもすぐにcuDNNをダウンロード出来るわけではありません。)

作者の環境での処理速度、VRAM使用量の計測結果は以下の通りです。

 * GPU : GTX 980 Ti
 * VRAM : 6GB
 * 処理内容 : 1000*1000のPNG 4ch画像でノイズ除去と拡大、JPEGノイズ除去レベル1、拡大率2.00、TTAモード未使用
 * 処理時間計測方法 : CUI版で10回の平均処理時間を計測。ただし初めに2回事前に処理を行う(初期化にかかる時間を含めないようにするため)
 * VRAM使用量計算方法 : (GUI版で処理中に使用した最大VRAM) - (GUI版を起動した後のVRAM使用量)

cuDNN RGBモデル

| 分割サイズ |   処理時間   |   VRAM使用量(MB)   |
|:-----------|:-------------|:-------------------|
| 100        | 00:00:03.170 | 278                |
| 125        | 00:00:02.745 | 279                |
| 200        | 00:00:02.253 | 365                |
| 250        | 00:00:02.147 | 446                |
| 500        | 00:00:01.982 | 1110               |

CUDA RGBモデル

| 分割サイズ |   処理時間   |   VRAM使用量(MB)   |
|:-----------|:-------------|:-------------------|
| 100        | 00:00:06.192 | 724                |
| 125        | 00:00:05.504 | 724                |
| 200        | 00:00:04.642 | 1556               |
| 250        | 00:00:04.436 | 2345               |
| 500        | 計測不能     | 計測不能(6144以上) |

cuDNN UpRGBモデル

| 分割サイズ |   処理時間   |   VRAM使用量(MB)   |
|:-----------|:-------------|:-------------------|
| 100        | 00:00:02.831 | 328                |
| 125        | 00:00:02.573 | 329                |
| 200        | 00:00:02.261 | 461                |
| 250        | 00:00:02.150 | 578                |
| 500        | 00:00:01.991 | 1554               |

CUDA UpRGBモデル

| 分割サイズ |   処理時間   |   VRAM使用量(MB)   |
|:-----------|:-------------|:-------------------|
| 100        | 00:00:03.669 | 788                |
| 125        | 00:00:03.382 | 787                |
| 200        | 00:00:02.965 | 1596               |
| 250        | 00:00:02.852 | 2345               |
| 500        | 計測不能     | 計測不能(6144以上) |


 使い方(GUI版)
--------

「waifu2x-caffe.exe」はGUIソフトです。ダブルクリックで起動します。
またはエクスプローラで「waifu2x-caffe.exe」にファイルやフォルダをドラッグ&ドロップで放り込むと、前回起動時の設定で変換を行います。
その場合、設定によっては変換に成功したら自動でダイアログが閉じられます。
また、GUIでもコマンドラインによるオプション設定を行うことが出来ます。
詳しくはコマンドラインオプション(共通)とコマンドラインオプション(GUI版)の項をお読みください。

起動後、「入力パス」欄に画像かフォルダをドラッグ&ドロップで放り込むと「出力パス」欄が自動で設定されます。
出力先を変えたい場合は「出力パス」欄を変更して下さい。

好みに合わせて変換設定を変更することが出来ます。


##入出力設定
     ファイルの入出力に関する設定項目群です。

###「入力パス」
     変換したいファイルのパスを指定します。
     フォルダを指定した場合は、サブフォルダ内も含めた「フォルダ内の変換する拡張子」が付くファイルを変換対象とします。
     複数のファイル、フォルダをドラッグで指定することが出来ます。
     その場合は新しいフォルダの中にファイル、フォルダ構造を維持したまま出力されます。
    （入力パス欄には「(Multi Files)」と表示されます。出力フォルダ名はマウスで掴んでいたファイル、フォルダ名から生成されます）
     参照ボタンを押してファイルを選択する場合、単体のファイル、フォルダか、複数のファイルが選択できます。

###「出力パス」
     変換後の画像を保存するパスを指定します。
    「入力パス」で フォルダを指定した場合は、指定したフォルダの中に変換したファイルを(フォルダ構造をそのままで)保存します。指定したフォルダがない場合は自動で作成します。

###「フォルダ内の変換する拡張子」
     「入力パス」がフォルダの場合の、フォルダ内の変換する画像の拡張子を指定します。
     デフォルト値は`png:jpg:jpeg:tif:tiff:bmp:tga`です。
     また、区切り文字は`:`です。
     大文字小文字は区別しません。
     例. png:jpg:jpeg:tif:tiff:bmp:tga

###「出力拡張子」
     変換後の画像の形式を指定します。
    「出力画質設定」と「出力深度ビット数」に設定できる値はここで指定する形式により異なります。

###「出力画質設定」
     変換後の画像の画質を指定します。
     設定できる値は整数です。
     指定できる値の範囲と意味は「出力拡張子」で設定した形式により異なります。

      * .jpg : 値の範囲(0～100) 数字が高いほど高画質
      * .webp : 値の範囲(1～100) 数字が高いほど高画質 100だと可逆圧縮
      * .tga : 値の範囲(0～1) 0なら圧縮なし、1ならRLE圧縮

###「出力深度ビット数」
     変換後の画像の1チャンネルあたりのビット数を指定します。
     指定できる値は「出力拡張子」で設定した形式により異なります。

##変換画質・処理設定
     ファイル変換の処理方法、画質に関する設定項目群です。

###「変換モード」
     変換モードを指定します。
      * ノイズ除去と拡大 : ノイズ除去と拡大を行います 
      * 拡大 : 拡大を行います
      * ノイズ除去 : ノイズ除去を行います
      * ノイズ除去(自動判別)と拡大 : 拡大を行います。入力がJPEG画像の場合のみノイズ除去も行います

###「JPEGノイズ除去レベル」
    ノイズ除去レベルを指定します。レベルの高いほうがより強力にノイズを除去しますが、のっぺりとした絵になる可能性もあります。

###「拡大サイズ」
    拡大後のサイズの設定を行います。
      * 拡大率で指定 : 画像を指定の拡大率で拡大します
      * 変換後の横幅で指定 : 画像の縦横比を維持したまま、指定された横幅になるように拡大します(単位はピクセル)
      * 変換後の縦幅で指定 : 画像の縦横比を維持したまま、指定された縦幅になるように拡大します(単位はピクセル)
      * 変換後の縦横幅で指定 : 指定された縦横幅になるように拡大します。「1920x1080」のように指定します(単位はピクセル)
    2倍を超える拡大率の場合、(ノイズを除去する場合は最初の1回だけ行い)指定された拡大率を超えるまで2倍ずつ拡大し、2の累乗倍でない拡大率の場合は最後に縮小するという処理を行います。そのため変換結果がのっぺりとした絵になる可能性があります。

###「モデル」
    使用するモデルを指定します。
    変換対象の画像によって最適なモデルは異なるので、様々なモデルを試してみることをおすすめします。
      * 2次元イラスト(RGBモデル) : 画像のRGBすべてを変換する2次元イラスト用モデル
      * 写真・アニメ(Photoモデル) : 写真・アニメ用のモデル
      * 2次元イラスト(UpRGBモデル) : 2次元イラスト(RGBモデル)より高速かつ同等以上の画質で変換するモデル。ただしRGBモデルより消費するメモリ(VRAM)の量が多いので、変換中に強制終了する場合は分割サイズを調節すること
      * 写真・アニメ(UpPhotoモデル) : 写真・アニメ(Photoモデル)より高速かつ同等以上の画質で変換するモデル。ただしPhotoモデルより消費するメモリ(VRAM)の量が多いので、変換中に強制終了する場合は分割サイズを調節すること
      * 2次元イラスト(Yモデル) : 画像の輝度のみを変換する2次元イラスト用モデル

###「TTAモードを使う」
    TTA(Test-Time Augmentation)モードを使うかどうかを指定します。
    TTAモードを使うと変換が8倍遅くなる代わりに、PSNR(画像の評価指数の一つ)が0.15くらい上がるそうです。

##処理速度設定
     画像変換の処理速度に影響する設定項目群です。

###「分割サイズ」
    内部で分割して処理を行う際の幅（ピクセル単位）を指定します。
    最適な(処理が最速で終わる)数値の決め方は「分割サイズ」の項で説明します。
    「-------」で区切られている上の方は入力された画像の縦横サイズの約数、
    下の方は「crop_size_list.txt」から読み出した汎用的な分割サイズです。
    分割サイズが大きすぎる場合、要求されるメモリの量(GPUを使う場合はVRAMの量)がPCで使用できるメモリを超えてソフトが強制終了するので気をつけてください。
    処理速度にそれなりに影響するので、同じ画像サイズの画像をフォルダ指定で大量に変換するときは、最適な分割サイズを調べてから変換することをおすすめします。

##動作設定
     あまり変更する機会がないと思われる動作設定をまとめた設定群です。

###「ファイル入力時自動変換開始設定」
     参照ボタンやドラッグアンドドロップで入力ファイルを指定した際に自動で変換を開始するのか設定を行います。
     exeに入力ファイルを引数で与えた場合ではこの項目の設定内容は影響しません。
      * 自動で開始しない : ファイルを入力しても自動で変換を開始しません
      * ファイルを1つでも入力したら開始 : ファイルを1つでも入力したら自動で変換を開始します
      * フォルダあるいは複数ファイルを入力したら開始 : フォルダ、複数ファイルを入力したら自動で変換を開始します。単体の画像ファイルを変換するのは変換設定の調節を行うときだけだ、という時にどうぞ

###「使用プロセッサー」
    変換を行うプロセッサーを指定します。
      * CUDA(使えたらcuDNN) : CUDA(GPU)を使って変換を行います(cuDNNが使える場合はcuDNNが使われます)
      * CPU : CPUのみを使って変換を行います

###「出力ファイルを上書きしない」
     この設定がONの場合、画像の書き込み先に同名のファイルが存在する場合は変換を行いません。

###「引数付き起動時設定」
     exeに入力ファイルを引数で与えた場合での動作を設定します。
      * 起動時に変換する : 起動時に自動で変換を開始します
      * 成功時に終了する : 変換終了時に失敗していなければ自動で終了します

###「使用GPU No」
     GPUが複数枚ある場合に使用するデバイス番号を指定できます。CPUモード時や無効なデバイス番号を指定した場合は無視されます。

###「入力参照時固定フォルダ」
     入力の参照ボタンを押した際に最初に表示されるフォルダをここで設定したフォルダに固定します。

###「出力参照時固定フォルダ」
     変換した画像の出力先フォルダをここで設定したフォルダに固定します。
     また、出力の参照ボタンを押した際に最初に表示されるフォルダをここで設定したフォルダに固定します。

##その他
     その他の設定項目群です。

###「UIの言語」
    UIの言語を設定します。
    初回起動時はPCの言語設定と同じ言語が選ばれます。(存在しない場合は英語になります)

###「cuDNNチェック」
    「cuDNNチェック」ボタンを押すとcuDNNが使えるか調べることが出来ます。
    cuDNNが使えない場合は理由が表示されます。

「実行」ボタンを押すと変換が始まります。
途中でキャンセルしたい場合は「キャンセル」ボタンを押します。
ただし、実際に停止するまでタイムラグがあります。
プログレスバーは複数枚の画像を変更した際の進行度合いを示しています。
ログに残り予想時間が表示されますが、これは同じ縦幅、横幅の複数ファイルを処理したときの予想です。
なので、ファイルの大きさがバラバラな場合は役に立ちませんし、処理する画像が2枚以下の時は「不明」としか表示されません。


 使い方(CUI版)
--------

「waifu2x-caffe-cui.exe」はコマンドラインツールです。
`コマンドプロンプト` を立ち上げ、次のようにコマンドを打ち込み、使用して下さい。


以下のコマンドは、使い方を画面に出力します。
```
waifu2x-caffe-cui.exe --help
```

以下のコマンドは、画像変換を実行するコマンドの例です。
```
waifu2x-caffe-cui.exe -i mywaifu.png -m noise_scale --scale_ratio 1.6 --noise_level 2
```
以上を実行すると、`mywaifu(noise_scale)(Level2)(x1.600000).png`に変換結果が保存されます。

コマンドリスト、各コマンドの詳細はコマンドラインオプション(共通)とコマンドラインオプション(CUI版)の項をお読みください。


 コマンドラインオプション(共通)
--------

本ソフトでは、以下のオプションを指定することが出来ます。
GUI版では入力ファイル以外のコマンドラインオプションを指定して起動した場合、現在オプションのファイル保存を行いません。
また、GUI版では指定されなかったオプションは前回終了時のオプションが使用されます。

###-l <文字列>,  --input_extention_list <文字列>
     input_fileがフォルダの場合の、フォルダ内の変換する画像の拡張子を指定します。
     デフォルト値は`png:jpg:jpeg:tif:tiff:bmp:tga`です。
     また、区切り文字は`:`です。
     例. png:jpg:jpeg:tif:tiff:bmp:tga

###-e <文字列>,  --output_extention <文字列>
     input_fileがフォルダの場合の、出力画像の拡張子を指定します。
     デフォルト値は`png`です。

###-m <noise|scale|noise_scale>,  --mode <noise|scale|noise_scale>
     変換モードを指定します。指定しなかった場合は`noise_scale`が選択されます。
      * noise : ノイズ除去を行います (正確には、ノイズ除去用のモデルを用いて画像変換を行います)
      * scale : 拡大を行います (正確には、既存アルゴリズムで拡大した後に、拡大画像補完用のモデルを用いて画像変換を行います)
      * noise_scale : ノイズ除去と拡大を行います (ノイズ除去を行った後に、引き続き拡大処理を行います)
      * auto_scale : 拡大を行います。入力がJPEG画像の場合のみノイズ除去も行います

###-s <小数点付き数値>, --scale_ratio <小数点付き数値>
     画像を何倍に拡大するかを指定します。デフォルト値は`2.0`ですが、2.0倍以外も指定できます。
     scale_widthかscale_heightが指定された場合、そちらが優先されます。
     2.0以外の数値を指定すると、次のような処理を行います。
      * まず、指定された倍率を必要十分にカバーするように、2倍拡大を繰り返し行います。
      * 2の累乗以外の数値が指定されている場合は、指定倍率になるように拡大した画像を線形フィルタで縮小します。

###-w <整数>, --scale_width <整数>
     画像の縦横比を維持したまま、指定された横幅になるように拡大します(単位はピクセル)。
     scale_heightと同時に指定すると、指定された縦横幅になるように画像を拡大します。

###-h <整数>, --scale_height <整数>
     画像の縦横比を維持したまま、指定された縦幅になるように拡大します(単位はピクセル)。
     scale_widthと同時に指定すると、指定された縦横幅になるように画像を拡大します。

###-n <0|1|2|3>, --noise_level <0|1|2|3>
     ノイズ除去レベルを指定します。ノイズ除去用のモデルはレベル0～3のみ用意されているので、
     0 か 1 か 2 か 3 を指定して下さい。
     デフォルト値は`0`です。

###-p <cpu|gpu|cudnn>, --process <cpu|gpu|cudnn>
     処理に使うプロセッサーを指定します。デフォルト値は`gpu`です。
      * cpu : CPUを使って変換を行います。
      * gpu : CUDA(GPU)を使って変換を行います。Windows版でのみ、cuDNNが使えるならcuDNNを使います。
      * cudnn : cuDNNを使って変換を行います。

###-c <整数>, --crop_size <整数>
     分割サイズを指定します。デフォルト値は`128`です。

###-q <整数>, --output_quality <整数>
     変換後の画像の画質を設定します。デフォルト値は`-1`です
     指定できる値と意味は「出力拡張子」で設定した形式により異なります。
     -1の場合は、各画像形式のデフォルト値が使われます。

###-d <整数>, --output_depth <整数>
     変換後の画像の1チャンネルあたりのビット数を指定します。デフォルト値は`8`です。
     指定できる値は「出力拡張子」で設定した形式により異なります。

###-b <整数>, --batch_size <整数>
     mini-batchサイズを指定します。デフォルト値は`1`です。
     mini-batchサイズは画像を「分割サイズ」で分割したブロックを同時に処理する数のことです。例えば`2`を指定した場合、2ブロックずつ変換していきます。
     mini-batchサイズを大きくすると分割サイズを大きくするとの同様にGPUの使用率が高くなりますが、計測した感じだと分割サイズを大きくした方が効果が高いです。
     (例えば分割サイズを`64`、mini-batchサイズを`4`にするより、分割サイズを`128`、mini-batchサイズを`1`にした方が処理が速く終わる)

### --gpu <整数>
     処理に使うGPUデバイス番号を指定します。デフォルト値は`0`です。
     GPUデバイス番号は0から始まることに注意してください。
     処理にGPUを使わない場合は無視されます。
     また、存在しないGPUデバイス番号を指定した場合はデフォルトのGPUで実行されます。

###-t <0|1>, --tta <0|1>
     `1`を指定するとTTAモードを使用します。デフォルト値は`0`です。

###--,  --ignore_rest
     このオプションが指定された後の全てのオプションを無視します。
     スクリプト・バッチファイル用です。


 コマンドラインオプション(GUI版)
--------

GUI版ではオプション指定に当てはまらなかった引数は入力ファイルとして認識されます。
入力ファイルはファイル、フォルダ、複数、ファイルとフォルダ同時に指定できます。

###-o <string>,  --output_folder <string>
     変換された画像を保存するフォルダへのパスを設定します。
     指定されたフォルダの中に変換後のファイルを保存します。
     変換後のファイルの命名規則はGUIで入力ファイルを設定した時に自動で決定される出力ファイル名と同じです。
     指定されなかった場合、ひとつ目の入力ファイルと同じフォルダに保存されます。

###--auto_start <0|1>
     `1`を指定すると起動時に自動で変換を開始します。

###--auto_exit <0|1>
     `1`を指定すると、起動時に自動で変換した場合に変換が成功すると自動で終了します。

###--no_overwrite <0|1>
     `1`を指定すると、画像の書き込み先に同名のファイルが存在する場合は変換を行いません。

###-y <upconv_7_anime_style_art_rgb|upconv_7_photo|anime_style_art_rgb|photo|anime_style_art_y>,  --model_type <upconv_7_anime_style_art_rgb|upconv_7_photo|anime_style_art_rgb|photo|anime_style_art_y>
     使用するモデルを指定します。
     GUIでの設定項目「モデル」と以下のように対応しています。
      * upconv_7_anime_style_art_rgb : 2次元イラスト(UpRGBモデル)
      * upconv_7_photo : 写真・アニメ(UpPhotoモデル)
      * anime_style_art_rgb : 2次元イラスト(RGBモデル)
      * photo : 写真・アニメ(Photoモデル)
      * anime_style_art_y : 2次元イラスト(Yモデル)


 コマンドラインオプション(CUI版)
--------

###--version
     バージョン情報を出力し、終了します。

###-?,  --help
     使い方を表示し、終了します。
     手軽に使い方を確認したい時などにどうぞ。

###-i <文字列>,  --input_file <文字列>
     (必須)  変換する画像へのパス
     フォルダを指定した場合、そのフォルダ以下の画像ファイルを全て変換してoutput_fileで指定したフォルダへ出力します。

###-o <string>,  --output_file <string>
     変換された画像を保存するファイルへのパス
     (input_fileがフォルダの場合)変換された画像を保存するフォルダへのパス
     (input_fileが画像ファイルの場合)拡張子(最後の.pngなど)は必ず入力するようにして下さい。
     指定しなかった場合は自動でファイル名を決定し、そのファイルに保存します。
     ファイル名の決定ルールは、
     `[元の画像ファイル名]``(モデル名)``(モード名)``(ノイズ除去レベル(ノイズ除去モードの場合))``(拡大率(拡大モードの場合))``(出力ビット数(8ビット以外の場合))``.出力拡張子`
     のようになっています。
     保存される場所は、基本的には入力画像と同じディレクトリになります。

###--model_dir <文字列>
     モデルが格納されているディレクトリへのパスを指定します。デフォルト値は`models/upconv_7_anime_style_art_rgb`です。
     標準では以下のモデルが付属しています。
      * `models/anime_style_art_rgb` : RGBすべてを変換する2次元画像用モデル
      * `models/anime_style_art` : 輝度のみを変換する2次元画像用モデル
      * `models/photo` : RGBすべてを変換する写真、アニメ画像用モデル
      * `models/upconv_7_anime_style_art_rgb` : anime_style_art_rgbより高速かつ同等以上の画質で変換するモデル
      * `models/upconv_7_photo` : photoより高速かつ同等以上の画質で変換するモデル
      * `models/ukbench` : 旧式の写真用モデル(拡大するモデルのみ付属しています。ノイズ除去は出来ません)
     基本的には指定しなくても大丈夫です。デフォルト以外のモデルや自作のモデルを使用する時などに指定して下さい。

###--crop_w <整数>
     分割サイズ(横幅)を指定します。設定しなかった場合はcrop_sizeの値が使用されます。
     入力する画像の横幅の約数を指定するとより高速に変換できる可能性があります。

###--crop_h <整数>
     分割サイズ(縦幅)を指定します。設定しなかった場合はcrop_sizeの値が使用されます。
     入力する画像の縦幅の約数を指定するとより高速に変換できま可能性があります。


 分割サイズ
--------

waifu2x-caffe(waifu2xもですが)は画像を変換する時、
画像を一定のサイズ毎に分割して一つずつ変換を行い、最後に結合して一枚の画像に戻す、という処理をしています。
分割サイズ(crop_size)とは、この画像を分割する際の幅（ピクセル単位）の事です。

CUDAで変換中でもGPUを使い切れていない（GPUの使用率が100%近くまでいっていない）場合、
この数値を大きくすることで処理が早く終わる可能性があります。（GPUを使い切ることが出来る様になるため）
[GPU-Z](http://www.techpowerup.com/gpuz/)などでGPU Load(GPU使用率)とMemory Used(VRAM使用率)を見ながら調節してみて下さい。
また、以下の様な特性があるので参考にして下さい。

 * 必ずしも数値が大きければ大きいほど速くなるわけでは無い
 * 分割サイズが画像の縦横サイズの約数（あるいは割ったときに余りが少ない数）だと、無駄に演算する量が減って速くなる。 (この条件にあまり当てはまらない数値が最速になるケースもあるらしい？)
 * 数値を2倍にした場合、理論上は使用するメモリ量は4倍になる(実際は3～4倍といったところ)のでソフトが落ちないように注意。特にCUDAはcuDNNに比べてメモリの消費量がとても多いので気をつけること


 アルファチャンネル付き画像について
--------

本ソフトではアルファチャンネル付き画像の拡大も対応しています。
しかし、アルファチャンネルを単体で拡大する処理になっているため、アルファチャンネル付き画像の拡大は無い場合と比べておよそ2倍の時間がかかるので注意してください。
ただし、アルファチャンネルが単色で構成されている場合はなしの場合とほぼ同じ時間で拡大できます。


 The format of language files
--------

Language files format is JSON.
If you create new language file, add language setting to 'lang/LangList.txt'.
'lang/LangList.txt' format is TSV(Tab-Separated Values).

  * LangName : Language name
  * LangID : Primary language [See MSDN](https://msdn.microsoft.com/en-us/library/windows/desktop/dd318693.aspx)
  * SubLangID : Sublanguage [See MSDN](https://msdn.microsoft.com/en-us/library/windows/desktop/dd318693.aspx)
  * FileName : Language file name

ex.

  * Japanese  LangID : 0x11(LANG_JAPANESE), SubLangID : 0x01(SUBLANG_JAPANESE_JAPAN)
  * English(US) LangID : 0x09(LANG_ENGLISH), SubLangID : 0x01(SUBLANG_ENGLISH_US)
  * English(UK) LangID : 0x09(LANG_ENGLISH), SubLangID : 0x02(SUBLANG_ENGLISH_UK)


おことわり
------------

本ソフトは無保証です。
利用者の判断の下に使用して下さい。
制作者はいかなる義務も負わないものとします。


謝辞
------
オリジナルの[waifu2x](https://github.com/nagadomi/waifu2x)、及びモデルの制作を行い、MITライセンスの下で公開して下さった [ultraist](https://twitter.com/ultraistter)さん、
オリジナルのwaifu2xを元に[waifu2x-converter](https://github.com/WL-Amigo/waifu2x-converter-cpp)を作成して下さった [アミーゴ](https://twitter.com/WL_Amigo)さん(READMEやLICENSE.txtの書き方、OpenCVの使い方等かなり参考にさせていただきました)
に、感謝します。
また、メッセージを英訳してくださった @paul70078 さん、メッセージを中国語(簡体字)に翻訳してくださった @yoonhakcher さん、中国語(簡体字)訳のプルリクエストを下さった @mzhboy さん、
メッセージを韓国語に翻訳してくださった @kenin0726 さん、韓国語訳の改善を提案してくださった @aruhirin さん、
メッセージを中国語(繁体字)に翻訳してくださった @lizardon1995 さん、@yoonhakcher さん、トルコ語訳のプルリクエストを下さった @Scharynche さん、フランス語訳のプルリクエストを下さった @Serized さん、
GUI版のアイコンを提供してくださった JYUNYAさん に感謝します。
