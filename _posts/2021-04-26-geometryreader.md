---
layout: post
title: Use GeometryReader in SwiftUI
repository: GeometryReader
---

## Topics

Get parent's frame and arrange controls manually

## Codes

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        VStack  {
        
            GeometryReader { proxy in
                
                Text("Parent's width: \(proxy.size.width), height: \(proxy.size.height)")
                    .border(Color.black)
                    // In GeometryReader, you should arrange controls manually
                    .position(CGPoint(x: proxy.size.width / 2, y: proxy.size.height / 2))
                
                Rectangle()
                    .stroke()
                    .foregroundColor(.red)
                    .position(CGPoint(x: proxy.size.width / 2, y: proxy.size.height / 2))
            }
        }
        .frame(width: 300, height: 500)
    }
}
```

## Screenshots

![GeometryReader](/assets/2021-04-26-geometryreader.png)

## Key Points

1. In GeometryReader, you should arrange controls manually.
1. `position` tips, the (0, 0) is the center of a control not the top-left of it.
