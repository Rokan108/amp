1. Create a simple "Hello World" application in Android Studio. Explain the purpose of the MainActivity and the activity_main.xml file.

== MainActivity.kt ==

package com.example.practical1 //Add the File name of the project

import android.os.Bundle
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_main)
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
    }
}

== activity_main.xml == 

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView                                             //Copy from here
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.5" />       //to Here

</androidx.constraintlayout.widget.ConstraintLayout>

---------------------------------------------------------------------------------------------

2. Create a login screen with EditText fields for username and password, and a Button to submit. Include validation for empty fields.

== MainActivity.kt == 

package com.example.pract2         //folder name of the project

import android.os.Bundle
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import android.widget.Button
import android.widget.EditText
import android.widget.Toast

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_main)
        val username = findViewById<EditText>(R.id.etUsername)
        val password = findViewById<EditText>(R.id.etPassword)
        val loginbtn = findViewById<Button>(R.id.btnLogin)

        loginbtn.setOnClickListener {
            val usernameText = username.text.toString()
            val passwordText = password.text.toString()

            if (usernameText.isEmpty() || passwordText.isEmpty()) {
                Toast.makeText(this, getString(R.string.error_empty_fields), Toast.LENGTH_SHORT).show()
            } else {
                Toast.makeText(this, getString(R.string.login_successful), Toast.LENGTH_SHORT).show()
            }
        }
    }
}


== activity_main.xml ==

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <EditText                                       //copy from here
        android:id="@+id/etUsername"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginHorizontal="24dp"
        android:layout_marginTop="60dp"
        android:hint="@string/hint_username"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <EditText
        android:id="@+id/etPassword"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginHorizontal="24dp"
        android:layout_marginTop="16dp"
        android:hint="@string/hint_password"
        android:inputType="textPassword"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/etUsername" />

    <Button
        android:id="@+id/btnLogin"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginHorizontal="24dp"
        android:layout_marginTop="24dp"
        android:text="@string/btn_login"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/etPassword" />          //to Here

</androidx.constraintlayout.widget.ConstraintLayout>


--------------------------------------------------------------------------------------------------------------------------

3. Create an android application to display Alert Dialog on pressing the Back button.

== MainActivity ==

package com.example.practicle3                 

import android.os.Bundle
import androidx.activity.OnBackPressedCallback
import androidx.appcompat.app.AlertDialog
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val callback = object : OnBackPressedCallback(true) {
            override fun handleOnBackPressed() {

                AlertDialog.Builder(this@MainActivity)
                    .setTitle("Exit Application")
                    .setMessage("Are you sure you want to exit?")
                    .setPositiveButton("Yes") { _, _ ->
                        finish()
                    }
                    .setNegativeButton("No") { dialog, _ ->
                        dialog.dismiss()
                    }
                    .show()
            }
        }

        onBackPressedDispatcher.addCallback(this, callback)
    }
}


== activity_main.xml ==

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Press Back Button"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>


---------------------------------------------------------------------------------------------------------------------------------

4. Create an android application which automatically notify the user when Aeroplane mode is turned on or off using broadcast receiver.

== MainActivity.kt ==

package com.example.practical4

import android.content.Intent
import android.content.IntentFilter
import android.os.Bundle
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat

class MainActivity : AppCompatActivity() {

    private lateinit var airplaneModeReceiver: AirplaneModeReceiver

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_main)
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }

        airplaneModeReceiver = AirplaneModeReceiver()
    }

    override fun onStart() {
        super.onStart()
        val filter = IntentFilter(Intent.ACTION_AIRPLANE_MODE_CHANGED)
        registerReceiver(airplaneModeReceiver, filter)
    }

    override fun onStop() {
        super.onStop()
        unregisterReceiver(airplaneModeReceiver)
    }
}

== activity_main.xml ==

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Toggle Airplane Mode to See Notification"
        android:textSize="20sp"
        android:textStyle="bold"
        android:textAlignment="center"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>


</androidx.constraintlayout.widget.ConstraintLayout>

== AndroidManifest.xml == 

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Practical4">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <receiver                                                    //copy from here
            android:name=".AirplaneModeReceiver"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.AIRPLANE_MODE"/>
            </intent-filter>
        </receiver>                                               // to here
    </application>

