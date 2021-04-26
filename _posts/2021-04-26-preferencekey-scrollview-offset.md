---
layout: post
title: Use PreferenceKey (Get Offset of ScrollView in SwiftUI Efficently)
repository: PreferenceKeyScrollViewOffset
---

## Topics

Pass data from a child view to its parent view

## Codes

Define a `PreferenceKey` struct

```swift
import SwiftUI

struct OffsetPreferenceKey: PreferenceKey {
  static var defaultValue: CGFloat = .zero
  static func reduce(value: inout CGFloat, nextValue: () -> CGFloat) {}
}
```

Read the offset of a ScrollView real-time

```swift
import SwiftUI

struct ContentView: View {
    
    @State var offset: CGFloat = 0
    
    var body: some View {
        VStack {
            Text("Offset is \(offset)")
            ScrollView {
                Text("This is a text view in scroll view")
                    .background(
                        GeometryReader { proxy in
                            Color.clear
                                .preference(
                                    key: OffsetPreferenceKey.self,
                                    value: proxy.frame(in: .named("scroll")).minY
                                )
                        }
                    )
            }
            .onPreferenceChange(OffsetPreferenceKey.self, perform: { value in
                offset = value
            })
            .frame(maxWidth: .infinity)
            .background(Color.yellow)
            .coordinateSpace(name: "scroll")
        }
    }
}
```

## Screenshots

![PreferenceKey ScrollView Offset](/assets/2021-04-26-preferencekey-scrollview-offset.gif)

## Key Points

1. Write value by `preference` modifier in child view
1. Read value by `onPreferenceChange` modifier in parent view
