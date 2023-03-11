# 展示

此页面展示Documenter支持的各种页面元素。应该与其源代码(`docs/src/showcase.md`)并排阅读，以了解创建各种元素所使用的确切语法。

## 目录

可以使用[`@contents`块](@ref)生成目录。
此页面的目录呈现为

```@contents
Pages = ["showcase.md"]
```

## 基本的Markdown

Documenter可以呈现Julia Markdown解析器支持的[Markdown语法](https://docs.julialang.org/en/v1/stdlib/Markdown/)。
您可以使用所有常见的Markdown语法，例如**粗体文本**和_斜体文本_和`print("inline code")`。

## 代码块

代码块呈现如下：

```
这是一个未高亮的代码块。
...以等宽字体呈现。
```

当为块指定语言时，例如通过以````` ```julia````开头的块，内容会适当地高亮显示（支持高亮显示器支持的语言）。

```julia
function foo(x::Integer)
    @show x + 1
end
```

## 数学

对于数学，可以使用内联和显示方程。
内联方程应在两个反引号之间编写LaTeX，例如``` ``A x^2 + B x + C = 0`` ``。
它将呈现为``A x^2 + B x + C = 0``。

显示方程的LaTeX必须包含在“````` ```math `````”代码块中，并呈现为

```math
x_{1,2} = \frac{-B \pm \sqrt{B^2 - 4 A C}}{2A}
```

默认情况下，HTML输出使用[KaTeX](https://katex.org/)呈现方程，但也可以选择使用[MathJax](https://www.mathjax.org/)。

!!! 警告
    与LaTeX类似，使用`$`和`$$`来转义内联和显示方程也可以工作。但是，这样做已被弃用，此功能可能会在将来的版本中删除。

## 图像

使用基本的Markdown语法包含图像：

![输入图像的描述性标题](assets/logo.png)

路径应相对于当前文件的目录。或者，使用`./`开始相对于文档的`src`的路径，例如，`./assets/logo.png`。

## 警告

警告是用于突出显示文档的各个部分的彩色框。

每个警告均以三个`!!!`开头，然后内容在下面用四个空格缩进：
```
!!! note "一个可选的标题"
    这是您应该注意的内容。
```

Documenter支持一系列警告类型，用于不同的情况。

###### 注意警告
!!! note "'note'警告"
    警告看起来像这样。这是一个`!!! note`类型的警告。

    请注意，警告本身也可以包含其他块级元素，例如代码块。例如：

    ```julia
    f(x) = x^2
    ```

    但是，您**不能**在警告中使用at-blocks，docstrings，doctests等。

    标题是可以的：
    # 标题1
    ## 标题2
    ### 标题3
    #### 标题4
    ##### 标题5
    ###### 标题6

###### 信息警告
!!! info "'info'警告"
    这是一个`!!! info`类型的警告。这与`!!! note`类型相同。

###### 提示警告
!!! tip "'tip'警告"
    这是一个`!!! tip`类型的警告。

###### 警告警告
!!! warning "'warning'警告"
    这是一个`!!! warning`类型的警告。

###### 危险警告
!!! danger "'danger'警告"
    这是一个`!!! danger`类型的警告。

###### 兼容警告
!!! compat "'compat'警告"
    这是一个`!!! compat`类型的警告。

###### 未知警告类
!!! ukw "未知警告类"
    具有未知警告类的警告。这是一个代码示例。

## 列表

紧凑的列表如下所示

* Lorem ipsum dolor sit amet, consectetur adipiscing elit.
* Nulla quis venenatis justo.
* In non _sodales_ eros.

如果列表包含段落或其他块级元素，则如下所示：

* Morbi et varius nisl, eu semper orci。

  Donec vel nibh sapien。Maecenas ultricies mauris sapien。Nunc et sem ac justo ultricies dignissim ac vitae sem。

* Nulla molestie aliquet metus, a dapibus ligula。

  Morbi pellentesque sodales sollicitudin。Fusce semper placerat suscipit。Aliquam semper tempus ex, non efficitur erat posuere in。Fusce at orci eu ex sagittis commodo。

  > Fusce tempus scelerisque egestas。Pellentesque varius nulla a varius fringilla。

  Fusce nec urna eu orci porta blandit。

还支持编号列表

1. Lorem ipsum dolor sit amet, consectetur adipiscing elit.
2. Nulla quis venenatis justo.
3. In non _sodales_ eros.

嵌套列表也是支持的

* Morbi et varius nisl, eu semper orci。

  Donec vel nibh sapien。Maecenas ultricies mauris sapien。Nunc et sem ac justo ultricies dignissim ac vitae sem。

  - Lorem ipsum dolor sit amet, consectetur adipiscing elit。
  - Nulla quis venenatis justo。
  - In non _sodales_ eros。

* Nulla molestie aliquet metus, a dapibus ligula。

  1. Lorem ipsum dolor sit amet, consectetur adipiscing elit。
  2. Nulla quis venenatis justo。
  3. In non _sodales_ eros。

  Fusce nec urna eu orci porta blandit。

列表也可以包含在其他可以包含块级项目的块中

!!! note "警告中的项目符号列表"

    * Lorem ipsum dolor sit amet, consectetur adipiscing elit。
    * Nulla quis venenatis justo。
    * In non _sodales_ eros。

!!! note "大型警告中的列表"

    * Morbi et varius nisl, eu semper orci。

      Donec vel nibh sapien。Maecenas ultricies mauris sapien。Nunc et sem ac justo ultricies dignissim ac vitae sem。

      - Lorem ipsum dolor sit amet, consectetur adipiscing elit。
      - Nulla quis venenatis justo。
      - In non _sodales_ eros。

    * Nulla molestie aliquet metus, a dapibus ligula。

      1. Lorem ipsum dolor sit amet, consectetur adipiscing elit。
      2. Nulla quis venenatis justo。
      3. In non _sodales_ eros。

      Fusce nec urna eu orci porta blandit。

> * Morbi et varius nisl, eu semper orci。
>
>   Donec vel nibh sapien。Maecenas ultricies mauris sapien。Nunc et sem ac justo ultricies dignissim ac vitae sem。
>
>   - Lorem ipsum dolor sit amet, consectetur adipiscing elit。
>   - Nulla quis venenatis justo。
>   - In non _sodales_ eros。

## Tables

| object | implemented |      value |
|--------|-------------|------------|
| `A`    |      ✓      |      10.00 |
| `BB`   |      ✓      | 1000000.00 |

使用显式的对齐方式。

| object | implemented |      value |
| :---   |    :---:    |       ---: |
| `A`    |      ✓      |      10.00 |
| `BB`   |      ✓      | 1000000.00 |

Tables that are too wide should become scrollable.

| object | implemented |      value |
| :---   |    :---:    |       ---: |
| `A`    |      ✓      |      10.00 |
| `BBBBBBBBBBBBBBBBBBBB` | ✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓ | 1000000000000000000000000000000000000000000000000000000.00 |

## 脚注

脚注引用可以使用`[^label]`语法添加。[^1] 脚注定义会被收集在页面底部。

脚注标签可以是任意字符串，甚至可以由块级元素组成。[^Clarke61]

[^1]: 脚注定义使用块级范围中的`[^label]: ...`语法。

[^Clarke61]:
    > 任何足够先进的技术都是不可区分的魔法。
    Arthur C. Clarke，《未来的轮廓》(1961): Clarke的第三定律。

## 标题

最后，标题如下呈现

### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题

要查看一级标题的示例，请参见页面标题，要查看二级标题，请参见本段落下面的标题。

!!! note "侧边栏中的标题"
    一级和二级标题会显示在当前页面的侧边栏中。

## Docstrings

当然，Documenter的关键特性是自动包含包中的docstrings到手册中。下面的示例docstrings来自演示[`DocumenterShowcase`](@ref)模块，其源代码可以在`docs/DocumenterShowcase.jl`中找到。

要将docstrings包含在手册页面中，您需要使用一个[`@docs`块](@ref)

````markdown
```@docs
DocumenterShowcase
```
````

这将包括一个单独的docstring，它看起来像这样

```@docs
DocumenterShowcase
```

您可以逐个包含与不同函数签名对应的docstrings。例如，[`DocumenterShowcase.foo`](@ref)函数有两个签名——`(::Integer)`和`(::AbstractString)`。

````markdown
```@docs
DocumenterShowcase.foo(::Integer)
```
````

产生以下docstring

```@docs
DocumenterShowcase.foo(::Integer)
```

现在，将`DocumenterShowcase.foo(::AbstractString)`放在`@docs`块中将给出另一个docstring

```@docs
DocumenterShowcase.foo(::AbstractString)
```

但是，如果您想要，您也可以将多个docstrings组合成单个docstring块。[`DocumenterShowcase.bar`](@ref)函数具有与之相同的签名。

如果我们只将`DocumenterShowcase.bar`放入`@docs`块中，它将组合docstrings如下：

```@docs
DocumenterShowcase.bar
```

如果您有很多docstrings，您可能还要考虑使用[`@autodocs`块](@ref)，它可以根据某些过滤选项自动包含整个docstrings集合。

### docstrings索引

[`@index`块](@ref)可用于生成页面（甚至跨页面）上所有docstrings的列表，如下所示

```@index
Pages = ["showcase.md"]
```

### 相同符号的多次使用

有时一个符号有多个docstrings，例如类型定义、内部和外部构造函数。下面的示例显示了如何在文档中使用特定的docstrings。

```@docs
DocumenterShowcase.Foo
DocumenterShowcase.Foo()
DocumenterShowcase.Foo{T}()
```

## Doctesting示例

通常，您想要编写如下代码示例：

```jldoctest
julia> f(x) = x^2
f (generic function with 1 method)

julia> f(3)
9
```

如果将它们编写为````` ```jldoctest ````` 代码块，Documenter可以确保doctest没有过时。有关更多信息，请参见[Doctests](@ref)。

脚本样式的doctest也受支持：

```jldoctest
2 + 2
# output
4
```
## 运行交互式代码

[`@example`块](@ref)运行代码片段并将输出插入到文档中。例如，下面的Markdown

````markdown
```@example
2 + 3
```
````

将变成以下代码-输出块对

```@example
2 + 3
```

如果最后一个元素可以呈现为图像或`text/html`等（特定MIME类型的相应`Base.show`方法必须被定义），则它将被适当地呈现。例如：

```@example
using Main: DocumenterShowcase
DocumenterShowcase.SVGCircle("000", "aaa")
```

这在与`Markdown`标准库结合使用时非常方便

```@example
using Markdown
Markdown.parse("""
`Markdown.MD` objects can be constructed dynamically on the fly and still get rendered "natively".
""")
```

如果`@example`块中的最后一个值是`nothing`，则块的评估输出的标准输出将被显示

```@example
println("Hello World")
```

但是，请注意，如果块打印到标准输出，但也有一个最终的非`nothing`值，则标准输出会被丢弃：

```@example
println("Hello World")
42
```

### 彩色输出

[`@repl`块](@ref)和[`@example`块](@ref)的输出支持彩色输出，将ANSI颜色代码转换为HTML。

!!! compat "Julia 1.6"
    彩色输出需要Julia 1.6或更高版本。
    要启用彩色输出，请将`ansicolor=true`传递给[`Documenter.HTML`](@ref)。

#### 彩色的`@example`块输出

**输入：**
````markdown
```@example
code_typed(sqrt, (Float64,))
```
````

**输出：**
```@example
code_typed(sqrt, (Float64,))
```

#### 彩色的`@repl`块输出

**输入：**
````markdown
```@repl
printstyled("This should be in bold light cyan.", color=:light_cyan, bold=true)
```
````

**输出：**
```@repl
printstyled("This should be in bold cyan.", color=:cyan, bold=true)
```

**本地禁用颜色：**
````markdown
```@repl; ansicolor=false
printstyled("This should be in bold light cyan.", color=:light_cyan, bold=true)
```
````
```@repl; ansicolor=false
printstyled("This should be in bold light cyan.", color=:light_cyan, bold=true)
```

#### 原始ANSI代码输出

无论颜色设置如何，当您直接打印ANSI转义代码时，都启用了着色。
```@example
for color in 0:15
    print("\e[38;5;$color;48;5;$(color)m  ")
    print("\e[49m", lpad(color, 3), " ")
    color % 8 == 7 && println()
end
print("\e[m")
```

### REPL类型

[`@repl`块](@ref)可用于模拟代码块的REPL评估。例如，下面的块

````markdown
```@repl
using Statistics
xs = collect(1:10)
median(xs)
sum(xs)
```
````

它会展开成像在REPL中评估的样子，前面带有`julia>`提示符等：

```@repl
using Statistics
xs = collect(1:10)
median(xs)
sum(xs)
```

## Doctest演示

目前只是为了在Documenter手册的手动页面中运行doctests。此页面不显示在导航中。

```jldoctest
julia> 2 + 2
4
```

以下doctests需要doctestsetup：

```jldoctest; setup=:(using Documenter)
julia> Documenter.splitexpr(:(Foo.Bar.baz))
(:(Foo.Bar), :(:baz))
```

让我们也尝试一下`@meta`块：

```@meta
DocTestSetup = quote
  f(x) = x^2
end
```

```jldoctest
julia> f(2)
4
```

```@meta
DocTestSetup = nothing
```

