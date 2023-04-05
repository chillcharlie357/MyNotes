
# opencv-python安装

conda和pip包名称不一样。
```shell
conda install opencv
```

```shell
pip install opencv-python
```

这个就是在TACTIC里注释掉的`cv2`

检查安装是否成功：
```python
import cv2
print(cv2.__version__)
```

# Google

The error message indicates that there is an issue with importing some modules in the code you are running. Specifically, there seems to be a problem with the Google Protocol Buffers library that is used by TensorFlow and Keras.

One possible cause of this error is that the Google Protocol Buffers library was not installed or is not properly configured. You could try reinstalling the library by running the following command in your terminal:

cssCopy code

`pip install --upgrade protobuf`

Another possible cause is that there is a conflict between different versions of the Google Protocol Buffers library. You could try uninstalling the current version of the library and installing an older or newer version by running the following commands:

Copy code

`pip uninstall protobuf pip install protobuf==3.11.3`

You could also try upgrading TensorFlow and Keras to the latest versions to ensure compatibility with the latest version of the Google Protocol Buffers library:

cssCopy code

`pip install --upgrade tensorflow keras`

If none of these solutions work, you could try searching for similar issues on the TensorFlow and Keras GitHub repositories or posting a question on a related forum or community to get help from other users and developers.