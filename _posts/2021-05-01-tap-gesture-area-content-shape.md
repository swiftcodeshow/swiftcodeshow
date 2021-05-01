---
layout: post
title: Specify the Tap Gesture Area by contentShape
description: Defines the content shape for hit testing. Factually, it's not always a full filled rect by default, and it depends on your codes.
repository: TapGestureAreaContentShape
---

## Topics

Defines the content shape for hit testing. Factually, it's not always a full filled rect by default, and it depends on your codes.

## Codes

```swift
import SwiftUI

struct ContentView: View {
    
    @State var showAlert = false
    
    var body: some View {

        VStack {
            
            Image(systemName: "photo")
            
            Spacer()
            
            Text("Only text and image can be tapped unless you set contentShape to a rectangle")
        }
        .frame(width: 200, height: 200)
        .padding()
        .border(Color.black)
        .contentShape(Rectangle())
        .onTapGesture {
            showAlert.toggle()
        }
        .alert(isPresented: $showAlert, content: {
            Alert(title: Text("Tapped!"))
        })
    }
}
```

## Snapshots

![Tap Gesture Area Content Shape](/assets/2021-05-01-tap-gesture-area-content-shape.gif)

## Key Points

1. By default, `contentShape` does not contain `Spacer`, unless you set background to a color (but `Color.clear`) or a full filled view.
