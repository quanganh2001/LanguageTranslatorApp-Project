# Create Project
Click to New Project -> Empty Activity -> Set name project: Language Translator App, package name: `com.gtappdevelopers.languagetranslatorapp`
# Setup Project
1. Setup color and theme:

File colors.xml:
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="purple_200">#0F9D58</color>
    <color name="purple_500">#0F9D58</color>
    <color name="purple_700">#0F9D58</color>
    <color name="teal_200">#0F9D58</color>
    <color name="teal_700">#FF018786</color>
    <color name="black">#FF000000</color>
    <color name="white">#FFFFFFFF</color>

    <color name="black_shade_1">#292D36</color>
    <color name="yellow">#ffa500</color>
</resources>
```
Setup theme: File themes.xml
```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Theme.LanguageTranslatorApp" parent="Theme.MaterialComponents.DayNight.DarkActionBar">
        <!-- Primary brand color. -->
        <item name="colorPrimary">@color/purple_500</item>
        <item name="colorPrimaryVariant">@color/purple_700</item>
        <item name="colorOnPrimary">@color/white</item>
        <!-- Secondary brand color. -->
        <item name="colorSecondary">@color/teal_200</item>
        <item name="colorSecondaryVariant">@color/teal_700</item>
        <item name="colorOnSecondary">@color/black</item>
        <!-- Status bar color. -->
        <item name="android:statusBarColor" tools:targetApi="l">?attr/colorPrimaryVariant</item>
        <!-- Customize your theme here. -->
    </style>
    
    <style name="AppTheme.MaterialTheme" parent="Theme.MaterialComponents.Light.DarkActionBar">
        <item name="windowNoTitle">true</item>
        <item name="windowActionBar">false</item>
        <item name="colorPrimary">@color/purple_200</item>
        <item name="colorPrimaryDark">@color/purple_200</item>
        <item name="colorAccent">@color/white</item>
        <item name="android:singleLine">true</item>
    </style>
</resources>
```
# Connect Google Firebase
1. Go to console.firebase.google.com -> Click to Add project -> set name project: Translator App
2. Click to Android logo button, then fill register app form

[![image.png](https://i.postimg.cc/GmYBzwY5/image.png)](https://postimg.cc/jDxdd1MH)

[![image.png](https://i.postimg.cc/J0kmmm1w/image.png)](https://postimg.cc/jntBQpwv)

3. Download config file (file google-service.json), copy to app folder

[![image.png](https://i.postimg.cc/hPZwsNgT/image.png)](https://postimg.cc/0bmc2Wzy)

4. Go to Tools -> Firebase, then go to Firebase ML -> Use Firebase ML to recognize text in images -> Connect your app to Firebase, then click to Add Firebase ML to your app

[![image.png](https://i.postimg.cc/zD6Kkk63/image.png)](https://postimg.cc/yWhD10Vz)

Here is build.gradle(app) file:
```txt
plugins {
    id 'com.android.application'
    id 'com.google.gms.google-services'
}

