# 学习写代码规范

程序员最讨厌的四件事：写注释、写文档、别人不写注释、别人不写文档\~

## 1. 格式规范

### 1.1 代码规范标准

#### 1.1.1 [PEP 8](https://www.python.org/dev/peps/pep-0008/) —— Python 官方代码规范

Python 官方的代码风格指南，包含了以下几个方面的内容：

1. 代码布局，介绍了 Python 中空行、断行以及导入相关的代码风格规范。比如一个常见的问题：当我的代码较长，无法在一行写下时，何处可以断行？
2. 表达式，介绍了 Python 中表达式空格相关的一些风格规范。
3. 尾随逗号相关的规范。当列表较长，无法一行写下而写成如下逐行列表时，推荐在末项后加逗号，从而便于追加选项、版本控制等。

```python
# Correct:
FILES = ['setup.cfg', 'tox.ini']
# Correct:
FILES = [
    'setup.cfg',
    'tox.ini',
]
# Wrong:
FILES = ['setup.cfg', 'tox.ini',]
# Wrong:
FILES = [
    'setup.cfg',
    'tox.ini'
]
```

4. 命名相关规范、注释相关规范、类型注解相关规范，我们将在后续章节中做详细介绍。

> A style guide is about consistency. Consistency with this style guide is important. Consistency within a project is more important. Consistency within one module or function is the most important.

> _PEP 8 -- Style Guide for Python Code_

一个需要注意的地方是，PEP 8 的代码规范并不是绝对的，项目内的一致性要优先于 PEP 8 的规范。OpenMMLab 各个项目都在 `setup.cfg` 设定了一些代码规范的设置，请遵照这些设置。一个例子是在 PEP 8 中有如下一个例子：

```python
# Correct:
hypot2 = x*x + y*y
# Wrong:
hypot2 = x * x + y * y
```

这一规范是为了指示不同优先级，但 OpenMMLab 的设置中通常没有启用 yapf 的 `ARITHMETIC_PRECEDENCE_INDICATION` 选项，因而格式规范工具不会按照推荐样式格式化，以设置为准。

#### 1.1.2 [Google 开源项目风格指南](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/contents/)

Google 使用的编程风格指南，包括了 Python 相关的章节。相较于 PEP 8，该指南提供了更为详尽的代码指南。该指南包括了语言规范和风格规范两个部分。

其中，语言规范对 Python 中很多语言特性进行了优缺点的分析，并给出了使用指导意见，如异常、Lambda 表达式、列表推导式、metaclass 等。 风格规范的内容与 PEP 8 较为接近，大部分约定建立在 PEP 8 的基础上，也有一些更为详细的约定，如函数长度、TODO 注释、文件与 socket 对象的访问等。

推荐将该指南作为参考进行开发，但不必严格遵照，一来该指南存在一些 Python 2 兼容需求，例如指南中要求所有无基类的类应当显式地继承 Object, 而在仅使用 Python 3 的环境中，这一要求是不必要的，依本项目中的惯例即可。二来 OpenMMLab 的项目作为框架级的开源软件，不必对一些高级技巧过于避讳，尤其是 MMCV。但尝试使用这些技巧前应当认真考虑是否真的有必要，并寻求其他开发人员的广泛评估。&#x20;

另外需要注意的一处规范是关于包的导入，在该指南中，要求导入本地包时必须使用路径全称，且导入的每一个模块都应当单独成行，通常这是不必要的，而且也不符合目前项目的开发惯例，此处进行如下约定：

```python
# Correct
from mmcv.cnn.bricks import (Conv2d, build_norm_layer, DropPath, MaxPool2d,
                             Linear)
from ..utils import ext_loader

# Wrong
from mmcv.cnn.bricks import Conv2d, build_norm_layer, DropPath, MaxPool2d, \
                            Linear  # 使用括号进行连接，而不是反斜杠
from ...utils import is_str  # 最多向上回溯一层，过多的回溯容易导致结构混乱
```

### 1.2 格式规范工具

#### 1.2.1 [flake8](http://flake8.pycqa.org/en/latest/)

Flake8 是 Python 官方发布的一款 Python 代码规范检测工具，是以下三种工具的一个封装：

* PyFlakes：该工具用以检查 Python 代码是否存在简单的逻辑错误，如某一个变量没有被使用，某一部分代码无法触达等。
* pycodestyle： 该工具用以检查 Python 代码是否符合 PEP 8 规定的代码风格。
* Ned Batchelder's McCabe script：该工具用以分析 Python 的[圈复杂度](https://en.wikipedia.org/wiki/Cyclomatic\_complexity)，圈复杂度通常用以衡量代码的逻辑结构是否过于复杂难以维护，flake8 中默认不会启用相关检查。

#### 1.2.2  [yapf](https://github.com/google/yapf)

Yapf 是另一款代码规范检测工具，由 Google 进行开发维护，作为 flake8 的补充，会在 OpenMMLab 各项目开发的 pre-commit hook 中执行。

## 2. 命名规范





## 3. Docstring 规范







## 4. 注释规范







## 5. 类型注解





## 6. 参考资料



