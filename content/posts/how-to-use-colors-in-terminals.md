+++
title = 'How to use colors in terminals'
date = 2022-03-24T23:25:28-08:00
+++

![colors_in_terminal.png](/colors_in_terminal.png)

With many programming languages, you can change the color and the style of your output to a terminal using [ANSI codes](https://en.wikipedia.org/wiki/ANSI_escape_code#SGR_(Select_Graphic_Rendition)_parameters).

Here's how:

**with C++:**

```cpp
cout << "\x1b[31m This will be red! \x1b[0m This will be the default color.";
```

**with Python:**

```python
print("\x1b[31m This will be red! \x1b[0m This will be the default color.")
```

![red_text_in_terminal.png](/red_text_in_terminal.png)

As you can see, the string did not need to change at all even with a different programming language. The odd-looking parts of the string are called ANSI codes. The first one, `\x1b[31m`, changed the color of all printed text after it to red until the second ANSI code, `\x1b[0m`, reset all following output to the defaults.

To use different colors and styles, you only need to change the second number. For example, `\x1b[31m` is red, `\x1b[32m` is green, `\x1b[33m` is yellow, etc. You can also change the background color, add style such as an underline or italics, and you can even mix and match these. Apply multiple colors and/or styles at once by putting semicolons between multiple numbers, such as `\x1b[4;32;43m` to print text that is underlined and green with a yellow background. Some programming languages have libraries that make working with colors easier, such as [Python's Colorama library](https://github.com/tartley/colorama). [Here's an MIT-licensed enum](https://github.com/wheelercj/ynot/blob/ddfc4e20041e099f971cb96e41b16bb1061fda41/ynot/terminal.h#L66) you can use with many colors and styles. Otherwise, you can see the ANSI code for many colors and styles on [Wikipedia](https://en.wikipedia.org/wiki/ANSI_escape_code#SGR_(Select_Graphic_Rendition)_parameters), or in the images below.

`\x1b[#m` (replace `#` with one of the numbers below).

![terminal-color-numbers1.png](/terminal-color-numbers1.png)

`\x1b[38;5;#m` (replace `#` with one of the numbers below).

![terminal-color-numbers2.png](/terminal-color-numbers2.png)

`\x1b[48;5;#m` (replace `#` with one of the numbers below).

![terminal-color-numbers3.png](/terminal-color-numbers3.png)

Or to choose from among 16,777,216 color options, use `\x1b[38;2;R;G;Bm` and replace `R`, `G`, and `B` with numbers each in the range 0-255 that represent red, green, and blue values respectively. This changes the foreground color. To change the background color, the same is true but with `\x1b[48;2;R;G;Bm`.

You can change the color of many emoji/Unicode symbols; see [How to print emoji with C++](https://blog.chriswheeler.dev/how-to-print-emoji-with-cpp) (and Python, and other languages). ANSI escape codes can also be used to control where the cursor is (you can even move the cursor up); see [How to control the terminal's cursor](https://blog.chriswheeler.dev/how-to-control-the-terminals-cursor).

To see some of the possible colors, styles, and combinations of these in your own terminal, try the code below.

**with C++:**

```cpp
// adapted from the code written by rabin utam: https://stackoverflow.com/questions/287871/how-to-print-colored-text-to-the-terminal
#include <iostream>
#include <string>
using namespace std;

int main() {
    for (int style = 0; style < 8; style++) {
        for (int fg = 30; fg < 38; fg++) {
            string s1 = "";
            for (int bg = 40; bg < 48; bg++) {
                string format = to_string(style) + ";" + to_string(fg) + ";" + to_string(bg);
                s1 += "\x1b[" + format + "m " + format + "\x1b[0m";
            }
            cout << s1 << endl;
        }
        cout << endl;
    }
}
```

**with Python:**

```python
# Credit to rabin utam: https://stackoverflow.com/questions/287871/how-to-print-colored-text-to-the-terminal
def print_format_table():
    """Prints table of formatted text format options."""
    for style in range(8):
        for fg in range(30,38):
            s1 = ''
            for bg in range(40,48):
                format = ';'.join([str(style), str(fg), str(bg)])
                s1 += '\x1b[%sm %s \x1b[0m' % (format, format)
            print(s1)
        print()

print_format_table()
```

Not all terminals display colors using ANSI codes and some have small differences in how they use ANSI codes to display some colors and styles. For example, most methods of running the code on Windows either cannot use ANSI codes at all or cannot display some things like italics, faint colors, and flashing colors. [Windows Terminal](https://aka.ms/terminal) is highly recommended for Windows users.
