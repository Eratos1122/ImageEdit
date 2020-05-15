# ImageEdit

--------------

![Preview](https://github.com/Eratos1122/ImageEdit/blob/master/mask1.png)
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
And Mask Image should consist of only two colors - white & black.

Code
--------------

![Code](https://github.com/Eratos1122/ImageEdit/blob/master/ImageEdit.swift)
