# System.Console 使用详解
System.Console 巨好用！此篇文章介绍它的大多数用法~

## 目录

* [打印内容](#打印内容)
  * [WriteLine](#WriteLine)
  * [Write](Write)
  * [Clear](Clear)
  * [ConsoleColor](#ConsoleColor)
  * [ForegroundColor](#ForegroundColor)
  * [BackgroundColor](#BackgroundColor)
  * [ResetColor](#ResetColor)
  * [SetOut](#SetOut)
  * [SetError](#SetError)
  * [BufferWidth](#BufferWidth)
  * [BufferHeight](#BufferHeight)
  * [WindowWidth](#WindowWidth)
  * [WindowHeight](#WindowHeight)
  * [SetCursorPosition](#SetCursorPosition)
* [读取内容](#读取内容)
  * [ReadLine](#ReadLine)
  * [Read](#Read)
  * [ConsoleKey](#ConsoleKey)
  * [ConsoleModifiers](#ConsoleModifiers)
  * [ConsoleKeyInfo](#ConsoleKeyInfo)
    * [Key](#Key)
    * [KeyChar](#KeyChar)
    * [Modifiers](#Modifiers)
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

诶那我想把屏幕都清空咋办？

Clear 上！

```c#
using System;

Console.WriteLine("qwq");
Console.ReadKey(true);
Console.Clear();
Console.WriteLine("awa");
```

刚开始会输出 "qwq"，但当按下一个键之后屏幕就会被清空，变为 "awa"~

上面的示例其实可以简化：

```c#
using static System.Console;

WriteLine("qwq");
ReadKey(true);
Clear();
WriteLine("awa");
```

### ConsoleColor

这个枚举就是表示控制台的各种颜色的~

共 15 种！

### ForegroundColor

设置控制台前景色！

之前的文本不会受到影响，之后的文本就是这个色的辣~

试试：

```c#
Console.WriteLine("default");
Console.ForegroundColor = ConsoleColor.Red;
Console.WriteLine("red~");
Console.ForegroundColor = ConsoleColor.Yellow;
Console.WriteLine("green!");
```

可以看到，控制台颜色改变了！default 默认控制台颜色，red~ 红色， green! 绿色！

### BackgroundColor

设置控制台背景色！

之前的文本同样不会受到影响，之后文本的背景色就是这个颜色~

试试：

```c#
Console.WriteLine("default");
Console.BackgroundColor = ConsoleColor.Red;
Console.WriteLine("red~");
Console.BackgroundColor = ConsoleColor.Yellow;
Console.WriteLine("green!");
```

### ResetColor

颜色更改全局，以后的都是这个色了，我想让一个语句是这个颜色其他都不是咋弄？来试试：

```c#
Console.ForegroundColor = ConsoleColor.Red;
Console.WriteLine("此句前景色为红！");
Console.ResetColor();
Console.WriteLine("恢复默认值~");
```

可以看到，"此句前景色为红！"是红色的，"恢复默认值~"又是默认的颜色qwq

### SetOut

设置输出流！

也就是说，可以让控制台输出到文件（

```c#
using System;
using System.IO;

FileStream fs = new("test.txt", FileMode.OpenOrCreate);
StreamWriter sr = new(fs);
Console.SetOut(sr);
Console.WriteLine("Hello!");
```

控制台并没有输出 "Hello!"，而是输出到文件 "test.txt" 去了！

### SetError

设置错误流~

用法和 SetOut 相似，此处就不讲了owo

### BufferWidth

缓冲区宽度！

那什么是缓冲区？缓冲区就是在内存中预留的存储空间，用来缓存输入 or 输出的数据

```c#
for (int i = 0; i < Console.BufferHeight; ++i)
	Console.Write('a');
Console.Write('a');
```

正好输出了一行 'a' 再加上多一个

也可以这么写：

```c#
for (int i = 0; i <= Console.BufferHeight; ++i)
	Console.Write('a');
```

缓冲区宽度还可以设置~

!!! 警告：Console.BufferWidth 的设置操作只可在 Windows 平台执行

```c#
Console.BufferWidth = 100;
for (int i = 0; i <= 100; ++i)
    Console.Write('a');
```

 Windows 平台下，打印了一行零一个 'a'，在其他平台下，打印了 PlatformNotSupportedException 的错误信息

来个 if 判断：

```c#
if (OperatingSystem.IsWindows())
    Console.BufferWidth = 100;
for (int i = 0; i <= 100; ++i)
    Console.Write('a');if ()
```

### BufferHeight

和 BufferWidth 相对的，缓冲区高度~

!!! 警告：Console.BufferHeight 的设置操作只可在 Windows 平台执行

### WindowWidth

窗口宽度awa

控制台的窗口和缓冲区可能一样大，也可能不一样大

窗口也就是控制台，有可以拖拽的边框的那个，不是缓冲区

```c#
Console.WriteLine("qwq");
Console.WriteLine(Console.WindowWidth);
```

可能输出：

```
qwq
80
```

当然，窗口宽度也能设置！

!!! 警告：Console.WindowWidth 的设置操作只可在 Windows 平台执行

```
Console.WindowWidth = 100;
```

Windows 平台下，窗口的宽度变为了 100，在其他平台下，打印了 PlatformNotSupportedException 的错误信息

### WindowHeight

和 WindowWidth 相对的，窗口高度~

!!! 警告：Console.WindowHeight 的设置操作只可在 Windows 平台执行

### SetCursorPosition

有时你不想从上至下从左至右按顺序输出怎么办？用 SetCursorPosition 把光标位置改了吧！

```c#
Console.WriteLine("qwq");
Console.SetCursorPosition(20, 10);
Console.WriteLine("At line 10");
```

"qwq" 正常输出，但是 "At line 10" 却是在第 10 行第 20 个字符处输出的，就是 Console.SetCurorPosition 做的

## 读取内容

### ReadLine

ReadLine 就是让你输入一行，然后返回个字符串（你输入的）

比如

```c#
string str = Console.ReadLine();
Console.WriteLine("输入的是{0}", str);
```

输出：

```text
`qwq`
输入的是qwq
```

### Read

从控制台一个一个的读取字符

比如

```c#
for (int i = 0; i < 10; ++i)
{
    x = Console.Read();
    Console.WriteLine($"{x} is {Convert.ToChar(x)}");
}
```

输出

```text
`awa`
97 is a
119 is w
97 is a
```

### ConsoleKey

表示控制台上所有键的枚举！

### ConsoleModifiers

表示 Shift Alt 还有 Ctrl 修改键，可以组合~（用 | ）

### ConsoleKeyInfo

表示按下的键，咋表示呢？

#### Key

他表示你按下的键，是个 ConsoleKey

#### KeyChar

他也表示你按下的键，但是是个 char

#### Modifiers

他表示你同时按下了什么修改键，是个 ConsoleModifiers 类型的

### ReadKey

他从控制台读取一个键并且返回 ConsoleKeyInfo 类型

比如

```c#
ConsoleKeyInfo info  = Console.ReadKey();
Console.WriteLine($"Key is {info.KeyChar}");
```

输出

```text
`Hello`
Key is H
key is e
Key is l
Key is l
Key is o
```

