### Fits To Image in Python

- This program converts .fits file to .jpg file. (Or you can convert fits file to png or bmp in little modification)


### Installation

- Command <code>git clone https://github.com/psds075/fitstoimg.git</code>.
- Make new folder 'data' and 'image' in current directory, and copy .fits files to data folder. (<code>mkdir data</code>, <code>mkdir image</code>)
- Command <code>python fitstoimg.py</code>.
- In linux environment, you should revise directory character '\\' -> '/'.
- It is tested on Windows 10, Linux Ubuntu 16.04.
<pre><code>$ git clone https://github.com/psds075/fitstoimg.git
$ mkdir data
$ mkdir image
$ python3 fitstoimg.py
</code></pre>

### Code (fitstoimg.py)

<pre><code>import os
import numpy as np
from PIL import Image
from astropy.io import fits
import img_scale

dir_path = os.getcwd()
for filename in os.listdir(dir_path+"\\data"):
    if filename.endswith(".fits"):
        image_data = fits.getdata("data\\"+filename)
        if len(image_data.shape) == 2:
            sum_image = image_data
        else:
            sum_image = image_data[0] - image_data[0]
            for single_image_data in image_data:
                sum_image += single_image_data  

        sum_image = img_scale.sqrt(sum_image, scale_min=0, scale_max=np.amax(image_data))
        sum_image = sum_image * 200
        im = Image.fromarray(sum_image)
        if im.mode != 'RGB':
            im = im.convert('RGB')
    
        im.save(dir_path+"\\image\\"+filename+".jpg")
        im.close()
</code></pre>


### Contact
- Dong Hyun Kim (Korea) : psds075@gmail.com
