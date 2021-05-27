# unity android
1. File > Build Settings...
> ![](https://i.imgur.com/19W9wFr.png)
2. Player Settings...
> ![](https://i.imgur.com/7MmqYv5.png)
3. Export
4. 新增資料夾"androidBuild"，並選擇存放至該資料夾
>AndroidUnitySample
>&nbsp;- Android
>&nbsp;&nbsp;&nbsp;&nbsp;- .gradle
>&nbsp;&nbsp;&nbsp;&nbsp;- .idea
>&nbsp;&nbsp;&nbsp;&nbsp;- app
>&nbsp;&nbsp;&nbsp;&nbsp;- build
>&nbsp;&nbsp;&nbsp;&nbsp;- gradle
>&nbsp;&nbsp;&nbsp;&nbsp;- .gitgnore
>&nbsp;&nbsp;&nbsp;&nbsp;......
>&nbsp;- Unity
>&nbsp;&nbsp;&nbsp;&nbsp;- androidBuild

#### settings.gradle
```js
include ':unityLibrary'
project(':unityLibrary').projectDir=new File('..\\Unity\\androidBuild\\unityLibrary')
```

#### build.gradle (My Application)
Project
```js
allprojects {
    repositories {
        /*...*/
        flatDir {
            dirs "${project(':unityLibrary').projectDir}/libs"
        }
    }
}
```

#### build.gradle (:app)
Moudle
```js
dependencies {
    implementation project(':unityLibrary')
    implementation fileTree(dir: project(':unityLibrary').getProjectDir().toString() + ('\\libs'), include: ['*.jar'])
    /*...*/
}
```

#### gradle.properties
```js
unityStreamingAssets=.unity3d, google-services-desktop.json, google-services.json, GoogleService-Info.plist
```

#### Click "Sync Now"
> 產生unityLibrary

#### local.properties
> download ndk
```js
sdk.dir=C\:\\Users\\...\\AppData\\Local\\Android\\Sdk
ndk.dir=C\:\\Users\\...\\AppData\\Local\\Android\\Sdk\\ndk\\22.1.7171670
```

#### build.gradle (:app)
Module
```js
android {
    /*...*/
    defaultConfig {
        minSdkVersion 19
        /*...*/
        ndk {
            abiFilters 'armeabi-v7a', 'arm64-v8a'
        }
        /*...*/
    }
    /*...*/
}
```

#### strings.xml
```xml
<string name="game_view_content_description">Game view</string>
```