android {
    compileSdk 32

    defaultConfig {
        applicationId "com.gtappdevelopers.languagetranslatorapp"
        minSdk 21
        targetSdk 32
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {

    implementation 'androidx.appcompat:appcompat:1.4.1'
    implementation 'com.google.android.material:material:1.6.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
    implementation 'com.google.firebase:firebase-core:17.3.0'
    implementation 'com.google.firebase:firebase-ml-natural-language:22.0.0'
    implementation 'com.google.firebase:firebase-ml-natural-language-language-id-model:20.0.7'
    implementation 'com.google.firebase:firebase-ml-natural-language-translate-model:20.0.0'
    testImplementation 'junit:junit:4.+'
    androidTestImplementation 'androidx.test.ext:junit:1.1.5'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.5.1'
}
```
You need add internet and record audio permissions (file AndroidManifest.xml)
```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.RECORD_AUDIO"/>
```
# Setup layout
Right click drawable folder -> Go to New -> Vector Asset

[![image.png](https://i.postimg.cc/MHL7MCfm/image.png)](https://postimg.cc/hQ9zN3rX)

Go to mic icon, setup name file is `ic_mic.xml` and right arrow icon, setup name file is `ic_right_arrow.xml`

Here is activity_main.xml file:
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    style="@style/AppTheme.MaterialTheme"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/black_shade_1"
    android:orientation="vertical"
    tools:context=".MainActivity">


    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        android:gravity="center"
        android:padding="4dp"
        android:text="Language Translator"
        android:textAlignment="center"
        android:textColor="@color/yellow"
        android:textSize="20sp"
        android:textStyle="bold" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginStart="20dp"
        android:layout_marginTop="20dp"
        android:layout_marginEnd="20dp"
        android:orientation="horizontal"
        android:weightSum="3">

        <Spinner
            android:id="@+id/idFromSpinner"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_margin="3dp"
            android:layout_weight="1"
            android:gravity="center"
            android:padding="3dp" />

        <ImageView
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:layout_weight="1"
            android:src="@drawable/ic_right_arrow"
            app:tint="@color/white" />

        <Spinner
            android:id="@+id/idToSpinner"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_margin="3dp"
            android:layout_weight="1"
            android:gravity="center"
            android:padding="3dp" />
    </LinearLayout>

    <com.google.android.material.textfield.TextInputLayout
        style="@style/Widget.MaterialComponents.TextInputLayout.OutlinedBox"
        android:layout_width="match_parent"
        android:layout_height="100dp"
        android:layout_marginStart="20dp"
        android:layout_marginTop="10dp"
        android:layout_marginEnd="20dp"
        android:hint="Source Text"
        android:padding="5dp"
        android:textColorHint="@color/white"
        app:hintTextColor="@color/white">

        <com.google.android.material.textfield.TextInputEditText
            android:id="@+id/idEdtSource"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:ems="10"
            android:importantForAutofill="no"
            android:inputType="textImeMultiLine|textMultiLine"
            android:textColor="@color/white"
            android:textColorHint="@color/white"
            android:textSize="14sp" />

    </com.google.android.material.textfield.TextInputLayout>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_marginTop="5dp"
        android:text="OR"
        android:textAlignment="center"
        android:textAllCaps="true"
        android:textColor="@color/white"
        android:textSize="20sp" />

    <ImageView
        android:id="@+id/idIVMic"
        android:layout_width="70dp"
        android:layout_height="70dp"
        android:layout_gravity="center"
        android:layout_marginTop="10dp"
        android:src="@drawable/ic_mic"
        app:tint="@color/yellow" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:text="Say Something"
        android:textAlignment="center"
        android:textColor="@color/white"
        android:textSize="15sp" />

    <com.google.android.material.button.MaterialButton
        android:id="@+id/idBtnTranslate"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginStart="20dp"
        android:layout_marginTop="20dp"
        android:layout_marginEnd="20dp"
        android:text="Translate"
        android:textAllCaps="false" />

    <TextView
        android:id="@+id/idTVTranslatedTV"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_marginTop="30dp"
        android:text="Translated Text"
        android:textAlignment="center"
        android:textAllCaps="false"
        android:textColor="@color/yellow"
        android:textSize="18sp" />

</LinearLayout>
```

Right click layout folder -> Go to New -> Layout Resource File

[![image.png](https://i.postimg.cc/tTzg1Wwq/image.png)](https://postimg.cc/Fd7v8dh8)

Set file name: spinner_item and change root element to TextView.

![imagefc3b3e11ea4e95af.png](https://img.upanh.tv/2023/09/19/imagefc3b3e11ea4e95af.png)

Here is spinner_item.xml file:
```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:gravity="center"
    android:textAlignment="center"
    android:padding="4dp"
    android:textColor="@color/white">

</TextView>
```

Here is MainActivity.java file:
```java
package com.gtappdevelopers.languagetranslatorapp;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.speech.RecognizerIntent;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ImageView;
import android.widget.Spinner;
import android.widget.TextView;
import android.widget.Toast;

import com.google.android.gms.tasks.OnFailureListener;
import com.google.android.gms.tasks.OnSuccessListener;
import com.google.android.material.button.MaterialButton;
import com.google.android.material.textfield.TextInputEditText;
import com.google.firebase.ml.common.modeldownload.FirebaseModelDownloadConditions;
import com.google.firebase.ml.naturallanguage.FirebaseNaturalLanguage;
import com.google.firebase.ml.naturallanguage.translate.FirebaseTranslateLanguage;
import com.google.firebase.ml.naturallanguage.translate.FirebaseTranslator;
import com.google.firebase.ml.naturallanguage.translate.FirebaseTranslatorOptions;

import java.util.ArrayList;
import java.util.Locale;

public class MainActivity extends AppCompatActivity {

    private Spinner fromSpinner, toSpinner;
    private TextInputEditText sourceEdt;
    private ImageView micIV;
    private MaterialButton translateBtn;
    private TextView translatedTV;
    String[] fromLanguages = {"From", "English", "Afrikaans", "Arabic", "Belarusian", "Bulgarian", "Bengali", "Catalan", "Czech",
            "Welsh", "Hindi", "Urdu", "French", "Vietnamese"};

    String[] toLanguages = {"To", "English", "Afrikaans", "Arabic", "Belarusian", "Bulgarian", "Bengali", "Catalan", "Czech",
            "Welsh", "Hindi", "Urdu", "French", "Vietnamese"};

    private static final int REQUEST_PERMISSION_CODE = 1;
    private static final int REQUEST_RECORD_AUDIO_PERMISSION = 200;
    int languageCode, fromLanguageCode, toLanguageCode = 0;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        fromSpinner = findViewById(R.id.idFromSpinner);
        toSpinner = findViewById(R.id.idToSpinner);
        sourceEdt = findViewById(R.id.idEdtSource);
        micIV = findViewById(R.id.idIVMic);
        translateBtn = findViewById(R.id.idBtnTranslate);
        translatedTV = findViewById(R.id.idTVTranslatedTV);
        fromSpinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> adapterView, View view, int position, long id) {
                fromLanguageCode = getLanguageCode(fromLanguages[position]);
            }

            @Override
            public void onNothingSelected(AdapterView<?> parent) {

            }
        });
        ArrayAdapter fromAdapter = new ArrayAdapter(this, R.layout.spinner_item, fromLanguages);
        fromAdapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
        fromSpinner.setAdapter(fromAdapter);

        toSpinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> adapterView, View view, int position, long id) {
                toLanguageCode = getLanguageCode(toLanguages[position]);
            }

            @Override
            public void onNothingSelected(AdapterView<?> adapterView) {

            }
        });

        ArrayAdapter toAdapter = new ArrayAdapter(this, R.layout.spinner_item, toLanguages);
        toAdapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
        toSpinner.setAdapter(toAdapter);

        translateBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                translatedTV.setText("");
                if (sourceEdt.getText().toString().isEmpty()) {
                    Toast.makeText(MainActivity.this, "Please enter your text to translate", Toast.LENGTH_SHORT).show();
                } else if (fromLanguageCode == 0) {
                    Toast.makeText(MainActivity.this, "Please select source language", Toast.LENGTH_SHORT).show();
                } else if (toLanguageCode == 0) {
                    Toast.makeText(MainActivity.this, "Please select the language to make translation", Toast.LENGTH_SHORT).show();
                } else {
                    translateText(fromLanguageCode, toLanguageCode, sourceEdt.getText().toString());
                }
            }
        });

        micIV.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent i = new Intent(RecognizerIntent.ACTION_RECOGNIZE_SPEECH);
                i.putExtra(RecognizerIntent.EXTRA_LANGUAGE_MODEL, RecognizerIntent.LANGUAGE_MODEL_FREE_FORM);
                i.putExtra(RecognizerIntent.EXTRA_LANGUAGE, Locale.getDefault());
                i.putExtra(RecognizerIntent.EXTRA_PROMPT, "Speak to convert into text");
                try {
                    startActivityForResult(i, REQUEST_PERMISSION_CODE);
                } catch (Exception e) {
                    e.printStackTrace();
                    Toast.makeText(MainActivity.this, "" + e.getMessage(), Toast.LENGTH_SHORT).show();
                }
            }
        });
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == REQUEST_PERMISSION_CODE) {
            if (resultCode == RESULT_OK && data != null) {
                ArrayList<String> result = data.getStringArrayListExtra(RecognizerIntent.EXTRA_RESULTS);
                sourceEdt.setText(result.get(0));
            }
        }
    }

    private void translateText(int fromLanguageCode, int toLanguageCode, String source) {
        translatedTV.setText("Downloading Model...");
        FirebaseTranslatorOptions options = new FirebaseTranslatorOptions.Builder()
                .setSourceLanguage(fromLanguageCode)
                .setTargetLanguage(toLanguageCode)
                .build();

        FirebaseTranslator translator = FirebaseNaturalLanguage.getInstance().getTranslator(options);

        FirebaseModelDownloadConditions conditions = new FirebaseModelDownloadConditions.Builder().build();

        translator.downloadModelIfNeeded(conditions).addOnSuccessListener(new OnSuccessListener<Void>() {
            @Override
            public void onSuccess(Void unused) {
                translatedTV.setText("Translating...");
                translator.translate(source).addOnSuccessListener(new OnSuccessListener<String>() {
                    @Override
                    public void onSuccess(String s) {
                        translatedTV.setText(s);
                    }
                }).addOnFailureListener(new OnFailureListener() {
                    @Override
                    public void onFailure(@NonNull Exception e) {
                        Toast.makeText(MainActivity.this, "Fail to translate : "+e.getMessage(), Toast.LENGTH_SHORT).show();
                    }
                });
            }
        }).addOnFailureListener(new OnFailureListener() {
            @Override
            public void onFailure(@NonNull Exception e) {
                Toast.makeText(MainActivity.this, "Fail to download language Model "+e.getMessage(), Toast.LENGTH_SHORT).show();
            }
        });
    }

    public int getLanguageCode(String language) {
        int languageCode = 0;
        switch (language) {
            case "English":
                languageCode = FirebaseTranslateLanguage.EN;
                break;
            case "Afrikaans":
                languageCode = FirebaseTranslateLanguage.AF;
                break;
            case "Belarusian":
                languageCode = FirebaseTranslateLanguage.BE;
                break;
            case "Bengali":
                languageCode = FirebaseTranslateLanguage.BN;
                break;
            case "Catalan":
                languageCode = FirebaseTranslateLanguage.CA;
                break;
            case "Czech":
                languageCode = FirebaseTranslateLanguage.CS;
                break;
            case "Welsh":
                languageCode = FirebaseTranslateLanguage.CY;
                break;
            case "Hindi":
                languageCode = FirebaseTranslateLanguage.HI;
                break;
            case "Urdu":
                languageCode = FirebaseTranslateLanguage.UR;
                break;
            case "French":
                languageCode = FirebaseTranslateLanguage.FR;
                break;
            case "Vietnamese":
                languageCode = FirebaseTranslateLanguage.VI;
                break;
            default:
                languageCode = 0;
        }
        return languageCode;
    }
}
```