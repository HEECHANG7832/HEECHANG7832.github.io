---
title: Android AsyncTask
author: HEECHANG
layout: post
---

# Android AsyncTask
### Android Study(5.16)

[AsyncTask](https://developer.android.com/reference/android/os/AsyncTask)

안드로이드는 UI를 총괄하는 메인Thread가 존재한다.
일반 Thread는 UI화면 처리를 할 수 없다.
메인과 일반 Thread를 핸들링하는 불편함을 줄이기 위해서
안드로이드에서는 AsyncTask를 제공합니다.

```JAVA
package com.example.facee.droneapp;

import android.os.AsyncTask;
import android.util.Log;

/**
 * Created by facee on 2018-05-09.
 */

public class TCPIPsocketTask extends AsyncTask<Integer, Integer, Integer> {

    private static String TAG = "TCPIPsocketTask";

    //Background작업을 하기 전에 UI작업을 수행할 수 있다.
    @Override
    protected void onPreExecute() {
        Log.i("AsyncTask", "onPreExecute");

    }

    //Background작업 이후에 UI작업을 수행할 수 있다.
    @Override
    protected void onPostExecute(Integer result) {

    }

    //Background에서 수행할 작업을 정의한다.
    @Override
    protected Integer doInBackground(Integer... params) {
        // TODO Auto-generated method stub
        return null;
    }

    //진행중에 값을 가지고 처리하고 싶은 일을 실행시키는 역활을 한다
    //publicProgress()함수로 넘겨준 인자를 받아와서 실행시킨다.
    @Override
    protected void onProgressUpdate(Integer... progress){
    }

}
```

***
실행시 이런식으로 인자를 3개 받게 되는데 각 인자는 다음과 같은 의미가 있습니다.
```JAVA
new MyTask().execute(url1, url2, url3); //실행시

private class MyTask extends AsyncTask<Void, Void, Void> { ... } //정의할때

```

첫번째 : excute()시에 넘겨받을 인자로 doInBackground에서 전달받습니다. //params
두번째 : onProgressUpdate()에서 넘겨받을 인자입니다. //progress
세번째 : onPostExecute()가 호출될때 넘겨받는 인자입니다. //result
***
실행순서

onPreExecute -> doInBackground (publicProgress가 넘겨받은 값으로 onProgressUpdate호출)
-> onPostExecute 순으로 실행된다.
***
**doInBackground실행중에 UI를 변경해야되는 경우 publishProgress()를 사용해서 onProgressUpdate()콜백에서 처리하면 됩니다. 즉 progressbar를 진행시키는 역활로 사용된다고 함**

실행전,실행중,실행후 3가지 상황에서 UI변경하면서 작업을 진행할수 있다!!
