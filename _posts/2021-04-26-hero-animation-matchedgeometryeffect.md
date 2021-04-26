---
layout: post
title: Create Hero Animation by matchedGeometryEffect in SwiftUI
repository: HeroAnimationMatchedGeometryEffect
---

## Topics

Attach two different views with ID and a fancy animation from one to another in only one line code. The animation is called Hero Animation.

## Codes

```swift
import SwiftUI

struct ContentView: View {
    
    @Namespace var animation
    
    @State var toggle = false
    
    var body: some View {
        
        if toggle {
            Rectangle()
                .matchedGeometryEffect(id: "shape", in: animation)
                .foregroundColor(.green)
                .frame(width: 100, height: 100)
                .onTapGesture {
                    withAnimation {
                        toggle.toggle()
                    }
                }
        } else {
            Circle()
                .matchedGeometryEffect(id: "shape", in: animation)
                .foregroundColor(.red)
                .frame(width: 50, height: 50)
                .onTapGesture {
                    withAnimation {
                        toggle.toggle()
                    }
                }
        }
    }
}
```

## Screenshots

![Hero Animation matchedGeometryEffect](/assets/2021-04-26-hero-animation-matchedgeometryeffect.gif)

## Key Points

1. `matchedGeometryEffect` must be used before `frame`, otherwise animation may be unexpected.
1. Views has same id in the same namespace **CAN'T** exists at the same time. That's why the two views in if-else blocks, otherwise a warning appears, and animation will be out of control.
1. An `@Namespace` variable is neccessary, and you can pass it as a variable to any other views when you want to make an animation cross views.
1. Animation maybe not work perfect in Xcode Preview, you'd better preview your animation in simulator and device.
