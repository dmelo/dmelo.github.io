

# Vim Tips

All my friends know that I'm a big fan of Vim. I use Vim to program in C++, Java, PHP, JavaScript and whatever language I have to. I do that because Vim is simple and powerful. It supports regular expressions to search and replace text. It leads you to the function's man page with a simple shortcut.

You can use Vim as a development environment for your projects. Instead of using one IDE for each language you program in, you can just use Vim.

If you have used Vim for more than 1 year, you probably know most of the tips I'm giving here.

###1 - Man page

When you are programming in C, PHP, Perl.... your system's man page may have the documentation for the function of the language that you are using. On Debian-based distributions, install the package manpages-dev.

    sudo apt-get install manpages-dev

Then, if you point the cursor over the word *printf* and press `<Alt>+K` Vim will lead you to the *printf* man page.

###2 - Tabs

Developers usually have to edit multiple files at once. Vim has tabs in a similar way to Firefox. To open a new tab you can just use:

    :tabe file.c

Use *Ctrl+Alt+PageUp* and *Ctrl+Alt+PageDown* to navigate between the tabs.

###3 - Vertical Split

Split is very useful. It lets you see two or more files on the same screen at once. Simple split is used in the same way 
