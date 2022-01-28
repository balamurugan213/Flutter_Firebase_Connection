# Connecting Firebase To The Flutter App(WEB and Android)

Created on: January 25, 2022 9:34 AM
Last edited: January 27, 2022 7:11 PM

## STEP 1

Create a flutter app

```dart
Flutter create flutter_firebase_app
```

## STEP 2 (Optional)

- Initializze Git in your flutter project.

```dart
git init
```

- Create a Git repo without readme.

![Untitled](/images/Untitled.png)

- Follow the steps provided by the by the github to add remote to the github repo.

 

![Untitled](/images/Untitled1.png)

Steps explained:

```dart
//stage readme file
git add README.md

//commit
git commit -m "zero commit"

//add github remote to the project
git remote add origin https://github.com/balamurugan213/Flutter_Firebase_Connection.git

//Change the branch to Main
git branch -M main

//Stage all the project files
git add .

//commit the Staged files
git commit -m "first commit"

//push the main branch to the github
git push -u origin main

```

RESULT:

![Untitled](/images/Untitled2.png)

## STEP 3

- Create a new firebase Project
    
    ![Capture5.JPG](/images/Capture5.jpg)
    
    ![Capture6.JPG](/images/Capture6.jpg)
    

### Adding an android app to in the poject

- Create an android app by clicking the icon.
    
    ![Capture7.JPG](/images/Capture7.jpg)
    
- Give the required details
    
    ![Capture8.JPG](/images/Capture8.jpg)
    
    - **Android package Name**
        
        Android package name will be availabe in a file inside the project in the following directory:
        
        Project>Andriod>app>build.gradle
        
    - **App Nickname**
        
        You can keep it  as per your wish
        
    - **Debug digning certificate SHA-1**
        
        Even though it is optional it s better if you give the SHA-1 key as it is required for doing mobile authentication and for other services
        
        You can also update after creating the app
        
- Download google-service.json and paste inside the following directory in the project file .(the file name should be exact same).
    
    andriod>app>[paste here]
    
    ![Capture10.JPG](/images/Capture10.jpg)
    
- Paste the codes in the gradle files.
    
    ![Capture11.JPG](/images/Capture11.jpg)

    
- Import the required dart  packages in pubspec.yaml file
    
    ```dart
    //To add firebase to flutter
    firebase_core: ^1.11.0
    
    //To implement cloud firestore
    cloud_firestore: ^3.1.6
    //To implement firebase auth
    firebase_auth: ^3.3.5
    ```
    
- Initial firebase app on the main function of the app
    
    ```dart
    import 'package:firebase_core/firebase_core.dart';
    
    Future<void> main() async {
      WidgetsFlutterBinding.ensureInitialized();
      await Firebase.initializeApp();
      runApp(const MyApp());
    }
    ```
    
    ![Capture16.JPG](/images/Capture16.jpg)
    
- Run the app in android
    - If the app is running properly then you implemented firebase in your app.
    - If you find any error then check the steps again.
    - On possible error might be minimum sdk version, by default it will be 16 but firebase requires 19 and above (mainly firebase analytics)
    - you can update the min sdk version in build.gradle in the app file

![Capture19.JPG](/images/Capture19.jpg)

---

### Adding a Web Application to the project

- Create a web app in the firebase console.
    
    ![Capture7.JPG](/images/Capture7.jpg)
    
- Give the necessary detial to create the app(skip and continue the other steps)
    
    ![Capture13.JPG](/images/Capture13.jpg)
    
- Go to the project setting and copy the firebaseConfig code
    
    ![Capture14.JPG](/images/Capture14.jpg)
    
- Paste the code in the dart main function as options parameter in firebase initializeApp function.
    
    ![Capture17.JPG](/images/Capture17.jpg)
    
- Adding the following scripts to the index.html file in the following directory
    
    `Project>web>index.html`
    
    ```html
    // To add firebase functionality to the project
    <script src="https://www.gstatic.com/firebasejs/9.6.3/firebase-app.js"></script>
    
    // To implement cloud firestore
    <script src="https://www.gstatic.com/firebasejs/9.6.3/firebase-firestore.js"></script>
    // To implement firebase auth
    <script src='https://www.gstatic.com/firebasejs/9.6.3/firebase-auth.js'></script>
    ```
    
- Run the flutter app in web, if it run without any error then you successfully implemented firebase in flutter web.

### One special step

- If you see for implementing in app we don’t require options in firebase initializeapp but for implemention in web requires option.
- This might lead to runtime error if you try to implement both without changing the code.
- To avioid the we can use KIsWeb in foundation package which returns true if we run on web and false for other platforms.

```dart
import 'package:flutter/foundation.dart';

Future<void> main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(
    options: kIsWeb
        ? const FirebaseOptions(
            .
            .
            .
        : null,
  );
  runApp(const MyApp());
}
```

## STEP 4 (for Github)

If you are going to upload your project to the Github you should keep in your mind that the google services json file and your web credentials are in main file.

If you upload in the public repository then there is possiblities of other people copying your credentials and using it.

To be safe export your web credential on seperate dart file like webcredentials.dart

```dart
const firebaseConfig = FirebaseOptions(
    .
    .
    .
);
```

```dart
import 'package:flutter_firebase_app/webcredentials.dart';

Future<void> main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(
    options: kIsWeb
        ? firebaseConfig
        : null,
  );
  runApp(const MyApp());
```

Now you can add these 2 file in the gitignore file to avoid staging
