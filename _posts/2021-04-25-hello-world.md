---
layout: post
title: Create an Image by UIGraphicsImageRenderer
---

## Topics

How to write the code for drawing 2d images more layered and structured in Swift?

## Codes

```swift

import SwiftUI

struct ContentView: View {
    
    // Place the two image at the center of a 200x200 image
    var body: some View {
        let img1 = image1()
        let img2 = image2()
        let img = UIGraphicsImageRenderer(size: CGSize(width: 200, height: 200)).image { ctx in
            img1.draw(at: CGPoint(x: (200 - img1.size.width) / 2,
                                  y: (200 - img1.size.height) / 2))
            img2.draw(at: CGPoint(x: (200 - img2.size.width) / 2,
                                  y: (200 - img2.size.height) / 2))
        }
        Image(uiImage: img)
    }
    
    // Create a 100x100 image and fill a red 50x50 rectangle at the bottom-right
    func image1() -> UIImage {
        UIGraphicsImageRenderer(size: CGSize(width: 100, height: 100)).image { ctx in
            ctx.cgContext.setFillColor(UIColor.red.cgColor)
            ctx.fill(CGRect(x: 50, y: 50, width: 50, height: 50))
        }
    }
    
    // Create a 50x50 image and fill a green 25x25 rectangle at the top-left
    func image2() -> UIImage {
        UIGraphicsImageRenderer(size: CGSize(width: 50, height: 50)).image { ctx in
            ctx.cgContext.setFillColor(UIColor.green.cgColor)
            ctx.fill(CGRect(x: 0, y: 0, width: 25, height: 25))
        }
    }
}

```

## Key Points

1. Independent graphic context when you draw a image with `UIGraphicImageRenderer`.
1. `UIGraphicsImageRenderer` returns an `UIImage` object so that easy to be reused.