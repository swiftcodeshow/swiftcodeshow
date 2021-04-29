---
layout: post
title: Use UIImagePickerController to Pick an Image in SwiftUI
description: Use UIViewControllerRepresentable to integrate UIImagePickerController to pick an image from user's photos library in SwiftUI.
repository: UIImagePickerControllerPickImage
---

## Topics

Use `UIViewControllerRepresentable` to integrate `UIImagePickerController` to pick an image from user's photos library in SwiftUI.

## Codes

Create a custom image picker called `CustomImagePicker`

```swift
struct CustomImagePicker: UIViewControllerRepresentable {
    
    @Environment(\.presentationMode) var presentationMode
    
    @Binding var image: UIImage?
    
    func makeCoordinator() -> Coordinator {
        Coordinator(self)
    }
    
    func makeUIViewController(context: UIViewControllerRepresentableContext<CustomImagePicker>) -> UIImagePickerController {
        let picker = UIImagePickerController()
        picker.delegate = context.coordinator
        return picker
    }
    
    func updateUIViewController(_ uiViewController: UIImagePickerController, context: UIViewControllerRepresentableContext<CustomImagePicker>) {
        
    }
    
    class Coordinator: NSObject, UINavigationControllerDelegate, UIImagePickerControllerDelegate {
        
        let parent: CustomImagePicker

        init(_ parent: CustomImagePicker) {
            
            self.parent = parent
        }
        
        func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
            if let uiImage = info[.originalImage] as? UIImage {
                    parent.image = uiImage
                }

                parent.presentationMode.wrappedValue.dismiss()
        }
    }
}
```

`CustomImagePicker` usage

```swift
import SwiftUI

struct ContentView: View {
    
    @State var image: UIImage?
    
    @State var show = false
    
    var body: some View {
        
        VStack {
            
            Button("Pick an image") {
                
                show.toggle()
            }
            
            if let img = image {
                Image(uiImage: img)
                .resizable()
                .aspectRatio(contentMode: .fit)
            }
        }
        .sheet(isPresented: $show) {
            
            CustomImagePicker(image: $image)
        }
    }
}
```

## Screenshots

![UIImagePickerController Pick Image](/assets/2021-04-28-uiimagepickercontroller-pick-image.gif)

## Key Points

1. `CustomImagePicker` implements `UIViewControllerRepresentable` not `UIViewRepresentable`.
1. `UIImagePickerController` has its own navigation controller, so do not try to navigate to it by `NavigationLink`, otherwise you will find two navigation bar in the `CustomImagePicker`.
