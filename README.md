App42-NokiaX-Push-Notification
==============================


# About Application

1. This application shows how can you integrate PushNotification using App42 API in NokiaX Android application.


# Running Sample

This is a sample Android app is made by using App42 Android API. It uses PushNotification API of App42 platform.
Here are the few easy steps to run this sample app.

1. [Register] (https://apphq.shephertz.com/register) with App42 platform.
2. Create an app once, you are on Quick start page after registration.
3. If you are already registered, login to [AppHQ] (http://apphq.shephertz.com) console and create an app from App Manager Tab.
4. Download the project from [here] (https://github.com/VishnuGShephertz/Android-NokiaX-Push-Notification/archive/master.zip) and import it in your Eclipse.
5. Open MainActivity.java file of sample project and make following changes.

```
A. Replace api-Key and secret-Key that you have received in step 2 or 3 at line number 34 and 35.
B. Replace your user-id by which you want to register your application for PushNotification at line number 36.
```

__NokiaX based Push Integration :__ <div style="clear:both"></div>
13. Navigate to the [Nokia Notifications developer console] (https://console.push.nokia.com).Login with your credentials or register accordingly.<div style="clear:both"></div>
14. Click on Create services button on right side.<div style="clear:both"></div>
15. Fill necessary information to create NokiaX service and click on create button.<div style="clear:both"></div>
16. Now you get your Authorization key of NokiaX.<div style="clear:both"></div>
17. Go to [AppHQ] (http://apphq.shephertz.com) click on Push Notification and select Android Settings in Settings option.<div style="clear:both"></div>
18. Select your app and provider as NokiaX and copy Authorization key that is generated in above step.(please copy value of key not copy 'key=' text in that key).<div style="clear:both"></div>
19. Open CommonUtilities.java file of sample project and make following changes.

```
A. Replace your NokiaXSenderId  by your NokiaX Sender ID created in above step.
```


20.Build your android application and run on your android device.

__Test and verify PushNotification from AppHQ console__
 
```
A. After registering for PushNotification go to AppHQ console and click on PushNotification and select
  application after selecting User tab.
B. Select desired user from registered User-list and click on Send Message Button.
C. Send appropriate message to user by clicking Send Button.

```
# Design Details:

__Registration for PushNotification__ To register for Push Notification on App42 , you have to use method written in CommonIntentServiceImpl.java file. That register user with its registration Id whether its from GCm or NokiaX.
 
```
 private void registerWithApp42(String regId,DeviceType deviceType) {
		App42Log.debug(" Registering on Server ....");
		App42API.buildPushNotificationService().storeDeviceToken(
				App42API.getLoggedInUser(), regId,deviceType, new App42CallBack() {
					@Override
					public void onSuccess(Object paramObject) {
						// TODO Auto-generated method stub
						App42Log.debug(" ..... Registeration Success ....");
					
					}

					@Override
					public void onException(Exception paramException) {
						App42Log.debug(" ..... Registeration Failed ....");
						App42Log.debug("storeDeviceToken :  Exception : on start up "
								+ paramException);

					}
				});

	}

```

__Customization in Your Existing Android Project :__ To customize this plugin in your existing Android app to use common Notification Plugin , follow thses steps.

```
A. Copy com.shephertz.app42.common.push.plugin package in your project with itsjava classes.
B. Add jar files from sample to your project.
```
You have to edit  AndroidManifest.xml file accordingly :
Add permissions

```
 <uses-permission android:name="android.permission.INTERNET" />
 <uses-permission android:name="android.permission.VIBRATE" />
    <!-- Keeps the processor from sleeping when a message is received. -->
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <!--
        Creates a custom permission so only this app can receive its messages.

        NOTE: the permission *must* be called PACKAGE.permission.C2D_MESSAGE,
        where PACKAGE is the application's package name.
    -->
    <permission android:name="<Your Package Name>.permission.C2D_MESSAGE"    android:protectionLevel="signature" />
    <uses-permission android:name="<Your Package Name>permission.C2D_MESSAGE" />

    <uses-permission android:name="com.nokia.pushnotifications.permission.RECEIVE" />

```
Add Receiver and Services
```
 <receiver
            android:name="com.shephertz.app42.common.push.plugin.App42NokiaReceiver"
            android:permission="com.nokia.pushnotifications.permission.SEND" >
            <intent-filter>
                <!-- Receives the actual messages. -->
                <action android:name="com.nokia.pushnotifications.intent.RECEIVE" />
                <!-- Receives the registration id. -->
                <action android:name="com.nokia.pushnotifications.intent.REGISTRATION" />
                <category android:name="<Your Package Name>" />
            </intent-filter>
        </receiver>
      
        <!--
            Application-specific subclass of PushBaseIntentService that will
            handle received messages.
            
            By default, it must be named .PushIntentService, unless the
            application uses a custom BroadcastReceiver that redefines its name.
        -->
        <service android:name="com.shephertz.app42.common.push.plugin.App42NokiaService" />
        <meta-data name="onMessageOpen" value="com.example.android.nokia.app42.push.MainActivity"/>
```
__Message Open Activity:__ You can also change the Activty on which user should navigate when PushNotification is clicked in Android change it in AndroidManifest.xml file .
```
<meta-data android:name="onMessageOpen" android:value="com.example.android.nokia.app42.push.MainActivity"/>
```


