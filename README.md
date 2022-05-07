# haem
A simple manager for headless Android emulators

This tool is largely intended to manage a group of headless emulators for dev services like CI.

## Troubleshooting Android Emulator
Error message:
```
emulator: ERROR: x86 emulation currently requires hardware acceleration!
Please ensure KVM is properly installed and usable.
CPU acceleration status: This user doesn't have permissions to use KVM (/dev/kvm).

---- OR ----
emulator: ERROR: x86 emulation currently requires hardware acceleration!
Please ensure KVM is properly installed and usable.
CPU acceleration status: Could not open /dev/kvm :Permission denied
```
Solution (root perm required):
```bash
groupadd kvm  # create a kvm user group
usermod -G kvm -a yourloginuser # add yourself to the group
echo 'KERNEL=="kvm",GROUP="kvm",MODE="0660"' >> /etc/udev/rules.d/androidUseKVM.rules  # tell udev to grant kvm group the access to /dev/kvm
<reboot>
```

* [more, in Chinese](http://www.cnblogs.com/howdop/p/5347729.html)
```bash
Headless Android Emulator Manager (haem)
Terminology:  只要记住四个概念
target - something like android-19 android-23 目标平台，比如android-19, android-23
abi - x86 x86_64 armeabi-v7a or arm64-v8 ABI，就是x86还是ARM，就这四种选择
avd - an arbitrary name for an Android Virtual Device (AVD)  模拟器配置名
port - every emulator listens on a local port, which can be inferred from its adb serialno, e.g., emulator-5444 模拟器的端口，比如emulator-5444端口对应就是5444
Usage: haem.py [OPTIONS] COMMAND [ARGS]... 好了，很简单，命令，参数和git一样

Options:
  --help  Show this message and exit.

Commands:
  check  确认你的环境能否跑模拟器，没有参数
  create 创建一个模拟器配置 参数 AVD TARGET 见上
  delete 删除一个模拟器配置 参数  AVD
  install 安装一个TARGET
  list 列举现在已经创建的模拟器配置
  running 用adb devices报告现在运行的模拟器
  start 启动模拟器 参数AVD
  stop 停止模拟器 参数PORT
```
