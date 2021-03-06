# 变量和函数

必要前驱：

- [Namespace](Chapter03.md)

上次我们说到了命名空间。使用关键字`namespace`来说明代码所处的命名空间。而在一个命名空间里，我们可以定义很多的类（并且我们在命名空间里能直接定义的也只有类）。这些类定义合起来就是我们的程序。

> 说命名空间里只能定义类有点不准确。其实接口也能直接在命名空间中定义。这个后边再讨论。

说到这里可能你有点懵逼。一般我们举的算法例子都是求最大公约数，求三角面积之类的过程。而C#里只写了一堆类（相当于名词），没有过程，怎么能完成求公约数之类的算法呢 ？

这就涉及到面向对象和面向过程之前的联系了。一般人们喜欢说面向对象是比面向过程更为高级的编程方法。实际上并不完全如此。面向对象思想主要是提倡打包数据。以至于把方法（过程，函数）也作为数据的一部分（类定义的一部分）来打包。所以从比较高的抽象层面上看，整个程序就是定义了一堆类。那么过程如何体现呢？按照面向对象的思路，我们定义了每一个类可能的行为（就是在类定义内部定义的方法），那么这些类凑到一起的时候各自按照自身的方法行动。从而自动的生成具体的执行过程。也就是说面向对象编程中，我们尽量不把过程定义得太具体，太死板。而是从更为抽象的角度对程序进行定义。在运行的时候，具体过程细节由程序根据我们抽象的定义自行确定。

同时我们应当看到，面向对象和面向过程的关注点是不太一样的。面向对象主要关注程序的组织方式。而只要是用描述过程的方式写的程序都可以算面向过程的。在用C#编程时，宏观层面我们贯彻了面向对象的思路。但是每个类定义内部，我们在定义类方法的时候仍然是用的面向过程的写法。那么我们这样费尽力气把面向过程的基础上加了面向对象，到底得到了什么呢？得到了更方便维护的代码。而且，减轻了编程工作的负担。我们不需要像纯面向过程里那样，把一整个算法过程事无巨细地全部描述出来。而只需要描述若干片段的过程，然后组织到类定义里。整体过程由程序自动拼凑我们写好的片段来组成。

这一期剩下的篇幅里我们主要说明两个问题——类定义的基本组成，以及函数的基础知识。

## 字段

类定义是数据的描述。在类定义的内部我们逐条描述它所包含的数据即可。但是这些被描述的数据可以分为两类——本来就是数据的数据（字段）和被当作数据的过程（方法）。

我们先来说说字段。我们在描述字段的时候要说明三个要素。首先，C#是静态强类型语言，我们要说明这个字段是什么类型的。第二，我们要给这个字段起名字以方便访问。第三条，给字段初始化值。

> 给字段初始化值不是必须的，但我强烈建议做。因为仅仅声明了名称和类型的字段是不能读取只能写入的。如果留着这样的字段，一不小心在第一次写入之前进行了读取，就会造成程序错误。

具体的语法就是“<类型> <名称> [<=> <初始值>] <;>”。一定不要忘了字段描述是个语句，结尾要加上分号。另外，我们其实还能给字段加上修饰，但是这些内容我们往后放一放。于是一个带有字段的类的定义就是这样的。

```C#
class Person
{
    int age = 0;
    string name = "";
}
```

## 方法

字段表示某个类的对象的属性，而方法表示类对象的行为。比如要把人类抽象为类，年龄、性别是属性，而吃喝、呼吸是行为。

在类中定义方法要比定义字段复杂一点。但是基本的思路是一致的。要有类型说明，有方法名称（函数名），有方法定义。

方法名和字段名一样，起个不重复的名字而已。

然后要确定方法的类型。方法和字段一样是有类型的。方法（或者说函数）的类型就是其输入和输出类型的组合。比如一个函数输入一个整数，返回一个字符串。它的类型就是“(int)->(string)”。

> 注意输入整数返回字符串的函数的类型和由一个整数以及一个字符串组成的类或者结构体不是一回事。

一般来说我们把返回值类型写到函数名前边。把输入参数类型写到函数后边。输入参数列表要用括号括起来（这是一个有序列表）。而且由于输入参数在定义函数体的时候都是用得到的变量，我们要像定义字段那样，在说明每个参数的类型的同时，给每个参数起名字。（当然也可以给给每个参数设定默认值。）

