---
layout: post
title: Create Horizontal Scrollable Menu (the Active Item Centered) in SwiftUI
description: We usually need a horizontal scrllable menu at the top of screen, and each item maybe a category name. And we want the active item to be centered always unless it is closed to the leading item or the trailing item.
repository: HorizontalScrollableMenu
---

## Topics

We usually need a horizontal scrllable menu at the top of screen, and each item maybe a category name. And we want the active item to be centered always unless it is closed to the leading item or the trailing item.

Maybe you want to use scrollTo and its anchor parameter to archive this feature, but you found that when you scroll to the last items, more space appears at the trailing, it's not what we want. This is why I wrote this article to guide you to a perfect solution for a scrollable horizontal menu.

## Codes

```swift
import SwiftUI

extension View {
    
    func getRect() -> CGRect {
        return UIScreen.main.bounds
    }
}

extension String {
    
    func getSize() -> CGSize {
        (self as NSString).size(withAttributes: nil)
    }
}

struct Type: Identifiable, Equatable {
    var id = UUID().uuidString
    var title: String
}

let types: [Type] = [
    Type(title: "Fruits"),
    Type(title: "Clothes"),
    Type(title: "Toys"),
    Type(title: "Foods"),
    Type(title: "Novels"),
    Type(title: "Sports"),
    Type(title: "Books"),
    Type(title: "Tools"),
    Type(title: "Cars"),
    Type(title: "Devices"),
    Type(title: "Colors"),
]

struct ContentView: View {
    
    @State var selection = types.first
    
    var body: some View {
        
        ScrollView(.horizontal, showsIndicators: false) {

            ScrollViewReader { proxy in

                HStack(spacing: 10) {

                    ForEach(types) { type in

                        Button(action: {

                            if let i = types.firstIndex(of: type) {

                                let textWidth = type.title.getSize().width

                                let midX = CGFloat(i) * (textWidth + 50) + 0.5 * (textWidth + 40)

                                let containerWidth = types.reduce(10) { (result, item) -> CGFloat in
                                    result + item.title.getSize().width + 50
                                }

                                withAnimation(.spring()) {
                                    selection = type
                                    if midX + getRect().width / 2 < containerWidth {
                                        proxy.scrollTo(type.id, anchor: .center)
                                    } else {
                                        proxy.scrollTo(types.last?.id, anchor: .trailing)
                                    }
                                }
                            }
                        }) {
                            Text(type.title)
                                .fontWeight(.semibold)
                        }
                        .frame(width: type.title.getSize().width + 40,
                               height: type.title.getSize().height + 24,
                               alignment: .center)
                        .background(Color.black.opacity(selection?.id == type.id ? 0.5 : 0))
                        .foregroundColor(.white)
                        .clipShape(RoundedRectangle(cornerRadius: 5))
                        .id(type.id)
                        .padding(.trailing, type.id == types.last?.id ? 10 : 0)
                    }
                }
                .padding(.horizontal, 15)
            }
        }
        .padding(.vertical)
        .background(Color.yellow)
    }
}
```

## Screenshots

![Horizontal Scrollable Menu](/assets/2021-04-28-horizontal-scrollable-menu.gif)

## Key Points

1. `Type` struct must implement `Identifiable` for `ForEach` usage.
1. `Type` struct must implement `Equatable` for collection compare, in this case `firstIndex` depends on it.
1. Extend `String` with a custom function `getSize` to get size of a string.
1. `ScrollViewReader` has a method `scrollTo` is key point in this case, more about it in *Further Reading* section.
1. `withAnimation` wraps `scrollTo` to make sure that the scroll is animated.

## Further Reading

* [Use ScrollViewReader and scrollTo to Control Scrolling in ScrollView](https://swiftcodeshow.com/2021/04/26/scrollviewreader-scrollto.html)
