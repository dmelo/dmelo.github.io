

# Fedora - Find package that provides a binary file

On RPM base systems, you can use the following command to discover which package
provides binary installed on your machine:

    rpm -qf `which find`

Replacing `find` by the name of the binary you want to find.
