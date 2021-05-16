---
layout: post
title: Create an Image by UIGraphicsImageRenderer
description: How to write the code for drawing 2d images more layered and structured in Swift?
repository: CreateImageUIGraphicsImageRenderer
---

## Table of Contents

* Brief Introduction to Core Graphics Low-Level APIs
* Brief Introduction to UIGraphicsImageRenderer
* Advantages of UIGraphicsImageRenderer
* UIGraphicsImageRenderer Works Together With Core Graphics Low-Level APIs
* Demo Project
* Conclusion

## Brief Introduction to Core Graphics Low-Level APIs

Core Graphics supported the Quartz drawing engine, has provided iOS developers with light-weight second rendering capabilities since iOS 2. Its utility is aware of nearly no bounds, as image masking, PDF document creation, parsing, and alternative similar functions area unit baked right in creating it a no-nonsense selection for any variety of drawing task.

Core Graphics provides low-level, lightweight 2D rendering with unmatched output fidelity. You use this framework to handle path-based drawing, transformations, color management, offscreen rendering, patterns, gradients and shadings, image data management, image creation, and image masking, as well as PDF document creation, display, and parsing.

In macOS, Core Graphics also includes services for working with display hardware, low-level user input events, and the windowing system.

How to create an image from something on screen developers will likely end up with something like this:

```swift
func createSomeImage() -> UIImage? {

    let drawSize = CGSize(width: 100, height: 100)
    UIGraphicsBeginImageContext(drawSize)
    // Release the image context when the function returns
    defer { UIGraphicsEndImageContext() }
    // Get current image context, and care about exceptions
    guard let ctx = UIGraphicsGetCurrentContext() else {
        fatalError("Failed to get current context...")
    }
    
    ctx.setFillColor(UIColor.red.cgColor)
    ctx.fill(CGRect(x: 50, y: 50, width: 50, height: 50))
    ctx.setStrokeColor(UIColor.red.cgColor)
    ctx.stroke(CGRect(x: 0, y: 0, width: 100, height: 100))
    
    // Return the final image, but the result is optional...
    return UIGraphicsGetImageFromCurrentImageContext()
}
```

## Brief Introduction to UIGraphicsImageRenderer

You can use image renderers to accomplish drawing tasks, while not having to handle configuration like color depth and image scale, or manage Core Graphics contexts. You initialize an image renderer with parameters like image output dimensions and format. You then use one or additional of the drawing functions to render images that share these properties.

Rewrite `createSomeImage` function in `UIGraphicsIamgeRenderer` way like this:

```swift
func createSomeImage() -> UIImage {
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
```

## Advantages of UIGraphicsImageRenderer

* **Without having to manage Core Graphics contexts and configurations.** 
* **Less codes than Core Graphics low-level APIs.**
* **Returns a non-optional UIImage.**

## UIGraphicsImageRenderer Works Together With Core Graphics Low-Level APIs


## Demo Project

```swift
import SwiftUI

struct ContentView: View {
    
    var body: some View {
        
        VStack {
            
            Text("UIGraphicsImageRenderer Results:")
            Image(uiImage: composeUIGraphicsImageRenderer())
                .border(Color.black, width: 1)
            
            if let image = composeCoreGraphics() {
                Text("CoreGraphics Results: (not clear)")
                Image(uiImage: image)
                    .border(Color.black, width: 1)
            }
        }
    }
    
    // - MARK: Create images by UIGraphicsImageRenderer
    
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
    
    func composeUIGraphicsImageRenderer() -> UIImage {
        
        let img1 = image1()
        let img2 = image2()
        
        return UIGraphicsImageRenderer(size: CGSize(width: 200, height: 200)).image { ctx in
            img1.draw(at: CGPoint(x: (200 - img1.size.width) / 2,
                                  y: (200 - img1.size.height) / 2))
            img2.draw(at: CGPoint(x: (200 - img2.size.width) / 2,
                                  y: (200 - img2.size.height) / 2))
        }
    }
    
    // - MARK: Create images by Core Graphics Low-Level APIs
    
    func image3() -> UIImage? {
        
        let drawSize = CGSize(width: 100, height: 100)
        UIGraphicsBeginImageContextWithOptions(drawSize, false, 0)
        // Release the image context when the function returns
        defer { UIGraphicsEndImageContext() }
        // Get current image context, and care about exceptions
        guard let ctx = UIGraphicsGetCurrentContext() else {
            fatalError("Failed to get current context...")
        }
        
        // The same logics as `image1`...
        ctx.setFillColor(UIColor.red.cgColor)
        ctx.fill(CGRect(x: 50, y: 50, width: 50, height: 50))
        ctx.setStrokeColor(UIColor.red.cgColor)
        ctx.stroke(CGRect(x: 0, y: 0, width: 100, height: 100))
        
        // Return the final image, but the result is optional...
        return UIGraphicsGetImageFromCurrentImageContext()
    }
    
    func image4() -> UIImage? {
        
        let drawSize = CGSize(width: 50, height: 50)
        UIGraphicsBeginImageContextWithOptions(drawSize, false, 0)
        // Release the image context when the function returns
        defer { UIGraphicsEndImageContext() }
        // Get current image context, and care about exceptions
        guard let ctx = UIGraphicsGetCurrentContext() else {
            fatalError("Failed to get current context...")
        }
        
        // The same logics as `image4`...
        ctx.setFillColor(UIColor.green.cgColor)
        ctx.fill(CGRect(x: 0, y: 0, width: 25, height: 25))
        ctx.setStrokeColor(UIColor.green.cgColor)
        ctx.stroke(CGRect(x: 0, y: 0, width: 50, height: 50))
        
        // Return the final image, but the result is optional...
        return UIGraphicsGetImageFromCurrentImageContext()
    }
    
    func composeCoreGraphics() -> UIImage? {
        
        let drawSize = CGSize(width: 200, height: 200)
        UIGraphicsBeginImageContextWithOptions(drawSize, false, 0)
        // Release the image context when the function returns
        defer { UIGraphicsEndImageContext() }
        
        if let img1 = image3(), let img2 = image4() {
            
            img1.draw(at: CGPoint(x: (200 - img1.size.width) / 2,
                                  y: (200 - img1.size.height) / 2))
            img2.draw(at: CGPoint(x: (200 - img2.size.width) / 2,
                                  y: (200 - img2.size.height) / 2))
        }
        
        // Return the final image, but the result is optional...
        return UIGraphicsGetImageFromCurrentImageContext()
    }
}

// Previews
struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

## Screenshot

![Screenshot](/assets/2021-04-25-create-image-uigraphicsimagerenderer.png)

## Conclusion

`UIGraphicsImageRenderer` is easier to use than Core Graphics Low-Level APIs.
