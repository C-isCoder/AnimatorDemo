# AnimatorDemoOne
郭神-Android属性动画完全解析(上)，初识属性动画的基本用法
##核心类：
<pre>
package com.example.iscoder.animatordemoone;

import android.animation.Animator;
import android.animation.AnimatorInflater;
import android.animation.AnimatorListenerAdapter;
import android.animation.AnimatorSet;
import android.animation.ObjectAnimator;
import android.animation.ValueAnimator;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.view.animation.Animation;
import android.view.animation.AnimationSet;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private TextView textView;
    private Button button_alpha;
    private Button button_rotation;
    private Button button_translation;
    private Button button_scaleY;
    private Button button_set;
    private ObjectAnimator animator;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initView();
    }

    private void initView() {
        textView = (TextView) findViewById(R.id.text);
        button_alpha = (Button) findViewById(R.id.button_alpha);
        button_alpha.setOnClickListener(this);
        button_rotation = (Button) findViewById(R.id.button_rotation);
        button_rotation.setOnClickListener(this);
        button_translation = (Button) findViewById(R.id.button_translation);
        button_translation.setOnClickListener(this);
        button_scaleY = (Button) findViewById(R.id.button_scaleY);
        button_scaleY.setOnClickListener(this);
        button_set = (Button) findViewById(R.id.button_set);
        button_set.setOnClickListener(this);

    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.button_alpha:
                animator = ObjectAnimator.ofFloat(textView, "alpha", 1f, 0f, 1f);
                animator.setDuration(5000);
                animator.start();
                break;
            case R.id.button_rotation:
                animator = ObjectAnimator.ofFloat(textView, "rotation", 0f, 360f);
                animator.setDuration(5000);
                animator.start();
                break;
            case R.id.button_translation:
                float curTranslationX = textView.getTranslationX();
                animator = ObjectAnimator.ofFloat(textView, "translationX", curTranslationX, -1000f, curTranslationX);
                animator.setDuration(5000);
                animator.start();
                break;
            case R.id.button_scaleY:
                animator = ObjectAnimator.ofFloat(textView, "scaleY", 1f, 3f, 1f);
                //animator = ObjectAnimator.ofFloat(textView, "scaleX", 1f, 3f, 1f);
                animator.setDuration(5000);
                animator.start();
                break;
            case R.id.button_set:
                //xml文件动画组合
                Animator animator = AnimatorInflater.loadAnimator(MainActivity.this, R.animator.set);
                animator.setTarget(textView);
                animator.start();
                /*ObjectAnimator moveIn = ObjectAnimator.ofFloat(textView, "translationX", -900f, 0f);
                ObjectAnimator rotate = ObjectAnimator.ofFloat(textView, "rotation", 0f, 360f);
                ObjectAnimator fadeInOut = ObjectAnimator.ofFloat(textView, "alpha", 1f, 0f, 1f);
                AnimatorSet animaSet = new AnimatorSet();
                animaSet.play(rotate).with(fadeInOut).after(moveIn);
                animaSet.setDuration(5000);
                animaSet.addListener(new AnimatorListenerAdapter() {
                    @Override
                    public void onAnimationStart(Animator animation) {
                        Toast.makeText(MainActivity.this, "start", Toast.LENGTH_SHORT).show();
                    }
                });*/
              /*  animaSet.addListener(new Animator.AnimatorListener() {
                    @Override
                    public void onAnimationStart(Animator animation) {
                        Toast.makeText(MainActivity.this, "start", Toast.LENGTH_SHORT).show();
                    }

                    @Override
                    public void onAnimationEnd(Animator animation) {
                        Toast.makeText(MainActivity.this, "end", Toast.LENGTH_SHORT).show();
                    }

                    @Override
                    public void onAnimationCancel(Animator animation) {
                        Toast.makeText(MainActivity.this, "cancel", Toast.LENGTH_SHORT).show();
                    }

                    @Override
                    public void onAnimationRepeat(Animator animation) {
                        Toast.makeText(MainActivity.this, "repeat", Toast.LENGTH_SHORT).show();
                    }
                });*/
                //animaSet.start();
        }
    }
}

</pre>
##XML动画配置：
alpha.xml
<?xml version="1.0" encoding="utf-8"?>
<objectAnimator xmlns:android="http://schemas.android.com/apk/res/android"
    android:propertyName="alpha"
    android:valueFrom="1"
    android:valueTo="0"
    android:valueType="floatType" />
transition.xml
<pre>
<?xml version="1.0" encoding="utf-8"?>
<animator xmlns:android="http://schemas.android.com/apk/res/android"
    android:valueFrom="0"
    android:valueTo="100"
    android:valueType="intType" />
</pre>
组合动画set.xml
<pre>
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:ordering="sequentially">
    <objectAnimator
        android:duration="2000"
        android:propertyName="translationX"
        android:valueFrom="-900"
        android:valueTo="0"
        android:valueType="floatType" />
    <set android:ordering="together">
        <objectAnimator
            android:duration="3000"
            android:propertyName="rotation"
            android:valueFrom="0"
            android:valueTo="360"
            android:valueType="floatType" />
        <set android:ordering="sequentially">
            <objectAnimator
                android:duration="1500"
                android:propertyName="alpha"
                android:valueFrom="1"
                android:valueTo="0"
                android:valueType="floatType" />
            <objectAnimator
                android:duration="1500"
                android:propertyName="alpha"
                android:valueFrom="0"
                android:valueTo="1"
                android:valueType="floatType" />
        </set>
    </set>
</set>
</pre>
