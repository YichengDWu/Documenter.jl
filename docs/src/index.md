# Documenter.jl

*Julia的文档生成器。*

一个从docstrings和markdown文件构建文档的包。

!!! 注意

    如果这是您第一次使用Julia的文档系统，请先阅读主Julia手册的[文档](https://docs.julialang.org/en/v1/manual/documentation/)部分。一旦您阅读了如何为您的代码编写文档，就可以回到这里。

## 包功能

- 在[Markdown](https://en.wikipedia.org/wiki/Markdown)中编写所有文档。
- 最小化配置。
- 对Julia代码块进行Doctests。
- 用于文档和部分标题的交叉引用。
- [``\LaTeX``语法](@ref latex_syntax)支持。
- 检查缺失的docstrings和不正确的交叉引用。
- 生成目录和docstring索引。
- 自动从Travis构建并部署文档到GitHub Pages。

[包指南](@ref)提供了一个教程，解释如何开始使用Documenter。

一些使用Documenter的包的示例可以在[示例](@ref)页面上找到。

完整的函数和类型文档列表，请参见[索引](@ref main-index)。

## 手册大纲

```@contents
Pages = [
    "man/guide.md",
    "man/examples.md",
    "man/syntax.md",
    "man/doctests.md",
    "man/hosting.md",
    "man/latex.md",
    "man/contributing.md",
]
Depth = 1
```

## 库大纲

```@contents
Pages = ["lib/public.md", "lib/internals.md"]
```

### [索引](@id main-index)

```@index
Pages = ["lib/public.md"]
```
