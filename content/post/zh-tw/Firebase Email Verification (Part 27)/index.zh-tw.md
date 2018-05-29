---
title: "[筆記] Build an Instagram Clone - Firebase Email Verification (Part 27)"
date: 2018-05-28T21:07:39-05:00
categories: ["Android", "筆記"]
tags: ["InstagramClone", "Firebase"]
thumbnail: "instagramclone_logo.png"
dirname: "firebase-email-verification-part-27"
---

一開始先到<code>FirebaseMethods</code>的<code>addNewUser</code>加入下面的註解

<!--more-->

<code>FirebaseMethods</code>

    /**
     * Add information to the  users nodes
     * Add information to the  user_account_settings node
     * @param email
     * @param username
     * @param description
     * @param website
     * @param profile_photo
     */
    public void addNewUser(String email, String username, String description, String website, String profile_photo) {}

接著在<code>addNewUser</code>加入一個新method叫<code>sendVerificationEmail</code>

<code>FirebaseMethods</code>

		public void sendVerificationEmail() {
        FirebaseUser user = FirebaseAuth.getInstance().getCurrentUser();

        if(user != null) {
            user.sendEmailVerification()
                    .addOnCompleteListener(new OnCompleteListener<Void>() {
                        @Override
                        public void onComplete(@NonNull Task<Void> task) {
                            if(task.isSuccessful()) {

                            } else {
                                Toast.makeText(mContext, "couldn't send verification email.", Toast.LENGTH_SHORT).show();
                            }
                        }
                    });
        }
    }


這個method我們希望在新user註冊的時候執行, 所以在上面的<code>registerNewEmail</code> 當user成功註冊的時候呼叫這個method

<code>FirebaseMethods</code>

		public void registerNewEmail(final String email, String password, final String username) {
        mAuth.createUserWithEmailAndPassword(email, password)
                .addOnCompleteListener(new OnCompleteListener<AuthResult>() {
                    @Override
                    public void onComplete(@NonNull Task<AuthResult> task) {
                        if (task.isSuccessful()) {
                            Log.d(TAG, "createUserWithEmail:onComplete:" + task.isSuccessful());

														// call sendVerificationEmail here
                            sendVerificationEmail();
                            // Sign in success, update UI with the signed-in user's information
                            Log.d(TAG, "createUserWithEmail:success");
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

然後當新的user註冊後, 這時候他若還沒有點擊email裡的開通連結時, 這個新user應該要在登出的狀態下. 不過因為Firebase註冊新user成功後會自動將這個user登入, 所以我們需要在<code>RegisterActivity</code>的<code>AuthListener</code>呼叫<code>signout()</code>

<code>RegisterActivity</code>

		/**
     * Setup firebase auth object
     */
    private void setupFirebaseAuth(){

			Log.d(TAG, "setupFirebaseAuth: setting up firebase auth");

        mAuth = FirebaseAuth.getInstance();
        mFirebaseDatabase = FirebaseDatabase.getInstance();
        myRef = mFirebaseDatabase.getReference();

        mAuthListener = new FirebaseAuth.AuthStateListener() {
            @Override
            public void onAuthStateChanged(@NonNull FirebaseAuth firebaseAuth) {

                FirebaseUser user = firebaseAuth.getCurrentUser();

								if(user != null) {
                    // user is signed in
                    Log.d(TAG, "onAuthStateChanged: sign_in: " + user.getUid());

                    myRef.addListenerForSingleValueEvent(new ValueEventListener() {
                        @Override
                        public void onDataChange(DataSnapshot dataSnapshot) {
													//1st check: Make sure the username is  not already in use
                            if(firebaseMethods.checkIfUsernameExists(username, dataSnapshot)) {
															/**
															 *
															 */
														}

														username = username + append;

                            //add new user to the database
                            firebaseMethods.addNewUser(email, username, "", "", "");

                            Toast.makeText(mContext, "Signup successful. Sending verification email.", Toast.LENGTH_SHORT).show();

                            mAuth.signOut();
                        }

                        @Override
                        public void onCancelled(DatabaseError databaseError) {

                        }
                    });

										finish();

									} else {
                    // User is signed out
                    Log.d(TAG, "setupFirebaseAuth: signed_out");
                }
            }
        };
    }

接著在else的前面呼叫<code>finish()</code>則是會將畫面導回到上一個Activity, 在這邊的情形是登入畫面<code>LoginActivity</code>. 回到登入畫面後, 當user登入時要檢查他的email是不是已經認證過了, 如果還沒有認證過需要跳出一個訊息說這個email還沒有認證過. 所以到<code>LoginActivity</code>, <code>init</code> method, <code>signInWithEmailAndPassword</code>加入上述功能

<code>LoginActivity</code>

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
                    Toast.makeText(mContext, "You must fill out all the fields", Toast.LENGTH_SHORT).show();
                } else {
                    mProgressBar.setVisibility(View.VISIBLE);
                    mPleaseWait.setVisibility(View.VISIBLE);

                    mAuth.signInWithEmailAndPassword(email, password)
                            .addOnCompleteListener(LoginActivity.this, new OnCompleteListener<AuthResult>() {
                                @Override
                                public void onComplete(@NonNull Task<AuthResult> task) {

                                    // If sign in succeeds, the auth state listener will be notified and logic to handle the
                                    // signed in user can be handled in the listener
                                    if (task.isSuccessful()) {
                                        Log.d(TAG, "signInWithEmail: onComplete: " + task.isSuccessful());
                                        FirebaseUser user = mAuth.getCurrentUser();
																				
																				// new code start here
                                        try{
                                            if(user.isEmailVerified()) {
                                                Log.d(TAG, "onComplete:  success. email is verified.");
                                                Intent intent = new Intent(LoginActivity.this, HomeActivity.class);
                                                startActivity(intent);
                                            } else {
                                                Toast.makeText(mContext, "Email is not verified \n check your email inbox.", Toast.LENGTH_SHORT).show();
                                                mProgressBar.setVisibility(View.GONE);
                                                mPleaseWait.setVisibility(View.GONE);
                                                mAuth.signOut();
                                            }
																				// end here

                                        } catch (NullPointerException e) {
                                            Log.e(TAG, "onComplete: NullPointerException: " + e.getMessage() );
                                        }
                                    } else {
                                        // If sign in fails, display a message to the user.
                                        Log.w(TAG, "signInWithEmail:failure", task.getException());
                                        Toast.makeText(LoginActivity.this, getString(R.string.auth_failed),
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

上面的code檢查email是不是有verified過, 如果有 畫面會導回到<code>HomeActivity</code>, 如果沒有則會跳出訊息並將user登出

## Email截圖
<figure>
<img style="display: block; margin-left: auto; margin-right: auto;" src="email-screenshot.jpg">
</figure>

## 認證截圖
<figure>
<img style="display: block; margin-left: auto; margin-right: auto;" src="verified-screenshot.jpg">
</figure>

## 程式截圖

<figure>
<img style="display: block; margin-left: auto; margin-right: auto;" src="screenshot.gif" height="600px">
</figure>

# 影片

{{< youtube lE24QX8gA_8 >}}