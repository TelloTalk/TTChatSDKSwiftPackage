# TelloTalk SDK - iOS

TelloTalk SDK is a solution for integrating chat messaging in your application.

## Pre Requisites
You must have **Access Key** & **Project Key** to use this SDK in your application.

## Installation
Add **TTChatSDK.xcframework** file provided into your application, then select you application target, in *General* tab, then add .framework file in *Embedded Binaries*

*or*
```
pod 'TTChatSDK'
````

*or*

Add `https://github.com/TelloTalk/TTChatSDKSwiftPackage.git` as a swift package.

## Usage

### Initializaition

```swift
import TTChatSDK
```

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    // Override point for customization after application launch.
    do {
        try TTChat.shared.setup(accessKey: "<ACCESS_KEY>", projectToken: "<PROJECT_KEY>")
    } catch let error { 
        print(error)
    }
    return true    
}
```

### Register User
When you're logging in or signing up the application for the first time you need to call this, if there is error then you will get value in `errorString` otherwise it will be nil
```swift
TTChat.shared.registerUser(profileId: "<ProfileId>", name: "<Name>", mobileNumber: "<Mobile Number>") { [weak self] (buddy, errorString) in
    guard let self = self else { return }
}
```

### Login User
`errorString` will be nil if `login` is successfull otherwise it will throw an error, After calling this method connection between server and mobile app will be started for getting messages.

```swift
do {
    try TTChat.shared.login(completion: { [weak self] (errorString) in 
        guard let self = self else { return }
    })
} catch let error {
    print(error)
    // might be profile id missing error
}
```

## Corporate List UI of TelloTalk SDK

You can directly open corporate list, you can set `title` of that list if you want otherwise passed `nil`
```swift
TTChat.shared.showCorporateUsers(controller: self, title: "Title")
```

### Receiving Messages Notifications (APNS)

#### Step 1:
Create Certificate and upload it to provided panel or contact tech@tilismtech.com

[Establishing a Certificate-Based Connection to APNs](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/establishing_a_certificate-based_connection_to_apns)

#### Step 2:
In `AppDelegate.swift`

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    application.registerForRemoteNotifications()
    return true
}
```

#### Step 3:
```swift
func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
    TTChat.shared.didRegisterForRemoteNotifications(with: deviceToken)
}

```

#### Step 4:
Turn On `Push Notifications` from *Capabilities* by selecting your target

### Unread Message Count:
```swift
TTChat.shared.getTotalUnreadMessageCount { [weak self] (count) in
    guard let self = self else { return }
}
```
`count` will be the unread messages count

### Permissions for Chat:
*  `NSCameraUsageDescription` - For sending images/video from camera
*  `NSPhotoLibraryUsageDescription` - For sending images/video from photo library
*  `NSContactsUsageDescription` - For sending contacts and syncing contacts (if required)
*  `NSLocationWhenInUseUsageDescription` - For sending location message
*  `NSMicrophoneUsageDescription` - For sending audio note message

### UI Customizations:
```swift
TTChat.shared.settings.themeColor = .green
TTChat.shared.settings.outgoingMessageBubbleColor = .green
TTChat.shared.settings.outgoingMessageBubbleTextColor = .white
TTChat.shared.settings.messageStateIconColor = UIColor.init(white: 0.8, alpha: 1)
TTChat.shared.settings.readIconColor = UIColor(red: 0, green: 255/255, blue: 71/255, alpha: 1)
TTChat.shared.settings.timeOutgoingColor = UIColor.init(white: 0.9, alpha: 1)
TTChat.shared.settings.audioRecordButtonColor = .systemGreen
TTChat.shared.settings.sendButtonColor = .systemGreen
```
