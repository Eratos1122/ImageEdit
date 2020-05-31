# ImageEdit

Mask, Border, Merge
--------------

![Preview](https://github.com/Eratos1122/ImageEdit/blob/master/mask.png)
![Preview](https://github.com/Eratos1122/ImageEdit/blob/master/border.png)
![Preview](https://github.com/Eratos1122/ImageEdit/blob/master/maskandmergewithborder.png)

Usage
--------------
```swift
mergedImage = mergeImage(topImage: UIImage(named: "border"), bottomImage: photo)

maskedImage = maskImage(image: mergedImage, mask: UIImage(named: "mask"))
```

Important
--------------

The <Gray> part of border image should be transparent for working.
And the mask image should consist of only two colors - white & black.

Code
--------------

- Crop Image To Square
```swift
func cropImageToSquare(image: UIImage) -> UIImage? {
    var imageHeight = image.size.height
    var imageWidth = image.size.width
    
    if imageHeight > imageWidth {
        imageHeight = imageWidth
    }
    else {
        imageWidth = imageHeight
    }
    
    let size = CGSize(width: imageWidth, height: imageHeight)
    
    let refWidth : CGFloat = CGFloat(image.cgImage!.width)
    let refHeight : CGFloat = CGFloat(image.cgImage!.height)
    
    let x = (refWidth - size.width) / 2
    let y = (refHeight - size.height) / 2
    
    let cropRect = CGRect.init(x: x, y: y, width: size.height, height: size.width)
    if let imageRef = image.cgImage!.cropping(to: cropRect) {
        return UIImage.init(cgImage: imageRef, scale: 0, orientation: image.imageOrientation)
    }
    
    return nil
}
```
- Get Aspect Fit Frame
```swift
func getAspectFitFrame(sizeImgView:CGSize, sizeImage:CGSize) -> CGRect{
    
    let imageSize:CGSize  = sizeImage
    let viewSize:CGSize = sizeImgView
    
    let hfactor : CGFloat = imageSize.width/viewSize.width
    let vfactor : CGFloat = imageSize.height/viewSize.height
    
    let factor : CGFloat = max(hfactor, vfactor)
    
    // Divide the size by the greater of the vertical or horizontal shrinkage factor
    let newWidth : CGFloat = imageSize.width / factor
    let newHeight : CGFloat = imageSize.height / factor
    
    var x:CGFloat = 0.0
    var y:CGFloat = 0.0
    if newWidth > newHeight{
        y = (sizeImgView.height - newHeight)/2
    }
    if newHeight > newWidth{
        x = (sizeImgView.width - newWidth)/2
    }
    let newRect:CGRect = CGRect(x: x, y: y, width: newWidth, height: newHeight)
    
    return newRect
}
```
- Mask Image
```swift
func maskImage(image: UIImage, mask: UIImage) -> UIImage{
    let size = image.size
    UIGraphicsBeginImageContext(size)
    let rect = getAspectFitFrame(sizeImgView: size, sizeImage: mask.size)
    mask.draw(in: rect, blendMode: .normal, alpha: 1.0)
    
    let newImage:UIImage = UIGraphicsGetImageFromCurrentImageContext()!
    UIGraphicsEndImageContext()
    
    let imageReference = image.cgImage
    let maskReference = newImage.cgImage
    
    let imageMask = CGImage(maskWidth: maskReference!.width,
                            height: maskReference!.height,
                            bitsPerComponent: maskReference!.bitsPerComponent,
                            bitsPerPixel: maskReference!.bitsPerPixel,
                            bytesPerRow: maskReference!.bytesPerRow,
                            provider: maskReference!.dataProvider!, decode: nil, shouldInterpolate: true)
    
    let maskedReference = imageReference!.masking(imageMask!)
    
    let maskedImage = UIImage.init(cgImage: maskedReference!)
    
    return maskedImage
}
```
- Merge Image
```swift
func mergeImage(topImage:UIImage, bottomImage: UIImage) -> UIImage {
    let size = bottomImage.size
    UIGraphicsBeginImageContext(size)
    
    let areaSize = CGRect(x: 0, y: 0, width: size.width, height: size.height)
    bottomImage.draw(in: areaSize)
    
    topImage.draw(in: getAspectFitFrame(sizeImgView: size, sizeImage: topImage.size), blendMode: .normal, alpha: 1.0)
    
    let newImage:UIImage = UIGraphicsGetImageFromCurrentImageContext()!
    UIGraphicsEndImageContext()
    
    return newImage
}
```
