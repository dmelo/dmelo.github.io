

# List resources being used by process

I've been looking for this for a while. I wanted a way to list all files opened
by a process. The answer is way simpler than I imagined:

```bash
lsof -p PID
```

where `PID` is the process ID. It gives not only the files opened but also sockets,
resources and directories.
