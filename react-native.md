If you are using Windows/Mac run the commands in the following way, or if you get an error "Cannot find entry file index.android.js"

```
1. mkdir android\app\src\main\assets
2. react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res

3. react-native run-android
```