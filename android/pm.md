```shell
adb shell pm list packages
adb shell pm uninstall --user 0 应用包名
adb shell pm disable --user 0 应用包名

下面提供一些 MIUI 国际版（欧版 [miui.eu](https://xiaomi.eu/)）应用包名（欧版可以随便删）：

com.google.android.googlequicksearchbox （Google）

com.miui.miservice （服务与反馈）

com.mi.health （健康）

com.mi.globalbrowser （国际版浏览器）

com.miui.huanji （小米换机）

com.miui.newmidrive （小米云盘）

com.miui.bugreport （用户反馈）

com.miui.personalassistant （智能助理）

com.android.hotwordenrollment.xgoogle （谷歌助理1）

com.android.hotwordenrollment.okgoogle （谷歌助理2）

com.xiaomi.mirecycle （小米回收）

com.miui.videoplayer （小米视频国际版）

com.google.android.projection.gearhead （Google Auto/Google 汽车）

com.google.android.gms.location.history （Google 地理位置历史记录）

com.google.ar.lens （Google 智能（虚拟）摄像头）

```shell
adb shell pm uninstall --user 0 com.google.android.googlequicksearchbox
adb shell pm uninstall --user 0 com.miui.miservice
adb shell pm uninstall --user 0 com.mi.health
adb shell pm uninstall --user 0 com.mi.globalbrowser
adb shell pm uninstall --user 0 com.miui.huanji
adb shell pm uninstall --user 0 com.miui.newmidrive
adb shell pm uninstall --user 0 com.miui.bugreport
adb shell pm uninstall --user 0 com.miui.personalassistant
adb shell pm uninstall --user 0 com.android.hotwordenrollment.xgoogle
adb shell pm uninstall --user 0 com.android.hotwordenrollment.okgoogle
adb shell pm uninstall --user 0 com.xiaomi.mirecycle
adb shell pm uninstall --user 0 com.miui.videoplayer
adb shell pm uninstall --user 0 com.google.android.projection.gearhead
adb shell pm uninstall --user 0 com.google.android.gms.location.history
adb shell pm uninstall --user 0 com.google.ar.lens
adb shell pm uninstall --user 0 cn.wps.moffice_eng.xiaomi.lite
```

