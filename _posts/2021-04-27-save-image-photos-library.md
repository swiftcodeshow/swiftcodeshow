---
layout: post
title: Save Image to Photos Library in One Line Code
description: How to save an image to photos library as simple as possible? Better to in one line only, and you got it.
repository: SaveImagePhotosLibrary
---

## Topics

How to save an image to photos library as simple as possible? Better to in one line only, and you got it.

## Codes

**First of all** Add *Privacy - Photo Library Additions Usage Description* to *Info.plist*. Otherwise, your app will crash when you try to write something to photos library.

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        
        Button(action: {
            
            // write to photos library and it will ask for permission only at the first time
            UIImageWriteToSavedPhotosAlbum(image(), nil, nil, nil)
        }, label: {
            Text("Save a black rectangle image to photos library")
                .padding()
                .background(Color.blue)
                .foregroundColor(.white)
        })
        
    }
    
    // Create a simple image
    private func image() -> UIImage {
        
        UIGraphicsImageRenderer(size: CGSize(width: 100, height: 100)).image { ctx in
            let bounds = CGRect(x: 0, y: 0, width: 100, height: 100)
            ctx.cgContext.setFillColor(UIColor.yellow.cgColor)
            ctx.fill(bounds)
            ("I am the fancy image you just created" as NSString).draw(in: bounds, withAttributes: nil)
        }
    }
}
```

## Screenshots

![Save Image Photos Library](/assets/2021-04-27-save-image-photos-library.gif)

## Key Points

1. Don't forget add *Privacy - Photo Library Additions Usage Description* key and description to your *Info.plist*, and description is must be human readable. Because your users will see it at the first time you ask for permission to their photos library.
1. If your user denied your request, no crash happen, just do nothing. You can read status and reason from callback function, I ignore it because this is just a kick-off tutorial.
1. If your user denied your request, you can tell him re-grant write permission to your app in *Settings* -> *Privacy* -> *Photos* and your app will be there, just active. It's very easy.
