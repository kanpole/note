# Android 开发记录

## adb使用

### 链接设备

> adb tcpip 5555  //开启tcpip调试端口
> adb pair 192.168.x.x:port             //需要在同一网段,并且在手机找到无线调试选项并找到端口,进行配对授权
> adb connect  192.168.x.x:debug-port  //自己的设备地址,必须在同一网段
> adb devices        //查看设备列表
> adb unroot/root             // 以非/root模式运行 , -s指定设备编号
> abd remount     //重新挂载分区可写
> abd shell            //登录到设备的shell
> adb -s -r 设备编号 uninstall/install apk    //卸载/安装软件,-s是安装到sd卡,-r是覆盖安装.另外如果安装一直失败,可以加上-t选项,运行test
> adb reboot  //重启
> adb start-server  //启动adb server
> abd kill-server    //停止adb server
> adb shell pm clear package-name    //清理数据和缓存
> adb shell dumpsys activity activities | grep mFocusedActivity  //查看前台Activity
> adb shell dumpsys activity services  packagename   //查看正在运行的services ,包名可不写
> adb shell dumpsys package packagename      //查看应用的详细信息
> adb shell input  opetion  //模拟操作
> adb am/pm     //页面和包管理命令
> adb pull/push  //下载/上传命令
> adb disconnect  //断开链接
> adb shell am start -n packagename  /packagename.activity  //启动一个应用的activity
> adb shell am start -n packagename/.activity      //上面的形式的简写
