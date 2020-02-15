# 参考

http://www.urong-answer.org/2018/08/changed-keymap-for-mint60/

1. QMKファームウェアをclone
2. QMKファームウェアをセットアップ
3. ほしいキーマップを探す
4. キーマップを修正
5. ファームウェアをビルド
6. ファームウェアをキーボードに焼く

らしい

# QMKファームウェアインストール

```
$ git clone https://github.com/qmk/qmk_firmware.git
```

# QMKファームウェアをセットアップ

pyenv使ってるときは `pyenv global system` で参照するpythonをもどしておく

```
$ cd qmk_firmware
$ ./util/qmk_install.sh
```

# ほしいキーマップを探す

`keyboards/` ディレクトリ配下に、キーマップがいっぱい入っている。
親指fn対応のminilaキーマップがどれなのかわからなかったので
`minila qmk` でぐぐったらたまたま
https://github.com/qmk/qmk_firmware/tree/master/keyboards/playkbtw/pk60
を発見。
さらにディレクトリを降りていくと
https://github.com/qmk/qmk_firmware/tree/master/keyboards/playkbtw/pk60/keymaps/rfvizarra
を発見。これっぽい。

# ファームウェアをビルド（1）  

まずはminilaのデフォルトキーマップをビルドしてみる。

```
$ make playkbtw/pk60:rfvizarra
QMK Firmware 0.7.95
WARNING:
 Some git sub-modules are out of date or modified, please consider running:
 make git-submodule
 You can ignore this warning if you are not compiling any ChibiOS keyboards,
 or if you have modified the ChibiOS libraries yourself.

Making playkbtw/pk60 with keymap rfvizarra

tmk_core/protocol/lufa.mk:14: lib/lufa/LUFA/makefile: No such file or directory
make[1]: *** No rule to make target `lib/lufa/LUFA/makefile'.  Stop.
make: *** [playkbtw/pk60:rfvizarra] Error 1
Make finished with errors
```

エラー。WARNING通り、 `make git-submodule` を実行してから再実行。

```
$ make playkbtw/pk60:rfvizarra
QMK Firmware 0.7.95
Making playkbtw/pk60 with keymap rfvizarra

avr-gcc (GCC) 8.3.0
Copyright (C) 2018 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

