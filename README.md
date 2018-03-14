# ImageEdit

--------------

![Preview](https://github.com/Eratos1122/ImageEdit/blob/master/mask.png)
![Preview](https://github.com/Eratos1122/ImageEdit/blob/master/border.png)
![Preview](https://github.com/Eratos1122/ImageEdit/blob/master/mask and merge with border.png)

Usage
--------------

mergedImage = mergeImage(topImage: UIImage(named: "border"), bottomImage: photo)

maskedImage = maskImage(image: mergedImage, mask: UIImage(named: "mask"))
