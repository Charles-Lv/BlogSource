---
title: MVC和MVVM架构理解备忘
date: 2017-06-13 23:26:08
tags:
---

> 最近公司由于公司的业务线越来越长，而且维护的项目越来越多。虽然统一了整体技术栈，但是具体业务的开发还是各自未战,所以团队的整体开发效率不高。所以大家决定统一规范,在提升团队开发效率的同时也能提升后续新同事的培训效率以及减少适应难度。但是在框架分层的定义上大家由于理解不同有了不同的见解和意见。经过讨论后决定记录一下探讨后自己的理解,作为备忘。
# MVC架构
MVC架构，Model-View-Controller
- Model呈现数据
- View呈现用户界面
- Controller调节两者之间的交互。从Model取数据，显示在View中。

MVC架构里Controller连接Model和View。所以MVC架构中Controller会比较重，里面杂合了业务逻辑、数据转换逻辑以及View和Model之间的关联逻辑。为了简化Controller的复杂度，所以衍化了MVVM架构。
# MVVM架构
MVVM其实就是MVC的增强版。我们将业务逻辑以及相关的数据转换逻辑从Controller中抽出，放到了一个新的对象里，即ViewModel中。而Controller(或者说是ViewController)负责View和ViewModel直接的关联。
MVVM益处：
- 代码层次清晰
- 兼容MVC模式
- 减少View Controller复杂性

PS：这里我自己有一个误区，认为MVVM的实现是依赖于数据的双向绑定的，因为很多网上的文章介绍的时候都是说MVVM是以"数据模型数据双向绑定”的思想作为核心。但其实MVVM只是一种架构，就算脱离了数据双向绑定这个概念，也可以实现MVVM，只是这样ViewController里就要实现反向的将Model的数据更新到View的逻辑。而双向数据绑定可以大大的简化这部分逻辑（或者说可以直接省略）从而减少ViewController的复杂度。

所以现有的MVVM框架基本都集成了双向数据绑定的功能。MVVM配合双向绑定机制可以大大的提升开发效率简化MVVM框架实现上的代码复杂度。

个人思考：
在ViewModel里的数据转换以及API调用单独抽离成数据转换层和API层，这样应该可以增强后期的代码可维护性以及可扩展性并且减少前端项目对后端API接口的耦合度