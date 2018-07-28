---
title: "[Note] Build an Instagram Clone - Check if Username Already Exists (Part 25)"
date: 2018-05-13T17:54:49-05:00
categories: ["Android", "Note"]
tags: ["InstagramClone", "Firebase"]
thumbnail: "instagramclone_logo.png"
dirname: "check-if-username-already-exists-part-25"
---

First open Tools/Firebase Android Studio

{{< figure src="tool_firebase.jpg" >}}

Right hand sidebar would have Firebase's tips, choose RealTime Database and click Save and retrieve data

{{< figure src="firebase_assistant.jpg" >}}

Then followe the second step to add Firebase Database into Module:app gradle

<!--more-->

Note here, the version of database I was adding was followed by the step, but I got an error message after I clicked sync now. The error message shows that the version of database is different than the version of Firebase and Authentication, so I changed the version of database to be the same

    //Firebase Database
    compile 'com.google.firebase:firebase-database:12.0.1' 

After successful added, the tip in the right should show successful connect
{{< figure src="firebase_database_add.jpg" >}}

Then we can go to <code>RegisterActivity</code>, add some global variable
<code>Login/RegisterActivity</code>

    //firebase
    private FirebaseAuth mAuth;
    private FirebaseMethods firebaseMethods;
    private FirebaseDatabase mFirebaseDatabase;
    private DatabaseReference myRef;

Next go to <code>setupFirebaseAuth</code> method and add following code. Note here I wasn't follow the steps from the author, because I didn't add mAuthListener in previous sing in part, some part of the code might be different that the author.
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

Next add a new package under project folder, name models, and add a java class named User.

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

Then open <code>FirebaseMethods</code> add a new method called <code>checkIfUsernameExists</code>
<code>Utils/FirebaseMethods</code>

    public boolean checkIfUsernameExists(String username, DataSnapshot datasnapshot) {
        Log.d(TAG, "checkIfUsernameExists: check if " + username + "already exists.");

        User user = new User();

        for (DataSnapshot ds: datasnapshot.getChildren()) {
            Log.d(TAG, "checkIfUsernameExists: datasnapshot: " + ds);

            user.setUsername(ds.getValue(User.class).getUsername());
        }
    }

Next go to Utils, add a new java class named StringManipulation

<code>Utils/StringManipulation</code>

    public class StringManipulation {

        public static String expandUsername(String username){
            return username.replace(".", " ");
        }

        public static String condenseUsername(String username){
            return username.replace(" " , ".");
        }

    }

Back to <code>FirebaseMethods</code> and finish <code>checkIfUsernameExists</code> Method
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

Then go to <code>RegisterActivity</code> finish the rest part of <code>setupFirebaseAuth</code>
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

in the end of video the author made a typo, the code should be

    username = username + append;

the rest part would continue