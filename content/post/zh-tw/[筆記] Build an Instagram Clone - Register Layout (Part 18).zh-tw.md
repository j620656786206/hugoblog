---
title: "[筆記] Build an Instagram Clone - Register Layout (Part 18)"
date: 2018-04-05T12:59:02-05:00
tags: ["Instagram"]
categories: ["Android"]
---

這個Instagram Clone app我是參照一個youtube頻道[CodingWithMitch](https://www.youtube.com/channel/UCoNZZLhPuuRteu02rh7bzsw)
實作的, 影片總共有97部, 在此筆記一下自己做了些什麼

<!--more-->

# Code

首先複製先前做的<code>activity_login.xml</code>, 然後在layout資料夾貼上, 將新檔案命名成<code>activity_register.xml</code>,
將之前的<code>ImageView</code>刪除並新增一個<code>TextView</code>, 請參考code

    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:paddingTop="55dp"
            android:paddingLeft="25dp"
            android:paddingRight="25dp">

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="REGISTER A NEW ACCOUNT"
                android:textSize="20sp"
                android:gravity="center_horizontal"
                android:textColor="@color/black"
                android:layout_marginBottom="40dp"/>

            <android.support.design.widget.TextInputLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="8dp"
                android:layout_marginBottom="8dp">

                <EditText
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:inputType="textEmailAddress"
                    android:hint="email"
                    android:id="@+id/input_email"/>

            </android.support.design.widget.TextInputLayout>

            <android.support.design.widget.TextInputLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="8dp"
                android:layout_marginBottom="8dp">

                <EditText
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:inputType="text"
                    android:hint="Full Name"
                    android:id="@+id/input_username"/>

            </android.support.design.widget.TextInputLayout>

            <android.support.design.widget.TextInputLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="8dp"
                android:layout_marginBottom="8dp">

                <EditText
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:inputType="textEmailAddress"
                    android:hint="password"
                    android:id="@+id/input_password"/>

            </android.support.design.widget.TextInputLayout>

            <android.support.v7.widget.AppCompatButton
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="25dp"
                android:layout_marginBottom="25dp"
                android:text="Register"
                android:id="@+id/btn_register"
                android:padding="12dp"
                android:background="@drawable/white_rounded_button"/>

        </LinearLayout>

        <ProgressBar
            android:layout_width="200dp"
            android:layout_height="200dp"
            android:id="@+id/progressBar"
            android:layout_centerInParent="true"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Please wait..."
            android:textColor="@color/black"
            android:textSize="20sp"
            android:layout_alignBottom="@+id/progressBar"
            android:layout_alignRight="@+id/progressBar"
            android:layout_alignLeft="@+id/progressBar"/>
    </RelativeLayout>

之後在<code>LikesActivity</code>

    setContentView(R.layout.activity_register);

# 影片

{{< youtube a_Bt2euhh58 >}}