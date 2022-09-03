======================
cookiecutter-pypackage
======================

一个通过cookiecutter标准化生成python工程的模板，参考了https://github.com/Nekroze/cookiecutter-pypackage

相较于参考模板，主体部分{{cookiecutter.repo_name}}去除了无用的travis.yml（持续集成）、tox.ini（矩阵测试）、rst文件，添加了mkdocs.yml配置文件和满足mkdocs所需依赖的requirements.txt文件，docs中没有具体文档，只有自动生成python文档的脚本gen_ref_pages.py，参考https://mkdocstrings.github.io/recipes/#generate-pages-on-the-fly

cookiecutters.json中去掉了一些配置项，添加 "_copy_without_render": [".github/workflows/*.yml"] 配置项，以应对含有{{}}项的github workflow文件。
