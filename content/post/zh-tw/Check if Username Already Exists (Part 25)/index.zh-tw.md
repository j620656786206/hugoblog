---
title: "[筆記] Build an Instagram Clone - Check if Username Already Exists (Part 25)"
date: 2018-05-13T17:54:49-05:00
categories: ["Android", "筆記"]
tags: ["InstagramClone", "Firebase"]
thumbnail: "instagramclone_logo.png"
dirname: "check-if-username-already-exists-part-25"
---

首先打開Tools/Firebase

{{< figure src="tool_firebase.jpg" >}}

右邊欄位會出現Firebase的assitant, 選擇RealTime Database並點選Save and retrieve data

{{< figure src="firebase_assistant.jpg" >}}

接著按照第二個步驟將Firebase的database加到Module:app的gradle裡面

這邊要稍微注意一下, 一開始我加入的database版本是按照步驟裡的指示, 不過點了sync now以後卻出現錯誤訊息說database的版本跟Firebase還有Authenciation的版本不同, 所以後來我是將database的版本降為一樣

    //Firebase Database
    compile 'com.google.firebase:firebase-database:12.0.1' 

加入成功後右邊會顯示成功訊息
{{< figure src="firebase_database_add.jpg" >}}

完成後先到<code>RegisterActivity</code>加入global variable
<code>Login/RegisterActivity</code>

    //firebase
    private FirebaseAuth mAuth;
    private FirebaseMethods firebaseMethods;
    private FirebaseDatabase mFirebaseDatabase;
    private DatabaseReference myRef;

然後到底下的<code>setupFirebaseAuth</code>加入以下code, 這邊我不是照著原作者的方式做, 因為在之前的sign in部分我已經沒有加入mAuthListener, 所以有部分的程式都跟原作者不同
<code>Login/RegisterActivity</code>

    private void setupFirebaseAuth(){
        Log.d(TAG, "setupFirebaseAuth: setting up firebase auth");
        mAuth = FirebaseAuth.getInstance();
        onStart();

        mFirebaseDatabase = FirebaseDatabase.getInstance();
        myRef = mFirebaseDatabase.getReference();

        FirebaseUser user = FirebaseAuth.getInstance().getCurrentUser();
        if(user != null) {
            // user is signed in
            Log.d(TAG, "onAuthStateChanged: sign_in: " + user.getUid());

            myRef.addListenerForSingleValueEvent(new ValueEventListener() {
                @Override
                public void onDataChange(DataSnapshot dataSnapshot) {
                    //1st check: Make sure the username is  not already in use

                    //add new user to the database

                    //add new user_account_setting to the database
                }

                @Override
                public void onCancelled(DatabaseError databaseError) {

                }
            });

        } else {
            Log.d(TAG, "setupFirebaseAuth: signed_out");
        }
    }

接著到新增一個package叫models, 並加入一個java class叫User

<code>models/User</code>

    public class User {
        private String user_id;
        private String phone_number;
        private String email;
        private String username;

        public User(String user_id, String phone_number, String email, String username) {
            this.user_id = user_id;
            this.phone_number = phone_number;
            this.email = email;
            this.username = username;
        }

        public User() {

        }

        public String getUser_id() {
            return user_id;
        }

        public void setUser_id(String user_id) {
            this.user_id = user_id;
        }

        public String getPhone_number() {
            return phone_number;
        }

        public void setPhone_number(String phone_number) {
            this.phone_number = phone_number;
        }

        public String getEmail() {
            return email;
        }

        public void setEmail(String email) {
            this.email = email;
        }

        public String getUsername() {
            return username;
        }

        public void setUsername(String username) {
            this.username = username;
        }

        @Override
        public String toString() {
            return "User{" +
                    "user_id='" + user_id + '\'' +
                    ", phone_number='" + phone_number + '\'' +
                    ", email='" + email + '\'' +
                    ", username='" + username + '\'' +
                    '}';
        }
    }

接著到<code>FirebaseMethods</code>加入一個新method<code>checkIfUsernameExists</code>
<code>Utils/FirebaseMethods</code>

    public boolean checkIfUsernameExists(String username, DataSnapshot datasnapshot) {
        Log.d(TAG, "checkIfUsernameExists: check if " + username + "already exists.");

        User user = new User();

        for (DataSnapshot ds: datasnapshot.getChildren()) {
            Log.d(TAG, "checkIfUsernameExists: datasnapshot: " + ds);

            user.setUsername(ds.getValue(User.class).getUsername());
        }
    }

然後新增一個java class叫StringManipulation

<code>Utils/StringManipulation</code>

    public class StringManipulation {

        public static String expandUsername(String username){
            return username.replace(".", " ");
        }

        public static String condenseUsername(String username){
            return username.replace(" " , ".");
        }

    }

然後回到<code>FirebaseMethods</code>完成<code>checkIfUsernameExists</code> Method
<code>Utils/FirebaseMethods</code>

    public boolean checkIfUsernameExists(String username, DataSnapshot datasnapshot) {
        Log.d(TAG, "checkIfUsernameExists: check if " + username + "already exists.");

        User user = new User();

        for (DataSnapshot ds: datasnapshot.getChildren()) {
            Log.d(TAG, "checkIfUsernameExists: datasnapshot: " + ds);

            user.setUsername(ds.getValue(User.class).getUsername());
            Log.d(TAG, "checkIfUsernameExists: username: " + user.getUsername());

            if(StringManipulation.expandUsername(user.getUsername()).equals(username)) {
                Log.d(TAG, "checkIfUsernameExists: FOUND A MATCH: " + user.getUsername());

                return true;
            }
        }
        return false;
    }

接著回到<code>RegisterActivity</code>完成剩餘的<code>setupFirebaseAuth</code>
<code>Login/RegisterActivity</code>

    private void setupFirebaseAuth(){
        Log.d(TAG, "setupFirebaseAuth: setting up firebase auth");
        mAuth = FirebaseAuth.getInstance();
        onStart();

        mFirebaseDatabase = FirebaseDatabase.getInstance();
        myRef = mFirebaseDatabase.getReference();

        FirebaseUser user = FirebaseAuth.getInstance().getCurrentUser();
        if(user != null) {
            // user is signed in
            Log.d(TAG, "onAuthStateChanged: sign_in: " + user.getUid());

            myRef.addListenerForSingleValueEvent(new ValueEventListener() {
                @Override
                public void onDataChange(DataSnapshot dataSnapshot) {
                    //1st check: Make sure the username is  not already in use
                    if(firebaseMethods.checkIfUsernameExists(username, dataSnapshot)) {
                        append = myRef.push().getKey().substring(3, 10);
                        Log.d(TAG, "onDataChange: username already exists. Appending random string to name: " + append);
                    }

                    username = username + append;

                    //add new user to the database

                    //add new user_account_setting to the database
                }

                @Override
                public void onCancelled(DatabaseError databaseError) {

                }
            });

        } else {
            Log.d(TAG, "setupFirebaseAuth: signed_out");
        }
    }

影片中的作者在最後的地方應該是手誤打成

    username = username = append;

其餘的部分未完待續