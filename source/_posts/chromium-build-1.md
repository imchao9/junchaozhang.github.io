title:  Chromium编译问题总结
date: 2016-05-06 23:17:27
categories:  "调试"
tag: 
- chromium
comments: true
---



参考资料:
谷歌Chromium官网 https://www.chromium.org/developers/how-tos/get-the-code

       


## 关键部分:
### 1. 问题1
执行`ninja -C out/Default chrome`后,
<!-- more --> 
```
ERROR at //ui/accessibility/BUILD.gn:16:19: Script returned non-zero exit code.
    atk_lib_dir = exec_script(pkg_config_script,
                  ^----------
Current dir: /opt/chromiumorg/chromium/src/out/Default/
Command: python -- /opt/chromiumorg/chromium/src/build/config/linux/pkg-config.py --libdir atk
Returned 1 and printed out:

Error from pkg-config.

stderr:

Package atk was not found in the pkg-config search path.
Perhaps you should add the directory containing `atk.pc'
to the PKG_CONFIG_PATH environment variable
No package 'atk' found

See //BUILD.gn:171:7: which caused the file to be included.
```

####解决:

#####1. 下载atk安装包

http://www.linuxfromscratch.org/blfs/view/svn/x/atk.html

#####2. 编译安装atk:

#####3. 编译安装pango:

将configure中的:
```
have_cairo=true  
have_cairo_png=true  
have_cairo_freetype=true 
```
修改,编译安装

#####4. 编译安装nss(The Network Security Services (NSS) package)
http://www.linuxfromscratch.org/blfs/view/svn/postlfs/nss.html

###2. 问题2

执行
```
ninja -C out_amd64-generic/Release -j1 chrome chrome_sandbox nacl_helper
```

Error Log:

```
../../third_party/llvm-build/Release+Asserts/bin/clang++ -MMD -MF obj/base/base/linux_util.o.d -DUSE_SYMBOLIZE -DV8_DEPRECATION_WARNINGS -DENABLE_MDNS=1 -DENABLE_NOTIFICATIONS -DENABLE_PEPPER_CDMS -DENABLE_PLUGINS=1 -DENABLE_PDF=1 -DENABLE_PRINTING=1 -DENABLE_BASIC_PRINTING=1 -DENABLE_PRINT_PREVIEW=1 -DENABLE_SPELLCHECK=1 -DUSE_UDEV -DUI_COMPOSITOR_IMAGE_TRANSPORT -DUSE_AURA=1 -DUSE_PANGO=1 -DUSE_CAIRO=1 -DUSE_CLIPBOARD_AURAX11=1 -DUSE_DEFAULT_RENDER_THEME=1 -DUSE_GLIB=1 -DUSE_NSS_CERTS=1 -DUSE_X11=1 -DENABLE_WEBRTC=1 -DENABLE_EXTENSIONS=1 -DENABLE_TASK_MANAGER=1 -DENABLE_THEMES=1 -DENABLE_CAPTIVE_PORTAL_DETECTION=1 -DENABLE_SESSION_SERVICE=1 -DENABLE_APP_LIST=1 -DENABLE_SETTINGS_APP=1 -DENABLE_SUPERVISED_USERS=1 -DENABLE_SERVICE_DISCOVERY=1 -DENABLE_AUTOFILL_DIALOG=1 -DENABLE_TOPCHROME_MD=1 -DFULL_SAFE_BROWSING -DSAFE_BROWSING_CSD -DSAFE_BROWSING_DB_LOCAL -DCHROMIUM_BUILD -DENABLE_MEDIA_ROUTER=1 -DFIELDTRIAL_TESTING_ENABLED -DCR_CLANG_REVISION=267383-1 -D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE -D_LARGEFILE64_SOURCE -D__STDC_CONSTANT_MACROS -D__STDC_FORMAT_MACROS -D_DEBUG -DDYNAMIC_ANNOTATIONS_ENABLED=1 -DWTF_USE_DYNAMIC_ANNOTATIONS=1 -D_GLIBCXX_DEBUG=1 -DBASE_IMPLEMENTATION -I../.. -Igen -I../../build/linux/debian_wheezy_amd64-sysroot/usr/local/include/glib-2.0 -I../../build/linux/debian_wheezy_amd64-sysroot/usr/local/lib/glib-2.0/include -fno-strict-aliasing --param=ssp-buffer-size=4 -fstack-protector -funwind-tables -fPIC -pipe -B../../third_party/binutils/Linux_x64/Release/bin -fcolor-diagnostics -fdebug-prefix-map=/opt/chromiumorg/chromium/src=. -pthread -m64 -march=x86-64 -Wall -Werror -Wextra -Wno-missing-field-initializers -Wno-unused-parameter -Wno-c++11-narrowing -Wno-covered-switch-default -Wno-deprecated-register -Wno-unneeded-internal-declaration -Wno-inconsistent-missing-override -Wno-shift-negative-value -Wno-undefined-var-template -O0 -g2 -gsplit-dwarf --sysroot=../../build/linux/debian_wheezy_amd64-sysroot -fvisibility=hidden -Xclang -load -Xclang ../../third_party/llvm-build/Release+Asserts/lib/libFindBadConstructs.so -Xclang -add-plugin -Xclang find-bad-constructs -Xclang -plugin-arg-find-bad-constructs -Xclang check-templates -Xclang -plugin-arg-find-bad-constructs -Xclang follow-macro-expansion -Xclang -plugin-arg-find-bad-constructs -Xclang check-implicit-copy-ctors -Xclang -plugin-arg-find-bad-constructs -Xclang check-ipc -Wheader-hygiene -Wstring-conversion -Wno-char-subscripts -Wexit-time-destructors -fno-threadsafe-statics -fvisibility-inlines-hidden -Wno-undefined-bool-conversion -Wno-tautological-undefined-compare -std=gnu++11 -fno-rtti -fno-exceptions -c ../../base/linux_util.cc -o obj/base/base/linux_util.o
Release/gen/library_loaders/libgio.h ::19: error: gio/gio.h file not found
   #include <gio/gio.h>
                  
