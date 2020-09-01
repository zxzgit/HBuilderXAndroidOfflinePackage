#部署说明

资料：https://nativesupport.dcloud.net.cn/AppDocs/usesdk/android

本项目建立 于 `HBuildX 2.8.8.20200820` , uni-app的 `Android-SDK@2.8.8.80368_20200821`

#uni-app 基础库文件集入注意

## app/libs目录
将 lib.5plus.base-release.aar、android-gif-drawable-release@1.2.17.aar、uniapp-release.aar和miit_mdid_1.0.10.aar（HBuilderX2.8.1之后更新到msa_mdid_1.0.13.aar）拷贝到 app/libs目录下

注意：自HBuilderX2.8.0开始，JS引擎默认从jscore改为V8，提升运算性能，离线sdk自HBuilderX2.8.1也将默认JS引擎切换到V8，新增 `uniapp-v8-release.aar`（uniapp-v8-release.aar和uniapp-release.aar不能同时使用。）。

## app/src/main/res/values/strings.xml

```
<resources>
    <string name="app_name">应用名称</string>
</resources>
```

## app/src/main/assets/data/dcloud_control.xml

appid= `<uni-app的dcloud账号项目id 对应于 uni-app应用包表示(AppId)，也就是 maninfest.json中 appid 属性>`

```
<hbuilder>
<apps>
    <app appid="__UNI__2****0" appver=""/>
</apps>
</hbuilder>
```

## app/build.gradle
```
android {
    compileSdkVersion 30
    buildToolsVersion "30.0.2"
    defaultConfig {
        applicationId "com.example.hbuilderxandroidofflinepackage"
        minSdkVersion 22
        targetSdkVersion 30
        versionCode 100 //对应 uni-app manifest.json 的 应用版本号
        versionName "1.0.0" ////对应 uni-app manifest.json 的 应用版本名称
        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    aaptOptions {//文档说明添加节点
        additionalParameters '--auto-add-overlay'
        ignoreAssetsPattern "!.svn:!.git:.*:!CVS:!thumbs.db:!picasa.ini:!*.scc:*~"
    }
}

dependencies {//文档说明修改其下内容
    implementation fileTree(dir: 'libs', include: ['*.aar', '*.jar'], exclude: [])
    implementation "com.android.support:support-v4:28.0.0"
    implementation "com.android.support:appcompat-v7:28.0.0"
    implementation 'com.android.support:recyclerview-v7:28.0.0'
    implementation 'com.facebook.fresco:fresco:1.13.0'
    implementation "com.facebook.fresco:animated-gif:1.13.0"
    implementation 'com.github.bumptech.glide:glide:4.9.0'
    implementation 'com.alibaba:fastjson:1.1.46.android'
}
```

## app/src/main/AndroidManifest.xml

（1）application 节点 `android:icon`属性值修改为 "@drawable/icon"

```
android:icon="@drawable/icon"
```



（2）application 节点下内容修改为如下

```

        <activity
            android:name="io.dcloud.PandoraEntry"
            android:configChanges="orientation|keyboardHidden|keyboard|navigation"
            android:label="@string/app_name"
            android:launchMode="singleTask"
            android:hardwareAccelerated="true"
            android:theme="@style/TranslucentTheme"
            android:screenOrientation="user"
            android:windowSoftInputMode="adjustResize" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name="io.dcloud.PandoraEntryActivity"
            android:launchMode="singleTask"
            android:configChanges="orientation|keyboardHidden|screenSize|mcc|mnc|fontScale|keyboard"
            android:hardwareAccelerated="true"
            android:permission="com.miui.securitycenter.permission.AppPermissionsEditor"
            android:screenOrientation="user"
            android:theme="@style/DCloudTheme"
            android:windowSoftInputMode="adjustResize">
            <intent-filter>
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <action android:name="android.intent.action.VIEW" />
                <data android:scheme="h56131bcf" />
            </intent-filter>
        </activity>

```

## app/src/main/assets/apps 目录下放置 HBuildX 离线打包资源

```
--app
-----src
--------main
------------apps
----------------__UNI__2****0 //HBuildX离线打包的包，包名对应于 uni-app应用包表示(AppId)，也就是 maninfest.json中 appid 属性
```

## app/src/main/res/drawable/下图片对应图片替换为自己的

```
--drawable

------ icon.png
------ push.png
------ splash.png
```


