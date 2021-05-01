---
layout: post
title: Check Camera Permission in SwiftUI
description: You need check camera permission everytime when app want to open the camera to capture a photo or record a video. Otherwise nothing happen, and never ask again if user rejected at the first time. Therefore, if you found your app has been rejected from camera access permission, you need ask user again manually. That's why we have to check camera permission manually and I write this article.
repository: CheckCameraPermission
---

## Topics

You need check camera permission everytime when app want to open the camera to capture a photo or record a video. Otherwise nothing happen, and never ask again if user rejected at the first time. Therefore, if you found your app has been rejected from camera access permission, you need ask user again manually. That's why we have to check camera permission manually and I write this article.

## Codes

**First of all** Add *Privacy - Camera Usage Description* to *Info.plist*.

```swift
import SwiftUI
import AVFoundation

struct ContentView: View {
    
    @State var title = ""
    
    @State var message = ""
    
    var body: some View {
        VStack {
            
            if title != "" {
                Text(title)
                Text(message)
            }
            
            Button("Check camera permission") {
                
                let status = AVCaptureDevice.authorizationStatus(for: .video)
                switch status {
                case .authorized:
                    title = "Authorized"
                    message = "You can use camera"
                case .notDetermined:
                    AVCaptureDevice.requestAccess(for: .video) { granted in
                        if granted {
                            title = "Authorized"
                            message = "User allows app to access camera"
                        }
                    }
                case .denied:
                    title = "Denied"
                    message = "User rejected app to access camera"
                case .restricted:
                    title = "Restricted"
                    message = "User is limited by Parent settings to access camera"
                @unknown default:
                    fatalError("Unknown status: \(status.rawValue)")
                }
            }
        }
    }
}
```
## Snapshots

![Check Camera Permission](/assets/2021-05-01-check-camera-permission.gif)

## Key Points

1. Import **AVFoundation** or **AVKit** module.
1. Request permission when you found the authorization status is **notDetermined**.