1 error generated.
ninja: build stopped: subcommand failed
```

Solve:
    1. 首先我先google了一遍,并且在chromium-dev讨论组中搜索,但没什么结果
    2. 怀疑是include path 的问题 于是查看gcc -v ,发现路径中含有/usr/include,照着 http://linuxintro.org/wiki/Error_messages_and_their_solutions#gio操作:
```
Symptom, in this case building from gqcam:
/usr/include/gdk/gdkapplaunchcontext.h:30:21: fatal error: gio/gio.h: No such file or directory
Solution, in this case for SUSE Linux 11.3:
cp -r /usr/include/glib-2.0/gio/ /usr/include/
```


问题依旧
  
3. 发现报错信息中执行的是clang++ 找到这个目录,发现clang++的include目录在:
`/home/dev/Develop/chromiumorg/chromium/src/third_party/llvm-build/Release+Asserts/lib/clang/3.9.0/include`
将/usr/include中的所有文件复制到该目录,问题解决.


###3. 问题3

```
/opt/chromiumorg/chromium/src/third_party/llvm-build/Release+Asserts/bin/../lib/clang/3.9.0/include/linux/sysinfo.h:10:2: error: unknown type name '__kernel_ulong_t'
```

Solve:
1. grep  -r "__kernel_ulong_t" . 先看看这个变量在什么地方
将`/usr/include/asm-generic/posix-types.h`的:

```
#ifndef __kernel_long_t
typedef long            __kernel_long_t;
typedef unsigned long   __kernel_ulong_t;
#endif
```
添加到
 `/home/dev/Develop/chromiumorg/chromium/src/build/linux/debian_wheezy_amd64-sysroot/usr/include/x86_64-linux-gnu/asm/posix-types.h`

##4. 问题4

build error. Initial unount failed. Possibly crosbug.com/23443

When I build:
`ninja -C out_amd64-generic/Release -j1 chrome chrome_sandbox nacl_helper`

```
losetup: /dev/loop2: detach failed: No such device or address
WARNING : losetup -d /dev/loop2 failed (try 1)
+ sudo losetup -a
/dev/loop0: [0045]:694630 (/home/dev/test.dat)
/dev/loop1: [0045]:694630 (/home/dev/test.dat)
+ grep -H /dev/loop2 /proc/mounts
+ true
+ set +x
losetup: /dev/loop2: detach failed: No such device or address
WARNING : losetup -d /dev/loop2 failed (try 2)

and build .
then get an error:


