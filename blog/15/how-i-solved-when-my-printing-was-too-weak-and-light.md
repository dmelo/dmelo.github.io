

# How I solved when my printing was too weak and light

I have built a Mendel Prusa i3 Rework. In the final steps of
calibration, something wrong was happening that was making my prints too
weak, light and fragile.

I'm using [slic3r](http://slic3r.org/) to generate the G-Code. The first
time I ran the software I had to configure the parameters for my printer. In
my case, what was causing the fragile printing was a wrong parameter.

I had the filament diameter set to 3 mm but my filament is actually 1.75 mm in
diameter. That was causing the printer to extrude almost half of what it was
supposed to.

After adjusting that value and regenerating the g-code, the problem was solved.