</manifest>

== AirplaneModeReceiver ==

package com.example.practical4

import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.widget.Toast

class AirplaneModeReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        val isAirplaneModeOn = intent.getBooleanExtra("state", false)
        if (isAirplaneModeOn) {
            Toast.makeText(context, "Airplane Mode ON", Toast.LENGTH_LONG).show()
        } else {
            Toast.makeText(context, "Airplane Mode OFF", Toast.LENGTH_LONG).show()
        }
    }
}

-------------------------------------------------------------------------------------------------------------------------------------------------------

5. Explain the Android Activity life cycle in detail. Implement a simple app that logs the lifecycle methods (onCreate(), onStart(), onResume(), onPause(), onStop(), onDestroy()).

== MainActivity.kt ==

package com.example.practical5

import android.os.Bundle
import android.util.Log
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    companion object {
        private const val TAG = "LifecycleEvents"
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        Log.d(TAG, "onCreate: Activity Created")
    }

    override fun onStart() {
        super.onStart()
        Log.d(TAG, "onStart: Activity Visible")
    }

    override fun onResume() {
        super.onResume()
        Log.d(TAG, "onResume: Activity Interactive")
    }

    override fun onPause() {
        super.onPause()
        Log.d(TAG, "onPause: Activity Paused")
    }

    override fun onStop() {
        super.onStop()
        Log.d(TAG, "onStop: Activity Stopped")
    }

    override fun onRestart() {
        super.onRestart()
        Log.d(TAG, "onRestart: Activity Restarted")
    }

    override fun onDestroy() {
        super.onDestroy()
        Log.d(TAG, "onDestroy: Activity Destroyed")
    }
}


== activity_main.xml ==

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

6. Insert the new contents in the following resources and demonstrate their uses in the android application Android Resources: (Color, Theme, String, Drawable, Dimension, Image)

== activity_main.xml ==

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="120dp"
        android:layout_height="120dp"
        android:src="@drawable/ic_launcher_background"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/welcome_message"
        android:textColor="@color/textColor"
        android:textSize="@dimen/text_size"
        app:layout_constraintTop_toBottomOf="@id/imageView"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Click Me"
        android:background="@drawable/button_background"
        android:padding="@dimen/padding"
        app:layout_constraintTop_toBottomOf="@id/textView"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>

== themes.xml ==

<resources>
    <style name="Theme.ResourceDemo" parent="Theme.Material3.DayNight.NoActionBar">
        <item name="colorPrimary">@color/primaryColor</item>
    </style>
</resources>


== strings.xml ==

<resources>
    <string name="app_name">ResourceDemo</string>
    <string name="welcome_message">Welcome to Android Resources Demo</string>
</resources>

== colors.xml ==

<resources>
    <color name="primaryColor">#6200EE</color>
    <color name="textColor">#000000</color>
</resources>

== AndroidManifest.xml == 

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"                   //add this images in drawable(right click -> New -> Image asset)
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.ResourceDemo">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>

------------------------------------------------------------------------------------------------------------------------------------------------------------------

7. Define the following string resources in the strings.xml file for the registration form:
• "Full Name", "Email", "Password", "Confirm Password", and "Submit".
• Use these string resources in the corresponding TextView and Button in
your layout XML.

== MainActiivity.kt ==

package com.example.practical7

import android.os.Bundle
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_main)
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
    }
}

