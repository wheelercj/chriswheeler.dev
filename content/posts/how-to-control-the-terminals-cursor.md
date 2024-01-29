+++
title = "How to control the terminal's cursor"
date = 2022-05-09T14:37:51-08:00
lastmod = 2022-07-22T17:06:00-08:00
tags = []
+++

Ever wanted to move the cursor up while printing output in a program? You can. Here's how with C++, and the same string works with any language in many terminals:

```cpp
cout << "\x1b[A";
```

That moves the cursor up one line. You can move it up by as many lines as you want; here's how to move it up by 3 lines:

```cpp
cout << "\x1b[3A";
```

A variable works there too:

```cpp
int lines = 3;
cout << "\x1b[" << lines << "A";
```

You can also move the cursor down, right, and left.

```cpp
cout << "\x1b[A";  // up
cout << "\x1b[B";  // down
cout << "\x1b[C";  // right
cout << "\x1b[D";  // left
```

Below is how to move the cursor to specific coordinates (x,y). The top-left space in the terminal has the coordinates (1,1), and y increases downward.

```cpp
cout << "\x1b[" << y << ";" << x << "H";
```

Here's how to save the cursor's location and then restore the saved location later:

```cpp
cout << "\x1b[s";  // saves the cursor's location
// ...
cout << "\x1b[u";  // restores the cursor's location
```

These are called [ANSI escape codes](https://en.wikipedia.org/wiki/ANSI_escape_code), and similar ones let you change your output text's color and style. (See [How to use colors in terminals](https://blog.chriswheeler.dev/how-to-use-colors-in-terminals).)

To make controlling the terminal's cursor easier, [here's a cross-platform library](https://github.com/wheelercj/ynot) I made that you can use. It includes many helpful functions including ones to get the cursor's current coordinates and to get the terminal's size. Windows also has ways to control the cursor that are not-cross platform; an example is below.

## controlling the terminal's cursor with windows.h

[windows.h](https://en.wikipedia.org/wiki/Windows.h) is a header file available on all Windows computers that gives you a lot of control over the operating system. Other platforms may be able to get and use this header file, but it is (at least usually) not installed by default unlike other similar but different header files specifically made for those platforms. You can include windows.h into your code with `#include <windows.h>`.

Below is an example of how to make a rotating half-circle as a loading symbol using windows.h's `SetConsoleCursorPosition`, `Sleep`, and other functions including ones that allow detecting the size of the terminal window.

![loading_symbol.gif](/loading_symbol.gif)

```cpp
#include <windows.h>
#include <iostream>
#include <string>

const long double PI = 3.14159265359;

struct Coord
{
	int x;
	int y;
	Coord(int new_x, int new_y)
    {
		x = new_x;
		y = new_y;
	}
};

void show_loading_symbol(Coord window_size, HANDLE handle);
double degrees_to_radians(double degrees);
HANDLE get_std_handle();
Coord get_window_size(HANDLE handle);
void show_cursor(bool choice, HANDLE handle);
void print_at(short x, short y, std::string message, HANDLE handle);

int main()
{
	HANDLE handle = get_std_handle();
	Coord window_size = get_window_size(handle);
	show_cursor(false, handle);
	show_loading_symbol(window_size, handle);
	return 0;
}

void show_loading_symbol(Coord window_size, HANDLE handle)
{
	int radius = 12;
	int center_x = window_size.x / 2;
	int center_y = window_size.y / 2;
	double theta1 = 0;  // in radians; for printing the circle
	double theta2 = PI;  // in radians; for erasing the circle
	for (int i = 0; ; i++)
    {
		// A circle's coordinates can be found with these formulas:
		// x = radius * cos(theta)
		// y = radius * sin(theta)
		int x1 = int(radius * cos(theta1)) + center_x;
		int y1 = int(radius * sin(theta1)) + center_y;
		int x2 = int(radius * cos(theta2)) + center_x;
		int y2 = int(radius * sin(theta2)) + center_y;
		print_at(x1, y1, "*", handle);  // Print the circle.
		print_at(x2, y2, " ", handle);  // Erase the circle.
		theta1 += degrees_to_radians(1);
		theta2 += degrees_to_radians(1);
		if (i % 5 == 0)
			Sleep(1);
	}
}

double degrees_to_radians(double degrees)
{
	return degrees * 2 * PI / 360;
}

HANDLE get_std_handle()
{
	HANDLE handle = GetStdHandle(STD_OUTPUT_HANDLE);
	if (handle == INVALID_HANDLE_VALUE)
		throw "Error: Invalid handle value.";
	return handle;
}

Coord get_window_size(HANDLE handle)
{
	CONSOLE_SCREEN_BUFFER_INFO buffer_info;
	GetConsoleScreenBufferInfo(handle, &buffer_info);
	int columns = buffer_info.srWindow.Right - buffer_info.srWindow.Left + 1;
	int rows = buffer_info.srWindow.Bottom - buffer_info.srWindow.Top + 1;
	return Coord(columns, rows);
}

void show_cursor(bool choice, HANDLE handle)
{
	CONSOLE_CURSOR_INFO cursor_info;
	GetConsoleCursorInfo(handle, &cursor_info);
	cursor_info.bVisible = choice;
	SetConsoleCursorInfo(handle, &cursor_info);
}

void print_at(short x, short y, std::string message, HANDLE handle)
{
	SetConsoleCursorPosition(handle, { x, y });
	std::cout << message;
}
```
