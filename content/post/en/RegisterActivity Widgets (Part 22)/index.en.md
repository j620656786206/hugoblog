---
title: "[Note] Build an Instagram Clone - RegisterActivity Widgets (Part 22)"
date: 2018-04-18T16:09:16-05:00
tags: ["InstagramClone", "Firebase"]
categories: ["Android", "Note"]
thumbnail: "instagramclone_logo.png"
dirname: "registeractivity-widgets-part-22"
---

<!--more-->

# Code

<code>Login/LoginActivity</code>

    public class LoginActivity extends AppCompatActivity {

      /*
      ----------------------------------Firebase--------------------------------------
      */
      private void init(){

          //initialize the button for logging in
          Button btnLogin = (Button) findViewById(R.id.btn_login);
          btnLogin.setOnClickListener(new View.OnClickListener() {
              /*
               * Sign in with email and password
               */
            }
          });

          TextView linkSignup = (TextView) findViewById(R.id.link_singup);
          linkSignup.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  Log.d(TAG, "onClick: navigating to register screen");
                  Intent intent = new Intent(LoginActivity.this, RegisterActivity.class);
                  startActivity(intent);
              }
          });

          /*
          If the user is logged in then navigate to HomeActivity and call 'finish()'
          */
          if(mAuth.getCurrentUser() != null) {
              Log.d(TAG, "onClick: navigating to home screen");
              Intent intent = new Intent(LoginActivity.this, HomeActivity.class);
              startActivity(intent);
              finish();
          }
      }
    }

<code>Login/RegisterActivity</code>

    public class RegisterActivity extends AppCompatActivity {

        private static final String TAG = "RegisterActivity";

        private Context mContext;
        private String email, password, username;
        private EditText mEmail, mPassword, mUsername;
        private TextView loadingPleaseWait;
        private Button btnRegister;
        private ProgressBar mProgressBar;

        //firebase
        private FirebaseAuth mAuth;

        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_register);
            Log.d(TAG, "onCreate: started.");

            initwidget();
            setupFirebaseAuth();
        }

        /**
        * Initialize the activity widgets
        */
        private void initwidget(){
            Log.d(TAG, "initwidget: Initializing Widgets.");
            mProgressBar = (ProgressBar) findViewById(R.id.progressBar);
            loadingPleaseWait = (TextView) findViewById(R.id.loadingPleaseWait);
            mEmail = (EditText) findViewById(R.id.input_email);
            mPassword = (EditText) findViewById(R.id.input_password);
            mContext = RegisterActivity.this;
            mProgressBar.setVisibility(View.GONE);
            loadingPleaseWait.setVisibility(View.GONE);

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

<code>res/layout/activity_register.xml</code>

    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        
        <!-- last TextView -->
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Please wait..."
            android:textColor="@color/black"
            android:textSize="20sp"
            android:layout_alignBottom="@+id/progressBar"
            android:layout_alignRight="@+id/progressBar"
            android:layout_alignLeft="@+id/progressBar"
            android:id="@+id/loadingPleaseWait"/>
    </RelativeLayout>


# Video

{{< youtube pxVBTTsyUXg >}}