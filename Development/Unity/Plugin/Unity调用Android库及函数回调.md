# Unity调用Android库及回调函数

我们在Android Studio中新建一个Android库模块，File->New Module，选择Android Library，编写一个播放音频的库，使用的是Android原生的类，代码如下
``` java
package com.cgtiger.androidunitylib;

import android.media.MediaPlayer;

public class AudioPlay {

    private MediaPlayer mediaPlayer;
    private OnCompletionListener onCompletionListener;

    public interface OnCompletionListener {
        void onCompletion();
    }

    public void playAudio(String path, OnCompletionListener listener)
    {
        if (mediaPlayer == null) {
            mediaPlayer = new MediaPlayer();
            onCompletionListener = listener;
        }
        try {
            mediaPlayer.reset();
            mediaPlayer.setDataSource(path);
            mediaPlayer.prepare();
            mediaPlayer.start();

            mediaPlayer.setOnCompletionListener(new MediaPlayer.OnCompletionListener() {
                @Override
                public void onCompletion(MediaPlayer mp) {
                    if (onCompletionListener != null) {
                        onCompletionListener.onCompletion();
                    }
                }
            });
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void stopPlay(bool release) {
        try {
            mediaPlayer.stop();
            mediaPlayer.reset();
            if (release) {
                mediaPlayer.release();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
生成库文件Build->Make Module，在该模块的`build\outputs\aar`文件夹下可以找到`.aar`文件（Unity 5开始支持.aar类型的Android插件，可以直接放到Unity中），解压该文件可以看到其中有一个classes.jar文件，这个就是我们需要的库文件了，将其放到Unity中的Plugins/Android文件夹中（由于我们没有使用到任何资源文件，也没有使用到任何权限，所以库中的其他文件夹都不需要，Unity中的AndroidManifest.xml文件也不需要修改）

下面来看一下我们在Unity中如何使用这个库文件中的方法以及怎样从Java代码中回调，虽然Unity自带的Java包中提供了UnityPlayer.UnitySendMessage来向C#中发送消息，但是局限性太大，我们这里采用另外一种方式。因为Java中的一切都是基于类的，所以我们需要在C#中创建对应的Java类来进行回调：
``` csharp
//创建一个类继承自AndroidJavaProxy用于回调
class OnComplishCallback : AndroidJavaProxy {
    //构造函数，需要传入基类所需的参数（格式：Java包名.类名$内部接口）
    public OnComplishCallback () : base("com.cgtiger.androidunitylib.AudioPlay$OnCompletionListener") { }
    //用于回调的函数，与Java中回调接口中的方法名称一致
    void onCompletion() {
        Debug.Log("Play Completion");
    }
}
```
``` csharp
public void PlayAudio(string path) {
    //使用Unity封装好的JNI类
    AndroidJavaObject jo = new AndroidJavaObject ("com.cgtiger.androidunitylib.AudioPlay");
    //调用Java对象的方法
    jo.Call("playAudio", path, new OnComplishCallback());
}
```

**参考**

[How to pass an interface to Java from Unity code?](http://answers.unity3d.com/questions/326521/how-to-pass-an-interface-to-java-from-unity-code.html)

[AndroidJavaProxy](https://docs.unity3d.com/ScriptReference/AndroidJavaProxy.html)

[Building Plugins for Android](https://docs.unity3d.com/Manual/PluginsForAndroid.html)

[Does Unity support using aar package as Android plugin library?](https://forum.unity3d.com/threads/does-unity-support-using-aar-package-as-android-plugin-library.272776/)

[Step-by-Step guide for developing Android Plugin for Unity3D (I)](https://xinyustudio.wordpress.com/2015/12/31/step-by-step-guide-for-developing-android-plugin-for-unity3d-i/)