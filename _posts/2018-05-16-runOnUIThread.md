---
title: Android runOnUiThread
author: HEECHANG
layout: post
---

# Android runOnUiThread

### Android Study(5.16)

[runOnUiThread](<https://developer.android.com/reference/android/app/Activity.html#onActivityResult(int>, int, android.content.Intent)

안드로이드는 여러 Thread가 UI자원에 접근해서 동기화 문제를 발생시키는 것을 방지하기 위해 Handler.post()같은 메시지 전달을 통해서만 UI자원 사용을 가능하게 한다

**정의**
runOnUiThread

added in API level 1
void runOnUiThread (Runnable action)
Runs the specified action on the UI thread. If the current thread is the UI thread, then the action is executed immediately. If the current thread is not the UI thread, the action is posted to the event queue of the UI thread.

**사용예**

```JAVA
runOnUiThread(new Runnable() {
        public void run () { // 메시지 큐에 저장될 메시지의 내용
            textView.setText("test");
        }
    });
```

**구조**

```JAVA
public final void runOnUiThread(Runnable action) {
        if (Thread.currentThread() != mUiThread) {
            mHandler.post(action);
        } else {
            action.run();
        }
    }
```

실제 구현된 코드입니다.
현재 Thread가 UIThread가 아니면 UIThread로 post하고 있습니다.
