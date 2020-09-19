---
title: "代码的坏味道 一"
date: 
lastmod: 
draft: true
tags: ["refacting"]
categories: ["refacting"]
author: "Rogers"

weight: 1
wordCount: 100
readingTime: 10

# You can also close(false) or open(true) something for this content.
# P.S. comment can only be closed
# comment: false
# toc: false

# You can also define another contentCopyright. e.g. contentCopyright: "This is another copyright."
contentCopyright: '<a href="https://github.com/gohugoio/hugoBasicExample" rel="noopener" target="_blank">See origin</a>'
# reward: false
mathjax: true
---


# 代码的坏味道 一
# Duplicate Code
臭味行列中首当其冲的就是Duplicate Code（重复的代码）。如果你在一个以上的地点看到相同的程序结构，那么当可肯定：设法将它们合而为一，程序会变得更好。

最单纯的Duplicated Code就是「同一个class内的两个函数含有相同表达式（expression）」。这时候你需要做的就是采用 提炼函数 的方法提炼出重复的代码，然后让这两个地点都调用被提炼出来的那一段代码。

另一种常见情况就是「两个互为兄弟〔sibling）的子类中含相同表达式」。要避免这种情况，只需对两个子类都使用提炼函数的方法，然后再对被提炼出来的代码使用提升作用域，将它推入父类中。如果代码之 间只是类似，并非完全相同，那么就得运用提炼函数将相似部分和差异部分割开，构成单独一个函数。然后你可能发现或许可以运用 模板模式 来改善代码。如果有些函数以不同的算法做相同的事，你可以择定其中较清晰的一个，并使用替换算法将其他函数的算法替换掉。

如果两个毫不相关的classes内出现Duplicated Code，你应该考虑对其中一个使用Extract Class，将重复代码提炼到一个独立class中，然后在另一个class内 使用这个新class。但是，重复代码所在的函数也可能的确只应该属于某个class， 另一个class只能调用它，抑或这个函数可能属于第三个class，而另两个classes应该引用这第三个class。你必须决定这个函数放在哪儿最合适，并确保它被安置后就不会再在其他任何地方出现。

# Long Method
拥有[短函数」（short methods）的对象会活得比较好、比较长。不熟悉面向对象技术的人，常常觉得对象程序中只有无穷无尽的delegation（委托），根本没有进行任何计算。和此类程序共同生活数年之后，你才会知道，这些小小函数有多大价值。「间接层」所能带来的全部利益——解释能力、共享能力、选择能力——都是由小型函数支持的。

很久以前程序员就巳认识到：程序愈长愈难理解。早期的编程语言中，「子程序调用动作」需要额外开销，这使得人们不太乐意使用small method。现代OO语言几乎已经完全免除了进程（process）内的「函数调用动作额外开销」。不过代码阅读者还是得多费力气，因为他必须经常转换上下文去看看子程序做了什么。某些开发环境允许用户同时看到两个函数，这可以帮助你省去部分麻烦，但是让small method容易理解的真正关键在于一个好名字。如果你能给函数起个好名字，读者就可以通过名字了解函数的作用，根本不必去看其中写了些什么。

最终的效果是：你应该更积极进取地分解函数。我们遵循这样一条原则：每当感觉需要以注释来说明点什么的时候，我们就把需要说明的东西写进一个独立函数中，并以其用途（而非实现手法）命名。我们可以对一组或甚至短短一行代码做这件事。哪怕替换后的函数调用动作比函数自身还长，只要函数名称能够解释其用途，我们也该毫不犹豫地那么做。关键不在于函数的长度，而在于函数「做什么」和「如何做」之间的语义距离。

百分之九十九的场合里，要把函数变小，只需使用提炼函数。找到函数中适合集在一起的部分，将它们提炼出来形成一个新函数。

如果函数内有大量的参数和临时变量，它们会对你的函数提炼形成阻碍。如果你尝试运用提炼函数，最终就会把许多这些参数和临时变量当作参数，传递给被提炼出来的新函数，导致可读性几乎没有任何提升。啊是的，你可以经常运用Replace Temp with Query （以查询取代临时变量）来消除这些暂时元素。Introduce Parameter Object （引入参数对象）和Preserve Whole Object（保持对象完整） 则可以将过长的参数列变得更简洁一些。

如果你已经这么做了，仍然有太多临时变量和参数，那就应该使出我们的杀手锏： [Replace Method with Method Object](../refactoring-techniques/method-object.md)（以函数对象取代函数）。

如何确定该提炼哪一段代码昵？ 一个很好的技巧是：寻找注释。它们通常是指出「代码用途和实现手法间的语义距离」的信号。如果代码前方有一行注释，就是在提醒 你：可以将这段代码替换成一个函数，而且可以在注释的基础上给这个函数命名。就算只有一行代码，如果它需要以注释来说明，那也值得将它提炼到独立函数去。

条件式和循环常常也是提炼的信号。你可以使用Decompose Conditional （分解条件式）处理条件式。至于循环，你应该将循环和其内的代码提炼到一个独立函数中。