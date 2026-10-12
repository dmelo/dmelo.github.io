

# Solving "You have no authority to access this router" on TP-Link router


Today I stumbled upon a funny problem. I'm working on a piece of equipment that
is a couple of hundred miles away. It sits behind a TP-Link router, model
TL-MR3220 to be more precise. I access my equipment through a VPN, so I rarely
have to deal with the router. Today was not one of those days.

There was a problem regarding the ISP and, long story short, I had to access
the router's web admin interface. Seems like a simple task: just start an SSH
tunnel from my laptop to port 80 of the router passing through my equipment.
Something like:

    ssh -N -f -L 3228:192.168.15.228:80 dmelo@10.20.2.1

Then I would be able to access the admin interface on the address
`http://localhost:3228`. Unfortunately, after the auth, I saw almost all the
frames showing the same message "You have no authority to access this router!".

At first, I thought it was a firmware problem. But then the problem would be
even worse. How would I remotely update the firmware? I would have to do
a road trip.

Looking for alternative scenarios, googling around, I found someone who had
the same problem: `http://forum.tp-link.com/showthread.php?74815-You-have-no-authority-to-access-this-router!`

One of the comments clarifies that the problem occurs because the router tries to
assess the `referer` parameter, which it expects to be on the same network as
the router.

It can be solved by modifying the `referer` request header. The forum suggests
using the RefControl addon for Firefox. But I only had luck using the `Referer
Control` extension for Chrome. After setting the referer header to the router's
address on the local network (in my case http://192.168.15.228), it worked.
