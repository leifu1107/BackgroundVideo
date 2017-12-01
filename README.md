# BackgroundVideo
视频做背景,模仿QQ,小红书等登录时的背景视频,视频文件放在raw中
----

![](https://github.com/leifu1107/BackgroundVideo/raw/master/art/1512114794341.gif) 

自定义一个VideoView 
```java
public class FullScreenVideoView extends VideoView {
    public FullScreenVideoView(Context context) {
        super(context);
    }

    public FullScreenVideoView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public FullScreenVideoView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }


    // 实现全屏,重新计算高度和宽度(如果不重新测量,视频不能充满整个屏幕)
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec)
    {
        int width = getDefaultSize(0, widthMeasureSpec);
        int height = getDefaultSize(0, heightMeasureSpec);
        setMeasuredDimension(width, height);
    }
}
```


去掉状态栏,全屏显示
```java
getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,
                WindowManager.LayoutParams.FLAG_FULLSCREEN);
```
在Activty中直接使用,把视频放在rse/raw中
```java
 private void playVideoView() {
 
        mVideoView.setVideoURI(Uri.parse("android.resource://" + getPackageName() + "/" + R.raw.video));
        //播放
        
        mVideoView.start();
        //循环播放
        
        mVideoView.setOnCompletionListener(new MediaPlayer.OnCompletionListener() {
            @Override
            public void onCompletion(MediaPlayer mediaPlayer) {
                mVideoView.start();
            }
        });
    }

    //返回重启加载
    @Override
    protected void onRestart() {
        playVideoView();
        super.onRestart();
    }

    //防止锁屏或者切出的时候，在播放
    @Override
    protected void onStop() {
        mVideoView.stopPlayback();
        super.onStop();
    }
```
