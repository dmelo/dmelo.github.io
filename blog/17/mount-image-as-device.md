

# Mount Image as Device

I have a lot of images from backup of projects I have worked with. Sometimes I
need to mount one of those images to get some file. I used `losetup` to get the
/dev/loopX device file and then I was trying to use `kpartx` to map the
partitions inside that device file. `kpartx` was giving the following error:

```
device-mapper: resume ioctl on loop2p1 failed: Invalid argument
create/reload failed on loop2p1
```

Then I stumbled on a post saying I could use `losetup` for mapping the
partitions as well. It worked:

```bash
losetup -fP disk.img
```
