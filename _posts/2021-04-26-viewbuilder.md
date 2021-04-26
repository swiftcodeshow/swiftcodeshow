---
layout: post
title: Use @ViewBuilder in SwiftUI
repository: ViewBuilder
---

## Topics

Accept a view as a constructor parameter of a custom view, in another words, write your user interface codes in SwiftUI style.

## Codes

Create a custom view to centerlize its content.

```swift
import SwiftUI

struct CustomView<Content: View>: View {
    
    let content: Content
    
    init(content: @escaping () -> Content) {
        self.content = content()
    }
    
    var body: some View {
        
        HStack {
            
            GeometryReader { proxy in
             
                // Centerlize the content
                content
                    .position(CGPoint(x: proxy.size.width / 2, y: proxy.size.height / 2))
                
            }
            
        }
        .background(Color.yellow)
        .frame(maxWidth: .infinity)
    }
}
```

Use `CustomView`

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        CustomView {
            Text("Hello World!")
                .padding()
                .background(Color.blue)
                .foregroundColor(.white)
        }
    }
}
```

## Screenshots

![ViewBuilder](/assets/2021-04-26-viewbuilder.png)

## Key Points

1. Specify a associated View type in struct declaration.
2. Use `@escaping` in parameter declaration.

## Further Reading

* [Create a Custom View with Multiple @ViewBuilder Parameters](/2021/04/26/create-custom-view-multiple-viewbuilder-parameters.html)
