Python用Pillow(PIL)进行简单的图像操作
==================================

# 一. 图像的坐标表示

> 图像中左上角是坐标原点(0, 0)，这和平常数学里的坐标系不太一样。这样定义的坐标系意味着，X轴是从左到右增长的，而Y轴是从上到下增长。在Pillow中如何使用上述定义的坐标系表示一块矩形区域？许多函数或方法要求提供一个矩形元组参数。元组参数包含四个值，分别代表矩形四条边的距离X轴或者Y轴的距离。顺序是(左，顶，右，底)。右和底坐标稍微特殊，表示直到但不包括。可以理解为[左, 右)和[顶， 底)这样左闭右开的区间。比如(3, 2, 8, 9)就表示了横坐标范围[3, 7]；纵坐标范围[2, 8]的矩形区域。

# 二. Pillow操作图像

## (一) 读取影像

> Pillow读取影像操作如下：

```python

from PIL import Image

# file path
im_path = "/Users/shaoqi/Desktop/ZLD.jpeg"

# open image
im = Image.open(im_path)

# get width and height
width, height = im.size
print(im.size, width, height)

# 格式，以及格式的详细描述
print(im.format, im.format_description)

# 保存输出
im.save("/Users/shaoqi/Desktop/ZLD_copy.jpeg")

# 显示影像
im.show()

```

> 输出结果如下：

```
(900, 900) 900 900

JPEG JPEG (ISO 10918)

```
> im.size返回一个元组，分别是宽和高。show()方法会调用系统默认图像查看软件，打开并显示。im.format可查看图像的格式。save()可保存处理后的图片，如果未经处理，保存后的图像占用的空间(字节数)一般也与原图像不一样，可能经过了压缩。

## (二) 新建影像

> Pillow也可以新建空白图像, 第一个参数是mode即颜色空间模式，如RGB，或者采用已有的影像颜色模式；第二个参数指定了图像的分辨率(宽x高)；第三个参数是颜色：可以直接填入常用颜色的名称，如'red'；也可以填入十六进制表示的颜色，如#FF0000表示红色；还能传入元组，比如(255, 0, 0, 255)或者(255， 0， 0)表示红色。

```python

from PIL import Image

#### 'RGB' #####
newim=Image.new('RGB',(100,100),'red')
newim.show()


#### 已有的影像颜色模式 ####
# file path
im_path = "/Users/shaoqi/Desktop/ZLD.jpeg"

# open image
im = Image.open(im_path)

newim=Image.new(im.mode,(100,100),'#FF0000')

```

## （三）裁剪影像

> PIllow可以采用crop()方法接收一个矩形区域元组来实现影像裁剪，结果返回一个返回一个新的image对象，是裁剪后的结果，对原图没有影响，结果及代码如下：

```python
from PIL import Image

# file path
im_path = "/Users/shaoqi/Desktop/ZLD.jpeg"

# open image
im = Image.open(im_path)

newim=im.crop((100,300,600,600))

newim.show()

newim.save("/Users/shaoqi/Desktop/ZLD_crop.jpeg")

```

