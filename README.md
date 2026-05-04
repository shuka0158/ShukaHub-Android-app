# ShukaHub Android App
Free chatting app created by me. (My Username on ShukaHub: @shuka0158) Feel free to messege.
SLIDE FROM LEFT TO RIGHT TO OPEN CHATS LIST.

(If you were not asked automatically after opening app for first time: then you need to maually allow notifications from: Settings > Apps > ShukaHub > Notifications
OR Settings > Notifications > App Notifications > ShukaHub)

ATTENTION!: IF YOU HAVE GESTURES ENABLED ON YOUR PHONE SO APP JUST CLOSES IF YOU SLIDE ON DISPLAY TO OPEN CHATS/FRIENDS LIST: USE VALERA VERSION! it has special button on left upper corner of display for opening chats list.

# ShukaHub

A private real-time chat application built with vanilla JavaScript, Firebase, and Capacitor. Available as a web app and Android APK.

---

## Features

### Authentication
- Register with a unique numeric ID and password (SHA-256 hashed, stored client-side)
- Login by ID or USERNAME
- Animated login screen with interactive character
- Change password and display name from settings
- Delete account

### Friends
- Search and add users by ID
- Friend list sorted by most recently chatted (live, no refresh needed)
- Unread message indicator dot per friend
- Nickname system — set a custom local nickname for any friend
- Block / unblock / report / mute users
- Hide friends from list without removing them

### Direct Chats
- Real-time messaging via Firestore `onSnapshot`
- Message status stamps:
  - ✓ gray — sent (not yet delivered)
  - ✓✓ gray — delivered (recipient's device received it)
  - ✓✓ green — seen (recipient opened the chat)
- Typing indicator
- Reply to specific messages
- Edit and delete sent messages
- Emoji reactions with display name tooltips
- Image sharing (compressed before upload)
- File sharing with type icons
- Link detection with click-to-open popup
- Optimistic rendering (message appears instantly on send)

### Groups
- Create groups with a name and optional icon
- Group members list, admin roles
- Add / remove members, promote/demote admins
- Leave or delete group
- @mention autocomplete for members and @everyone
- Group list sorted by most recently chatted (live)
- Group icon upload

### UI / Theming
- Glass-morphism design with CSS variables
- Multiple color themes (selectable per user)
- Message decoration options
- Floating glass chat input bar
- Image lightbox with download
- Profile modal (avatar, bio, last seen, online status)
- In-app notifications (toast-style)
- Admin broadcast message channel

### Push Notifications
- Firebase Cloud Messaging (FCM) integration
- Notifications for new messages (direct and group)
- Deep-link navigation on notification tap (opens correct chat)

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML / CSS / JavaScript (ES2020) |
| Database | Firebase Firestore (Web SDK v8) |
| Auth | Custom (SHA-256 hash, Firestore storage) |
| Push | Firebase Cloud Messaging |
| Android | Capacitor 5.7.8 |
| Hosting | Firebase Hosting |
| Build | Gradle (Android) |

---

## Project Structure

ShukaHub/
├── shukahub_android/        # Capacitor app source
│   ├── index.html           # Main SPA entry point
│   ├── js/app.js            # Application logic
│   ├── css/style.css        # Styles
│   ├── www/                 # Built assets synced to Android
│   │   ├── js/app.js        # Normal build
│   │   └── js/app_valera.js # Valera variant build
│   └── android/             # Android native project (Gradle)
│       └── app/
│           └── build.gradle
├── shukahub_deploy/         # Firebase Hosting deployment source
│   ├── index.html
│   ├── js/app.js
│   └── css/style.css
├── functions/               # Firebase Cloud Functions
├── cf-worker/               # Cloudflare Worker (notifications)
├── ShukaHub.apk             # Latest normal APK (output)
├── ShukaHub_valera.apk      # Latest Valera APK (output)
└── .firebaserc              # Firebase project config



---

## Building

### Prerequisites
- Node.js + npm
- Java 17 (`JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64`)
- Android SDK
- Firebase CLI (`npm install -g firebase-tools`)
- ADB (for device install)

### Web deployment

```bash
cd shukahub_deploy
firebase deploy --only hosting
Android APK (normal)

cd shukahub_android
npx cap sync android
cd android
JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64 ./gradlew assembleRelease
# Output: android/app/build/outputs/apk/release/app-release.apk
Android APK (Valera variant)

# Swap www/js/app.js with www/js/app_valera.js, then build the same way
cp www/js/app_valera.js www/js/app.js
npx cap sync android
cd android
JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64 ./gradlew assembleRelease
Install on device

adb uninstall com.shukahub.app
adb install ShukaHub.apk
Firebase Setup
Project: shukahub-chat
Hosting URL: https://shukahub.web.app
Firestore collections: users, chats, groups
App ID (Android): com.shukahub.app
Signing keystore: android/app/shukahub-release.jks
Message Status Flow

Sender sends       →  status: "sent"     →  ✓  (gray)
Recipient loads app →  status: "delivered" →  ✓✓ (gray)
Recipient opens chat →  status: "read"     →  ✓✓ (green)
Delivery is marked automatically in listenUnread() when the recipient's device receives the message. Read is marked in openChat() when the chat is opened.
