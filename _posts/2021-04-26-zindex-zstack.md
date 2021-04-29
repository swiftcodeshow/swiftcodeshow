---
layout: post
title: Use zIndex in ZStack
description: zIndex is neccessary in ZStack to specify orders of controls at z-axis in ZStack.
repository: ZIndexZStack
---

## Topics

`zIndex` is neccessary in `ZStack` to specify orders of controls at z-axis in `ZStack`.

## Codes

```swift
import SwiftUI

struct ContentView: View {
    
    @State var toggle = false
    
    var body: some View {
    
        VStack {
            
            Button("Switch 2 shapes") {
                toggle.toggle()
            }
         
            ZStack {
                
                Rectangle()
                    .frame(width: 100, height: 100)
                    .foregroundColor(.green)
                    .zIndex(toggle ? 0 : 1)
                
                Circle()
                    .frame(width: 120, height: 120)
                    .foregroundColor(.red)
                    .zIndex(toggle ? 1 : 0)
            }
            
        }
    }
}
```

## Screenshots

![Z-Index ZStack](/assets/2021-04-26-zindex-zstack.gif)

## Key Points

1. By default, the default z-index of controls are the same as their orders in your code.
1. Always use z-index if you have complex animation, otherwise unexpected crash will happen sometimes, it looks randomly but absolutely happen.
1. `withAnimation` doesn't work with `zIndex` modifier.
