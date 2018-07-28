---
title: "[Note] Build an Instagram Clone - Firebase User Signout (Part 28)"
date: 2018-06-04T11:38:59-05:00
categories: ["Android", "Note"]
tags: ["InstagramClone", "Firebase"]
thumbnail: "instagramclone_logo.png"
dirname: "firebase-user-signout-part-28"
---

First go to res/layout, make screen setup in <code>fragment_signout.xml</code>

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

Then go to Profile directory, add following global variables in <code>SignOutFragment</code>

<code>SignOutFragment</code>

    // firebase
    private FirebaseAuth mAuth;
    private FirebaseAuth.AuthStateListener mAuthListener;

    private ProgressBar mProgressBar;
    private TextView tvSignout, tvSigningout;

Next declare these variables in <code>onCreateView</code> method

<code>SignOutFragment</code>

    tvSignout = (TextView) view.findViewById(R.id.tvConfirmSignout);
    mProgressBar = (ProgressBar) view.findViewById(R.id.progressBar);
    tvSigningout = (TextView) view.findViewById(R.id.tvSigningout);
        Button btnConfirmSingout = (Button) view.findViewById(R.id.btnConfirmSignout);

    mProgressBar.setVisibility(View.GONE);
    tvSigningout.setVisibility(View.GONE);

Now go to <code>HomeActivity</code> in Home directory, copy all the Firebase code and paste below <code>onCreateView</code> method

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

Then delete <code>checkCurrentUser</code> method, and the code below where call this method. Next, in <code>setupFirebaseAuth</code> declare <code>mAuthListener</code>, when AuthState change, AuthStateListener will catch this change, and redirect the screen back to Login screen <code>LoginActivity</code>. Before back to the login page, we need to clear the Activity stack, otherwise when we click the back button, the page would still redirect to the page where we log in before.

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

To here we finish the aprt in the video, but I found a bug that the password didn't show * character after I typed in, so I went back to res/layout folder, change the passeword type in <code>activity_login</code>

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

## Screenshot

<figure>
<img style="display: block; margin-left: auto; margin-right: auto;" src="screenshot.gif" height="600px">
</figure>

# Video

{{< youtube rScEX_w4Z0Y >}}