== activity_main.xml ==

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/tvFullName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/full_name"
        android:textSize="18sp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"/>

    <EditText
        android:id="@+id/etFullName"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="@string/full_name"
        android:inputType="textPersonName"
        android:autofillHints="name"
        app:layout_constraintTop_toBottomOf="@id/tvFullName"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <TextView
        android:id="@+id/tvEmail"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/email"
        app:layout_constraintTop_toBottomOf="@id/etFullName"
        app:layout_constraintStart_toStartOf="parent"
        android:layout_marginTop="16dp"/>

    <EditText
        android:id="@+id/etEmail"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="@string/email"
        android:inputType="textEmailAddress"
        android:autofillHints="emailAddress"
        app:layout_constraintTop_toBottomOf="@id/tvEmail"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <TextView
        android:id="@+id/tvPassword"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/password"
        app:layout_constraintTop_toBottomOf="@id/etEmail"
        app:layout_constraintStart_toStartOf="parent"
        android:layout_marginTop="16dp"/>

    <EditText
        android:id="@+id/etPassword"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="@string/password"
        android:inputType="textPassword"
        android:autofillHints="password"
        app:layout_constraintTop_toBottomOf="@id/tvPassword"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <TextView
        android:id="@+id/tvConfirmPassword"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/confirm_password"
        app:layout_constraintTop_toBottomOf="@id/etPassword"
        app:layout_constraintStart_toStartOf="parent"
        android:layout_marginTop="16dp"/>

    <EditText
        android:id="@+id/etConfirmPassword"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="@string/confirm_password"
        android:inputType="textPassword"
        android:autofillHints="password"
        app:layout_constraintTop_toBottomOf="@id/tvConfirmPassword"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <Button
        android:id="@+id/btnSubmit"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/submit"
        app:layout_constraintTop_toBottomOf="@id/etConfirmPassword"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="24dp"/>

</androidx.constraintlayout.widget.ConstraintLayout>

== strings.xml ==

<resources>
    <string name="app_name">Practical7</string>

    <string name="full_name">Full Name</string>          //copy from here
    <string name="email">Email</string>
    <string name="password">Password</string>
    <string name="confirm_password">Confirm Password</string>
    <string name="submit">Submit</string>                       // to here
</resources>

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

8. Design a form with a RadioGroup for selecting the gender (Male/Female/Other). Include a Submit button that shows the selected gender in a TextView.

== MainActivity.kt ==

package com.example.practical8

import android.os.Bundle
import android.widget.Button
import android.widget.RadioGroup
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val genderGroup = findViewById<RadioGroup>(R.id.genderGroup)
        val submitBtn = findViewById<Button>(R.id.btnSubmit)
        val resultText = findViewById<TextView>(R.id.resultText)

        submitBtn.setOnClickListener {

            val selectedId = genderGroup.checkedRadioButtonId

            if (selectedId != -1) {
                val selectedRadio = findViewById<android.widget.RadioButton>(selectedId)
                val gender = selectedRadio.text.toString()

                resultText.text = "Selected Gender: $gender"
            } else {
                resultText.text = "Please select a gender"
            }
        }
    }
}

== activity_main.xml ==

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/titleText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Select Gender"
        android:textSize="22sp"
        android:textStyle="bold"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <RadioGroup
        android:id="@+id/genderGroup"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:layout_constraintTop_toBottomOf="@id/titleText"
        app:layout_constraintStart_toStartOf="parent"
        android:layout_marginTop="20dp">

        <RadioButton
            android:id="@+id/radioMale"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Male"/>

        <RadioButton
            android:id="@+id/radioFemale"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Female"/>

        <RadioButton
            android:id="@+id/radioOther"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Other"/>

    </RadioGroup>

    <Button
        android:id="@+id/btnSubmit"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Submit"
        app:layout_constraintTop_toBottomOf="@id/genderGroup"
        app:layout_constraintStart_toStartOf="parent"
        android:layout_marginTop="20dp"/>

    <TextView
        android:id="@+id/resultText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Selected Gender:"
        android:textSize="18sp"
        app:layout_constraintTop_toBottomOf="@id/btnSubmit"
        app:layout_constraintStart_toStartOf="parent"
        android:layout_marginTop="20dp"/>

</androidx.constraintlayout.widget.ConstraintLayout>

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

9. Create an android application to demonstrate the use of sub menu the toast should be appeared by selecting the sub menu item

== MainActivity.kt ==

package com.example.practical9

import android.os.Bundle
import android.view.Menu
import android.view.MenuItem
import android.view.SubMenu
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import com.google.android.material.appbar.MaterialToolbar

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val toolbar: MaterialToolbar = findViewById(R.id.toolbar)
        setSupportActionBar(toolbar)
    }

    override fun onCreateOptionsMenu(menu: Menu): Boolean {
        // Create a sub-menu
        val subMenu: SubMenu = menu.addSubMenu(0, 0, 0, "Colors")

        // Add items to the sub-menu
        subMenu.add(0, 1, 0, "Red")
        subMenu.add(0, 2, 0, "Green")
        subMenu.add(0, 3, 0, "Blue")

        return true
    }

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        // Handle item selection
        when (item.itemId) {
            1 -> {
                Toast.makeText(this, "Red Selected", Toast.LENGTH_SHORT).show()
                return true
            }
            2 -> {
                Toast.makeText(this, "Green Selected", Toast.LENGTH_SHORT).show()
                return true
            }
            3 -> {
                Toast.makeText(this, "Blue Selected", Toast.LENGTH_SHORT).show()
                return true
            }
        }
        return super.onOptionsItemSelected(item)
    }
}

