

# Fedora - Find package that provides a binary file

On RPM-based systems, you can use the following command to discover which package
provides a binary installed on your machine:

    rpm -qf `which find`

Replace `find` with the name of the binary you want to find.
