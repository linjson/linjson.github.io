隐藏状态栏设置
```xml
<item name="android:windowFullscreen">true</item>
```
以上设置在android7以下可正常显示

但是在android8(API:28)隐藏状态栏后,状态栏显示会黑色,内容视图不会顶住状态栏位置,需在代码中添加设置异形屏模式
```java
    WindowManager.LayoutParams lp = getWindow().getAttributes();
    //使内容视图部分向上顶,占据状态栏位置
    lp.layoutInDisplayCutoutMode = WindowManager.LayoutParams.LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES;
    getWindow().setAttributes(lp);
```