---
layout: post
title: Use ScrollViewReader and scrollTo to Control Scrolling in ScrollView
description: By now, you can scroll to an control by ID with fancy animation in ScrollView.
repository: ScrollViewReaderScrollTo
---

## Tips

By now, you can scroll to an control by ID with fancy animation in ScrollView.

## Codes

```swift
import SwiftUI

// Model
struct Item: Identifiable {
    var id = UUID().uuidString
    var name: String
}

// Demo data
let items: [Item] = [
    Item(name: "Kevin"),
    Item(name: "Cary"),
    Item(name: "Yilia"),
    Item(name: "Lora"),
    Item(name: "Anastasia"),
    Item(name: "Rachel"),
    Item(name: "Jane"),
    Item(name: "Tina"),
    Item(name: "Olivia"),
    Item(name: "Helen")
]

// View
struct ContentView: View {
    var body: some View {
        
        ScrollView(.horizontal, showsIndicators: false) {
            
            ScrollViewReader { proxy in
                
                HStack {
                    ForEach(items) { item in
                        
                        Button(item.name) {
                            
                            withAnimation {
                                
                                proxy.scrollTo(item.id, anchor: .leading)
                            }
                        }
                        .padding()
                        .background(Color.accentColor)
                        .foregroundColor(.white)
                        // Here must be id, NOT tag
                        .id(item.id)
                    }
                }
            }
        }
    }
}
```

## Screenshots

![scrollTo](/assets/2021-04-26-scrollviewreader-scrollto.gif)

## Key Points

1. `scrollTo` find position by the ID of controls, **NOT** its tag.
1. Wrap `scrollTo` with `withAnimation` making scrolling smooth.
1. Argument `anchor` specifies the ending position of scrolling.
