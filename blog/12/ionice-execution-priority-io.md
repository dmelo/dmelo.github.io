

# ionice - Execution priority for IO

Today I'm doing some very intensive IO usage with MySQL. It's basically
restoring dumps which include lots of indexes. It's taking more than 6 hours
and it makes it difficult for other processes to use the IO too.

What I discovered is that you can use the command `ionice`, which is similar to
`nice` but (as the name suggests) for IO.

The command `ionice -c 3 -p 1394` changes the priority of the process 1394 to
idle, which means it will only get IO access when no other process has
requested it. The `-c` flag says the priority class of the project, from 0 to 3; 3
means idle.

After that command, other processes (GUI processes) started working much better.
