---
layout: post
title: Push Changes Manually for an ObservableObject
repository: PushChangesManuallyObservableObject
---

## Topics

Assume that you have a **class** variable marked `@Published` in your `ObservableObject` class, you need to push changes manually when you change the property of this class variable, because the reference of the `@Published` variable doesn't changed.

## Codes

```swift
import SwiftUI

class Person {
    var id = UUID().uuidString
}

class Model: ObservableObject {
    @Published var person = Person()
}

struct ContentView: View {
    
    @StateObject var data = Model()
    
    var body: some View {
        VStack {
            Button("Change id but not push changes") {
                // Set a new id
                data.person.id = UUID().uuidString
            }
            .padding()
            .foregroundColor(.white)
            .background(Color.red)
            Text("Person's ID is \(data.person.id)")
            Button("Change id and push changes") {
                data.objectWillChange.send()
                // Set a new id
                data.person.id = UUID().uuidString
            }
            .padding()
            .foregroundColor(.white)
            .background(Color.green)
        }
    }
}
```

## Screenshots

![Push Changes Manually ObservableObject](/assets/2021-04-26-push-changes-manually-observableobject.gif)

## Key Points

1. `objectWillChange.send()` must run before you change values.
1. If you have a dictionary variable with `@Published`, and change a value (not key) of it, you should push changes manually.
1. Most of time, you are recommanded use struct instead of class as possible as you can.
