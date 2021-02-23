# VIMOS ANDROID SDK
![version](https://img.shields.io/badge/version-v1.0.1-blue)

The Vimos Android SDK is a realtime Video KYC solution for Android in **Kotlin**

Find the changelog and release history [Here](CHANGELOG.md)

# Table Of Content

- [Prerequisite](#prerequisite)
- [Android SDK Requirements](#android-sdk-requirements)
- [Download](#download)
  - [Using maven repository](#using-maven-repository)
- [Setup](#setup)
  - [Permissions](#permissions)
- [Quick Start](#quick-start)
  - [Invoking the Vimos SDK](#invoking-the-vimos-sdk)
- [Vimos SDK Result](#vimos-sdk-result)
- [Vimos SDK Error Codes](#vimos-sdk-error-codes)
- [Vimos SDK Parameters](#vimos-sdk-parameters)
- [Help](#help)

## Prerequisite

You will need a valid license to use the Vimos Android SDK, which can be obtained by contacting `support@frslabs.com` . 

Once you have the license , follow the below instructions for a successful integration of Vimos Android SDK onto your Android Application.

## Android SDK Requirements

**Minimum SDK Version** -  **21**

## Download

#### Using maven repository

Add the following code to your `project` level `build.gradle` file

```groovy
allprojects { 
    repositories { 
       maven { 
            // Maven Url and Credentials for Vimos SDK. 
            url "https://vimos-android.repo.frslabs.space/"                  
            credentials { 
                   username 'repo-username' 
                   password 'repo-password' 
            }
       }
        
    }
}
```

After that, add the following code to your `app` level `build.gradle` file

```groovy
// ...

  compileOptions {
       sourceCompatibility = 1.8
       targetCompatibility = 1.8
  }

// ...
```

And then, add the dependencies
```groovy

// ...

dependencies {

    /* Standard AndroidX Library Dependencies */ 
    implementation 'com.google.android.material:material:1.1.0'
    implementation 'androidx.appcompat:appcompat:1.1.0'
   
    //REQUIRED
    implementation 'com.frslabs.android.sdk:vimos:1.0.1' //Vimos SDK Dependency
    implementation "org.jetbrains.kotlin:kotlin-stdlib:1.4.10" //Kotlin Dependency
}
```

## Setup

#### Permissions

Vimos SDK uses the required permissions and features
```xml

     <!--Permissions Required for features including Video Call-->
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <!--Features-->
    <uses-feature android:name="android.hardware.camera" />
    <uses-feature android:name="android.hardware.camera.autofocus" />

```

## Quick Start

#### Invoking the Vimos SDK

Initialize the `Vimos` instance with the appropriate configurations. 
Call `start` on the instance to invoke the SDK.
Handle the result by extending the `VimosResultCallback`

<details><summary>Sample code in Java</summary>
<p>
  
```java
public class InitialActivity extends AppCompatActivity implements VimosResultCallback {

    // ...
    
    private String LICENSE_KEY = "ENTER_YOUR_LICENSE_KEY_HERE";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_initial);
        
        Button button = findViewById(R.id.invoke_sdk_btn);
        button.setOnClickListener(view -> invokeVimosSdk());
        
    }

    private void invokeVimosSdk() {
        VimosConfig vimosConfig = new VimosConfig();
        vimosConfig.setLicenseKey(LICENSE_KEY);
        vimosConfig.setVimosUrl("ENTER_VIMOS_URL_HERE");
        vimosConfig.setExitOnPageLoadError(false);

        Vimos vimos = new Vimos(vimosConfig);
        vimos.start(this, this);
    }

    @Override
    public void onVimosSuccess() {
        Toast.makeText(this, "Success", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onVimosFailure(VimosErrorCode vimosErrorCode) {
        Toast.makeText(this, "Vimos Error : " + vimosErrorCode, Toast.LENGTH_SHORT).show();
    }
    
    // ...
    
}
```
</p>
</details>

<details><summary>Sample code in Kotlin</summary>
<p>
  
```kotlin
class InitialActivity : AppCompatActivity(), VimosResultCallback {

    // ...
     
    private val LICENSE_KEY = "ENTER_YOUR_LICENSE_KEY_HERE"

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_initial)
        
        val invokeSdkBtn = findViewById<Button>(R.id.invoke_sdk_btn)
        invokeSdkBtn.setOnClickListener {
            invokeVimosSdk()
        }
        
    }

    private fun invokeVimosSdk() {

        val vimosConfig = VimosConfig().apply {
            licenseKey = LICENSE_KEY
            vimosUrl = "ENTER_VIMOS_URL_HERE"
            exitOnPageLoadError = false
        }

        Vimos(vimosConfig).start(this, this)

    }

    override fun onVimosSuccess() {
        Toast.makeText(this, "Success", Toast.LENGTH_SHORT).show()
    }

    override fun onVimosFailure(vimosErrorCode: VimosErrorCode) {
        Toast.makeText(this, "Vimos Error: $vimosErrorCode", Toast.LENGTH_SHORT).show()
    }

    // ...
    
}

```
</p>
</details>

## Vimos SDK Result

When control is returned to the `onVimosSuccess` method , it denotes a success state. No other Result/parameter is given back to the app.

## Vimos SDK Error Codes

Error codes and their meaning are tabulated below

| Label          | Code |Message                 |
| -------------- | ----- |---------------------- |
|ERROR_CODE_PERMISSION | 1000 | Required permissions for Vimos SDK were not granted |
|ERROR_CODE_EXPIRED_LICENSE | 1001 | Vimos SDK License has expired |
|ERROR_CODE_INVALID_LICENSE | 1002 | Invalid Vimos SDK License |
|ERROR_CODE_BACK_BTN_PRESSED | 1010 | Vimos SDK has exited due to back button press |
|ERROR_CODE_INVALID_CONFIG_URL | 1050 | Invalid Vimos URL provided |
|ERROR_CODE_PAGE_LOAD_ERROR | 1100 | Webpage loading error |

## Vimos SDK Parameters

- `setLicenseKey(String licenseKey)`   ***(Required)***
  
  Accepts the Vimos SDK licence key as a `String`
  
- `setVimosUrl(String vimosUrl)`   ***(Required)***
  
  Sets the Vimos URL as a `String`
  
- `setExitOnPageLoadError(boolean value)`   ***(Optional)***
  
  Toggle the ExitOnPageLoadError Flag. If enabled will exit the Vimos SDK when a webview page load error occurs.

## Help
For any queries/feedback , contact us at `support@frslabs.com` 
