# GifWallpaper

当你看到美丽的GIF动画无缝循环播放时，你有没有想过是否可以将其用作Android设备上的动态壁纸呢？ 嗯，这个demo可以
![demo](gifwallpaper5.gif)

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
接下来，要确保应用程序只能安装在可以运行动态壁纸的设备上，请将以下代码片段添加到清单中：

```xml
<uses-feature
    android:name="android.software.live_wallpaper"
    android:required="true" >
</uses-feature>
```
## 创建服务
创建一个新的Java类并将其命名为  GIFWallpaperService.java。 这个类继承WallpaperService
因为  WallpaperService是一个抽象类，你必须重写它的onCreateEngine方法并返回一个自己的实例Engine，它可以渲染GIF的帧。

要使用GIF动画，您首先必须将其转换为Movie对象。 可以使用Movie类的decodeStream方法来执行此操作。 一旦Movie对象被创建，把它作为一个参数传递给自定义Engine的构造函数。
```java
public class FreeWallpaperService extends WallpaperService {

    @Override
    public WallpaperService.Engine onCreateEngine() {
        try {
            Movie movie = Movie.decodeStream(
                    getResources().getAssets().open("star.gif"));

            return new GIFWallpaperEngine(movie);
        }catch(IOException e){
            Log.w("GIF", "Could not load asset");
            return null;

        }
    }
   ...//engine
}
```
