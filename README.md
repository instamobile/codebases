# How to run our codebases

## Prerequisites

- [Expo](https://expo.dev/)
- [React Native](https://reactnative.dev/)
- [Node 16](https://nodejs.org/en)
- (Optional) You can use [NVM](https://github.com/nvm-sh/nvm) to switch to the correct Node version.

## Running the app

Open the Terminal, locate the React Native project (make sure you are in the folder that contains the `package.json` file), and run:

```
yarn install && expo start
```

Our apps are basic Expo apps, so this is all you need to do. If you have any issues with running the app, you need to make sure Expo works fine on your machine - check out [Expo documentation](https://expo.dev/).

## Integrating Your Own Firebase

### Update Firebase Config
To link our Expo app to your own Firebase backend, open `src/core/firebase/config.js` and update the `firebaseConfig` variable with your own configuration.

```
const firebaseConfig = {
  apiKey: 'your api key',
  authDomain: 'your-project-id.firebaseapp.com',
  projectId: 'your-project-id',
  storageBucket: 'your-project-id.appspot.com',
  messagingSenderId: 'your sender id',
  appId: 'your app id',
}
```

### Enable Firebase Auth Methods

To enable Firebase Auth, simply go to Firebase Console -> Authentication -> Sign-in Methods and enable the methods that you are going to support in your app. By default, our React Native apps have integration with Email/Password, Phone and Facebook.

### Enable Firestore

In order to allow the mobile app to read and write data to/from Firebase Firestore, we need to set up the correct access permissions. To do that, just head over to Database -> Cloud Firestore and set the Rules for writes and reads to public.

```
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

### Enable Firebase Storage

If your mobile app needs access to Firebase Storage (e.g. for uploading photos and videos, for instance), you have to enable Firebase Storage, so that the functionality works properly. To enable it, just go to Storage in the left menu. This is how your Storage rules need to look like in order for the app to function correctly:

```
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### Deploying Firebase Functions

You can find all the firebase functions related to the projects in the `firebase/` folder. You need to follow the documentation from Firebase on [how to deploy these functions to your own projects](https://firebase.google.com/docs/functions/get-started).


Once you deployed the Firebase Functions, **please add the `allUsers` permission to `uploadMedia` function in [Google Cloud Console](https://console.cloud.google.com/functions) - this is needed to be able to upload media from web.**

### Enable Push Notifications
In order for push notifications to work properly, you need to retrieve from Firebase your own authorization key for the project, in Project Settings -> Cloud Messaging.

Then head over to `src/core/firebase/config.js` and override the Authorization field of the `retrieveFCMTokenForPushToken` method.

```  let config = {
    headers: {
      Authorization:
        'key=AAAA_-2JkDo:APA91bFPHWEtBQfcPnMQbiahs4-5ikZDcbXrK_PuD8bTdKhhNjNhb0rwbpEOkMQx_YOJWIeJLWLB9-1iwm5KaFYo8kxz5ygjJ9DFLL0caG9oigCa-hh5qKsFA-Zyrd-r3v_v8NqAUYy_',
      'Content-Type': 'application/json',
    },
  }
```

Notice: Push notifications won't work while developing using the Expo app, as you cannot retrieve a certificate from Apple for the Expo App ID. You will need to create your own app binary (which is needed for the app store submission anyway), in order to see push notifications working.

One you have your own app binary (e.g. by ejecting from Expo), you can follow [these instructions](https://instamobile.io/docs/documentation/push-notifications/push-notifications-on-ios/) to
1. Get a push notification certificate
2. Upload it to Firebase

Make you your App ID matches the app ID from Firebase, which matches the Bundle ID of the iOS project and the App ID of the certificate.
