---
title: "Replace Method with Method Object"
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

# 用方法对象代替方法
## Replace Method with Method Object

### 问题
你有一个很长的方法，其中的局部变量很多且缠绕在一起，你不能应用提取方法。


```
class Account {
  // ...
  public double transfer(double amount) {
    double exchangeRate;
    double serviceCharge;
    double balance;
    // 又臭又长的逻辑代码
  }
}
```


### 解决方案
将该方法转换为单独的类，以便局部变量成为类的字段。然后，您可以将该方法分割为同一个类中的几个小方法。

```
class Account {
  // ...
  public double transfer(double amount) {
    return new AccoutTransfer(this).compute(amount);
  }
}

class AccoutTransfer {
  private double exchangeRate;
  private double serviceCharge;
  private double balance;
  
  public AccoutTransfer(Account account) {
    // account object复制比较属性
  }
  
  public double compute(double amount) {
    // 逻辑代码
  }
}
```