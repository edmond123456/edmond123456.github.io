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
Single-view demoの再現--完了
実行手順：
	1. terminalで ipython notebookと打つ
	2. Documents
	3. GitHub
	4. SfMLearner-master
	5. demo1を開く
	6. kernelでrestart & clear output
	7. kernelでrestart & run all




学習データの準備
参考　https://github.com/tinghuiz/SfMLearner
手順：
	1.http://www.cvlibs.net/download.php?file=raw_data_downloader.zipからデータセットをダウンロード
	2. wgetをインストール　http://rudix.org/packages/wget.html --成功
	3. cd  /Users/junjie_hua/Documents/GitHub/raw_data
	4../raw_data_downloader.sh



	1. cd  /Users/junjie_hua/Documents/GitHub/SfMLearner-master


	1. python data/prepare_train_data.py --dataset_dir=/path/to/raw/kitti/dataset/ --dataset_name='kitti_raw_eigen' --dump_root=/path/to/resulting/formatted/data/ --seq_length=3 --img_width=416 --img_height=128 --num_threads=4
	2. PermissionError: [Errno 13] Permission denied: '/path'



```

## 9/27
```
Virtualでの設定
https://blog.easwy.com/archives/proxy-setting-on-linux-console/
pyenvでPythonのAnaconda環境構築
https://qiita.com/aical/items/2d066801a7464a676994
Linuxbrewをインストール　○

現状１
brew install pyenv
$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
を入れてからまたbraincs-sg01にアクセスできなくなる

Linuxbrewを通してインストールしたpyenvは元々のpyenvと…?

現状２
vitualではlinuxbrewのフォルダがあるが、ターミナルから立ち上がらない


```


## 10/04
```
訓練データ
/project/dnn-modeling/shared/data/KITTI_raw_data 


/Users/junjie_hua/brain

sfMLearnerをダウンロード
git clone https://github.com/tinghuiz/SfMLearner.git

手順：

ssh braincs-hp01
source envs-sg/tflow/bin/activate
cd  SfMLearner

(Prepare formatted data)

python data/prepare_train_data.py --dataset_dir=/project/dnn-modeling/shared/data/KITTI_raw_data/ --dataset_name='kitti_raw_eigen' --dump_root=/home/junjie_hua/formatteddata/ --seq_length=3 --img_width=416 --img_height=128 --num_threads=4
Result: permission denied

(Train data)

python train.py --dataset_dir= /project/dnn-modeling/shared/data/KITTI_raw_data/ --checkpoint_dir= /home/junjie_hua/check /--img_width=416 --img_height=128 --batch_size=4


```

## 10/04
```
手順：
★

ssh braincs-hp01
source envs-sg/tflow/bin/activate
cd  SfMLearner

(Prepare formatted data)

python data/prepare_train_data.py --dataset_dir=/project/dnn-modeling/shared/data/KITTI_raw_data/ --dataset_name='kitti_raw_eigen' --dump_root=/home/junjie_hua/formatteddata/ --seq_length=3 --img_width=416 --img_height=128 --num_threads=4

問題

/gpfs/data/home/junjie_hua/SfMLearner/data/kitti/kitti_raw_loader.py in load_image_raw(self=<kitti.kitti_raw_loader.kitti_raw_loader object>, drive='2011_09_26_drive_0079_sync', cid='02', frame_id='0000000000')
    113         return example
    114
    115     def load_image_raw(self, drive, cid, frame_id):
    116         date = drive[:10]
    117         img_file = os.path.join(self.dataset_dir, date, drive, 'image_' + cid, 'data', frame_id + '.png')
--> 118         img = scipy.misc.imread(img_file)
        img = undefined
        img_file = '/project/dnn-modeling/shared/data/KITTI_raw_data...9_26_drive_0079_sync/image_02/data/0000000000.png'
    119         return img
    120
    121     def load_intrinsics_raw(self, drive, cid, frame_id):
    122         date = drive[:10]

AttributeError: 'module' object has no attribute 'imread'

解決策　https://stackoverflow.com/questions/15345790/scipy-misc-module-has-no-attribute-imread

You need to install Pillow (formerly PIL). From the docs on scipy.misc:

結果ー＞
もう一つの問題

/gpfs/data/home/junjie_hua/envs-sg/tflow/lib/python2.7/site-packages/joblib/externals/loky/backend/popen_loky_posix.py in dump_example(n=1)
     34     intrinsics = example['intrinsics']
     35     fx = intrinsics[0, 0]
     36     fy = intrinsics[1, 1]
     37     cx = intrinsics[0, 2]
     38     cy = intrinsics[1, 2]
---> 39     dump_dir = os.path.join(args.dump_root, example['folder_name'])
     40     # if not os.path.isdir(dump_dir):
     41     #     os.makedirs(dump_dir, exist_ok=True)
     42     try:
     43         os.makedirs(dump_dir)

AttributeError: 'Namespace' object has no attribute 'dump_root'
解決策

python data/prepare_train_data.py --dataset_dir=/project/dnn-modeling/shared/data/KITTI_raw_data/ --dataset_name='kitti_raw_eigen' --dump_root=/home/junjie_hua/SfMLearner/formatteddata/ --seq_length=3 --img_width=416 --img_height=128 --num_threads=4

