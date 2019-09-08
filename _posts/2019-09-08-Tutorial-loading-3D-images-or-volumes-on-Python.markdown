---
layout: post
title: Tutorial loading 3D/4D images or volumes on Python
image: 8.jpg
date: 2019-09-08 17:00:20 +0200
tags:
categories: Tutorial 3DImages
---
I encountered myself with more difficulties that I expected when loading a 3D image on Python. In my case my purpose was to load a 3D image obtained through X-Ray computer tomography to apply segmentation techniques and other related algorithms. In the following lines I will comment a bit of my research process to be able to work with this kind of files. This is the beginning of my Master thesis and I have the advantage that I could pick the program or lenguage that suit the best projects requirements but the reader may have different origin data or is constrained for one tool. That is why I will not assume project is to be done in Python, but its strengths will be hightlighted.

#### Data origin
The first we must consider is how is the data we want to work with and understand some of its technical concepts. In my case I had a 3D volume image in .raw format with 8 unsigned bit pixels. If you do not have a clear idea of what unsigned pixels stand for you can check it [here](https://gisgeography.com/unsigned-integer-signed-integer-raster-data/). 3D image can be missunderstood as a 2D image with three channels of color (RGB usually) in this blog 3D image will be always referred to 3D grey picture unless specified. One important detail is that my raw file is a stack of images, one single file not containing several images in slices, this kind of file is called multi-page image. 

#### Comparison Python vs Matlab
At the time of choosing the coding lenguage to work with probably you need to consider several arguments. It seems Matlab has been the standard for image processing although I do not know how much competency python is doing nowadays. This means that companies or research groups have their scripts and functions developed mostly in matlab, that was my case for example. my work was not much dependent on what was done before also I chose python because I do not want to deal with education licences and several installations in 3 or 4 computers located in different places where I work. At the time of comparing 3D functionalities between Python and Matlab is difficult to state a winner and probably it depends a lot on what you want to do with the images. 

I basically choose python because of the following reasons:
1. Python has libraries for almost anything: It is possible to treat the image, open a third 3D viewer software, obtain properties from the volume and store them in a relational database.. all from its environment. 
2. Personally I prefer coding in python since it is cleaner than matlab. Functions in matlab need to be a different file which I find terribly tedious.
3. Open source: which translates into better doubts support (for example stack overflow) incredibly important for a noob like me. Also it means more easiness to comment and share the code with anyone.
4. Machine learning methods: In the (far yet) future the intention is to apply statistical models to the images and python is one step ahead of Matlab in that.

#### Libraries
Ok, so we have decided to work with python, I am afraid to tell you that was the easiest choice. Problem is which library are we going to use to work with our images? I found these are the most common libraries to work with images:

1. **GDAL:** Library used for geo-spatial data. It is not native from python. You can check more information [here](https://gdal.org/). I coud not load sucessfully my 3D stack using this library, the main problem was trying to load all the pages from my image, I could only access to the first one. Probably it can be used, I just took another way that I found easier.
2. **Rasterio:** It is another geospatial library, same as before I could not work with the multipage image. I guess it could be used too, I just did not find the way.
3. **OpenCV:** It is the library to work with images and machine learning, its main problem is that is not 3D supported.
4. **PIL:** (Python Image Library): Finally the one that solve our needs. Its documentation is super clear and easy to follow: (https://pillow.readthedocs.io/en/stable/)

Just one detail before we get to the code, PIL does not support .raw files, we are forced to use a third image software to transform it to a readable format as .tif. I would suggest ImageJ which can be downloaded [here](https://imagej.nih.gov/ij/download.html) which is an open source image software easy to use. In other posts you will see it can serve as 3D viewer to inspect segmentation methods in a volume. Saving the raw file as a tif is immediate. 



###### Ok, let's start to code!

{% highlight python %}
from PIL import Image
import numpy as np
from pathlib import Path

{% endhighlight %} 
* Import PIL to load the image.
* Numpy to be able to work with the image as an array.
* Pathlib to make easier to work with paths to images and working directories.


{% highlight python %}
def read_tiff(pathlibpath,nframes='all'):
    '''
    Read a multiframe tif image and loads it in a numpy array
    '''
    img = Image.open(str(pathlibpath), mode='r')
    stack = []
    if nframes == 'all':
        for i in range(img.n_frames):
            img.seek(i)
            stack.append(np.array(img))
    else:
        for i in range(nframes):
            img.seek(i)
            stack.append(np.array(img))
    img.close() 

    return np.transpose(np.array(stack), (2, 1, 0))
{% endhighlight %} 
* First python opens the image from the path you specify in the "read" mode, it must be a string, not a path object.
* Then initializes the variable "stack" which is to store the 3D array.
* If condition to make easy to pick all the slices or just one specified number from the multi-page tif.
* Seek serves as the pagemarker inside the loop to keep track###### which slice is to save at each time.
* Finally it saves the slice as a numpy array. Once the loop is over the function returns the stack reshaped. The change in the dimensions is just to keep consistency because ImageJ and Python have different origin coordinates.

Let's try our function:
{% highlight python %}
directorio = Path("C:/yourpath")
imagen1 = 'tif_image.tif'
ruta1 = directorio / imagen1	
prueba1 = read_tiff(ruta1,100) # for 100 slices
{% endhighlight %} 

* Make sure the path is written with "/" otherwise it will not work. 

And that was it. Simple code but with some details that can make you loose a lot of time as the open and seek or the pathlib path using "/" and calling it as a string.

Thanks, for reading, if you have any doubt, comment or critic just let me know by a mail in my contact area. 
Please forgive any grammar mistake or mispelled word and let me know about them!
see you in nexts post.

### Happy Coding!


