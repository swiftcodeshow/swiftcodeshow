---
layout: post
title: Use UIImagePickerController to Open Camera in SwiftUI
repository: UIImagePickerControllerCamera
---

## Topics

Allow user take a new photo in your app.

## Codes

**First of all** Add *Privacy - Camera Usage Description* to *Info.plist*. Otherwise, your app will crash when you try to access camera.

Create a custom image picker.

```swift
import SwiftUI

struct CustomImagePicker: UIViewControllerRepresentable {
    
    @Environment(\.presentationMode) var presentationMode
    
    @Binding var image: UIImage?
    
    func makeCoordinator() -> Coordinator {
        Coordinator(self)
    }
    
    func makeUIViewController(context: UIViewControllerRepresentableContext<CustomImagePicker>) -> UIImagePickerController {
        let picker = UIImagePickerController()
        // This line is key point
        // However, in simulator app will crash because simulator has no camera
        picker.sourceType = .camera
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

Use `CustomImagePicker`

```swift
struct ContentView: View {
    
    @State var image: UIImage?
    
    @State var show = false
    
    var body: some View {
        
        VStack {
            
            Button("Open camera") {
                
                show.toggle()
            }
            
            if let img = image {
                Image(uiImage: img)
                .resizable()
                .aspectRatio(contentMode: .fit)
            }
        }
        .fullScreenCover(isPresented: $show) {
            
            CustomImagePicker(image: $image)
        }
    }
}
```

## Screenshots

![UIImagePickerController Camera](/assets/2021-04-28-uiimagepickercontroller-camera.gif)

## Key Points

1. Add *Privacy - Camera Usage Description* to *Info.plist*. Otherwise, your app will crash when you try to access camera.
1. Specify the source type to `.camera` for your `UIImagePickerController`.
