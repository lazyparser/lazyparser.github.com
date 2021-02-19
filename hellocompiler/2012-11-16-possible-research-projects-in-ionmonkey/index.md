---
title: "IonMonkey 中可能的研究点"
date: "2012-11-16"
categories: 
  - "spidermonkey"
tags: 
  - "alias-analysis"
  - "control-flow-elimination"
  - "eascape-analysis"
  - "firefox"
  - "ionmonkey"
  - "javascript"
  - "jit"
  - "mozilla"
  - "research-2"
---

一周前，一位巴西的大四学生给 Mozilla JS-Internals 邮件列表发了一封邮件\[1\]，说自己下半年就开始计算机科学的研究生学业了，希望能够在 IonMonkey 上做些研究，但是刚接触 IonMonkey 没有什么感觉，希望能够得到一些指点。今天 Mozilla JS Engine 的负责人 David Anderson 回复了他，指出了几个他们感兴趣的研究项目\[2\]，有兴趣的读者可以关注一下：

1. **Escape Analysis（逃逸分析）**：目前还没有任何的工作，所以即使不是完整的算法实现，能够得到一些测试数据也是很好的。逃逸分析能够帮助减少冗余的堆内存占用（当一个线程中的堆内存对象不确定是否被其它线程引用的时候是不能轻易的删除的）。
2. **Better Alias Analysis（别名分析）**：目前 IonMonkey 中有一个别名分析（位于 js/src/ion/AliasAnalysis.{h,cpp}），但是比较的粗糙，例如在遇到类似“v.x + v.y + v.z”这样的表达式时，现在的别名分析会将 v.x 和 v.z 都看成是 v.y 的别名。这阻碍了后续的优化工作。
3. **RA Improvements（寄存器分配算法的改进）**：要重写一个 RA 是非常难的，工作量也非常的大。如果能够在现在 RA 实现的基础上做一些改进，也是很有意义的。
4. **Control-flow Elimination（不常用控制流消除）**：目前 IonMonkey 能够消除（eliminate）单个指令，但是无法消除 CFG 中的 Block 。如果这个功能实现了，我们（开发人员）就可以做进一步的实验，尝试更加激进的优化，消除掉不常用的分支，或许还可以促进 RA 的效果。

目前 IonMonkey 还在开发中，支持的分析和优化还不是很多，实现上也是比较简单的实现，应该还有不少的机会。

Reference:

\[1\]: [http://www.mail-archive.com/dev-tech-js-engine-internals@lists.mozilla.org/msg00120.html](http://www.mail-archive.com/dev-tech-js-engine-internals@lists.mozilla.org/msg00120.html)

\[2\]: [http://www.mail-archive.com/dev-tech-js-engine-internals@lists.mozilla.org/msg00122.html](http://www.mail-archive.com/dev-tech-js-engine-internals@lists.mozilla.org/msg00122.html)