== activity_main.xml ==

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <com.google.android.material.appbar.MaterialToolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:layout_marginTop="40dp"
        android:background="?attr/colorPrimary"
        app:title="Practical 9"
        app:titleTextColor="@android:color/white"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Select Menu Option"
        android:textSize="22sp"
        android:textStyle="bold"
        app:layout_constraintTop_toBottomOf="@id/toolbar"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

10. Create a RelativeLayout for a profile screen with:
• A Profile Image centered at the top of the screen.
• A TextView displaying the name below the profile image, aligned to the
center.
• A Button below the name, aligned to the center.


== MainActivity.kt ==

package com.example.practical10

import android.os.Bundle
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_main)
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
    }
}

== activity_main.xml ==

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <RelativeLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:padding="20dp">

        <ImageView
            android:id="@+id/profileImage"
            android:layout_width="150dp"
            android:layout_height="150dp"
            android:src="@mipmap/ic_launcher"
            android:layout_centerHorizontal="true"
            android:layout_marginTop="40dp"
            android:contentDescription="Profile Image"/>        //add this image 

        <TextView
            android:id="@+id/profileName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="John Doe"
            android:textSize="22sp"
            android:textStyle="bold"
            android:layout_below="@id/profileImage"
            android:layout_centerHorizontal="true"
            android:layout_marginTop="20dp"/>

        <Button
            android:id="@+id/editButton"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Edit Profile"
            android:layout_below="@id/profileName"
            android:layout_centerHorizontal="true"
            android:layout_marginTop="20dp"/>

    </RelativeLayout>

</androidx.constraintlayout.widget.ConstraintLayout>


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

11. Design a ListView that displays a list of items. Each item should be a simple TextView with a string from an array of names. The list should be populated dynamically in your Activity.

== MainActivity.kt ==

package com.example.pract11

import android.os.Bundle
import android.widget.ArrayAdapter
import android.widget.ListView
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    private var listView: ListView? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        listView = findViewById(R.id.listViewNames)

        // Array of names
        val names = arrayOf(
            "Rahul",
            "Anita",
            "Karan",
            "Priya",
            "Amit",
            "Neha",
            "Ravi",
            "Sneha",
            "Rahul",
            "Anita",
            "Karan",
            "Priya",
            "Amit",
            "Neha",
            "Ravi",
            "Sneha",
            "Rahul",
            "Anita",
            "Karan",
            "Priya",
            "Amit",
            "Neha",
            "Ravi",
            "Sneha"
        )

        // Adapter to connect array with ListView
        // Use android.R.layout.simple_list_item_1 for the default Android list item layout
        val adapter = ArrayAdapter(
            this,
            android.R.layout.simple_list_item_1,
            names
        )

        // Set adapter to ListView using property access
        listView?.adapter = adapter
    }
}


== activity_main.xml ==

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <ListView
            android:id="@+id/listViewNames"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>

----------------------------------------------------------------------------------------------------------------------------------------------------------------------

12. Create an Activity that hosts a Fragment. When a button in the Activity is clicked, pass a string value to the Fragment and display it in a TextView.

== MainActivity.kt ==

package com.example.practical12

import android.os.Bundle
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import android.widget.Button

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_main)
        val btnSend = findViewById<Button>(R.id.btnSend)

        btnSend.setOnClickListener {

            val message = "Hello from Akash"

            val fragment = MyFragment()

            val bundle = Bundle()
            bundle.putString("msg", message)

            fragment.arguments = bundle

            supportFragmentManager.beginTransaction()
                .replace(R.id.fragment_container, fragment)
                .commit()
        }
    }
}