![image](https://github.com/ShaoQiBNU/Python-Pillow-PIL-/blob/master/images/ZLD.jpeg)

![image](https://github.com/ShaoQiBNU/Python-Pillow-PIL-/blob/master/images/crop.png)

## (四) 复制粘贴图像

> Image的copy函数如其名会产生一个原图像的副本，在这个副本上的任何操作不会影响到原图像。paste()方法用于将一个图像粘贴（覆盖）在另一个图像上面。谁调用它，它就在该Image对象上直接作修改。如果之后还会用到原图的信息，由于信息被改变就很麻烦。所以paste前最好使用copy()复制一个副本，在此副本操作，不会影响到原图信息。虽然在程序里原图信息已改变，但由于保存文件时用的其他文件名，相当于改变没有生效，所以查看的时候原图还是没有改变的。结果及代码如下：

```python
from PIL import Image

# file path
im_path = "/Users/shaoqi/Desktop/ZLD.jpeg"

# open image
im = Image.open(im_path)

# crop
newim=im.crop((100,300,600,600))

# copy
copyIm = im.copy()

# paste
copyIm.paste(newim, (0, 0))

# save
copyIm.save("/Users/shaoqi/Desktop/paste.jpeg")

# show
copyIm.show()

```

![image](https://github.com/ShaoQiBNU/Python-Pillow-PIL-/blob/master/images/paste.jpeg)

## (五) 调整图像大小

> resize方法返回指定宽高度的新Image对象，接受一个含有宽高的元组作为参数。宽高的值得是整数。

```python
from PIL import Image

# file path
im_path = "/Users/shaoqi/Desktop/ZLD.jpeg"

# open image
im = Image.open(im_path)

# size
width, height = im.size

# resize
resizedIm = im.resize((width, height*2))

resizedIm.save("/Users/shaoqi/Desktop/resize.jpeg")

resizedIm.show()

```

![image](https://github.com/ShaoQiBNU/Python-Pillow-PIL-/blob/master/images/resize.png)

## （六）旋转和翻转影像

> rotate()返回旋转后的新Image对象, 保持原图像不变。逆时针旋转。

```python

from PIL import Image

# file path
im_path = "/Users/shaoqi/Desktop/ZLD.jpeg"

# open image
im = Image.open(im_path)

# 20°
im.rotate(20).save('/Users/shaoqi/Desktop/rotate20.png')

# 90°
im.rotate(90).save('/Users/shaoqi/Desktop/rotate90.png')

# 180°
im.rotate(180).save('/Users/shaoqi/Desktop/rotate180.png')

# 270°
im.rotate(270).save('/Users/shaoqi/Desktop/rotate270.png')

```
![image](https://github.com/ShaoQiBNU/Python-Pillow-PIL-/blob/master/images/rotate20.png)
![image](https://github.com/ShaoQiBNU/Python-Pillow-PIL-/blob/master/images/rotate90.png)
![image](https://github.com/ShaoQiBNU/Python-Pillow-PIL-/blob/master/images/rotate180.png)
![image](https://github.com/ShaoQiBNU/Python-Pillow-PIL-/blob/master/images/rotate270.png)

> 图像的镜面翻转。transpose()函数可以实现，必须传入Image.FLIP_LEFT_RIGHT或者Image.FLIP_TOP_BOTTOM，第一个是水平翻转，第二个是垂直翻转。

```python
from PIL import Image

# file path
im_path = "/Users/shaoqi/Desktop/ZLD.jpeg"

# open image
im = Image.open(im_path)

im.transpose(Image.FLIP_LEFT_RIGHT).save("/Users/shaoqi/Desktop/transepose_lr.png")

im.transpose(Image.FLIP_TOP_BOTTOM).save("/Users/shaoqi/Desktop/transepose_tb.png")

```

![image](https://github.com/ShaoQiBNU/Python-Pillow-PIL-/blob/master/images/transepose_lr.png)
![image](https://github.com/ShaoQiBNU/Python-Pillow-PIL-/blob/master/images/transepose_tb.png)


# 三. 利用Pillow实现影像切分

> 将一张图片填充为正方形后切为9张图，9张图之间留有白色缝隙，然后保存输出，代码入下：

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
            
            ######## 每个item的坐标范围：左 上 右 下 ########
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
    
    ######## 显示影像 ########
    result.show()



######## main ########
if __name__ == '__main__':
    
    ######## file ########
    file_path = "/Users/shaoqi/Desktop/ZLD.jpeg"
    
    ######## open image ########
    image = Image.open(file_path) 
    
    ######## 显示影像 ########
    image.show()
    
    ######## fill image ########
    image = fill_image(image)
    
    ######## cut image ########
    image_list = cut_image(image)
    
    ######## save image ########
    save_images(image_list,image)
```

![image](https://github.com/ShaoQiBNU/Python-Pillow-PIL-/blob/master/images/result.png)

