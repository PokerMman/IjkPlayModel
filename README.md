# IjkPlayModel

IjkPlayerView

Apache 2.0 License 

IjkPlayerView是一个基于ijkplayer的视屏播放库，可以用于播放本地和网络视频。

Screenshot



Using IjkPlayerView

你需要在项目的根 build.gradle 加入如下JitPack仓库链接：

allprojects {
repositories {
...
maven { url 'https://jitpack.io' }
}
}
接着在你的需要依赖的Module的build.gradle加入依赖:

compile 'com.github.Rukey7:IjkPlayerView:{lastest-version}'
其中 {lastest-version} 为最新的版本，你可以查看上面显示的jitpack版本信息，也可以到jitpack.io仓库查看。

Usage

在项目的AndroidManifest.xml文件中队activity进行如下配置：

<activity  
android:name=".IjkPlayerActivity"  
android:configChanges="orientation|keyboardHidden|screenSize"/>
把IjkPlayerView作为一个控件添加到你的布局中：

<com.dl7.player.media.IjkPlayerView  
android:id="@+id/player_view"  
android:layout_width="match_parent"  
android:layout_height="200dp"/>  
最后，在activity中你需要做一些功能上的控制处理，就如下面这样配置：

public class IjkPlayerActivity extends AppCompatActivity {  

private IjkPlayerView mPlayerView;  

@Override  
protected void onCreate(Bundle savedInstanceState) {  
super.onCreate(savedInstanceState);  
setContentView(R.layout.activity_ijk_player);  
setSupportActionBar(mToolbar);  
//  Choose any one interface you need, init() must be the first to use.
Glide.with(this).load(IMAGE_URL).fitCenter().into(mPlayerView.mPlayerThumb); // Show the thumb before play
mPlayerView.init()              // Initialize, the first to use 
.setTitle("Title")  	// set title  
.setSkipTip(1000*60*1)  // set the position you want to skip  
.enableOrientation()    // enable orientation 
//      .setVideoPath(VIDEO_URL)    // set video url  
.setVideoSource(null, VIDEO_URL, VIDEO_URL, VIDEO_URL, null) // set multiple video url  
.setMediaQuality(IjkPlayerView.MEDIA_QUALITY_HIGH)  // set the initial video url
.enableDanmaku()        // enable Danmaku  
.setDanmakuSource(getResources().openRawResource(R.raw.comments)) // add Danmaku source, you need to use enableDanmaku() first 
.start();   // Start playing 
}  

@Override  
protected void onResume() {  
super.onResume();  
mPlayerView.onResume();  
}  

@Override  
protected void onPause() {  
super.onPause();  
mPlayerView.onPause();  
}  

@Override  
protected void onDestroy() {  
super.onDestroy();  
mPlayerView.onDestroy();  
}  

@Override  
public void onConfigurationChanged(Configuration newConfig) {  
super.onConfigurationChanged(newConfig);  
mPlayerView.configurationChanged(newConfig);  
}  

@Override  
public boolean onKeyDown(int keyCode, KeyEvent event) {  
if (mPlayerView.handleVolumeKey(keyCode)) {  
return true;  
}  
return super.onKeyDown(keyCode, event);  
}  

@Override  
public void onBackPressed() {  
if (mPlayerView.onBackPressed()) {  
return;  
}  
super.onBackPressed();  
} 
}   
如果你要使用固定全屏播放，可以按下处理：


@Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
mPlayerView = new IjkPlayerView(this);
setContentView(mPlayerView);
Glide.with(this).load(IMAGE_URL).fitCenter().into(mPlayerView.mPlayerThumb);
mPlayerView.init()
.setTitle("Title")
.alwaysFullScreen()			// keep fullscreen
.setVideoPath(VIDEO_URL)	
.start();
}
库里也提供了自定义弹幕的功能，可根据需要添加，更多信息请查看例子。


mPlayerView.init()
.enableDanmaku()
.setDanmakuCustomParser(new DanmakuParser(), DanmakuLoader.instance(), DanmakuConverter.instance())
.setDanmakuSource(stream)
.setVideoPath(VIDEO_URL)	
.setDanmakuListener(new OnDanmakuListener<DanmakuData>() {
@Override
public boolean isValid() {
return true;
}

@Override
public void onDataObtain(DanmakuData data) {
}
});
Other

可能影响到沉浸式全屏的几个问题：

使用 android:fitssystemwindows="true" 属性
使用 SystemBarTint 来渲染状态栏
事实上，你要确保在变换为全屏时IjkPlayerView控件能够填充整个屏幕，不然就会出现播放界面被挤压的情况。这个问题是因为全屏的时候是对当前的IjkPlayerView直接做宽高，所以有局限性，你可以参考别的播放库有别的实现方式来避免这个问题。

ChangeLog

v1.0.3 -> v1.0.4(1.0.4)

1、最开始依赖版本都在前面加了个‘v’，之前有人反馈库依赖不了是由于少了这个，后面依赖版本加了不带‘v’的；

2、增加多个视频切换播放功能；

3、增加网络异常的处理；

Thanks

ijkplayer
DanmakuFlameMaster
jjdxm_ijkplayer
License

Copyright 2016 Rukey7

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
