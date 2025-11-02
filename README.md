# IDF071W
Inoly IDF071W photoframe as Homeassistant Wallpanel

Got few of those digital photoframes, they looked nice for HA panels.
Luckly noticed that they are Android based.
Hardware:
RockChip 3126
512MB RAM
8GB ROM
Speaker
7" display with capacity touch sensor
no battery, 4W, 5V, powered via DC plug or microusb
Android 6, Rooted out of box !
WebView 44 (but its upgradable) 

So, after some fights with it - with also totaly broke Webview in one of it - here are some steps to make it work as HA panel with WebView.
Done with Windows PC, but steps will be the same on linux.

1. Preparation
a. Install ADB Universal driver
b. get adb
c. check adb connection with adb devices
d. (connect to wifi and update it if you didnt, some security patches.. can be skiped, as android 6 is far from secure anyway)
3. Launcher
   a. lets jailbreak it from frameo
   
   adb install com.android.launcher_6.0.1-23_minAPI23(nodpi)_apkmirror.com.apk
   
   adb shell am start -a android.intent.action.MAIN
   
   chose other launcher, and get rid of this ^^&^
   
   adb shell pm uninstall --user 0 net.frameo.frame
   
   GREAT, now you got ALMOST working android tablet, You can do anything from this point.
   
   What doesnt work: No action buttons like HOME, No status ribbon
   
   first can be bypassed with
   
   adb shell input keyevent KEYCODE_HOME
4. Make full Android UI
  a. I dont care - so i skip it ;)
5. Webview

   a. juicy part, Webview installed in root fs is odexed AOSP version v44, so it need to be com.android.webview version, not chrome one. v90 from linageos works fine, armv7, newer might work, but max API23.     Currently 90 is OK for HA.

   b.Lets pusch our webview to temp
   
   adb push webview90.apk /data/local/tmp/webview.apk
   
   c. time for shell
   
   adb shell
   
   (RISKY ! can break OS from this point !!!)
   su
   
   mount -o remount,rw /system
   
   make backup if you like
   
   cp /system/app/webview/webview.apk /system/app/webview.apk_bak
   
   replace with new one
   
   cp /data/local/tmp/webview.apk /system/app/webview/webview.apk
   
   check permisions
   
   ls -l /system/app/webview
   
   (should be -rw-r--r-- root     root     71957611 2025-10-16 23:02 webview.apk)
   
   if isnt
   
   # Fix permissions
   
    chmod 644 /system/app/webview/webview.apk
    
    chown root:root /system/app/webview/webview.apk
   d. We are done here.
   reboot
6. WallPanel
   a. lets install
   adb install d:/Pobrane/WallPanelApp-prod-universal-v0.12.0.apk
   b. And swap it
   adb shell am start -a android.intent.action.MAIN
   c. you can remove normal luncher, but why ...
7. Fin, you are done
8. Usefull comands:
   we got no Brightess setting working, so:
  adb shell settings put system screen_brightness 0
   no HOME button, so
   adb shell input keyevent KEYCODE_HOME
  
 https://www.apkmirror.com/apk/lineageos/android-system-webview-2/android-system-webview-2-90-0-4430-82-release/android-system-webview-90-0-4430-82-13-android-apk-download
   
   

   



