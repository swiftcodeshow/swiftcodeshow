---
layout: post
title: Create an Image by UIGraphicsImageRenderer
repository: CreateImageUIGraphicsImageRenderer
---

## Topics

How to write the code for drawing 2d images more layered and structured in Swift?

## Codes

```swift
import SwiftUI

struct ContentView: View {
    
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
            .border(Color.black, width: 1)
    }
    
    func image1() -> UIImage {
        // Create a 100x100 image
        UIGraphicsImageRenderer(size: CGSize(width: 100, height: 100)).image { ctx in
            // Fill a red 50x50 rectangle at the bottom-right
            ctx.cgContext.setFillColor(UIColor.red.cgColor)
            ctx.fill(CGRect(x: 50, y: 50, width: 50, height: 50))
            // Stroke a red 100x100 rectangle for the final image
            ctx.cgContext.setStrokeColor(UIColor.red.cgColor)
            ctx.stroke(CGRect(x: 0, y: 0, width: 100, height: 100))
        }
    }
    
    func image2() -> UIImage {
        // Create a 50x50 image
        UIGraphicsImageRenderer(size: CGSize(width: 50, height: 50)).image { ctx in
            // Fill a green 25x25 rectangle at the top-left
            ctx.cgContext.setFillColor(UIColor.green.cgColor)
            ctx.fill(CGRect(x: 0, y: 0, width: 25, height: 25))
            // Stroke a green border for the final image
            ctx.cgContext.setStrokeColor(UIColor.green.cgColor)
            ctx.stroke(CGRect(x: 0, y: 0, width: 50, height: 50))
        }
    }
}

```

## Screenshot

![Screenshot](/assets/2021-04-25-create-image-uigraphicsimagerenderer-p1.png)

## Key Points

1. Independent graphic context when you draw a image with `UIGraphicImageRenderer`.
1. `UIGraphicsImageRenderer` returns an `UIImage` object so that easy to be reused.
1. Much easier to use rather than `CoreGraphics` low level APIs on context management.