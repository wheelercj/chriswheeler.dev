+++
title = 'How to print emoji with C++'
date = 2022-04-28T17:21:23-08:00
tags = []
+++

![emoji in terminal](/emoji_in_terminal.png)

Printing emoji can be tricky because C++ running in Windows doesn't always support UTF-8. Emoji and other [Unicode](https://en.wikipedia.org/wiki/Unicode) symbols can be printed with C++ by copying the symbol itself into your code or by using a Unicode code point. You can find emoji and other symbols at sites like [unicode-table.com](https://unicode-table.com) or with a keyboard shortcut (`Windows+.` on Windows, `ctrl+cmd+space` on Mac, and `ctrl+.` on some Linux distros). On Mac and Linux, working with emoji in C++ is straightforward and doesn't require any tricks. With Windows you will need to use a modern terminal like [Windows Terminal](https://aka.ms/terminal) and the code below.

```cpp
#include <iostream>  // wcout
#include <string>  // wstring
#ifdef _WIN32  // if the current operating system is Windows
#include <windows.h>  // WriteConsoleW
#endif

void wprint(std::wstring message)
{
    /*  wprint prints a string that can have any emoji or Unicode characters.
        When creating a wstring, put an L in front of it.
        For example, `wprint(L"Hi! 🔥");` prints `Hi! 🔥`.
        Your file must be saved in the UTF-8 (_with_ BOM/signature) encoding.
    */
#ifdef _WIN32
    HANDLE handle = GetStdHandle(STD_OUTPUT_HANDLE);
    DWORD n_written;
    WriteConsoleW(handle, message.c_str(), (DWORD)message.size(), &n_written, NULL);
#else
    std::wcout << message;
#endif
}
```

One downside of the code above is that `std::wstring`s are not truly cross-platform. [Here](https://github.com/wheelercj/ynot) is a library I made that has a similar print function (it can print any Unicode characters) that takes a `std::string` as an argument instead.

## further reading

* You can change the color of many Unicode symbols by using ANSI codes. See [How to use colors in terminals](/posts/how-to-use-colors-in-terminals).
* [The Absolute Minimum Every Software Developer Absolutely, Positively Must Know About Unicode and Character Sets (No Excuses!)](https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-Unicode-and-character-sets-no-excuses) by Joel Spolsky

If you want to see all Unicode symbols in your terminal, you can do so easily with Python:

```python
import unicodedata
print("\x1b[32m Hold enter. \x1b[39m")
for i in range(0, int("FFFFFFFF", base=16)):
    try:
        code_point = f"\\U{hex(i)[2:].zfill(8)}"
        symbol = eval(f"'{code_point}'")
        name = unicodedata.name(symbol)
        print(end=f"{symbol}  {code_point}  {name}")
        _ = input()  # pause output
    except (ValueError, SyntaxError):
        pass  # skip invalid unicode symbols & control characters
    except Exception as e:
        print(e)
```