== activity_main.xml ==

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center_horizontal"
        android:orientation="vertical"
        android:padding="16dp">

        <Button
            android:id="@+id/btnSend"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="50dp"
            android:text="Send Message" />

        <FrameLayout
            android:id="@+id/fragment_container"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="20dp" />

    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>

== MyFragment.kt ==

package com.example.practical12

import android.os.Bundle
import androidx.fragment.app.Fragment
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
class MyFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {

        val view = inflater.inflate(R.layout.fragment_my, container, false)

        val txtMessage = view.findViewById<TextView>(R.id.txtMessage)

        val message = arguments?.getString("msg")

        txtMessage.text = message

        return view
    }
}

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

13. Create an android application using linear layout and insert 10 animals in the list view and display the appropriate Toast.

== MainActivity.kt == 

package com.example.practicle13

import android.os.Bundle
import android.widget.ArrayAdapter
import android.widget.ListView
import android.widget.Toast
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_main)
        
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }

        val listView = findViewById<ListView>(R.id.listViewAnimals)

        val animals = arrayOf(
            "Lion",
            "Tiger",
            "Elephant",
            "Dog",
            "Cat",
            "Horse",
            "Monkey",
            "Deer",
            "Zebra",
            "Giraffe",
            "Prince"
        )

        val adapter = ArrayAdapter(
            this,
            android.R.layout.simple_list_item_1,
            animals
        )

        listView.adapter = adapter

        listView.setOnItemClickListener { _, _, position, _ ->
            val selectedAnimal = animals[position]

            Toast.makeText(
                this,
                "Selected Animal: $selectedAnimal",
                Toast.LENGTH_SHORT
            ).show()
        }
    }
}


== activity_main.xml ==

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <ListView
            android:id="@+id/listViewAnimals"
            android:layout_width="match_parent"
            android:layout_height="match_parent"/>

    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


14. Create an android application to demonstrate the Grid view and display the images using GridLayout.

== MainActivity.kt ==

package com.example.practical14

import android.os.Bundle
import android.widget.GridView
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    private val images = arrayOf(
        R.mipmap.img1,                     //add this image through the drawable folder
        R.mipmap.img1, 
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1,
        R.mipmap.img1
    )

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_main)
        
        val gridView = findViewById<GridView>(R.id.gridView)
        val adapter = ImageAdapter(this, images)
        gridView.adapter = adapter
    }
}


== activity_main.xml == 

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <GridView
            android:id="@+id/gridView"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:numColumns="3"
            android:verticalSpacing="10dp"
            android:horizontalSpacing="10dp"/>

    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>


== ImageAdapter.kt ==

package com.example.practical14

import android.content.Context
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.BaseAdapter
import android.widget.ImageView

class ImageAdapter(private val context: Context, private val images: Array<Int>) : BaseAdapter() {

    override fun getCount(): Int {
        return images.size
    }

    override fun getItem(position: Int): Any {
        return images[position]
    }

    override fun getItemId(position: Int): Long {
        return position.toLong()
    }

    override fun getView(position: Int, convertView: View?, parent: ViewGroup?): View {
        val view = convertView ?: LayoutInflater.from(context).inflate(R.layout.fragment_image_adapter, parent, false)
        val imageView = view.findViewById<ImageView>(R.id.itemImageView)
        imageView.setImageResource(images[position])
        return view
    }
}


-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

15. Create a Registration Form with the following fields:
• Username (Text Input)
• Email Address (Text Input)
• Phone Number (Text Input)
• Password (Password Input)
Submit Button (Floating Action Button)

== MainActivity.kt ==

package com.example.practical15

import android.os.Bundle
import android.widget.EditText
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import com.google.android.material.floatingactionbutton.FloatingActionButton

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        setContentView(R.layout.activity_main)
        val username = findViewById<EditText>(R.id.etUsername)
        val email = findViewById<EditText>(R.id.etEmail)
        val phone = findViewById<EditText>(R.id.etPhone)
        val password = findViewById<EditText>(R.id.etPassword)
        val fabSubmit = findViewById<FloatingActionButton>(R.id.fabSubmit)

        fabSubmit.setOnClickListener {

            val user = username.text.toString()
            val mail = email.text.toString()
            val ph = phone.text.toString()

            Toast.makeText(
                this,
                "Registered: $user",
                Toast.LENGTH_SHORT
            ).show()
        }
    }
}


