

# List resources being used by process

I've been looking for this for a while. I wanted a way to list all files openned
by a process. The answare is way simpler than I imagined:

```bash
lsof -p PID
```

where `PID` is the process ID. It gives not only the files openned but sockets,
resources and directories.
