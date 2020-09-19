---
title: "simplify Path"
date: 2015-05-01
lastmod: 
draft: false
tags: ["leetcode"]
categories: ["leetcode"]
author: "Rogers"

weight: 2
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


题目
==

> Given an absolute path for a file (Unix-style), simplify it.  
> For example,  
> path = “/home/“, => “/home”  
> path = “/a/./b/../../c/“, => “/c”

思路1：
----

这个问题其实是一个双端队列的问题

*   每次碰到一个`/`或者连续很多的`/`就生成新的目录，让后将目录到`addLast`到双端队列里
*   每次碰到`.`相当于不动，`..`相当于尾部出队。
*   需要注意特殊的case，比如多个`/`连接在一起,我用的是`getNextSubPath`函数来处理这个情况，在第二个方法中有更加有优雅的方式那就是正则表达式.
``` java
public  class Solution {

       private int j;

    public String simplifyPath(String path) {

        StringBuilder sb= new StringBuilder();

        LinkedList<String> queue =new LinkedList<>();

        String temp;

        char[] cs=path.toCharArray();

        for( int i=0;i<path.length();){

           temp=getNextSubPath(cs, i);

            if(temp!= null){

                 switch (temp) {

                       case ".":

                            break;

                       case "..":

                            queue.pollLast();

                            break;

                       default:

                            queue.addLast(temp);

                            break;

                      }

                i= j;

           } else i++;

        }

        for(String str: queue){

           sb.append( '/').append(str);

        }

        return sb.length()==0? "/":sb.toString();

    }

    private String getNextSubPath( cha[] path, int start){

      int i;

      for(i=start;i<path. length;i++){

            if(path[i]!= '/') break;

     }

      for( j=i+1; j<path. length; j++){

            if(path[ j]== '/') break;

     }

      if(i==path. length||i>= j) return null;

      return new String(path,i, j-i);

    }

}
```

思路2：
----

*   用正则表达式`/+`来分割字符串,将字符串分割为很多子串，然后其他的处理和思路1一样。

```java
//用正则表达式

public  class Solution{

    public String simplifyPath(String path) {

        StringBuilder sb= new StringBuilder();

        LinkedList<String> queue = new LinkedList<>();

        String strs[]=path.split( "/+");

        for(String temp:strs){

                 switch (temp) {

                       case ".":

                            break;

                       case "..":

                           queue.pollLast();

                            break;

                       default:

                            if (!temp.isEmpty())

                           queue.addLast(temp);

                            break;

                      }

        }

        for(String str:queue){

           sb.append( '/').append(str);

        }

        return sb.length()==0? "/":sb.toString();

    }

}

```