(Training)

python train.py --dataset_dir=/home/junjie_hua/SfMLearner/formatteddata/ --checkpoint_dir=/home/junjie_hua/SfMLearner/checkpoints/ --img_width=416 --img_height=128 --batch_size=4
成功

リモートでjupyter notebookにアクセス
https://qiita.com/Miggy/items/5466a2c1e968602f3ebe
 

Out[1]: 'sha1:95c09c193eb1:ef8dd40230884128c007112156cf1875f243969c'

```


## 10/25
```
Braincs-sg01で
python train.py --dataset_dir=/home/junjie_hua/SfMLearner/formatteddata/ --checkpoint_dir=/home/junjie_hua/SfMLearner/checkpoints/ --img_width=416 --img_height=128 --batch_size=4を実行すると

tensorflow.python.framework.errors_impl.NotFoundError: /home/junjie_hua/SfMLearner/formatteddata/2011_09_26_drive_0018_sync_03/0000000091_cam.txt; No such file or directory
確認した

先生のファイルを比較し、prepare_train_data.pyの

27  def dump_example(n,args):


88  Parallel(n_jobs=args.num_threads)(delayed(dump_example)(n,args) for n in range(data_loader.num_train))
もう一回prepare_train_data.pyを実行すると

python data/prepare_train_data.py --dataset_dir=/project/dnn-modeling/shared/data/KITTI_raw_data/ --dataset_name='kitti_raw_eigen' --dump_root=/home/junjie_hua/SfMLearner/formatteddata/ --seq_length=3 --img_width=416 --img_height=128 --num_threads=10
成功

Train.pyを実行すると

python train.py --dataset_dir=/home/junjie_hua/SfMLearner/formatteddata/ --checkpoint_dir=/home/junjie_hua/SfMLearner/checkpoints2/ --img_width=416 --img_height=128 —batch_size=4
成功


tensorboardを立ち上げる

source envs-sg/tflow/bin/activate
tensorboard --logdir=/home/junjie_hua/SfMLearner/checkpoints2/ --port=8889

ssh -N -f -L localhost:8888:localhost:8889 junjie_hua@braincs-sg01

http://localhost:8888/
成功
```
![tensor1](https://edmond123456.github.io/img/my_fig/tensor1.png)

![tensor2](https://edmond123456.github.io/img/my_fig/tensor2.png)


```
(Evaluation on KITTI)

Depth
ダウンロード完了

python kitti_eval/eval_depth.py --kitti_dir=/project/dnn-modeling/shared/data/KITTI_raw_data/ --pred_file=kitti_eval/kitti_eigen_depth_predictions.npy


下記の結果を得た：

abs_rel,     sq_rel,        rms,    log_rms,     d1_all,         a1,         a2,         a3
    0.1978,     1.8363,     6.5645,     0.2750,     0.0000,     0.7176,     0.9010,     0.9606

Pose
ダウンロード完了

python kitti_eval/eval_pose.py --gtruth_dir=kitti_eval/pose_data/ground_truth/10/ --pred_dir=kitti_eval/pose_data/ours_results/10/
dirが違います


(KITTI testing code)
Depth

Sample model

bash ./models/download_depth_model.sh


obtain the single-view depth predictions on the KITTI eigen test split formatted properly for evaluation by running

python test_kitti_depth.py --dataset_dir /project/dnn-modeling/shared/data/KITTI_raw_data/ --output_dir /home/junjie_hua/SfMLearner/test_depth --ckpt_file /home/junjie_hua/SfMLearner/models/
下記の結果を得た

NotFoundError (see above for traceback): Unsuccessful TensorSliceReader constructor: Failed to find any matching files for /home/junjie_hua/SfMLearner/models/
```


## 11/01
```
(Evaluation on KITTI)
Pose
ダウンロード完了

(tflow)junjie_hua@braincs-sg01:~/SfMLearner$ bash ./kitti_eval/download_kitti_pose_eval_data.sh
警告: タイムスタンプの比較は -O では無効です。
詳しくはマニュアルを参照してください。

--2018-11-01 13:21:44--  http://people.eecs.berkeley.edu/~tinghuiz/projects/SfMLearner/pose_eval_data.tar
proxy.nict.go.jp (proxy.nict.go.jp) をDNSに問いあわせています... 133.243.18.8
proxy.nict.go.jp (proxy.nict.go.jp)|133.243.18.8|:3128 に接続しています... 接続しました。
Proxy による接続要求を送信しました、応答を待っています... 200 OK
長さ: 9400320 (9.0M) [application/x-tar]
`./kitti_eval/pose_eval_data.tar' に保存中

100%[==========================================================>] 9,400,320   1.59MB/s 時間 11s    

2018-11-01 13:21:55 (870 KB/s) - `./kitti_eval/pose_eval_data.tar' へ保存完了 [9400320/9400320]


python kitti_eval/eval_pose.py --gtruth_dir=kitti_eval/pose_data/ground_truth/10/ --pred_dir=kitti_eval/pose_data/ours_results/10/
結果

ATE mean: 0.0202, std: 0.0152


(KITTI testing code)
https://blog.csdn.net/songyu0120/article/details/77966814