plain floppy: device "/proc/950/fd/3" busy (Resource temporarily unavailable):
Cannot initialize 'S:'
Bad target s:/ldlinux.sys
syslinux: failed to create ldlinux.sys
ERROR   : 2016年 05月 05日 星期四 15:28:00 CST
ERROR   :  PGID  PPID   PID     ELAPSED     TIME %CPU COMMAND
ERROR   :     6     6    10       13:01 00:00:00  0.0 /bin/bash ./build_image --board=amd64-generic --noenable_rootfs_verification dev
ERROR   :     6    10 30796       00:24 00:00:00  0.2  \_ /bin/bash /mnt/host/source/src/scripts/bin/cros_make_image_bootable/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1 chromiumos_image.bin --force_developer_mode
ERROR   :     6 30796   746       00:00 00:00:00  0.0      \_ /bin/bash /mnt/host/source/src/scripts/update_bootloaders.sh --arch=amd64 --to=/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/chromiumos_image.bin --from=/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/rootfs_dir/boot --vmlinuz=/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/rootfs_dir/boot/vmlinuz --to_offset=127926272 --to_size=16777216 --kernel_partition='/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/vmlinuz.image' --install_syslinux
ERROR   :     6   746   958       00:00 00:00:00  0.0          \_ /bin/bash /mnt/host/source/src/scripts/update_bootloaders.sh--arch=amd64 --to=/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/chromiumos_image.bin --from=/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/rootfs_dir/boot --vmlinuz=/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/rootfs_dir/boot/vmlinuz --to_offset=127926272 --to_size=16777216 --kernel_partition='/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/vmlinuz.image' --install_syslinux
ERROR   :     6   958   959       00:00 00:00:00  0.0              \_ ps f -o pgid,ppid,pid,etime,cputime,%cpu,command
ERROR   : Arguments of 746:  '--arch=amd64' '--to=/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/chromiumos_image.bin' '--from=/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/rootfs_dir/boot''--vmlinuz=/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/rootfs_dir/boot/vmlinuz' '--to_offset=127926272' '--to_size=16777216' '--kernel_partition='/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/vmlinuz.image'' '--install_syslinux'
ERROR   : Backtrace:  (most recent call is last)
ERROR   :  update_bootloaders.sh:195:main(), called: die_err_trap  
ERROR   : 
ERROR   : Command failed:
ERROR   :   Command 'sudo syslinux -d /syslinux "${ESP_DEV}"' exited with nonzero code: 1
umount: /tmp/esp.eZETwO: not mounted
WARNING : Initial unmount failed. Possibly crosbug.com/23443. Retrying
umount: /tmp/esp.eZETwO: not mounted
INFO    : Unmounting image from /mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/stateful_dir and/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/rootfs_dir
Cleaning up /usr/local symlinks for /mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/stateful_dir/dev_image
ERROR   : 2016年 05月 05日 星期四 15:28:06 CST
ERROR   :  PGID  PPID   PID     ELAPSED     TIME %CPU COMMAND
ERROR   :     6     6    10       13:07 00:00:00  0.0 /bin/bash ./build_image --board=amd64-generic --noenable_rootfs_verification dev
ERROR   :     6    10 30796       00:30 00:00:00  0.2  \_ /bin/bash /mnt/host/source/src/scripts/bin/cros_make_image_bootable/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1 chromiumos_image.bin --force_developer_mode
ERROR   :     6 30796  1289       00:00 00:00:00  0.0      \_ /bin/bash /mnt/host/source/src/scripts/bin/cros_make_image_bootable /mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1 chromiumos_image.bin--force_developer_mode
ERROR   :     6  1289  1290       00:00 00:00:00  0.0          \_ ps f -o pgid,ppid,pid,etime,cputime,%cpu,command
ERROR   : Arguments of 30796:  '/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1''chromiumos_image.bin' '--force_developer_mode'
ERROR   : Backtrace:  (most recent call is last)
ERROR   :  cros_make_image_bootable:430:main(), called: make_image_bootable '/mnt/host/source/src/build/images/amd64-generic/R52-8277.0.2016_05_05_1515-a1/chromiumos_image.bin' 
ERROR   :  cros_make_image_bootable:1:make_image_bootable(), called: die 'cros_make_image_bootable failed.' 
ERROR   : 
ERROR   : Error was:
ERROR   :   cros_make_image_bootable failed.

```
Solve:
1. I see that maybe it 's a bug :crosbug.com/23443
2. search in google group, find a url:https://bugs.chromium.org/p/chromium/issues/detail?id=508713
In comment12 , see somebody had solved the problem:
https://chromium-review.googlesource.com/#/c/303962/1/update_bootloaders.sh
edit the update_bootloaders.sh and run the cros_sdk again

Reason:
Maybe the reason is that 
the The current mtools already takes a lock; it's just not patient enough to wait for it and expecting the caller (Syslinux) to retry until available. Syslinux does not retry, does not look at the specific error code and just dies:
We cannot syslinux ESP_DEV directly because of a race condition with udev.