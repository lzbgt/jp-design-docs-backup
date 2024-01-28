### 简介

cython可以将python代码编译成系统的动态库, 及windows下为.DLL (.pyd), linux下为.SO等, 实现和C/C++编写软件同等效果, 不至于暴露源代码.

### 步骤

#### 安装CYTHON

```bash
pip install cython
```

#### 为项目添加编译脚本

```python
## filename: compile.py
from distutils.core import setup
from distutils.extension import Extension
from Cython.Distutils import build_ext
ext_modules = [
    Extension("learn", ["learn.py"]),
    Extension("data", ["data.py"]),
    #   ... all your modules that need be compiled ...
]
setup(
    name='o2',
    cmdclass={'build_ext': build_ext},
    ext_modules=ext_modules
)
```

其中 &quot;ext\_modules&quot; 是所有要编译的python文件, 编译后再windows下会生成对应的.pyd文件,可以直接用来替换原先的.py文件

#### 编译

```bash
python compile.py build_ext --inplace
```