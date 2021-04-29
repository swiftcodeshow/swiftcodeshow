---
layout: post
title: Create a Custom View with Multiple @ViewBuilder Parameters
description: You need multiple @ViewBuilder parameters when you expose more than one slots of your custom view.
repository: CreateCustomViewMultipleViewBuilderParameters
---

## Topics

You need multiple @ViewBuilder parameters when you expose more than one slots of your custom view.

## Codes

Create a custom view with two slots in its layout.

```swift
import SwiftUI

struct CustomView<V1: View, V2: View>: View {
    
    let view1: V1
    
    let view2: V2
    
    init(@ViewBuilder content1: @escaping () -> V1, @ViewBuilder content2: @escaping () -> V2) {
        self.view1 = content1()
        self.view2 = content2()
    }
    
    var body: some View {

        VStack {
            
            VStack {
                
                Text("Part One")
                    .foregroundColor(.white)
                
                view1
            }
            .background(Color.red)
            
            VStack {
                
                Text("Part Two")
                    .foregroundColor(.white)
                
                view2
            }
            .background(Color.blue)
        }
        
    }
}

```

Create an `CustomView` instance with two different contents.

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        CustomView {
            
            RoundedRectangle(cornerRadius: 25.0)
                .padding()
            
        } content2: {
            
            Circle()
                .padding()
        }
    }
}
```

## Screenshots

![Create Custom View Multiple ViewBuilder Parameters](/assets/2021-04-26-create-custom-view-multiple-viewbuilder-parameters.png)

## Key Points

1. The type of a @ViewBuilder parameter must be different with each other.
1. Don't forget `@escaping` in the parameter declaration.
