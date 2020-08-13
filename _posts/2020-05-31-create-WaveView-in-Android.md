---
layout: post
title: Hướng dẫn tạo WaveView trong Android
date: 2020-05-31 22:30:20 +0700
description: Hướng dẫn tạo WaveView trong Android # Add post description (optional)
img:  /waveview-android/caption.png # Add image post (optional)
fig-caption: /waveview-android/caption.png # Add figcaption (optional)
tags: [Android]
published: true

---

<h1>Giới thiệu</h1>
Trong bài viết này tôi sẽ giới thiệu đến các bạn cách tạo WaveView trong Android. Đây là thư viện tạo sóng trong UI, ở đây tôi sử dụng thư viện của NIGHT (tên tác giả). Chi tiết thư viện các bạn có thể xem tại : [library](https://github.com/gelitenight/WaveView)<br>
Bằng cách sử dụng điều này chúng ta có thể hiển thị trạng thái tiến độ (hoặc thứ gì khác) theo phần trăm. <br>
<h2>Step 1: Tạo dự án</h2>
Tạo project mới trong Android Studio (Empty Project) có tên là WaveView.

![create project](/assets/img/waveview-android/create-project-android.png)

> Chọn finish.
<h2> Step 2: Cài đặt thư viện cho WaveView</h2>
Mở file Gradle -> build.gradle <br>

![gradle](/assets/img/waveview-android/gradle-position.png)

Thêm dependency vào file build.gradle của ứng dụng

    allprojects {  
      repositories {  
      maven { url "https://jitpack.io" }  
	  }
     }
     
    implementation 'com.gelitenight.waveview:waveview:1.0.0'

![change gradle](/assets/img/waveview-android/change-gradle.png)

> Nhớ click Synch now
<h2>Step 3: Chỉnh sửa giao diện </h2>
Tiếp theo mở file: app -> res -> layout -> activity_main.xml.<br>

![activity_main](/assets/img/waveview-android/layout-main-position.png)

Chỉnh sửa file như sau:<br>

![acitivity_main](/assets/img/waveview-android/acitivity_main_content.png)

<h2>Step 4: Tạo file WaveHelper.java</h2>
Tạo file WaveHelper.java tại app -> java:<br>
Thêm đoạn code như sau:<br>
  

    import android.animation.Animator;  
    import android.animation.AnimatorSet;  
    import android.animation.ObjectAnimator;  
    import android.animation.ValueAnimator;  
    import android.view.animation.DecelerateInterpolator;  
    import android.view.animation.LinearInterpolator;  
    import com.gelitenight.waveview.library.WaveView;  
    import java.util.ArrayList;  
    import java.util.List;  
      
    public class WaveHelper {  
        private WaveView mWaveView;  
      
     private AnimatorSet mAnimatorSet;  
      
     public WaveHelper(WaveView waveView, float progress) {  
            mWaveView = waveView;  
      initAnimation(progress);  
      }  
      
        public void start() {  
            mWaveView.setShowWave(true);  
     if (mAnimatorSet != null) {  
                mAnimatorSet.start();  
      }  
        }  
      
        private void initAnimation(float progress) {  
            List<Animator> animators = new ArrayList<>();  
      
      // horizontal animation.  
     // wave waves infinitely.  ObjectAnimator waveShiftAnim = ObjectAnimator.ofFloat(  
                    mWaveView, "waveShiftRatio", 0f, 1f);  
      waveShiftAnim.setRepeatCount(ValueAnimator.INFINITE);  
      waveShiftAnim.setDuration(900);  
      waveShiftAnim.setInterpolator(new LinearInterpolator());  
      animators.add(waveShiftAnim);  
      
      // vertical animation.  
     // water level increases from 0 to center of WaveView  ObjectAnimator waterLevelAnim = ObjectAnimator.ofFloat(  
                    mWaveView, "waterLevelRatio", 0f, progress);  
      waterLevelAnim.setDuration(5000);  
      waterLevelAnim.setInterpolator(new DecelerateInterpolator());  
      animators.add(waterLevelAnim);  
      
      // amplitude animation.  
     // wave grows big then grows small, repeatedly  ObjectAnimator amplitudeAnim = ObjectAnimator.ofFloat(  
                    mWaveView, "amplitudeRatio", 0.0001f,progress/10f);  
      amplitudeAnim.setRepeatCount(ValueAnimator.INFINITE);  
      amplitudeAnim.setRepeatMode(ValueAnimator.REVERSE);  
      amplitudeAnim.setDuration(5000);  
      amplitudeAnim.setInterpolator(new LinearInterpolator());  
      animators.add(amplitudeAnim);  
      
      mAnimatorSet = new AnimatorSet();  
      mAnimatorSet.playTogether(animators);  
      }  
      
        public void cancel() {  
            if (mAnimatorSet != null) {  
                mAnimatorSet.end();  
		    }  
        }  
    }
> Có thể format code hơi lệch so với IDE, các bạn thông cảm ^.^
<h2>Step 5: MainActivity.java</h2>
Mở file : app -> java -> MainActivity.java<br>
Thay đổi code như sau: <br> 

![MainActivity.java](/assets/img/waveview-android/main-activity.png)

<h2>Step 6: Thực thi ứng dụng </h2>
Chắc hẳn các bạn cũng biết cách biên dịch ứng dụng trong Android Studio, nên tôi không nhắc lại.<br>
Đây là kết quả:<br>

![result](/assets/img/waveview-android/result.png)

Chúc các bạn thành công! Các bạn hãy thích và bình luận để góp ý cho tôi nhé ^.^.
