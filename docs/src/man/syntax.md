# 语法

本手册的本节介绍了Documenter用于构建文档的语法。有关支持的Markdown语法，请参见Julia手册中的[Markdown标准库的文档](https://docs.julialang.org/en/v1/stdlib/Markdown/)。

```@contents
Pages = ["syntax.md"]
Depth = 2:2
```

## `@docs` 块

将一个或多个文档字符串拼接到文档中以替换代码块，即

````markdown
```@docs
Documenter
makedocs
deploydocs
```
````

如果定义了，则此块类型将在`CurrentModule`模块内进行评估，否则在`Main`内进行评估，因此块中列出的每个对象应从该模块中可见。在生成文档期间，未定义的对象将引发警告，并导致代码块在最终文档中保持不变。

文档中的对象不能重复列出。当检测到重复对象时，将引发错误并终止构建过程。

为了确保在最终文档中包含模块的所有文档字符串，可以将[`makedocs`](@ref)的`modules`关键字设置为所需的模块或模块，即

```julia
makedocs(
    modules = [Documenter],
)
```

这将导致在调用[`makedocs`](@ref)时引发任何未列出的文档字符串警告。如果未定义`modules`，则即使文档缺少文档字符串，也不会打印警告。

还要注意，您可以使用`@docs`仅显示特定方法的文档字符串，方法是指定调度类型。例如
````markdown
```@docs
f(::Type1, ::Type2)
```
````
将仅显示与这些类型相关的`f`的文档字符串。当您的模块扩展函数并向该新方法添加文档字符串时，这可能很有用。

请注意，在指定签名时，它应与方法定义完全匹配。Documenter不会根据调度规则匹配方法。例如，假设您将文档字符串附加到`foo(::Integer) = ...`，则`foo(::Number)`或`foo(::Int64)`都不会在`at-docs`块中匹配它（即使`Int64 <: Integer <: Number`）。唯一的办法是在at-docs块中列出`foo(::Integer)`以完全拼接该文档字符串。

## `@autodocs` 块

自动拼接提供的模块中的所有文档字符串，以替换代码块。这相当于在`@docs`块中手动添加所有文档字符串。

````markdown
```@autodocs
Modules = [Foo, Bar, Bar.Baz]
Order   = [:function, :type]
```
````

上面的`@autodocs`块向文档中添加了在模块`Foo`，`Bar`和`Bar.Baz`中找到的所有函数或类型的文档字符串。请注意，必须显式列出子模块才能包括其中的文档字符串。

每个模块都按顺序添加，因此所有来自`Foo`的文档将出现在`Bar`之前。`Order`向量的可能值包括：

- `:module`
- `:constant`
- `:type`
- `:function`
- `:macro`

如果未提供`Order`，则使用上面列出的顺序。

当在列出的模块中找到潜在的文档字符串，但不匹配`Order`中的任何值时，则将其从文档中省略。因此，`Order`也充当基本过滤器和排序器。

除了`Order`之外，`@autodocs`中还可以包含`Pages`向量，以根据定义它们的源文件过滤文档字符串：

````markdown
```@autodocs
Modules = [Foo]
Pages   = ["a.jl", "b.jl"]
```
````

在上面的示例中，包括在源文件中以`a.jl`和`b.jl`结尾的模块`Foo`中找到的文档字符串。`Pages`提供的页面顺序也用于对文档字符串进行排序。请注意，页面匹配是使用提供的字符串的结尾进行的，因此`a.jl`将与以`a.jl`结尾的*任何*源文件匹配，即`src/a.jl`或`src/foo/a.jl`。

要按照自己的标准过滤掉某些文档字符串，可以使用`Filter`关键字提供函数：

````markdown
```@autodocs
Modules = [Foo]
Filter = t -> typeof(t) === DataType && t <: Foo.C
```
````

在给定的示例中，仅显示`Foo.C`的子类型的文档字符串。您也可以提供之前定义的函数的名称，而不是[匿名函数](https://docs.julialang.org/en/v1/manual/functions/index.html#man-anonymous-functions-1)：

````markdown
```@autodocs
Modules = [Foo]
Filter =  myCustomFilterFunction
```
````

要仅包含`Modules`中列出的模块中导出的名称，请使用`Private = false`。以类似的方式，`Public = false`可用于仅显示未导出的名称。默认情况下，这两个设置为`true`，以便显示所有名称。

````markdown
从`Foo`导出的函数：

```@autodocs
Modules = [Foo]
Private = false
Order = [:function]
```

模块`Foo`中的私有类型：

```@autodocs
Modules = [Foo]
Public = false
Order = [:type]
```
````

!!! 注意

    当需要更复杂的排序时，请使用`@docs`显式定义它。

## `@ref` 链接

作为URL在markdown链接中使用，告诉Documenter自动生成交叉引用。链接的文本部分可以是文档字符串、标题名称或GitHub PR/Issue号码。

````markdown
# Syntax

... [`makedocs`](@ref) ...

# Functions

```@docs
makedocs
```

... [Syntax](@ref) ...

... [#42](@ref) ...
````

链接中的“文本”部分中的纯文本将交叉引用标题，或者，当它是以`#`为前缀的数字时，是GitHub问题/拉取请求。用反引号包装的文本将交叉引用`@docs`块中的文档字符串。

`@ref`还可以使用相同的语法引用不同页面上的文档字符串或标题，以及当前页面。

请注意，根据设置的`CurrentModule`，文档字符串`@ref`可能需要在定义它的模块之前加上前缀。

### 命名的 `@ref`

也可以通过将适当的标签添加到链接中来覆盖`@ref`链接的目标，例如文档字符串引用或页面标题。

```markdown
Both of the following references point to `g` found in module `Main.Other`:

* [`Main.Other.g`](@ref)
* [the `g` function](@ref Main.Other.g)

Both of the following point to the heading "On Something":

* [On Something](@ref)
* [The section about something.](@ref "On Something")
```

这可以避免为未导入到当前模块的引用编写完全限定名称，或者当链接中显示的文本用于为周围的文本添加额外的含义时，例如

```markdown
Use [`for i = 1:10 ...`](@ref for) to loop over all the numbers from 1 to 10.
```

!!! 注意

    应该谨慎使用命名的文档`@ref`，因为在某些情况下，编写未限定名称可能会使在特定文档字符串中难以确定*引用哪个*函数，如果恰好有多个模块提供具有相同名称的定义。

### 重复的标题

在某些情况下，文档可能包含多个具有相同名称但位于不同页面或不同级别的标题。为了允许`@ref`交叉引用重复的标题，必须像以下示例中一样给它一个名称

```markdown
# [Header](@id my_custom_header_name)

...

## Header

... [Custom Header](@ref my_custom_header_name) ...
```

包装命名标题的链接在最终文档中被删除。命名的`@ref ...`的文本不需要与它引用的标题匹配。命名的`@ref ...`可以像未命名的`@ref ...`一样引用不同页面上的标题。

不会发生重复的文档字符串引用，因为将同一文档字符串插入文档超过一次是不允许的。

!!! 注意 "标签优先级"

    在发生冲突的情况下，用户定义的和内部生成的标题引用标签都优先于文档字符串引用。

## `@meta` 块

此块类型用于定义元数据键/值对，可以在页面的其他位置使用。目前识别的键：
- `CurrentModule`：Documenter评估的模块，例如[`@docs` 块](@ref)和[`@ref`链接](@ref)。
- `DocTestSetup`：在doctest之前要评估的代码，请参见[Doctests](@ref)下的[设置代码](@ref)部分。
- `DocTestFilters`：用于处理来自doctest的，例如不可预测的输出的过滤器，请参见[Doctests](@ref)下的[过滤Doctests](@ref)部分。
- `EditURL`：链接到可以编辑页面的位置。这默认为`.md`页面本身，但如果源是其他内容（例如，如果`.md`页面作为文档生成的一部分生成），则可以设置本地链接或绝对URL。
- `Description`：页面特定的描述，显示在搜索引擎和链接预览中。覆盖[`makedocs`](@ref)中的全站点描述。
- `Draft`：用于覆盖页面的全局草稿模式的布尔值。

示例：

````markdown
```@meta
CurrentModule = FooBar
DocTestSetup  = quote
    using MyPackage
end
DocTestFilters = [r"Stacktrace:[\s\S]+"]
EditURL = "link/to/source/file"
```
````

请注意，`@meta`块始终在`Main`中评估。

## `@index` 块

生成链接列表，这些链接指向已插入文档中的文档字符串。有效的设置为`Pages`、`Modules`和`Order`。例如：

````markdown
```@index
Pages   = ["foo.md"]
Modules = [Foo, Bar]
Order   = [:function, :type]
```
````

当未提供`Pages`或`Modules`时，将包括所有页面或模块。如果未指定，则`Order`默认为

```julia
[:module, :constant, :type, :function, :macro]
```

。`Order`和`Modules`的行为与[`@autodocs` 块](@ref)相同，并过滤掉不匹配指定模块或类别之一的文档字符串。

请注意，分配给`Pages`、`Modules`和`Order`的值可以是任何有效的Julia代码，因此如果需要，可以是更复杂的内容，而不仅仅是数组文字，即

````markdown
```@index
Pages = map(file -> joinpath("man", file), readdir("man"))
```
````

应该注意的是，在这种情况下，`Pages`可能不按用户期望的顺序排序。尽量坚持使用数组文字。

## `@contents` 块

生成嵌套的链接列表，这些链接指向文档部分。有效的设置为`Pages`和`Depth`。

````markdown
```@contents
Pages = ["foo.md"]
Depth = 5
```
````

与`@index`一样，如果未提供`Pages`，则将包括所有页面。默认的`Depth`值为`2`，即包括标题级别为1和2。`Depth`还接受`UnitRange`，以便也可以配置要显示的最小标题级别。例如，`Depth = 2:3`可用于仅包括级别为2-3的标题。

## `@example` 块

评估代码块，并将最后一个表达式的结果与原始源代码一起插入到最终文档中。如果最后一个表达式返回`nothing`，则插入整个块的`stdout`和`stderr`流。最后一行末尾的分号`;`没有影响。

````markdown
```@example
a = 1
b = 2
a + b
```
````

上述`@example`块将以下内容插入到最终文档中

````markdown
```julia
a = 1
b = 2
a + b
```

```
3
```
````

首尾换行符从渲染的代码块中删除。每行末尾的尾随空格也将被删除。

!!! 注意
    工作目录`pwd`设置为`build`中写入文件的目录，并且`include`调用中的路径被解释为相对于`pwd`。这可以使用[`makedocs`](@ref)的`workdir`关键字进行自定义。

**隐藏源代码**

代码块可能具有一些不需要在最终文档中显示的内容。可以在不需要呈现的行的末尾附加`# hide`注释，即

````markdown
```@example
import Random # hide
Random.seed!(1) # hide
A = rand(3, 3)
b = [1, 2, 3]
A \ b
```
````

请注意，在`@example`块的每一行附加`# hide`将导致在呈现的文档中隐藏该块。但是，结果块仍将被呈现。`@setup`块是隐藏整个块（包括输出）的便捷方式。

**空输出**

当`@example`块返回`nothing`时，结果块将显示整个块产生的`stdout`和`stderr`流。如果它们为空，则不显示结果块；仅在呈现的文档中显示源代码块。

**命名的`@example`块**

默认情况下，`@example`块在自己的匿名`Module`中运行，以避免块之间的副作用。要在页面上的不同块之间共享相同的模块，可以使用以下语法命名`@example`：

````markdown
```@example 1
a = 1
```

```@example 1
println(a)
```
````

名称可以是任何文本，不仅仅是上面示例中的整数，例如`@example foo`。

当生成需要中间解释或多媒体（如绘图）的文档时，命名的`@example`块可能很有用，如下面的示例所示

````markdown
首先定义一些函数

```@example 1
using PyPlot # hide
f(x) = sin(2x) + 1
g(x) = cos(x) - x
```

然后我们在从“-π”到“π”的区间上绘制`f`

```@example 1
x = range(-π, π; length=50)
plot(x, f.(x), color = "red")
savefig("f-plot.svg"); nothing # hide
```

![](f-plot.svg)

然后我们用`g`做同样的操作

```@example 1
plot(x, g.(x), color = "blue")
savefig("g-plot.svg"); nothing # hide
```

![](g-plot.svg)
````

请注意，`@example`块在`build`的目录中评估。这意味着在上面的示例中，`savefig`将输出`.svg`文件到该目录中。这样可以轻松引用图像，无需担心相对路径。

!!! info
    如果您使用默认后端[GR.jl](https://github.com/jheinen/GR.jl)与[Plots.jl](https://github.com/JuliaPlots/Plots.jl)一起使用，则可能会看到以下警告：
    ```
    qt.qpa.xcb: could not connect to display
    qt.qpa.plugin: Could not load the Qt platform plugin "xcb" in "" even though it was found.
    ```
    要解决这些问题，您需要将环境变量`GKSwstype`设置为`100`。例如，如果您使用GitHub操作来构建文档，则可以修改默认脚本以：
    ```
    - name: Build and deploy
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # For authentication with GitHub Actions token
        DOCUMENTER_KEY: ${{ secrets.DOCUMENTER_KEY }} # For authentication with SSH deploy key
        GKSwstype: "100" # https://discourse.julialang.org/t/generation-of-documentation-fails-qt-qpa-xcb-could-not-connect-to-display/60988
      run: julia --project=docs --color=yes docs/make.jl
    ```
    Alternatively, you can set this environment variable directly in Julia using
    ```julia
    ENV["GKSwstype"] = "100"
    ```

`@example`块会自动定义`ans`，就像在Julia REPL中一样，它绑定到最后一个评估表达式的值。在以下情况下，这可能非常有用，其中将`plot`返回的对象绑定到命名变量看起来不协调在最终呈现的文档中：

````markdown
```@example
using Gadfly # hide
plot([sin, x -> 2sin(x) + x], -2π, 2π)
draw(SVG("plot.svg", 6inch, 4inch), ans); nothing # hide
```

![](plot.svg)
````

**彩色输出**

`@example`块支持通过将[ANSI转义代码](https://en.wikipedia.org/wiki/ANSI_escape_code)映射到HTML来进行彩色文本输出。例如，以下块：
````markdown
```@example
printstyled("Here are some colors:\n"; color=:red, bold=true)
for color in 0:15
    print("\e[38;5;$(color);48;5;$(color)m  ")
    print("\e[49m", lpad(color, 3), " ")
    color % 8 == 7 && println()
end
print("\e[m")
```
````
将产生以下输入和输出块：
```@example
printstyled("Here are some colors:\n"; color=:red, bold=true)
for color in 0:15
    print("\e[38;5;$(color);48;5;$(color)m  ")
    print("\e[49m", lpad(color, 3), " ")
    color % 8 == 7 && println()
end
print("\e[m")
```

!!! 注意 "禁用彩色输出"
    要全局禁用彩色输出，请将`ansicolor=false`传递给[`Documenter.HTML`](@ref)，
    要在块级别本地禁用，请使用`ansicolor=false`，如下所示：
    
    ````markdown
    ```@example; ansicolor=false
    printstyled("hello, world"; color=:red, bold=true)
    ```
    ````

**延迟执行`@example`块**

`@example`块接受一个关键字参数`continued`，可以设置为`true`或`false`（默认为`false`）。当`continued = true`时，代码的执行被延迟到下一个`continued = false` `@example`块。例如，当块中的表达式不完整时，就需要这样做。示例：
````markdown
```@example half-loop; continued = true
for i in 1:3
    j = i^2
```
一些解释说明我们应该如何处理`j`
```@example half-loop
    println(j)
end
```
````

在此处，第一个块不完整--循环缺少`end`。因此，通过在此处设置`continued = true`，我们延迟了第一个块的评估，直到我们到达第二个块。具有`continued = true`的块没有任何输出。

## `@repl` 块

这些与`@example`块类似，但在每个顶级表达式之前添加`julia> `提示，并且在遇到错误时不会失败。
可以在`@repl`块中像在`@example`块中一样使用`# hide`语法。此外，行末尾的分号`;`将像在Julia REPL中一样抑制输出。

````markdown
```@repl
a = 1
b = 2
a + b
```
````

将生成

````markdown
```julia
julia> a = 1
1

julia> b = 2
2

julia> a + b
3
```
````

同样地，

````markdown
```@repl
sqrt(-1)
```
````

将生成

````markdown
```julia
julia> sqrt(-1)
ERROR: DomainError with -1.0:
sqrt will only return a complex result if called with a complex argument. Try sqrt(Complex(x)).
```
````

`@repl`块支持彩色输出，就像`@example`块一样。以下块
````markdown
```@repl
printstyled("hello, world"; color=:red, bold=true)
```
````
给出
```@repl
printstyled("hello, world"; color=:red, bold=true)
```
!!! 注意 "禁用彩色输出"
    要全局禁用彩色输出，请将`ansicolor=false`传递给[`Documenter.HTML`](@ref)，
    要在块级别本地禁用，请使用`ansicolor=false`，如下所示：

    ````markdown
    ```@repl; ansicolor=false
    printstyled("hello, world"; color=:red, bold=true)
    ```
    ````
命名为`@repl <name>`的块的行为方式与命名为`@example <name>`的块相同。

!!! 注意
    工作目录`pwd`设置为在其中写入文件的`build`目录，而`include`调用中的路径被解释为相对于`pwd`。可以使用[`makedocs`](@ref)的`workdir`关键字自定义此设置。

!!! 注意 "软作用域与硬作用域"

    Julia 1.5将REPL更改为在处理`for`循环等中的全局变量时使用_soft scope_。当使用Julia 1.5或更高版本的Documenter时，Documenter在`@repl`块和REPL类型的doctest中使用软作用域。

## `@setup <name>` 块

这些与`@example`块类似，但输入和输出都从最终文档中隐藏。如果需要隐藏几行设置代码，这可能很方便。

!!! 注意

    与`@example`和`@repl`块不同，`@setup`需要一个`<name>`属性，以将其与下游的`@example <name>`和`@repl <name>`块关联起来。

````markdown
```@setup abc
using RDatasets
using DataFrames
iris = dataset("datasets", "iris")
```

```@example abc
println(iris)
```
````

## `@eval` 块

评估块的内容并将生成的值插入到最终文档中，除非最后一个表达式评估为`nothing`。

在以下示例中，我们使用PyPlot包生成图形并在最终文档中显示它。

````markdown
```@eval
using PyPlot

x = range(-π, π; length=50)
y = sin.(x)

plot(x, y, color = "red")
savefig("plot.svg")

nothing
```

![](plot.svg)
````

在上面的示例中，我们可以返回一个新的`Markdown.MD`对象通过`Markdown.parse`。当文件名在块本身的评估之前不知道时，这可能更合适。

另一个示例是从CSV或JSON等机器可读数据格式生成markdown表格。

````markdown
```@eval
using CSV
using Latexify
df = CSV.read("table.csv")
mdtable(df,latex=false)
```
````

这将生成CSV文件`table.csv`的markdown版本，并在输出格式中呈现它。

`@eval`块中的最终表达式必须是`nothing`或有效的`Markdown.MD`对象。其他对象将生成警告，并将以文本形式呈现为代码块，但此行为可能会更改，不应依赖于此。

请注意，每个`@eval`块都在单独的模块中评估其内容。在评估每个块时，当前工作目录`pwd`设置为在其中写入文件的`build`目录，而`include`调用中的路径被解释为相对于`pwd`。

!!! 注意

    在大多数情况下，`@example`优先于`@eval`。就像在正常的Julia代码中，`eval`只应被视为最后的手段，应该以同样的方式处理`@eval`。


## `@raw <format>` 块

允许将代码逐字插入最终文档中。例如，将自定义HTML或LaTeX代码插入输出中。

`format`参数是必需的，Documenter使用它来确定是否应将特定块复制到输出中。目前支持的格式为`html`和`latex`，由各自的编写器使用。一个未被识别的`@raw`块格式通常会被忽略，因此可以为每个输出格式拥有一个原始块，而不会在输出中重复块。

下面的示例显示了如何使用`@raw`块将具有自定义样式的SVG代码包含到文档中。

````markdown
```@raw html
<svg style="display: block; margin: 0 auto;" width="5em" heigth="5em">
	<circle cx="2.5em" cy="2.5em" r="2em" stroke="black" stroke-width=".1em" fill="red" />
</svg>
```
````

它将显示如下，代码已经逐字复制到HTML文件中。

```@raw html
<svg style="display: block; margin: 0 auto;" width="5em" heigth="5em">
	<circle cx="2.5em" cy="2.5em" r="2em" stroke="black" stroke-width=".1em" fill="red" />
    (SVG)
</svg>
```
