# windowmanger子视图置顶(从状态栏层布局)


### 效果图如下
![](7A5B58DD-A5D7-4839-8F8E-437F339F791B.png)


### Code
```
int size=144;
WindowManager.LayoutParams params = new WindowManager.LayoutParams(
        WindowManager.LayoutParams.MATCH_PARENT,
        size,
        WindowManager.LayoutParams.TYPE_APPLICATION,
        WindowManager.LayoutParams.FLAG_NOT_TOUCH_MODAL | WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE,
        PixelFormat.TRANSLUCENT);
//下一句是关键        
params.systemUiVisibility=View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN;
//android28以上,针对异形屏处理
params.layoutInDisplayCutoutMode= WindowManager.LayoutParams.LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES;

params.gravity = Gravity.TOP;
WindowManager mWindowManager = (WindowManager) getActivity().getSystemService(Context.WINDOW_SERVICE);
            mWindowManager.addView(view, params);
```



