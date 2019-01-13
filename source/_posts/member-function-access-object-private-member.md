title: C++成​员​函​数​中​访​问​对​象​的​私​有​成​员
tags:
  - C/C++
id: 146
categories:
  - C/C++
date: 2014-06-07 17:28:00
---

转自[百度文库](http://wenku.baidu.com/view/4e13d5343968011ca300912c.html)，以前学习C++的时候没有注意这个知识点。

知识点描述：

1.  在C++的类的成员函数中，允许直接访问<span style="color: #ff0000;">该类的对象的私有成员变量</span>。
2.  在类的成员函数中可以访问<span style="color: #ff0000;">同类型实例</span>的私有变量。
3.  拷贝构造函数里，可以直接访问<span style="color: #ff0000;">另外一个同类对象</span>（引用）的私有成员。
4.  类的成员函数可以直接访问<span style="color: #ff0000;">作为其参数的同类型对象</span>的私有成员。
举例说明：

1\. 在拷贝构造函数中可以访问引用对象的私有变量：例如:
<pre class="font:verdana lang:c++ decode:true">class   Point
{
    public:
        Point(int xx = 0, int yy = 0) {
            X = xx;
            Y = yy;
        }
        Point(Point &amp;p);
    private:
        int   X, Y;
};

Point::Point(Point &amp;p)
{
    X = p.X;
    Y = p.Y;
}</pre>
2. 在类的成员函数中可以访问同类型实例的私有变量：
<pre class="lang:c++ decode:true">class  A
{
    public:
        int getA() const {
            return a;
        }
        void setA(int val) {
            a = val;
        }
        void assign(A&amp; _AA) {
            this-&gt;a = _AA.a;
            AA.a = 10;		//可以直接访问
        }
        void display() {
            cout &lt;&lt; "a:" &lt;&lt; a &lt;&lt; endl;
        }
    private:
        int a;
};</pre>
3. 在类的成员函数中可以访问同类型实例的私有变量：
<pre class="lang:c++ decode:true">#include &lt;iostream&gt;
using namespace std;
class TestClass
{
    public:
        TestClass(int amount) {
            this-&gt;_amount = amount;
        }
        void UsePrivateMember() {
            cout &lt;&lt; "amount:" &lt;&lt; this-&gt;_amount &lt;&lt; endl;
            /*----------------------*/
            TestClass objTc(10);
            objTc._amount = 15;   //访问同类型实例的私有变量
            cout &lt;&lt; objTc._amount &lt;&lt; endl;
            /*----------------------*/
        }
    private:
        int _amount;
};
int main()
{
    TestClass tc(5);
    tc.UsePrivateMember();
    return(0);
}</pre>
<span style="color: #ff0000;">关于该知识点的讨论和解释：</span>

1\. 私有是为了实现“对外”的信息隐藏，或者说保护，在类自己内部，有必要禁止私有变量的直接访问吗？难道还要让我们声明一个类为<span style="color: #ff0000;">自身的友元</span>？

2\. C++的访问修饰符的作用是以类为单位，而不是以对象为单位。通俗的讲，同类的对象间可以“互相访问”对方的数据成员，只不过访问途径不是直接访问。

步骤是：通过一个对象调用其public成员函数，此成员函数可以访问到自己的或者同类其他对象的public/private/protected数据成员和成员函数(类的所有对象共用)，而且还需要指明是哪个对象的数据成员(调用函数的对象自己的成员不用指明，因为有this指针；其他对象的数据成员可以通过引用或指针间接指明)

**可以提供访问行为的主语为“函数”。**

类体内的访问没有访问限制一说，即private函数可以访问public/protected/private成员函数或数据成员，同理，protected函数，public函数也可以任意访问该类体中定义的成员public继承下，基类中的public和protected成员继承为该子类的public和protected成员（成员函数或数据成员），然后访问仍然按类内的无限制访问。

3. 注意公有和私有的概念：每个类的对象都有自己的存贮空间，用于存储内部变量和类成员；但同一个类的所有对象共享一组类方法，即每种方法只有一个源本。很明显，类方法是所有该类对象共同使用的，因此不存在每个对象有一组类方法的副本。源本的类方法自然可以访问所有该类对象的私有成员。

4. 访问权限是相对于类而言，而非对象！

一个问题，为什么成员函数可以访问私有成员呢？结果很显然，如果不能访问，那么私有成员的存在就没有了意义。<span style="color: #ff0000;">而对于成员函数中允许访问对象的数据成员</span>，一方面保证了安全性与封装性，另一方面提供方便的操作。第一句话的解释，就是承认只有成员函数可以访问私有成员，这里不涉及友元及派生。这样一来，安全性仍然得到了保证，也完成了封装工作。对于第二句话，试想，如果都得靠接口来实现数据传送，那么操作是否极为不便？既然处于成员函数中，已经保证了足够的安全和封装性，那么这里如果还得借助接口，就有些不合情合理了。作为对数据成员的灵活处理，<span style="color: #ff0000;">设计者允许在成员函数中访问对象的私有成员</span>，为使用者提供了很大的方便。这同时也反映了语言的灵活性和原则性.

&nbsp;

&nbsp;

&nbsp;