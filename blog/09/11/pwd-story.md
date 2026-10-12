# pwd Story

This post is just some curious geek stuff about Linux.

pwd is a shell built-in command that shows you the working directory. A good part of the built-in commands also have an external program that, in theory, behaves just like the built-in one.

Well, that's the theory; in practice some external commands behave differently than the respective built-in. This post shows the example of the pwd command. When I type *pwd* here, the return is */home/dmelo*.

Now comes the weird demo. Create a symbolic link to /tmp in your home directory and go to your new link.

    dmelo@merov:~$ ln -s /tmp/ tmp
    dmelo@merov:~$ cd tmp

Now, run two commands: the first is *pwd* and the second, */bin/pwd*.
You will see something like this:

    dmelo@merov:~/tmp$ pwd
    /home/dmelo/tmp
    dmelo@merov:~/tmp$ /bin/pwd
    /tmp

The thing is that the pwd built-in command does not care about where your symbolic link is pointing to. On the other hand, the program /bin/pwd cares. /bin/pwd replaces the symbolic link with the directory it points to. As the built-in commands have precedence over external commands, when you just type pwd it uses the built-in version of pwd.

See you in the next geek creepy story about Linux :D