Compiling: keyboards/playkbtw/pk60/pk60.c                                                           [OK]
Compiling: keyboards/playkbtw/pk60/keymaps/rfvizarra/keymap.c                                       [OK]
Compiling: quantum/quantum.c                                                                        [OK]
Compiling: quantum/keymap_common.c                                                                  [OK]
Compiling: quantum/keycode_config.c                                                                 [OK]
Compiling: quantum/matrix.c                                                                         [OK]
Compiling: quantum/debounce/sym_g.c                                                                 [OK]
Compiling: quantum/color.c                                                                          [OK]
Compiling: quantum/rgblight.c                                                                       [OK]
Compiling: quantum/backlight/backlight.c                                                            [OK]
Compiling: quantum/backlight/backlight_avr.c                                                        [OK]
Compiling: drivers/avr/ws2812.c                                                                     [OK]
Compiling: quantum/led_tables.c                                                                     [OK]
Compiling: quantum/process_keycode/process_space_cadet.c                                            [OK]
Compiling: tmk_core/common/host.c                                                                   [OK]
Compiling: tmk_core/common/keyboard.c                                                               [OK]
Compiling: tmk_core/common/action.c                                                                 [OK]
Compiling: tmk_core/common/action_tapping.c                                                         [OK]
Compiling: tmk_core/common/action_macro.c                                                           [OK]
Compiling: tmk_core/common/action_layer.c                                                           [OK]
Compiling: tmk_core/common/action_util.c                                                            [OK]
Compiling: tmk_core/common/print.c                                                                  [OK]
Compiling: tmk_core/common/debug.c                                                                  [OK]
Compiling: tmk_core/common/util.c                                                                   [OK]
Compiling: tmk_core/common/eeconfig.c                                                               [OK]
Compiling: tmk_core/common/report.c                                                                 [OK]
Compiling: tmk_core/common/avr/suspend.c                                                            [OK]
Compiling: tmk_core/common/avr/timer.c                                                              [OK]
Compiling: tmk_core/common/avr/bootloader.c                                                         [OK]
Assembling: tmk_core/common/avr/xprintf.S                                                           [OK]
Compiling: tmk_core/common/bootmagic.c                                                              [OK]
Compiling: tmk_core/common/mousekey.c                                                               [OK]
Compiling: tmk_core/protocol/lufa/lufa.c                                                            [OK]
Compiling: tmk_core/protocol/usb_descriptor.c                                                       [OK]
Compiling: tmk_core/protocol/lufa/outputselect.c                                                    [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Class/Common/HIDParser.c                                       [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/Device_AVR8.c                                        [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/EndpointStream_AVR8.c                                [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/Endpoint_AVR8.c                                      [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/Host_AVR8.c                                          [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/PipeStream_AVR8.c                                    [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/Pipe_AVR8.c                                          [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/USBController_AVR8.c                                 [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/USBInterrupt_AVR8.c                                  [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/ConfigDescriptors.c                                       [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/DeviceStandardReq.c                                       [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/Events.c                                                  [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/HostStandardReq.c                                         [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/USBTask.c                                                 [OK]
Linking: .build/playkbtw_pk60_rfvizarra.elf                                                         [OK]
Creating load file for flashing: .build/playkbtw_pk60_rfvizarra.hex                                 [OK]
Copying playkbtw_pk60_rfvizarra.hex to qmk_firmware folder                                          [OK]
Checking file size of playkbtw_pk60_rfvizarra.hex                                                   [OK]
 * The firmware size is fine - 22476/28672 (78%, 6196 bytes free)
```

通った。
`playkbtw_pk60_rfvizarra.hex` というファイルができている。

# ファームウェアをキーボードに焼く（1）

USBでキーボードをつないで下記を実行。

```
$ make playkbtw/pk60:rfvizarra:avrdude
```

Ali Explessのマニュアルに書いてあるresetを試して `avrdude` してもまったく反応しない。
https://www.aliexpress.com/item/32799437588.html?spm=a2g0s.9042311.0.0.22834c4ddyWYu6
https://drive.google.com/file/d/1DSw1veAr0dJdA9mKuFLPUf6RfAAe24GF/view

PCB名である `YD60MQ reset` とかでググるとまたもやQMKのGithubにヒット。
https://github.com/qmk/qmk_firmware/tree/master/keyboards/yd60mq

を見てみると、 `make yd60mq:default:dfu` でファームウェアを焼け、とある。

マニュアルに書いてあるreset（Fn + Esc）をしながらdfuモードで成功した様子。

```
$ make yd60mq:default:dfu
QMK Firmware 0.7.95
Making yd60mq with keymap default and target dfu

avr-gcc (GCC) 8.3.0
Copyright (C) 2018 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

Size before:
   text	   data	    bss	    dec	    hex	filename
      0	  22256	      0	  22256	   56f0	.build/yd60mq_default.hex

Copying yd60mq_default.hex to qmk_firmware folder                                                   [OK]
Checking file size of yd60mq_default.hex                                                            [OK]
 * The firmware size is fine - 22256/28672 (77%, 6416 bytes free)
dfu-programmer: no device present.
ERROR: Bootloader not found. Trying again in 5s.
dfu-programmer: no device present.
ERROR: Bootloader not found. Trying again in 5s.
Bootloader Version: 0x00 (0)
Erasing flash...  Success
Checking memory from 0x0 to 0x6FFF...  Empty.
Checking memory from 0x0 to 0x56FF...  Empty.
0%                            100%  Programming 0x5700 bytes...
[>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>]  Success
0%                            100%  Reading 0x7000 bytes...
[>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>]  Success
Validating...  Success
0x5700 bytes written into 0x7000 bytes memory (77.68%).
```

# キーマップを修正


http://www.urong-answer.org/2018/08/changed-keymap-for-mint60/
にならって、customのキーマップを作ってみる。

```
$ cp -r keyboards/playkbtw/pk60/keymaps/rfvizarra/ keyboards/playkbtw/pk60/keymaps/custom
```

```
$ make playkbtw/pk60:custom:dfu
```

* JPのキーコードで定義する方法
    https://skyhigh-works.hatenablog.com/entry/2018/11/14/033242


なんかJIS配列にならんなと思ったら、JISかUSかはOS制御だった。
そのため、キーボード側ではキーマップだけ定義してあげて、
例えばShiftキーを押したときにJISと同じ記号を出すようにするには
OS側でJIS配列として読み取ればOK。


# その他

* スタビライザーの音がカシャカシャうるさかったのでグリスを塗った
    + 本当はLube 206ってのが良いらしくて、遊舎工房のレンタル作業場でも使わせてもらえるのだけど
      非売品らしい。205っていうのは売ってるけど、1000円くらいするのと粘土が低い。
    + ネットでググるとSuper Lubeってのがあるけど、キースイッチ用かもしれない。
      値段も1500円くらいする。
    + プラスチックと金具に塗るんだったら、ミニ四駆のグリスで良いんじゃないか...？と思い
      ググると、スタビライザーに使えるという記事をいくつか発見。
      値段も400円くらいだったので、Amazonで購入。
    + 効果はてきめん。スペースバーが特にカシャカシャうるさかったのだけど
      スタビライザーがついていないキーと同じ打鍵音になった。
      あとは部品の劣化が激しくならなければミニ四駆グリスをおすすめしたい。
    + 唯一スタビライザーの打鍵感が気に食わなかった1号機、完璧に気持ちいいやつになった!!!
      ずっと打ってられそう。ちょっと重いけど...


# O-ring

* 新しくZilin？のキースイッチで作成したキーボードの音が激うるさかったので
  O-ringをつけてみた。
  底打ちの音が特にすごかったのだけど、なんとか使えるレベルにはなったかも...
  ただ、やはり静音タイプのキースイッチに比べるとちょっとうるさい...
  どうかな、会社で使えるかな...というレベル

* 打鍵感は若干ゴム感が強くなった。けど、自分としては割と好き。
  タクタイルタイプの軸を使っているけど、タクタイル感自体はあまり失われていない。
  底打ちのときの感触がゴムっぽくなるという感じ。

* Enter, Spaceキーなど広いタイプのキーの打鍵音はまだ結構気になるかも。
  特にEnter, Spaceはよく打鍵するキーなのでちょっと辛いかもしれない。
 
* 強く打鍵したときの音はだいぶ緩和された（というか打鍵の強弱によって差が出なくなった）ので
  それはよかったかもしれない。
  （つまり、今残っている音は底打ち以外の音で、キーが戻るときに生じる音。これはO-ringでは緩和されない）

* たまに押下時に反応しないキーがある。今の所、そのキーを一度強く押し込んでO-ringのゴムを奥まで
  入れ込むようにすれば解消する。



