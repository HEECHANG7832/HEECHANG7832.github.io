---
title: Android와 JAVA server Socket통신 + GPS
author: HEECHANG
layout: post
---

# Android와 JAVA server Socket통신 + GPS
### Android Study(5.16)

[Socket](https://developer.android.com/reference/java/net/Socket)

android 클라이언트와 Ubunto 위에 java server와 소켓통신을 해볼려고 한다.

안드로이드에서는 주기적으로 gps정보를 받고 socket통신으로 gps좌표를 java 서버에 전달합니다.


###안드로이드 소스

**서버와 소켓생성을 요구하는 코드**
```JAVA
//socket생성 thread
        Thread worker = new Thread() {    
            public void run() {

              //socket생성부분
                try {
                    //소켓을 생성하고 입출력 스트립을 소켓에 연결한다.
                    socket = new Socket("IP주소!!!", 4000); //소켓생성
                    out = new PrintWriter(socket.getOutputStream(), true); //데이터를 내보내는 스트림
                    in = new BufferedReader(new InputStreamReader( //데이터를 수신하는 스트림
                            socket.getInputStream()));

                    //소켓이 연결되어 있는지 확인
                    if(socket.isConnected()){
                      //연결한 서버의 주소 확인
                        Log.i("TAG", String.valueOf(socket.getInetAddress()));
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }

                //소켓에 데이터 전달
                //2초마다 gps정보를 out스트림으로 전달한다
                TimerTask tt = new TimerTask(){
                    @Override
                    public void run(){
                        data = Double.toString(latPoint)+","+Double.toString(lngPoint);
                        Log.w("NETWORK", " " + data);
                        if (data != null) { //만약 데이타가 아무것도 입력된 것이 아니라면
                            out.println(data); //data를   stream 형태로 변형하여 전송.
                        }
                    }
                };
                Timer timer = new Timer();
                timer.schedule(tt, 0, 2000);

                //소켓에서 데이터를 읽는 부분
                   try {
                       while (true) {
                           data = in.readLine(); // in으로 받은 데이타를 String 형태로 읽어 data 에 저장
                           output.post(new Runnable() {
                               public void run() {
                                   output.setText(data); //글자출력칸에 서버가 보낸 메시지를 받는다.
                               }
                           });
                       }
                   } catch (Exception e) {
                   }
            }
        };
        worker.start();

```

**GPS**
[Location](https://developer.android.com/guide/topics/location/strategies)  

퍼미션을 제공합니다.
```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

```

두가지 방법으로 위치정보를 받을 수 있다.
1. Network provider
2. GPS provider

gps 소스
```JAVA
//for gps
    LocationManager locationManager;
    LocationListener locationListener;
    boolean isGPSEnabled;
    boolean isNetworkEnabled; //gps와 network 확인
    Double latPoint = 0.0;
    Double lngPoint = 0.0;

    //for gps
    locationManager = (LocationManager) getSystemService(Context.LOCATION_SERVICE);
    isGPSEnabled = locationManager.isProviderEnabled(LocationManager.GPS_PROVIDER);
    isNetworkEnabled = locationManager.isProviderEnabled(LocationManager.NETWORK_PROVIDER); //gps와 network 확인
    gpsSetting(context);//gps 값을 계속 수신받는다


public void gpsSetting(Context context) {
  final List<String> m_lstProviders = locationManager.getProviders(false);
  locationListener = new LocationListener() {
      @Override
      public void onLocationChanged(Location location) {
          Log.e("location", "[" + location.getProvider() + "] (" + location.getLatitude() + "," + location.getLongitude() + ")");
          latPoint = location.getLatitude();
          lngPoint = location.getLongitude();

          locationManager.removeUpdates(locationListener);
      }

      @Override
      public void onStatusChanged(String provider, int status, Bundle extras) {
          Log.e("onStatusChanged", "onStatusChanged");
      }

      @Override
      public void onProviderEnabled(String provider) {
          Log.e("onProviderEnabled", "onProviderEnabled");
      }

      @Override
      public void onProviderDisabled(String provider) {
          Log.e("onProviderDisabled", "onProviderDisabled");
      }
  };

  //permission check
  if (ActivityCompat.checkSelfPermission(context, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED
          && ActivityCompat.checkSelfPermission(context, Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
      Toast.makeText(context, "Don't have permission", Toast.LENGTH_LONG).show();
      return;
  }

  locationManager.requestLocationUpdates(LocationManager.GPS_PROVIDER, 1000, 1, locationListener);
  locationManager.requestLocationUpdates(LocationManager.NETWORK_PROVIDER, 1000, 1, locationListener);

  Log.i(TAG, Double.toString(latPoint));
  Log.i(TAG, Double.toString(lngPoint));
}
```

**java 서버**

수신받은 gps정보를 txt파일로 저장한다
```JAVA

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class ServerMain {

   public static void main(String[] args) throws IOException {

      ServerSocket serverSocket = null;
      Socket clientSocket = null;
      PrintWriter out = null;
      BufferedReader in = null;

      //기록할 파일의 경로
      String folderpath = "/var/www/html/gisa.txt";

      serverSocket = new ServerSocket(4000);

      try {
        //연결요정이 들어오는 것을 기다린다
         clientSocket = serverSocket.accept();
         System.out.println("연결성공");

         out = new PrintWriter(clientSocket.getOutputStream(), true);
         in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));

         while (true) {
            String inputLine = null;
            inputLine = in.readLine(); //소켓에 들어온 정보를 읽는다
            System.out.println("gps reception: " + inputLine);
            String[] words = inputLine.split(",");

          //읽어온 정보를 파일에 기록한다
            if (inputLine != null) {
               // ?뚯씪???곌린
               try {
                  FileWriter f = new FileWriter(folderpath);
                  BufferedWriter fileout = new BufferedWriter(f);
                  fileout.write("{\"Lat\":\""+words[0]+"\",\"Lng\":\""+words[1]+"\"}");

                  System.out.println("writing success");
                  fileout.close();
               } catch (IOException e) {
                  System.err.println(e);
                  System.exit(1);
               }

               //받은 정보를 그대로 재전송한다
               out.println(inputLine);
               if (inputLine.equals("quit"))
                  break;
            }
         }
         out.close();
         in.close();
         clientSocket.close();
         serverSocket.close();
      } catch (Exception e) {
         e.printStackTrace();
      }
   }
}
```