Depth
Sample model

(tflow)junjie_hua@braincs-sg01:~/SfMLearner$ bash ./models/download_depth_model.sh
警告: タイムスタンプの比較は -O では無効です。
詳しくはマニュアルを参照してください。

--2018-11-01 13:35:25--  http://people.eecs.berkeley.edu/~tinghuiz/projects/SfMLearner/models/kitti_depth_model.tar
proxy.nict.go.jp (proxy.nict.go.jp) をDNSに問いあわせています... 133.243.18.8
proxy.nict.go.jp (proxy.nict.go.jp)|133.243.18.8|:3128 に接続しています... 接続しました。
Proxy による接続要求を送信しました、応答を待っています... 200 OK
長さ: 139591680 (133M) [application/x-tar]
`./models/kitti_depth_model.tar' に保存中



python test_kitti_depth.py --dataset_dir /project/dnn-modeling/shared/data/KITTI_raw_data/ --output_dir /home/junjie_hua/SfMLearner/test_depth --ckpt_file /home/junjie_hua/SfMLearner/models/model-190532
成功


Pose
Sample download

(tflow)junjie_hua@braincs-sg01:~/SfMLearner$ bash ./models/download_pose_model.sh
警告: タイムスタンプの比較は -O では無効です。
詳しくはマニュアルを参照してください。

--2018-11-01 14:00:46--  http://people.eecs.berkeley.edu/~tinghuiz/projects/SfMLearner/models/kitti_pose_model.tar
proxy.nict.go.jp (proxy.nict.go.jp) をDNSに問いあわせています... 133.243.18.8
proxy.nict.go.jp (proxy.nict.go.jp)|133.243.18.8|:3128 に接続しています... 接続しました。
Proxy による接続要求を送信しました、応答を待っています... 200 OK
長さ: 148981760 (142M) [application/x-tar]
`./models/kitti_pose_model.tar' に保存中



python test_kitti_pose.py --test_seq 9 --dataset_dir /project/dnn-modeling/shared/data/KITTI_raw_data/ --output_dir /home/junjie_hua/SfMLearner/test_pose --ckpt_file /home/junjie_hua/SfMLearner/models/model-100280



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

Mode=‘poses’
結果：おそらく最初からはpose推定用の資料がないため

ValueError: No variables to save


Pose test
```
python test_kitti_pose.py --test_seq 9 --dataset_dir /project/dnn-modeling/shared/data/VideoBlocksStimuli20150212/trial001/ --output_dir /home/junjie_hua/brain_SfMLearner/test_pose --ckpt_file /home/junjie_hua/brain_SfMLearner/models/model-100280
```
Error
```
IOError: [Errno 2] No such file or directory: '/project/dnn-modeling/shared/data/VideoBlocksStimuli20150212/trial001/sequences/09/times.txt'
```
Kitti_odometry data/をダウンロード--完了
場所

/home/junjie_hua/brain_SfMLearner/dataset
test_kitti_pose.pyを変える
もう一回実行するとーー成功?


```
Traceback (most recent call last):
  File "test_kitti_pose.py", line 109, in <module>
    main()
  File "test_kitti_pose.py", line 100, in main
    FLAGS.img_width)
  File "test_kitti_pose.py", line 32, in load_image_sequence
    curr_frame_id = frames[curr_idx]
IndexError: list index out of range
```

