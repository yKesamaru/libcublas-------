`apt autoremove`する時は気をつけましょうという話。
# cublas等がごそっと消えた
`dlib`を使ったPythonプログラムの起動に失敗しました。  
```bash:エラー
`ImportError: libcublas.so.11: cannot open shared object file: No such file or directory
```  
今まで起こらなかったエラーです。  
`nvidia-smi`を確認した様子が以下。  
![](https://raw.githubusercontent.com/yKesamaru/libcublas-reinstall/master/img/shadow_470.png)  

正常なようです。DriverやCUDAバージョンも変わりないように見えます。    
Pythonインタラクティブモードで`dlib`が正常に呼び出せるか確認してみました。  
```bash
# $ source ../venv3.7/bin/activate
(venv3.7) $ python -V
Python 3.7.11
(venv3.7) $ python
Python 3.7.11 (default, Nov  3 2021, 08:07:41) 
[GCC 7.5.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import dlib
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/{user}/venv3.7/lib/python3.7/site-packages/dlib/__init__.py", line 19, in <module>
    from _dlib_pybind11 import *
ImportError: libcublas.so.11: cannot open shared object file: No such file or directory
>>> 
```
単純に`libcublas.so`がないらしいです。そんなわけあるかと思い`Synaptic`を起動してみたところ・・  
CUDA関連のライブラリがごっそり消えている？  
# ログを確認する
どうして消えてしまったのか以下のログ内容を確認しました。  
- /var/log/
  - kern.log
  - auth.log
  - unattended-upgrades.log
  - apt/
    - history.log
    - term.log
## `/var/log/apt/history.log`
`/var/log/apt/history.log`の中の`libcublas`を調べます。以下の様になってました。  
```bash
Start-Date: 2022-01-03  06:44:31
Commandline: apt autoremove
Requested-By: {user} (1000)
Remove: libnpp-dev-11-4:amd64 (11.4.0.110-1), cuda-nsight-compute-11-4:amd64 (11.4.3-1), cuda-nvprof-11-4:amd64 (11.4.120-1), libcusparse-dev-11-4:amd64 (11.6.0.120-1), cuda-toolkit-11-config-common:amd64 (11.5.117-1), cuda-nvprune-11-4:amd64 (11.4.120-1), cuda-gdb-11-4:amd64 (11.4.120-1), cuda-visual-tools-11-4:amd64 (11.4.3-1), cuda-cupti-11-4:amd64 (11.4.120-1), cuda-libraries-dev-11-4:amd64 (11.4.3-1), cuda-nvrtc-dev-11-4:amd64 (11.4.152-1), cuda-cccl-11-4:amd64 (11.4.122-1), cuda-cupti-dev-11-4:amd64 (11.4.120-1), cuda-libraries-11-4:amd64 (11.4.3-1), liburcu6:amd64 (0.10.1-1ubuntu1), cuda-sanitizer-11-4:amd64 (11.4.120-1), cuda-nvrtc-11-4:amd64 (11.4.152-1), cuda-nsight-systems-11-4:amd64 (11.4.3-1), libcublas-11-4:amd64 (11.6.5.2-1), libcusolver-dev-11-4:amd64 (11.2.0.120-1), cuda-tools-11-4:amd64 (11.4.3-1), cuda-nvtx-11-4:amd64 (11.4.120-1), libnpp-11-4:amd64 (11.4.0.110-1), libcufile-11-4:amd64 (1.0.2.10-1), cuda-cudart-11-4:amd64 (11.4.148-1), cuda-nvdisasm-11-4:amd64 (11.4.152-1), cuda-samples-11-4:amd64 (11.4.120-1), cuda-documentation-11-4:amd64 (11.4.126-1), cuda-nvcc-11-4:amd64 (11.4.152-1), cuda-nvvp-11-4:amd64 (11.4.120-1), cuda-cuxxfilt-11-4:amd64 (11.4.120-1), libcurand-11-4:amd64 (10.2.5.120-1), cuda-toolkit-11-4:amd64 (11.4.3-1), nsight-compute-2021.2.2:amd64 (2021.2.2.1-1), cuda-cudart-dev-11-4:amd64 (11.4.148-1), cuda-nvml-dev-11-4:amd64 (11.4.120-1), libcufile-dev-11-4:amd64 (1.0.2.10-1), libcublas-dev-11-4:amd64 (11.6.5.2-1), nsight-systems-2021.3.2:amd64 (2021.3.2.4-027534f), libcurand-dev-11-4:amd64 (10.2.5.120-1), libcusolver-11-4:amd64 (11.2.0.120-1), cuda-compiler-11-4:amd64 (11.4.3-1), libcufft-dev-11-4:amd64 (10.5.2.100-1), linux-hwe-5.4-headers-5.4.0-89:amd64 (5.4.0-89.100~18.04.1), libcusparse-11-4:amd64 (11.6.0.120-1), cuda-nsight-11-4:amd64 (11.4.120-1), cuda-command-line-tools-11-4:amd64 (11.4.3-1), cuda-driver-dev-11-4:amd64 (11.4.148-1), cuda-toolkit-11-4-config-common:amd64 (11.4.148-1), cuda-memcheck-11-4:amd64 (11.4.120-1), libnvjpeg-11-4:amd64 (11.5.2.120-1), cuda-toolkit-config-common:amd64 (11.5.117-1), libnvjpeg-dev-11-4:amd64 (11.5.2.120-1), libcufft-11-4:amd64 (10.5.2.100-1), cuda-cuobjdump-11-4:amd64 (11.4.120-1), gds-tools-11-4:amd64 (1.0.2.10-1)
End-Date: 2022-01-03  06:44:38  
```
1/3にコマンドラインから`apt autoremove`してあります。自分は全く覚えてません。  
そこで`/var/log/`からその時の状況を追ってみます。 
## `/var/log/kern.log.2` 
```bash:/var/log/kern.log.2.gz
Jan  3 06:32:12 {user} kernel: [    0.000000] Linux version 5.4.0-91-generic (buildd@lgw01-amd64-024) (gcc version 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04)) #102~18.04.1-Ubuntu SMP Thu Nov 11 14:46:36 UTC 2021 (Ubuntu 5.4.0-91.102~18.04.1-generic 5.4.151)
```
お正月3日目、午前6時32分にPCの電源を入れたようです。  
## `unattended-upgrades.log`自動アップデートも見てみてみる
```bash:/var/log/unattended-upgrades/unattended-upgrades.log
2022-01-03 06:32:22,203 INFO 初期状態でブラックリストにあるパッケージ: 
2022-01-03 06:32:22,204 INFO 初期状態でホワイトリストにあるパッケージ: 
2022-01-03 06:32:22,204 INFO 自動アップグレードスクリプトを開始します
2022-01-03 06:32:22,205 INFO 許可されているパッケージ導入元: o=Ubuntu,a=bionic, o=Ubuntu,a=bionic-security, o=UbuntuESMApps,a=bionic-apps-security, o=UbuntuESM,a=bionic-infra-security
2022-01-03 06:32:27,750 INFO 初期状態でブラックリストにあるパッケージ: 
2022-01-03 06:32:27,751 INFO 初期状態でホワイトリストにあるパッケージ: 
2022-01-03 06:32:27,751 INFO 自動アップグレードスクリプトを開始します
2022-01-03 06:32:27,751 INFO 許可されているパッケージ導入元: o=Ubuntu,a=bionic, o=Ubuntu,a=bionic-security, o=UbuntuESMApps,a=bionic-apps-security, o=UbuntuESM,a=bionic-infra-security
2022-01-03 06:32:30,869 INFO 自動更新可能なパッケージおよび保留中の自動削除が見つかりません
```
起動してから自動アップデートが走っています。`2022-01-03 06:32:30,869 INFO 自動更新可能なパッケージおよび保留中の自動削除が見つかりません`とのこと。
##  `/var/log/auth.log1`
認証関係はこのログで見ることが出来ます。見つかった問題行動が以下。  
```bash:/var/log/auth.log.1
Jan  3 06:43:56 {user} sudo:    {user} : TTY=pts/0 ; PWD=/home/{user} ; USER=root ; COMMAND=/usr/bin/apt update
Jan  3 06:43:56 {user} sudo: pam_unix(sudo:session): session opened for user root by (uid=0)
Jan  3 06:44:02 {user} sudo: pam_unix(sudo:session): session closed for user root
Jan  3 06:44:07 {user} sudo:    {user} : TTY=pts/0 ; PWD=/home/{user} ; USER=root ; COMMAND=/usr/bin/apt upgrade
Jan  3 06:44:07 {user} sudo: pam_unix(sudo:session): session opened for user root by (uid=0)
Jan  3 06:44:08 {user} sudo: pam_unix(sudo:session): session closed for user root
Jan  3 06:44:28 {user} sudo:    {user} : TTY=pts/0 ; PWD=/home/{user} ; USER=root ; COMMAND=/usr/bin/apt autoremove
```
仮想端末から`apt update; apt upgrade; apt autoremove;`を行っていた事が分かりました。`{user}`と書いてある所は**自分**です。  
    
## 疑問
どうして`autoremove`の候補にCUDA関連ライブラリが上がってきたのか原因がよく分かりません。過去に遡って12月17日に`cuda-drivers:amd64 (470.57.02-1), cuda-runtime-11-4:amd64 (11.4.2-1), cuda-demo-suite-11-4:amd64 (11.4.100-1), cuda-11-4:amd64 (11.4.2-1)`が削除されているようです。新規で別のものを上書きしてましたけど。これが関係しているのかな？よく分かりません。  

```bash:/var/log/apt/history.log.1
Start-Date: 2021-12-17  09:40:56
Commandline: /usr/sbin/synaptic
Upgrade: nvidia-docker2:amd64 (2.7.0-1, 2.8.0-1), containerd.io:amd64 (1.4.11-1, 1.4.12-1), cuda-toolkit-11-config-common:amd64 (11.5.50-1, 11.5.117-1), libnvidia-container-tools:amd64 (1.6.0-1, 1.7.0-1), cuda-drivers-470:amd64 (470.57.02-1, 470.82.01-1), python3-software-properties:amd64 (0.96.24.32.14, 0.96.24.32.18), openssl:amd64 (1.1.1-1ubuntu2.1~18.04.13, 1.1.1-1ubuntu2.1~18.04.14), ubuntu-advantage-tools:amd64 (27.3~18.04.1, 27.4.2~18.04.1), google-chrome-stable:amd64 (95.0.4638.69-1, 96.0.4664.110-1), docker-ce-rootless-extras:amd64 (5:20.10.10~3-0~ubuntu-bionic, 5:20.10.12~3-0~ubuntu-bionic), software-properties-gtk:amd64 (0.96.24.32.14, 0.96.24.32.18), libnvidia-container1:amd64 (1.6.0-1, 1.7.0-1), libssl-dev:amd64 (1.1.1-1ubuntu2.1~18.04.13, 1.1.1-1ubuntu2.1~18.04.14), libcudnn8-dev:amd64 (8.3.0.98-1+cuda11.5, 8.3.1.22-1+cuda11.5), code:amd64 (1.62.2-1636665017, 1.63.1-1639448820), apache2-bin:amd64 (2.4.29-1ubuntu4.19, 2.4.29-1ubuntu4.20), rsync:amd64 (3.1.2-2.1ubuntu1.1, 3.1.2-2.1ubuntu1.2), libssl1.1:amd64 (1.1.1-1ubuntu2.1~18.04.13, 1.1.1-1ubuntu2.1~18.04.14), libssl1.1:i386 (1.1.1-1ubuntu2.1~18.04.13, 1.1.1-1ubuntu2.1~18.04.14), libcudnn8:amd64 (8.3.0.98-1+cuda11.5, 8.3.1.22-1+cuda11.5), nvidia-container-toolkit:amd64 (1.6.0-1, 1.7.0-1), docker-scan-plugin:amd64 (0.9.0~ubuntu-bionic, 0.12.0~ubuntu-bionic), cuda-toolkit-config-common:amd64 (11.5.50-1, 11.5.117-1), docker-ce:amd64 (5:20.10.10~3-0~ubuntu-bionic, 5:20.10.12~3-0~ubuntu-bionic), docker-ce-cli:amd64 (5:20.10.10~3-0~ubuntu-bionic, 5:20.10.12~3-0~ubuntu-bionic), software-properties-common:amd64 (0.96.24.32.14, 0.96.24.32.18)
Remove: cuda-drivers:amd64 (470.57.02-1), cuda-runtime-11-4:amd64 (11.4.2-1), cuda-demo-suite-11-4:amd64 (11.4.100-1), cuda-11-4:amd64 (11.4.2-1)
End-Date: 2021-12-17  09:43:39
```
autoremoveの候補に上がってきても今度からは消しません。  
しかしいつどのようにやらかしたのかは分かりました。  
# cublas再インストール
## 環境
```bash
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 18.04.6 LTS
Release:	18.04
Codename:	bionic
```
## 方法
やり方は複数あるのですが一例をメモします。  
### リポジトリ追加
今回のように誤ってライブラリを消してしまった場合はリポジトリまで消えてません。バージョンを指定したい場合は以下を参照。  
https://qiita.com/yukoba/items/4733e8602fa4acabcc35  
以下の様な感じだと最新がインストールされます。  
https://developer.nvidia.com/cuda-downloads  
上記URLでOS等を選びます。Ubuntu18.04のネットワークインストールだとリポジトリの登録が以下の様に指示されます。  
```
$ wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pinsudo 
$ mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
$ sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
$ sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/ /"
$ sudo apt-get update
$ sudo apt-get -y install cuda
```

`toolkit driver version`は以下URLの`table 3`にあります。  
https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html  
![](https://raw.githubusercontent.com/yKesamaru/libcublas-reinstall/master/img/shadow_toolkit_driver_version.png)  
今回パッケージマネージャーから選んだところドライバーは`cuda-drivers-495 (バージョン 495.29.05-1) がインストールされます`とでました。

:::details 再インストールの様子
```bash
cuda-drivers-470 が削除されます
libnvidia-cfg1-470 が削除されます
libnvidia-common-470 が削除されます
libnvidia-compute-470 が削除されます
libnvidia-decode-470 が削除されます
libnvidia-encode-470 が削除されます
libnvidia-extra-470 が削除されます
libnvidia-fbc1-470 が削除されます
libnvidia-gl-470 が削除されます
libnvidia-ifr1-470 が削除されます
nvidia-compute-utils-470 が削除されます
nvidia-dkms-470 が削除されます
nvidia-driver-470 が削除されます
nvidia-kernel-common-470 が削除されます
nvidia-kernel-source-470 が削除されます
nvidia-utils-470 が削除されます
xserver-xorg-video-nvidia-470 が削除されます
cuda (バージョン 11.5.1-1) がインストールされます
cuda-11-5 (バージョン 11.5.1-1) がインストールされます
cuda-cccl-11-5 (バージョン 11.5.62-1) がインストールされます
cuda-command-line-tools-11-5 (バージョン 11.5.1-1) がインストールされます
cuda-compiler-11-5 (バージョン 11.5.1-1) がインストールされます
cuda-cudart-11-5 (バージョン 11.5.117-1) がインストールされます
cuda-cudart-dev-11-5 (バージョン 11.5.117-1) がインストールされます
cuda-cuobjdump-11-5 (バージョン 11.5.119-1) がインストールされます
cuda-cupti-11-5 (バージョン 11.5.114-1) がインストールされます
cuda-cupti-dev-11-5 (バージョン 11.5.114-1) がインストールされます
cuda-cuxxfilt-11-5 (バージョン 11.5.119-1) がインストールされます
cuda-demo-suite-11-5 (バージョン 11.5.50-1) がインストールされます
cuda-documentation-11-5 (バージョン 11.5.114-1) がインストールされます
cuda-driver-dev-11-5 (バージョン 11.5.117-1) がインストールされます
cuda-drivers (バージョン 495.29.05-1) がインストールされます
cuda-drivers-495 (バージョン 495.29.05-1) がインストールされます
cuda-gdb-11-5 (バージョン 11.5.114-1) がインストールされます
cuda-libraries-11-5 (バージョン 11.5.1-1) がインストールされます
cuda-libraries-dev-11-5 (バージョン 11.5.1-1) がインストールされます
cuda-memcheck-11-5 (バージョン 11.5.114-1) がインストールされます
cuda-nsight-11-5 (バージョン 11.5.114-1) がインストールされます
cuda-nsight-compute-11-5 (バージョン 11.5.1-1) がインストールされます
cuda-nsight-systems-11-5 (バージョン 11.5.1-1) がインストールされます
cuda-nvcc-11-5 (バージョン 11.5.119-1) がインストールされます
cuda-nvdisasm-11-5 (バージョン 11.5.119-1) がインストールされます
cuda-nvml-dev-11-5 (バージョン 11.5.50-1) がインストールされます
cuda-nvprof-11-5 (バージョン 11.5.114-1) がインストールされます
cuda-nvprune-11-5 (バージョン 11.5.119-1) がインストールされます
cuda-nvrtc-11-5 (バージョン 11.5.119-1) がインストールされます
cuda-nvrtc-dev-11-5 (バージョン 11.5.119-1) がインストールされます
cuda-nvtx-11-5 (バージョン 11.5.114-1) がインストールされます
cuda-nvvp-11-5 (バージョン 11.5.114-1) がインストールされます
cuda-runtime-11-5 (バージョン 11.5.1-1) がインストールされます
cuda-samples-11-5 (バージョン 11.5.56-1) がインストールされます
cuda-sanitizer-11-5 (バージョン 11.5.114-1) がインストールされます
cuda-toolkit-11-5 (バージョン 11.5.1-1) がインストールされます
cuda-toolkit-11-5-config-common (バージョン 11.5.117-1) がインストールされます
cuda-toolkit-11-config-common (バージョン 11.5.117-1) がインストールされます
cuda-toolkit-config-common (バージョン 11.5.117-1) がインストールされます
cuda-tools-11-5 (バージョン 11.5.1-1) がインストールされます
cuda-visual-tools-11-5 (バージョン 11.5.1-1) がインストールされます
gds-tools-11-5 (バージョン 1.1.1.25-1) がインストールされます
libcublas-11-5 (バージョン 11.7.4.6-1) がインストールされます
libcublas-dev-11-5 (バージョン 11.7.4.6-1) がインストールされます
libcufft-11-5 (バージョン 10.6.0.107-1) がインストールされます
libcufft-dev-11-5 (バージョン 10.6.0.107-1) がインストールされます
libcufile-11-5 (バージョン 1.1.1.25-1) がインストールされます
libcufile-dev-11-5 (バージョン 1.1.1.25-1) がインストールされます
libcurand-11-5 (バージョン 10.2.7.107-1) がインストールされます
libcurand-dev-11-5 (バージョン 10.2.7.107-1) がインストールされます
libcusolver-11-5 (バージョン 11.3.2.107-1) がインストールされます
libcusolver-dev-11-5 (バージョン 11.3.2.107-1) がインストールされます
libcusparse-11-5 (バージョン 11.7.0.107-1) がインストールされます
libcusparse-dev-11-5 (バージョン 11.7.0.107-1) がインストールされます
libnpp-11-5 (バージョン 11.5.1.107-1) がインストールされます
libnpp-dev-11-5 (バージョン 11.5.1.107-1) がインストールされます
libnvidia-cfg1-495 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
libnvidia-common-495 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
libnvidia-compute-495 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
libnvidia-compute-495:i386 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
libnvidia-decode-495 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
libnvidia-decode-495:i386 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
libnvidia-encode-495 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
libnvidia-encode-495:i386 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
libnvidia-extra-495 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
libnvidia-fbc1-495 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
libnvidia-fbc1-495:i386 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
libnvidia-gl-495 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
libnvidia-gl-495:i386 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
libnvjpeg-11-5 (バージョン 11.5.4.107-1) がインストールされます
libnvjpeg-dev-11-5 (バージョン 11.5.4.107-1) がインストールされます
liburcu6 (バージョン 0.10.1-1ubuntu1) がインストールされます
nsight-compute-2021.3.1 (バージョン 2021.3.1.4-1) がインストールされます
nsight-systems-2021.3.3 (バージョン 2021.3.3.2-b99c4d6) がインストールされます
nvidia-compute-utils-495 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
nvidia-dkms-495 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
nvidia-driver-495 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
nvidia-kernel-common-495 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
nvidia-kernel-source-495 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
nvidia-utils-495 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
xserver-xorg-video-nvidia-495 (バージョン 495.46-0ubuntu0.18.04.1) がインストールされます
```  
:::  
再インストール後、再起動して`nvidia-smi`で確認を行います。再起動しない場合はエラーになります。  
```bash: エラー
$ nvidia-smi
Failed to initialize NVML: Driver/library version mismatch
```
![](https://raw.githubusercontent.com/yKesamaru/libcublas-reinstall/master/img/shadow_495.png)  
*再起動後*
```bash:再起動後
$ python
Python 3.7.11 (default, Nov  3 2021, 08:07:41) 
[GCC 7.5.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import dlib
>>> dlib.DLIB_USE_CUDA
True
>>> 
```
直りました。
# 今後
他にもこの時期に同じ症状が出た方はいたんでしょうか。  
再度`apt autoremove`の候補に出てくるかどうか様子を見たいと思います。