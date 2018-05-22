---
title: "[Note] Build an Instagram Clone - Get started with Firebase (Part 19)"
date: 2018-04-10T13:39:37-05:00
tags: ["InstagramClone", "Firebase", "SHA1"]
categories: ["Android", "Note"]
thumbnail: "instagramclone_logo.png"
dirname: "get-started-with-firebase-part-19"
---

# Code

Create a new directory called<code>Login</code> first, and add two new java class

<!--more-->

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

Then go to Firebase console and add a new project

{{< figure src="screenshot1.jpg" >}}

next click Android icon to add the Firebase

{{< figure src="screenshot2.jpg" >}}

the package name should be the same as in the program

{{< figure src="screenshot3.jpg" >}}

SHA1 can be found in the gradle from the right bar of Android Studio

{{< figure src="screenshot4.jpg" >}}

After fill out SHA1, click register, it would pop out an window that can download <code>google-service.json</code> file, then copy this file into the program's directory
{{< figure src="screenshot5.jpg" >}}

Then go back to Android Studio and follow the steps in the Firebase, add the following code in project's gradle

    classpath 'com.google.gms:google-services:3.2.0'

{{< figure src="screenshot6.jpg" >}}

then go to app's gradle and add

    compile 'com.google.firebase:firebase-core:12.0.1'

{{< figure src="screenshot7.jpg" >}}

after adding the codes, I clicked <code>sync now</code> on the window, the author in the video didn't have problem here, but I have the following error message

{{< figure src="screenshot8.jpg" >}}

after a little of google, I added the following code in the project gradle

    maven {
            url "https://maven.google.com"
        }

then clicked <code>sync now</code> and it work

# Video

{{< youtube xKyARhddKPQ >}}

# Reference

* https://stackoverflow.com/questions/37310188/failed-to-resolve-com-google-firebasefirebase-core9-0-0