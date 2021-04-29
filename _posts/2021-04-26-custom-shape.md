---
layout: post
title: Create Custom Shape in SwiftUI
description: Create complex shapes for clipping.
repository: CustomShape
---

## Topics

Create complex shapes for clipping.

## Codes

Create a custom shape named `Triangle` implementing from `Shape`

```swift
import SwiftUI

struct Triangle: Shape {
    
    func path(in rect: CGRect) -> Path {
        
        Path { path in
            
            let pt1 = CGPoint(x: rect.width / 2, y: 0)
            let pt2 = CGPoint(x: 0, y: rect.height)
            let pt3 = CGPoint(x: rect.width, y: rect.height)
            
            path.move(to: pt1)
            path.addLine(to: pt2)
            path.addLine(to: pt3)
            path.addLine(to: pt1)
        }
    }
}
```

Use `Triangle` to clip a `Rectangle`

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        Rectangle()
            .frame(width: 100, height: 100)
            .clipShape(Triangle())
    }
}
```

## Screenshots

![Custom Shape](/assets/2021-04-26-custom-shape.png)

## Key Points

1. `Rectangle`, `Cricle`, `Capsule` and `RoundedRectangle` are built-in shape in SwiftUI.
1. A shape is widely used in UI development because of its reusibility.
