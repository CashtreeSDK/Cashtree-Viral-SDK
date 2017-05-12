# Cashtree-Viral-SDK
AD Tracking & Viral SDK for Cashtree

## Integration Method
Import the SDK file which is provided by AdTracking (AdTrackingSDK_v1.5.0.jar) to Project.

----

### Gradle
```
dependencies {
    compile 'com.android.support:support-v4:23.2.1'

    compile files('libs/AdTrackingSDK_v1.5.0.jar')

    compile files('libs/okhttp-3.7.0.jar')
    compile files('libs/okio-1.12.0.jar')
}
```

Insert the libraries that need SDK as below into the Project.
- *android-support-v4.jar*
- *okhttp-3.7.0.jar*
- *okio-1.12.0.jar*

----

### AndroidManifest.xml
In case there is not any permission as below, insert it into **AndroidManifest.xml** file.
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
```

Insert the following receiver below application in **AndroidManifest.xml**.
```xml
<receiver android:name="com.ad.tracking.cpe.agent.ADTrackingReceiver" android:exported="true">
    <intent-filter>
        <action android:name="com.ad.tracking.cpe.agent.COMPLETED" />
    </intent-filter>
</receiver>
<receiver android:name="com.ad.tracking.cpe.agent.ReferrerCatcher"  android:enabled="true" android:exported="true" >
    <intent-filter>
        <action android:name="com.android.vending.INSTALL_REFERRER" />
    </intent-filter>
</receiver>
<receiver android:name="com.ad.tracking.cpe.agent.MyNetChange">  
    <intent-filter>  
        <action android:name="android.net.conn.CONNECTIVITY_CHANGE"/>  
    </intent-filter>  
</receiver> 
```

If you want to register **multiple referrer receiver**, you insert the codes as below.
```xml
<receiver android:name="com.ad.tracking.cpe.agent.ADTrackingReceiver" android:exported="true">
    <intent-filter>
        <action android:name="com.ad.tracking.cpe.agent.COMPLETED" />
    </intent-filter>
</receiver>
<receiver android:name="com.ad.tracking.cpe.agent.ReferrerCatcher" android:enabled="true" android:exported="true" >
    <intent-filter>
        <action android:name="com.android.vending.INSTALL_REFERRER" />
    </intent-filter>
    <!-- for multiple referrer receiver -->
    <meta-data android:name="OtherReceiver1" android:value="com.example.ad_tracking_sample.SampleReferrerReceiver"/>
</receiver>
<receiver android:name="com.ad.tracking.cpe.agent.MyNetChange">
    <intent-filter>  
        <action android:name="android.net.conn.CONNECTIVITY_CHANGE"/>  
    </intent-filter>  
</receiver>
```

----

## Function Integration
### Init
First parameter: context 
Second parameter (merchant_id): merchant_id provided by ADTrackingAgent
If the third parameter (debugMode) is true, print SDK Log Output.
```java
...
ADTrackingAgent.init(Context context, String merchant_id, boolean isDebugMode);
...
```

### Complete
Call the SDK function in the desired time/location.
For example, call upon first launch, call upon sign up/ register, call upon reaching a certain level.

First parameter: context 
Second parameter (merchant_id): merchant_id provided by ADTrackingAgent
```java
...
ADTrackingAgent.completed(Context context, String merchant_id);
...
```

## Code Obfuscastion (ProGuard Usage)
ADTrackingAgent SDK has already been obfuscated.
Thus, when obfuscating application, please exclude SDK.

Enter the jar file name which is actually applied in the libraryjars
```
-libraryjars libs/AdTrackingSDK_v1.5.0.jar
-keep class com.ad.tracking.cpe.agent.**{*;}
-dontwarn com.ad.tracking.cpe.agent
-dontwarn okio.**
```

Input the code above in the obfuscation file. 

Provided here in, SampleApp.zip file, is the example of how to apply ADTrackingAgent SDK