![learning](https://edmond123456.github.io/img/my_fig/learning.png)


## 2018年12月27日 
Pilで画像を保存する--成功
https://www.cnblogs.com/denny402/p/5121897.html
http://www.itboth.com/d/NNF3ee/python


## 2019年1月31日 
Testフォルダの下にあるファイルーーファイル名を整理する必要がる
```
['im0000069.jpg', 'im0000177.jpg', 'im0000153.jpg', '._im0000079.jpg', 'im0000102.jpg', '._im0000060.jpg', 'im0000113.jpg', 'im0000067.jpg', '._im0000004.jpg', 'im0000068.jpg', '._im0000014.jpg', '._im0000076.jpg', 'im0000034.jpg', 'im0000089.jpg', '._im0000015.jpg', 'im0000093.jpg', 'im0000169.jpg', '._im0000045.jpg', 'im0000005.jpg', 'im0000050.jpg', '._im0000037.jpg', '._im0000065.jpg', 'im0000175.jpg', 'im0000167.jpg', 'im0000100.jpg', 'im0000136.jpg', 'im0000138.jpg', 'im0000165.jpg', '._im0000051.jpg', '._im0000011.jpg', 'im0000158.jpg', '._im0000087.jpg', '._im0000061.jpg', '._im0000021.jpg', 'im0000101.jpg', '._im0000048.jpg', '._im0000068.jpg', 'im0000192.jpg', '._im0000010.jpg', '._im0000063.jpg', 'im0000095.jpg', 'im0000162.jpg', 'im0000190.jpg', 'im0000077.jpg', '._im0000096.jpg', '._im0000016.jpg', 'im0000119.jpg', 'im0000051.jpg', 'im0000014.jpg', 'im0000174.jpg', 'im0000072.jpg', 'im0000139.jpg', '._im0000077.jpg', 'im0000125.jpg', 'im0000168.jpg', 'im0000170.jpg', '._im0000001.jpg', 'im0000156.jpg', 'im0000070.jpg', '._im0000049.jpg', 'im0000018.jpg', '._im0000073.jpg', 'im0000047.jpg', '._im0000062.jpg', '._im0000083.jpg', 'im0000020.jpg', 'im0000022.jpg', 'im0000003.jpg', '._im0000042.jpg', 'im0000133.jpg', '._im0000086.jpg', 'im0000056.jpg', '._im0000055.jpg', 'im0000007.jpg', '._im0000030.jpg', 'im0000012.jpg', 'im0000037.jpg', '._im0000075.jpg', 'im0000130.jpg', 'im0000096.jpg', '._im0000035.jpg', 'im0000011.jpg', 'im0000028.jpg', 'im0000143.jpg', 'im0000184.jpg', 'im0000035.jpg', 'im0000161.jpg', 'im0000036.jpg', 'im0000164.jpg', 'im0000134.jpg', 'im0000154.jpg', '._im0000047.jpg', '._im0000070.jpg', 'im0000075.jpg', 'im0000189.jpg', '._im0000090.jpg', 'im0000039.jpg', 'im0000131.jpg', '._im0000059.jpg', 'im0000013.jpg', 'im0000196.jpg', 'im0000004.jpg', 'im0000052.jpg', '._im0000078.jpg', 'im0000032.jpg', '._im0000023.jpg', 'im0000090.jpg', 'im0000040.jpg', 'im0000182.jpg', 'im0000166.jpg', '._im0000038.jpg', 'im0000043.jpg', '._im0000013.jpg', 'im0000132.jpg', 'im0000124.jpg', 'im0000180.jpg', 'im0000060.jpg', 'im0000163.jpg', 'im0000015.jpg', 'im0000062.jpg', 'im0000178.jpg', '._im0000005.jpg', 'im0000044.jpg', 'im0000195.jpg', 'im0000104.jpg', 'im0000092.jpg', 'im0000023.jpg', 'im0000038.jpg', 'im0000199.jpg', 'im0000141.jpg', 'im0000135.jpg', 'im0000144.jpg', 'im0000027.jpg', 'im0000200.jpg', 'im0000137.jpg', 'im0000127.jpg', 'im0000057.jpg', 'im0000159.jpg', 'im0000146.jpg', '._im0000034.jpg', 'im0000006.jpg', 'im0000046.jpg', 'im0000010.jpg', 'im0000176.jpg', 'im0000030.jpg', 'im0000194.jpg', 'im0000066.jpg', '._im0000043.jpg', 'im0000151.jpg', 'im0000091.jpg', '._im0000089.jpg', '._im0000088.jpg', 'im0000197.jpg', 'im0000111.jpg', '._im0000017.jpg', '._im0000085.jpg', 'im0000041.jpg', 'im0000150.jpg', '._im0000040.jpg', 'im0000103.jpg', 'im0000108.jpg', 'im0000112.jpg', '._im0000041.jpg', 'im0000172.jpg', 'im0000080.jpg', '._im0000032.jpg', '._im0000099.jpg', '._im0000066.jpg', '._im0000036.jpg', '._im0000071.jpg', '._im0000092.jpg', '._im0000074.jpg', '._im0000031.jpg', 'im0000181.jpg', 'im0000105.jpg', '._im0000006.jpg', 'im0000045.jpg', '._im0000033.jpg', '._im0000080.jpg', '._im0000054.jpg', 'im0000106.jpg', 'im0000109.jpg', '._im0000091.jpg', '._im0000008.jpg', 'im0000193.jpg', 'im0000053.jpg', 'im0000002.jpg', 'im0000061.jpg', '._im0000069.jpg', 'im0000114.jpg', 'im0000042.jpg', 'im0000085.jpg', 'im0000123.jpg', 'im0000017.jpg', '._im0000012.jpg', 'im0000048.jpg', '._im0000026.jpg', '._im0000007.jpg', '._im0000050.jpg', 'im0000157.jpg', '._im0000081.jpg', 'im0000083.jpg', '._im0000027.jpg', '._im0000039.jpg', '._im0000024.jpg', 'im0000179.jpg', 'im0000118.jpg', 'im0000059.jpg', '._im0000044.jpg', '._im0000029.jpg', 'im0000171.jpg', '._im0000064.jpg', 'im0000058.jpg', '._im0000067.jpg', 'im0000145.jpg', 'im0000065.jpg', 'im0000140.jpg', 'im0000071.jpg', 'im0000098.jpg', 'im0000120.jpg', '._im0000003.jpg', 'im0000063.jpg', 'im0000122.jpg', 'im0000121.jpg', 'im0000074.jpg', '._im0000098.jpg', 'im0000008.jpg', 'im0000147.jpg', 'im0000078.jpg', 'im0000152.jpg', 'im0000031.jpg', 'im0000087.jpg', '._im0000053.jpg', 'im0000001.jpg', '._im0000002.jpg', 'im0000054.jpg', 'im0000099.jpg', 'im0000185.jpg', '._im0000022.jpg', 'im0000186.jpg', '._im0000082.jpg', 'im0000148.jpg', 'im0000097.jpg', '._im0000056.jpg', 'im0000116.jpg', 'im0000086.jpg', 'im0000055.jpg', '._im0000020.jpg', 'im0000107.jpg', '._im0000018.jpg', '._im0000094.jpg', 'im0000149.jpg', '._im0000009.jpg', '._im0000097.jpg', 'im0000049.jpg', 'im0000064.jpg', '._im0000072.jpg', 'im0000073.jpg', 'im0000082.jpg', 'im0000115.jpg', 'im0000191.jpg', 'im0000025.jpg', 'im0000126.jpg', 'im0000019.jpg', '._im0000095.jpg', 'im0000129.jpg', 'im0000009.jpg', 'im0000084.jpg', '._im0000093.jpg', 'im0000024.jpg', '._im0000052.jpg', 'im0000021.jpg', '._im0000025.jpg', 'im0000076.jpg', '._im0000057.jpg', '._im0000019.jpg', 'im0000160.jpg', 'im0000016.jpg', 'im0000094.jpg', 'im0000110.jpg', 'im0000117.jpg', 'im0000187.jpg', 'im0000033.jpg', '._im0000058.jpg', '._im0000084.jpg', 'im0000029.jpg', 'im0000142.jpg', 'im0000081.jpg', 'im0000173.jpg', 'im0000155.jpg', 'im0000026.jpg', 'im0000088.jpg', '._im0000046.jpg', 'im0000128.jpg', 'im0000183.jpg', 'im0000079.jpg', '._im0000028.jpg', 'im0000198.jpg', 'im0000188.jpg']
```

## 2019年2月4日 
https://www.oschina.net/question/1264215_130602
https://blog.csdn.net/liuyudong_/article/details/80319592

ファイル名を整理するーー完了
```
['im0000069.jpg', 'im0000177.jpg', 'im0000153.jpg', 'im0000102.jpg', 'im0000113.jpg', 'im0000067.jpg', 'im0000068.jpg', 'im0000034.jpg', 'im0000089.jpg', 'im0000093.jpg', 'im0000169.jpg', 'im0000005.jpg', 'im0000050.jpg', 'im0000175.jpg', 'im0000167.jpg', 'im0000100.jpg', 'im0000136.jpg', 'im0000138.jpg', 'im0000165.jpg', 'im0000158.jpg', 'im0000101.jpg', 'im0000192.jpg', 'im0000095.jpg', 'im0000162.jpg', 'im0000190.jpg', 'im0000077.jpg', 'im0000119.jpg', 'im0000051.jpg', 'im0000014.jpg', 'im0000174.jpg', 'im0000072.jpg', 'im0000139.jpg', 'im0000125.jpg', 'im0000168.jpg', 'im0000170.jpg', 'im0000156.jpg', 'im0000070.jpg', 'im0000018.jpg', 'im0000047.jpg', 'im0000020.jpg', 'im0000022.jpg', 'im0000003.jpg', 'im0000133.jpg', 'im0000056.jpg', 'im0000007.jpg', 'im0000012.jpg', 'im0000037.jpg', 'im0000130.jpg', 'im0000096.jpg', 'im0000011.jpg', 'im0000028.jpg', 'im0000143.jpg', 'im0000184.jpg', 'im0000035.jpg', 'im0000161.jpg', 'im0000036.jpg', 'im0000164.jpg', 'im0000134.jpg', 'im0000154.jpg', 'im0000075.jpg', 'im0000189.jpg', 'im0000039.jpg', 'im0000131.jpg', 'im0000013.jpg', 'im0000196.jpg', 'im0000004.jpg', 'im0000052.jpg', 'im0000032.jpg', 'im0000090.jpg', 'im0000040.jpg', 'im0000182.jpg', 'im0000166.jpg', 'im0000043.jpg', 'im0000132.jpg', 'im0000124.jpg', 'im0000180.jpg', 'im0000060.jpg', 'im0000163.jpg', 'im0000015.jpg', 'im0000062.jpg', 'im0000178.jpg', 'im0000044.jpg', 'im0000195.jpg', 'im0000104.jpg', 'im0000092.jpg', 'im0000023.jpg', 'im0000038.jpg', 'im0000199.jpg', 'im0000141.jpg', 'im0000135.jpg', 'im0000144.jpg', 'im0000027.jpg', 'im0000200.jpg', 'im0000137.jpg', 'im0000127.jpg', 'im0000057.jpg', 'im0000159.jpg', 'im0000146.jpg', 'im0000006.jpg', 'im0000046.jpg', 'im0000010.jpg', 'im0000176.jpg', 'im0000030.jpg', 'im0000194.jpg', 'im0000066.jpg', 'im0000151.jpg', 'im0000091.jpg', 'im0000197.jpg', 'im0000111.jpg', 'im0000041.jpg', 'im0000150.jpg', 'im0000103.jpg', 'im0000108.jpg', 'im0000112.jpg', 'im0000172.jpg', 'im0000080.jpg', 'im0000181.jpg', 'im0000105.jpg', 'im0000045.jpg', 'im0000106.jpg', 'im0000109.jpg', 'im0000193.jpg', 'im0000053.jpg', 'im0000002.jpg', 'im0000061.jpg', 'im0000114.jpg', 'im0000042.jpg', 'im0000085.jpg', 'im0000123.jpg', 'im0000017.jpg', 'im0000048.jpg', 'im0000157.jpg', 'im0000083.jpg', 'im0000179.jpg', 'im0000118.jpg', 'im0000059.jpg', 'im0000171.jpg', 'im0000058.jpg', 'im0000145.jpg', 'im0000065.jpg', 'im0000140.jpg', 'im0000071.jpg', 'im0000098.jpg', 'im0000120.jpg', 'im0000063.jpg', 'im0000122.jpg', 'im0000121.jpg', 'im0000074.jpg', 'im0000008.jpg', 'im0000147.jpg', 'im0000078.jpg', 'im0000152.jpg', 'im0000031.jpg', 'im0000087.jpg', 'im0000001.jpg', 'im0000054.jpg', 'im0000099.jpg', 'im0000185.jpg', 'im0000186.jpg', 'im0000148.jpg', 'im0000097.jpg', 'im0000116.jpg', 'im0000086.jpg', 'im0000055.jpg', 'im0000107.jpg', 'im0000149.jpg', 'im0000049.jpg', 'im0000064.jpg', 'im0000073.jpg', 'im0000082.jpg', 'im0000115.jpg', 'im0000191.jpg', 'im0000025.jpg', 'im0000126.jpg', 'im0000019.jpg', 'im0000129.jpg', 'im0000009.jpg', 'im0000084.jpg', 'im0000024.jpg', 'im0000021.jpg', 'im0000076.jpg', 'im0000160.jpg', 'im0000016.jpg', 'im0000094.jpg', 'im0000110.jpg', 'im0000117.jpg', 'im0000187.jpg', 'im0000033.jpg', 'im0000029.jpg', 'im0000142.jpg', 'im0000081.jpg', 'im0000173.jpg', 'im0000155.jpg', 'im0000026.jpg', 'im0000088.jpg', 'im0000128.jpg', 'im0000183.jpg', 'im0000079.jpg', 'im0000198.jpg', 'im0000188.jpg']
```


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

csh file
```
#!/usr/bin/env tcsh

set gpus=2
set nframes=1440
set nskip=240
set nfold=4# for validation movies

set indirtags=(Trn01 Trn02 Trn03 Trn04 Trn05 Trn06 Trn07 Val01 Val02 Val03 Val04 Val05 Val06 Val07 Val08 Val09)

if (! -d dnn_features_raw) then
mkdir dnn_features_raw
endif

set n=13

while ($n <= 16)
set gpuid=`echo "($n-1) % $gpus" | bc`
set run=`printf %02d $n`

set imagedir="/home/junjie_hua/brain_SfMLearner/dataset/formatted_data/Nishida_Vimeo_$indirtags[$n]/im*.jpg"

if ($n < 12) then
echo ./infer.py "'$imagedir'" $nframes dnn_features_raw/NishidaVimeo_1024x576/NishidaVimeo${run}.zarr --nskip $nskip --gpu $gpuid
#eval ./infer.py "'$imagedir'" $nframes dnn_features_raw/NishidaVimeo_1024x576/NishidaVimeo${run}.zarr --nskip $nskip --gpu $gpuid
else
echo ./infer.py "'$imagedir'" $nframes dnn_features_raw/NishidaVimeo_1024x576/NishidaVimeo${run}.zarr --nskip $nskip --nfold $nfold --gpu $gpuid
#eval ./infer.py "'$imagedir'" $nframes dnn_features_raw/NishidaVimeo_1024x576/NishidaVimeo${run}.zarr --nskip $nskip --nfold $nfold --gpu $gpuid
endif

@ n = $n + 1

End
```

## 2019年3月7日 
Infer_Hua
```

#%%
import zarr
from __future__ import division
import os
import numpy as np
import PIL.Image as pil
import tensorflow as tf
from mySfMLearner import SfMLearner
from utils import normalize_depth_for_display


img_height=128
img_width=416
ckpt_file = 'models/model-190532'
#ckpt_file = 'checkpoints/wo_reg/run_seq5_sm0.05/model.latest'
#ckpt_file = 'checkpoints/wo_reg/run_seq3/model.latest'
fh = open('misc/sample.png', 'rb')
I = pil.open(fh)
I = I.resize((img_width, img_height), pil.ANTIALIAS)
I = np.array(I)

sfm = SfMLearner()
sfm.setup_inference(img_height,
img_width,
mode='depth')

saver = tf.train.Saver([var for var in tf.model_variables()])
with tf.Session() as sess:
saver.restore(sess, ckpt_file)
d_pred = sfm.inference(I[None,:,:,:], sess, mode='depth')

#%%
#print (d_pred)
cnv1b = d_pred['d_cnv1b']
cnv2b = d_pred['d_cnv2b']
cnv3b = d_pred['d_cnv3b']
cnv4b = d_pred['d_cnv4b']
cnv5b = d_pred['d_cnv5b']
cnv6b = d_pred['d_cnv6b']
icnv7 = d_pred['d_icnv7']
icnv6 = d_pred['d_icnv6']
icnv5 = d_pred['d_icnv5']
icnv4 = d_pred['d_icnv4']
icnv3 = d_pred['d_icnv3']
icnv2 = d_pred['d_icnv2']
icnv1 = d_pred['d_icnv1']
#print (cnv6b)
print (icnv6.shape)


#%%
store = zarr.DirectoryStore('/Users/junjie_hua/braincs-home/brain_SfMLearner/example.zarr')
root = zarr.group(store=store, overwrite=True)
#z = root.array(cnv6b.shape)
z1 = root.zeros('depth', shape=(13, 4), chunks=True, dtype='i4')
z2 = root.zeros('pose', shape=(7, 4), chunks=True, dtype='i4')

z1[0, :]=cnv1b.shape
z1[1, :]=cnv2b.shape
z1[2, :]=cnv3b.shape
z1[3, :]=cnv4b.shape
z1[4, :]=cnv5b.shape
z1[5, :]=cnv6b.shape
z1[6, :]=icnv7.shape
z1[7, :]=icnv6.shape
z1[8, :]=icnv5.shape
z1[9, :]=icnv4.shape
z1[10, :]=icnv3.shape
z1[11, :]=icnv2.shape
z1[12, :]=icnv1.shape

print(z1[:])
print(z2[:])
```

## 2019年3月11日 
loopをつくりました
```

#%%
import zarr
from __future__ import division
import os
import numpy as np
import PIL.Image as pil
import tensorflow as tf
from mySfMLearner import SfMLearner
from utils import normalize_depth_for_display
#from PIL import Image
import string


img_height=128
img_width=416
ckpt_file = 'models/model-190532'
#ckpt_file = 'checkpoints/wo_reg/run_seq5_sm0.05/model.latest'
#ckpt_file = 'checkpoints/wo_reg/run_seq3/model.latest'


#import figure
path = "/Users/junjie_hua/braincs-home/brain_SfMLearner/dataset/formatted_data/"
files = os.listdir(path)
files.sort()
sfm = SfMLearner()
sfm.setup_inference(img_height,
img_width,
mode='depth')
saver = tf.train.Saver([var for var in tf.model_variables()])
store = zarr.DirectoryStore('/Users/junjie_hua/braincs-home/brain_SfMLearner/example.zarr')
root = zarr.group(store=store, overwrite=True)

#%%
#infer
num = 0
for file in files:
if not os.path.isdir(file):
name = file
if "._" in name:
continue
num = num+1
#print(num)
#print(path + name)
fh = open(path + name, 'rb')
I = pil.open(fh)
I = I.resize((img_width,img_height),pil.ANTIALIAS)
I = np.array(I)
with tf.Session() as sess:
saver.restore(sess, ckpt_file)
d_pred = sfm.inference(I[None,:,:,:], sess, mode='depth')
#print (d_pred)
cnv1b = d_pred['d_cnv1b']
cnv2b = d_pred['d_cnv2b']
cnv3b = d_pred['d_cnv3b']
cnv4b = d_pred['d_cnv4b']
cnv5b = d_pred['d_cnv5b']
cnv6b = d_pred['d_cnv6b']
icnv7 = d_pred['d_icnv7']
icnv6 = d_pred['d_icnv6']
icnv5 = d_pred['d_icnv5']
icnv4 = d_pred['d_icnv4']
icnv3 = d_pred['d_icnv3']
icnv2 = d_pred['d_icnv2']
icnv1 = d_pred['d_icnv1']
name = root.create_group(num)
z1 = name.create_dataset('depth', shape=(13, 4), chunks=True, dtype='i4')
#z2 = root.zeros('pose', shape=(7, 4), chunks=True, dtype='i4')
z1[0, :]=cnv1b.shape
z1[1, :]=cnv2b.shape
z1[2, :]=cnv3b.shape
z1[3, :]=cnv4b.shape
z1[4, :]=cnv5b.shape
z1[5, :]=cnv6b.shape
z1[6, :]=icnv7.shape
z1[7, :]=icnv6.shape
z1[8, :]=icnv5.shape
z1[9, :]=icnv4.shape
z1[10, :]=icnv3.shape
z1[11, :]=icnv2.shape
z1[12, :]=icnv1.shape
```


## 2019年3月14日
- [x] groupは最初に作成した１つだけ(root)を使用し，
forループの初回で必要となる大きさのdataset(4次元配列)を作成して下さい．
- [x] 2回目以降のループでは既に作成してあるdatasetの対応する箇所にデータを代入
- [x] レイヤー毎に異なるshapeのデータセットを個別に作成してデータ自体を代入するようにして下さい．

```

#%%
import zarr
from __future__ import division
import os
import numpy as np
import PIL.Image as pil
import tensorflow as tf
from mySfMLearner import SfMLearner
from utils import normalize_depth_for_display
import glob
import string


img_height=128
img_width=416
ckpt_file = 'models/model-190532'
#ckpt_file = 'checkpoints/wo_reg/run_seq5_sm0.05/model.latest'
#ckpt_file = 'checkpoints/wo_reg/run_seq3/model.latest'


#import figure
path = "/Users/junjie_hua/braincs-home/brain_SfMLearner/dataset/formatted_data/"
files = os.listdir(path)
files.sort()
n_total = len(files)
print(n_total)
sfm = SfMLearner()
sfm.setup_inference(img_height,
img_width,
mode='depth')
saver = tf.train.Saver([var for var in tf.model_variables()])
store = zarr.DirectoryStore('/Users/junjie_hua/braincs-home/brain_SfMLearner/example.zarr')
root = zarr.group(store=store, overwrite=True)







#%%
#infer

#num = 0
#for file in files:
# if not os.path.isdir(file):
# name = file
# if "._" in name:
# continue
# num = num+1
# root.create_dataset(num, shape=(1,4), dtype=np.float32)



# depth
chunk_size = 15
num = 0
for file in files:
if not os.path.isdir(file):
name = file
if "._" in name:
continue
num = num+1
#print(num)
#print(path + name)
fh = open(path + name, 'rb')
I = pil.open(fh)
I = I.resize((img_width,img_height),pil.ANTIALIAS)
I = np.array(I)
with tf.Session() as sess:
saver.restore(sess, ckpt_file)
d_pred = sfm.inference(I[None,:,:,:], sess, mode='depth')
cnv1b = d_pred['d_cnv1b']
cnv2b = d_pred['d_cnv2b']
cnv3b = d_pred['d_cnv3b']
cnv4b = d_pred['d_cnv4b']
cnv5b = d_pred['d_cnv5b']
cnv6b = d_pred['d_cnv6b']
icnv7 = d_pred['d_icnv7']
icnv6 = d_pred['d_icnv6']
icnv5 = d_pred['d_icnv5']
icnv4 = d_pred['d_icnv4']
icnv3 = d_pred['d_icnv3']
icnv2 = d_pred['d_icnv2']
icnv1 = d_pred['d_icnv1']
if num == 1:
root.create_dataset('cnv1b', shape=(n_total,)+cnv1b.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('cnv2b', shape=(n_total,)+cnv2b.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('cnv3b', shape=(n_total,)+cnv3b.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('cnv4b', shape=(n_total,)+cnv4b.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('cnv5b', shape=(n_total,)+cnv5b.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('cnv6b', shape=(n_total,)+cnv6b.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('icnv7', shape=(n_total,)+icnv7.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('icnv6', shape=(n_total,)+icnv6.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('icnv5', shape=(n_total,)+icnv5.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('icnv4', shape=(n_total,)+icnv4.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('icnv3', shape=(n_total,)+icnv3.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('icnv2', shape=(n_total,)+icnv2.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('icnv1', shape=(n_total,)+icnv1.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root['cnv1b'][num-1,:,:,:] = cnv1b.squeeze()
root['cnv2b'][num-1,:,:,:] = cnv2b.squeeze()
root['cnv3b'][num-1,:,:,:] = cnv3b.squeeze()
root['cnv4b'][num-1,:,:,:] = cnv4b.squeeze()
root['cnv5b'][num-1,:,:,:] = cnv5b.squeeze()
root['cnv6b'][num-1,:,:,:] = cnv6b.squeeze()
root['icnv7'][num-1,:,:,:] = icnv7.squeeze()
root['icnv6'][num-1,:,:,:] = icnv6.squeeze()
root['icnv5'][num-1,:,:,:] = icnv5.squeeze()
root['icnv4'][num-1,:,:,:] = icnv4.squeeze()
root['icnv3'][num-1,:,:,:] = icnv3.squeeze()
root['icnv2'][num-1,:,:,:] = icnv2.squeeze()
root['icnv1'][num-1,:,:,:] = icnv1.squeeze()




#pose
sfm.setup_inference(img_height,
img_width,
mode='pose')
saver = tf.train.Saver([var for var in tf.model_variables()])

chunk_size = 15
num = 0
for file in files:
if not os.path.isdir(file):
name = file
if "._" in name:
continue
num = num+1
fh = open(path + name, 'rb')
I = pil.open(fh)
I = I.resize((img_width*3,img_height),pil.ANTIALIAS)
I = np.array(I)
with tf.Session() as sess:
saver.restore(sess, ckpt_file)
p_pred = sfm.inference(I[None,:,:,:], sess, mode='pose')
cnv1 = p_pred['p_cnv1']
cnv2 = p_pred['p_cnv2']
cnv3 = p_pred['p_cnv3']
cnv4 = p_pred['p_cnv4']
cnv5 = p_pred['p_cnv5']
if num == 1:
root.create_dataset('cnv1', shape=(n_total,)+cnv1.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('cnv2', shape=(n_total,)+cnv2.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('cnv3', shape=(n_total,)+cnv3.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('cnv4', shape=(n_total,)+cnv4.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root.create_dataset('cnv5', shape=(n_total,)+cnv5.shape[1:], chunks=(chunk_size, None), dtype=np.float32)
root['cnv1'][num-1,:,:,:] = cnv1.squeeze()
root['cnv2'][num-1,:,:,:] = cnv2.squeeze()
root['cnv3'][num-1,:,:,:] = cnv3.squeeze()
root['cnv4'][num-1,:,:,:] = cnv4.squeeze()
root['cnv5'][num-1,:,:,:] = cnv5.squeeze()


#%%
print(root.tree())
print("over!")




```