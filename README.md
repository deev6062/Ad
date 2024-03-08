# Ad

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/txtName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:ems="10"
        android:hint="Enter Name"
        android:inputType="textPersonName"
        android:minHeight="48dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.497"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.121" />

    <Spinner
        android:id="@+id/spCity"
        android:layout_width="304dp"
        android:layout_height="wrap_content"
        android:minHeight="48dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.429"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.226" />

    <RadioGroup
        android:id="@+id/rgGender"
        android:layout_width="414dp"
        android:layout_height="67dp"
        android:layout_gravity="center"
        android:gravity="center"
        android:orientation="horizontal"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.177"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.328">

        <RadioButton
            android:id="@+id/rbMale"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Male" />

        <RadioButton
            android:id="@+id/rbFemale"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Female" />

    </RadioGroup>

    <CheckBox
        android:id="@+id/ckDance"
        android:layout_width="101dp"
        android:layout_height="51dp"
        android:layout_gravity="center"
        android:gravity="center"
        android:text="Dance"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.051"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.45" />

    <CheckBox
        android:id="@+id/ckShare"
        android:layout_width="122dp"
        android:layout_height="49dp"
        android:layout_gravity="center"
        android:gravity="center"
        android:text="Share"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.453" />

    <CheckBox
        android:id="@+id/ckSing"
        android:layout_width="104dp"
        android:layout_height="48dp"
        android:layout_gravity="center"
        android:gravity="center"
        android:text="Sing"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.905"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.452" />

    <Button
        android:id="@+id/btnRegister"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Register"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.464"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.598" />

</androidx.constraintlayout.widget.ConstraintLayout>

==================================================================================================================



package com.example.t2;

import androidx.appcompat.app.AlertDialog;
import androidx.appcompat.app.AppCompatActivity;

import android.content.DialogInterface;
import android.os.Bundle;
import android.view.View;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.EditText;
import android.widget.RadioButton;
import android.widget.RadioGroup;
import android.widget.Spinner;

public class MainActivity extends AppCompatActivity {
    EditText txtName;
    Spinner spCity;
    RadioGroup rgGender;
    RadioButton rbMale,rbFemale;
    CheckBox ckSing,ckDance,ckShare;
    Button btnRegister;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ControlInitialization();
        ButtonClicks();
    }
    private void ButtonClicks(){
        btnRegister.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                String Name = txtName.getText().toString();
                String Gender="";
                if(rbMale.isChecked()){
                    Gender=rbMale.getText().toString();
                }else
                    Gender=rbFemale.getText().toString();

                String City=spCity.getSelectedItem().toString();
                String hobby="";
                if(ckDance.isChecked()){
                    hobby=ckDance.getText().toString();
                }
                if(ckShare.isChecked()){
                    hobby=ckShare.getText().toString();
                }
                if(ckShare.isChecked()){
                    hobby=ckShare.getText().toString();

                }
                AlertDialog.Builder builder = new AlertDialog.Builder(MainActivity.this);
                builder.setTitle("Alert Dialog Example");
                builder.setMessage("Name : "+Name +"\n"+
                        "City : "+ City+"\n"+
                        "Gender :" + Gender +"\n"+
                        "Hobby : " + hobby +"\n"
                );
                builder.setCancelable(false);
                builder.setPositiveButton("YES", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        finish();
                    }
                });
                builder.setNegativeButton("NO", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        dialog.cancel();
                    }
                });

                AlertDialog dialog = builder.create();
                dialog.show();
            };

        });
    }


    private void ControlInitialization(){
        txtName=findViewById(R.id.txtName);
        spCity=findViewById(R.id.spCity);
        rgGender=findViewById(R.id.rgGender);
        rbMale=findViewById(R.id.rbMale);
        rbFemale=findViewById(R.id.rbFemale);
        ckDance=findViewById(R.id.ckDance);
        ckSing=findViewById(R.id.ckSing);
        ckShare=findViewById(R.id.ckShare);
        btnRegister=findViewById(R.id.btnRegister);

        ArrayAdapter<String> adapter = new ArrayAdapter<String>(getApplicationContext(),
                android.R.layout.simple_list_item_1,getResources().getStringArray(R.array.spCity));
        spCity.setAdapter(adapter);
    }

}


==================================================================================================================

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">


    <androidx.appcompat.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:background="?attr/colorPrimary"
        android:minHeight="?attr/actionBarSize"
        android:theme="?attr/actionBarTheme"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>

==================================================================================================================


package com.example.test;

import androidx.appcompat.app.AppCompatActivity;


import android.os.Bundle;
import android.view.Menu;
import android.view.MenuInflater;
import android.widget.Toolbar;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Toolbar toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
    }

    private void setSupportActionBar(Toolbar toolbar) {
    }

    public boolean onCreateOptionMenu(Menu menu)
    {
        MenuInflater mi = new MenuInflater(this);
        mi.inflate(R.menu.options_menu, menu);
        return true;


    }
}

==================================================================================================================

option_menu.xml

<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
<item
    android:title="Profile"
    android:id="@+id/profile"
    />

    <item
        android:title="Setting"
        android:id="@+id/setting"
        />

    <item
        android:title="More Options"
        android:id="@+id/options"
        />

    <item
        android:title="Exit"
        android:id="@+id/exit"
        />
</menu>