接下来方法定义，就是写函数体。这个相当于定义字段时候赋予初始值，但是不同的是这个是必填项，而不是可选项。函数体紧跟着参数列表，在一对花括号内书写。花括号内用面向过程的方法逐句定义过程。但函数定义的结尾（函数题的右花括号后边）不需要加分号。所以一个有字段又有方法的函数看起来是这样的：

```C#
class Person
{
    int age = 0;
    string name = "";
    void Eat(string food)
    {
        ;
    }
}
```

> 方法同样有修饰，以后再说。

## 主函数

下一期我们要具体说明函数体内部怎么写。再次之前我们先分析一下Hello World程序中的主函数。

在写一部小说的时候，我们做好各种人设。然后可以按照人设来构造人物之间的关系和剧情走向。但是做好人设之后必须有一个作为开头的桥段把人物都凑起来，才能让他们之间产生反应。而写程序的时候，我们有了很多对象，要想让它们之间自动生成过程，也需要一个作为开头的过程把它们凑起来。这个特殊的作为程序入口的过程就是主函数。

由于面向对象方法抽象到最后都是类，所以我们显然不能直接在命名空间中写一段过程作为主函数。我们需要把它写成一个类的方法。这个类和这个方法都是特殊指定的。在Hello World中`Program`就是那个特殊的包含程序入口的类，`Main`就是那个特殊的作为程序入口的方法。程序启动的时候首先调用`Program.Main`，以后的事情就是按照各个类的定义自动运行了。

```C#
class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Hello World!");
    }
}
```

### 面向过程中的主函数

在面向过程编程中，函数的主要作用就是打包代码块以供复用。程序由小到大打包成各种函数。最后整个程序打包成一个函数。启动程序的时候只需要执行这个函数，然后由这个函数去调用其他函数。这个函数就叫做主函数。

在C语言中，作为函数名的`main`虽然不是关键字，但是实际上是被预先赋予了特殊含义的。名为`main`的函数就是程序的入口。如果定义了多个名为`main`的函数，或者没有按照标准的`int main(int argc, char *argv[])`之类的形式来定义`main`就可能导致未定义行为。

> `argc`是命令行参数数量，`argv`是具体的命令行参数内容。所谓的命令行参数，就是你从命令行启动这个程序时所用命令中所哟通过空格相互隔开的部分。例如`ls -a`中的`ls`和`-a`就是命令行参数。执行这个命令的时候我们会启动`ls`命令的可执行文件，从它的主函数开始执行。而`2`和`["ls","-a"]`则作为给主函数的参数。

### C#中的主函数

面向对象最基本的思想就是用类来组织数据。所以面向对象这种范式和面向过程关注的是不同的领域。你完全可以仍旧通过`main`函数来启动程序，只不过其中用到的变量是对象。但是这样的使用方式造成了很多特例。比如这样一来`main`函数就不属于任何类，它自己也不是一个类，但却直接存在于命名空间中。

而C#更加彻底地支持面向对象，当然要解决这些问题。在C#中，一个可执行的程序仍然是通过主函数启动（只不过命名风格变成了符合C#风格的`Main`）。但是C#中的主函数有如下特点：

- 可以在任何一个类中定义。但是不能在类外定义。
- 有`static`修饰符。表示这个函数是静态方法，即使类没有实例化，也能执行这个方法。
- 其返回值仍然只能是`int`或者`void`。可以接受一个`string[]`类型的值作为参数（表示启动时的命令行参数。）。

> C#对待命令函参数的方式和C不太一样。例如运行`dotnet TXProject.dll --tellme`时，`dotnet`表示启动dotnet运行环境，`TXProject.dll`是我要在dotnet环境中执行的程序，`--tellme`是要要传递给TXProject.dll的参数。那么主函数收到的命令行参数实际上只有`--tellme`，`dotnet`和`TXProject.dll`都不被当作是“命令行参数”。

> 如果要区分一个参数是给`dotnet`命令本身的，还是给运行在dotnet环境中的程序的，可以通过加一个`--`参数，在这个参数之后的都被认为是给运行在dotnet环境中的程序的。例如`dotnet run --help`会显示dotnet-run这个模块的说明。而`dotnet run -- --help`才是把`--help`传递给你自己写的程序。

