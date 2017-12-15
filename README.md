# GifWallpaper

当你看到美丽的GIF动画无缝循环播放时，你有没有想过是否可以将其用作Android设备上的动态壁纸呢？ 嗯，这个demo可以

## 描述壁纸
动态壁纸需要一个描述它的文件。 创建一个名为res / xml / wallpaper.xml的新XML文件，并使用以下XML替换其内容：

```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <wallpaper
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:label="GIF Wallpaper"
    android:thumbnail="@mipmap/ic_launcher">
  </wallpaper>
```

## 注册服务
需要在AndroidManifest.xml添加使用android.permission.BIND_WALLPAPER权限的服务，代码如下：

```xml
   <service
       android:name=".FreeWallpaperService"
       android:enabled="true"
       android:label="Free Wallpaper"
       android:permission="android.permission.BIND_WALLPAPER" >
         <intent-filter>
             <action android:name="android.service.wallpaper.WallpaperService"/>
         </intent-filter>
         <meta-data
              android:name="android.service.wallpaper"
              android:resource="@xml/wallpaper" >
          </meta-data>
    </service>
```
