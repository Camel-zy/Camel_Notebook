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
### admonition

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
