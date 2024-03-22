activity_main.xml (insert)

<?xml version="1.0" encoding="utf-8"?>
<androidx.appcompat.widget.LinearLayoutCompat xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:gravity="center"
    android:orientation="vertical"
    android:padding="20dp">

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Name"
        android:id="@+id/reg_name"
        android:padding="20dp"/>

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Email"
        android:id="@+id/reg_email"
        android:padding="20dp"/>

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Password"
        android:id="@+id/reg_password"
        android:padding="20dp"/>

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Gender"
        android:id="@+id/reg_gender"
        android:padding="20dp"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Register"
        android:onClick="registerUser"/>

</androidx.appcompat.widget.LinearLayoutCompat>

================================================================================

MainActivity.java


package com.example.crud;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.EditText;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    EditText reg_name,reg_email,reg_password,reg_gender;
    DbHelper dbhelper;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        reg_name = findViewById(R.id.reg_name);
        reg_email = findViewById(R.id.reg_email);
        reg_password = findViewById(R.id.reg_password);
        reg_gender = findViewById(R.id.reg_gender);

        dbhelper = new DbHelper(getApplicationContext());


    }

    public void registerUser(View view)
    {
        String name = reg_name.getText().toString();
        String email = reg_email.getText().toString();
        String password = reg_password.getText().toString();
        String gender = reg_gender.getText().toString();

        dbhelper.registerUser(name,email,password,gender);
        Toast.makeText(this,"User Registered Successfully",Toast.LENGTH_SHORT).show();

        // For set text as empty whenever user is register

        reg_name.setText("");
        reg_email.setText("");
        reg_password.setText("");
        reg_gender.setText("");
    }
}



==============================================================

DbHelper.java

package com.example.crud;

import android.content.ContentValues;
import android.content.Context;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

import androidx.annotation.Nullable;

public class DbHelper extends SQLiteOpenHelper {

    private static final String Database_name = "demo_db";
    private static final int Database_version = 1;


    public DbHelper(@Nullable Context context) {
        super(context, Database_name, null,Database_version );
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        String create_table = "create table register(id integer primary key autoincrement,name text,email text,password text,gender text)";
        db.execSQL(create_table);

    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("drop table if exists register");
        onCreate(db);
    }



    public void registerUser(String name, String email, String password, String gender) {
        SQLiteDatabase sqldb = this.getWritableDatabase();
        ContentValues cv = new ContentValues();
        cv.put("Name",name);
        cv.put("Email",email);
        cv.put("Password",password);
        cv.put("Gender",gender);
        sqldb.insert("register",null,cv);

    }
}

=================================================================================================================


package com.example.crud;

import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

public class DbHelper extends SQLiteOpenHelper {

    private static final String Database_name = "demo_db";
    private static final int Database_version = 1;

    public DbHelper(Context context) {
        super(context, Database_name, null, Database_version);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        String create_table = "create table register(id integer primary key autoincrement,name text,email text,password text,gender text)";
        db.execSQL(create_table);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("DROP TABLE IF EXISTS register");
        onCreate(db);
    }

    public void registerUser(String name, String email, String password, String gender) {
        SQLiteDatabase sqldb = this.getWritableDatabase();
        ContentValues cv = new ContentValues();
        cv.put("name", name);
        cv.put("email", email);
        cv.put("password", password);
        cv.put("gender", gender);
        sqldb.insert("register", null, cv);
        sqldb.close();
    }

    public void deleteUser(int userId) {
        SQLiteDatabase sqldb = this.getWritableDatabase();
        sqldb.delete("register", "id=?", new String[]{String.valueOf(userId)});
        sqldb.close();
    }

    public void updateUser(int userId, String name, String email, String password, String gender) {
        SQLiteDatabase sqldb = this.getWritableDatabase();
        ContentValues cv = new ContentValues();
        cv.put("name", name);
        cv.put("email", email);
        cv.put("password", password);
        cv.put("gender", gender);
        sqldb.update("register", cv, "id=?", new String[]{String.valueOf(userId)});
        sqldb.close();
    }

    public Cursor getUsers() {
        SQLiteDatabase sqldb = this.getReadableDatabase();
        return sqldb.query("register", null, null, null, null, null, null);
    }
}
