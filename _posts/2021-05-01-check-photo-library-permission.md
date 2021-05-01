---
layout: post
title: Check Photo Library Permission in SwiftUI
description: You need check photo library permission everytime when app want to access user's photo library. Otherwise nothing happen, and never ask again if user rejected at the first time. Therefore, if you found your app has been rejected from photo library access permission, you need ask user again manually. That's why we have to check photo library permission manually and I write this article.
repository: CheckPhotoLibraryPermission
---

## Topics

You need check photo library permission everytime when app want to access user's photo library. Otherwise nothing happen, and never ask again if user rejected at the first time. Therefore, if you found your app has been rejected from photo library access permission, you need ask user again manually. That's why we have to check photo library permission manually and I write this article.

## Codes

**First of all** Add *Privacy - Photo Library Usage Description* to *Info.plist*.

```swift
import SwiftUI
import Photos

struct ContentView: View {
    
    @State var title = ""
    
    @State var message = ""
    
    var body: some View {
        
        VStack {

            if title != "" {
                
                Text(title)
                
                Text(message)
            }
            
            Button("Check Photo Library Permission") {
                
                let status = PHPhotoLibrary.authorizationStatus(for: .readWrite)
                switch status {
                case .limited:
                    title = "Limited"
                    message = "You can access user specified photos only"
                case .authorized:
                    title = "Authorized"
                    message = "You can access all photos"
                case .notDetermined:
                    PHPhotoLibrary.requestAuthorization(for: .readWrite) { result in
                        switch result {
                        case .authorized:
                            title = "Authorized"
                            message = "You can access all photos"
                        case .limited:
                            title = "Authorized"
                            message = "You can access all photos"
                        case .denied:
                            title = "Denied"
                            message = "You can not access anything, and you should guide user to settings here"
                        case .restricted:
                            fatalError("Should never be RESTRICT.")
                        case .notDetermined:
                            fatalError("Should never be NOT DETERMINED.")
                        @unknown default:
                            fatalError("Unknown status: \(result.rawValue)")
                        }
                    }
                case .denied:
                    title = "Denied"
                    message = "You can not access anything, and you should guide user to settings here"
                case .restricted:
                    title = "Restricted"
                    message = "Forbidden by Parent strategy"
                @unknown default:
                    fatalError("Unknown status: \(status.rawValue)")
                }
            }
        }
    }
}
```

## Screenshots

![Check Photo Library Permission](/assets/2021-05-01-check-photo-library-permission.gif)

## Key Points

1. Import **Photos** module.
1. Request permission when you found the authorization status is **notDetermined**.
