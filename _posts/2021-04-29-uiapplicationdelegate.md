---
layout: post
title: Use UIApplicationDelegate in SwiftUI
description: SwiftUI also use UIApplicationDelegate to manage shared behaviors for your app when need. This tutorial tells you how to use UIApplicationDelegate in SwiftUI.
repository: UIApplicationDelegate
---

## Topics

SwiftUI also use UIApplicationDelegate to manage shared behaviors for your app when need. This tutorial tells you how to use UIApplicationDelegate in SwiftUI.

## Codes

```swift
import SwiftUI

@main
struct UIApplicationDelegateApp: App {
    
    @UIApplicationDelegateAdaptor(ApplicationDelegate.self) var appDelegate
    
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}

class ApplicationDelegate: NSObject, UIApplicationDelegate {
    
    // Any hooks you need in UIApplicationDelegate,
    // or anything you want to do in this class the same as before (not in SwiftUI)
    
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]? = nil) -> Bool {
        
        // Do anything what you want ...
        
        return true
    }
}
```

## Key Points

1. `ApplicationDelegate` class must inherited from `NSObject` and implements `UIApplicationDelegate`.
1. Use `@UIApplicationDelegateAdaptor` import `ApplicationDelegate`
1. No Value-Type for variable `appDelegate` on delcaration.

## Further Reading

* [Guide User to Your App Settings Page](https://swiftcodeshow.com/2021/04/29/guide-user-app-settings.html)
