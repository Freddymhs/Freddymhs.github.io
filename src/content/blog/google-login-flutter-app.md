---
title: "Login de google en Flutter Multiplataforma"
description: "adding login into a flutter project"
pubDate: "jun 16 2024"
heroImage: "/blog-placeholder-2.jpg"
---



------
[guia de referencia de youtube](https://www.youtube.com/watch?v=utMg6fVmX0U)

------
# Como Crear un Proyecto Flutter con Google Auth

# Google Sign-In de Supabae en Flutter(android/web/ios)

1. open your flutter project
2. install:
    1. google-sign-in (pub.dev/packages/google_sign_in) 
        - `flutter pub add google_sign_in`
    2. supabase_flutter (pub.dev/packages/supabase_flutter)
        - `flutter pub add supabase_flutter`
3. into google console https://console.cloud.google.com/apis/credentials
    1. create credentials for CHROME.
    2. create credentials for ANDROID
      * get the `package name` from `build.gradle` in your project
      * get the `finger-print`with `SHA-1 with "keytool -list -v -alias androiddebugkey -keystore ~/.android/debug.keystore -storepass android"` in terminal
    3. create credentials for IOSp
    - * get the `package id` in -> `open ios/runner.xcworkspace` in `macOS`  
        - go to -> runner->general->Identity -> Bundle Identifier and copy
4. into supabase
    - create a project
      1. authentication -> providers -> google 
         - enable it
         - enable skip none checks for ios
         - copy the `client web id` and `paste` it in the `Authorized Client IDs (for Android, One Tap, and Chrome extensions)` field and Client ID (for OAuth) `from supabase`
         - copy the `client anon from web` and past it in `Client Secret (for OAuth)` from `supabase`
         -  add `http://localhost:3000/` into `URL CONFGURATION` into `supabase`
         -  go to supabase -> providers-> `Callback URL (for OAuth)` and copy it to paste into `google console web client id || URI de redireccionamiento autorizados`
   
    2. go to https://pub.dev/packages/google_sign_in_ios#ios-integration
       - complete the step 6 and replace com.googleusercontent.apps.[your-ios-client-id]
       - copy the example and paste under the last TRUE from info.plist
       - paste and edit the "<string>com.googleusercontent.apps.49813008970-ebmf6qv6pd4m9m86f151ad4gmahi0a55</string>"
         - remember remove the ".apps.googleusercontent.com"

5. into main.dart 
   1. add:   await Supabase.initialize(url: 'https://...', anonKey: 'somestring');
      1. all this data is from   supabase.com-> ProjectSettings -> API -> URL && ANON
6. test supabase client works in main.dart
   1. test this code in main.dart
7. final config
    - go to URL CONFIGURATION in supabase
      - https://supabase.com/dashboard/project/samvrmunxovxptwwmplv/auth/url-configuration
      - set the url:  `localhost:3000` in local but in production use `real url` http://.com


```

import 'dart:io';

import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';
import 'package:google_sign_in/google_sign_in.dart';
import 'package:supabase_flutter/supabase_flutter.dart';

import 'src/settings/settings_controller.dart';
import 'src/settings/settings_service.dart';

final supabase = Supabase.instance.client;

void main() async {
  // Set up the SettingsController, which will glue user settings to multiple
  // Flutter Widgets.
  final settingsController = SettingsController(SettingsService());

  // Load the user's preferred theme while the splash screen is displayed.
  // This prevents a sudden theme change when the app is first displayed.
  await settingsController.loadSettings();

  // supabase start
  await Supabase.initialize(
      url: 'https://sample.supabase.co',
      anonKey:'sample'
      );

  // Run the app and pass in the SettingsController. The app listens to the
  // SettingsController for changes, then passes it further down to the
  // SettingsView.
  // runApp(MyApp(settingsController: settingsController));
  // test supabase client with google sign in
  runApp(const MainApp());
}

class MainApp extends StatelessWidget {
  const MainApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  String? _userId;

  @override
  void initState() {
    super.initState();
    supabase.auth.onAuthStateChange.listen((data) {
      setState(() {
        _userId = data.session?.user?.id;
      });
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Login Page'),
      ),
      body: Center(
        child: ElevatedButton(
            onPressed: () async {
              if (!kIsWeb && (Platform.isAndroid || Platform.isIOS)) {
                await googleSignIn();
              } else {
                webGoogleSignIn();
              }
            },
            child: Text('Hola-> ${_userId}')),
      ),
    );
  }
}

Future<void> webGoogleSignIn() async {
  supabase.auth.signInWithOAuth(OAuthProvider.google);
}

Future<AuthResponse> googleSignIn() async {
  /// TODO: update the Web client ID with your own.
  ///
  /// Web Client ID that you registered with Google Cloud.
  const webClientId ='sample.apps.googleusercontent.com';

  /// TODO: update the iOS client ID with your own.
  ///
  /// iOS Client ID that you registered with Google Cloud.
  const iosClientId ='sample.apps.googleusercontent.com';

  // Google sign in on Android will work without providing the Android
  // Client ID registered on Google Cloud.

  final GoogleSignIn googleSignIn = GoogleSignIn(
    clientId: iosClientId,
    serverClientId: webClientId,
  );
  final googleUser = await googleSignIn.signIn();
  final googleAuth = await googleUser!.authentication;
  final accessToken = googleAuth.accessToken;
  final idToken = googleAuth.idToken;

  if (accessToken == null) {
    throw 'No Access Token found.';
  }
  if (idToken == null) {
    throw 'No ID Token found.';
  }

  return supabase.auth.signInWithIdToken(
    provider: OAuthProvider.google,
    idToken: idToken,
    accessToken: accessToken,
  );
}

```




```


