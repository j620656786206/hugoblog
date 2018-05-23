---
title: "[Note] Build an Instagram Clone - Testing Firebase Authentication (Part 21)"
date: 2018-04-17T12:45:25-05:00
tags: ["InstagramClone", "Firebase"]
categories: ["Android", "Note"]
thumbnail: "instagramclone_logo.png"
dirname: "testing-firebase-authentication-part-21"
---

Copy the Firebase part in <code>HomeActivity</code> from the last part, and follow the instructions in Firebase. I was getting crash when I tried to run the app on emulator,

<!--more-->

then I found out I have to add <code>mAuth = FirebaseAuth.getInstance();</code> in <code>private void setupFirebaseAuth()</code>, I declared in <code>onCreate</code> in <code>HomeActivity</code> previously.

# Code

<code>Login/LoginActivity</code>

    public class LoginActivity extends AppCompatActivity {

        private static final String TAG = "LoginActivity";

        //firebase
        private FirebaseAuth mAuth;

        private Context mContext;
        private ProgressBar mProgressBar;
        private EditText mEmail, mPassword;
        private TextView mPleaseWait;

        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_login);
            mProgressBar = (ProgressBar) findViewById(R.id.progressBar);
            mPleaseWait = (TextView) findViewById(R.id.pleaseWait);
            mEmail = (EditText) findViewById(R.id.input_email);
            mPassword = (EditText) findViewById(R.id.input_password);
            mContext = LoginActivity.this;
            Log.d(TAG, "onCreate: started.");

            mPleaseWait.setVisibility(View.GONE);
            mProgressBar.setVisibility(View.GONE);

            setupFirebaseAuth();
            init();
        }

        private boolean isStringNull(String string){
            Log.d(TAG, "isNull: checking string if null");

            if(string.equals("")) {
                return true;
            } else {
                return false;
            }
        }

        /*
        ----------------------------------Firebase--------------------------------------
        */
        private void init(){

            //initialize the button for logging in
            Button btnLogin = (Button) findViewById(R.id.btn_login);
            btnLogin.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    Log.d(TAG, "onClick: attemping to log in.");

                    String email = mEmail.getText().toString();
                    String password = mPassword.getText().toString();

                    if(isStringNull(email) && isStringNull(password)){
                        Toast.makeText(mContext, 
                          "You must fill out all the fields",
                          Toast.LENGTH_SHORT).show();
                    } else {
                        mProgressBar.setVisibility(View.VISIBLE);
                        mPleaseWait.setVisibility(View.VISIBLE);

                        mAuth.signInWithEmailAndPassword(email, password)
                                .addOnCompleteListener(LoginActivity.this, 
                                new OnCompleteListener<AuthResult>() {
                                    @Override
                                    public void onComplete(
                                      @NonNull Task<AuthResult> task) {
                                        if (task.isSuccessful()) {
                                            // Sign in success, 
                                            // update UI with the signed-in user's information
                                            Log.d(TAG, "signInWithEmail:success");
                                            Toast.makeText(LoginActivity.this, 
                                              getString(R.string.auth_success),
                                              Toast.LENGTH_SHORT).show();
                                            FirebaseUser user = mAuth.getCurrentUser();
                                            //updateUI(user);
                                            mProgressBar.setVisibility(View.GONE);
                                            mPleaseWait.setVisibility(View.GONE);
                                        } else {
                                            // If sign in fails, display a message to the user.
                                            Log.w(TAG, "signInWithEmail:failure", 
                                                    task.getException());
                                            Toast.makeText(LoginActivity.this, 
                                                getString(R.string.auth_failed),
                                                Toast.LENGTH_SHORT).show();
                                            //updateUI(null);
                                            mProgressBar.setVisibility(View.GONE);
                                            mPleaseWait.setVisibility(View.GONE);
                                        }

                                        // ...
                                    }
                                });
                    }
                }
            });
        }

        /**
        * Setup firebase auth object
        */
        private void setupFirebaseAuth(){
            Log.d(TAG, "setupFirebaseAuth: setting up firebase auth");
            mAuth = FirebaseAuth.getInstance();
            onStart();
        }

        @Override
        public void onStart() {
            super.onStart();
            /*
            // Check if user is signed in (non-null) and update UI accordingly.
            FirebaseUser currentUser = mAuth.getCurrentUser();
            //updateUI(currentUser);

            if(currentUser != null) {
                //user is signed in
                Log.d(TAG, "setupFirebaseAuth: signed_in" + currentUser.getUid());
            } else {
                //user is signed out
                Log.d(TAG, "setupFirebaseAuth: signed_out");
            }
            */
        }

        @Override
        protected void onStop() {
            super.onStop();
        }
    }

<code>res/values/strings</code>

    <resources>
        <string name="app_name">InstagramClone</string>

        <string name="edit_profile_fragment">Edit Profile</string>
        <string name="sign_out_fragment">Sign Out</string>

        <!-- Firebase messages -->
        <string name="auth_failed">Fail to Authentication</string>
        <string name="auth_success">Authentication Success</string>
    </resources>

<code>res/layout/activity_login.xml</code>

    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

    <!--
      Input Fields
      Button Field
      Signup  Field
    --> 

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
        android:layout_alignLeft="@+id/progressBar"
        android:id="@+id/pleaseWait"/>
    </RelativeLayout>

# Video

{{< youtube 9iGpE-9hzCU >}}