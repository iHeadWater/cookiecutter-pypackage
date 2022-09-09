cookiecutter-pypackage
======================


概述
====

一个通过cookiecutter标准化生成python工程的模板，参考了https://github.com/Nekroze/cookiecutter-pypackage

会自动根据python docstring（三双引号注释）在readthedocs生成mkdocs文档，以及自动发布到pypi

修改
====

相较于参考模板，主体部分{{cookiecutter.repo_name}}去除了无用的travis.yml（持续集成）、tox.ini（矩阵测试）、rst文件，添加了mkdocs.yml配置文件和满足mkdocs所需依赖的requirements.txt文件，docs中没有具体文档，只有自动生成python文档的脚本gen_ref_pages.py，参考https://mkdocstrings.github.io/recipes/#generate-pages-on-the-fly

cookiecutters.json中去掉了一些配置项，添加 "_copy_without_render": [".github/workflows/*.yml"] 配置项，以应对含有{{}}项的github workflow文件；project_name用以指定IDE中工程名，repo_name用以指定主文件夹名和GitHub库名

使用
====

对于示例工程而言流程非常简单。

1.打开终端，输入pip install -U(-U表当前用户，也可不加）cookiecutter

2.等待安装完毕后，输入cookiecutter https://github.com/iHeadWater/cookiecutter-pypackage.git
便会自动下载模板，画面大致如下：
![图片](https://user-images.githubusercontent.com/23413915/189249796-f2849bff-f79b-433a-9c0f-d3bb46ebab19.png)
从这里可以看到，github_username及以下内容与cookiecutter.json指定的内容完全一致，一般情况下可以一路回车，当然也可以输入自己指定的名称（例如指定repo_name为calc-test-repo2，接着{{cookiecutter.repo_name}}等配置项便会被用户指定名称替换)

3.之后cookiecutter便会在*当前命令执行路径*下载模板，并完成整个工程的构建。

展示
====
![图片](https://user-images.githubusercontent.com/23413915/189249993-8e7aaafa-f949-43b4-be75-78ca6ec71158.png)

上图为GitHub上示例工程的结构。

.github/workflows内部是持续集成所用工作流，可在代码发生变动时，自动构建并通过twine上传到testpypi，需要在github repository的settings→secrets→actions下自行添加"TEST_PYPY_TOKEN"项，填入事先在test pypi上申请的api token。注意：由于TEST_PYPI_TOKEN名称唯一，所以同一个人、同一个模板生成的所有库都应采取相同token。

.readthedocs.yaml则告知readthedocs网站如何显示文档，详情可参见[此文档](https://dlut-water.yuque.com/kgo8gd/tnld77/qu11r6)，docs则用以给readthedocs（rtd）做标示，存储文档或者相关脚本，但不能没有。

calc-test-repo（repo_name修改而来，通常是用户指定的名字）存放主要业务代码，平时开发工作均在其中进行，test用来编写一些测试。

MANIFEST.in是打包文件（用以包含一些特定需要的文件）。

requirements.txt是pip所需的依赖管理文件，pip会根据此文件中配置，为用户自动下载安装所需依赖项（参见此文档，值得提及的是，intellij idea ultimate/pycharm用户可以安装requirement插件，当用户引入依赖时，该插件会自动将依赖写入requirements.txt文件，免去重复添加的麻烦，但environment.yml无相应支持）。

setup.py中指定python工程的基本信息（Metadata，元数据），直接决定了推送到pypi上的构建包名字：
![图片](https://user-images.githubusercontent.com/23413915/189250746-c35a675e-e9a5-4ecb-970d-b17922476505.png)
![图片](https://user-images.githubusercontent.com/23413915/189250776-33482e4d-9cbb-4e75-9f91-7942f27adb4c.png)

（注意calc-test-repo这个名字，与setup.py和示例工程中cookiecutters.json/repo_name完全一致，实际可以根据需求再调整）


结语
====
一个典型的cookiecutters python库架构大概如此，使用时可以根据需求进一步调整，例如将tox.ini和pyproject.toml文件加回去，使用tox对模块进行矩阵化测试。
