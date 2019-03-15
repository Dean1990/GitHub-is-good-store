#### 获取屏幕分辨率

```shell
adb shell wm size
```

#### 模拟点击(540, 1104)坐标

```shell
adb shell input tap 540 1104
```

#### 模拟输入“001” 不支持中文

```shell
adb shell input text “001”
```

#### 模拟home按键 查阅笔记最后的Keyevent Code

```shell
adb shell input keyevent 3
```

#### 模拟滑动，从(250,250)滑动到(300,300) 

```shell
adb shell input swipe 250 250 300 300
```

#### 模拟长按 1秒

```shell
adb shell input swipe x y x y 1000
```

#### 截屏并通过perl输出到本地目录

```shell
adb shell screencap -p | perl -pe 's/\x0D\x0A/\x0A/g' > screen.png
```

#### 解锁屏幕，如果有密码，只会点亮屏幕

```shell
adb shell input keyevent 82
```

#### 查看当前app的入口

```shell
adb shell dumpsys window windows | grep "Current"
```

#### 打开APP [包名/类名]

可能报错 Security exception: Permission Denial: starting Intent 

当组件没有intent-filter时exported 属性默认为false，此组件只能由本应用用户访问，配备了intent-filter后此值改变为true，允许外部调用

```shell
adb shell am start -n cn.xuexi.android/com.alibaba.android.rimet.biz.home.activity.HomeActivity
```

#### 获取当前应用屏幕上所有控件的信息并保存在sdcard下ui.xml文件里面. 只支持sdk版本16以上

```shell
adb shell uiautomator dump /sdcard/ui.xml 
```





#### Keyevent Code

KEYCODE_UNKNOWN=0;
KEYCODE_SOFT_LEFT=1;
KEYCODE_SOFT_RIGHT=2;
KEYCODE_HOME=3;     //home键
KEYCODE_BACK=4;     //back键
KEYCODE_CALL=5;
KEYCODE_ENDCALL=6;
KEYCODE_0=7;
KEYCODE_1=8;
KEYCODE_2=9;
KEYCODE_3=10;
KEYCODE_4=11;
KEYCODE_5=12;
KEYCODE_6=13;
KEYCODE_7=14;
KEYCODE_8=15;
KEYCODE_9=16;
KEYCODE_STAR=17;
KEYCODE_POUND=18;
KEYCODE_DPAD_UP=19;
KEYCODE_DPAD_DOWN=20;
KEYCODE_DPAD_LEFT=21;
KEYCODE_DPAD_RIGHT=22;
KEYCODE_DPAD_CENTER=23;
KEYCODE_VOLUME_UP=24;
KEYCODE_VOLUME_DOWN=25;
KEYCODE_POWER=26;
KEYCODE_CAMERA=27;
KEYCODE_CLEAR=28;
KEYCODE_A=29;
KEYCODE_B=30;
KEYCODE_C=31;
KEYCODE_D=32;
KEYCODE_E=33;
KEYCODE_F=34;
KEYCODE_G=35;
KEYCODE_H=36;
KEYCODE_I=37;
KEYCODE_J=38;
KEYCODE_K=39;
KEYCODE_L=40;
KEYCODE_M=41;
KEYCODE_N=42;
KEYCODE_O=43;
KEYCODE_P=44;
KEYCODE_Q=45;
KEYCODE_R=46;
KEYCODE_S=47;
KEYCODE_T=48;
KEYCODE_U=49;
KEYCODE_V=50;
KEYCODE_W=51;
KEYCODE_X=52;
KEYCODE_Y=53;
KEYCODE_Z=54;
KEYCODE_COMMA=55;
KEYCODE_PERIOD=56;
KEYCODE_ALT_LEFT=57;
KEYCODE_ALT_RIGHT=58;
KEYCODE_SHIFT_LEFT=59;
KEYCODE_SHIFT_RIGHT=60;
KEYCODE_TAB=61;
KEYCODE_SPACE=62;
KEYCODE_SYM=63;
KEYCODE_EXPLORER=64;
KEYCODE_ENVELOPE=65;
KEYCODE_ENTER=66;
KEYCODE_DEL=67;
KEYCODE_GRAVE=68;
KEYCODE_MINUS=69;
KEYCODE_EQUALS=70;
KEYCODE_LEFT_BRACKET=71;
KEYCODE_RIGHT_BRACKET=72;
KEYCODE_BACKSLASH=73;
KEYCODE_SEMICOLON=74;
KEYCODE_APOSTROPHE=75;
KEYCODE_SLASH=76;
KEYCODE_AT=77;
KEYCODE_NUM=78;
KEYCODE_HEADSETHOOK=79;
KEYCODE_FOCUS=80;//*Camera*focus
KEYCODE_PLUS=81;
KEYCODE_MENU=82;
KEYCODE_NOTIFICATION=83;
KEYCODE_SEARCH=84;
KEYCODE_MEDIA_PLAY_PAUSE=85;
KEYCODE_MEDIA_STOP=86;
KEYCODE_MEDIA_NEXT=87;
KEYCODE_MEDIA_PREVIOUS=88;
KEYCODE_MEDIA_REWIND=89;
KEYCODE_MEDIA_FAST_FORWARD=90;
KEYCODE_MUTE=91;