# darknet_setup
darknetの設定手順を記録します
環境:Ubuntu16.04, kernel:4.15.0-64-generic  
  
1. darknetをクローン[1](今回はpjreddieの代わりにAlexeyABを使用)  
> git clone https://github.com/AlexeyAB/darknet  
> cd darknet  

2. makefileを編集[2]
- GPU,CUDNN,OPENCV,OPENMP,LIBSOを1に変更  
  
3. openmp, opencvをインストール
> sudo apt install libome-dev  
> sudo apt install libopencv-dev  
  
4. cuda9.0とcuDNN7.0をインストール[3]
※長いので参考文献参照  
※著者はこの時点でnvidia-418とcuda-9-0を導入し、nvidia-smiではなぜかcuda10.1と認識された。/usr/local/cuda/version.txtにはCUDA Version 9.0.176と書かれていたため問題ないと判断して先にすすめる  
　　
5. cmakeのアップグレード(3.5.1 -> 3.15.4)  
darknetビルド時にUbuntuのaptで入る最新よりも新しいcmakeを要求されるので、現行の最新をインストール[4][5]  
(バイナリは不要。ソースからビルドするべし)  
(Ubuntuを使用しているので"sudo make install"は禁止。"sudo checkinstall"を使用すること。)  
> wget https://cmake.org/files/v3.15/cmake-3.15.4.tar.gz  
> tar -xzvf cmake-3.15.4.tar.gz  
> cd cmake-3.15.4  
> ./bootstrap  
> make -j8  
> sudo apt purge --auto-remove cmake #ここで依存パッケージを大量に消されます。すぐ入れなおすのでこの時に消されたパッケージ一覧は適当なテキストエディタでメモしておくこと。  
> sudo checkinstall  
> #ここで、cmakeに巻き込まれて消されたやつをすべて入れなおす  
  
6. darknetのディレクトリに戻ってビルド  
> make -j8  
  
7. 自前のデータで学習[6]  
> VoTTで出力したデータの場所へ移動(yolo-obj.cfgの存在するディレクトリ)  
> #以下darknet本体が~/workspace/darknet_workspace/darknetにある場合  
> ~/workspace/darknet_workspace/darknet/darknet detector train ./data/obj.data ./yolo-obj.cfg  
  
  
<参考文献>  
[1]http://weekendproject9.hatenablog.com/entry/2018/04/30/205622  
[2]https://qiita.com/yamamo-to/items/85c324e076ae87ece8eb  
[3]https://qiita.com/JeJeNeNo/items/b30597918db3781e20cf  
[4]https://qiita.com/koara-local/items/9d01c6bb9dd93563b7c6  
[5]https://askubuntu.com/questions/355565/how-do-i-install-the-latest-version-of-cmake-from-the-command-line  
[6]https://qiita.com/KTake/items/73cc995d6f25247b6864  

