---
layout: post
title: Use ActionSheet in SwiftUI
repository: ActionSheet
---

## Topics

Use an action sheet when you want the user to make a choice between two or more options, in response to their own action. If you want the user to act in response to the state of the app or the system, rather than a user action, use an `Alert` instead.

## Codes

```swift
struct ContentView: View {
    
    @State var showActionSheet = false
    
    @State var favoritePlace = ""
    
    var body: some View {
        VStack {
            
            Button("Show Action Sheet") {
                showActionSheet.toggle()
            }
            
            Text("Your favorite place is: \(favoritePlace)")
        }
        // Only one actionSheet here, otherwise unexpected result will happen
        .actionSheet(isPresented: $showActionSheet, content: {
            ActionSheet(title: Text("Where are you going?"), message: Text("Tell me your favorite place"), buttons: [
                .default(Text("United State"), action: {
                    favoritePlace = "United State"
                }),
                .default(Text("China"), action: {
                    favoritePlace = "China"
                }),
                .default(Text("Korea"), action: {
                    favoritePlace = "Korea"
                }),
                .default(Text("United Kindom"), action: {
                    favoritePlace = "United Kindom"
                }),
                .cancel()
            ])
        })
    }
}
```

## Screenshots

![ActionSheet](/assets/2021-04-29-actionsheet.gif)

## Key Points

1. Don't need disappear an action sheet manually, I mean, you never set `showActionSheet` to `nil` manually in this case, because `ActionSheet` will do that when user select one item or cancel the action sheet.
1. Never use `actionSheet` multiple times in a same scene, I mean, only one `actionSheet` will work no matter how many `actionSheet` you wrote.
