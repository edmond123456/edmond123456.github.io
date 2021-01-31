---
layout:     post
title:      Network based on SfmLearner
subtitle:    
date:       2019-03-31
author:     Junjie Hua
header-img: img/post-bg-keybord.jpg
catalog: true
tags:
    - Internship
---

##　Internship @Cinet 2018/07-2019/03



## 7/29
```
mac下bash的基本命令
https://blog.csdn.net/qq_18832439/article/details/69790323

ipython notebook
コードの実行
ls -a /users/junjie_hua/Documents/Github/SfMLearner-master

demoの実行
bash /users/junjie_hua/Documents/Github/SfMLearner-master/models/download_depth_model.sh
モデルをダウンロードしたが、実行すると問題起こる
```

## 9/7
```




学習データの準備
参考　https://github.com/tinghuiz/SfMLearner
手順：
	1.http://www.cvlibs.net/download.php?file=raw_data_downloader.zipからデータセットをダウンロード
	2. wgetをインストール　http://rudix.org/packages/wget.html --成功
	3. cd  /Users/junjie_hua/Documents/GitHub/raw_data
	4../raw_data_downloader.sh







```

## 9/27
```
Virtualでの設定
https://blog.easwy.com/archives/proxy-setting-on-linux-console/
pyenvでPythonのAnaconda環境構築
https://qiita.com/aical/items/2d066801a7464a676994
Linuxbrewをインストール　
```


## 10/04


## 10/04



## 10/25

```
動画　ダウンロード　　フレーム　分解
切り出す
３枚　出力してみる
Image magical 　　ffmpeg (homebrew)
cmlでできるように　.bash  10sampleぐらい

Ffmpeg download—成功
https://looooooooop.blog.fc2.com/blog-entry-1021.html#fmai
https://www.jianshu.com/p/91727ab25227
Imagemajick download—成功

自動化 shell
https://eng-entrance.com/linux-shellscript-do
http://www.runoob.com/linux/linux-shell.html
https://github.com/qinjx/30min_guides/blob/master/shell.md

imagemagick
http://www.netingcn.com/imagemagick-composite.html

#!/usr/bin/bash
#!/usr/bin/python

cd /Users/junjie_hua/personal/video_data
rm -rf test
mkdir test
ls

cp -R video.mp4 /Users/junjie_hua/personal/video_data/test/1.mp4
cd test

ffmpeg -i 1.mp4 -f image2 -vf fps=fps=1/20 data%d.png
#ls /Users/junjie_hua/personal/video_data/test

convert +append data1.png data2.png data3.png 1.png



#echo OK!
```
![ffmpeg](https://edmond123456.github.io/img/my_fig/ffmpeg.png)


##11/15

自動化更新：
	* 
アドレス  ok 
	* 
ファイル名 ok
	* 
Loop  ok


 http://www.protein.osaka-u.ac.jp/rcsfp/supracryst/suzuki/jpxtal/Katsutani/gif1.php
https://www.cnblogs.com/wangzhao2016/p/5708489.html
https://blog.csdn.net/seteor/article/details/17377963

```
#!/usr/bin/bash
#!/usr/bin/python

read -p "please input address of the video files: " ip_video # /Users/junjie_hua/personal/video_data
read -p "please input name of the video files: " name_video # video.mp4
read -p "please input address where you want to save: " ip_save # /Users/junjie_hua/personal/video_data
cd $ip_save
#pwd
rm -rf test
mkdir test
#ls

cd $ip_video
cp -R $name_video /Users/junjie_hua/personal/video_data/test/1.mp4
cd test

ffmpeg -i 1.mp4 -f image2 -vf fps=fps=1/10 data%d.png
#ls /Users/junjie_hua/personal/video_data/test

#convert +append data1.png data2.png data3.png 1.png

num=0
add=1
for file in *.png
do num=$[$num+$add]
done
echo "We have $num png files."


int=1
add_1=1
add_2=2
devide=3
while(($int<=$[$num / $devide]))
do
convert +append "data"$[$int*3-$add_2]".png" "data"$[$int*3-$add_1]".png" "data"$[$int*3]".png" ""$int".png"
let "int++"
done


echo DONE!
```
##結果
60sのビデオを6フレームに分けて、123を一つにする　456を一つにする

![cut](https://edmond123456.github.io/img/my_fig/cut.png)

## 2018年12月13日 



## 2018年12月27日 
Pilで画像を保存する--成功
https://www.cnblogs.com/denny402/p/5121897.html
http://www.itboth.com/d/NNF3ee/python


## 2019年1月31日 



## 2019年2月14日
ファイル名揃え作業--完了
https://bbs.csdn.net/topics/390316131

## 2019年2月21日 
	* slide切り出し--完了
https://blog.csdn.net/qq_33728573/article/details/57124124
https://codeday.me/bug/20180615/179041.html
https://codeday.me/bug/20170525/18565.html

## 2019年3月5日 
zarr groups
https://zarr.readthedocs.io/en/stable/tutorial.html#groups
Zarr pytorch
https://city.shaform.com/zh/2018/11/11/zarr/



## 2019年3月7日 
