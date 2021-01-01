# System.Console 使用详解
System.Console 巨好用！此篇文章介绍它的大多数用法~

## 目录

* [打印内容](#打印内容)
  * [WriteLine](#WriteLine)
  * [Write](Write)
  * [Clear](Clear)
* [读取内容](#读取内容)
  * [ReadLine](#ReadLine)
  * [Read](#Read)
  * [ReadKey](#ReadKey)

## 打印内容
### WriteLine

来看一段示例代码

```c#
using System;

Console.WriteLine("Hello, world!");
```
输出：
```
Hello, world!
```
来看看它的声明：`public static void WriteLine(string? value)`
有没有发现 `string` 是可为空的，那传一个 `null` 试试看？
```c#
using System;

Console.WriteLine(null);
```
QAQ它居然报错！看看错误信息：`以下方法或属性之间的调用具有二义性:“Console.WriteLine(char[]?)”和“Console.WriteLine(string?)”`
两个参数可为空的方法……怪不得报错，那来个强转不就得了？

```c# 
using System;

Console.WriteLine("(string?)null:");
Console.WriteLine((string?)null);
Console.WriteLine("(char[]?)null:");
Console.WriteLine((char[]?)null);
Console.WriteLine("(object?)null:");
Console.WriteLine((object?)null);
Console.WriteLine(":");
Console.WriteLine();
Console.WriteLine("string.Empty:");
Console.WriteLine(string.Empty);
```

诶诶诶……怎么有个警告……还是先看一下：`只能在 "#nullable" 注释上下文内的代码中使用可为 null 的引用类型的注释。`，知道问题了，再改改

```c# 
#nullable enable
// ...
```

OK，编译……运行！

Output：

```
(string?)null:

(char[]?)null:

(object?)null:

:

string.Empty:

```

可以看到他们都是一样的~

但是我想格式化咋搞？好办~

看这里：

```c#
// #nullable enable
using System;

Console.WriteLine("Enter age:");
int age = 0;
while (!int.TryParse(Console.ReadLine(), out age))
{
    Console.WriteLine("Age？我让你输 age 你给我输的是啥？啊？");
    Console.WriteLine("Enter age:");
}
Console.WriteLine("Enter name:");
string name = Console.ReadLine() ?? "Unknown";

Console.WriteLine("Hello, {0}! You're {1} years old!", name, age);
```

试一下（被``包住的是输入）：

```
Enter age:
`12`
Enter name:
`Haceau`
Hello, Haceau! You're 12 years old!
```

这里，{0} 代表第 0 个格式化参数，{1} 代表第 1 个格式化参数，既然都有序号那肯定能重用！上代码：

```c#
using System;
// format 'Hello, world!'
Console.WriteLine("{0}{1}{2}{2}{3}{4}{5}{6}{3}{7}{2}{8}{9}", 'H', 'e', 'l', 'o', ',', ' ', 'w', 'r', 'd', '!');
```

这个例子很好滴说明了格式化参数可以重用

当然也能乱序，比如：

```c#
using System;
// format 'Hello, world!'
Console.WriteLine("{1}{0}{3}{3}{2}{5}{4}{7}{2}{6}{3}{9}{8}", 'H', 'e', 'l', 'o', ',', ' ', 'w', 'r', 'd', '!');
```

! 注意：虽然可以乱序，但是格式化字符串中还是要有 {0}，格式化但是没有 {0} 是 8 可以的（会抛出 FormatException）

还有一种格式化方法：

```c#
using System;

Console.WriteLine(string.Format("{0}, {1}!", "Hello", "world"));
```

或者：

```c#
using System;

string name = "Haceau-Zoac";
int age = 11;
Console.WriteLine($"Hello, I'm {name}! I'm {age} years old!");
```

我们还可以输出字符数组：

```c#
using System;

char[] hello = "Hello, world!".ToCharArray();
Console.WriteLine(hello);
// Hello, world!
```

亦或是：

```c#
using System;

char[] hello = "Hello, world!".ToCharArray();
//       字符数组，截取起点，截取字符数
Console.WriteLine(hello, 5, 3);
// , w
```

### Write

诶，你有没有发现他们结尾会自动换行啊，我不想换行咋办？好整，用 Write！

```c#
using System;

Console.Write("Enter age: ");
int age;
while (!int.TryParse(Console.ReadLine(), out age))
{
    Console.WriteLine("Age？我让你输 age 你给我输的是啥？啊？");
    Console.Write("Enter age:");
}
Console.Write("Enter name: ");
string name = Console.ReadLine() ?? "Unknown";

Console.Write("Hello, ");
Console.Write(name);
Console.Write("! You're ");
Console.Write(age);
Console.Write(" years old!");
```

结果：

```
Enter age:
`12`
Enter name:
`Haceau`
Hello, Haceau! You're 12 years old!
```

其他的就都和 WriteLine 一样啦qwq~

### Clear

TODO

## 读取内容

### ReadLine

TODO

### Read

TODO

### ReadKey

TODO
