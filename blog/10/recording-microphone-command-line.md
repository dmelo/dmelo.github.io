

# Recording microphone from command line

First of all, you must install lame. `apt-get install lame` on Debian-based distributions must do the job. Just after that, this must do the work.

    arecord -f cd -t raw | lame -x -r - out.mp3

As the line suggests, the microphone will be recorded to the out.mp3 file. Use `alsamixer` and press F4 to control the volume of your microphone.

Have fun! And leave comments ;)
