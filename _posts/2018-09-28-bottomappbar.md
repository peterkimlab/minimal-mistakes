---
title: "BottomAppBar 자바로 구현하기"
date: 2018-09-28 14:26:28 -1400
categories: android
---

# Bottom App Bars
**BottomAppBar**의 사용법에 대해 알아보겠습니다. 상단에 고정 되어 있는 Toolbar의 영역을, 화면 공간으로 대체하여, 위쪽 공간을 효율적으로 사용 할 수 있는 **MATERIAL DESIGN**의 주요 feature 입니다.

특징으로는 FAB(FloatingActionButton)과 함께 사용하며, FAB는 다음 화면으로 전환시, Back Button으로 사용 할 수 있습니다. 애니메이션이 나름 ios 같으며, 하단에 Bar가 있으므로, Bottom Sheets으로 Option Menu or Navigation Views를 대체 합니다.

# Usage
MainActivity.java는 Activity로 생성하고, **화면 전환하는 View는 Fragment**로 생성하였다.
일반적으로 appbar는 AndroidManifest에서 선언하는데, 이렇게 되면 BottomAppBar의 fab 애니메이션 적용이 안되서 인지, 선언이 되지 않았다. **결론은 BottomAppBar는 화면 전환 시, BottomAppBar를 detach 하고, fab를 이동 시킨 후, 다시 BottomAppBar를 붙혀야 하는 과정을 거친다.**

[MainActivity.java]
```java
package com.exam.bottomappbar;

import android.os.Bundle;
import android.os.Handler;
import android.support.design.bottomappbar.BottomAppBar;
import android.support.design.widget.FloatingActionButton;
import android.support.v4.app.FragmentTransaction;
import android.support.v7.app.AppCompatActivity;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.widget.Toast;
import com.exam.bottomappbar.Fragment.HomeFragment;
import com.exam.bottomappbar.Fragment.PlusButtonFragment;

public class MainActivity extends AppCompatActivity {

    private Handler handler = new Handler();
    private FragmentTransaction transaction = null;
    private FloatingActionButton fabs = null;
    private BottomAppBar bottomAppBar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        fabs = (FloatingActionButton) findViewById(R.id.fabs);

        if (savedInstanceState == null) {
            transaction = getSupportFragmentManager().beginTransaction();
            transaction.replace(R.id.flContainerHome, HomeFragment.newInstance());
            transaction.commit();
        }

        fabs.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                if (bottomAppBar.getFabAlignmentMode() == BottomAppBar.FAB_ALIGNMENT_MODE_CENTER) {
                    fabs.setImageResource(R.drawable.baseline_reply_white_24);
                    transaction = getSupportFragmentManager().beginTransaction();
                    transaction.addToBackStack(HomeFragment.class.getSimpleName());
                    transaction.add(R.id.flContainerHome, PlusButtonFragment.newInstance());
                    transaction.commit();
                    detachFab();
                    moveToDetails();
                } else {
                    fabs.setImageResource(R.drawable.baseline_add_white_24);
                    detachFab();
                    returnToHome();
                    onBackPressed();
                }
            }
        });
        bottomAppBar = findViewById(R.id.bottom_appbar);
        setSupportActionBar(bottomAppBar);
        bottomAppBar.replaceMenu(R.menu.my_menu);
    }

    public void detachFab() {
        bottomAppBar.setFabAttached(false);
    }

    public void moveToDetails() {
        bottomAppBar.setFabAlignmentMode(BottomAppBar.FAB_ALIGNMENT_MODE_END);
        attachFab();
    }

    public void returnToHome() {
        bottomAppBar.setFabAlignmentMode(BottomAppBar.FAB_ALIGNMENT_MODE_CENTER);
        attachFab();
        if (bottomAppBar.getFabAlignmentMode() == BottomAppBar.FAB_ALIGNMENT_MODE_CENTER) {
            fabs.setImageResource(R.drawable.baseline_add_white_24);
        }
    }

    public void attachFab() {
        handler.postDelayed(new Runnable() {
            @Override
            public void run() {
                bottomAppBar.setFabAttached(true);
            }
        }, 150);
    }

    @Override
    public void onBackPressed() {
        detachFab();
        returnToHome();
        super.onBackPressed();
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.my_menu, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        int itemId = item.getItemId();
        switch (itemId) {
            case R.id.next_btn:
                Toast.makeText(this, "next_btn", Toast.LENGTH_SHORT).show();
                //bottomSheetDialog.show(getSupportFragmentManager(),"bottomSheet");
                return true;
            case R.id.action_settings:
                Toast.makeText(this, "action_settings", Toast.LENGTH_SHORT).show();
                return true;
            default:
                return super.onOptionsItemSelected(item);
        }
    }
}
```
***
MainActivity에는 FrameLayout을 배치 하였고, 이곳에서 HomeFragment로 transaction 시킨다.
또한 차례로 BottomAppBar와 FloatingActionButton를 배치한다.

[activity_main.xml]
```java
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.exam.bottomappbar.MainActivity">

    <FrameLayout
        android:id="@+id/flContainerHome"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

    <android.support.design.bottomappbar.BottomAppBar
        android:id="@+id/bottom_appbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:layout_gravity="bottom"
        app:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
        app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
        app:fabAttached="true"
        app:backgroundTint="@color/colorPrimary"
        app:fabCradleVerticalOffset="12dp"/>

    <android.support.design.widget.FloatingActionButton
        android:id="@+id/fabs"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|center_horizontal"
        android:src="@android:drawable/ic_input_add"
        app:layout_anchor="@+id/bottom_appbar"/>

</android.support.design.widget.CoordinatorLayout>

```
___
build.gradle(Module:app)에는 BottomAppBar 사용을 위해, 아래의 내용을 implementation 한다.
```java
implementation 'com.android.support:design:28.0.0-alpha1'
```
# Output
참조 된 소스는, 아래의 뷰 구성이 가능 합니다.
![Main 화면]({{ "/assets/images/bottomappbar.jpg" | absolute_url }})

# source
전체 소스 : [GitHub](https://github.com/peterkimlab/BottomAppBar)
