### 异常处理

- (版本太旧)This app is using a deprecated version of the Android embedding. To avoid unexpected runtime failures, or future build failures, try to migrate this app to the V2 embedding. Take a look at the docs for migrating an app: https://github.com/flutter/flutter/wiki/Upgrading-pre-1.12-Android-projects 
```
mobile_app/android/app/src/main/AndroidManifest.xml 
AndroidManifest.xml >  manifest >  application 
android:name="${applicationName}" 
<meta-data android:name="flutterEmbedding" android:value="2" /> 
```

- Could not find a file named pubspec.yaml 
```
dart create -t console vector_victor  
```

- Flutter 3.3.6 中FlatButton, RaisedButton, & OutlineButton找不到 
![[imgs/GetImage.png]]
```flutter
FlatButton, RaisedButton, and OutlineButton这三个widget 已经有了新的控件替代，并且有新的主题 

做对应的替换即可 

FlatButton( 
  onPressed: onPressed, 
  child: Text('Button'), 
  // ... 
); 

RaisedButton( 
  onPressed: onPressed, 
  child: Text('Button'), 
  // ... 
); 

OutlineButton( 
  onPressed: onPressed, 
  child: Text('Button'), 
  // ... 
); 

TextButton( 
  onPressed: onPressed, 
  child: Text('Button'), 
  // ... 
); 

ElevatedButton( 
  onPressed: onPressed, 
  child: Text('Button'), 
  // ... 
); 

OutlinedButton( 
  onPressed: onPressed, 
  child: Text('Button'), 
  // ... 
); 
```
