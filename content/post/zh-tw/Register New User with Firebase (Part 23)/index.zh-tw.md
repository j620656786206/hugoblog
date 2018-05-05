---
title: "[筆記] Build an Instagram Clone - Register New User with Firebase (Part 23)"
date: 2018-04-30T13:14:43+08:00
tags: ["InstagramClone", "Firebase"]
categories: ["Android", "筆記"]
thumbnail: "instagramclone_logo.png"
dirname: "register-new-user-with-firebase-part-23"
---

步驟跟原作者的不太一樣, 不過只要照著Firebase裡的Authentication, sign up new user步驟做大致上就沒有什麼問題

<!--more-->

不過要注意一下的地方在 <code>FirebaseMethods</code>裡的<code>registerNewEmail</code>, 在原本的sign up new user步驟會有

		this, email, password
, 需要將<code>mAuth.createUserWithEmailAndPassword</code>的argument改成
		email, password
然後接下來則是在<code>Toast.makeText</code>的第一個argument給入<code>mContext</code>就沒有什麼太大問題了

# Code

<code>Utils/FirebaseMethods</code>

    public class FirebaseMethods {

        private static final String TAG = "FirebaseMethods";

        //firebase
        private FirebaseAuth mAuth;
        private String userID;

        private Context mContext;

        public FirebaseMethods(Context context) {
            mAuth = FirebaseAuth.getInstance();
            mContext = context;

            if (mAuth.getCurrentUser() != null) {
                userID = mAuth.getCurrentUser().getUid();
            }
        }

        /**
        * Register a new email and password to Firebase Authentication
        * @param email
        * @param password
        * @param username
        */
        public void registerNewEmail(final String email, String password, final String username) {
            mAuth.createUserWithEmailAndPassword(email, password)
                    .addOnCompleteListener(new OnCompleteListener<AuthResult>() {
                        @Override
                        public void onComplete(@NonNull Task<AuthResult> task) {
                            if (task.isSuccessful()) {
                                Log.d(TAG, "createUserWithEmail:onComplete:" + task.isSuccessful());

                                // Sign in success, update UI with the signed-in user's information
                                Log.d(TAG, "createUserWithEmail:success");
                                FirebaseUser user = mAuth.getCurrentUser();
                                userID = mAuth.getCurrentUser().getUid();
                                Log.d(TAG, "onComplete: AuthState changed: " + userID);
                                //updateUI(user);
                            } else {
                                // If sign in fails, display a message to the user.
                                Log.w(TAG, "createUserWithEmail:failure", task.getException());
                                Toast.makeText(mContext, R.string.auth_failed,
                                        Toast.LENGTH_SHORT).show();
                                //updateUI(null);
                            }

                            // ...
                        }
                    });
        }
    }

<code>Login/RegisterActivity</code>

    public class RegisterActivity extends AppCompatActivity {

      @Override
      protected void onCreate(@Nullable Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_register);
          mContext = RegisterActivity.this;
          firebaseMethods = new FirebaseMethods(mContext);
          Log.d(TAG, "onCreate: started.");

          initwidget();
          setupFirebaseAuth();
          init();
      }

      private void init() {
        btnRegister.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                email = mEmail.getText().toString();
                password = mPassword.getText().toString();
                username = mUsername.getText().toString();

                if(checkInputs(email, password, username)) {
                    mProgressBar.setVisibility(View.VISIBLE);
                    loadingPleaseWait.setVisibility(View.VISIBLE);

                    firebaseMethods.registerNewEmail(email, password, username);
                }
            }
        });
      }

				
			private boolean checkInputs(String email, String password, String username) {
					Log.d(TAG, "checkInputs: checking inputs for null values.");

					if(email.equals("") || password.equals("") || username.equals("")){
							Toast.makeText(mContext, "All fields must be filled out.", Toast.LENGTH_SHORT).show();
							return  false;
					}
					return true;
			}

			/**
			* Initialize the activity widgets
			*/
			private void initwidget(){
					Log.d(TAG, "initwidget: Initializing Widgets.");
					mProgressBar = (ProgressBar) findViewById(R.id.progressBar);
					loadingPleaseWait = (TextView) findViewById(R.id.loadingPleaseWait);
					mUsername = (EditText) findViewById(R.id.input_username);
					btnRegister = (Button) findViewById(R.id.btn_register);
					mEmail = (EditText) findViewById(R.id.input_email);
					mPassword = (EditText) findViewById(R.id.input_password);
					mContext = RegisterActivity.this;
					mProgressBar.setVisibility(View.GONE);
					loadingPleaseWait.setVisibility(View.GONE);

			}
    }

# 影片

{{< youtube Zqf68sKjG80 >}}