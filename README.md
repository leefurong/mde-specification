# 缘起

Mark Down for Education.

21世纪的学习， 不再只是吸收知识， 更要学会思考。
学生在阅读文字时，如果能够同时做答题， 这无疑能促进学生思考。

今天的文字系统（博客、微信公众号文章、电子书）大都注重呈现， 研究如何显示文字。 因此老师缺乏一种工具，能够创作“激发思考”的教材。

因此，我们在markdown的基础上， 加以扩展， 设计出mde（mark down for education）。
运用mde， 书籍的作者可以在文字中加入“互动”的部分。

这种新型的学习材料， 使用者将不仅是“阅读”， 而是“学习”。（学+习）
鉴于此种转变， 以下将不再称使用材料者为“读者”，而是“学习者”。

本仓库的主旨， 在于介绍这种规范（以下简称mde）， 并且（在未来）提供一个参考实现。

# 基础架构

所有的markdown文档都是合法的mde文档， 反之亦然。
那如何实现“互动答题”呢？我们采用markdown的“代码”来表示。

## 如何在mde中添加题目？

以选择题为例

```mde: options
2020年流行的病毒叫什么？
[ ] 新型冠状病毒
[ ] ncov2019
[ ] 以上都不是
----
0 1
```

## react参考实现方案

把代码分为三部分， 一是解析器， 负责把字符串结构化， 二是组件代码， 只负责把结构化的数据渲染成互动的组件， 三是应用代码， 负责整体的方案（例如石墨文档、简书等）。

应用代码只需要负责把这段字符串传给组件代码即可。

### 组件代码

组件代码定义这个组件在三种不同形态下的呈现：
1. 作者的界面（可以先仅仅实现为纯文本编辑， 此处暂不详述）
2. 学习者的界面（又分为未答题和已答题两种状态）
3. 应用开发者的界面

* 学习者的界面
组件代码负责把这段字符串传送给解析器（parse函数）， 解析器生成一个结构化的数据。
组件代码便可利用这个结构化的数据， 来渲染成一个交互的react widget。 
对于已答题的状态， 组件代码可以通过storage.read()得到这位学习者的答题信息。

* 应用开发者的界面
应用开发者调用组件时， 需要传递描述这个题目的字符串。
如果希望把答题数据记录在云端， 或者做个性化处理， 可以传递一个storage对象。这个对象应该实现两个方法： storage.write(k, v)和storage.read(k)

