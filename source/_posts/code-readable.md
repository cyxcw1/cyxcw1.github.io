title: 论代码可读性
date: 2017-02-15 23:11:58
tags:
	- 重构
---

可读性高的代码，好处就不多说了，看上去美观，便于维护（瞬间定位Bug+秒解Bug）、便于扩展，便于团队协作，当遇到变态的需求，代码需要重构时，也为代码重构提供良好的条件。下面从几个基本的方面，谈谈几个有助于提高代码的可读性的点。

### 1. 代码规范
这是最基本的也是最容易学的，依葫芦画瓢即可。首先参考这边文章中的[「安卓编码规范」](magnet:?xt=urn:btih:9e93f2aa5e0dadae9b03da8befafa08c0c722069
)。只是参考，不做过度的强求，某些代码风格方面（如大括号不换行还是换行）不是非要统一，这样做没有多大意义。个人认为几个比较重要的点：

#### （1）Let Code Talk
类、变量、方法的名称，尽量起一个有意义的名称，养成良好的习惯。当然，一些临时变量，使用`tmp`，或者一些简单的函数返回结果使用`res`，也没多大关系，但是遇到`active1`~`active32`这样的命名方式，简直会让人抓狂。起个好名字，至少他人维护你的老代码时，能够瞬间理解这个变量的意义，而不需要特意麻烦你再给他解释一遍。

另外，建议Let Code Talk也别做得太过了，造成一个变量就好像一句话一样冗长。我们是写代码，而不是真的「Talk」或者在写文章，见下节。
#### （2）命名长度问题
一般的规范建议**25个**字符，工程比较大的话，多几个字符，**30个**字符也ok。实在放不下了，可以用缩写，如`KnifeBreakerActionManager`-->`KnifrBrkrMgr`。还是放不下，可以使用**注释**。
#### （3）注释
上文提到了注释可以解决命名长度问题，遇到一些复杂的业务逻辑，不要懒，一定也要留下注释，以免日后自己维护的时候需要拼命回忆，如果是他人维护，那就算那人倒霉。

当然，也不要过度注释：

``` java
if(breakfastReady) {
// 如果早餐准备好了
}
```

如果觉得好玩儿，当我没说，:)

**注释**是一个良好的编码习惯，写代码的过程中，想想如果一年后自己来维护这段代码，或者交给别人维护，是否能在短时间内get到这段代码在表达什么。

#### （4）80列
一行代码不要过长，不然需要拖动横向滚动条，才能看全这一行代码。你的显示器够大，别人的显示器却可能是小笔记本，养成良好的习惯，找一个合适的位置，啪地敲一下回车来断行。旧时习惯在80列的位置，项目稍微大一点80列可能不太够用，多加几个字符也无伤大雅。

如果细心观察，Android Studio编辑区域的右边有一条灰色竖线，大概在100列的位置，这是为了提醒你控制你这一行的代码长度。另外，`strings.xml`等各种资源文件里的内容，个人认为不需要拘泥于**80列**，特别是`strings`，换行可能会影响格式。

#### （5）缩进
提到了「80列」，不得不提到缩进。过多层级的缩进，不仅仅影响代码的可读性（如轻易达到「80列」），还给调试的时候带来困难。先看看糟糕的缩进：

``` java
if(haveApple) {
	if(havePancil) {
		if(combineApplePan) {
			// todo
		}
	}
}
```
这只是一个缩进层级过多的例子，且很容易使用符号`&&`组合判断条件。在日常编码中，最常遇到的是几个连续的http异步回调，或者几条顺序执行的`DataCommand`的异步回调，如：

``` java
DataCommand1.getInstance().start(new MyCallback() {

    @Override
    public void onSuccess() {
        DataCommand2.getInstance().start(new MyCallback() {
            @Override
            public void onSuccess() {
                DataCommand3.getInstance().start(new MyCallback() {
                    @Override
                    public void onSuccess() {
                        
                    }

                    @Override
                    public void onFailure() {

                    }
                    
                });
            }

            @Override
            public void onFailure() {

            }
            
        });
    }
    
    @Override
    public void onFailure() {
        
    }

});
```
上述代码中，如果在某些`onSuccess`中增加一些`if`判断之类的条件判断，那这缩进可真是够呛的，更要命的是，debug时设置断点还可能设错行。

这时，可以尝试使用两个函数把`DataCommand2.getInstance().start()`与`DataCommand3.getInstance().start()`分别封装起来，效果会好很多。

（曾经听到过人讨论「首个大括号是否换行」的代码风格，有人支持换行的理由是，方便计算缩进的个数。其实，当缩进个数需要计算的时候，就表示你的代码需要重构了）

#### （6）避免过多的魔数
魔数，也就是Magic Number、Hard Code。一个常数，如果只使用一次，看起来可以不使用变量保存；如果一个常数在多处Hard Code，当这个常数需要修改时，就需要修改多处，更要命的是，某处还忘了修改。这些个常数，我们称之为魔数。

不管一个魔数是使用一次，还是多次，最好还是使用一个变量保存。即使只使用一次，通过变量名，我们能够准确地了解这个数字表达的意思。比如一个妹子的生日：

``` java
// 谁也不知道1012是一个妹子的生日，也不知道为什么num == 1012的含义
if(num == 1012) {
    // do something
}
//------分割线------
// 这样做会更好
public static final int BIRTHDAY_OF_MEIZI = 1012;
...
if(num == BIRTHDAY_OF_MEIZI) {
    // do something
}
```

学习设计模式时，我们常把「对扩展开放，对修改封闭」挂在嘴边，消灭魔数，就是对「开闭原则」的一个最简单的实践。

#### （7）尽量缩小变量作用域
缩小变量作用域，不仅使得相应的代码更易于维护，这也是「高内聚，低耦合」中「高内聚」的一个简单实践。遵循一些原则，如「使用时才初始化」「把相关语句放一起」「函数封装」可以有效地达到「缩小变量作用域」这个目的。这也需要在日常的编码过程中多思考，从细微处保证代码的内聚性。下面简单举例说明一下「把相关语句放一起」，首先看一个反例：

``` java
public void ComputShopData() {
    MarketData mrkData;
    SalesData slsData;
    TravelData trData;

    mrkData.ComputQuarterly();
    slsData.ComputQuarterly();
    trData.ComputQuarterly();

    mrkData.ComputAnnual();
    slsData.ComputAnnual();
    trData.ComputAnnual();

    mrkData.Print();
    slsData.Print();
    trData.Print();

}
```

每个变量`mrkData`，`slsData`，`trData`的作用域跨度都很大，几乎从函数开始至函数结束，通过以下修改，把相关的语句放一起，会使得代码的思路更加清晰：

``` java
public void ComputShopData() {
    MarketData mrkData;
    mrkData.ComputQuarterly();
    mrkData.ComputAnnual();
    mrkData.Print();

    SalesData slsData;
    slsData.ComputQuarterly();
    slsData.ComputAnnual();
    slsData.Print();

    TravelData trData;
    trData.ComputQuarterly();
    trData.ComputAnnual();
    trData.Print();

}
```

功能不相关的代码，或者功能相差比较大的代码区域，中间可以空一行进行分隔，也可以**封装成函数**，方便以后扩展。

