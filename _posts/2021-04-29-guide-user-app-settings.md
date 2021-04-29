---
layout: post
title: Guide User to Your App Settings Page
repository: GuideUserAppSettings
---

## Topics

You should provide a shortcut to your app settings page when users use some iOS features and your app is not granted permissions. For example, users disallowed your app access their photo library, however some time they want you use it in your app, then you should check photo library permissions when they active the app feature, and guide users to your app settings to grant photo library permission to your app.

## Codes

In this demo, we will ask user for camera permission but not open camera really.

**First of all** Add *Privacy - Camera Usage Description* to *Info.plist*.

Next, request permission when app have launched. **In GuideUserAppSettingsApp.swift NOT ContentView.swift**

```swift
import SwiftUI
import AVFoundation

@main
struct GuideUserAppSettingsApp: App {
    
    @UIApplicationDelegateAdaptor(AppDelegate.self) var appDelegate
    
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}

class AppDelegate: NSObject, UIApplicationDelegate {
    
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]? = nil) -> Bool {
        
        let status = AVCaptureDevice.authorizationStatus(for: .video)
        if status == .notDetermined {
            AVCaptureDevice.requestAccess(for: .video) { _ in }
        }
        
        return true
    }
}

```

Finally, we place a button to show an `Alert` to ask user to open *Settings* page in **ContentView.swift**.

```swift
import SwiftUI

struct ContentView: View {

    @State var show = false
    
    var body: some View {
        Button("Ask user for Settings") {
            show.toggle()
        }
        .alert(isPresented: $show, content: {
            return Alert(title: Text("Permission Denied"), message: Text("Grant permissions to App in Settings"), primaryButton: .default(Text("Go to Settings"), action: {
                let url = URL(string: UIApplication.openSettingsURLString)!
                UIApplication.shared.open(url, options: [:], completionHandler: nil)
            }), secondaryButton: .cancel())
        })
    }
}
```

## Screenshots

![Guide User App Settings](/assets/2021-04-29-guide-user-app-settings.gif)

## Key Points

1. Your app must requested some permissions excactlly, otherwise **Settings** page will be empty.
