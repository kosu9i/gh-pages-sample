# インターフェイス 2019年1月号のチュートリアルをやっていく

[インターフェイス 2019年1月号](https://interface.cqpub.co.jp/magazine/201901/) に  
Ultra96でYoloを動かす記事が載っている。

# 用語

* PetaLinux  
    - Xilinx社のボードで動かすための組み込みLinuxシステムの構築をするツール  
    - Linuxのディストリビューション名ではなく、ツール名
    - 内部ではYocto Projectという組み込みボードのLinuxシステムの構築によく使われるものが  
      使われている [らしい](https://qiita.com/iwatake2222/items/6e6915f7318689818368#petalinux%E3%83%84%E3%83%BC%E3%83%AB)
    - 動作させるのにUbuntuが必要
* Vivado
    - ボードで動かす回路をコンパイルするIDEみたいなもの
    - 動作させるのにWindows or Ubuntuが必要
* BSP
    - ボードサポートパッケージの略
    - 構築済みのブートローダ、システムイメージ、ビットストリームが含まれる。
    - 下記、公式ページから引用
```
PetaLinux ボード サポート パッケージ (BSP) およびリファレンス サンプルには、あらかじめ構築済みのブート ローダー、システム イメージ、およびビットストリームが含まれます。ビルトイン ツールを利用して、シングル コマンドで物理的ハードウェアまたはフルシステム シミュレータ (QEMU) へこれらのエレメントを配置して実行できます。PetaLinux を利用することによって、ザイリンクス ベースのハードウェアをインストールしてから 5 分以内に起動して動作させることができます (アプリケーション、ライブラリ、ドライバー開発の準備が整う)。
PetaLinux ツールダウンロード ページで最新の利用可能な PetaLinux BSP の一覧をご覧いただけます。
```



# 準備

## Ubuntu

PetaLinux, Vivadoを動かすためにMac内のVirtualBoxでUbuntu 18.04を準備。

* インターフェイスの記事ではUbuntu16.04でないと動作しないと書いてあったが  
  Xilinxから出ている最新版では18.04もサポートすると書いてあるので  
  [18.04 LTS Desktop](https://ubuntu.com/download/desktop)を入れることにした。
* VirtualBoxでホストOSからゲストOSにssh接続するときは  
  NATの場合はポートフォワーディングで特定portを使って実施する。  
  参考: https://askubuntu.com/questions/48436/how-to-scp-a-file-from-mac-ubuntu-virtualbox

## Peta Linux

[公式ドキュメント](https://japan.xilinx.com/support/documentation/sw_manuals_j/xilinx2019_2/ug1144-petalinux-tools-reference-guide.pdf)

`Peta linux` でググると[公式のダウンロードページ](https://japan.xilinx.com/products/design-tools/embedded-software/petalinux-sdk.html)が出てくると思う。

[Peta linuxのダウンロードページ](https://japan.xilinx.com/support/download/index.html/content/xilinx/ja/downloadNav/embedded-design-tools.html)から最新のインストーラをダウンロードしておく。

Ubuntuで以下を準備

```
$ sudo apt-get install -y gawk gcc git make net-tools libncurses5-dev tftpd zlib1g-dev libssl-dev flex bison libselinux1 gnupg \
    wget diffstat chrpath socat xterm autoconf libtool tar unzip texinfo zlib1g-dev gcc-multilib build-essential \
    zlib1g:i386 screen pax gzip
```

/bin/shにdashが使われている場合、bashに変更しておく

```
$ sudo  dpkg-reconfigure dash
```

インストール先のディレクトリをユーザ書き込み権限ありで作成

```
$ sudo mkdir -p /opt/pkg/petalinux/2019.2/
$ sudo chown -R $USER:$USER /opt/pkg/
```

インストール実行

```
$ chmod 755 petalinux-v2019.2-final-installer.run
$ ./petalinux-v2019.2-final-installer.run /opt/pkg/petalinux/2019.2/
```

セットアップ

```
$ source /opt/pkg/petalinux/2019.2/settings.sh
```

以下が出力されれば成功

```
PetaLinux environment set to '/opt/pkg/petalinux/2019.2/'
INFO: Checking free disk space
INFO: Checking installed tools
INFO: Checking installed development libraries
INFO: Checking network and other services
WARNING: No tftp server found - please see "PetaLinux SDK Installation
Guide" for its
impact and solution
```

```
$ echo $PETALINUX
/opt/pkg/petalinux/2019.2/
```

## BSP

[Peta linuxのダウンロードページ](https://japan.xilinx.com/support/download/index.html/content/xilinx/ja/downloadNav/embedded-design-tools.html)と同じところからダウンロードできる。

とりあえず適当に一番最初にある [ZCU102 BSP](https://japan.xilinx.com/member/forms/download/xef.html?filename=xilinx-zcu102-v2019.2-final.bsp)というやつをやってみる。  
ダウンロードにはユーザ登録とログインが必要になる。

プロジェクトを作成

```
$ petalinux-create -t project -s ./xilinx-zcu102-v2019.2-final.bsp 
INFO: Create project:
INFO: Projects:
INFO:   * xilinx-zcu102-2019.2
INFO: has been successfully installed to /home/kosugi/
INFO: New project successfully created in /home/kosugi/
```

BSPと同ディレクトリ配下に `xilinx-zcu102-2019.2` というディレクトリが作成されるはず。

プロジェクトのビルド

```
$ cd xilinx-zcu102-2019.2/
$ petalinux-build
[INFO] building project
[INFO] generating Kconfig for project
[INFO] silentconfig project
[INFO] sourcing bitbake
[INFO] generating plnxtool conf
[INFO] generating meta-plnx-generated layer
[INFO] generating bbappends for project . This may take time !
ERROR: Unable to start bitbake server
ERROR: Last 60 lines of server log for this session (/home/kosugi/xilinx-zcu102-2019.2/build/bitbake-cookerdaemon.log):
ERROR: Error parsing configuration files
...
ERROR: Error parsing configuration files
[INFO] generating u-boot configuration files
[INFO] generating kernel configuration files
ERROR: Failed to add meta-plnx-generated layer:/home/kosugi/xilinx-zcu102-2019.2/project-spec/meta-plnx-generated
ERROR: Failed to build project
```

失敗する。

調べてみると、201902用のUltra96 v2のBSPはないらしい。

# Ubuntu 16.04で再検証

## Ubuntu

[日本語Remix版の16.04](https://www.ubuntulinux.jp/News/ubuntu1604-ja-remix)をVirtualBoxにインストール。

sshdをインストール

```
sudo apt install openssh-server
```

## Peta Linux

v2018.2の日本語版ドキュメントは見つからなかったので以下を参照。  
[ug1144-petalinux-tools-reference-guide.pdf](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2018_2/ug1144-petalinux-tools-reference-guide.pdf)

```
$ sudo apt-get install -y gawk gcc git make net-tools libncurses5-dev tftpd zlib1g-dev libssl-dev \
    flex bison libselinux1 gnupg wget diffstat chrpath socat xterm autoconf libtool tar unzip \
    texinfo zlib1g-dev gcc-multilib build-essential libsdl1.2-dev libglib2.0-dev zlib1g:i386 \
    screen pax gzip
```

/bin/shにdashが使われている場合、bashに変更しておく

```
$ ll /bin/sh
lrwxrwxrwx 1 root root 4  2月 16 22:08 /bin/sh -> dash*
$ sudo  dpkg-reconfigure dash
'dash による /bin/sh から /bin/sh.distrib への退避 (divert)' を削除しています
'bash による /bin/sh から /bin/sh.distrib への退避 (divert)' を追加しています
'dash による /usr/share/man/man1/sh.1.gz から /usr/share/man/man1/sh.distrib.1.gz への退避 (divert)' を削除しています
'bash による /usr/share/man/man1/sh.1.gz から /usr/share/man/man1/sh.distrib.1.gz への退避 (divert)' を追加しています
$ ll /bin/sh
lrwxrwxrwx 1 root root 4  2月 17 01:03 /bin/sh -> bash*
```

インストール先のディレクトリをユーザ書き込み権限ありで作成

```
$ sudo mkdir -p /opt/pkg/petalinux/2018.2/
$ sudo chown -R $USER:$USER /opt/pkg/
```

```
$ ./petalinux-v2018.2-final-installer.run /opt/pkg/petalinux/2018.2/
```

セットアップ

```
$ source /opt/pkg/petalinux/2018.2/settings.sh
PetaLinux environment set to '/opt/pkg/petalinux/2018.2'
INFO: Checking free disk space
INFO: Checking installed tools
INFO: Checking installed development libraries
INFO: Checking network and other services
WARNING: No tftp server found - please refer to "PetaLinux SDK Installation Guide" for its impact and solution
```

## BSP

[xilinx-ultra96-reva-v2018.2-final.bsp](https://www.xilinx.com/member/forms/download/xef.html?filename=xilinx-ultra96-reva-v2018.2-final.bsp)をダウンロード。

プロジェクト作成

```
$ petalinux-create -t project -s ./xilinx-ultra96-reva-v2018.2-final.bsp
```

ビルド

```
$ cd xilinx-ultra96-reva-2018.2/
$ petalinux-build
```

なんか時間がかかっててうまくビルドしている様子...

