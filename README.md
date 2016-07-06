**NOTICE: Firebase ANE is currently in Beta mode and is not yet available to public. Make sure you are registered in our [newsletter](http://www.myflashlabs.com/contact/) to be notified as soon as it's ready. Before the final release of the ANE, the documentation may change a lot!**
--------------------------------------------------

# Firebase Air Native Extension V1.0.0 Android+iOS
Firebase ANE gives you access to the [Google Firebase project](https://firebase.google.com/docs/) in your AdobeAir projects supported on both Android and iOS with 100% identical ActionScript API. 

If you decide to use Firebase in your next AdobeAir project, you should consider the following structure: Firebase Air Native Extension is consist of a *Core* ANE plus some other individual ANEs which are all dependent on the *Core*. i.e, If you wish to use [Firebase Cloud Messaging (FCM)](https://firebase.google.com/docs/cloud-messaging/), you need to embed the Core ANE first and then use the required ANE(s) for the FCM. This structure will make sure that you are not compiling unused native code in your AdobeAir project. In result, your app file size will be as small as possible and faster to debug/compile. [The Wiki pages](https://github.com/myflashlab/Firebase-ANE/wiki) will provide you detailed information about how you can embed each ANE based on the Firebase feature you wish to use in your app.

**Main Features:**
* [Analytics](https://firebase.google.com/docs/analytics/) Reimagine analytics for mobile 
* [Cloud Messaging (FCM)](https://firebase.google.com/docs/cloud-messaging/) Deliver and receive messages across platforms reliably 
* [Authentication](https://firebase.google.com/docs/auth/) Reduce friction with robust authentication 
* [Realtime Database](https://firebase.google.com/docs/database/) Store and sync app data in realtime 
* [Storage](https://firebase.google.com/docs/storage/) Store files with ease 
* [Remote Config](https://firebase.google.com/docs/remote-config/) Customize your app on the fly 
* [Crash Reporting](https://firebase.google.com/docs/crash/) Keep your app stable 
* [Notifications](https://firebase.google.com/docs/notifications/) Engage with users at the right moment 
* [App Indexing](https://firebase.google.com/docs/app-indexing/) Drive organic search traffic to your app 
* [Dynamic Links](https://firebase.google.com/docs/dynamic-links/) Send users to the right place inside your app 
* [Invites](https://firebase.google.com/docs/invites/) Empower your users to share your app 
* [AdWords](https://firebase.google.com/docs/adwords/) Acquire users with the reach of Google 
* [Admob](https://firebase.google.com/docs/admob/) Monetize through engaging ads 

# asdoc
[find the latest asdoc for this ANE here.](http://myflashlab.github.io/asdoc/com/myflashlab/air/extensions/firebase/core/package-detail.html)  
[How to get started? **read here**](https://github.com/myflashlab/Firebase-ANE/wiki)

**NOTICE**: Unlike our other ANEs, the Firebase will not be available for demo. You need to download the [Firebase core from here](http://www.myflashlabs.com/product/firebase-air-native-extension/).

# Air Usage
```actionscript
import com.myflashlab.air.extensions.firebase.core.Firebase;
import com.myflashlab.air.extensions.firebase.core.FirebaseConfig;

// initialize the Firebase as early as possible in your project
var isConfigFound:Boolean = Firebase.init();

/*
	Calling Firebase.init() is just good enough to initialize Firebase core in your Air 
	project. However for debugging reasons, you can check the config values like below:
	
	What config files you may ask? well, This is explained in details in the Wiki pages. 
	https://github.com/myflashlab/Firebase-ANE/wiki but to give you a quick idea, I 
	should say that before being able to start with Firebase, you need to create a 
	Firebase account (It's free) and introduce your app to your Firebase console. When 
	you do that, your Firebase console will give you a config file. it will be a .plist 
	for iOS "GoogleService-Info.plist" and a .json file "google-services.json" for the 
	Android side. The content of these two config files are similar to these: 
	https://github.com/myflashlab/Firebase-ANE/tree/master/FD/bin
	
	You need to make sure that these two config files are being embedded in your project 
	by putting them in the bin folder of your project. (Next to the main .swf file of 
	your app)
*/

if (isConfigFound)
{
	var config:FirebaseConfig = Firebase.getConfig();
	trace("default_web_client_id = " + 			config.default_web_client_id);
	trace("firebase_database_url = " + 			config.firebase_database_url);
	trace("gcm_defaultSenderId = " + 			config.gcm_defaultSenderId);
	trace("google_api_key = " + 				config.google_api_key);
	trace("google_app_id = " + 					config.google_app_id);
	trace("google_crash_reporting_api_key = " + config.google_crash_reporting_api_key);
	trace("google_storage_bucket = " + 			config.google_storage_bucket);
}
else
{
	trace("Config file is not found!");
}

/*
	To know how to use other features of Firebase, read the Wiki:
	https://github.com/myflashlab/Firebase-ANE/wiki
*/
```

# Air .xml manifest
```xml
<!--
FOR ANDROID:
-->
<manifest android:installLocation="auto">
	
	<uses-permission android:name="android.permission.INTERNET" />
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.WAKE_LOCK"/>
	
	<application>
		
		<activity>
			<intent-filter>
				<action android:name="android.intent.action.MAIN" />
				<category android:name="android.intent.category.LAUNCHER" />
			</intent-filter>
			<intent-filter>
				<action android:name="android.intent.action.VIEW" />
				<category android:name="android.intent.category.BROWSABLE" />
				<category android:name="android.intent.category.DEFAULT" />
			</intent-filter>
		</activity>
		
		<!-- 
			Required by the firebase_common.ane 
			Change "air.com.doitflash.firebaseCore" to your own app package name
		-->
		<provider
			android:name="com.google.firebase.provider.FirebaseInitProvider"
			android:authorities="air.com.doitflash.firebaseCore.firebaseinitprovider"
			android:exported="false"
			android:initOrder="100" />
		
		<!-- Required by the googlePlayServices_basement.ane -->
		<meta-data 
			android:name="com.google.android.gms.version" 
			android:value="@integer/google_play_services_version" />
		
	</application>
</manifest>





<!--
FOR iOS:
-->
	<InfoAdditions>
		
		<!--iOS 7.0 or higher can support this ANE-->
		<key>MinimumOSVersion</key>
		<string>7.0</string>
		
	</InfoAdditions>
	
	
	
	
	
<!--
Embedding the ANE:
-->
  <extensions>
	
	<!-- download the dependency ANEs from https://github.com/myflashlab/common-dependencies-ANE -->
	<extensionID>com.myflashlab.air.extensions.dependency.firebase.common</extensionID>
	<extensionID>com.myflashlab.air.extensions.dependency.googlePlayServices.basement</extensionID>
	<extensionID>com.myflashlab.air.extensions.dependency.googlePlayServices.tasks</extensionID>
	<extensionID>com.myflashlab.air.extensions.dependency.androidSupport</extensionID>
	
	<!-- And finally embed the Firebase core ANE -->
	<extensionID>com.myflashlab.air.extensions.firebase.core</extensionID>
	
  </extensions>
-->
```

# Requirements 
1. Android API 15 or higher
2. iOS SDK 7.0 or higher
3. Air SDK 22 or higher
4. This ANE is dependent on **androidSupport.ane**, **firebase_common.ane**, **googlePlayServices_basement.ane** and **googlePlayServices_tasks.ane** You need to add these ANEs to your project too. [Download them from here:](https://github.com/myflashlab/common-dependencies-ANE)
5. To compile on iOS, you will need to add the Firebase core frameworks to your Air SDK.
  - download **FirebaseCoreFrameworks.zip** package from our github and extract it on your computer.
  - you will find some *.framework* files inside the package. Just copy them as they are and go to your AdobeAir SDK.
  - when in your Air SDK, go to "\lib\aot\stub". There you will find all the iOS frameworks provided by Air SDK by default.
  - paste the Firebase frameworks you had copied into this folder and you are ready to build your project on iOS too.

# Commercial Version
http://www.myflashlabs.com/product/firebase-air-native-extension/

![Firebase ANE](http://www.myflashlabs.com/wp-content/uploads/2016/06/product_adobe-air-ane-extension-firebase-595x738.jpg)

# Tutorials
[How to embed ANEs into **FlashBuilder**, **FlashCC** and **FlashDevelop**](https://www.youtube.com/watch?v=Oubsb_3F3ec&list=PL_mmSjScdnxnSDTMYb1iDX4LemhIJrt1O)  
[How to get started with Firebase?](https://github.com/myflashlab/Firebase-ANE/wiki)

# Changelog
*Jul 05, 2016 - V1.0.0*
* beginning of the journey!