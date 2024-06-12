---
tags: 
date: 2024-06-09 16:43
source:
  - https://developers.google.com/maps/documentation/android-sdk/start?hl=ja
---
## Google Maps SDKのAPIキーの設定
- [google console](https://console.cloud.google.com/home/dashboard?hl=ja&pli=1&project=rentalnetreview)
### 1. Secrets Gradleプラグインの導入
#### プロジェクトレベルの`build.gradle`ファイル
```kotlin
// Top-level build file where you can add configuration options common to all sub-projects/modules.  
plugins {  
    alias(libs.plugins.android.application) apply false  
    alias(libs.plugins.jetbrains.kotlin.android) apply false
    // これを追加
    id("com.google.android.libraries.mapsplatform.secrets-gradle-plugin") version "2.0.1" apply false  
}
```
#### モジュールレベルの`build.gradle`ファイル
```kotlin
plugins {  
    alias(libs.plugins.android.application)  
    alias(libs.plugins.jetbrains.kotlin.android)  
	//これを追加
    id("com.google.android.libraries.mapsplatform.secrets-gradle-plugin")  
}
```
### 2. gradleを同期する
### 3. `local.properties`にAPIキーを設定
```kotlin
sdk.dir=/Users/kouseihujisaki/Library/Android/sdk  
MAPS_API_KEY={}
```
### 4. `AndroidManifest.xml`の設定
```kotlin
<?xml version="1.0" encoding="utf-8"?>  
<manifest xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:tools="http://schemas.android.com/tools">  
  
    <application        android:allowBackup="true"  
        android:dataExtractionRules="@xml/data_extraction_rules"  
        android:fullBackupContent="@xml/backup_rules"  
        android:icon="@mipmap/ic_launcher"  
        android:label="@string/app_name"  
        android:roundIcon="@mipmap/ic_launcher_round"  
        android:supportsRtl="true"  
        android:theme="@style/Theme.RentalNetMap"  
        tools:targetApi="31">
        // ここ追加
        <meta-data android:name="com.google.android.geo.API_KEY"  
            android:value="${MAPS_API_KEY}" />  
        <activity            android:name=".MainActivity"  
            android:exported="true"  
            android:label="@string/app_name"  
            android:theme="@style/Theme.RentalNetMap">  
            <intent-filter>
	            <action android:name="android.intent.action.MAIN" />  
				<category android:name="android.intent.category.LAUNCHER" />  
            </intent-filter>
        </activity>
    </application>
    // ユーザーパーミッションを追加
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>  
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>  
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />  
</manifest>
```
## Google Maps Composeプラグインの設定
```kotlin
implementation ( "com.google.maps.android:maps-compose:2.11.4" )  
implementation ( "com.google.android.gms:play-services-maps:18.1.0" )
```
