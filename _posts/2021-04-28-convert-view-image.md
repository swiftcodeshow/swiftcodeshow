---
layout: post
title: Convert View to Image in SwiftUI
description: Convert a view to an image, usually we need do that in an editor app.
repository: ConvertViewImage
---

## Topics

Convert a view to an image, usually we need do that in an editor app.

## Codes

```swift
extension View {
    
    func snapshot() -> UIImage {
        let controller = UIHostingController(rootView: self)
        let view = controller.view
        
        let targetSize = controller.view.intrinsicContentSize
        view?.bounds = CGRect(origin: .zero, size: targetSize)
        view?.backgroundColor = .clear
        
        let renderer = UIGraphicsImageRenderer(size: targetSize)
        
        return renderer.image { _ in
            view?.drawHierarchy(in: controller.view.bounds, afterScreenUpdates: true)
        }
    }
}

struct ContentView: View {
    
    @State var showRectangle = true
    
    @State var showCircle = true
    
    @State var image: UIImage?
    
    var body: some View {
        
        VStack {
            
            // Declare view but not be embed right now,
            // because we hold it with a variable
            let canvas = HStack {
            
                if showRectangle {
                    
                    Rectangle()
                        .fill()
                        .foregroundColor(.green)
                }
            
                if showCircle {
                 
                    Circle()
                        .fill()
                        .foregroundColor(.red)
                }
            }
            // set size of the destination view here not later,
            // otherwise you will get wrong size of your image
            .frame(width: 300, height: 300)
            
            HStack {
                
                Button("\(showRectangle ? "Hide" : "Show") a Rectangle") {
                    showRectangle.toggle()
                }
                .background(Color.blue)
                .foregroundColor(.white)
                
                Button("\(showCircle ? "Hide" : "Show") a Circle") {
                    showCircle.toggle()
                }
                .background(Color.blue)
                .foregroundColor(.white)
                
                Button("Generate final image") {
                    image = canvas.snapshot()
                }
                .background(Color.red)
                .foregroundColor(.white)
            }
            
            canvas
            
            Text("Your generated image below: ")
            
            if let img = image {
                
                Image(uiImage: img)
                    .frame(width: 300, height: 300)
            }
        }
    }
}

```

## Screenshots

![Convert View Image](/assets/2021-04-28-convert-view-image.gif)

## Key Points

1. Add a method called `snapshot` for `View`, in `snapshot` we put the destination SwiftUI view into a UIKit view controller to fetch its content size.
1. We use a local variable to hold a SwiftUI view but not insert it into its parent view immediately.

## Further Reading

* [Create an Image by UIGraphicsImageRenderer](https://swiftcodeshow.com/2021/04/25/create-image-uigraphicsimagerenderer.html)
