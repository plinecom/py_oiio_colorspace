# Python OpenImageIO(OIIO) to burn LUT into OpenEXR image

# Self Introduction
I am Koike @plinecom, a pipeline engineer at digitalbigmo, Inc.

# What is this?
To OpenEXR, you want to apply a LUT file. You can do that with OpenImageIO (OIIO). OpenImageIO includes some functions of OpenColorIO (OCIO), so you can make an image with Lut applied. I will show how to do that.


# For now, the code.
```python:main.py
import OpenImageIO as oiio
import glob

if __name__ == '__main__':

  for path in glob.glob('/foo/bar/*.exr'):
    print(path)
    jpg_path = path.replace('.exr', '_lut.jpg')

    #Read File
    buf = oiio.ImageBuf(path)

    #Apply Lut File
    ImageBuf()
    isOK = oiio.ImageBufAlgo.ociofiletransform(dst, buf,
                      "/foo/bar/test.cube",
                      False)
    if not isOK:
      print("ociofiletransform error: " + dst.geterror())
      continue

    #Write File
      dst.write(jpg_path)
```

# Python external modules required.
* OpenImageIO
* NumPy

I also prepared a Dockerfile.
```Dockerfile:Dockerfile
FROM rockylinux:8
RUN dnf install -y which python3 epel-release
RUN dnf config-manager --set-enabled powertools
RUN dnf install -y python3-openimageio python3-numpy
```

```terminal:terminal
docker pull plinecom/py_oiio
```

## Code description

## Instructions for applying Lut.
```python:
  dst = oiio.ImageBuf()
  isOK = oiio.ImageBufAlgo.ociofiletransform(dst, buf,
                    "/foo/bar/test.cube",
                    False)
```
A dst temporary buffer is prepared, and the Lut file of test.cube is applied to it. The false at the end indicates that it is a reverse transformation.

```python:
  if not isOK:
    print("ociofiletransform error: " + dst.geterror())
    continue
```
Error handling. The ociofiletransform() function has strict restrictions on the format of the Lut file it accepts, so we need to handle the error to find out why it did not pass and print out the error message. This will give us a clue as to what the problem is.

# Advertising
digitalbigmo, Inc. sells skin beauty plug-ins and provides video VFX production services. If you are interested, please visit our web page. Let's work together.

https://digitalbigmo.com

# References
[OpenImageIO main house (English)](https://sites.google.com/site/openimageio/home)
[OpenImageIO Reference (English)](https://openimageio.readthedocs.io/en/latest/imagebufalgo.html#color-space-conversion)