== activity_main.xml ==

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:orientation="vertical"
        android:padding="20dp"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <EditText
            android:id="@+id/etUsername"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Username"
            android:autofillHints="username"
            android:inputType="text"/>

        <EditText
            android:id="@+id/etEmail"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Email Address"
            android:autofillHints="emailAddress"
            android:inputType="textEmailAddress"/>

        <EditText
            android:id="@+id/etPhone"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Phone Number"
            android:autofillHints="phone"
            android:inputType="phone"/>

        <EditText
            android:id="@+id/etPassword"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Password"
            android:autofillHints="password"
            android:inputType="textPassword"/>

        <com.google.android.material.floatingactionbutton.FloatingActionButton
            android:id="@+id/fabSubmit"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="20dp"
            android:src="@android:drawable/ic_menu_send"
            android:contentDescription="Submit"
            app:backgroundTint="@color/purple_500"/>

    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

16. Create an app that listens for ACTION_WIFI_MODE_CHANGED using a BroadcastReceiver. Display a Toast message whenever the Wifi mode is turned on or off.

== MainActivity.kt ==

package com.example.practicle16

import android.content.Context
import android.content.IntentFilter
import android.net.wifi.WifiManager
import android.os.Build
import android.os.Bundle
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat

class MainActivity : AppCompatActivity() {
    private val wifiReceiver = WifiReceiver()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_main)
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
    }

    override fun onStart() {
        super.onStart()
        val intentFilter = IntentFilter(WifiManager.WIFI_STATE_CHANGED_ACTION)
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {
            registerReceiver(wifiReceiver, intentFilter, Context.RECEIVER_EXPORTED)
        } else {
            registerReceiver(wifiReceiver, intentFilter)
        }
    }

    override fun onStop() {
        super.onStop()
        unregisterReceiver(wifiReceiver)
    }
}

== WifiReceiver.kt ==

package com.example.practicle16

import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.net.wifi.WifiManager
import android.util.Log
import android.widget.Toast

class WifiReceiver : BroadcastReceiver() {

    override fun onReceive(context: Context, intent: Intent) {
        if (intent.action == WifiManager.WIFI_STATE_CHANGED_ACTION) {
            val wifiState = intent.getIntExtra(
                WifiManager.EXTRA_WIFI_STATE,
                WifiManager.WIFI_STATE_UNKNOWN
            )

            Log.d("WifiReceiver", "WiFi State Changed: $wifiState")

            when (wifiState) {
                WifiManager.WIFI_STATE_ENABLED ->
                    Toast.makeText(context, "WiFi is ON", Toast.LENGTH_SHORT).show()

                WifiManager.WIFI_STATE_DISABLED ->
                    Toast.makeText(context, "WiFi is OFF", Toast.LENGTH_SHORT).show()

                WifiManager.WIFI_STATE_ENABLING ->
                    Toast.makeText(context, "WiFi is Turning ON...", Toast.LENGTH_SHORT).show()

                WifiManager.WIFI_STATE_DISABLING ->
                    Toast.makeText(context, "WiFi is Turning OFF...", Toast.LENGTH_SHORT).show()
            }
        }
    }
}

== AndroidManifest.xml == 

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Practicle16">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <receiver                                                         //copy from here
            android:name=".WifiReceiver"
            android:exported="true">
            <intent-filter>
                <action android:name="android.net.wifi.WIFI_STATE_CHANGED" />
            </intent-filter>
        </receiver>                                                        //to here
    </application>

</manifest>


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

17. Create an android application to make use of Frame Layout –

• Display the text with name
• Set the image in the frame
• Add the image to the center.

== activity_main.xml ==

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:id="@+id/txtName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Grak The Army Commander"
            android:textSize="24sp"
            android:layout_gravity="top|center_horizontal"
            android:padding="20dp"/>

        <ImageView
            android:id="@+id/imageView"
            android:layout_width="200dp"
            android:layout_height="200dp"
            android:src="@mipmap/image1"             //add image from drawable folder
            android:layout_gravity="center"/>

    </FrameLayout>

</androidx.constraintlayout.widget.ConstraintLayout>



===================================================== END ===================================================









