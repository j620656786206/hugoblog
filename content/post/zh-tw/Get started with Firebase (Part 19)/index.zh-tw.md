---
title: "[筆記] Build an Instagram Clone - Get started with Firebase (Part 19)"
date: 2018-04-10T13:39:37-05:00
tags: ["InstagramClone"]
categories: ["Android"]
thumbnail: "instagramclone_logo.png"
dirname: "get-started-with-firebase-part-19"
---

<!--more-->

# Code

首先新增一個資料夾<code>Login</code>,並新增兩個java class

<code>LoginActivity</code>

    public class LoginActivity extends AppCompatActivity {

      private static final String TAG = "LoginActivity";

      @Override
      protected void onCreate(@Nullable Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_login);
          Log.d(TAG, "onCreate: started.");
      }
    }

<code>RegisterActivity</code>

    public class RegisterActivity extends AppCompatActivity {

        private static final String TAG = "RegisterActivity";

        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_register);
            Log.d(TAG, "onCreate: started.");
        }
    }

接著到Firebase新增專案

首先新增一個專案
{{< figure src="screenshot1.jpg" >}}

然後接著點選將Firebase加到Android
{{< figure src="screenshot2.jpg" >}}

套件名稱請對應到自己程式鐘將硬的package name
{{< figure src="screenshot3.jpg" >}}

這裡的SHA1可在Android Studio右手邊的gradle查到
{{< figure src="screenshot4.jpg" >}}

將SHA1填入後便可點註冊應用程式, 會跳出視窗可下載<code>google-service.json</code>, 並把檔案複製到程式的資料夾中
{{< figure src="screenshot5.jpg" >}}

接著按照firebase步驟在project的gradle加入

    classpath 'com.google.gms:google-services:3.2.0'

{{< figure src="screenshot6.jpg" >}}

然後在到app的gradle加入

    compile 'com.google.firebase:firebase-core:12.0.1'

{{< figure src="screenshot7.jpg" >}}

接著點選提示視窗出現的<code>sync now</code>, 影片中的作者沒有出現問題, 不過我在這出現了以下的錯誤訊息

{{< figure src="screenshot8.jpg" >}}
google了一下, 我在project的gradle加入

    maven {
            url "https://maven.google.com"
        }

然後在<code>sync now</code>一次便成功了



# 影片

{{< youtube xKyARhddKPQ >}}

# 參考文章

* https://stackoverflow.com/questions/37310188/failed-to-resolve-com-google-firebasefirebase-core9-0-0