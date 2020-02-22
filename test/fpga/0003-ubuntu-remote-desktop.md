# FPGA用Ubuntuの準備

FPGA用のUbuntuはそれなりにスペックが要求されるので  
ローカルのVirtualMachineじゃ厳しくなってきた。

そのため、GCP上にUbuntuをたててリモートデスクトップを試みる。


# GCPな理由

AWSだと月に1,000円しか無料枠がないため、ハイスペックだと超えそう。  
GCPは12ヶ月で30,000円無料枠を使える。1ヶ月で使い切ってもいいはず。  

n1-standard-4は4CPU, 15GBメモリで月に$150（1時間あたり$0.2）くらい。   
使うときだけ起動していればそこまで食わないはず。  


# Chrome Remote Desktop

1. ひとまずn1-standard-1で16.04LTSインスタンスを起動。（セットアップ後にスペックアップすればOK）
    * ディスクサイズは100GBにしておく
    * ディスクIOはそれなりに食いそうなので、SSDにする
2. Ubuntu16.04にssh接続し、[公式ドキュメント](https://cloud.google.com/solutions/chrome-desktop-remote-on-compute-engine?hl=ja#configuring_and_starting_the_chrome_remote_desktop_service)にある通りセットアップしていく。
```
wget https://dl.google.com/linux/directchrome-remote-desktop_current_amd64.deb/
sudo apt update
sudo dpkg --install chrome-remote-desktop_current_amd64.deb
sudo apt install --assume-yes --fix-broken

# 以下、Xfceを選択

sudo DEBIAN_FRONTEND=noninteractive \
    apt install --assume-yes xfce4 desktop-base
sudo bash -c 'echo "exec /etc/X11/Xsession /usr/bin/xfce4-session" > /etc/chrome-remote-desktop-session'
sudo apt install --assume-yes xscreensaver
sudo apt install --assume-yes task-xfce-desktop

sudo systemctl disable lightdm.service # エラーになるが無視
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

sudo dpkg --install google-chrome-stable_current_amd64.deb
sudo apt install --assume-yes --fix-broken
```
3. 残りも公式ドキュメントの以下を実施すれば、ひとまずXfceのデスクトップには接続できた。
    + [Chrome リモート デスクトップ サービスの構成と開始](https://cloud.google.com/solutions/chrome-desktop-remote-on-compute-engine?hl=ja#configuring_and_starting_the_chrome_remote_desktop_service)
    + [VM インスタンスへの接続](https://cloud.google.com/solutions/chrome-desktop-remote-on-compute-engine?hl=ja#connecting_to_the_vm_instance)

