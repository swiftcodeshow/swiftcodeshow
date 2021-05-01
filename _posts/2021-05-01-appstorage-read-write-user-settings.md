---
layout: post
title: Use @AppStorage Read/Write User Settings
description: A property wrapper type that reflects a value from UserDefaults and invalidates a view on a change in value in that user default. 
repoistory: AppStorageReadWriteUserSettings
---

## Topics

A property wrapper type that reflects a value from *UserDefaults* and invalidates a view on a change in value in that user default. 

## Codes

```swift
import SwiftUI

struct ContentView: View {
    
    @AppStorage("skipTutorials") var skipTutorials = false
    
    var body: some View {
        
        if skipTutorials {
            
            Text("Tutorials are skipped.")
            
        } else {
            
            VStack {
                
                Text("This is tutorials")
                
                Button("Skip") {
                    skipTutorials = true
                }
            }
        }
    }
}
```

## Snapshots

![AppStorage Read Write User Settings](/assets/2021-05-01-appstorage-read-write-user-settings.gif)

## Key Points

1. You can store any object to UserDefaults.
1. @AppStorage is an observable object.
