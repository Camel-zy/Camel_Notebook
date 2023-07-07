---
title: Mkdocs使用
---

# Mkdocs使用
## 安装
首先安装mkdocs和material主题
```shell
pip install mkdocs
pip install mkdocs-material
```

## 基本用法
具体用法可以参阅[mkdocs-material官方文档](https://squidfunk.github.io/mkdocs-material/getting-started/)，此处仅作简单摘要

创建新项目
```shell
mkdocs new .
```

## Python Markdown扩展语法
### Admonitions

基本语法

=== "basic"
    ```markdown
    !!! note "This is the title of my note"
        This is the content of my note
    ```
    !!! note "This is the title of my note"
        This is the content of my note

=== "collapsible"
    ```markdown
    ??? note "Collapsible note"
        This is the content of my note
    ```

    ??? note "Collapsible note"
        This is the content of my note

=== "collapsible expanded"
    ```markdown
    ???+ note "Collapsible note with expanded content"
        This is the content of my note
    ```

    ???+ note "Collapsible note with expanded content"
        This is the content of my note

内联用法
=== "inline note"
    ```markdown
    !!! info inline "inline note"
        This is the content of my note
    ```
    !!! info inline "inline note"
        This is the content of my note

=== "inline end note"
    ```markdown
    !!! info inline end "inline end note"
        This is the content of my note
    ```
    !!! info inline end "inline end note"
        This is the content of my note

admonition支持以下类型：

=== "note"
    !!! note "Note"
        This is the content of my note
=== "abstract"
    !!! abstract "Abstract"
        This is the content of my abstract
=== "info"
    !!! info "Info"
        This is the content of my info
=== "tip"
    !!! tip "Tip"
        This is the content of my tip
=== "success"
    !!! success "Success"
        This is the content of my success
=== "question"
    !!! question "Question"
        This is the content of my question
=== "warning"
    !!! warning "Warning"
        This is the content of my warning
=== "failure"
    !!! failure "Failure"
        This is the content of my failure
=== "danger"
    !!! danger "Danger"
        This is the content of my danger
=== "bug"
    !!! bug "Bug"
        This is the content of my bug
=== "example"
    !!! example "Example"
        This is the content of my example
=== "quote"
    !!! quote "Quote"
        This is the content of my quote
### Annotations
基本语法(1)
{.annotate}

1. :warning: 注意：此功能为mkdocs-material 9.2.0b0版本新增，为当前(1)最新的beta版本，稳定版暂无此功能.
    {.annotate}

    1. 2023.07.07

```markdown
This is a paragraph (1) with a annotation
{.annotate}

1. :man_raising_hand: This is the (1) annotation
    {.annotate}

    1. :woman_raising_hand: This is the annotation
```

This is a paragraph (1) with a annotation
{.annotate}

1. :man_raising_hand: This is the (1) annotation
    {.annotate}

    1. :woman_raising_hand: This is the annotation

!!! warning "Known limitations"
    annotations 目前在data tables中无效，详见[mkdocs-material issue #3453](https://github.com/squidfunk/mkdocs-material/issues/3453)

### Buttons
基本语法
=== "无填充"
    ```markdown
    [Button](`javascript:alert$.next('Hello World!')`){ .md-button }
    ```

    [Button](`javascript:alert$.next('Hello World!')`){ .md-button }

=== "有填充"
    ```markdown
    [Filled Button](`javascript:alert$.next('Hello World!')`){ .md-button .md-button--primary }
    ```
    
    [Filled Button](`javascript:alert$.next('Hello World!')`){ .md-button .md-button--primary }

### Code blocks
基本语法
=== "添加标题"
    ````markdown
    ``` py title="bubble_sort.py"
    def bubble_sort(items):
        for i in range(len(items)):
            for j in range(len(items) - 1 - i):
                if items[j] > items[j + 1]:
                    items[j], items[j + 1] = items[j + 1], items[j]
    ```
    ````

    ``` py title="bubble_sort.py"
    def bubble_sort(items):
        for i in range(len(items)):
            for j in range(len(items) - 1 - i):
                if items[j] > items[j + 1]:
                    items[j], items[j + 1] = items[j + 1], items[j]
    ```
=== "添加annotation"
    将annotation添加到代码的注释中即可

    以下两者的区别在于显示时是否显示注释标记

    ````markdown
    ``` yaml
    theme:
    features:
        - content.code.annotate # (1)
        - content.code.copy # (2)!
    ``` 

    1. 添加注释功能
    2. 添加复制功能
    ````
    ``` yaml
    theme:
    features:
        - content.code.annotate # (1)
        - content.code.copy # (2)!
    ``` 

    1. 添加注释功能
    2. 添加复制功能

=== "添加行号"
    ````markdown
    ``` py linenums="1"
    def bubble_sort(items):
        for i in range(len(items)):
            for j in range(len(items) - 1 - i):
                if items[j] > items[j + 1]:
                    items[j], items[j + 1] = items[j + 1], items[j]
    ```
    ````

    ``` py linenums="1"
    def bubble_sort(items):
        for i in range(len(items)):
            for j in range(len(items) - 1 - i):
                if items[j] > items[j + 1]:
                    items[j], items[j + 1] = items[j + 1], items[j]
    ```

=== "高亮"
    高亮行
    ````markdown
    ``` py hl_lines="2 3"
    def bubble_sort(items):
        for i in range(len(items)):
            for j in range(len(items) - 1 - i):
                if items[j] > items[j + 1]:
                    items[j], items[j + 1] = items[j + 1], items[j]
    ```
    ````

    ``` py hl_lines="2 3"
    def bubble_sort(items):
        for i in range(len(items)):
            for j in range(len(items) - 1 - i):
                if items[j] > items[j + 1]:
                    items[j], items[j + 1] = items[j + 1], items[j]
    ```
    高亮行内代码
    ```markdown
    The `#!python range()` function is used to generate a sequence of numbers.
    ```
    The `#!python range()` function is used to generate a sequence of numbers.



支持代码高亮的语言[列表](https://pygments.org/docs/lexers/)

### Diagrams
基本语法

=== "flowcharts"

    ````markdown
    ```mermaid
    graph LR
    A[Start] --> B{Error?};
    B -->|Yes| C[Hmm...];
    C --> D[Debug];
    D --> B;
    B ---->|No| E[Yay!];
    ```
    ````

    ```mermaid
    graph LR
    A[Start] --> B{Error?};
    B -->|Yes| C[Hmm...];
    C --> D[Debug];
    D --> B;
    B ---->|No| E[Yay!];
    ```
=== "sequence diagrams"
    ````markdown
    ``` mermaid
    sequenceDiagram
    autonumber
    Alice->>John: Hello John, how are you?
    loop Healthcheck
        John->>John: Fight against hypochondria
    end
    Note right of John: Rational thoughts!
    John-->>Alice: Great!
    John->>Bob: How about you?
    Bob-->>John: Jolly good!
    ```
    ````

    ``` mermaid
    sequenceDiagram
    autonumber
    Alice->>John: Hello John, how are you?
    loop Healthcheck
        John->>John: Fight against hypochondria
    end
    Note right of John: Rational thoughts!
    John-->>Alice: Great!
    John->>Bob: How about you?
    Bob-->>John: Jolly good!
    ```

=== "state diagrams"

    ````markdown
    ``` mermaid
    stateDiagram-v2
    state fork_state <<fork>>
        [*] --> fork_state
        fork_state --> State2
        fork_state --> State3

        state join_state <<join>>
        State2 --> join_state
        State3 --> join_state
        join_state --> State4
        State4 --> [*]
    ```
    ````

    ``` mermaid
    stateDiagram-v2
    state fork_state <<fork>>
        [*] --> fork_state
        fork_state --> State2
        fork_state --> State3

        state join_state <<join>>
        State2 --> join_state
        State3 --> join_state
        join_state --> State4
        State4 --> [*]
    ```

=== "class diagrams"

    ````markdown
    ``` mermaid
    classDiagram
    Person <|-- Student
    Person <|-- Professor
    Person: +String name
    Person: +String phoneNumber
    Person: +String emailAddress
    Person: +purchaseParkingPass()
    Address "1" <-- "0..1" Person:lives at
    class Student{
        +int studentNumber
        +int averageMark
        +isEligibleToEnrol()
        +getSeminarsTaken()
    }
    class Professor{
        +int salary
    }
    class Address{
        +String street
        +String city
        +String state
        +int postalCode
        +String country
        -validate()
        +outputAsLabel()  
    }
    ```
    ````

    ``` mermaid
    classDiagram
    Person <|-- Student
    Person <|-- Professor
    Person: +String name
    Person: +String phoneNumber
    Person: +String emailAddress
    Person: +purchaseParkingPass()
    Address "1" <-- "0..1" Person:lives at
    class Student{
        +int studentNumber
        +int averageMark
        +isEligibleToEnrol()
        +getSeminarsTaken()
    }
    class Professor{
        +int salary
    }
    class Address{
        +String street
        +String city
        +String state
        +int postalCode
        +String country
        -validate()
        +outputAsLabel()  
    }
    ```

=== "entity-relationship diagrams"

    ````markdown
    ``` mermaid
    erDiagram
    CUSTOMER ||--o{ ORDER : places
    ORDER ||--|{ LINE-ITEM : contains
    LINE-ITEM {
        string name
        int pricePerUnit
    }
    CUSTOMER }|..|{ DELIVERY-ADDRESS : uses
    ```
    ````

    ``` mermaid
    erDiagram
    CUSTOMER ||--o{ ORDER : places
    ORDER ||--|{ LINE-ITEM : contains
    LINE-ITEM {
        string name
        int pricePerUnit
    }
    CUSTOMER }|..|{ DELIVERY-ADDRESS : uses
    ```

### footnotes

```markdown
Here's a simple footnote,[^1] and here's a longer one.[^bignote]

[^1]: This is the first footnote.
[^bignote]: Here's one with multiple paragraphs and code.

    Indent paragraphs to include them in the footnote.

    `{ my code }`

    Add as many paragraphs as you like.
```

Here's a simple footnote,[^1] and here's a longer one.[^bignote]

[^1]: This is the first footnote.
[^bignote]: Here's one with multiple paragraphs and code.

    Indent paragraphs to include them in the footnote.

    `{ my code }`

    Add as many paragraphs as you like.

### Formatting

自行查看[官方文档](https://squidfunk.github.io/mkdocs-material/reference/formatting/)，包含critic、caret格式化语法以及上下标、键盘按键的格式

键盘按键：
```markdown
++ctrl+alt+del++
```

++ctrl+alt+del++

### Grids

自行查看[官方文档](https://squidfunk.github.io/mkdocs-material/reference/grids/)，包含grid布局

### Icons, Emojis

基本语法

```markdown
:smile:
```

:smile:

可用的icon和emoji列表可在[官网](https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/)搜索

### Images

=== "基本语法"

    ```markdown
    ![Camel](https://s2.loli.net/2022/01/11/yJKGvoha21PnYI3.png) {align=right width="300" loading=lazy}
    ```

    ![Camel](https://s2.loli.net/2022/01/11/yJKGvoha21PnYI3.png){align=right width="300" loading=lazy}

=== "带标题"

    ```markdown
    <figure markdown>
    ![Camel Title](https://s2.loli.net/2022/01/11/yJKGvoha21PnYI3.png){ width="300" }
    <figcaption>Camel Caption</figcaption>
    </figure>
    ```

    <figure markdown>
    ![Camel Title](https://s2.loli.net/2022/01/11/yJKGvoha21PnYI3.png){ width="300" }
    <figcaption>Camel Caption</figcaption>
    </figure>

### Lists

Markdown标准语法中的list略去不表

=== "Definition list"

    ```markdown
    `A`

    : The first item

    `B`

    : The second item
    ```

    `A`

    : The first item

    `B`

    : The second item

=== "task lists"

    ```markdown
    - [x] ab
    - [ ] cd
        * [x] ef
        * [ ] gh
    ```

    - [x] ab
    - [ ] cd
        * [x] ef
        * [ ] gh

### Tooltips

```markdown
[Hover me](https://example.com "I'm a tooltip!")
```

[Hover me](https://example.com "I'm a tooltip!")


其它更多用法参见[官方文档](https://squidfunk.github.io/mkdocs-material/reference/tooltips/)