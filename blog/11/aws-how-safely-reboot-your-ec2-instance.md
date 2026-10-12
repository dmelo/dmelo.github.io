

# AWS - How to safely reboot your EC2 instance

If you install a new kernel, you will have to reboot your system in order to use it, right? When you are doing such things on a real machine that is next to you, there isn't much trouble rebooting it. That's because you have a keyboard plugged into the machine. But, what happens if your server is a virtual server, like EC2? If you reboot your machine and SSH doesn't come back, you're gonna have problems. Especially if you use just EC2-Storage instead of EBS.

A few months ago, I had to reboot a server and I faced the worst possible scenario. The SSH didn't come back, and I was using EC2-Storage to store all the data. The server was hosting around four websites, all of them were important to the client. I remember that (after kicking myself) I spent the next 28 hours:

* trying to recover the system;
* writing emails telling the people about the incident;
* setting up a new server;
* configuring the new server with the most updated backup I had of each system.

After two or three days an Amazon support tech recovered the partitions and I could merge end the job of getting the systems back to normal. It was painful as hell. I lost the entire week recovering from a dumb error.

Now, **here is how to reboot safely: DON'T REBOOT**. At least, don't reboot the one that is hosting the important stuff. Instead, create another server, clone all the EBS it uses and reproduce the exact same system you have live. The only difference should be the IP address. You will reboot and perform all the administrative tasks on the clone server. If everything goes ok on the clone server, then you change your elastic IP to point to the new server, and you can delete the old one.




