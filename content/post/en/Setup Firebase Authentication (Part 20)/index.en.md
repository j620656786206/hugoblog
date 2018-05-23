---
title: "[Note] Build an Instagram Clone - Setup Firebase Authentication (Part 20)"
date: 2018-04-13T12:44:10-05:00
tags: ["InstagramClone", "Firebase"]
categories: ["Android", "Note"]
thumbnail: "instagramclone_logo.png"
dirname: "setup-firebase-authentication-part-20"
---

Beacasue the version of Firebase Authentication is different from in the video, the steps are different, I don't have the <code>AuthStateListener</code> part, the rest are basically the same.

<!--more-->

# Code

<code>Home/HomeActivity</code>

    public class HomeActivity extends AppCompatActivity {
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_home);
          Log.d(TAG, "onCreate: starting.");

          initImageLoader();
          setupBottomNavigationView();
          setupViewPager();

          mAuth = FirebaseAuth.getInstance();
          setupFirebaseAuth();
      }

      /*
       * other codes
       */

      /*
      ----------------------------------Firebase--------------------------------------
      */

      private void checkCurrentUser(FirebaseUser user) {
          Log.d(TAG, "checkCurrentUser: checking if user is logged in.");

          if(user == null) {
              Intent intent = new Intent(mContext, LoginActivity.class);
              startActivity(intent);
          }
      }

      /**
      * Setup firebase auth object
      */
      private void setupFirebaseAuth(){
          Log.d(TAG, "setupFirebaseAuth: setting up firebase auth");
          onStart();
      }

      @Override
      public void onStart() {
          super.onStart();
          // Check if user is signed in (non-null) and update UI accordingly.
          FirebaseUser currentUser = mAuth.getCurrentUser();
          //updateUI(currentUser);

          //check if the user is logged in
          checkCurrentUser(currentUser);

          if(currentUser != null) {
              //user is signed in
              Log.d(TAG, "setupFirebaseAuth: signed_in" + currentUser.getUid());
          } else {
              //user is signed out
              Log.d(TAG, "setupFirebaseAuth: signed_out");
          }
      }

    }


<code>manifests/AndroidManifest.xml</code>

    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
              package="tabian.com.instagramclone">

        <uses-permission android:name="android.permission.INTERNET"/>

        <application
            android:allowBackup="true"
            android:icon="@mipmap/ic_launcher"
            android:label="@string/app_name"
            android:roundIcon="@mipmap/ic_launcher_round"
            android:supportsRtl="true"
            android:theme="@style/AppTheme">
            <activity android:name=".Home.HomeActivity">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN"/>

                    <category android:name="android.intent.category.LAUNCHER"/>
                </intent-filter>
            </activity>
            <activity android:name=".Likes.LikesActivity"></activity>
            <activity android:name=".Profile.ProfileActivity"></activity>
            <activity android:name=".Search.SearchActivity"></activity>
            <activity android:name=".Share.ShareActivity"></activity>
            <activity android:name=".Profile.AccountSettingActivity"></activity>
            <activity android:name=".Login.LoginActivity"></activity>
            <activity android:name=".Login.RegisterActivity"></activity>
        </application>

    </manifest>

# Video

{{< youtube 9-qMjrK_sPQ >}}
