python切图处理
=============

# 一. python切图处理


# 二. 代码


> 代码如下：

```python
# -*- coding: utf-8 -*-
'''
将一张图片填充为正方形后切为9张图，9张图之间留有白色缝隙，然后保存输出
'''
from PIL import Image
import sys
import matplotlib.pyplot as plt


######## fill image 将图片填充为正方形 ########
def fill_image(image):
    
    ######## get image size ########
    width, height = image.size
    
    ######## 选取长和宽中较大值作为新图片的size ########
    new_image_length = width if width > height else height
    
    ######## 生成新图片[白底] ########
    new_image = Image.new(image.mode, (new_image_length, new_image_length), color='white')
    
    ######## 将之前的图粘贴在新图上，居中 ########
    if width > height:#原图宽大于高，则填充图片的竖直维度
        new_image.paste(image, (0, int((new_image_length - height) / 2)))#(x,y)二元组表示粘贴上图相对下图的起始位置
    else:
        new_image.paste(image, (int((new_image_length - width) / 2),0))
    
    return new_image


######## cut image 切图 ########
def cut_image(image):
        
    ######## get image size ########
    width, height = image.size
    
    ######## get item image size ########
    item_width = int(width / 3)
    
    ######## item image size list ########
    box_list = []
    
    ######## get item image size list 从左至右，从上至下 ########
    for j in range(0,3):
        for i in range(0,3):
            box = (j*item_width,i*item_width,(j+1)*item_width,(i+1)*item_width)
            box_list.append(box)
        
    ######## get item image ########
    image_list = [image.crop(box) for box in box_list]

    return image_list

    
######## save image ########
def save_images(image_list,image):
        
    ######## index ########
    index = 0
    
    ######## get image size ########
    width, height = image.size
    
    ######## set item image interval 设置影像间隔即白色缝隙 ########
    interval=15
    
    ######## create result image 创建新图片[白底] ########
    result = Image.new(image.mode, (width+interval*2, height+interval*2), color='white')
    
    ######## 将item图粘贴在新图上 ########
    for image in image_list:
        result.paste(image,(int(index/3)*(int(width/3)+interval),int(index%3)*(int(width/3)+interval)))
        index += 1
    
    ######## 保存输出 ########
    result.save('/Users/shaoqi/Desktop/result.png', 'PNG')



######## main ########
if __name__ == '__main__':
    
    ######## file ########
    file_path = "/Users/shaoqi/Desktop/ZLD.jpeg"
    
    ######## open image ########
    image = Image.open(file_path) 
    #image.show()
    
    ######## fill image ########
    image = fill_image(image)
    
    ######## cut image ########
    image_list = cut_image(image)
    
    ######## save image ########
    save_images(image_list,image)
```
