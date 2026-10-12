

# SSH connection breaks after some idle time

For some reason, while I'm at work, if I leave an SSH connection sitting idle for
a few minutes, I lose the connection. It gets stuck; whatever I type doesn't
show on the remote shell. To fix this problem, I have set two variables in my
`.ssh/config` file:

```
Host *
  ServerAliveInterval 30
  ServerAliveCountMax 5
```

It causes SSH to send dummy/ping packets every 30 seconds. If the connection
is unstable and the server doesn't respond to the packet, it will retry up to 5
times. Only after 5 consecutive failed attempts will it give up on the connection.
