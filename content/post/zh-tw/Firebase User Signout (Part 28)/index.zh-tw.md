---
title: "[筆記] Build an Instagram Clone - Firebase User Signout (Part 28)"
date: 2018-06-04T11:38:59-05:00
categories: ["Android", "筆記"]
tags: ["InstagramClone", "Firebase"]
thumbnail: "instagramclone_logo.png"
dirname: "firebase-user-signout-part-28"
---

首先到res/layout底下的<code>fragment_signout.xml</code>做畫面的配置

<!--more-->

<code>fragment_signout.xml</code>

    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

    <RelativeLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="80dp"
        android:layout_centerHorizontal="true">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="20sp"
            android:textColor="@color/black"
            android:id="@+id/tvConfirmSignout"
            android:text="Are you sure you want to sing out?"/>

        <Button
            android:layout_width="200dp"
            android:layout_height="wrap_content"
            android:text="Sign Out"
            android:layout_centerHorizontal="true"
            android:textSize="20sp"
            android:id="@+id/btnConfirmSignout"
            android:layout_below="@+id/tvConfirmSignout"
            android:background="@drawable/white_rounded_button"
            android:layout_marginTop="20dp"/>

    </RelativeLayout>

    <ProgressBar
        android:layout_width="200dp"
        android:layout_height="200dp"
        android:layout_centerInParent="true"
        android:id="@+id/progressBar"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Signing out..."
        android:textSize="20sp"
        android:layout_alignBottom="@+id/progressBar"
        android:layout_alignLeft="@+id/progressBar"
        android:layout_alignRight="@+id/progressBar"
        android:textColor="@color/black"
        android:id="@+id/tvSigningout"/>

    </RelativeLayout>

接著到Profile資料夾底下的<code>SignOutFragment</code>先新增幾個全域變數

<code>SignOutFragment</code>

    // firebase
    private FirebaseAuth mAuth;
    private FirebaseAuth.AuthStateListener mAuthListener;

    private ProgressBar mProgressBar;
    private TextView tvSignout, tvSigningout;

接著在底下的<code>onCreateView</code>宣告這幾個變數

<code>SignOutFragment</code>

    tvSignout = (TextView) view.findViewById(R.id.tvConfirmSignout);
    mProgressBar = (ProgressBar) view.findViewById(R.id.progressBar);
    tvSigningout = (TextView) view.findViewById(R.id.tvSigningout);
        Button btnConfirmSingout = (Button) view.findViewById(R.id.btnConfirmSignout);

    mProgressBar.setVisibility(View.GONE);
    tvSigningout.setVisibility(View.GONE);

然後先到Home資料夾底下的<code>HomeActivity</code>複製所有Firebase的程式碼, 將它貼到<code>onCreateView</code>的下方

<code>SignOutFragment</code>

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

    @Override
    public void onStop() {
        super.onStop();
    }

將<code>checkCurrentUser</code>刪除, 底下有呼叫到這函式的也一併刪除. 接著在<code>setupFirebaseAuth</code>宣告<code>mAuthListener</code>, 當AuthState狀態改變時, AuthStateListener會抓到這個狀態, 將畫面導回到<code>LoginActivity</code>登入畫面, 然後回到登入畫面前我們需要將Activity clear掉, 不然我們登出在按返回鍵的話, 程式還是會導回到之前登入的畫面.

<code>SignOutFragment</code>

    private void setupFirebaseAuth(){
        Log.d(TAG, "setupFirebaseAuth: setting up firebase auth");

        mAuth = FirebaseAuth.getInstance();

        mAuthListener = new FirebaseAuth.AuthStateListener() {
            @Override
            public void onAuthStateChanged(@NonNull FirebaseAuth firebaseAuth) {
                FirebaseUser user = firebaseAuth.getCurrentUser();

                if (user != null) {
                    //user is signed in
                    Log.d(TAG, "setupFirebaseAuth: signed_in" + user.getUid());
                } else {
                    //user is signed out
                    Log.d(TAG, "setupFirebaseAuth: signed_out");

                    Log.d(TAG, "onAuthStateChanged: navigating back to login screen.");
                    Intent intent = new Intent(getActivity(), LoginActivity.class);
                    intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK);
                    startActivity(intent);
                }

            }
        };
    }

到這邊就完成了影片中所介紹的部分, 不過我在輸入密碼的時候發現密碼顯示不是*符號, 所以回到res/layout,<code>activity_login</code>將password的type改成正確的

    <android.support.design.widget.TextInputLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            android:layout_marginBottom="8dp">

        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:inputType="textPassword"
            android:hint="password"
            android:id="@+id/input_password"/>

      </android.support.design.widget.TextInputLayout>

## 畫面截圖

<figure>
<img style="display: block; margin-left: auto; margin-right: auto;" src="screenshot.gif" height="600px">
</figure>

# 影片

{{< youtube rScEX_w4Z0Y >}}