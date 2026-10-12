# ERROR: Couldn't connect to Docker daemon at http+docker://localunixsocket - is it running?

I was getting the following error when I tried to build my docker-compose.yml
with `docker-compose build --pull`:

```
ERROR: Couldn't connect to Docker daemon at http+docker://localunixsocket - is it running?

If it's at a non-standard location, specify the URL with the DOCKER_HOST environment variable.
```
When I checked on journalctl with `journalctl -u docker -f` I got:

```
Nov 27 15:12:35 fedora-aaa dockerd[912]: time="2017-11-27T15:12:35.097377389-02:00" level=error msg="Handler for POST /v1.25/build returned error: Error processing tar file(archive/tar: invalid tar header): "
```

Another curious thing was that Docker was filling `/tmp` with cache data. There
was a file with more than 2GB.

It turned out that the error messages from Docker were very misleading. I had a lot
of data inside the project directory (about 5GB) that was not
in `.dockerignore`. After adding the files/directories to
`.dockerignore` I was able to successfully build my project.

Docker will copy everything inside your Docker root directory to a context area.
Adding the directories to `.dockerignore` caused Docker to skip those
directories and, in my case, allowed it to move on.



